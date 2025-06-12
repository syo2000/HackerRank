# Basic join

Created time: June 28, 2023 11:00 AM

## **African Cities**

Given the **CITY** and **COUNTRY** tables, query the names of all cities where the *CONTINENT* is *'Africa'*.

**Note:** *CITY.CountryCode* and *COUNTRY.Code* are matching key columns.

**Input Format**

The **CITY** and **COUNTRY** tables are described as follows:

**CITY**

| **Field** | **Type** |
| --- | --- |
| ID | NUMBER |
| NAME | VARCHAR2(17) |
| COUNTRYCODE | VARCHAR2(3) |
| DISTRICT | VARCHAR2(20) |
| POPULATION | NUMBER |

**COUNTRY**

| **Field** | **Type** |
| --- | --- |
| CODE | VARCHAR2(3) |
| NAME | VARCHAR2(44) |
| CONTINENT | VARCHAR2(13) |
| REGION | VARCHAR2(25) |
| SURFACEAREA | NUMBER |
| INDEPYEAR | VARCHAR2(5) |
| POPULATION | NUMBER |
| LIFEEXPECTANCY | VARCHAR2(4) |
| GNP | NUMBER |
| GNPOLD | VARCHAR2(9) |
| LOCALNAME | VARCHAR2(44) |
| GOVERNMENTFORM | VARCHAR2(44) |
| HEADOFSTATE | VARCHAR2(32) |
| CAPITAL | VARCHAR2(4) |
| CODE2 | VARCHAR2(2) |

```sql
select c.NAME
from CITY c
left join COUNTRY ctry
on c.COUNTRYCODE = ctry.CODE
where ctry.CONTINENT = 'Africa' 
```

## **Average Population of Each Continent**

Given the **CITY** and **COUNTRY** tables, query the names of all the continents (*COUNTRY.Continent*) and their respective average city populations (*CITY.Population*) rounded *down* to the nearest integer.

**Note:** *CITY.CountryCode* and *COUNTRY.Code* are matching key columns.

**Input Format**

The **CITY** and **COUNTRY** tables are described as follows:

| **Field** | **Type** |
| --- | --- |
| ID | NUMBER |
| NAME | VARCHAR2(17) |
| COUNTRYCODE | VARCHAR2(3) |
| DISTRICT | VARCHAR2(20) |
| POPULATION | NUMBER |

| **Field** | **Type** |
| --- | --- |
| CODE | VARCHAR2(3) |
| NAME | VARCHAR2(44) |
| CONTINENT | VARCHAR2(13) |
| REGION | VARCHAR2(25) |
| SURFACEAREA | NUMBER |
| INDEPYEAR | VARCHAR2(5) |
| POPULATION | NUMBER |
| LIFEEXPECTANCY | VARCHAR2(4) |
| GNP | NUMBER |
| GNPOLD | VARCHAR2(9) |
| LOCALNAME | VARCHAR2(44) |
| GOVERNMENTFORM | VARCHAR2(44) |
| HEADOFSTATE | VARCHAR2(32) |
| CAPITAL | VARCHAR2(4) |
| CODE2 | VARCHAR2(2) |

```sql
select ctry.CONTINENT, floor(avg(c.POPULATION))
from CITY c
left join COUNTRY ctry
on c.COUNTRYCODE = ctry.CODE
where ctry.CONTINENT is not null
group by ctry.CONTINENT
```

# **The Report**

You are given two tables: *Students* and *Grades*. *Students* contains three columns *ID*, *Name* and *Marks*.

| **Column**  | **Type** |
| --- | --- |
| ID | Integer |
| Name | String |
| Marks | Integer |

Grades contains the following data:

| **Grade** | **Min_Mark** | **Max_Mark** |
| --- | --- | --- |
| 1 | 0 | 9 |
| 2 | 10 | 19 |
| 3 | 20 | 29 |
| 4 | 30 | 39 |
| 5 | 40 | 49 |
| 6 | 50 | 59 |
| 7 | 60 | 69 |
| 8 | 70 | 79 |
| 9 | 80 | 89 |
| 10 | 90 | 100 |

*Ketty* gives *Eve* a task to generate a report containing three columns: *Name*, *Grade* and *Mark*. *Ketty* doesn't want the NAMES of those students who received a grade lower than *8*. The report must be in descending order by grade -- i.e. higher grades are entered first. If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically. Finally, if the grade is lower than 8, use "NULL" as their name and list them by their grades in descending order. If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in ascending order.

Write a query to help Eve.

**Sample Input**

| **ID** | **Name** | **Marks** |
| --- | --- | --- |
| 1 | Julia | 88 |
| 2 | Samantha | 68 |
| 3 | Maria | 99 |
| 4 | Scarlet | 78 |
| 5 | Ashley | 63 |
| 6 | Jane | 81 |

**Sample Output**

```
Maria 10 99
Jane 9 81
Julia 9 88
Scarlet 8 78
NULL 7 63
NULL 7 68
```

**Note**

Print "NULL"  as the name if the grade is less than 8.

**Explanation**

Consider the following table with the grades assigned to the students:

| **ID** | **Name** | **Marks** | **Grade** |
| --- | --- | --- | --- |
| 1 | Julia | 88 | 9 |
| 2 | Samantha | 68 | 7 |
| 3 | Maria | 99 | 10 |
| 4 | Scarlet | 78 | 8 |
| 5 | Ashley | 63 | 7 |
| 6 | Jane | 81 | 9 |

So, the following students got *8*, *9* or *10* grades:

- *Maria (grade 10)*
- *Jane (grade 9)*
- *Julia (grade 9)*
- *Scarlet (grade 8)*

```sql
select S.NAME, G.GRADE, S.MARKS 
from STUDENTS S 
left join GRADES G
on S.MARKS BETWEEN G.MIN_MARK AND G.MAX_MARK 
where G.GRADE >= 8

union 

select NULL, G.GRADE, S.MARKS  
from STUDENTS S 
left join GRADES G 
on S.MARKS BETWEEN G.MIN_MARK AND G.MAX_MARK
where G.GRADE < 8 
order by G.GRADE desc, S.NAME asc ;
```

**Top Competitors**

Julia just finished conducting a coding contest, and she needs your help assembling the leaderboard! Write a query to print the respective *hacker_id* and *name* of hackers who achieved full scores for *more than one* challenge. Order your output in descending order by the total number of challenges in which the hacker earned a full score. If more than one hacker received full scores in same number of challenges, then sort them by ascending *hacker_id*.

**Input Format**

The following tables contain contest data:

- *Hackers:* The *hacker_id* is the id of the hacker, and *name* is the name of the hacker.

| **Column** | **Type** |
| --- | --- |
| hacker_id | integer |
| name | string |
- *Difficulty:* The *difficult_level* is the level of difficulty of the challenge, and *score* is the score of the challenge for the difficulty level.

| **Column** | **Type** |
| --- | --- |
| difficulty | integer |
| score | integer |
- *Challenges:* The *challenge_id* is the id of the challenge, the *hacker_id* is the id of the hacker who created the challenge, and *difficulty_level* is the level of difficulty of the challenge.

| **Column** | **Type** |
| --- | --- |
| challenge_id | integer |
| hacker_id | integer |
| difficulty_level | integer |
- *Submissions:* The *submission_id* is the id of the submission, *hacker_id* is the id of the hacker who made the submission, *challenge_id* is the id of the challenge that the submission belongs to, and *score* is the score of the submission.

| **Column** | **Type** |
| --- | --- |
| submission_id | integer |
| hacker_id | integer |
| challenge_id | integer |
| score | integer |

**Sample Input**

*Hackers* Table:

| **hacker_id** | **name** |
| --- | --- |
| 5580 | Rose |
| 8439 | AngelA |
| 27250 | Frank |
| 52243 | Patrick |
| 52348 | Lisa |

*Difficulty* Table:

| **difficulty_level** | **score** |
| --- | --- |
| 1 | 20 |
| 2 | 30 |
| 3 | 40 |
| 4 | 60 |
| 5 | 80 |
| 6 | 100 |
| 7 | 120 |

*Challenges* Table:

| **challenge_id** | **hacker_id** | **difficulty_level** |
| --- | --- | --- |
| 4810 | 77726 | 4 |
| 21089 | 27205 | 1 |
| 36566 | 5580 | 7 |
| 66730 | 52243 | 6 |
| 71055 | 52243 | 2 |

*Submissions* Table:

| **submission_id** | **hacker_id** | **challenge_id** | **score** |
| --- | --- | --- | --- |
| 68628 | 77726 | 36568 | 30 |
| 65300 | 77726 | 21089 | 10 |
| 40326 | 52443 | 36568 | 77 |
| 8941 | 27205 | 4810 | 4 |
| 83554 | 77726 | 68730 | 30 |
| 43353 | 52443 | 68730 | 0 |

**Sample Output**

`90411 Joe`

**Explanation**

Hacker *86870* got a score of *30* for challenge *71055* with a difficulty level of *2*, so *86870* earned a full score for this challenge.

Hacker *90411* got a score of *30* for challenge *71055* with a difficulty level of *2*, so *90411* earned a full score for this challenge.

Hacker *90411* got a score of *100* for challenge *66730* with a difficulty level of *6*, so *90411* earned a full score for this challenge.

Only hacker *90411* managed to earn a full score for more than one challenge, so we print the their *hacker_id* and *name* as 2 space-separated values.

```sql
select h.HACKER_ID, h.NAME
from HACKERS h 
left join SUBMISSIONS s 
on s.HACKER_ID = h.HACKER_ID 
left join CHALLENGES c 
on c.CHALLENGE_ID = s.CHALLENGE_ID 
left join DIFFICULTY d 
on d.DIFFICULTY_LEVEL = c.DIFFICULTY_LEVEL
where s.SCORE = d.SCORE
group by h.HACKER_ID,h.NAME 
having count(c.CHALLENGE_ID) > 1 
order by count(c.CHALLENGE_ID) desc, h.HACKER_ID asc
```

## Ollivander's Inventory

Harry Potter and his friends are at Ollivander's with Ron, finally replacing Charlie's old broken wand.

Hermione decides the best way to choose is by determining the minimum number of gold galleons needed to buy each *non-evil* wand of high power and age. Write a query to print the *id*, *age*, *coins_needed*, and *power* of the wands that Ron's interested in, sorted in order of descending *power*. If more than one wand has same power, sort the result in order of descending *age*.

**Input Format**

The following tables contain data on the wands in Ollivander's inventory:

- *Wands:* The *id* is the id of the wand, *code* is the code of the wand, *coins_needed* is the total number of gold galleons needed to buy the wand, and *power* denotes the quality of the wand (the higher the power, the better the wand is).

| **Column** | **Type** |
| --- | --- |
| id | integer |
| code | integer |
| coins_needed | integer |
| power | integer |

*Wands_Property:* The *code* is the code of the wand, *age* is the age of the wand, and *is_evil* denotes whether the wand is good for the dark arts. If the value of *is_evil* is *0*, it means that the wand is not evil. The mapping between *code* and *age* is one-one, meaning that if there are two pairs, (code1, age1) and (code2, age2) , then code1 <> code 2  and age1 <> age2.

| **Column** | **Type** |
| --- | --- |
| code | integer |
| age | integer |
| is_evil | integer |

**Sample Input**

*Wands* Table:

| **id** | **code** | **coins_needed** | **power** |
| --- | --- | --- | --- |
| 1 | 4 | 3688 | 8 |
| 2 | 3 | 9365 | 3 |
| 3 | 3 | 7187 | 10 |
| 4 | 3 | 734 | 8 |
| 5 | 1 | 6020 | 2 |
| 6 | 2 | 6773 | 7 |
| 7 | 3 | 9873 | 9 |

*Wands_Property* Table:

| code | age | is_evil |
| --- | --- | --- |
| 1 | 45 | 0 |
| 2 | 40 | 0 |
| 3 | 4 | 1 |
| 4 | 20 | 0 |
| 5 | 17 | 0 |

**Sample Output**

`9 45 1647 10
12 17 9897 10
1 20 3688 8
15 40 6018 7
19 20 7651 6
11 40 7587 5
10 20 504 5
18 40 3312 3
20 17 5689 3
5 45 6020 2
14 40 5408 1`

**Explanation**

The data for wands of *age 45* (code 1):

| id | age | coins_needed | power |
| --- | --- | --- | --- |
| 5 | 45 | 6020 | 2 |
| 6 | 45 | 1647 | 10 |
- The minimum number of galleons needed for wand ( age = 45, power = 2 ) = 6020
- The minimum number of galleons needed for wand ( age = 45, power = 10) = 1647

The data for wands of *age 40* (code 2):

| **id** | **age** | **coins_needed** | **power** |
| --- | --- | --- | --- |
| 6 | 40 | 6773 | 7 |
| 11 | 40 | 7587 | 5 |
| 15 | 40 | 6018 | 7 |
| 17 | 40 | 8798 | 7 |
| 18 | 40 | 3312 | 3 |
| 14 | 40 | 5408 | 1 |
- The minimum number of galleons needed for ( age = 40, power = 1 ) = 5048
- The minimum number of galleons needed for ( age = 40, power = 3 ) = 3312
- The minimum number of galleons needed for ( age = 40, power = 5 ) = 7587
- The minimum number of galleons needed for ( age = 40, power = 7 ) = 6018

The data for wands of *age 20* (code 4):

| **id** | **age** | **coins_needed** | **power** |
| --- | --- | --- | --- |
| 10 | 20 | 504 | 5 |
| 16 | 20 | 7710 | 5 |
| 19 | 20 | 7651 | 6 |
| 1 | 20 | 3688 | 8 |
- The minimum number of galleons needed for ( age = 20, power = 5 ) = 504
- The minimum number of galleons needed for ( age = 20, power = 6 ) = 7651
- The minimum number of galleons needed for ( age = 20, power = 8 ) = 3688

The data for wands of *age 17* (code 5):

| **id** | **age** | **coins_needed** | **power** |
| --- | --- | --- | --- |
| 20 | 17 | 5689 | 3 |
| 12 | 17 | 9897 | 10 |
- The minimum number of galleons needed for ( age = 17, power = 3 ) = 5689
- The minimum number of galleons needed for ( age = 17, power = 10 ) = 9897

```sql
select ID, AGE, COINS_NEEDED, POWER
from
(
    select a.ID, b.AGE, a.COINS_NEEDED, a.POWER
        , row_number() over(partition by b.AGE, a.POWER order by a.COINS_NEEDED asc ) as rn
    from WANDS a
    join WANDS_PROPERTY b
    on a.CODE = b.CODE
    where IS_EVIL = 0 
) x
where rn = 1
order by POWER desc, AGE desc
```

## Challenges

Julia asked her students to create some coding challenges. Write a query to print the *hacker_id*, *name*, and the total number of challenges created by each student. Sort your results by the total number of challenges in descending order. If more than one student created the same number of challenges, then sort the result by *hacker_id*. If more than one student created the same number of challenges and the count is less than the maximum number of challenges created, then exclude those students from the result.

**Input Format**

The following tables contain challenge data:

- *Hackers:* The *hacker_id* is the id of the hacker, and *name* is the name of the hacker.

| **Column** | **Type** |
| --- | --- |
| hacker_id | integer |
| name | string |

*Challenges:* The *challenge_id* is the id of the challenge, and *hacker_id* is the id of the student who created the challenge.

| **Column** | **Type** |
| --- | --- |
| challenge_id | integer |
| hacker_id | integer |

**Sample Input 0**

*Hackers* Table:

| **hacker_id** | **name** |
| --- | --- |
| 5077 | Rose |
| 21283 | Angela |
| 62743 | Frank |
| 88255 | Patrick |
| 96196 | Lisa |

*Challenges* Table:

| **challenge_id** | **hacker_id** |
| --- | --- |
| 61654 | 5077 |
| 58302 | 21283 |
| 40587 | 88255 |
| 29477 | 5077 |
| 1220 | 21283 |
| 69514 | 21283 |
| 46561 | 62743 |
| 58077 | 62743 |
| 18483 | 88255 |

**Sample Output 0**

```
21283 Angela 6
88255 Patrick 5
96196 Lisa 1

```

**Sample Input 1**

*Hackers* Table:

| **hacker_id** | **name** |
| --- | --- |
| 12299 | Rose |
| 34856 | Angela |
| 79345 | Frank |
| 80491 | Patrick |
| 81041 | Lisa |

*Challenges* Table:

| **challenge_id** | **hacker_id** |
| --- | --- |
| 63963 | 81041 |
| 63117 | 79345 |
| 28225 | 34856 |
| 21989 | 12299 |

**Sample Output 1**

```
12299 Rose 6
34856 Angela 6
79345 Frank 4
80491 Patrick 3
81041 Lisa 1
```

**Explanation**

For *Sample Case 0*, we can get the following details:

| **hacker_id** | **name** | **challenges_created** |
| --- | --- | --- |
| 21283 | Angela | 6 |
| 88255 | Patrick | 5 |
| 5077 | Rose | 4 |
| 62743 | Frank | 4 |
| 96196 | Lisa | 1 |

Students 5077 and  62743 both created 4 challenges, but the maximum number of challenges created is 6 so these students are excluded from the result.

For *Sample Case 1*, we can get the following details:

| **hacker_id** | **name** | **challenges_created** |
| --- | --- | --- |
| 12299 | Rose | 6 |
| 34856 | Angela | 6 |
| 79345 | Frank | 4 |
| 80491 | Patrick | 3 |
| 81041 | Lisa | 1 |

Students 12299 and 34856 both created 6 challenges. Because 6 is the maximum number of challenges created, these students are included in the result.

```sql
with result as
(
    select h.HACKER_ID, h.NAME, count(CHALLENGE_ID) as [Number of challenge]
    from HACKERS h
    left join CHALLENGES c
    on h.HACKER_ID = c.HACKER_ID
    group by h.HACKER_ID, h.NAME
   
)
select a.*
from result a
left join
( 
    select s.HACKER_ID from
    result s
    join
    (
        select [Number of challenge] 
        from result
        group by [Number of challenge] 
        having count(*) > 1
    ) y
    on s.[Number of challenge] = y.[Number of challenge] 
    where s.[Number of challenge] < ( select max([Number of challenge]) from result )   
) b
on a.HACKER_ID = b.HACKER_ID
where b.HACKER_ID is null
order by a.[Number of challenge] desc , a.HACKER_ID asc
```

## **Contest Leaderboard**

You did such a great job helping Julia with her last coding contest challenge that she wants you to work on this one, too!

The total score of a hacker is the sum of their maximum scores for all of the challenges. Write a query to print the *hacker_id*, *name*, and total score of the hackers ordered by the descending score. If more than one hacker achieved the same total score, then sort the result by ascending *hacker_id*. Exclude all hackers with a total score of 0 from your result.

**Input Format**

The following tables contain contest data:

- *Hackers:* The *hacker_id* is the id of the hacker, and *name* is the name of the hacker.

| **Column** | **Type** |
| --- | --- |
| hacker_id | integer |
| name | string |
- *Submissions:* The *submission_id* is the id of the submission, *hacker_id* is the id of the hacker who made the submission, *challenge_id* is the id of the challenge for which the submission belongs to, and *score* is the score of the submission.

| **Column** | **Type** |
| --- | --- |
| submission_id | integer |
| hacker_id | integer |
| challenge_id | integer |
| score | integer |

**Sample Input**

*Hackers* Table:

| **hacker_id** | **name** |
| --- | --- |
| 4071 | Rose |
| 4806 | Angela |
| 26071 | Frank |
| 49438 | Patrick |
| 74842 | Lisa |

*Submissions* Table:

| **submission_id** | **hacker_id** | **challenge_id** | **score** |
| --- | --- | --- | --- |
| 67194 | 74842 | 63132 | 76 |
| 64479 | 74842 | 19797 | 98 |
| 40742 | 26071 | 49593 | 20 |
| 17513 | 4806 | 49593 | 32 |
| 69846 | 80305 | 19797 | 19 |
| 41002 | 26071 | 89343 | 36 |
| 52826 | 49438 | 49593 | 9 |
| 31093 | 26071 | 19797 | 2 |
| 81614 | 84072 | 49593 | 100 |

**Sample Output**

`4071 Rose 191
74842 Lisa 174
84072 Bonnie 100
4806 Angela 89
26071 Frank 85
80305 Kimberly 67
49438 Patrick 43`

**Explanation**

Hacker *4071* submitted solutions for challenges *19797* and *49593*, so the total score .

= 95 + max(43, 69) = 191

Hacker *74842* submitted solutions for challenges *19797* and *63132*, so the total score

= max(98, 5) + 76 = 174

Hacker *84072* submitted solutions for challenges *49593* and *63132*, so the total score .

= 100 + 0 = 100

The total scores for hackers *4806*, *26071*, *80305*, and *49438* can be similarly calculated.

```sql

select HACKER_ID, NAME, coalesce(sum(SCORE),0) as SCORE
from
(
    select h.HACKER_ID, h.NAME, CHALLENGE_ID, max(SCORE) as SCORE
    from HACKERS h
    left join SUBMISSIONS s
    on h.HACKER_ID = s.HACKER_ID
    group by h.HACKER_ID, h.NAME, CHALLENGE_ID
) x
group by HACKER_ID, NAME
having coalesce(sum(SCORE),0) <> 0
order by sum(SCORE) desc, HACKER_ID asc
```

## **Population Census**

Given the **CITY** and **COUNTRY** tables, query the sum of the populations of all cities where the *CONTINENT* is *'Asia'*.

**Note:** *CITY.CountryCode* and *COUNTRY.Code* are matching key columns.

**Input Format**

The **CITY** and **COUNTRY** tables are described as follows:

**CITY**

| **Field** | **Type** |
| --- | --- |
| ID | NUMBER |
| NAME | VARCHAR2(17) |
| COUNTRYCODE | VARCHAR2(3) |
| DISTRICT | VARCHAR2(20) |
| POPULATION | NUMBER |

**COUNTRY**

| **Field** | **Type** |
| --- | --- |
| CODE | VARCHAR2(3) |
| NAME | VARCHAR2(44) |
| CONTINENT | VARCHAR2(13) |
| REGION | VARCHAR2(25) |
| SURFACEAREA | NUMBER |
| INDEPYEAR | VARCHAR2(5) |
| POPULATION | NUMBER |
| LIFEEXPECTANCY | VARCHAR2(4) |
| GNP | NUMBER |
| GNPOLD | VARCHAR2(9) |
| LOCALNAME | VARCHAR2(44) |
| GOVERNMENTFORM | VARCHAR2(44) |
| HEADOFSTATE | VARCHAR2(32) |
| CAPITAL | VARCHAR2(4) |
| CODE2 | VARCHAR2(2) |

```sql
select sum(c.POPULATION) as POPULATION
from CITY c
join COUNTRY cou
on c.COUNTRYCODE = cou.CODE
where cou.CONTINENT = 'Asia'
```