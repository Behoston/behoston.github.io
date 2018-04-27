---
layout: post
title:  "Czy sortowanie (produktów) jest potrzebne?"
date:   2018-03-23
categories: PL software-engineering
---

Aktualnie jestem w trakcie tworzenia serwisu i stanąłem przed, wydawać by się mogło, nic nieznaczącą decyzją:

>Jakie opcje sortowania zaimplementować?

Po pewnych przemyśleniach moja odpowiedź to:

>Prawdopodobnie żadne.

W tym artykule postaram się przedstawić wszystkie wzięte przeze mnie pod uwagę aspekty i "wytłumaczyć się ze swojej **radykalnej** decyzji".

**WAŻNE:** w tym artykule będę rozważał przypadek typowego własnego serwisu z produktami/usługami.


## Inne witryny

Pierwsze pytanie, jakie warto sobie zadać to:
> po czym pozwalają sortować inne strony?


|| Legenda |
| --- | --- |
| **pogrubione** | opcje domyślne |
| \[+/-] | sortowanie zarówno rosnące, jak i malejące |

### [Allegro](https://allegro.pl):
- **trafność (od najlepszej)**
- cena \[+/-]
- cena z dostawą \[+/-]
- popularność (od największej)
- czas do końca \[+/-]

![sortowanie na allegro](/assets/img/sortowanie/allegro.png)
---

### [otomoto](https://www.otomoto.pl):
- **data dodania (od najnowszej)**
- cena \[+/-]
- przebieg \[+/-]
- moc silnika \[+/-]

![sortowanie na otomoto](/assets/img/sortowanie/otomoto.png)
---

### [Apteka doz](https://www.doz.pl/):
- **domyślne**
- cena \[+/-]
- nazwa \[+/-]

![sortowanie na doz](/assets/img/sortowanie/doz.png)
---

### [Elektronika morele](https://www.morele.net):
- **domyślne**
- cena \[+/-]
- popularność (od najwyższej)
- ocena (od najwyższej)
- komentarze (od największej ilości)

![sortowanie na morele](/assets/img/sortowanie/morele.png)
---

#### Z wyżej wymienionych przykładów łatwo wyciągnąć jedną wspólną opcję: *cena*.


## Które opcje wybieramy?

Najczęstszym kryterium sortowania jest oczywiście najniższa cena,
która najprawdopodobniej będzie jedynym słusznym i możliwym sortowaniem w moim, nowo powstającym serwisie.
Warto jednak zastanowić się skąd biorą się pozostałe opcje.


### data dodania

Ta opcja jest przydatna w serwisie, gdzie użytkownicy mogą zamieszczać ogłoszenia.
Najczęściej w tego typu serwisach nikt nie martwi się tym,
że ogłoszenie "wisi" tydzień dłużej niż rzeczywiście powinno.

We własnym serwisie, gdzie to my jesteśmy odpowiedzialni za zawartość,
ta opcja ma bardzo ograniczone znaczenie.


### sortowanie pozytywne

Wśród wszystkich opcji sortowania wyróżnia się grupa tych, które mają tylko jeden kierunek.
Są to parametry, które powinny świadczyć o tym, że produkt jest warty wyboru.
Są to:
 - popularność
 - komentarze
 - ocena

Brak tu opcji sortowania od najgorszej oceny, bo przecież nikt nie chce kupić czegoś kiepskiego.

*Jest to jedyna grupa, z której moim zdaniem warto zaczerpnąć ewentualne dodatkowe opcje,
jednak ich zastosowanie może przyczynić się do utrwalenia słabej sprzedaży mniej popularnych produktów!*


### parametr produktu

Ta opcja jest niewątpliwie użyteczna tam,
gdzie wszystkie produkty/usługi mają jakiś parametr, mogący przyjmować dowolne wartości liczbowe.

Najczęściej jednak serwisy nie oferują jednego rodzaju produktu.
Mimo to, można pokusić się o bardzo ambitne zadanie implementacji sortowania zależnej od cech wspólnych.
Jednak to rozwiązanie najczęściej będzie dość karkołomne, niewydajne i mało intuicyjne,
a do tego może dojść do sytuacji, kiedy takie sortowanie po prostu nie ma sensu.

Tego typu parametry zwykle są obsługiwane przez filtry (zakresowe), a nie sortowanie.


### przebieg/moc silnika/cena

Tu sprawa ma się bardzo podobnie jak w przypadku każdego innego parametru,
ale warto osobno spojrzeć na to, dlaczego nie trafiły do grupy "sortowanie pozytywne".
To właśnie ta grupa generuje bardzo silny argument w dyskusji.

W przypadku ceny, zwykle jest proporcjonalna (zwykle nie wprost) do jakości,
stąd potrzeba wybrania czegoś z wyższej półki.

W przypadku przebiegu i mocy silnika mamy do czynienia z sortowaniem specyficznym dla domeny.
Na pierwszy rzut oka tu również wydawać by się mogło, że rację bytu mają tylko opcje "przebieg - od najmniejszego" oraz "moc silnika - od najwyższej"

Jednak tutaj mamy do czynienia z serwisem, który moderację ma co najwyżej symboliczną, a do tego sprzedawcy bardzo często oszukują w przypadku faktycznego przebiegu.
Co za tym idzie, sortowanie od najwyższego przebiegu zwiększa znacząco szanse trafienia na uczciwego sprzedawcę.

W przypadku mocy silnika ważną rolę może tu odgrywać cena ubezpieczenia pojazdu, im większa moc, tym prawdopodobieństwo rozbicia większe a co za tym idzie, rośnie też cena ubezpieczenia.

Jak widać, są to przypadki, kiedy sortowanie jest bardzo dobrze przemyślane i jak najbardziej potrzebne.
Ta potrzeba wynika z charakteru serwisu. Tak jak zaznaczyłem, moderacja jest znikoma,
a weryfikacja sprzedawców praktycznie nie może mieć miejsca ze względu na skalę serwisu.
Podczas tworzenia serwisu z własnymi produktami/usługami nie spotkamy podobnych problemów,
o ile będziemy w stanie zagwarantować jakość produktów i świadczonych usług.


### sortowanie bez sensu

Takie jak na przykład sortowanie po nazwie wydaje się bardzo intuicyjne,
jednak w praktyce nie sądzę, by znajdowało zastosowanie.
Od wyszukiwania jest wyszukiwarka, a nie scroll użytkownika.
Jeśli, w przypadku apteki, ktoś szuka preparatu na "wu...",
to wyszukiwarka powinna w tym wypadku wyszukiwać po prefiksach (i tak właśnie działa na doz.pl).


### sortowanie "domyślne" lub "największa trafność"

Sortowanie bez nazwy jest moim zdaniem najgorszym możliwym wyborem.
Ta opcja nie mówi nam zupełnie nic o faktycznym porządku prezentowanej listy.
Być może jest to kolejność po rosnących kluczach głównych z bazy (a więc mniej więcej czasie dodania),
być może są to produkty, które najsłabiej schodzą, a może po prostu tak wyszło z bazy, więc tak jest.
Użytkownik nie wie czego się spodziewać, ciężko mu porównać produkty,
a najczęściej i tak zmieni to sortowanie, jeśli nie znajdzie dokładnie tego czego szukał na pierwszej stronie.

W przypadku `największa trafność` może mieć sens, jeśli rzeczywiście serwis,
na którym to sortowanie występuje, ma dostateczną ilość danych,
 aby taki "sprytny" algorytm sortowania zaimplementować
(chociażby na podstawie danych użytkownika, jego poprzednich zakupów, zainteresowań itp.).


### Mój konkretny przypadek

Serwis, który aktualnie tworzę, będzie zajmował się pośrednictwem w sprzedaży usług.
Każda pozycja przed dodaniem na pewno będzie weryfikowana i wiem,
że będę w stanie ręczyć za jakość mimo dużej otwartości na oferty ze strony użytkowników.
Bardzo też chciałbym, żeby zarówno nowe usługi miały niski próg wejścia,
jak i łatwo było im zaistnieć,
ale też bardzo cenię sobie długofalową współpracę,
bo na takich osobach można polegać,
a nierzadko jest to okazja do nawiązania nowych znajomości.

Kompromisem wydaje się osobne miejsce w przestrzeni strony do prezentacji
kilku najlepiej ocenionych i kilku najnowszych pozycji
**koniecznie** spełniających kryteria, których użyto do wyszukiwania.


### Przykłady z realnego życia

Na pewno jest wiele przykładów, gdzie nie ma możliwości sortowania,
jednym z takich przykładów jest [Just Join IT](http://justjoin.it).
Jest to dość młoda, polska strona służąca do wyszukiwania ofert pracy w IT.

Widać tutaj dość dobrze przemyślany UX, są odpowiednie filtry,
stawkę, jaka nas interesuje, można wybrać jako zakres.
Sortowanie jest od ofert najnowszych,
co ma taki sam sens jak w przypadku samochodów czy innych portali z ogłoszeniami.
Po tym, jak na dane stanowisko znajdzie się odpowiednia osoba, oferta nadal wisi,
bo po prostu nie ma sensu jej zdejmować (może jeszcze jakieś ciekawe CV wpłynie do firmy)
albo po prostu jest to któryś z kolei portal, gdzie ogłoszenie zostało umieszczone i zapomniane.


![sortowanie na Just Join IT](/assets/img/sortowanie/justjoinit.png)

---

Kolejnym przykładem jest, również polski, serwis [Ada](https://ada.place).
Jest to portal do wyszukiwania mieszkań.

Filtry są dobrze pomyślane,
ogłoszenia prawdopodobnie przechodzą jakąś weryfikację, a opcje sortowania są dwie:
- od najniższej ceny
- od daty dodania

i moim zdaniem pokrywają wszystkie potrzeby użytkownika co do porządkowania listy ofert.

![sortowanie na Ada](/assets/img/sortowanie/ada.png)


## Czy warto w takim razie w ogóle implementować sortowanie?

Warto, jeśli masz konkretne argumenty za tym, żeby dawać takie opcje użytkownikowi.

**Tym artykułem chciałbym zmusić do zastanowienia się i zmiany podejścia.
Wyjdźmy od jednej opcji: sortowanie od najniższej ceny.**
Przecież nie zależy nam na tym, żeby klient spędził dużo czasu na stronie,
zależy nam na tym, żeby coś od nas kupił i żeby od momentu wyszukania,
do znalezienia minęło jak najmniej czasu.

Jeśli klient szuka konkretnej rzeczy, to wpisze konkretną frazę w wyszukiwarkę na stronie i trafi błyskawicznie,
jeśli dopiero szuka, jego głównym kryterium wyboru będzie cena.

Wyobraźmy sobie klienta internetowej apteki, który kieruje się porządkiem leksykograficznym przy wyborze leku.
Prawdopodobnie taka osoba prędzej odwiedzi wróżbitę niż internetową aptekę. Zbędne opcje sortowania mogą wyglądać po prostu głupio i nieprofesjonalnie.

Jeśli jesteśmy w stanie poświadczyć jakość tego, co chcemy sprzedać
inne sortowanie niż po cenie, może nie mieć sensu.
Po prostu to odpowiada w większości potrzebie klienta,
a my jako sprzedawca mamy za cel te potrzeby zaspokajać.

Sortowanie po popularności może zaszkodzić tym mniej popularnym produktom,
a przecież je też chcielibyśmy sprzedać.
To może mieć znaczenie w przypadku produktów nowych,
które automatycznie wylądują na samym końcu listy.
Wtedy wypadałoby dodać sortowanie od produktów najnowszych,
aby zainteresować klienta nowinkami, więc te opcje powinny iść w parze.


## Podsumowanie

Sortowanie trzeba dobierać z głową. Musimy rozważnie wybierać dostępne opcje,
aby zachować równowagę (np. między nowymi a popularnymi produktami).

Na pewno nie warto dawać opcji sortowania, które nazywa się *domyślne*.
Moim zdaniem jest to brak szacunku dla użytkownika.
Większość osób i tak zmieni to ustawienie na jakiekolwiek,
w którym będzie wiedział czego się spodziewać, po co więc dokładać komuś pracy?

Zbędne opcje tylko wprowadzają niepotrzebny zamęt, a im prostsza obsługa, tym lepsze wrażenia.
Co więcej, bezsensowne opcje mogą wyglądać równie nieprofesjonalnie co flashplayer.

![flashplayer](/assets/img/sortowanie/flashplayer.png)

Często dawanie wyboru opcji sortowania nie jest potrzebne,
warto wtedy jedynie poinformować użytkownika, że lista,
którą widzi, jest już posortowana np. od najtańszego produktu.
