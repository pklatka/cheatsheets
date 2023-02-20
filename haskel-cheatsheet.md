# Haskell Cheatsheet

Uwaga: zawarte tu informację mogą być błędne lub przestarzałe, korzystasz tylko na własną odpowiedzialność!

## Informacje ogólne
- Haskell jest językiem funkcyjnym
- Haskell jest leniwy - będzie obliczał wartości funkcji tylko wtedy gdy faktycznie są potrzebne -> niebezpieczeńśtwo: dłuższy czas wykonywania jakiegoś kodu
- Haskell jest statycznie typizowany, ale posiada mechanizm typów inferencji, dzięki którym kompilator sam domyśli się, jakiego typu jest zmienna


## Typy proste

Operator `::` mówi *ta zmienna jest typu np. Int*.

```haskell
1 :: Int -- (skończona długość reprezentacji)*
123^230 :: Integer -- (dowolne liczy całkowite)
3.141592653589793 :: Double/Float
True :: Bool
'a' :: Char
"Ala" :: String -- String = [Char], typ złożony? :)

let x = 1 -- Ustawienie stałej: let x = 1 w ghci jest równoważne wpisaniu x = 1 w skrypcie i załadowaniu go później
```

## Typy złożone 

```haskell
(1, 'a', True) :: (Int, Char, Bool) -- krotka (tuple)
[1,2,3,4] :: [Int] -- lista
not :: Bool -> Bool -- funkcja
```

## Listy
- Deklaracja: np.` x = [1,2,3,4]`
- Typ String to lista charów `"ALA" = ['A', 'L','A']`
- Konkatenacja list: `[1,2] ++ [3,4] -> [1,2,3,4]`
- Konkatenacja stringów odbywa się za pomocą operatora `++`
- Dodawanie elementu na początek: `5:[1,2,3] -> [5,1,2,3]`
- Pobranie elementu z danego indeksu: `[1,2,3,4] !! 1 -> 2`
- Listy mogą być porównywane, i są w porządku leksykograficznym, tzn. najpierw porównywany jest element na indeksie 0, jeżeli elementy są równe, to porównywany jest następny indeks, itd. Przykład: `[3,4,2] > [3,2,1] -> True`
- Funkcje na listach (uwaga: tail zwraca element bez head, init bez last):
  - `head [5,4,3,2] -> 5`
  - `tail [5,4,3,2] -> [4,3,2]`
  - `init [5,4,3,2] -> [5,4,3]`
  - `last [5,4,3,2] -> 2`
  - `length [5,4,3,2] -> 4`
  - `null [5,4,3,2] -> False`
  - `take 2 [5,4,3,2] -> [5,4]` (bierze x elementów z przodu)
  - `drop 2 [5,4,3,2] -> [3,2]` (działa jak take ale od tyłu)
  - `elem 2 [5,4,3,2] -> True` (czy dany element jest w liście, uwaga: może być zapisany w postaci infiksowej, np. 2 \`elem\` [2])
  - `maximum`, `minimum`, `sum`, `product` (mnożenie)

## Ranges w listach
- Automatyczne uzupełnianie list, np. `[1..5]` zwróci` [1,2,3,4,5]`
- Domyślnie ranges zawsze inkrementuje liczby o 1
- Można dodawać krok w ranges, np. `[1,3..7] -> [1,3,5,7]`; `[5,4..1] -> [5,4,3,2,1]`
- Uwaga: możemy tworzyć nieskończone listy, bo Haskell jest leniwy, np. [2,4..] i np. `take 5` zwróci `[2,4,6,8,10]`, drugi przykład: `[1..]` zwróci `[1,2,3,4,...]`
- Funkcje tworzące listy nieskończone:
  - `take 10 (cycle [1,2,3]) -> [1,2,3,1,2,3,1,2,3,1]  `
  - `take 10 (repeat 5) -> [5,5,5,5,5,5,5,5,5,5]` (repeat to taki cycle z jednym elementem)
  - `replicate 3 10 -> [10,10,10]` (lepsza funkcja niż repeat) 

## List comprehensions
- Przykład: `[x*2 | x <- [1..10]] -> [2,4,6,8,10,12,14,16,18,20]  `
- Dodawanie warunków dla liczb: `[x*2 | x <- [1..10], x*2 >= 12] -> [12,14,16,18,20]  `
- Możemy korzystać również z zapisanych już list, np. `[ if x < 10 then "BOOM!" else "BANG!" | x <- xs, odd x]  ` 
- Możemy korzystać z więcej niż dwóch list, np. `[ x*y | x <- [2,5,10], y <- [8,10,11]] -> [16,20,22,40,50,55,80,100,110]` (iloczyn kartezjański)
- Mechanizm działa również dla list zagnieżdżonych, np. `[ [ x | x <- xs, even x ] | xs <- xxs] -> [[2,2,4],[2,4,6,8],[2,4,2,6,2,6]]  `


## Krotki
- Może zawierać różne typy danych
- Funkcje na krotkach (tylko dwuelementowych!):
  - `fst (8,2) -> 8`
  - `snd (8,2) -> 2`
  - `zip [1,2,3] ['a','b','c'] -> [(1,'a'), (2,'b'), (3, 'c')]`

## Operatory

Uwaga: Operatory to również funkcje!
np. a + b  <=>  (+) a b

- a == b, a /= b
- a < b, a > b
- a <= b, a >= b
- a && b, a || b, not a
- a + b, a - b
- a * b, a ^ b, a ** b
- da / b, a \`mod\` b, a \`div\` b

a \`div\` b to tzw. infix function, można również zapisać w postaci div a b

Uwaga: Oba argumenty muszą być tego samego typu (z wyjątkiem `^` oraz `**`)


## Wywoływanie funkcji
Przykłady:
- f(g(x)) = f (g x)
- f(x) = f x
- f(x, g(x)) = f x (g x)

Uwaga: ponieważ `-` jest funkcją, to funkcję z ujemną liczbą wywołujemy: f (-2)

## Definicja funkcji

```haskell
areEqual :: (Int, Int) -> Bool
areEqual (x, y) = x == y
areEqual (1, 2) -- False
```

Więcej niż jeden parametr:
```haskell
addThree :: Int -> Int -> Int -> Int
addThree x y z = x+y+z
```

Jeżeli pominiemy definicję funkcji, kompilator sam ją wywnioskuje

Uwaga: Każda funkcja w Haskelu przyjmuje tylko jeden argument.

## Wyrażenie warunkowe

```haskell
sgn :: Int -> Int
sgn n = if n < 0
        then -1
        else if n == 0 -- zagnieżdżony 'if'
            then 0
            else 1
```

## Definicja funkcji za pomocą guards

```haskell
sgn :: Int -> Int
sgn n 
    | n < 0 = -1
    | n == 0 = 0
    | otherwise = 1
```

## Definicja funkcji za pomocą dopasowywania wzorców

```haskell
isTheName :: String -> Bool
isTheName "Rumpelstilkstin" = True
isTheName _ = False
```

_ oznacza dowolną wartość

Uwaga: kolejność wzorców ma znaczenie!

Dopasowywanie wzorców z listami:

```haskell
first :: (Num a) => [a] -> a -- Zwraca pierwszy element w liście
first [] = 0
first (x:_) = x

second :: (Num a) => [a] -> a -- Zwraca drugi element
second [] = 0
second (x:[]) = 0
second (x:y:_) = y 

length' :: (Num b) => [a] -> b  -- Zwraca długość listy rekurencyjnie
length' [] = 0  
length' (_:xs) = 1 + length' xs  -- xs to dalsza część listy, za headem

sum' :: (Num a) => [a] -> a   -- Zwraca sumę listy
sum' [] = 0  
sum' (x:xs) = x + sum' xs  
```

Korzystanie z paternów

```haskell
capital :: String -> String  
capital "" = "Empty string, whoops!"  
capital all@(x:xs) = "The first letter of " ++ all ++ " is " ++ [x]  
```

`all` to cała lista przesłana jako argument funkcji, `x` to head listy, `xs` to dalsza część listy za headem.

## Definicja funkcji za pomocą case ... of

```haskell
not :: Bool -> Bool
not b = case b of
        True -> False
        False -> True -- _ -> True ?
```

## Klauzula where i let ... in
```haskell
roots :: (Double, Double, Double) -> (Double, Double)
roots (a, b, c) = ( (-b - d) / e, (-b + d) / e )
    where d = sqrt (b * b - 4 * a * c); e = 2 * a
```

Z klauzuli `where` możemy korzystać wszędzie, np. w funkcji zadeklarowanej za pomocą guards.
W `where` możemy również deklarować funkcje (w końcu wszystko w Haskellu to funkcja :).

Elementy w klauzuli `where` można też definiować po enterze.

```haskell
roots' :: (Double, Double, Double) -> (Double, Double)
roots' (a, b, c) =
    let d = sqrt (b * b - 4 * a * c) -- drugi sposób d = sqrt(...) ; e  = 2*a
        e = 2 * a
    in ( (-b - d) / e, (-b + d) / e )
```

Elementy zdefiniowane w ten sposób mają pierszeństwo nad zmiennymi globalnymi.


Uwaga:
```haskell
-- Definicje po let przesłaniają te po where
f x =   let a = 10 * x
        in a
        where a = 100 * x
```

Różnica między `let` a `where` - `let` jest wyrażeniem, `where` jest ala aliasem, konstrukcją składniową.

## Typ zwracanej zmiennej

Czasami Haskell nie wie co ma zostać zwrócone. Wtedy możemy użyć typów adnotacji (type annotations)

```haskell
read "5" :: Float -- -> 5.0 :: Float
```

## Wnioskowanie typu gdy funkcja jest polimorficzna

Polimorfizm parametryczny: fragmenty programu (funkcje i/lub struktury danych) mogą być parametryzowane typami. Typ (struktura danych, funkcja) jest polimorficzny, jeśli zawiera co najmniej jedną zmienną typu, np.
```haskell
id :: a -> a
id x = x
```
Powyższy kod oznacza, że x może być *dowolnego* typu.

```haskell
first :: (a,b) -> a
first (a,b) = a
```

Powyższa funkcja zwróci pierwszy element w krotce, więc wynik funkcji jest tego samego typu co ten element.

## Podstawowe klasy (typeclasses)

Typeclassees, to coś w rodzaju interfejsu w Javie. 

Rodzaje:

Eq:
- ==
- /=

Ord (dziedziczy z Eq):
- < , <=
- \>, \>=
- max, min

Num (należą do niej Int, Integer, Float, Double):
- \+
- \-
- \*
- negate
- abs
- signum
- fromInteger

Fractional (dziedziczy z Num):
- /
- recip
- fromRational


Jeżeli jakaś klasa dziedziczy od kogoś, to w wnioskowaniu typu korzystamy z klasy dziedziczącej.

Przykłady wnioskowania:

```haskell
swap(x,y) = (y,x)
swap :: (t1, t) -> (t, t1)

f1 (x,y,z) = (y,x,z)
f1 :: (t1, t, t2) -> (t, t1, t2)

f2 (x,y,z) = if (x > y) then (x,y,z) else (y,x,z)
f2 :: Ord t => (t, t, t1) -> (t, t, t1)

f3 x = if x > 3 then 2 * x else x / 42
f3 :: (Fractional a, Ord a) => a -> a

-- f4 x = if x > 3 then 2 * x else False -> błędna funkcja bo zwraca Bool oraz Num 

f2 x = True
f2 :: t -> Bool

f3 (x,y) = x + y
f3 :: (Num a) => (a,a) -> a

f5 (x,y) = x /= y
f5 :: (Ord x) => (x,x) -> Bool

f6 (x,y) = x > y
f6 :: (Ord x) => (x,x) -> Bool

f7 (x,y) = if x > y then x + y else x / 4
f7 :: (Ord x, Fractional x) => (x,x) -> x
```

Porady:
- przed znakiem => definiujemy klasy/interfejsy, z których metod korzystamy w funkcji: gdy korzystamy z '>' -> Ord x
- powyższa porada jest na podstawie faktu, żeby skorzystać z operatorów w Haskellu, musimy korzystać z tych samych typów zmiennych
- jeżeli funkcja zwraca coś innego niż Num to musimy to napisać (np a==b nie zwróci Eq, tylko Bool!)

## Komendy w ghci

- :? -> help
- :l -> load
- :r -> reload
- :t -> type of function for example
- :i -> info
- :q -> quit
- :set +t -> ustawia dla każdego wywołania opcję t
- :unset +t -> usuwa ustawienie


## Pisanie programów w pliku

```haskell
printHello = putStrLn "Hello"

main = printHello -- To wskazuje interpreterowi jaką funkcję ma wywołać
```

## Rekurencja

Rekurencja bez akumulatora z użyciem mechanizmów guards oraz pattern matching
```haskell
maximum' :: (Ord a) => [a] -> a  
maximum' [] = error "maximum of empty list"  
maximum' [x] = x  
maximum' (x:xs)   
    | x > maxTail = x  
    | otherwise = maxTail  
    where maxTail = maximum' xs  

-- Drugi sposób (krótszy)
maximum' :: (Ord a) => [a] -> a  
maximum' [] = error "maximum of empty list"  
maximum' [x] = x  
maximum' (x:xs) = max x (maximum' xs)   -- (x:xs) reprezentuje listę, gdzie x to head a xs to tail

-- korzystanie z guards
sumOfTwo :: (Num a, Ord a) => [a] -> a
sumOfTwo (x:y:_) 
        | x<y = x+y
        | otherwise = x-y
sumOfTwo _ = 0

-- Rekurencja ogonowa
length' :: [a] -> Int
length' = loop 0
  where loop acc [] = acc
        loop acc (x:xs) = loop (acc + 1) xs
```

## Curried functions i partial application

```haskell
max 4 5 -- zwykłe wywołanie
(max 4) 5  -- curried function
```

Deklaracja funkcji wieloargumentowej

```haskell
max :: (Ord a) => a -> a -> a
-- max :: (Ord a) => a -> (a-> a) -> równoważne zapisowi wyżej
```
Odczytywanie takiej deklaracji: funkcja max bierze argument a i zwraca funkcję, która bierze argument a i zwraca a.

Takie rozwiązanie pozwala zapisać funkcję z nie wszystkimi argumentami i potem wywoływać zapisaną funkcję z brakującymi (tzw. partial application), przykład:
```haskell
multThree :: (Num a) => a -> a -> a -> a  
multThree x y z = x * y * z  


let multikulti = multThree 5 2
-- multikulti 5  -> 5*2*5
```

Dzięki temu mechanizmowi możemy zapisywać funkcje krócej.


```haskell
compareWithHundred :: (Num a, Ord a) => a -> Ordering  
-- compareWithHundred x = compare 100 x  
compareWithHundred = compare 100 -- x przekazujemy podczas wywoływania, bo compareWithHundred zwraca funkcję, która przyjmuje dwa argumenty, więc możemy pominąć deklarację x

```

Co jeżeli chcemy przekazać argument na prawą stronę? Zawijamy naszą funkcję w nawiasy (operatory sekcji):

```haskell
divideByTen :: (Floating a) => a -> a  
divideByTen = (/10)  
-- divideByTen 200 -> 200 / 10
```

Inne przykłady:

`(2^)` (left section) -> ` \x -> 2 ^ x`
`(^2)` (right section) -> `\x -> x ^ 2`

Uwaga: znak minus `(-)` to nie jest funkcja! Jak chcemy odejmować elementy to musimy skorzystać np. z funkcji subtract. 

### Funkcje wyższego rzędu

Funkcja f przyjmuje dwa argumenty:

```haskell
f :: a->a->a
```

Tak samo możemy przyjmować funkcje i je zwracać:

```haskell
applyTwice :: (a -> a) -> a -> a  
applyTwice f x = f (f x)  

-- applyTwice (+3) 10 -> 16
```

Przykładowe funkcje:
- zipWith f list1 list2 -> wykonuje funkcję f na listach i je łączy w jedną (w szczególności: (,) tworzy krotkę dwóch list)
- flip - zwraca funkcję, która ma dwa pierwsze argumenty odwrócone, tzn jak mamy f = \x y -> x/y to filp f = \y x -> y/x

### Funkcje modyfikujące listy
- map f list -> na każdym elemencie listy jest wykonywana funkcja f
- filter f list -> usuwa elementy, które nie spełniają funkcji f
- foldl f start_value list -> redukuje elementy na podstawie funkcji f do wartości startowej start_value od lewej strony do prawej strony
- foldr -> tak samo jak powyżej, ale dodaje redukuje elementy od prawej do lewej strony
- foldl1 f list -> nie musimy podawać start_value, ale działają poprawnie tylko dla nie pustych list

Uwagi:
- `filter` nie działa na listach nieskończonych


Przykłady:
```haskell
filter (`elem` ['A'..'Z']) "i lauGh At You BecAuse u r aLL the Same" -- -> "GAYBALLS"  
```

### Collection Pipeline
Wzorzec, który wykonuje funkcje filter, map, reduce (w Haskellu: foldr) na danej liście


### Operatory ($) oraz (.)

$ w skrócie: f (g (z x)) == f $ g $ z x
Dolar zastępuje nawiasy i sprawia, że argumenty są przypisywane lewostronnie, a nie prawostronnie jak domyślnie.

Przykładowe użycie $ jako funkcji aplikującej:

```haskell
map ($ 3) [(4+), (10*), (^2), sqrt]
```

(.) w skrócie: f (g (z x)) == (f . g . z) x
Jest to inaczej złożenie funkcji. Pozwala nam to na uniknięcie wewnętrznych nawiasów.

### Typy danych

Znak | interpretujemy jako or.

```haskell
data Bool = False | True
```
Definicja własnego typu danych:

```haskell
data Shape = Circle Float Float Float | Rectangle Float Float Float Float   

surface :: Shape -> Float  
surface (Circle _ _ r) = pi * r ^ 2  
surface (Rectangle x1 y1 x2 y2) = (abs $ x2 - x1) * (abs $ y2 - y1)  

-- Jako parametr przekazujemy konstruktor
```

Aby Shape mógł być printowalny musimy dodać, extendować ten data type:

```haskell
data Shape = Circle Float Float Float | Rectangle Float Float Float Float deriving (Show)  
```

Teraz możemy printować:

```haskell
Circle 10 20 5  -- Circle 10.0 20.0 5.0  
Rectangle 50 230 60 90 -- Rectangle 50.0 230.0 60.0 90.0  
```

Stwórzmy dodatkowy typ Point.
```haskell
data Point = Point Float Float deriving (Show)  
data Shape = Circle Point Float | Rectangle Point Point deriving (Show)  
```
Musimy przez to zmienić naszą funkcję `surface`

```haskell
surface :: Shape -> Float  
surface (Circle _ r) = pi * r ^ 2  
surface (Rectangle (Point x1 y1) (Point x2 y2)) = (abs $ x2 - x1) * (abs $ y2 - y1)  

surface (Rectangle (Point 0 0) (Point 100 100)) -- 10000.0  
```

Przesuwanie figur

```haskell
nudge :: Shape -> Float -> Float -> Shape  
nudge (Circle (Point x y) r) a b = Circle (Point (x+a) (y+b)) r  
nudge (Rectangle (Point x1 y1) (Point x2 y2)) a b = Rectangle (Point (x1+a) (y1+b)) (Point (x2+a) (y2+b))  
```

Możemy exportować nasze funkcje

```haskell
module Shapes   
( Point(..)  
, Shape(..)  
, surface  
, nudge  
, baseCircle  
, baseRect  
) where  

-- Drugi sposób:
module Shape
```
 

### Składnia rekordu

Definiujemy nowy typ za pomocą składni rekordu:

```haskell
data Person = Person { firstName :: String  
                     , lastName :: String  
                     , age :: Int  
                     , height :: Float  
                     , phoneNumber :: String  
                     , flavor :: String  
                     } deriving (Show)  
```

Dzięki temu Haskell stworzył gettery o nazwach pól tego typu danych.

```haskell
ghci> :t flavor  
flavor :: Person -> String  
ghci> :t firstName  
firstName :: Person -> String  
```

Tworzenie nowych rekordów:

```haskell
data Car = Car {copmany :: String, model :: String, year :: Int} deriving (Show)  

Car {company="Ford", model="Mustang", year=1967}  
```

### Parametry typów

Typ może przyjmować argumenty, żeby deklarować typy dla parametrów w konstruktorze.

```haskell
data Car a b c = Car { company :: a  
                     , model :: b  
                     , year :: c   
                     } deriving (Show)  


-- Gdy Car nie ma argumentów:

tellCar :: Car -> String
tellCar (Car {company=c, model=m, year=y}) = "This " ++ c ++ " " ++ m ++ " was made in " ++ show y

-- let stang = Car {company="Ford", model="Mustang", year=1967}  
-- tellCar stang  
-- "This Ford Mustang was made in 1967"  


-- Gdy Car ma argumenty:

tellCar :: (Show a) => Car String String a -> String  
tellCar (Car {company = c, model = m, year = y}) = "This " ++ c ++ " " ++ m ++ " was made in " ++ show y  

-- Zatem parametryzowanie typów nie jest dobre.
```

Uwaga: nigdy nie dodawaj stałych do deklaracji typów danych.

Kolejny przykład z parametryzowaniem:

```haskell
data Vector a = Vector a a a deriving (Show)  
-- Parametr to jest "typ" dla składowych typów tego Vector-a.  

vplus :: (Num t) => Vector t -> Vector t -> Vector t  
(Vector i j k) `vplus` (Vector l m n) = Vector (i+l) (j+m) (k+n)  
  
vectMult :: (Num t) => Vector t -> t -> Vector t  
(Vector i j k) `vectMult` m = Vector (i*m) (j*m) (k*m)  
  
scalarMult :: (Num t) => Vector t -> Vector t -> t  
(Vector i j k) `scalarMult` (Vector l m n) = i*l + j*m + k*n  
```

### Instancje

Jak sprawdzać dwa obiekty tego samego typu? Implementujemy klasę Eq do naszej klasy Person.

```haskell
data Person = Person { firstName :: String  
                     , lastName :: String  
                     , age :: Int  
                     } deriving (Eq)  

```

UWAGA: Żeby sprawdzić czy dwa obiekty są równe, parametry muszą również implementować Eq.

Gdy implementujemy z Eq, Person zawiera również funkcje z Eq np. `elem`.

Pamiętajmy, że Eq, Ord, Enum, Bounded, Show, Read, czyli tzw. typeclasses to są "interfejsy".

Przykład: Ord

```haskell
data Bool = False | True deriving (Ord)  
```

Ponieważ False jest przed True, to False  < True.


Enumy:
  
```haskell
data Day = Monday | Tuesday | Wednesday | Thursday | Friday | Saturday | Sunday 
            deriving (Eq, Ord, Show, Read, Bounded, Enum)  
```

### type i newtype

Type jest aliasem dla typu. newtype jest nowym typem, ale ma tylko jedną wartość konstruktora.

```haskell
type String = [Char]  

type PhoneNumber = String  
type Name = String  
type PhoneBook = [(Name,PhoneNumber)]  

-- Parametryzacja type
type AssocList k v = [(k,v)]  

-- np. k jest Num, v jest Eq

-- Na type działa partial application
type IntMap v = Map Int v  -- lub type IntMap = Map Int
```

### Rekurencyjne typy danych



```haskell
data List a = Empty | Cons a (List a) deriving (Show, Read, Eq, Ord)  
-- to samo co: data List a = Empty | Cons { listHead :: a, listTail :: List a} deriving (Show, Read, Eq, Ord)  

-- Cons <=> :
-- Empty <=> []
```

Inny sposób powyższej deklaracji:

```haskell  
infixr 5 :-:  
data List a = Empty | a :-: (List a) deriving (Show, Read, Eq, Ord)  

-- a :-: (List a) zamiast Cons a (List a).
```

`infixr` lub `infixl` definiuje jakie operatory mają być w jakiej kolejności.

Konkatenacja list:

```haskell
infixr 5  .++  
(.++) :: List a -> List a -> List a   
Empty .++ ys = ys  
(x :-: xs) .++ ys = x :-: (xs .++ ys)  
```

Implementacja BST:

```haskell
data Tree a = EmptyTree | Node a (Tree a) (Tree a) deriving (Show, Read, Eq)  

singleton :: a -> Tree a  
singleton x = Node x EmptyTree EmptyTree  
  
treeInsert :: (Ord a) => a -> Tree a -> Tree a  
treeInsert x EmptyTree = singleton x  
treeInsert x (Node a left right)   
    | x == a = Node x left right  
    | x < a  = Node a (treeInsert x left) right  
    | x > a  = Node a left (treeInsert x right)  

treeElem :: (Ord a) => a -> Tree a -> Bool  
treeElem x EmptyTree = False  
treeElem x (Node a left right)  
    | x == a = True  
    | x < a  = treeElem x left  
    | x > a  = treeElem x right  
```

### IO 
Przykładowe akcje IO:

- `getChar` - pobiera znak z wejścia i zwraca go jako `char`
- `putChar` - wypisuje znak na wyjście
- `getLine` - pobiera linię tekstu z wejścia i zwraca ją jako `String`
- `putStr` - wypisuje linię tekstu na wyjście
- `print` - wypisuje linię tekstu na wyjście (dowolny typ)
- `return` - zwraca wartość z funkcji

`return` jest swego rodzaju przeciwieństwem `<-`. Podczas gdy `return` pobiera wartość i wrapuje ją w "pudło", `<-` bierze "pudło" (i wykonuje je) i pobiera z niego wartość, wiążąc ją z nazwą.

Przykład:
```haskell
main = do  
    putStrLn "Hello, what's your name?"  
    name <- getLine  -- Pobieranie danych od użytkownika
    putStrLn $ "Read this carefully, because this is your future: " ++ tellFortune name

main = do  
    a <- return "hell"  
    b <- return "yeah!"  
    putStrLn $ a ++ " " ++ b    
```

### Łączenie akcji IO

- `(>>) :: Monad m => m a -> m b -> m b` - wykonuje pierwszą akcję, a następnie drugą
- `(>>=) :: Monad m => m a -> (a -> m b) -> m b` - wykonuje pierwszą akcję, a następnie drugą, która zwraca akcję

Przykład:
```haskell
twoQuestions = putStr "What is your name? " >> getLine >>= \name -> putStr "How old are you? " >> getLine >>= \age -> return (name, age)

-- inaczej:
twoQuestions = do
  putStr "What is your name? "
  name <- getLine
  putStr "How old are you? "
  age <- getLine
  print (name,age)
```

### Akcje IO

- `sequence` - wykonuje listę akcji IO
- `sequence_` - wykonuje listę akcji IO, ignorując wyniki

Przykład
```haskell
sequence'        :: [IO ()] -> IO ()
sequence' []     =  return ()
sequence' (a:as) =  do a
                       sequence' as

sequence'' :: [IO ()] -> IO ()
sequence'' = foldr (\x y -> x >> y) (return ())

sequence''' :: [IO ()] -> IO ()
sequence''' x = foldl (>>) (return ()) (reverse x)
```

### Funktory

Funktory to kontenery, które umożliwiają mapowanie funkcji na wartości wewnątrz kontenera.

- `fmap :: Functor f => (a -> b) -> f a -> f b` - *mapuje* funkcję na wartość wewnątrz kontenera
- `(<$>)` - alias dla `fmap`
- `(<$) :: Functor f => a -> f b -> f a` - *zamienia wartość* wewnątrz kontenera na inną


### Aplikatory

- `pure :: Applicative f => a -> f a` - tworzy kontener z wartością (np. wrap wartości w Just)
- `(<*>) :: Applicative f => f (a -> b) -> f a -> f b` - *aplikuje* funkcję wewnątrz kontenera do wartości wewnątrz innego kontenera (uwaga: typy proste muszą być opakowane w Just np. za pomocą `pure`)
- `ZipList` - tworzy kontener z listą wartości
- `(*>) :: Applicative f => f a -> f b -> f b` - wykonuje drugą akcję, ignorując wynik pierwszej, prz. `Nothing *> Just 2` zwróci `Just 2` 
- `(<*) :: Applicative f => f a -> f b -> f a` - wykonuje drugą akcję, ignorując wynik pierwszej, prz. `Nothing <* Just 2` zwróci `Nothing`

Przykłady:
```haskell
(+) <$> Just 1 <*> Just 2 -- Just 3

pure (+1) <*> Left 0 -- Left 0
pure (+1) <*> Right 0 -- Right 1 (analogicznie jak z fmap, wartość dodawana jest na prawy koniec elementu np. fmap (+1) (0,0,0) -> (0,0,1))

[(+1), (*2)] <*> [1,2,3] -- [2,3,4,2,4,6]
(*) <$> [1,2,3] <*> [100,101,102] -- [100,101,102,200,202,204,300,303,306]
pure (+) <*> ZipList [1,2,3] <*> ZipList [100,100,100] -- ZipList {getZipList = [101,102,103]}

-- Drzewo binarne
data Tree a = Node a (Tree a) (Tree a) | Leaf

paths :: Tree a -> [[a]]
paths Leaf = pure []
paths (Node a l r) = concat $ ([(a:)] <*>) <$> (fmap paths [l, r])

pathsSum :: Num a => Tree a -> [a]
pathsSum Leaf = pure 0
pathsSum (Node a l r) = concat $ ([(a+)] <*>) <$> (fmap pathsSum [l, r])
```