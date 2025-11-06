Challenge Question: Find the 3rd to 7th highest patient satisfaction scores from the patients table, showing patient_id, name, service, and satisfaction. Display only these 5 records.

SELECT patient_id, name, service, satisfaction
   FROM patients
   ORDER BY satisfaction DESC
   LIMIT 5 OFFSET 2;
+--------------+------------------+-----------+--------------+
| patient_id   | name             | service   | satisfaction |
+--------------+------------------+-----------+--------------+
| PAT-1112a754 | Ashley Smith     | surgery   |           99 |
| PAT-8bfedd28 | Amanda Harvey    | emergency |           99 |
| PAT-84559395 | Jamie Smith      | surgery   |           99 |
| PAT-87fa07bd | Martha Smith     | emergency |           99 |
| PAT-2b471903 | Tiffany Townsend | surgery   |           99 |
+--------------+------------------+-----------+--------------+
