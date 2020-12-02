---
layout: post
title:  "Moje projekty"
date:   2020-11-30
categories: PL software-engineering
header_image : /assets/img/moje_projekty/header.jpg
image: /assets/img/moje_projekty/header_big.jpg
---

W tym artykule chciałbym zebrać wszystkie swoje projekty, którymi mogę się pochwalić.
Projektów zaczętych i nieskończonych było całe mnóstwo, ale tu znajdzie się miejsce jedynie
dla tych, które są doprowadzone przynajmniej do etapu PoC.

## Dostanesie.pl

![logo dostanesie](/assets/img/moje_projekty/dostanesie.png)

Mój najbardziej znany projekt. Wpisujesz wyniki z matury, serwis podstawia dane do wzorów liczących punkty i sprawdza,
jak otrzymany wynik ma się do progu punktowego z ubiegłego roku.

Historia tego projektu zasługuje na własny wpis, tu będę musiał ograniczyć się do ogółów.
Wspomnę tylko na wstępie, że w projekt robię wspólnie z [Davasny](https://github.com/Davasny),
a zaangażowanych jest jeszcze parę osób.

Projekt jak każdy inny zaczął się z pewnej potrzeby - miałem młodszego znajomego, który właśnie napisał maturę i borykał się
z problemem gdzie dostanie się na studia z wynikami, jakie szacował, że dostanie. Sam przechodziłem to parę lat wcześniej -
mnóstwo niezrozumiałych wzorów, dziwne tabelki, przeliczniki dla matury, która straciła ważność 5 lat temu, a to tylko w jednej uczelni.
Każda uczelnia ma swoje wzory, często też różne dla każdego kierunku.

Drugim problemem był punkt odniesienia - super, mam 85,56 punka rekrutacyjnego na 100 możliwych i co dalej?
Niestety szukanie progów punktowych jest tu kolejnym wyzwaniem, nawet powiedziałbym, że największym. 
Tylko niektóre uczelnie publikują takie informacje, często też te strony przestają być aktywne po paru miesiącach
lub najzwyczajniej w świecie takie dane nie są oficjalnie dostępne i trzeba szukać ich na własną rękę na grupkach na Facebooku,
forach internetowych lub nawet brać bezpośrednio z tabel przyjęć wywieszanych na wydziałach (tak, zbieraliśmy też dane w ten sposób). 

Zacząłem z nudów przepisywać wzory do Pythona, część z nich okazała się wtedy znacznie prostsza, a nawet można było znaleźć powtarzające
się fragmenty. Progi punktowe dla kilku większych i mniejszych uczelni też udało się znaleźć. Więc skoro można pomóc jednej
osobie, to można pomóc też innym. Davasny skleił pierwszą wersję frontendu, ja napisałem API w Sanic (bo tak).

Portal opublikowaliśmy i posłaliśmy informację na grupki maturalne, co spotkało się z ciepłym przyjęciem.
Projekt cieszy się sporym zainteresowaniem i zdobył uznanie maturzystów. Nawet, teraz gdy tematy maturalne są dość odległe w czasie,
na stronę wchodzi około 500-1000 osób tygodniowo. 

Od 2018 roku projekt przeszedł 2 zmiany backendu. Najpierw z Sanic na Flask, potem z Flask na Django.
Wzory w API były początkowo hardcodowane, co bardzo utrudniało utrzymanie wersji z paru lat wstecz lub kilku tur rekrutacji.
W trakcie przenoszenia backendu do wersji drugiej przenieśliśmy wzory do bazy danych. Było to chyba największym wyzwaniem
od strony programistycznej, szczególnie jeśli mieliśmy zapewnić zbliżoną wydajność do pierwszej wersji.
Jak można sprawdzić - udało się.
Frontend przeszedł wiele iteracji, obecna wersja jest już chyba 6 z kolei. Front przeszedł drogę od surowego JS z jQuery
przez różne frameworki, żeby finalnie skończyć w React. 

Projekt jako jedyny z tej listy nie jest open-source. 
Ma za to pełne CI/CD na gitlabie, jest skonteneryzowany oraz jest HA, ale trzeba mi tu uwierzyć na słowo :D

Sam serwis dostępny jest pod adresem: [dostanesie.pl](https://dostanesie.pl)

## Cebulo Bot

![logo Cebulo Bota](/assets/img/moje_projekty/cebulo_bot.png)

CebuloBot to prosty scraper połączony z botem do komunikatora telegram. Scraper pobiera cykliczne promocje z 
kilku polskich sklepów internetowych i wysyła je na kanał wiadomości.

Pomysł na cebulobota powstał kiedy zacząłem regularnie sprawdzać cykliczne promocje. 
Jeśli dobrze pamiętam, to pierwsze sklepy, jakie miały promocje to był [x-kom](https://x-kom.pl) o godzinie 10:00 oraz 22:00,
satysfakcja.pl (obecnie [al.to](http://al.to)) o godzinie 9:00 oraz 21:00. Na promocjach dobre rzeczy znikały w błyskawicznym tempie, 
a ja nie zawsze pamiętałem o tym, żeby odwiedzić stronę i kilka potrzebnych mi fantów zniknęło mi sprzed nosa. 
Jako że moja praca zawodowa polegała w dużej mierze na scrapowaniu, szybko udało mi się wyłuskać potrzebne dane i 
bot stanął na nogi. 

Przez długi czas bot stał na [mikrusie](https://mikr.us) - darmowym serwerze u Unknowa i pojawił się 
również w jednym z wydań ["Unknown news"](https://news.mrugalski.pl/post/175843387007/13-lipca-2018), 
co znacznie powiększyło ilość osób subskrybujących kanał. 
Od pewnego momentu bot zaczął gubić promocje, przyczyną okazały się parametry darmowego serwera, a dokładniej mówiąc 
RAM ograniczony do zaledwie 16 MB. To wymusiło małe zmiany i pobieranie promocji jedna za drugą zamiast równolegle. 
W międzyczasie mikrus stracił wszystkie dane, a ja przeniosłem się na swoje serwery.

Obecnie projekt jest rozwijany raz na jakiś czas, głównie gdy przestaną pobierać się promocje z jednego ze sklepów. 
Bot jest ciągle online i można go znaleźć pod adresem: 
[https://t.me/armia_cebulo_boya](https://t.me/armia_cebulo_boya) 
kod źródłowy jest dostępny na moim githubie: 
[https://github.com/Behoston/CebuloBoy](https://github.com/Behoston/CebuloBoy)


## hmm_profile

Mój pierwszy pakiet do Pythona, który robi coś użytecznego ([patrz error-cats poniżej](###error-cats))
Pakiet służy do obsługi plików `.hmm` zawierających profile HMM. 
Po załadowaniu modeli są one obiektami w Pythonie, na których można wygodnie operować.

Ten pakiet to właściwie część mojej pracy magisterskiej i wynikł z potrzeby modyfikacji tych plików. Mając już wiele 
negatywnych doświadczeń z pracą na gołych plikach, postanowiłem to zapakować w wygodne obiekty.

Pakiet jest ciekawy bardziej od strony stacka technologicznego użytego dookoła projektu, niż od strony samego kodu.
To był mój pierwszy projekt, w którym użyłem [Github Actions](https://github.com/features/actions) - wcześniej 
na githubie trzeba było korzystać z zewnętrznych CI/CD. Akcje githuba były wtedy jeszcze ciepłe, 
bo od daty releasu do użycia w hmm_profile minęło około miesiąca.

Udało mi się również zintegrować [zest.releaser](https://zestreleaser.readthedocs.io/en/latest/) z Github Actions,
co nie działa może w 100% perfekcyjnie, ale wystarczy właściwie pamiętać o wypełnieniu changeloga i można wypchnięciem
taga zrobić automatyczny release paczki.

Github Actions oferują też coś na wzór crona - akcje, które odpalają się w określonym momencie (notacja jak w cron).
Dzięki temu byłem w stanie zaimplementować cykliczne testowanie mojego pakietu. Profile HMM dostępne są praktycznie tylko
w jednym miejscu - w [Pfam](https://pfam.xfam.org/). Codziennie więc pobieram plik zawierający całą bazę modeli
i sprawdzam, czy da się je odczytać i zapisać oraz, czy po zapisie i ponownym wcztaniu zgadzają się z tym wczytanym z pliku oryginalnego.
Dzięki temu mam pewność, że mój pakiet działa nawet gdy nie rozwijam go aktywnie i nie puszczam releasów 
(pełny test bazy jest domyślnie pomijany, bo trwa około 6 minut i testowanie tego co zmianę nie ma sensu). 

Do tej pory dostałem całe dwie ★★ od [Jie Zhu](https://github.com/alienzj) oraz [Evan Kiefl](https://github.com/ekiefl)
obaj mają jakiś związek z biologią, więc liczę, że mój pakiet im się przydał, co i tak przerosło moje oczekiwania :P.
Być może po publikacji notki technicznej na temat tego pakietu przyda się większej grupie ludzi, choć temat jest dość niszowy.

Projekt dostępny na moim githubie: [https://github.com/Behoston/hmm_profile](https://github.com/Behoston/hmm_profile)
oraz oczywiście na pypi: [https://pypi.org/project/hmm-profile/](https://pypi.org/project/hmm-profile/)


## Projekty drobne

W tej części znajdą się projekty, które, mimo że działają, są bardzo małe i nie zasługują na nagłówek 2 poziomu :P

### error-cats

Mój pierwszy pakiet. Chciałem ugryźć robienie pluginów do flaska, sanica oraz django, więc postanowiłem
napisać kawałek kodu, który podmienia strony z błędami na obrazki [kotów](https://http.cat/) lub [psów](https://httpstatusdogs.com/)

Projekt można znaleźć na moim githubie: [https://github.com/Behoston/error-cats](https://github.com/Behoston/error-cats)
oraz na pypi: [https://pypi.org/project/Error-Cats/](https://pypi.org/project/Error-Cats/)


### NFA

NFA to akronim od Nickname From Anything. Skrypt służy do generowania nicków na podstawie dowolnego tekstu.
Dzięki temu można wygenerować nickname "w stylu" danego tekstu. Nie pamiętam jak dokładnie to działa :P

Inspiracją do napisania tego kawałka kodu były dostępne w internecie generatory 
(za najlepszy uważam ten podczas tworzenia nowej postaci w Tibii).

Jako przykładowe dane testowe użyłem "Pana Tadeusza". 
Choć generator wypluwa dużo bezużytecznych słów, da się wyłowić z niego coś ciekawego np.:

- Udziewicze
- Gonieszkaj
- Westrof
- Euszlad
- Pierwat

Co prawda nie wszystko nadaje się na nickname, ale przy braku inspiracji każda pomoc się przyda.
