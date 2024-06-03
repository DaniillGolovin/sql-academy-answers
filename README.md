Решение задач по SQL. 
Список задач https://sql-academy.org/ru/trainer

**Task №1:** _Вывести имена всех когда-либо обслуживаемых пассажиров авиакомпаний._
```sql
SELECT name
FROM passenger;
```

**Task №2:** _Вывести названия всеx авиакомпаний._
```sql
SELECT name
FROM Company;
```
**Task №3:** _Вывести все рейсы, совершенные из Москвы._
```sql
SELECT *
FROM Trip
WHERE town_from = 'Moscow';
```
**Task №4:** _Вывести имена людей, которые заканчиваются на "man"._
```sql
SELECT name
FROM passenger
WHERE name LIKE '%man';
```
**Task №5:** _Вывести количество рейсов, совершенных на TU-134._
```sql
SELECT COUNT(*) as count
FROM trip
WHERE plane = 'TU-134';
```

**Task №6:** _Какие компании совершали перелеты на Boeing._
```sql
SELECT DISTINCT name
FROM Company, Trip
WHERE plane = 'Boeing' AND Company.id = Trip.company;
```




