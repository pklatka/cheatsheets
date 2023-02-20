# Bazy danych
Uwaga: zawarte tu informację mogą być błędne lub przestarzałe, korzystasz tylko na własną odpowiedzialność!

## Podstawowe pojęcia
Baza danych – kolekcja powiązanych elementów danych w ramach określonego procesu biznesowego lub podstawionego problemu

System Zarządzania Bazami Danych (DBMS) - pakiet oprogramowania używany do definiowania, tworzenia, używania i utrzymywania bazy danych

Połączenie DBMS i bazy danych jest często nazywane systemem bazodanowym (SZBD)

Kiedyś było na plikach

• Przechowywanie zduplikowanych lub nadmiarowych informacji
• Niebezpieczeństwo niespójnych danych 
• Silne powiązanie między danymi i aplikacjami 
• Trudno zarządzać kontrolą współbieżności 
• Aplikacje trudne do zintegrowania

Koncepcyjny model danychz apewnia wysokopoziomowy opis elementów danych wraz z ich charakterystykami i relacjami

Logiczny model danych to translacja lub mapowanie konceptualnego modelu danych na określone środowisko implementacyjne

Zewnętrzny model -> widoki

Architektura trójwarstwowa
- Zewnętrzna warstwa -> widoki
- Konceptualna/Logiczna warstwa
- Wewnętrzna warstwa np. jak reprezentowane są rekordy

Katalog
- Jądro DBMS
- Definicje danych, metadane, widoków, logiczne i wewnętrzne modele

DDL a DML -> create table, alter table, a DML to CRUD

Fizyczna niezależność danych - aplikacje nie muszą być zmieniane gdy zmieni się model zapisu

Logiczna niezależność danych - nie trzeba zmieniać aplikacji na poziomie logicznym/konceptualnym

Model danych to jawna reprezentacja elementów danych wraz z ich charakterystykami i zależnościami

Koncepcyjny model danych powinien zapewniać formalne i doskonałe odwzorowanie wymagań dotyczących danych w procesie biznesowym i jest tworzony we współpracy z użytkownikiem biznesowym

Dane ustrukturyzowane np. tabela
Dane nieustrukturyzowane np. dokument z biografiami
Dane częściowo ustrukturyzowane


Reguły sytaktyczne - sposób przechowywania danych
Reguły semantyczne - poprawność danych

ACID
- atomowość - coś niepodzielnego
- spójność - przenosi jeden stan do drugiego stanu bazy
- izolacja - efekt równoległych operacji powinien być taki sam jak byłyby wykonywane w izolacji
- trwałość - zmiany muszą być trwałe


Architektura DBMS

### Model ER

Baza danych - zbiór relacji

Relacja -> zbiór krotek

Krotka (tabela) -> lista wartości atrybutów, opisująca aspekt encji

Model ER -> Model relacyjny
Typ encji -> relacja
Encja -> krotka
Typ atrybutu -> nazwa kolumny
Atrybut -> Kolumna

Wartość NULL oznacza, że brakuje wartości, jest ona nieistotna lub nie dotyczy.

Relacja jest zbiorem bez duplikatów i bez porządkowania
Atrybut musi być wartością niepodzielną

### Klucze

Nadklucz - podzbiór typów atrybutów relacji R (kilka kluczy)

Klucz główny - do identyfikacji rekordów

Klucze alternatywne - klucze kandydujące do bycia kluczem głównym

Klucz obcy - referencja do innego rekordu w innej relacji

Ograniczenia relacyjne
- ograniczenie dziedziny
- ograniczenie klucza (każda relacja ma klucz)
- ograniczenie integralności encji (klucz główny zawsze not null)
- klucz obcy ma tą samą dziedzinę co PK do którego się odwołuje

### Encje

Typ encji to określenie encji np. student
Encja to określone wystąpienie lub instancja typu encji


### Typ atrybutu

- dziedzina (z wartościami null, nie są przedstawiane w ER)

### Atrybut klucza

- odpowiednik kolumny
- atrybuty mogą być złożone, czyli takie, które można dalej rozłożyć na atrybuty proste (atomowe, pochodne)


### Typy związków

- stopień -> liczba typów encji uczestniczących w typie związków
- współczynnik liczności
- 1:1, 1:n, n:1, m:n
- mogą mieć atrybuty!
- silny typ encji posiada atrybut klucza
- słaby nie ma własnego klucza i jest zależny zawsze od jego właściciela

### Model ER

- nie może zagwarantować spójności
- dziedziny nie są uwzględnione
- funkcje nie są uwzględnione

### Model EER

- Specjalizacja: definiowanie zbioru podklas encji np. Artist i podklsy singer, actor. Udoskonala koncepcję
- Generalizacja: definiowanie superklas. Udoskonala koncepcję.
- Ograniczenie rozłączności: do jakich podklas może należeć encja nadklasy
- Ograniczenie kompletności: czy wszystkie nadklasy powinny należec do jednej z podklas
- Każda podklasa może mieć tylko jedną nadlklasę i dziedziczy wszystkie atrybuty ze wszystkich nadklas (ale czasem może mieć więcej, dobre slajdy XD)


Kategoryzacja
- Kategoria to podklasa, która ma kilka nadklas

Agregacja
- Łączenie typów encji


### Normalizacja

- Dekompozycja - podzielenie na dwa schematy
- Zależności funkcyjne: X -> Y (X jednoznacznie określa Y np. ID określa nazwę produktu)

- Wytyczne normalizacji
    - łatwo wyjasnic znaczenie modelu
    - atrybuty z wielu typów encji nie powinny byc łączone
    - unikać nadmiernej ilości wartości NULL

Postacie normalne


### SQL tips

– PRIMARY KEY ograniczenie definiuje klucz główny tabeli
– FOREIGN KEY ograniczenie definiuje klucz obcy tabeli
– UNIQUE ograniczenie definiuje klucz alternatywny tabeli
– NOT NULL ograniczenie zabrania wartości NULL dla kolumny
– DEFAULT ograniczenie ustawia domyślną wartość dla kolumny
– CHECK ograniczenie definiuje ograniczenie na wartościach kolumn

- ON UPDATE/DELETE CASCADE: aktualizacja/usunięcie powinno
być kaskadowane do wszystkich odnoszących się krotek
- ON UPDATE/DELETE RESTRICT: aktualizacja/usuwanie jest
zatrzymywane, jeśli istnieją krotki odnoszące się 
- ON UPDATE/DELETE SET NULL: klucze obce w odsyłających
krotkach są ustawiane na NULL
- ON UPDATE/DELETE SET DEFAULT: klucze obce w odsyłających krotkach są ustawione na ich domyślną wartość

- blob/clob -> do dużych danych

- CREATE TYPE
- CREATE DOMAIN - dziedziny wartości (definiowanie juz constraintów)

- Materializacja widoku -> widok aktualny do stanu bazy danych
- Widoki można modyfikować

- WITH CHECK -> Sprawdza instrukcje update z widokiem

Indeksy
- klastrowe -> grupujące
- nieklastrowe -> wartości unikalne w tabeli

- INSTEAD OF -> działanie zamiast np. insert w triggerach
- trigger DDL -> create, alter, drop, grant itd

- Uprawnienia
    - read
    - insert
    - update
    - delete

- with grant option -> przekazywanie uprawnień

### Przechowywanie danych

Media fizyczne tworzą hierarchię pamięci składającą się z:
– pamięci operacyjnej o organizacji blokowej 
– pamięci zewnętrznej o organizacji plikowej (bufory)

Rekord może mieć stałą wielkość lub zmienną

Update rekordów
- przesuwanie
- przesunięcie ostatniego rekordu do usuniętego
- zapamiętywanie pustych pól w pamięci


Bufor - porcja pamięci głównej (taki cache)

Gdy brakuje miejsca w buforze – najczęściej usuwany blok najdawniej używany (LRU strategy)

### Transakcje

- jawne: begin/end transaction
- niejawne: normalne zapytanie SQL

### Plik dziennika
- rejestruje obrazy przed rekordów biorących udział w transakcji
- obrazy po rekordów po transakcji
- checkpoints - momenty w których aktualizacje buforowane przez aktywne transakcje, obecne w buforze bazy danych, są natychmiast zapisywane na dysk
- strategia zapisu dziennkia do przodu:
    - aktualizacje są zapisywane przed zapisaniem na dysku
    - before images są rejestrowane przed nadpisaniem plików na dysku

Awarie
- systemu
- transakcji
- nośnika

Odtwarzanie nośników
- mirroring dysku
- archiwizacja

### Współbieżność i jej problemy

- problem utraconej aktualizacji - udana aktualizacja została nadpisana przez inną
- problem niezatwierdzonych zależności - odczytywanie wartości przez transakcję w trakcie aktualizacji wartości przez inną (uncommited dependency problem) -> gdy np. transakcja się skończy ale potem nagle bedzie rollback i reset do starych wartości
- problem analizy niespójności - odczyt częściowych danych podczas wykonywania innej transakcji
- niepowtarzalny odczyt -  występuje gdy transakcja T1 odczytuje ten sam wiersz wiele razy, ale uzyskuje różne kolejne wartości, ponieważ w międzyczasie inna transakcja T2 zaktualizowała ten wiersz
- odczyt fantomów -  może wystąpić, gdy transakcja T2 wykonuje operacje wstawiania lub usuwania na zestawie wierszy odczytywanych przez transakcję T1

### Harmonogramy
- sekwencyjne uporządkowanie transakcji 
- szeregowy -> jeden po drugim -> uniemożliwiają równoległość
- szeregowalne -> niesekwencyjny harmonogram
- graf pierwszeństwa - testowanie harmonogramu pod kątem szeregowalności -> jeżeli zawiera cykl to harmonogram nie jest szeregowalny

Protokoły
- optymistyczny
- pesymistyczny -> kolidowanie transakcji

Blokowanie
– Pesymistyczne harmonogramowanie: blokowanie służy do ograniczenia jednoczesnego wykonania transakcji
– Optimistyczne harmonogramowanie: blokowanie służy do wykrywania konfliktów podczas realizacji transakcji

Cel blokowania -> unikanie konfliktów

Blokady
- na wyłączność -> pojedyncza transakcja uzyskuje wyłączne uprawnienie do interakcji z tym konkretnym obiektem bazy danych w danym czasie
- współdzielona -> gwarantuje, że żadne inne transakcje nie zaktualizują tego samego obiektu tak długo, jak utrzymywana jest blokada

Jeśli transakcja chce zaktualizować obiekt, wymagana jest blokada na wyłączność.

Menadżer blokad -> implementuje protokół blokowania

2PL -> dwufazowe blokowanie
- uzyskanie współdzielonej blokady obiektu przez transakcje
- czy mozna przyznać blokadę na podstawie macierzy kompatybilności
- warianty
    - rygrystyczny -> utrzymywanie blokad do zatwierdzenia transakcji
    - statyczny -> wszystkie blokady na początku transakcji

Zakleszczenie (deadlocks) występuje, gdy 2 lub więcej transakcji czeka na zwolnienie swoich blokad

Jak sobie z nimi radzić -> graf oczekiwań lub wybór ofiary
Można uniknąć zakleszczenia korzystając z statyczego 2PL

Poziomy izolacji
- 2PL zbyt rygorystyczny
- Blokada długoterminowa jest przyznawana i zwalniana zgodnie z protokołem i jest utrzymywana przez dłuższy czas, aż do zatwierdzenia transakcji
- Blokada krótkoterminowa jest utrzymywana tylko w przedziale czasu potrzebnym do zakończenia powiązanej operacji
- niezatwierdzony odczyt 0 -> najniższy boziom izolacji (konflikty nie występują), tylko dla odczytu
- odczyt zatwierdzonych danych 1 -> długoterminowe blokady zapisu i krótkoterminowe odczytu
- powtarzalne odczyty -> dłgoterminowe blokady odczytu i zapisu
- serializowany 3 -> 2PL

Ziarnistość blokad MGL
-  zapewnia, że odpowiednie transakcje, które uzyskały blokady na obiektach bazy danych, które są ze sobą powiązane hierarchicznie, nie mogą być ze sobą w konflikcie
- wprowadza dodatkowe blokady

### Architektury

- Scentralizowana architektura
- Warstwowa architektura
- Wariant „gruby” klient
- Wariant „cienki” klient -> logika prezentacji tylko przez klienta
- Architektura trójwarstwowa: oddziela logikę aplikacji od DBMS i umieszcza ją w osobnej warstwie (tj. serwer aplikacji)

Wiązanie SQL -> odnosie się do tłumaczenia kodu SQL na reprezentację niższego poziomu, która może być wykonana przez SZBD po wykonaniu zadań takich jak walidacja nazwy tabeli i pól, sprawdzenie, czy użytkownik lub klient ma wystarczające prawa dostępu i wygenerowanie planu zapytania aby uzyskać dostęp do danych fizycznych w najbardziej wydajny sposób.
- wczesne -> prekompilator
- późne -> w czasie wykonywania




### Dodatkowe informacje

Relacja=tabela=typ encji
Krotka=rekord=encja
Nazwa kolumny = typ atrybutu
atrybut = komórka


Ograniczenie dziedziny - wartość atomowa
Ograniczenie klucza - każda krotka musi miec klucz główny
Ograniczenie integralności encji - PK != NULL
Ograniczenie integralności referencyjneij - FK != NULL && domain(FK) == domain(PK)

Definicja NULL
null: 1)wartość nieznana 2) wartość istnieje, ale nie jest dostępna 3) atrybut nie dotyczy tej krotki

Atrybuty
- proste (atomowe)
- złożone -> można rozłożyć na atomowe
- jednowartościowe np. numer 
- wielowartościowe np. email
- pochodne -> można wyprowadzić z innego atrybutu

Słaby typ encji -> bez klucza głównego, np. tabela do obsługi relacji M:N


Ograniczenia ER:
- brak dziedzin
- brak funkcji
- brak spójności

Dekompozycja beztratna -> brak utraty informacji po rozdzieleniu tabel

Zależność funkcyjna X -> Y x określa y

X->Y jest trywialna gdy Y jest podzbiorem X

Zbiór wszystkich zależności funkcyjnych wynikających z F to F+


Reguły Amstronga
- reguła zwrotności
- rozszerzalności
- przechodniości
- unii
- dekompozycji
- pseudoprzehodniości


Dekompozycja jest bezstratna gdy przynajmniej jedna z zależności jest w F+



2NF -> zupełna zależność funkcyjna -> każdy atrybut nie-podstawowy jest zależny zupełnie od dowolnego klucza w relacji.

3NF -> przechodnia zależność funkcyjna -> żaden atrybut podstawowy nie jest zależny przejściowo od klucza głównego

BCNF -> trywialna zależność funkcyjna -> każda z nietrywialnych zależnośći funkcyjnych X->Y, X: jest nadkluczem, X jest albo kluczem kandydującym albo jego nadzbiorem

Relacja jest w 4 NF, jeżeli jest w BCNF i dla każdej z jej nietrywialnych zależności wielowartościowych X →→ Y, X jest nadkluczem—to znaczy X jest albo kluczem kandydującym lub jego nadzbiorem

=--=

Blok może zawierać wiele rekordów

Atrybuty zmiennej długośći opisywane są jako para (offset, długość)

Najpierw zapisywane są atrybuty o stałej długości

Cel SZBD: minimalizacja liczby transferów bloków między dyskiem a pamięcią

