## day 01

### exercise 00

Task text:
Please write a SQL statement which returns menu’s identifier and pizza names from `menu` table and person’s identifier and person name from `person` table in one global list (with column names as presented on a sample below) ordered by object_id and then by object_name columns.

code
```
SELECT id AS object_id, pizza_name AS object_name 
FROM menu
UNION ALL
SELECT 
    id AS object_id, 
    name AS object_name 
FROM person
ORDER BY object_id, object_name;
```
output:
![](/day01/img/ex1.png)

### exercise 01

Task text:
Please modify a SQL statement from “exercise 00” by removing the object_id column. Then change ordering by object_name for part of data from the `person` table and then from `menu` table (like presented on a sample below). Please save duplicates!

code
```
SELECT object_name 
FROM (SELECT pizza_name AS object_name 
    FROM menu
    UNION ALL
    SELECT name AS object_name 
    FROM person) AS combined
ORDER BY 
    CASE 
        WHEN object_name IN (SELECT name FROM person) THEN 1 
        ELSE 2 
    END, 
    object_name;
```

output:
![](/day01/img/ex2.png)

### exercise 02

Task text:
Please write a SQL statement which returns unique pizza names from the `menu` table and orders them by pizza_name column in descending mode. Please pay attention to the Denied section.

code
```
SELECT pizza_name 
FROM menu 
except 
select pizza_name
from menu
where pizza_name like '%mushroom%'
ORDER BY pizza_name DESC;
```

output:
![](/day01/img/ex3.png)

### exercise 03

Task text:
Please write a SQL statement which returns common rows for attributes order_date, person_id from `person_order` table from one side and visit_date, person_id from `person_visits` table from the other side (please see a sample below). In other words, let’s find identifiers of persons, who visited and ordered some pizza on the same day. Actually, please add ordering by action_date in ascending mode and then by person_id in descending mode.

code
```
SELECT 
    po.person_id,
    po.order_date AS action_date
FROM 
    person_order po
JOIN 
    person_visits pv ON po.person_id = pv.person_id 
                     AND po.order_date = pv.visit_date
ORDER BY 
    action_date ASC, 
    po.person_id DESC;
```

output:
![](/day01/img/ex4.png)

### exercise 04

Task text:
Please write a SQL statement which returns a difference (minus) of person_id column values with saving duplicates between `person_order` table and `person_visits` table for order_date and visit_date are for 7th of January of 2022

code
```
select person_id 
from(
select person_id, 1 as first
from person_order
where order_date = '2022-01-07'
except all
select person_id , -1 as first
from person_visits
where visit_date = '2022-01-07'
)as combo 
group by person_id
having sum(first) > 0
order by person_id desc
```

output:
![](/day01/img/ex5.png)

### exercise 05

Task text:
Please write a SQL statement which returns all possible combinations between `person` and `pizzeria` tables and please set ordering by person identifier and then by pizzeria identifier columns. Please take a look at the result sample below. Please be aware column's names can be different for you.

code
```
SELECT 
    p.id AS person_id,
    p.name AS person_name,
    p.age,
    p.gender,
    p.address,
    pi.id AS pizzeria_id,
    pi.name AS pizzeria_name,
    pi.rating
FROM 
    person p
CROSS JOIN 
    pizzeria pi
ORDER BY 
    p.id, 
    pi.id;
```

output:
![](/day01/img/ex6.png)

### exercise 06

Task text:
Let's return our mind back to exercise #03 and change our SQL statement to return person names instead of person identifiers and change ordering by action_date in ascending mode and then by person_name in descending mode. Please take a look at a data sample below.

code
```
SELECT 
    o.order_date AS action_date,
    p.name AS person_name
FROM 
    person_order o
JOIN 
    person p ON o.person_id = p.id
ORDER BY 
    o.order_date ASC, 
    p.name DESC;
```

output:
![](/day01/img/ex7.png)

### exercise 07

Task text:
Please write a SQL statement which returns the date of order from the `person_order` table and corresponding person name (name and age are formatted as in the data sample below) which made an order from the `person` table. Add a sort by both columns in ascending mode.

code
```
SELECT 
    o.order_date,
    CONCAT(p.name, ' (age:', p.age, ')') AS person_information
FROM 
    person_order o
JOIN 
    person p ON o.person_id = p.id
ORDER BY 
    o.order_date ASC, 
    p.name ASC;
```

output:
![](/day01/img/ex8.png)

### exercise 08

Task text:
Please rewrite a SQL statement from exercise #07 by using NATURAL JOIN construction. The result must be the same like for exercise #07.  

code
```
SELECT 
    o.order_date,
    CONCAT(p.name, ' (age:', p.age, ')') AS person_information
FROM 
    person_order o
NATURAL JOIN 
    person p
ORDER BY 
    o.order_date ASC, 
    p.name ASC;
```

output:
![](/day01/img/ex9.png)

### exercise 09

Task text:
Please write 2 SQL statements which return a list of pizzerias names which have not been visited by persons by using IN for 1st one and EXISTS for the 2nd one.

code
pt1
```
SELECT 
    name 
FROM 
    pizzeria 
WHERE 
    id NOT IN (SELECT DISTINCT pizzeria_id FROM person_visits);
```

output:
![](/day01/img/ex10pt1.png)

pt2
```
SELECT 
    name 
FROM 
    pizzeria p 
WHERE 
    NOT EXISTS (SELECT 1 FROM person_visits pv WHERE pv.pizzeria_id = p.id);
```

output:
![](/day01/img/ex10pt2.png)

### exercise 10

Task text:
Please write a SQL statement which returns a list of the person names which made an order for pizza in the corresponding pizzeria. 
The sample result (with named columns) is provided below and yes ... please make ordering by 3 columns (`person_name`, `pizza_name`, `pizzeria_name`) in ascending mode.

code

```
SELECT 
    p.name AS person_name,
    m.pizza_name,
    pi.name AS pizzeria_name
FROM 
    person_order o
JOIN 
    menu m ON o.menu_id = m.id
JOIN 
    pizzeria pi ON m.pizzeria_id = pi.id
JOIN 
    person p ON o.person_id = p.id
ORDER BY 
    p.name ASC, 
    m.pizza_name ASC, 
    pi.name ASC;
```

output:
![](/day01/img/ex11.png)

