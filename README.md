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

**Task №7:** _Вывести все названия самолётов, на которых можно улететь в Москву (Moscow)._
```sql
SELECT DISTINCT name
FROM Company, Trip
WHERE plane = 'Boeing' AND Company.id = Trip.company;
```

**Task №8:** _В какие города можно улететь из Парижа (Paris) и сколько времени это займёт?_
```sql
SELECT town_to, TIMEDIFF(time_in, time_out) as flight_time
FROM trip
WHERE town_from = 'Paris';
```

**Task №9:** _Какие компании организуют перелеты из Владивостока (Vladivostok)?_
```sql
SELECT DISTINCT name
FROM company, trip
WHERE trip.company = company.id AND town_from = 'Vladivostok';
```

**Task №10:** _Вывести вылеты, совершенные с 10 ч. по 14 ч. 1 января 1900 г._
```sql
SELECT *
FROM trip
WHERE time_out BETWEEN '1900-01-01T10:0:00.000Z' AND '1900-01-01T14:0:00.000Z';
```

**Task №11:** _Выведите пассажиров с самым длинным ФИО. Пробелы, дефисы и точки считаются частью имени._
```sql
SELECT name FROM Passenger
ORDER BY LENGTH(name) DESC LIMIT 1;
-- или так
SELECT name
FROM passenger
WHERE LENGTH(name) = (
    SELECT MAX(LENGTH(name))
    FROM passenger);
```

**Task №12:** _Вывести id и количество пассажиров для всех прошедших полётов._
```sql
SELECT trip, COUNT(passenger) as count
FROM Pass_in_trip
GROUP BY trip;
```

**Task №13:** _Вывести имена людей, у которых есть полный тёзка среди пассажиров._
```sql
SELECT name
FROM passenger
GROUP BY name
HAVING COUNT(*) > 1;
```

**Task №14:** _В какие города летал Bruce Willis._
```sql
SELECT town_to
FROM Trip
JOIN Pass_in_trip ON Pass_in_trip.trip = Trip.id
JOIN Passenger ON Pass_in_trip.passenger = Passenger.id
WHERE Passenger.name = 'Bruce Willis';
```

**Task №15:** _Выведите дату и время прилёта пассажира Стив Мартин (Steve Martin) в Лондон (London)._
```sql
SELECT time_in
FROM Trip
JOIN Pass_in_trip ON Pass_in_trip.trip = Trip.id
JOIN Passenger ON Pass_in_trip.passenger = Passenger.id
WHERE Passenger.name = 'Steve Martin' AND Trip.town_to = 'London';
```

**Task №16:** _Вывести отсортированный по количеству перелетов (по убыванию) и имени (по возрастанию) список пассажиров, совершивших хотя бы 1 полет._
```sql
SELECT passenger.name, COUNT(Pass_in_trip.passenger) as count
FROM Pass_in_trip
JOIN Passenger ON Pass_in_trip.passenger = Passenger.id
GROUP BY passenger.name
HAVING count >= 1
ORDER BY count DESC, passenger.name ASC 
```















