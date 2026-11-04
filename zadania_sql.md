ŁATWE

1\.



SELECT COUNT(\*) AS pracownicy FROM employees;



2\.



SELECT first\_name, last\_name, salary FROM employees WHERE department\_id = 1 ORDER BY salary DESC;



3\.



SELECT first\_name, last\_name, salary from employees WHERE salary > 12000;



4\.



SELECT first\_name, last\_name, phone FROM employees WHERE phone IS NULL;



5\.



SELECT MAX(salary) AS najwyzsza, MIN(salary) as najnizsza FROM employees;



6\.



SELECT DISTINCT city FROM departments;



7\.



SELECT first\_name, last\_name, hire\_date

FROM employees

WHERE hire\_date >= '2023-01-01'

&#x20; AND hire\_date < '2024-01-01';



8\.



SELECT title, price FROM books WHERE price > 150 ORDER BY price DESC;



9\.



SELECT title, in\_stock FROM books WHERE in\_stock < 15 ORDER BY in\_stock ASC;



10\.

SELECT title, publish\_year FROM books WHERE publish\_year < 2000;



ŚREDNIE

11\.

SELECT department\_id, COUNT(\*) AS liczba

FROM employees

GROUP BY department\_id

ORDER BY department\_id;



12\.

SELECT ROUND(AVG(salary), 2) AS srednia FROM employees;



13\.

SELECT

&#x20;   e.first\_name || ' ' || e.last\_name AS pracownicy,

&#x20;   d.name AS dzial

FROM employees e

JOIN departments d ON e.department\_id = d.id

WHERE d.name = 'Sales';



14\.

SELECT

&#x20;   d.name AS dzial,

&#x20;   COUNT(\*) AS liczba

FROM employees e

JOIN departments d ON e.department\_id = d.id

GROUP BY d.name, e.department\_id

HAVING COUNT(\*) > 2;



15\.

SELECT b.title, p.name AS wydawca

FROM books b

INNER JOIN publishers p ON b.publisher\_id = p.id

WHERE p.country = 'USA'

ORDER BY b.title;



16\.

SELECT p.name AS wydawca, COUNT(b.id) as ksiazki FROM publisher p LEFT JOIN books b ON b.publisher\_id = p.id

GROUP BY p.id, p.name

ORDER BY ksiazki DESC, p.name;



17\.

SELECT p.name AS wydawca, SUM(price \* in\_stock) AS wartosc

FROM books b

LEFT JOIN publishers p ON b.publisher\_id = p.id

GROUP BY p.name

ORDER BY wartosc DESC;



18\.

SELECT a.first\_name, a.last\_name

FROM authors a

WHERE NOT EXISTS (SELECT 1 FROM book\_authors ba WHERE ba.author\_id = a.id);



19\.



SELECT d.name, COUNT(\*) AS pracownicy, ROUND(AVG(salary), 2) AS srednia

FROM employees e

INNER JOIN departments d ON e.department\_id = d.id

WHERE is\_active = TRUE

GROUP BY d.id, d.name;



TRUDNE

20\.

SELECT d.name AS dzial, ROUND(AVG(salary), 2) AS srednia

FROM employees e

INNER JOIN departments d ON e.department\_id = d.id

GROUP BY d.name

HAVING AVG(salary) > 10000

ORDER BY srednia DESC;



21\.
SELECT e.first\_name || ' ' || e.last\_name AS pracownik,

&#x20;      m.first\_name || ' ' || m.last\_name AS manager

FROM employees e

JOIN employees m ON m.id = e.manager\_id

ORDER BY manager, pracownik;



22\.

SELECT title

FROM books

WHERE price > (SELECT AVG(price) FROM books);



23\.
SELECT a.first\_name || ' ' || a.last\_name AS autor,

&#x20;      COUNT(ba.book\_id) AS liczba\_ksiazek

FROM authors a

INNER JOIN book\_authors ba ON ba.author\_id = a.id

GROUP BY a.id

ORDER BY liczba\_ksiazek DESC;



24\.

SELECT wydawca, tytul, cena

FROM (

&#x20;   SELECT p.name AS wydawca, b.title as tytul, b.price as cena,

MAX(price) OVER(PARTITION BY b.publisher\_id) as max\_cena

FROM books b

INNER JOIN publishers p ON b.publisher\_id = p.id

) AS wyniki

WHERE cena = max\_cena

ORDER BY cena DESC;



25\.
SELECT dzial, imie, nazwisko, zarobki

FROM (

&#x20;   SELECT e.first\_name AS imie, e.last\_name AS nazwisko, e.salary AS zarobki, d.name AS dzial,

&#x20;          ROW\_NUMBER() OVER (

&#x20;              PARTITION BY e.department\_id

&#x20;              ORDER BY e.salary DESC

&#x20;          ) AS pozycja

&#x20;   FROM employees e

INNER JOIN departments d ON e.department\_id = d.id

) AS wyniki

WHERE pozycja = 2
ORDER BY zarobki DESC;



26\.
WITH raport AS (

&#x20;   SELECT p.country, COUNT(b.title) AS liczba\_tytulow, ROUND(AVG(b.price), 2) AS srednia\_cena,

&#x20;      SUM(b.price \* b.in\_stock) AS wartosc\_magazynu

FROM books b

INNER JOIN publishers p ON b.publisher\_id = p.id

GROUP BY p.country;

)

SELECT \*

FROM raport

WHERE wartosc\_magazynu > 1000

ORDER BY wartosc\_magazynu DESC;

