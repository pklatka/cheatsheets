# SQL Cheatsheet
Uwaga: zawarte tu informację mogą być błędne lub przestarzałe, korzystasz tylko na własną odpowiedzialność!

## Podstawowe komendy

### Wyszukiwanie rekordów w bazie

```sql
SELECT [ALL | DISTINCT] <select_list>
FROM {<table_source>} [, …n]
WHERE <search_condition>
-- DISTINCT działa jak set() w pythonie
```

### Klauzula WHERE

- Operatory porównań: =, >, <, >=, <=, <> ( != )
- Porównywanie stringów: (NOT) LIKE
- Operatory logiczne: AND, OR, NOT
- Zakres wartości: (NOT) BETWEEN
- Listy wartości: (NOT) IN
- Nieznane wartości IS (NOT) NULL
- Nawiasy mają znaczenie

### Porównywanie stringów

Operator like działa tylko dla danych typu:
char, nchar, varchar, nvarchar,
binary, varbinary, smalldatetime, datetime,
oraz pod pewnymi założeniami dla text, ntext, image.

| Znak specjalny    | Opis                | Przykład  |
|-------------------|---------------------|-----------|
| %                 | 0 lub więcej znaków | 'Br%' |
| _                 | pojedynczy znak     |  'Br_n' |
| [] | pojedynczy znak z zakresu|  [CK], [S-V] |
| [^]     | pojedynczy znak z poza zakresu | [^c], [^S-V] |

### Sortowanie danych

Komenda ORDER BY <column name> [ASC | DESC]
Domyślnie sortowanie odbywa się rosnąco.

```sql
SELECT [ALL | DISTINCT] <select_list>
FROM {<table_source>} [, …n]
WHERE <search_condition>
ORDER BY < column_name > [ASC | DESC]
```

### Zmiana nazwy kolumn

Komenda AS <new_column_name>

```sql
SELECT firstname AS First, 
    lastname AS Last, 
    employeeid AS 'Employee ID:'
FROM employee
```

### Kolumny wyliczane

W każdym zapytaniu możemy dokonywać podstawowe działania matematyczne jak i konkatenacje stringów (varcharów).

```sql
SELECT orderid,
       unitprice * 1.05 as newprice,
       firstname + ' ' + lastname
FROM Orders
```

## Modyfikacja wyświetlanych danych

Wszystkie przydatne funkcje można znaleźć [tutaj](https://www.w3schools.com/sql/sql_ref_sqlserver.asp).

## Agregacja danych

### Wyświetlanie pierwszych n wartości

Komenda TOP [number]

```sql
SELECT TOP 5 orderid
FROM orders
ORDER BY quantity DESC
```

### Funkcje agregujące

| Funkcja agregująca | Opis                                        |
|--------------------|---------------------------------------------|
| AVG                | średnia wartości w wyrażeniu numerycznym    |
| COUNT              | liczba wartości w wyrażeniu                 |
| COUNT (*)          | liczba wybranych wierszy                    |
| MAX                | największa wartość w wyrażeniu              |
| MIN                | najmniejsza wartość w wyrażeniu             |
| SUM                | ogólna wartość w wyrażeniu numerycznym      |
| STDEV              | statystyczne odchylenie wszystkich wartości |
| STDEVP             | statystyczne odchylenie dla populacji       |
| VAR                |statystyczna wariancja dla wszystkich wartości|
| VARP               |statystyczna wariancja dla wszystkich wartości w populacji |

#### Uwagi

- Funkcja `Count(*)` zlicza wiersze z wartościami null, aby temu zapobiec należy użyć `COUNT(reportsto)`

### Grupowanie danych

```sql
SELECT [ALL | DISTINCT] <select_list>
FROM {<table_source>} [, …n]
WHERE <search_condition>
GROUP BY < column_name >
HAVING < condition >
```

#### Przykład

Zapytanie pogrupuje sumy ilości dla każdego id produktu

```sql
SELECT productid, SUM(quantity) AS total_quantity
FROM [
order details]
GROUP BY productid
```

Użycie klauzuli GROUP BY z klauzulą HAVING

```sql
SELECT productid, SUM(quantity) AS total_quantity
FROM orderhist
GROUP BY productid
HAVING SUM(quantity) >= 30
```

### Generowanie wartości zagregowanych w zbiorach wynikowych

#### Użycie klazuli GROUP BY z operatorem ROLLUP

Operator ROLLUP dodaje dodatkowe rekordy z całościową wartością grupowania np. całościowa suma wartości produktów

```sql
SELECT productid, orderid, SUM(quantity) AS total_quantity
FROM orderhist
GROUP BY productid, orderid WITH ROLLUP -- <- użycie operatora
ORDER BY productid, orderid
```

W powyższym przypadku ROLLUP da ogólną ilość:

- każdego produktu w każdym zamówieniu
- *wszystkich* produktów dla każdego zamówienia
- *wszystkich* produktów dla wszystkich zamówień

#### Użycie klauzuli GROUP BY z operatorem CUBE

Operator CUBE działa podobnie jak ROLLUP, ale dodatkowo zwróci rekordy ze wszystkimi kombinacjami grupowań
(wygeneruje dodatkowo 2<sup>n</sup> wpisów)

#### Użycie funkcji GROUPING

Funkcja GROUPING zwraca 1, gdy wartości w określonej kolumnie zostały pogrupowane razem
operatorem CUBE.

```sql
SELECT productid,
       GROUPING(productid),
       orderid,
       GROUPING(orderid),
       SUM(quantity) AS total_quantity
FROM orderhist
GROUP BY productid, orderid WITH CUBE
ORDER BY productid, orderid
```

## Łączenie danych z wielu tabel

### Łączenie danych jako iloczyn kartezjański

Iloczyn kartezjański <-> jeden rekord z tabeli_1 posiada len(tabela_2) wpisów.

```sql
SELECT b.buyer_name AS [b.buyer_name],
b.buyer_id AS [b.buyer_id],
s.buyer_id AS [s.buyer_id],
qty AS [s.qty]
FROM buyers AS b, sales AS s
WHERE b.buyer_name = ‘Adam Barr’ -- <- warunek złączenia
```

### Operator JOIN

- Słowo kluczowe JOIN wskazuje, że tabele są łączone i określa w
  jaki sposób
- Słowo kluczowe ON specyfikuje warunki połączenia

Składnia:
`[INNER|{{LEFT|RIGHT|FULL}[OUTER]}] [<join_hint>] JOIN <table> ON <search_condition>`

#### INNER JOIN

Można interpretować jako część wspólna dwóch tabel, tzn. INNER JOIN zwróci rekordy, których wartości są takie same w
obu tabelach (... ON table1.c1 = table2.c2 - wartości muszą być takie same).

Jeżeli chcemy połączyć więcej niż jedna tabela wystarczy użyć polecenia więcej niż jeden raz.

#### CROSS JOIN

Zwraca wszystkie rekordy z dwóch tabel. Jeżeli użyjemy WHERE to działa jak INNER JOIN.

```sql
SELECT column_name
FROM table1
         CROSS JOIN table2
```

#### SELF JOIN

Łączy tabelę samą ze sobą.

```sql
SELECT a.buyer_id AS buyer1
     , a.prod_id
     , b.buyer_id AS buyer2
FROM sales AS a
         JOIN sales AS b
    SQL – operacje zł
ączenia
ON a.prod_id = b.prod_id
WHERE a.buyer_id < b.buyer_id
```

#### Operator UNION

Do tworzenia pojedynczego zbioru wynikowego z wielu zapytań. Każde zapytanie musi mieć:

- zgodne typy danych
- taką samą liczbę kolumn
- taki sam porządek kolumn w select-

```sql
SELECT (firstname + ' ' + lastname) AS name
     , city
     , postalcode
FROM employees
UNION
SELECT companyname, city, postalcode
FROM customers
```

#### LEFT JOIN

Zwraca wszystkie rekordy z lewej tabeli (table1) i pasujące rekordy z prawej tabeli (table2).

```sql
SELECT column_name
FROM table1
         LEFT JOIN table2 ON table1.column_name = table2.column_name
```

#### RIGHT JOIN

Działa analogicznie jak LEFT JOIN ale zwraca wszystkie rekordy z tabeli prawej (table2) i zwraca pasujące z tabeli
lewej (table1).

## Podzapytania

### Podstawowe podzapytania

W miejscu FROM wstawiamy podzapytanie:

```sql
SELECT T.orderid, T.customerid
FROM (SELECT orderid, customerid
      FROM orders) AS T

```

Podzapytanie może być traktowane jako wyrażenie (musi zwracać tylko jedną wartość):

```sql
SELECT title
     , price
     , (SELECT AVG(price) FROM titles)         AS average
     , price - (SELECT AVG(price) FROM titles) AS difference
FROM titles
WHERE type = 'popular_com'
```

### Operator (NOT) EXISTS

Zewnętrzne zapytanie testuje wystąpienie (lub nie) zbioru
wynikowego określonego przez zapytanie wewnętrzne.

Uwaga: zapytanie wewnętrzne zwraca TRUE lub FALSE.

```sql
SELECT lastname,
       employeeid
FROM employees AS e
WHERE EXISTS(SELECT *
             FROM orders AS o
             WHERE e.employeeid = o.employeeid
               AND o.orderdate = '9/5/97')
```

