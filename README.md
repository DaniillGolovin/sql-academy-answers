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
SELECT town_to, TIMEDIFF(time_in, time_out) AS flight_time
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
SELECT trip, COUNT(passenger) AS count
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
SELECT passenger.name, COUNT(Pass_in_trip.passenger) AS count
FROM Pass_in_trip
JOIN Passenger ON Pass_in_trip.passenger = Passenger.id
GROUP BY passenger.name
HAVING count >= 1
ORDER BY count DESC, passenger.name ASC;
```

**Task №17:** _Определить, сколько потратил в 2005 году каждый из членов семьи. В результирующей выборке не выводите тех членов семьи, которые ничего не потратили._
```sql
SELECT member_name, status, SUM(Payments.amount * Payments.unit_price) AS costs
FROM FamilyMembers
JOIN Payments ON FamilyMembers.member_id = Payments.family_member
WHERE YEAR(Payments.date) = 2005
GROUP BY member_name, status;
```

**Task №18:** _Выведите имя самого старшего человека. Если таких несколько, то выведите их всех._
```sql
SELECT member_name
FROM FamilyMembers
WHERE birthday = (
    SELECT MIN(birthday)
    FROM FamilyMembers);
```

**Task №19:** _Определить, кто из членов семьи покупал картошку (potato)._
```sql
SELECT DISTINCT status
FROM FamilyMembers
JOIN Payments ON FamilyMembers.member_id = Payments.family_member
JOIN Goods ON Goods.good_id = Payments.good
WHERE Goods.good_name = 'potato';
```

**Task №20:** _Сколько и кто из семьи потратил на развлечения (entertainment). Вывести статус в семье, имя, сумму._
```sql
SELECT status, member_name, SUM(Payments.amount * Payments.unit_price) AS costs
FROM FamilyMembers
JOIN Payments ON Payments.family_member = FamilyMembers.member_id
JOIN Goods ON Goods.good_id = Payments.good
JOIN GoodTypes ON GoodTypes.good_type_id = Goods.type
WHERE GoodTypes.good_type_name = 'entertainment'
GROUP BY status, member_name;
```

**Task №21:** _Определить товары, которые покупали более 1 раза._
```sql
SELECT good_name
FROM Goods
JOIN Payments ON Goods.good_id = Payments.good
GROUP BY good_name
HAVING COUNT(Payments.good) > 1;
```

**Task №22:** _Найти имена всех матерей (mother)._
```sql
SELECT member_name
FROM FamilyMembers
WHERE status = 'mother';
```

**Task №23:** _Найдите самый дорогой деликатес (delicacies) и выведите его цену._
```sql
SELECT good_name, unit_price 
FROM Payments
JOIN Goods ON Payments.good = Goods.good_id
JOIN GoodTypes ON Goods.type = GoodTypes.good_type_id
WHERE good_type_name = 'delicacies'
LIMIT 1;
```

**Task №24:** _Определить кто и сколько потратил в июне 2005._
```sql
SELECT member_name, SUM(amount * unit_price) AS costs
FROM FamilyMembers
JOIN Payments ON Payments.family_member = FamilyMembers.member_id
WHERE DATE_FORMAT(Payments.date, '%m.%Y') = '06.2005'
GROUP BY FamilyMembers.member_name;
```

**Task №25:** _Определить, какие товары не покупались в 2005 году._
```sql
SELECT good_name
FROM Goods
WHERE good_id NOT IN (
    SELECT good
    FROM Payments
    WHERE YEAR(date) = '2005');
```

**Task №26:** _Определить группы товаров, которые не приобретались в 2005 году._
```sql
SELECT good_type_name
FROM GoodTypes
WHERE GoodTypes.good_type_id NOT IN (
    SELECT Goods.type
    FROM Goods
    JOIN Payments ON Goods.good_id = Payments.good
    WHERE YEAR(Payments.date) = '2005');
```

**Task №27:** _Узнайте, сколько было потрачено на каждую из групп товаров в 2005 году. Выведите название группы и потраченную на неё сумму. Если потраченная сумма равна нулю, т.е. товары из этой группы не покупались в 2005 году, то не выводите её._
```sql
SELECT good_type_name, SUM(amount*unit_price) AS costs FROM GoodTypes
JOIN Goods ON good_type_id = type
JOIN Payments ON good = good_id AND YEAR(date) = 2005
GROUP BY good_type_name;
```

**Task №28:** _Сколько рейсов совершили авиакомпании из Ростова (Rostov) в Москву (Moscow) ?_
```sql
SELECT COUNT(id) AS count 
FROM Trip
WHERE town_from = 'Rostov' AND town_to = 'Moscow';

-- или так
SELECT COUNT(town_from) as count
FROM Trip
WHERE town_to = 'Moscow'
```

**Task №29:** _Выведите имена пассажиров улетевших в Москву (Moscow) на самолете TU-134._
```sql
SELECT DISTINCT name
FROM Passenger
JOIN Pass_in_trip ON Pass_in_trip.passenger = Passenger.id
JOIN Trip ON Trip.id = Pass_in_trip.trip
WHERE Trip.town_to = 'Moscow' AND Trip.plane = 'TU-134';
```

**Task №30:** _Выведите нагруженность (число пассажиров) каждого рейса (trip). Результат вывести в отсортированном виде по убыванию нагруженности._
```sql
SELECT trip, COUNT(passenger) AS count
FROM Pass_in_trip
GROUP BY trip
ORDER BY count DESC;
```

**Task №31:** _Вывести всех членов семьи с фамилией Quincey._
```sql
SELECT *
FROM FamilyMembers
WHERE member_name LIKE '%_Quincey';
```

**Task №32:** _Вывести средний возраст людей (в годах), хранящихся в базе данных. Результат округлите до целого в меньшую сторону._
```sql
SELECT FLOOR(AVG(TIMESTAMPDIFF(YEAR, birthday, NOW()))) AS age
FROM FamilyMembers;
```

**Task №33:** _Найдите среднюю цену икры на основе данных, хранящихся в таблице Payments. В базе данных хранятся данные о покупках красной (red caviar) и черной икры (black caviar). В ответе должна быть одна строка со средней ценой всей купленной когда-либо икры._
```sql
SELECT AVG(unit_price) AS cost
FROM Payments
JOIN Goods ON Goods.good_id = Payments.good
WHERE Goods.good_name IN ('black caviar', 'red caviar');

-- или так 
SELECT AVG(unit_price) AS cost
FROM Payments
WHERE good IN (
    SELECT good_id 
    FROM Goods
    WHERE good_name LIKE '%caviar');
```

**Task №34:** _Сколько всего 10-ых классов._
```sql
SELECT COUNT(name) AS count
FROM Class
WHERE name LIKE '10%';
```

**Task №35:** _Сколько различных кабинетов школы использовались 2 сентября 2019 года для проведения занятий?_
```sql
SELECT COUNT(classroom) AS count
FROM Schedule
WHERE DATE(date) = '2019-09-02';
```

**Task №36:** _Выведите информацию об обучающихся живущих на улице Пушкина (ul. Pushkina)?_
```sql
SELECT *
FROM Student
WHERE address LIKE 'ul. Pushkina%';
```

**Task №37:** _Сколько лет самому молодому обучающемуся ?_
```sql
SELECT TIMESTAMPDIFF(YEAR, MAX(birthday), NOW()) AS year
FROM Student;
```

**Task №38:** _Сколько Анн (Anna) учится в школе ?_
```sql
SELECT COUNT(first_name) AS count
FROM Student
WHERE first_name = 'Anna';
```
**Task №39:** _Сколько обучающихся в 10 B классе ?_
```sql
SELECT COUNT(student) as count
FROM Student_in_class
WHERE class IN (
    SELECT id
    FROM class
    WHERE name = '10 B');

-- или так 
SELECT COUNT(student) AS count
FROM Student_in_class
JOIN Class ON Student_in_class.class = Class.id
WHERE Class.name = '10 B';
```

**Task №40:** _Выведите название предметов, которые преподает Ромашкин П.П. (Romashkin P.P.). Обратите внимание, что в базе данных есть несколько учителей с такими фамилией и инициалами._
```sql
SELECT name as subjects
FROM Teacher
JOIN Schedule ON Schedule.teacher = Teacher.id
JOIN Subject ON Subject.id = Schedule.subject
WHERE first_name LIKE 'P%'
  AND middle_name LIKE 'P%'
  AND last_name LIKE 'Romashkin';
```

**Task №41:** _Выясните, во сколько по расписанию начинается четвёртое занятие._
```sql
SELECT start_pair
FROM Timepair
WHERE id = 4;

-- или так
SELECT start_pair
FROM Timepair
WHERE id IN (
    SELECT number_pair
    FROM Schedule
    WHERE number_pair = '4');
```

**Task №42:** _Сколько времени обучающийся будет находиться в школе, учась со 2-го по 4-ый уч. предмет?_
```sql
SELECT DISTINCT TIMEDIFF(
    (
    SELECT end_pair
    FROM Timepair
    WHERE id = '4'),
    (
    SELECT start_pair
    FROM Timepair
    WHERE id = '2')) AS time
FROM Timepair;
```

**Task №43:** _Выведите фамилии преподавателей, которые ведут физическую культуру (Physical Culture). Отсортируйте преподавателей по фамилии в алфавитном порядке._
```sql
SELECT last_name
FROM Teacher
JOIN Schedule ON Teacher.id = Schedule.teacher
JOIN Subject ON Schedule.subject = Subject.id
WHERE Subject.name = 'Physical Culture'
ORDER BY last_name;

-- или так 
SELECT last_name
FROM Teacher
WHERE id IN (
    SELECT teacher
    FROM Schedule
    WHERE subject = (
        SELECT id
        FROM Subject
        WHERE name = 'Physical Culture'))
ORDER BY last_name;
```

**Task №44:** _Найдите максимальный возраст (количество лет) среди обучающихся 10 классов на сегодняшний день. Для получения текущих даты и времени используйте функцию NOW()._
```sql
SELECT MAX(TIMESTAMPDIFF(YEAR,birthday,CURRENT_DATE)) AS max_year
FROM Student
JOIN Student_in_class ON Student.id = Student_in_class.student
JOIN Class ON Student_in_class.class = Class.id
WHERE Class.name LIKE '10%';

-- или так
SELECT MAX(TIMESTAMPDIFF(YEAR, birthday, NOW())) AS max_year
FROM Student
WHERE id IN (
    SELECT student
    FROM Student_in_class
    WHERE class IN (
        SELECT id
        FROM Class
        WHERE name LIKE '10%'));
```

**Task №45:** _Какие кабинеты чаще всего использовались для проведения занятий? Выведите те, которые использовались максимальное количество раз._
```sql
SELECT classroom
FROM Schedule
GROUP BY classroom
HAVING COUNT(classroom) = (
    SELECT COUNT(classroom)
    FROM Schedule
    GROUP BY classroom
    ORDER BY COUNT(classroom) DESC
    LIMIT 1);
```

**Task №46:** _В каких классах введет занятия преподаватель "Krauze" ?_
```sql
SELECT DISTINCT name
FROM Class
JOIN Schedule ON Class.id = Schedule.class
JOIN Teacher ON Schedule.teacher = Teacher.id
WHERE Teacher.last_name='Krauze';

-- или так 
SELECT name
FROM Class
WHERE id IN (
    SELECT class
    FROM Schedule
    WHERE teacher IN (
        SELECT id
        FROM Teacher
        WHERE last_name = 'Krauze'));
```

**Task №47:** _Сколько занятий провел Krauze 30 августа 2019 г.?_
```sql
SELECT COUNT(teacher) AS count
FROM Schedule
WHERE teacher = (
    SELECT id
    FROM Teacher
    WHERE last_name = 'Krauze') AND DATE(date) = '2019-08-30';
```

**Task №48:** _Выведите заполненность классов в порядке убывания._
```sql
SELECT name, COUNT(Student_in_class.student) AS count
FROM Class
JOIN Student_in_class ON Student_in_class.class = Class.id
GROUP BY Class.name
ORDER BY count DESC;
```

**Task №49:** _Какой процент обучающихся учится в "10 A" классе? Выведите ответ в диапазоне от 0 до 100 с округлением до четырёх знаков после запятой, например, 96.0201._
```sql
SELECT COUNT(student) * 100 / (SELECT COUNT(student) FROM Student_in_class) AS percent
FROM Student_in_class
JOIN Class ON Student_in_class.class = Class.id
WHERE name='10 A';

-- или так 
SELECT ROUND(
(
   SELECT COUNT(*)
   FROM Student_in_class
   WHERE class IN (
       SELECT id
       FROM Class
       WHERE name = '10 A'
   )) * 100.0 / COUNT(*), 4) AS percent
FROM Student_in_class;
```

**Task №50:** _Какой процент обучающихся родился в 2000 году? Результат округлить до целого в меньшую сторону._
```sql
SELECT FLOOR(
    (
       SELECT COUNT(*)
       FROM Student
       WHERE YEAR(birthday) = 2000
    ) * 100 / COUNT(*)) as percent
FROM Student
```

**Task №51:** _Добавьте товар с именем "Cheese" и типом "food" в список товаров (Goods)._
```sql
INSERT INTO Goods (good_id, good_name, type)
SELECT
    (SELECT COUNT(*) + 1 FROM Goods),
    'Cheese',
    (SELECT good_type_id FROM GoodTypes WHERE good_type_name = 'food');
```

**Task №52:** _Добавьте в список типов товаров (GoodTypes) новый тип "auto"._
```sql
INSERT INTO GoodTypes (good_type_id, good_type_name)
SELECT
    (SELECT COUNT(*) + 1 FROM GoodTypes),
    'auto';
```

**Task №53:** _Измените имя "Andie Quincey" на новое "Andie Anthony"._
```sql
UPDATE FamilyMembers
SET member_name = 'Andie Anthony'
WHERE member_name = 'Andie Quincey';
```

**Task №54:** _Удалить всех членов семьи с фамилией "Quincey"._
```sql
DELETE FROM FamilyMembers
WHERE member_name LIKE '%Quincey';
```

**Task №55:** _Удалить компании, совершившие наименьшее количество рейсов._
```sql
DELETE FROM Company
WHERE Company.id IN (
    SELECT company
    FROM Trip
    GROUP BY company
    HAVING COUNT(id) = (
        SELECT MIN(count)
        FROM (
             SELECT COUNT(id) AS count
             FROM Trip
             GROUP BY company) AS subquery));
```
