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
