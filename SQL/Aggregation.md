## **Revising Aggregations - The Count Function**

Query a *count* of the number of cities in **CITY** having a *Population* larger than **100000** .

**Input Format**

The **CITY** table is described as follows:

| Field | Type |
| --- | --- |
| ID | NUMBER |
| NAME | VARCHAR |
| COUNTRYCODE | VARCHAR |
| DISTRICT | VARCHAR |
| POPULATION | NUMBER |

```sql
-- MS SQL Server, MySQL
select count(COUNTRYCODE) 
from CITY 
where POPULATION > 100000
```

## **Revising Aggregations - The Sum Function**

Query the total population of all cities in **CITY** where *District* is **California**.

**Input Format**

The **CITY** table is described as follows:

| Field | Type |
| --- | --- |
| ID | NUMBER |
| NAME | VARCHAR |
| COUNTRYCODE | VARCHAR |
| DISTRICT | VARCHAR |
| POPULATION | NUMBER |

```sql
-- MS SQL Server, MySQL
select sum(POPULATION) 
from CITY 
where DISTRICT = 'California'
```

## Revising Aggregations - Averages

Query the average population of all cities in **CITY** where *District* is **California**.

**Input Format**

The **CITY** table is described as follows:

| Field | Type |
| --- | --- |
| ID | NUMBER |
| NAME | VARCHAR |
| COUNTRYCODE | VARCHAR |
| DISTRICT | VARCHAR |
| POPULATION | NUMBER |

```sql
-- MS SQL Server, MySQL
select avg(POPULATION) 
from CITY 
where DISTRICT = 'California'
```

## Average Population

Query the average population for all cities in **CITY**, rounded *down* to the nearest integer.

**Input Format**

The **CITY** table is described as follows:

| Field | Type |
| --- | --- |
| ID | NUMBER |
| NAME | VARCHAR |
| COUNTRYCODE | VARCHAR |
| DISTRICT | VARCHAR |
| POPULATION | NUMBER |

```sql
--MS SQL
DECLARE @value float(10)
SET @value = .1234567890
SELECT ROUND(@value, 10) -- 0.123456791
SELECT CEILING(@value)   -- 1
SELECT FLOOR(@value)     -- 0
```

```sql
-- MS SQL Server, MySQL
select floor(avg(POPULATION)) 
from CITY
```

## Japan Population

Query the sum of the populations for all Japanese cities in **CITY**. The *COUNTRYCODE* for Japan is **JPN**.

**Input Format**

The **CITY** table is described as follows:

| Field | Type |
| --- | --- |
| ID | NUMBER |
| NAME | VARCHAR |
| COUNTRYCODE | VARCHAR |
| DISTRICT | VARCHAR |
| POPULATION | NUMBER |

```sql
-- MS SQL Server, MySQL
select sum(POPULATION) 
from CITY 
where COUNTRYCODE = 'JPN'
```

## Population Density Difference

Query the difference between the maximum and minimum populations in **CITY**.

**Input Format**

The **CITY** table is described as follows:

| Field | Type |
| --- | --- |
| ID | NUMBER |
| NAME | VARCHAR |
| COUNTRYCODE | VARCHAR |
| DISTRICT | VARCHAR |
| POPULATION | NUMBER |

```sql
select max(POPULATION) - min(POPULATION) 
from CITY;
```

## The Blunder

Samantha was tasked with calculating the average monthly salaries for all employees in the **EMPLOYEES** table, but did not realize her keyboard's **0** key was broken until after completing the calculation. She wants your help finding the difference between her miscalculation (using salaries with any zeros removed), and the actual average salary.

Write a query calculating the amount of error (i.e.:  ***actual - miscalculated*** average monthly salaries), and round it up to the next integer.

**Input Format**

The **EMPLOYEES** table is described as follows:

| Column | Type |
| --- | --- |
| ID | Integer |
| Name | String |
| Salary | Integer |

**Note:** *Salary* is per month.

**Constraints**

**1000 < Salary ≤ $10^5$** 

**Sample Input**

| ID | Name | Salary |
| --- | --- | --- |
| 1 | Kristeen | 1420 |
| 2 | Ashley | 2006 |
| 3 | Julia | 2210 |
| 4 | Maria | 3000 |

**Sample Output**

`2061`

**Explanation**

The table below shows the salaries *without zeros* as they were entered by Samantha:

| ID | Name | Salary |
| --- | --- | --- |
| 1 | Kristeen | 142 |
| 2 | Ashley | 26 |
| 3 | Julia | 221 |
| 4 | Maria | 3 |

Samantha computes an average salary of **98.00**. The *actual* average salary is **2159.00**.

The resulting error between the two calculations is **2159.00 - 98.00 = 2061.00** . Since it is equal to the integer **2061**, it does not get rounded up.

```sql
--MS SQL Server
select cast(ceiling((avg(cast(SALARY AS float)) - avg(cast(replace(SALARY, 0, '')as FLOAT)))) as INT) 
FROM EMPLOYEES;
```

## **Top Earners**

We define an employee's *total earnings* to be their monthly  worked, and the *maximum total earnings* to be the maximum total earnings for any employee in the **Employee** table. Write a query to find the *maximum total earnings* for all employees as well as the total number of employees who have maximum total earnings. Then print these values as  **2** space-separated integers.

**Input Format**

The **Employee** table containing employee data for a company is described as follows:

| Column | Name |
| --- | --- |
| employee_id | Integer |
| name | String |
| months | Integer |
| salary | Integer |

where *employee_id* is an employee's ID number, *name* is their name, *months* is the total number of months they've been working for the company, and *salary* is the their monthly salary.

**Sample Input**

**Sample Output**

`69952 1`

**Explanation**

The table and earnings data is depicted in the following diagram:

The maximum *earnings* value is **69952**. The only employee with *earnings = **69952*** is *Kimberly*
, so we print the maximum *earnings* value ( **69952**) and a count of the number of employees who have earned **$69952** (which is **1**) as two space-separated values.

```sql
--MS SQL Server
select top 1 MONTHS*SALARY , count(MONTHS*SALARY)
from EMPLOYEE
group by MONTHS*SALARY
order by MONTHS*SALARY desc
```

## Weather Observation Station 2

Query the following two values from the **STATION** table:

1. The sum of all values in *LAT_N* rounded to a scale of **2** decimal places.
2. The sum of all values in *LONG_W* rounded to a scale of **2** decimal places.

**Input Format**

The **STATION** table is described as follows:

| ID | NUMBER |
| --- | --- |
| CITY | VARCHAR2(21) |
| STATE | VARCHAR2(21) |
| LAT_N | NUMBER |
| LONG_W | NUMBER |

where *LAT_N* is the northern latitude and *LONG_W* is the western longitude.

**Output Format**

Your results must be in the form:

lat lon

where *lat* is the sum of all values in *LAT_N* and *lon* is the sum of all values in *LONG_W*. Both results must be rounded to a scale of **2** decimal places.

```sql
select cast(sum(LAT_N) as decimal(10,2)), cast(sum(LONG_W) as decimal(10,2))
from STATION
```

## **Weather Observation Station 13**

Query the sum of *Northern Latitudes* (*LAT_N*) from **STATION** having values greater than  **38.7880** and less than **137.2345.** Truncate your answer to 4 decimal places.

**Input Forma**

The **STATION** table is described as follows:

| Field | Type |
| --- | --- |
| ID | NUMBER |
| CITY | VARCHAR2(21) |
| STATE | VARCHAR2(21) |
| LAT_N | NUMBER |
| LONG_W | NUMBER |

where *LAT_N* is the northern latitude and *LONG_W* is the western longitude.

```sql
select convert(decimal(10,4),sum(LAT_N))
from STATION
where LAT_N between 38.7880 and 137.2345;

select cast(sum(LAT_N) as decimal(10,4))
from STATION
where LAT_N between 38.7880 and 137.2345;
```

## **Weather Observation Station 14**

Query the greatest value of the *Northern Latitudes* (*LAT_N*) from **STATION** that is less than **137.2345**. Truncate your answer to **4** decimal places.

**Input Forma**

The **STATION** table is described as follows:

| Field | Type |
| --- | --- |
| ID | NUMBER |
| CITY | VARCHAR2(21) |
| STATE | VARCHAR2(21) |
| LAT_N | NUMBER |
| LONG_W | NUMBER |

where *LAT_N* is the northern latitude and *LONG_W* is the western longitude.

```sql
select cast(max(LAT_N) as decimal(10,4))
from STATION
where LAT_N < 137.2345
```

## **Weather Observation Station 15**

Query the *Western Longitude* (*LONG_W*) for the largest *Northern Latitude* (*LAT_N*) in **STATION** that is less than **137.2345** . Round your answer to **4** decimal places.

**Input Format**

The **STATION** table is described as follows:

| Field | Type |
| --- | --- |
| ID | NUMBER |
| CITY | VARCHAR2(21) |
| STATE | VARCHAR2(21) |
| LAT_N | NUMBER |
| LONG_W | NUMBER |

where *LAT_N* is the northern latitude and *LONG_W* is the western longitude.

```sql
select top 1 cast(LONG_W as decimal(10,4))
from STATION
where LAT_N <= 137.2345
order by LAT_N desc
```

## **Weather Observation Station 16**

Query the smallest *Northern Latitude* (*LAT_N*) from **STATION** that is greater than 38.7780 . Round your answer to  decimal places.

**Input Format**

The **STATION** table is described as follows:

| ID | NUMBER |
| --- | --- |
| CITY | VARCHAR2(21) |
| STATE | VARCHAR2(21) |
| LAT_N | NUMBER |
| LONG_W | NUMBER |

```sql
select cast(min(LAT_N) as decimal(10,4)) 
from STATION 
where LAT_N >= 38.7780
```

## **Weather Observation Station 17**

Query the *Western Longitude* (*LONG_W*)where the smallest *Northern Latitude* (*LAT_N*) in **STATION** is greater than **38.7780** . Round your answer to **4** decimal places.

**Input Format**

The **STATION** table is described as follows:

| ID | NUMBER |
| --- | --- |
| CITY | VARCHAR2(21) |
| STATE | VARCHAR2(21) |
| LAT_N | NUMBER |
| LONG_W | NUMBER |

where *LAT_N* is the northern latitude and *LONG_W* is the western longitude.

```sql
select top 1 cast(LONG_W as decimal(10,4)) 
from STATION
where LAT_N >= 38.7780
order by LAT_N asc
```

## **Weather Observation Station 18**

Consider **P1(a, b)** and  **P2(c, d)** to be two points on a *2D* plane.

- **a** happens to equal the minimum value in *Northern Latitude* (*LAT_N* in **STATION**).
- **b** happens to equal the minimum value in *Western Longitude* (*LONG_W* in **STATION**).
- **c** happens to equal the maximum value in *Northern Latitude* (*LAT_N* in **STATION**).
- **d** happens to equal the maximum value in *Western Longitude* (*LONG_W* in **STATION**).

Query the [Manhattan Distance](https://xlinux.nist.gov/dads/HTML/manhattanDistance.html) between points **P1** and **P2** and round it to a scale of **4** decimal places.

**Input Format**

The **STATION** table is described as follows:

| ID | NUMBER |
| --- | --- |
| CITY | VARCHAR2(21) |
| STATE | VARCHAR2(21) |
| LAT_N | NUMBER |
| LONG_W | NUMBER |

where *LAT_N* is the northern latitude and *LONG_W* is the western longitude.

```sql
Manhattan distance
Definition: The distance between two points measured along axes at right angles. In a plane with p1 at (x1, y1) and p2 at (x2, y2), it is |x1 - x2| + |y1 - y2|
select cast(abs(min(LAT_N) - max(LAT_N)) + abs(min(LONG_W) - max(LONG_W)) as decimal(10,4))
from STATION 
```

## **Weather Observation Station 19**

Consider P1(a,c) and P2(b,d) to be two points on a 2D plane where (a,b) are the respective minimum and (c,d) maximum values of *Northern Latitude* (*LAT_N*) and  are the respective minimum and maximum values of *Western Longitude* (*LONG_W*) in **STATION**.

Query the [Euclidean Distance](https://en.wikipedia.org/wiki/Euclidean_distance) between points P1 and P2 and *format your answer* to display 4 decimal digits.

**Input Format**

The **STATION** table is described as follows:

| ID | NUMBER |
| --- | --- |
| CITY | VARCHAR2(21) |
| STATE | VARCHAR2(21) |
| LAT_N | NUMBER |
| LONG_W | NUMBER |

```sql
select cast(SQRT(power(min(LAT_N) - max(LAT_N),2) + power(min(LONG_W) - max(LONG_W),2)) as decimal(10,4))
from STATION 
```

## **Weather Observation Station 20**

A [*median*](https://en.wikipedia.org/wiki/Median) is defined as a number separating the higher half of a data set from the lower half. Query the *median* of the *Northern Latitudes* (*LAT_N*) from **STATION** and round your answer to 4 decimal places.

**Input Format**

The **STATION** table is described as follows:

| ID | NUMBER |
| --- | --- |
| CITY | VARCHAR2(21) |
| STATE | VARCHAR2(21) |
| LAT_N | NUMBER |
| LONG_W | NUMBER |

where *LAT_N* is the northern latitude and *LONG_W* is the western longitude.

```sql
with row_number_lat_n as
(
    select case when RowNumberAsc = RowNumberDesc and TotalRow % 2 <> 0  then 'Select'
                when RowNumberAsc + 1 = RowNumberDesc and TotalRow % 2 = 0 then 'Select'
                when RowNumberAsc - 1 = RowNumberDesc and TotalRow % 2 = 0 then 'Select'
           else 'Delete'
           end as CheckMedian
          ,LAT_N
    from
    (
        select LAT_N
        , row_number() over(order by LAT_N asc) as RowNumberAsc
        , row_number() over(order by LAT_N desc) as RowNumberDesc
        , count(*) over() as TotalRow
        from STATION
    ) x
)
select cast(avg(LAT_N) as decimal(10,4))  
from row_number_lat_n
where CheckMedian = 'Select'
```
