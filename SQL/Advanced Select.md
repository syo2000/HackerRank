## **The PADS**

Generate the following two result sets:

1. Query an *alphabetically ordered* list of all names in **OCCUPATIONS**, immediately followed by the first letter of each profession as a parenthetical (i.e.: enclosed in parentheses). For example: `AnActorName(A)`, `ADoctorName(D)`, `AProfessorName(P)`, and `ASingerName(S)`.
2. Query the number of ocurrences of each occupation in **OCCUPATIONS**. Sort the occurrences in *ascending order*, and output them in the following format:
    
    ```
    There are a total of [occupation_count] [occupation]s.
    ```
    
    where `[occupation_count]` is the number of occurrences of an occupation in **OCCUPATIONS** and `[occupation]` is the *lowercase* occupation name. If more than one *Occupation* has the same `[occupation_count]`, they should be ordered alphabetically.
    

**Note:** There will be at least two entries in the table for each type of occupation.

**Input Format**

The **OCCUPATIONS** table is described as follows:

| **Column** | **Type** |
| --- | --- |
| Name | String |
| Occupation | String |

*Occupation* will only contain one of the following values: **Doctor**, **Professor**, **Singer** or **Actor**.

**Sample Input**

An **OCCUPATIONS** table that contains the following records:

| **Name** | **Occupation** |
| --- | --- |
| Samantha | Doctor |
| Julia | Actor |
| Maria | Actor |
| Meera | Singer |
| Ashely | Professor |
| Ketty | Professor |
| Christeen | Professor |
| Jane | Actor |
| Jenny | Doctor |
| Priya | Singer |

**Sample Output**

`Ashely(P)
Christeen(P)
Jane(A)
Jenny(D)
Julia(A)
Ketty(P)
Maria(A)
Meera(S)
Priya(S)
Samantha(D)
There are a total of 2 doctors.
There are a total of 2 singers.
There are a total of 3 actors.
There are a total of 3 professors.`

**Explanation**

The results of the first query are formatted to the problem description's specifications.The results of the second query are ascendingly ordered first by number of names corresponding to each profession ( **2 ≤ 2 ≤ 3 ≤ 3**), and then alphabetically by profession (***doctor≤ singer, and actor ≤ professor*** ).

```sql
select concat(NAME, "(", left(OCCUPATION,1), ")") 
from OCCUPATIONS 
order by NAME asc;

select concat("There are a total of", " ", count(OCCUPATION), " ", lower(OCCUPATION), "s.")
from OCCUPATIONS
group by OCCUPATION
order by count(OCCUPATION) asc,OCCUPATION;
```

## **Occupations**

[Pivot](https://en.wikipedia.org/wiki/Pivot_table) the *Occupation* column in **OCCUPATIONS** so that each *Name* is sorted alphabetically and displayed underneath its corresponding *Occupation*. The output column headers should be *Doctor*, *Professor*, *Singer*, and *Actor*, respectively.

**Note:** Print **NULL** when there are no more names corresponding to an occupation.

**Input Format**

The **OCCUPATIONS** table is described as follows:

| Column | Type |
| --- | --- |
| Name | string |
| Occupation | String |

*Occupation* will only contain one of the following values: **Doctor**, **Professor**, **Singer** or **Actor**.

**Sample Input**

| **Name** | **Occupation** |
| --- | --- |
| Samantha | Doctor |
| Julia | Actor |
| Maria | Actor |
| Meera | Singer |
| Ashely | Professor |
| Ketty | Professor |
| Christeen | Professor |
| Jane | Actor |
| Jenny | Doctor |
| Priya | Singer |

**Sample Output**

```
Jenny    Ashley     Meera  Jane
Samantha Christeen  Priya  Julia
NULL     Ketty      NULL   Maria
```

**Explanation**

The first column is an alphabetically ordered list of Doctor names.

The second column is an alphabetically ordered list of Professor names.

The third column is an alphabetically ordered list of Singer names.

The fourth column is an alphabetically ordered list of Actor names.

The empty cell data for columns with less than the maximum number of names per occupation (in this case, the Professor and Actor columns) are filled with **NULL** values.

```sql

select max(case when OCCUPATION = 'Doctor' then NAME end) as Doctor
, max(case when OCCUPATION = 'Professor' then NAME end) as Professor
, max(case when OCCUPATION = 'Singer' then NAME end) as Singer
, max(case when OCCUPATION = 'Actor' then NAME end) as Actor 
from 
(
	select NAME, OCCUPATION, row_number() over (partition by OCCUPATION order by NAME) AS RowNumber 
    from OCCUPATIONS 
) x
group by RowNumber order by RowNumber
```

## **Binary Tree Nodes**

You are given a table, *BST*, containing two columns: *N* and *P,* where *N* represents the value of a node in *Binary Tree*, and *P* is the parent of *N*.

| Column | Type |
| --- | --- |
| N | Integer |
| P | Integer |

Write a query to find the node type of *Binary Tree* ordered by the value of the node. Output one of the following for each node:

- *Root*: If node is root node.
- *Leaf*: If node is leaf node.
- *Inner*: If node is neither root nor leaf node.

**Sample Input**

| N | P |
| --- | --- |
| 1 | 2 |
| 3 | 2 |
| 6 | 8 |
| 9 | 8 |
| 2 | 5 |
| 8 | 5 |
| 5 | null |

```
1 Leaf
2 Inner
3 Leaf
5 Root
6 Leaf
8 Inner
9 Leaf
```

**Sample Output**

```sql
select N
, case when P is null then 'Root' 
  when N not in (select distinct(P) from bst where P is not null) then 'Leaf' 
  else 'Inner' 
  end 
from BST order by n
```

## **New Companies**

Amber's conglomerate corporation just acquired some new companies. Each of the companies follows this hierarchy:

Founder ⇒ Lead manager ⇒ Senior manager ⇒ Manager ⇒ Employee

Given the table schemas below, write a query to print the *company_code*, *founder* name, total number of *lead* managers, total number of *senior* managers, total number of *managers*, and total number of *employees*. Order your output by ascending *company_code*.

**Note:**

- The tables may contain duplicate records.
- The *company_code* is string, so the sorting should not be **numeric**. For example, if the *company_codes* are *C_1*, *C_2*, and *C_10*, then the ascending *company_codes* will be *C_1*, *C_10*, and *C_2*.

**Input Format**

The following tables contain company data:

- *Company:* The *company_code* is the code of the company and *founder* is the founder of the company.

| Column | Type |
| --- | --- |
| Company_code | string |
| founder | string |
- *Lead_Manager:* The *lead_manager_code* is the code of the lead manager, and the *company_code* is the code of the working company.

| Column | Type |
| --- | --- |
| lead_manager_code | string |
| Company_code | string |
- *Senior_Manager:* The *senior_manager_code* is the code of the senior manager, the *lead_manager_code* is the code of its lead manager, and the *company_code* is the code of the working company.

| Column | Type |
| --- | --- |
| lead_manager_code | string |
| Company_code | string |
| senior_manager_code | string |
- *Manager:* The *manager_code* is the code of the manager, the *senior_manager_code* is the code of its senior manager, the *lead_manager_code* is the code of its lead manager, and the *company_code* is the code of the working company.

| Column | Type |
| --- | --- |
| lead_manager_code | string |
| Company_code | string |
| senior_manager_code | string |
| manager_code | string |
- *Employee:* The *employee_code* is the code of the employee, the *manager_code* is the code of its manager, the *senior_manager_code* is the code of its senior manager, the *lead_manager_code* is the code of its lead manager, and the *company_code* is the code of the working company.

| Column | Type |
| --- | --- |
| lead_manager_code | string |
| Company_code | string |
| senior_manager_code | string |
| manager_code | string |
| employee_code |  |

**Sample Input**

*Company* Table:

| Company_code | founder |
| --- | --- |
| C1 | Monika |
| C2 | Samantha |

*Lead_Manager* Table:

| lead_manager_code | Company_code |
| --- | --- |
| LM1 | C1 |
| LM2 | C2 |

*Senior_Manager* Table:

| senior_manager_code | lead_manager_code | Company_code |
| --- | --- | --- |
| SM1 | LM1 | C1 |
| SM2 | LM1 | C1 |
| SM3 | LM2 | C2 |

*Manager* Table:

| manager_code | senior_manager_code | lead_manager_code | Company_code |
| --- | --- | --- | --- |
| M1 | SM1 | LM1 | C1 |
| M2 | SM3 | LM2 | C2 |
| M3 | SM3 | LM2 | C2 |

*Employee* Table:

| employee_code | manager_code | senior_manager_code | lead_manager_code | Company_code |
| --- | --- | --- | --- | --- |
| E1 | M1 | SM1 | LM1 | C1 |
| E2 | M1 | SM1 | LM1 | C1 |
| E3 | M2 | SM3 | LM2 | C2 |
| E4 | M3 | SM3 | LM2 | C2 |

**Sample Output**

```
C1 Monika 1 2 1 2
C2 Samantha 1 1 2 2
```

**Explanation**

In company *C1*, the only lead manager is *LM1*. There are two senior managers, *SM1* and *SM2*, under *LM1*. There is one manager, *M1*, under senior manager *SM1*. There are two employees, *E1* and *E2*, under manager *M1*.

In company *C2*, the only lead manager is *LM2*. There is one senior manager, *SM3*, under *LM2*. There are two managers, *M2* and *M3*, under senior manager *SM3*. There is one employee, *E3*, under manager *M2*, and another employee, *E4*, under manager, *M3*.

```sql
select
e.COMPANY_CODE,
c.FOUNDER,
count(distinct(e.LEAD_MANAGER_CODE)),
count(distinct(e.SENIOR_MANAGER_CODE)),
count(distinct(e.MANAGER_CODE)),
count(distinct(e.EMPLOYEE_CODE))
from COMPANY c
join EMPLOYEE e ON c.COMPANY_CODE = e.COMPANY_CODE
group by e.COMPANY_CODE, c.FOUNDER
order by e.COMPANY_CODE;
```

## **Type of Triangle**

Write a query identifying the *type* of each record in the **TRIANGLES** table using its three side lengths. Output one of the following statements for each record in the table:

- **Equilateral**: It's a triangle with 3 sides of equal length.
- **Isosceles**: It's a triangle with 2 sides of equal length.
- **Scalene**: It's a triangle with 3 sides of differing lengths.
- **Not A Triangle**: The given values of *A*, *B*, and *C* don't form a triangle.

**Input Format**

The **TRIANGLES** table is described as follows:                           

| Column | Type |
| --- | --- |
| A | Integer |
| B | Integer |
| C | Integer |

Each row in the table denotes the lengths of each of a triangle's three sides.

**Sample Input**

| A | B | C |
| --- | --- | --- |
| 20 | 20 | 23 |
| 20 | 20 | 20 |
| 20 | 21 | 22 |
| 13 | 14 | 30 |

**Sample Output**

`Isosceles
Equilateral
Scalene
Not A Triangle`

**Explanation**

Values in the tuple (20,20,23) form an Isosceles triangle, because A ||| B.                                                                                                                                                                                                                                                      

Values in the tuple (20,20,20) form an Equilateral triangle, because A|||B|||C . Values in the tuple (20,21,22) form a Scalene triangle, because A <> B <> C

Values in the tuple (13,14,30) cannot form a triangle because the combined value of sides A and B is not larger than that of side C .

```sql
select
case when A + B <= C OR A + C <= B OR B + C <= A then "Not A Triangle"
     when A = B AND B=C AND C=A then "Equilateral"
     when A = B OR B = C OR C = A then "Isosceles"
     else "Scalene"
     end
from TRIANGLES
```
