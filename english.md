# Task solutions from the  [SQL Academy](https://sql-academy.org/)

[English version](english.md)

[Certificate](yunus_ganiyev.pdf)

1. Print the names of all the people who are in the airline database 
[(link)](https://sql-academy.org/en/trainer/tasks/1)

<details>
  <summary>Solution</summary>

```mysql
SELECT name
FROM passenger;
```

</details>

2. Print the names of all airlines [(link)](https://sql-academy.org/en/trainer/tasks/2)

<details>
  <summary>Solution</summary>

```mysql
SELECT name
FROM Company;
```

</details>

3. Print all trips made from Moscow [(link)](https://sql-academy.org/en/trainer/tasks/3)

<details>
  <summary>Solution</summary>

```mysql
SELECT *
FROM trip
WHERE town_from = 'Moscow';
```

</details>

4. Print the names of people that end in "man" [(link)](https://sql-academy.org/en/trainer/tasks/4)

<details>
  <summary>Solution</summary>

```mysql
SELECT name
FROM passenger
WHERE name LIKE '%man';
```

</details>

5. Print the number of trips completed on TU-134 [(link)](https://sql-academy.org/en/trainer/tasks/5)

<details>
  <summary>Solution</summary>

```mysql
SELECT COUNT(*) AS count
FROM trip
WHERE plane = 'TU-134';
```

</details>

6. Which companies have flown on Boeing [(link)](https://sql-academy.org/en/trainer/tasks/6)

<details>
  <summary>Solution</summary>

```mysql
SELECT DISTINCT cp.name
FROM company cp
         JOIN trip tr ON cp.id = tr.company
WHERE plane = 'Boeing';
```

</details>

7. Display all the names of aircraft that you can fly to Moscow [(link)](https://sql-academy.org/en/trainer/tasks/7)

<details>
  <summary>Solution</summary>

```mysql
SELECT DISTINCT plane
FROM trip
WHERE town_to = 'Moscow';
```

</details>

8. What cities can I fly to from Paris and how long will it take? [(link)](https://sql-academy.org/en/trainer/tasks/8)

<details>
  <summary>Solution</summary>

```mysql
SELECT town_to,
       TIMEDIFF(time_in, time_out) AS flight_time
FROM trip
WHERE town_from = 'Paris';
```

</details>

9. What companies organize flights from Vladivostok? [(link)](https://sql-academy.org/en/trainer/tasks/9)

<details>
  <summary>Solution</summary>

```mysql
SELECT name
FROM trip tr
         JOIN company cp ON tr.company = cp.id
WHERE town_from = 'Vladivostok';
```

</details>

10. Print trips made from 10 a.m. to 2 p.m. on January 1, 1900. [(link)](https://sql-academy.org/en/trainer/tasks/10)

<details>
  <summary>Solution</summary>

```mysql
SELECT *
FROM trip
WHERE DATE(time_out) = '1900-01-01'
  AND TIME_FORMAT(time_out, '%H:%i') >= '10:00'
  AND TIME_FORMAT(time_out, '%H:%i') <= '14:00';
```

</details>

11. Print the passengers with the longest full name. Spaces, hyphens, and dots are considered part of the name.
    [(link)](https://sql-academy.org/en/trainer/tasks/11)

<details>
  <summary>Solution</summary>

```mysql
SELECT name
FROM passenger
ORDER BY LENGTH(name) DESC
LIMIT 1;
```

</details>

12. Print the id and number of passengers for all past trips [(link)](https://sql-academy.org/en/trainer/tasks/12)

<details>
  <summary>Solution</summary>

```mysql
SELECT trip,
       COUNT(*) AS count
FROM passenger ps
         JOIN Pass_in_trip pt ON ps.id = pt.passenger
GROUP BY trip;
```

</details>

13. Display the names of people who have a full namesake among passengers
[(link)](https://sql-academy.org/en/trainer/tasks/13)

<details>
  <summary>Solution</summary>

```mysql
SELECT name
FROM passenger
GROUP BY name
HAVING COUNT(*) > 1;
```

</details>

14. Which cities did Bruce Willis visit [(link)](https://sql-academy.org/en/trainer/tasks/14)

<details>
  <summary>Solution</summary>

```mysql
SELECT town_to
FROM passenger ps
         JOIN Pass_in_trip pt ON ps.id = pt.passenger
         JOIN trip tr ON tr.id = pt.trip
WHERE name = 'Bruce Willis';
```

</details>

15. Print the date and time of arrival of the passenger Steve Martin to London (London)
    [(link)](https://sql-academy.org/en/trainer/tasks/15)

<details>
  <summary>Solution</summary>

```mysql
SELECT time_in
FROM trip tr
         JOIN Pass_in_trip pt ON tr.id = pt.trip
         JOIN passenger ps ON pt.passenger = ps.id
WHERE name = 'Steve Martin'
  AND town_to = 'London';
```

</details>

16. Display the list of passengers sorted by the number of flights (in descending order) and name (in ascending order) 
who have made at least 1 flight. [(link)](https://sql-academy.org/en/trainer/tasks/16)

<details>
  <summary>Solution</summary>

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

17. Determine how much each family member spent in 2005. In the resulting sample, do not output those family members who
have not spent anything. [(link)](https://sql-academy.org/en/trainer/tasks/17)

<details>
  <summary>Solution</summary>

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

18. Print the name of the oldest person. If there are several of them, then output them all. 
[(link)](https://sql-academy.org/en/trainer/tasks/18)

<details>
  <summary>Solution</summary>

```mysql
SELECT member_name
FROM FamilyMembers
ORDER BY birthday ASC
LIMIT 1;
```

</details>

19. Determine which family member bought potatoes (potato) [(link)](https://sql-academy.org/en/trainer/tasks/19)

<details>
  <summary>Solution</summary>

```mysql
SELECT status
FROM FamilyMembers fm
         JOIN Payments ps ON fm.member_id = ps.family_member
         JOIN Goods gs ON ps.good = gs.good_id
WHERE good_name = 'potato'
GROUP BY status;
```

</details>

20. How much and who from the family spent on entertainment. Print family status, name, amount
[(link)](https://sql-academy.org/en/trainer/tasks/20)

<details>
  <summary>Solution</summary>

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

21. Identify products that have been purchased more than 1 time [(link)](https://sql-academy.org/en/trainer/tasks/21)

<details>
  <summary>Solution</summary>

```mysql
SELECT good_name
FROM Goods gs
         JOIN Payments ps ON gs.good_id = ps.good
GROUP BY good
HAVING COUNT(*) > 1;
```

</details>

22. Print the names of all mothers [(link)](https://sql-academy.org/en/trainer/tasks/22)

<details>
  <summary>Solution</summary>

```mysql
SELECT member_name
FROM FamilyMembers
WHERE status = 'mother';
```

</details>

23. Find the most expensive delicacy (delicacies) and print its price 
[(link)](https://sql-academy.org/en/trainer/tasks/23)

<details>
  <summary>Solution</summary>

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

24. Determine who spent how much in June 2005 [(link)](https://sql-academy.org/en/trainer/tasks/24)

<details>
  <summary>Solution</summary>

```mysql
SELECT member_name,
       (amount * unit_price) AS costs
FROM FamilyMembers fm
         JOIN Payments ps ON fm.member_id = ps.family_member
WHERE YEAR(date) = 2005
  AND MONTH(date) = 6;
```

</details>

25. Determine which products were not purchased in 2005 [(link)](https://sql-academy.org/en/trainer/tasks/25)

<details>
  <summary>Solution</summary>

```mysql
SELECT good_name
FROM Goods
WHERE good_id NOT IN (
    SELECT good
    FROM Payments
    WHERE YEAR(date) = 2005
);
```

</details>

26. Identify groups of products that were not purchased in 2005 [(link)](https://sql-academy.org/en/trainer/tasks/26)

<details>
  <summary>Solution</summary>

```mysql
SELECT good_type_name
FROM GoodTypes
WHERE good_type_id NOT IN (
    SELECT type
    FROM Goods gs
             JOIN Payments ps ON gs.good_id = ps.good
    WHERE YEAR(date) = 2005
    GROUP BY good_id
);
```

</details>

27. Find out how much was spent on each of the product groups in 2005. Print the group name and amount
    [(link)](https://sql-academy.org/en/trainer/tasks/27)

<details>
  <summary>Solution</summary>

```mysql
SELECT good_type_name,
       SUM(amount * unit_price) AS costs
FROM GoodTypes gt
         JOIN Goods gs ON gt.good_type_id = gs.type
         JOIN Payments ps ON gs.good_id = ps.good
WHERE YEAR(date) = 2005
GROUP BY good_type_name;
```

</details>

28. How many trips were made by airlines from Rostov to Moscow ? [(link)](https://sql-academy.org/en/trainer/tasks/28)

<details>
  <summary>Solution</summary>

```mysql
SELECT COUNT(*) AS COUNT
FROM Trip
WHERE town_from = 'Rostov'
  AND town_to = 'Moscow';
```

</details>

29. Print the names of passengers who flew to Moscow on TU-134 [(link)](https://sql-academy.org/en/trainer/tasks/29)

<details>
  <summary>Solution</summary>

```mysql
SELECT name
FROM Passenger ps
         JOIN Pass_in_trip pt ON ps.id = pt.passenger
         JOIN Trip tr ON pt.trip = tr.id
WHERE plane = 'TU-134'
  AND town_to = 'Moscow'
GROUP BY name;
```

</details>

30. Output the load (number of passengers) of each trip . Output the result in sorted form in descending order of load. 
[(link)](https://sql-academy.org/en/trainer/tasks/30)

<details>
  <summary>Solution</summary>

```mysql
SELECT trip,
       COUNT(passenger) AS count
FROM Pass_in_trip
GROUP BY trip
ORDER BY count DESC;
```

</details>

31. Print all family members with the last name Quincey. [(link)](https://sql-academy.org/en/trainer/tasks/31)

<details>
  <summary>Solution</summary>

```mysql
SELECT *
FROM FamilyMembers
WHERE member_name LIKE '% Quincey';
```

</details>

32. Display the average age of people (in years) stored in the database. The result is rounded to the nearest whole in 
the smaller side. [(link)](https://sql-academy.org/en/trainer/tasks/32)

<details>
  <summary>Solution</summary>

```mysql
SELECT FLOOR(
               AVG(TIMESTAMPDIFF(YEAR, birthday, CURRENT_TIMESTAMP))
           ) AS age
FROM FamilyMembers;
```

</details>

33. Find the average cost of caviar. The database stores data on purchases of red caviar and black caviar.
[(link)](https://sql-academy.org/en/trainer/tasks/33)

<details>
  <summary>Solution</summary>

```mysql
SELECT AVG(unit_price) AS cost
FROM Payments ps
         JOIN Goods gs ON ps.good = gs.good_id
WHERE good_name = 'red caviar'
   OR good_name = 'black caviar';
```

</details>

34. How many 10th grades are there in total [(link)](https://sql-academy.org/en/trainer/tasks/34)

<details>
  <summary>Solution</summary>

```mysql
SELECT COUNT(name) AS count
FROM Class
WHERE name LIKE '10 %';
```

</details>

35. СHow many different classrooms of the school were used on September 2, 2019 for classes?
    [(link)](https://sql-academy.org/en/trainer/tasks/35)

<details>
  <summary>Solution</summary>

```mysql
SELECT COUNT(DISTINCT classroom) AS count
FROM Student_in_class sc
         JOIN Class cl ON sc.class = cl.id
         JOIN Schedule sh ON sh.class = cl.id
WHERE DATE_FORMAT(date, '%e.%m.%Y') = '2.09.2019';
```

</details>

36. Print information about students living on Pushkin street (ul. Pushkina)?
    [(link)](https://sql-academy.org/en/trainer/tasks/36)

<details>
  <summary>Solution</summary>

```mysql
SELECT *
FROM Student
WHERE address RLIKE 'Pushkina';
```

</details>

37. How old is the youngest student ? [(link)](https://sql-academy.org/en/trainer/tasks/37)

<details>
  <summary>Solution</summary>

```mysql
SELECT TIMESTAMPDIFF(YEAR, birthday, CURRENT_TIMESTAMP) AS year
FROM Student
ORDER BY year ASC
LIMIT 1;
```

</details>

38. How many Ann (Anna) goes to school ? [(link)](https://sql-academy.org/en/trainer/tasks/38)

<details>
  <summary>Solution</summary>

```mysql
SELECT COUNT(*) AS count
FROM Student
WHERE first_name = 'Anna';
```

</details>

39. How many students are in grade 10 B ? [(link)](https://sql-academy.org/en/trainer/tasks/39)

<details>
  <summary>Solution</summary>

```mysql
SELECT COUNT(*) AS count
FROM Student_in_class sc
         JOIN Class cl ON sc.class = cl.id
WHERE name = '10 B';
```

</details>

40. Print the name of the subjects that Romashkin P. P. teaches ? [(link)](https://sql-academy.org/en/trainer/tasks/40)

<details>
  <summary>Solution</summary>

```mysql
SELECT name AS subjects
FROM Subject sj
         JOIN Schedule sc ON sj.id = sc.subject
         JOIN Teacher tc ON tc.id = sc.teacher
WHERE last_name = 'Romashkin'
  AND first_name LIKE 'P%'
  AND middle_name LIKE 'P%';
```

</details>

41. What time does the 4th class start according to the schedule ? [(link)](https://sql-academy.org/en/trainer/tasks/41)

<details>
  <summary>Solution</summary>

```mysql
SELECT start_pair
FROM Timepair
WHERE id = 4;
```

</details>

42. How long will the student be in school, studying from the 2nd to the 4th academic subject?
    [(link)](https://sql-academy.org/en/trainer/tasks/42)

<details>
  <summary>Solution</summary>

```mysql
SELECT TIMEDIFF(MAX(end_pair), MIN(start_pair)) AS time
FROM Timepair
WHERE id BETWEEN 2 AND 4;
```

</details>

43. Print the last names of the teachers who teach physical education (Physical Culture). Sort the teachers by last name
in alphabetical order. [(link)](https://sql-academy.org/en/trainer/tasks/43)

<details>
  <summary>Solution</summary>

```mysql
SELECT last_name
FROM Teacher tc
         JOIN Schedule sc ON tc.id = sc.teacher
         JOIN Subject sj ON sj.id = sc.subject
WHERE name = 'Physical Culture'
ORDER BY last_name;
```

</details>

44. Find the maximum age (number years) among students of 10 classes ?
[(link)](https://sql-academy.org/en/trainer/tasks/44)

<details>
  <summary>Solution</summary>

```mysql
SELECT TIMESTAMPDIFF(YEAR, birthday, CURRENT_TIMESTAMP) AS max_year
FROM Student st
         JOIN Student_in_class sc ON sc.student = st.id
         JOIN Class cl ON cl.id = sc.class
WHERE name LIKE '10 %'
ORDER BY max_year DESC
LIMIT 1;
```

</details>

45. Which classrooms were most often used for classes? Output the ones that have been used the maximum number of times. 
[(link)](https://sql-academy.org/en/trainer/tasks/45)

<details>
  <summary>Solution</summary>

```mysql
SELECT classroom
FROM Schedule
GROUP BY classroom
HAVING count(classroom) = (
    SELECT COUNT(*) AS count
    FROM Schedule
    GROUP BY classroom
    ORDER BY count DESC
    LIMIT 1
);
```

</details>

46. In which classes will the Krauze teacher introduce classes ? [(link)](https://sql-academy.org/en/trainer/tasks/46)

<details>
  <summary>Solution</summary>

```mysql
SELECT name
FROM Schedule sc
         JOIN Teacher tc ON tc.id = sc.teacher
         JOIN Class cl ON cl.id = sc.class
WHERE last_name = 'Krauze'
GROUP BY name;
```

</details>

47. How many classes did Krauze hold on August 30, 2019? [(link)](https://sql-academy.org/en/trainer/tasks/47)

<details>
  <summary>Solution</summary>

```mysql
SELECT COUNT(*) AS count
FROM Schedule sc
         JOIN Teacher tc ON tc.id = sc.teacher
WHERE DATE_FORMAT(date, '%e %M %Y') = '30 August 2019'
  AND last_name = 'Krauze';
```

</details>

48. Print the number of students in classes in descending order [(link)](https://sql-academy.org/en/trainer/tasks/48)

<details>
  <summary>Solution</summary>

```mysql
SELECT name,
       COUNT(student) AS count
FROM Class cl
         JOIN Student_in_class sc ON sc.class = cl.id
GROUP BY name
ORDER BY count DESC;
```

</details>

49. What percentage of students are styding in the "10 A" class? Print the answer in the range from 0 to 100 without 
rounding, for example, 96.0201. [(link)](https://sql-academy.org/en/trainer/tasks/49)

<details>
  <summary>Solution</summary>

```mysql
SELECT COUNT(*) * 100 / (
    SELECT COUNT(*)
    FROM Student_in_class
) AS percent
FROM Student_in_class sc
         JOIN Class cs ON cs.id = sc.class
WHERE name = '10 A';
```

</details>

50. What percentage of students were born in 2000? The result of rounding to the nearest whole in the smaller side.
    [(link)](https://sql-academy.org/en/trainer/tasks/50)

<details>
  <summary>Solution</summary>

```mysql
SELECT FLOOR(
                       COUNT(*) * 100 / (
                   SELECT COUNT(*)
                   FROM Student_in_class
               )
           ) AS percent
FROM Student_in_class sc
         JOIN Student st ON st.id = sc.student
WHERE YEAR(birthday) = 2000;
```

</details>

51. Add a product with the name "Cheese" and the type "food" to the list of goods (Goods).
    [(link)](https://sql-academy.org/en/trainer/tasks/51)

<details>
  <summary>Solution</summary>

```mysql
INSERT INTO Goods
SET good_id   = (
    SELECT COUNT(*) + 1
    FROM Goods AS gs
),
    good_name = 'Cheese',
    type      = (
        SELECT good_type_id
        FROM GoodTypes
        WHERE good_type_name = 'food'
    );
```

</details>

52. Add to the list of product types (GoodTypes) a new type of "auto". 
[(link)](https://sql-academy.org/en/trainer/tasks/52)

<details>
  <summary>Solution</summary>

```mysql
INSERT INTO GoodTypes
SET good_type_id   = (
    SELECT COUNT(*) + 1
    FROM GoodTypes AS gt
),
    good_type_name = 'auto';
```

</details>

53. Change the name "Andie Quincey" to the new "Andie Anthony". [(link)](https://sql-academy.org/en/trainer/tasks/53)

<details>
  <summary>Solution</summary>

```mysql
UPDATE FamilyMembers
SET member_name = 'Andie Anthony'
WHERE member_name = 'Andie Quincey';
```

</details>

54. Remove all family members whose last name is "Quincey". [(link)](https://sql-academy.org/en/trainer/tasks/54)

<details>
  <summary>Solution</summary>

```mysql
DELETE
FROM FamilyMembers
WHERE member_name LIKE '% Quincey';
```

</details>

55. Delete the companies that made the least number of flights. [(link)](https://sql-academy.org/en/trainer/tasks/55)

<details>
  <summary>Solution</summary>

```mysql
DELETE
FROM company
WHERE id IN (
    SELECT company
    FROM trip
    GROUP BY company
    HAVING COUNT(*) = (
        SELECT COUNT(*) AS count
        FROM trip
        GROUP BY company
        ORDER BY count
        LIMIT 1
    )
);
```

</details>

56. Delete all flights made from Moscow (Moscow). [(link)](https://sql-academy.org/en/trainer/tasks/56)

<details>
  <summary>Solution</summary>

```mysql
DELETE
FROM trip
WHERE town_from = 'Moscow';
```

</details>

57. Reschedule the schedule of all classes for 30 minutes. forward [(link)](https://sql-academy.org/en/trainer/tasks/57)

<details>
  <summary>Solution</summary>

```mysql
UPDATE Timepair
SET start_pair = ADDTIME(start_pair, '00:30:00'),
    end_pair   = ADDTIME(end_pair, '00:30:00');
```

</details>

58.Add a 5-star comment on housing located at 11218 Friel Place, New York on behalf of George Clooney 
[(link)](https://sql-academy.org/en/trainer/tasks/58)

<details>
  <summary>Solution</summary>

```mysql
INSERT INTO Reviews
SET id             = (
    SELECT COUNT(*) + 1
    FROM Reviews rw
),
    reservation_id = (
        SELECT rs.id
        FROM Reservations rs
                 JOIN Rooms rm ON rm.id = rs.room_id
                 JOIN Users us ON rs.user_id = us.id
        WHERE address = '11218, Friel Place, New York'
          AND name = 'George Clooney'
    ),
    rating         = 5;
```

</details>

59. Print users who have a Belarusian phone number? The telephone code of Belarus is +375.
    [(link)](https://sql-academy.org/en/trainer/tasks/59)

<details>
  <summary>Solution</summary>

```mysql
SELECT *
FROM Users
WHERE phone_number LIKE '+375 %';
```

</details>

60. Print the identifiers of the teachers who taught at least once for the entire time in each of the eleventh grades. 
[(link)](https://sql-academy.org/en/trainer/tasks/60)

<details>
  <summary>Solution</summary>

```mysql
SELECT teacher
FROM Schedule sc
         JOIN Class cl ON sc.class = cl.id
WHERE name LIKE '11 %'
GROUP BY teacher
HAVING COUNT(DISTINCT name) = 2;
```

</details>

61. Output a list of rooms that have been reserved for at least one day in the 12th week of 2020. In this task, take a 
period of seven days as one week, the first of which begins on January 1, 2020. For example, the first week of the year 
is January 1-7, and the third is January 15-21. [(link)](https://sql-academy.org/en/trainer/tasks/61)

<details>
  <summary>Solution</summary>

```mysql
SELECT Rooms.*
FROM Reservations
         JOIN Rooms ON Rooms.id = Reservations.room_id
WHERE WEEK(start_date, 1) = 12
  AND YEAR(start_date) = 2020;
```

</details>

62. List the 2nd-level domain names used by users for email in descending order of popularity. The resulting result must
be additionally sorted in ascending order of domain names. [(link)](https://sql-academy.org/en/trainer/tasks/62)

<details>
  <summary>Solution</summary>

```mysql
SELECT SUBSTRING_INDEX(email, '@', -1)        AS domain,
       COUNT(substring_index(email, '@', -1)) AS count
FROM Users
GROUP BY domain
ORDER BY count DESC,
         domain;
```

</details>

63. Print the sorted list (in ascending order) of students' lastnames and firstnames in the form Surname.F.
    [(link)](https://sql-academy.org/en/trainer/tasks/63)

<details>
  <summary>Solution</summary>

```mysql
SELECT CONCAT(last_name, '.', LEFT(first_name, 1), '.') AS name
FROM Student
ORDER BY name;
```

</details>

64. Display the number of reservations for each month of each year that had at least 1 reservation. Sort the result in 
ascending order of reservation date. [(link)](https://sql-academy.org/en/trainer/tasks/64)

<details>
  <summary>Solution</summary>

```mysql
SELECT YEAR(start_date)  AS year,
       MONTH(start_date) AS month,
       COUNT(*)          AS amount
FROM Reservations
GROUP BY YEAR(start_date),
         MONTH(start_date)
ORDER BY year,
         month;
```

</details>

65. It is necessary to display the rating for rooms that have been rented at least once, as the average of the rating of
reviews, rounded down to the integer. [(link)](https://sql-academy.org/en/trainer/tasks/65)

<details>
  <summary>Solution</summary>

```mysql
SELECT room_id,
       FLOOR(AVG(rating)) AS rating
FROM Reservations rs
         JOIN Reviews rw ON rw.reservation_id = rs.id
GROUP BY room_id;
```

</details>

66. Display a list of rooms with all amenities (TV, Internet, kitchen, and air conditioning), as well as the total 
number of days and the amount for all days of renting each of these rooms. 
[(link)](https://sql-academy.org/en/trainer/tasks/66)

<details>
  <summary>Solution</summary>

```mysql
SELECT home_type,
       address,
       IFNULL(SUM(total / rs.price), 0) AS days,
       IFNULL(SUM(total), 0)            AS total_fee
FROM Rooms rm
         LEFT JOIN Reservations rs ON rs.room_id = rm.id
WHERE has_tv = 1
  AND has_internet = 1
  AND has_kitchen = 1
  AND has_air_con = 1
GROUP BY home_type,
         address;
```

</details>

67. Вывести время отлета и время прилета для каждого перелета в формате "ЧЧ:ММ, ДД.ММ - ЧЧ:ММ, ДД.ММ", где часы и минуты
    с ведущим нулем, а день и месяц без. [(link)](https://sql-academy.org/en/trainer/tasks/67)

<details>
  <summary>Solution</summary>

```mysql
SELECT CONCAT(
               DATE_FORMAT(time_out, '%H:%i, %e.%c'),
               ' - ',
               DATE_FORMAT(time_in, '%H:%i, %e.%c')
           ) AS flight_time
FROM Trip;
```

</details>

68. Для каждой комнаты, которую снимали как минимум 1 раз, найдите имя человека, снимавшего ее последний раз, и дату,
    когда он выехал [(link)](https://sql-academy.org/en/trainer/tasks/68)

<details>
  <summary>Solution</summary>

```mysql
SELECT rs.room_id,
       name,
       date AS end_date
FROM (
         SELECT room_id,
                MAX(end_date) AS date
         FROM Reservations
         GROUP BY room_id
     ) rs
         JOIN Reservations rsv ON rs.room_id = rsv.room_id
    AND rs.date = rsv.end_date
         JOIN Users us ON rsv.user_id = us.id;
```

</details>

69. Вывести идентификаторы всех владельцев комнат, что размещены на сервисе бронирования жилья и сумму, которую они
    заработали [(link)](https://sql-academy.org/en/trainer/tasks/69)

<details>
  <summary>Solution</summary>

```mysql
SELECT owner_id,
       IFNULL(SUM(total), 0) AS total_earn
FROM Rooms rm
         LEFT JOIN Reservations rs ON rm.id = rs.room_id
GROUP BY owner_id;
```

</details>

70. Необходимо категоризовать жилье на economy, comfort, premium по цене соответственно <= 100,
    100 < цена < 200, >= 200. В качестве результата вывести таблицу с названием категории и количеством жилья, попадающего в
    данную категорию [(link)](https://sql-academy.org/en/trainer/tasks/70)

<details>
  <summary>Solution</summary>

```mysql
SELECT CASE
           WHEN price <= 100 THEN 'economy'
           WHEN price > 100
               AND price < 200 THEN 'comfort'
           WHEN price >= 200 THEN 'premium'
           END      AS category,
       COUNT(price) AS count
FROM Rooms
GROUP BY category;
```

</details>

71. Найдите какой процент пользователей, зарегистрированных на сервисе бронирования, хоть раз арендовали или сдавали в
    аренду жилье. Результат округлите до сотых. [(link)](https://sql-academy.org/en/trainer/tasks/71)

<details>
  <summary>Solution</summary>

```mysql
SELECT ROUND(
                       (
                           SELECT COUNT(*)
                           FROM (
                                    SELECT DISTINCT owner_id
                                    FROM Rooms rm
                                             JOIN Reservations rs ON rm.id = rs.room_id
                                    UNION
                                    SELECT user_id
                                    FROM Reservations
                                ) active_users
                       ) * 100 / (
                           SELECT COUNT(*)
                           FROM Users
                       ),
                       2
           ) AS percent;
```

</details>

72. Выведите среднюю стоимость бронирования для комнат, которых бронировали хотя бы один раз. Среднюю стоимость
    необходимо округлить до целого значения вверх. [(link)](https://sql-academy.org/en/trainer/tasks/72)

<details>
  <summary>Solution</summary>

```mysql
SELECT room_id,
       CEILING(AVG(price)) AS avg_price
FROM Reservations
GROUP BY room_id;
```

</details>

73. Выведите id тех комнат, которые арендовали нечетное количество раз
    [(link)](https://sql-academy.org/en/trainer/tasks/73)

<details>
  <summary>Solution</summary>

```mysql
SELECT room_id,
       COUNT(*) AS count
FROM Reservations
GROUP BY room_id
HAVING count % 2 != 0;
```

</details>

74. Выведите идентификатор и признак наличия интернета в помещении. Если интернет в сдаваемом жилье присутствует, то
    выведите «YES», иначе «NO». [(link)](https://sql-academy.org/en/trainer/tasks/74)

<details>
  <summary>Solution</summary>

```mysql
SELECT id,
       IF(has_internet = 1, 'YES', 'NO') AS has_internet
FROM Rooms;
```

</details>

75. Выведите фамилию, имя и дату рождения студентов, кто был рожден в мае.
    [(link)](https://sql-academy.org/en/trainer/tasks/75)

<details>
  <summary>Solution</summary>

```mysql
SELECT last_name,
       first_name,
       birthday
FROM Student
WHERE MONTHNAME(birthday) = 'May';
```

</details>

76. Вывести имена всех пользователей сервиса бронирования жилья, а также два признака: является ли пользователь
    собственником какого-либо жилья (is_owner) и является ли пользователь арендатором (is_tenant). В случае наличия у
    пользователя признака необходимо вывести в соответствующее поле 1, иначе 0.
    [(link)](https://sql-academy.org/en/trainer/tasks/76)

<details>
  <summary>Solution</summary>

```mysql
SELECT name,
       IF(
                   id IN (
                   SELECT owner_id
                   FROM Rooms
               ),
                   1,
                   0
           ) AS is_owner,
       IF(
                   id IN (
                   SELECT user_id
                   FROM Reservations
               ),
                   1,
                   0
           ) AS is_tenant
FROM Users;
```

</details>

77. Создайте представление с именем "People", которое будет содержать список имен (first_name) и фамилий (last_name)
    всех студентов (Student) и преподавателей(Teacher) [(link)](https://sql-academy.org/en/trainer/tasks/77)

<details>
  <summary>Solution</summary>

```mysql
CREATE VIEW People AS
SELECT first_name,
       last_name
FROM Student
UNION
SELECT first_name,
       last_name
FROM Teacher
```

</details>
