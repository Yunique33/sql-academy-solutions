# Решение заданий из тренажера [SQL Academy](https://sql-academy.org/)

1. Вывести имена всех людей, которые есть в базе данных
   авиакомпаний [(сайт)](https://sql-academy.org/ru/trainer/tasks/1)

<details>
  <summary>Решение</summary>

```mysql
SELECT name
FROM passenger;
```

</details>

2. Вывести названия всеx авиакомпаний [(сайт)](https://sql-academy.org/ru/trainer/tasks/2)

<details>
  <summary>Решение</summary>

```mysql
SELECT name
FROM Company;
```

</details>

3. Вывести все рейсы, совершенные из Москвы [(сайт)](https://sql-academy.org/ru/trainer/tasks/3)

<details>
  <summary>Решение</summary>

```mysql
SELECT *
FROM trip
WHERE town_from = 'Moscow';
```

</details>

4. Вывести имена людей, которые заканчиваются на "man" [(сайт)](https://sql-academy.org/ru/trainer/tasks/4)

<details>
  <summary>Решение</summary>

```mysql
SELECT name
FROM passenger
WHERE name LIKE '%man';
```

</details>

5. Вывести количество рейсов, совершенных на TU-134 [(сайт)](https://sql-academy.org/ru/trainer/tasks/5)

<details>
  <summary>Решение</summary>

```mysql
SELECT COUNT(*) AS count
FROM trip
WHERE plane = 'TU-134';
```

</details>

6. Какие компании совершали перелеты на Boeing [(сайт)](https://sql-academy.org/ru/trainer/tasks/6)

<details>
  <summary>Решение</summary>

```mysql
SELECT DISTINCT cp.name
FROM company cp
         JOIN trip tr ON cp.id = tr.company
WHERE plane = 'Boeing';
```

</details>

7. Вывести все названия самолётов, на которых можно улететь в Москву (Moscow)
   [(сайт)](https://sql-academy.org/ru/trainer/tasks/7)

<details>
  <summary>Решение</summary>

```mysql
SELECT DISTINCT plane
FROM trip
WHERE town_to = 'Moscow';
```

</details>

8. В какие города можно улететь из Парижа (Paris) и сколько времени это займёт?
   [(сайт)](https://sql-academy.org/ru/trainer/tasks/8)

<details>
  <summary>Решение</summary>

```mysql
SELECT town_to,
       TIMEDIFF(time_in, time_out) AS flight_time
FROM trip
WHERE town_from = 'Paris';
```

</details>

9. Какие компании организуют перелеты из Владивостока (Vladivostok)?
   [(сайт)](https://sql-academy.org/ru/trainer/tasks/9)

<details>
  <summary>Решение</summary>

```mysql
SELECT name
FROM trip tr
         JOIN company cp ON tr.company = cp.id
WHERE town_from = 'Vladivostok';
```

</details>

10. Вывести вылеты, совершенные с 10 ч. по 14 ч. 1 января 1900 г. [(сайт)](https://sql-academy.org/ru/trainer/tasks/10)

<details>
  <summary>Решение</summary>

```mysql
SELECT *
FROM trip
WHERE DATE(time_out) = '1900-01-01'
  AND TIME_FORMAT(time_out, '%H:%i') >= '10:00'
  AND TIME_FORMAT(time_out, '%H:%i') <= '14:00';
```

</details>

11. Выведите пассажиров с самым длинным ФИО. Пробелы, дефисы и точки считаются частью имени.
    [(сайт)](https://sql-academy.org/ru/trainer/tasks/11)

<details>
  <summary>Решение</summary>

```mysql
SELECT name
FROM passenger
ORDER BY LENGTH(name) DESC
LIMIT 1;
```

</details>

12. Вывести id и количество пассажиров для всех прошедших полётов [(сайт)](https://sql-academy.org/ru/trainer/tasks/12)

<details>
  <summary>Решение</summary>

```mysql
SELECT trip,
       COUNT(*) AS count
FROM passenger ps
         JOIN Pass_in_trip pt ON ps.id = pt.passenger
GROUP BY trip;
```

</details>

13. Вывести имена людей, у которых есть полный тёзка среди пассажиров
    [(сайт)](https://sql-academy.org/ru/trainer/tasks/13)

<details>
  <summary>Решение</summary>

```mysql
SELECT name
FROM passenger
GROUP BY name
HAVING COUNT(*) > 1;
```

</details>

14. В какие города летал Bruce Willis [(сайт)](https://sql-academy.org/ru/trainer/tasks/14)

<details>
  <summary>Решение</summary>

```mysql
SELECT town_to
FROM passenger ps
         JOIN Pass_in_trip pt ON ps.id = pt.passenger
         JOIN trip tr ON tr.id = pt.trip
WHERE name = 'Bruce Willis';
```

</details>

15. Выведите дату и время прилёта пассажира Стив Мартин (Steve Martin) в Лондон (London) 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/15)

<details>
  <summary>Решение</summary>

```mysql
SELECT time_in
FROM trip tr
         JOIN Pass_in_trip pt ON tr.id = pt.trip
         JOIN passenger ps ON pt.passenger = ps.id
WHERE name = 'Steve Martin'
  AND town_to = 'London';
```

</details>

16. Вывести отсортированный по количеству перелетов (по убыванию) и имени (по возрастанию) список пассажиров, 
совершивших хотя бы 1 полет. [(сайт)](https://sql-academy.org/ru/trainer/tasks/16)

<details>
  <summary>Решение</summary>

```mysql
SELECT name,
       COUNT(name) AS count
FROM passenger ps
         JOIN Pass_in_trip pt ON ps.id = pt.passenger
         JOIN trip tr ON pt.trip = tr.id
GROUP BY name
ORDER BY count DESC,
         name ASC;
```

</details>

17. Определить, сколько потратил в 2005 году каждый из членов семьи. В результирующей выборке не выводите тех членов 
семьи, которые ничего не потратили. [(сайт)](https://sql-academy.org/ru/trainer/tasks/17)   

<details>
  <summary>Решение</summary>

```mysql
SELECT member_name,
       status,
       SUM(unit_price * amount) AS costs
FROM FamilyMembers fm
         JOIN Payments ps ON fm.member_id = ps.family_member
WHERE YEAR(DATE) = 2005
GROUP BY member_name,
         status;
```

</details>

18. Узнать, кто старше всех в семьe [(сайт)](https://sql-academy.org/ru/trainer/tasks/18) 

<details>
  <summary>Решение</summary>

```mysql
SELECT member_name
FROM FamilyMembers
ORDER BY birthday ASC
LIMIT 1;
```

</details>

19. Определить, кто из членов семьи покупал картошку (potato) [(сайт)](https://sql-academy.org/ru/trainer/tasks/19)

<details>
  <summary>Решение</summary>

```mysql
SELECT status
FROM FamilyMembers fm
         JOIN Payments ps ON fm.member_id = ps.family_member
         JOIN Goods gs ON ps.good = gs.good_id
WHERE good_name = 'potato'
GROUP BY status;
```

</details>

20. Сколько и кто из семьи потратил на развлечения (entertainment). Вывести статус в семье, имя, сумму 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/20)

<details>
  <summary>Решение</summary>

```mysql
SELECT status,
       member_name,
       (amount * unit_price) AS costs
FROM FamilyMembers fm
         JOIN Payments ps ON fm.member_id = ps.family_member
         JOIN Goods gs ON ps.good = gs.good_id
         JOIN GoodTypes gt ON gs.type = gt.good_type_id
WHERE good_type_name = 'entertainment';
```

</details>

21. Определить товары, которые покупали более 1 раза [(сайт)](https://sql-academy.org/ru/trainer/tasks/21)

<details>
  <summary>Решение</summary>

```mysql
SELECT good_name
FROM Goods gs
         JOIN Payments ps ON gs.good_id = ps.good
GROUP BY good
HAVING COUNT(*) > 1;
```

</details>

22. Найти имена всех матерей (mother) [(сайт)](https://sql-academy.org/ru/trainer/tasks/22)

<details>
  <summary>Решение</summary>

```mysql
SELECT member_name
FROM FamilyMembers
WHERE status = 'mother';
```

</details>

23. Найдите самый дорогой деликатес (delicacies) и выведите его цену  
[(сайт)](https://sql-academy.org/ru/trainer/tasks/23)


<details>
  <summary>Решение</summary>

```mysql
SELECT good_name,
       unit_price
FROM Goods gs
         JOIN GoodTypes gt ON gs.type = gt.good_type_id
         JOIN Payments ps ON gs.good_id = ps.good
WHERE good_type_name = 'delicacies'
ORDER BY unit_price DESC
LIMIT 1;
```

</details>
