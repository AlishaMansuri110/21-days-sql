Challenge Question: Create a comprehensive report showing patient_id, patient name, age, service, and the total number of staff members available in their service. Only include patients from services that have more than 5 staff members. Order by number of staff descending, then by patient name. 

SELECT
     p.patient_id,
     p.name,
     p.age,
     p.service,
     COUNT(s.staff_id) AS total_staff
 FROM patients p
 JOIN staff s
     ON p.service = s.service
 GROUP BY
     p.patient_id,
     p.name,
     p.age,
     p.service
 HAVING
     COUNT(s.staff_id) > 5
 ORDER BY
     total_staff DESC,
     p.name ASC;
+--------------+-------------------------+------+------------------+-------------+
| patient_id   | name                    | age  | service          | total_staff |
+--------------+-------------------------+------+------------------+-------------+
| PAT-1cc7c1b5 | Adam Vaughan            |    2 | ICU              |          48 |
| PAT-bf1b13b6 | Alejandro Deleon        |   14 | ICU              |          48 |
| PAT-d78122d6 | Alexa Buck              |   73 | ICU              |          48 |
| PAT-f316dd98 | Alexander Collins       |   17 | ICU              |          48 |
| PAT-615c986b | Alexandra Dominguez     |   80 | ICU              |          48 |
| PAT-ddd92df7 | Alison Brown            |   50 | ICU              |          48 |
| PAT-c012faab | Allison Hickman         |    6 | ICU              |          48 |
| PAT-d4fc50d8 | Allison Spencer         |   84 | ICU              |          48 |
| PAT-4275a12b | Alyssa Day              |   69 | ICU              |          48 |
| PAT-c1717ebc | Alyssa Haynes           |   82 | ICU              |          48 |
| PAT-12523d2c | Amanda Sullivan MD      |   65 | ICU              |          48 |
| PAT-91fbb614 | Amy Kelley              |   79 | ICU              |          48 |
| PAT-20043fb2 | Amy Miller              |   30 | ICU              |          48 |
| PAT-994866af | Andrea Hubbard          |   41 | ICU              |          48 |
| PAT-333f8368 | Andrew Reynolds         |   23 | ICU              |          48 |
