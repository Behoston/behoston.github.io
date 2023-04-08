---
layout: post
title:  "OpenWRT w roli nagrywarki dla kamery monitoringu."
date:   2023-01-29
categories: [PL, hardware]
header_image : /assets/img/openwrt-ipcam/header.jpg
image: /assets/img/openwrt-ipcam/header_big.jpg
---

## Kompilacja OpenWRT

[Ogólne instrukcje można znaleźć tutaj.](https://openwrt.org/docs/guide-developer/toolchain/use-buildsystem#cleaning_up)

[Instrukcje jak dodać ffmpeg z obsługą h.264.](https://forum.openwrt.org/t/how-to-build-ffmpeg-with-h-264-support/116554)

[Jak zaoszczędzić trochę miejsca.](https://openwrt.org/docs/guide-user/additional-software/saving_space)

Konfiguracja:

- Global build settings
    - [x] Compile with support for patented functionality
    - [ ] Enable IPv6 support in packages - usunięcie tego pozwoli zaoszczędzić nieco miejsca
- Kernel
    - modules
        - Filesystems
            - `kmod-fs-vfat` - obsługa systemu plików FAT32
            - `kmod-fs-ext4` - obsługa systemu plików ext4 dla większych magazynów danych
        - USB
            - `kmod-usb-storage` - obsługa pamięci USB
- Libraries
    - `libffmpeg-full` - pełna biblioteka do ffmpeg
    - `libx264` - kodek do kamery
- Multimedia
    - `ffmpeg` - program, którym będziemy zrzucać obraz
- LuCI
    - Collections
        - luci - webowy interfejs
    - Modules
        - Minify Lua sources
        - Minify JavaScript sources
        - Minify CSS files
    - Network
        - [ ] ppp - obsługa Point to Point Protocol, zazwyczaj jest zbędna

Dodatkowo polecam od razu dorzucić `block-mount`, który przyda się potem do wygenerowania konfiguracji fstaba oraz
ewentualnie `usbutils`, który między innymi podaje informacje na temat podłączonych urządzeń USB.

## Konfiguracja systemu

### Montowanie USB

[Instrukcje dotyczące montowania USB na Wiki](https://openwrt.org/docs/guide-user/storage/usb-drives)

Żeby zamontować USB, można wesprzeć się poleceniem  `lsubs -t` z pakietu `usbutils`. Pendrive polecam uprzednio
sformatować do ext4 bądź FAT32. Nośnik pamięci z jedną partycją powinien pojawić się od razu po podłączeniu pod
ścieżką `/dev/sda` z partycją `/dev/sda1`.
Najpierw warto zamontować sobie wszystko ręcznie, żeby upewnić się, że wszystko działa.

```shell
mkdir /mnt/usb
mount /dev/sda1 /mnt/usb
cd /mnt/usb
ls
touch dupa
ls
```

Następnie można zabrać się za ustawienie montowania automatycznie podczas startu systemu:

```shell
block detect | uci import fstab
uci set fstab.@mount[-1].enabled='1'
uci commit fstab
service fstab boot

```

Następnie sprawdzamy, czy dysk/pendrive zamontował się poprawnie.

Polecam doczytać na wiki, są tam ciekawe opcje takie jak sprawdzanie filesystemu podczas montowania albo spindown HDD (
który przy nagrywarce raczej się nie przyda).

### Zaklęcie ffmpeg

Kolejnym krokiem po skompilowaniu obrazu jest ustawienie nagrywania w moim przypadku było to takie zaklęcie dla `ffmpeg`
w wersji 5.x. Dobrze jest to polecenie potestować sobie na lokalnym komputerze, ponieważ może ono wymagać dopracowania w
przypadku innych kamer czy innych potrzeb. Należy przy tym pamiętać, że **kolejność parametrów ma znaczenie!**

```shell
ffmpeg \
    -timeout 5000000 \
    -hide_banner \
    -y \
    -loglevel info \
    -rtsp_transport tcp \
    -use_wallclock_as_timestamps 1 \
    -i "rtsp://admin:*******@NOMI-IPC-F26F:554/cam/realmonitor?channel=1&subtype=0&unicast=true&proto=Onvif" \
    -vcodec copy \
    -acodec copy \
    -f hls \
    -tag:v hvc1 \
    -hls_playlist 1 \
    -hls_list_size ${FRAGMENTS} \
    -hls_flags delete_segments \
    -reset_timestamps 1 \
    -strftime 1 \
    -hls_segment_filename %Y%m%dT%H%M%S.ts \
    -hls_time ${FRAGMENT_TIME} \
    all.m3u8
```

Nie rozumiem do końca każdej z opcji, ale postaram się rozpisać to, co zrozumiałem:

- `timeout` - ta opcja jest niezbędna i niezbędnie musi stać gdzieś na początku (`ffmpeg` zwraca uwagę na kolejność
  parametrów). Dzięki niej po urwaniu się streama komenda nie czeka w nieskończoność i jest w stanie zrzucić na dysk
  ostatni, niepełny fragment. To jest kluczowa opcja, jeśli chcemy, żeby nagrywarka była użyteczna, zwykle to ostatnie
  chwile przed odcięciem zasilania są najważniejsze. W wersji `4.x` ta opcja nazywa się `stimeout`. Jednostka to
  mikrosekundy, stąd 5000000 to 5 sekund.
- `hide_banner` - ukrywa informacje o wersji ffmpeg i tym podobne śmieci
- `y` - nadpisywanie plików bez pytania, może nie jest to najlepsza opcja, ale przy obecnej konfiguracji nigdy nie
  powinno dojść do konfliktu nazw poza nazwą playlisty, którą faktycznie trzeba nadpisać.
- `loglevel` - to jest bardziej do debugowania, co się dzieje w trakcie zapisu. Można tę opcję ustawić na `error` żeby
  pozbyć się zbędnych śmieci z logów.
- `rtsp_transport` - wymusza wysyłanie danych po protokole TCP zamiast UDP (chociaż u mnie chyba domyślnie i tak stream
  leciał po TCP)
- `use_wallclock_as_timestamps` - jeśli dobrze pamiętam, to pozwalało to używać czasu komputera, jako timestampów. Ktoś
  gdzieś pisał, że pomaga to na problemy z przewijaniem nagranego wideo
- `i` - input, skąd brać dane. Tego stringa znalazłem gdzieś w internecie dla mojego modelu kamery, bardzo możliwe, że
  da się go wyciągnąć za pomocą programu [ONVIF Device Manager](https://sourceforge.net/projects/onvifdm/).
- `vcodec` - kodek video, musi być ustawiony na `copy` co oznacza, że będzie zrzucany do plików bez konwersji
- `acoded` - kodek audio, jw.
- `f` - jakiego fragmentera używać, tutaj `hls` jest nan potrzebny ze względu na możliwość nagrywania w pętli i
  kasowania najstarszych plików automatycznie podczas zapisu nowych
- `tag:v hvc1` - `ffmpeg` wyświetlał komunikat, że stream najprawdopodobniej to `hvc1` i żeby podać tę opcję explicite.
- `hls_playlist` - czy tworzyć playlistę hls, jest ona potrzebna do odtwarzania fragmentów razem oraz do utrzymywania
  stałej ilości fragmentów na dysku
- `hls_list_size` - jak długa ma być lista ostatnich fragmentów
- `hls_flags delete_segments` - ta flaga mówi o tym, żeby kasować fragment z dysku, jeśli jest kasowany z listy
- `reset_timestamps` - to też jest jakaś opcja pomagająca na problemy z przewijaniem
- `strftime` - dzięki tej opcji będzie można użyć nazwy pliku, która zawiera elementy daty oraz godziny
- `hls_segment_filename` - template, według którego mają być nazywane pliki fragmentów. W moim przypadku pełna data i
  godzina wraz z sekundami (żeby uniknąć nakładania się nazw plików przy fragmencie krótszym niż 2 minuty)
- `hls_time` - długość fragmentu w sekundach


