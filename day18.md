Challenge Question: Create a comprehensive personnel and patient list showing: identifier (patient_id or staff_id), full name, type ('Patient' or 'Staff'), and associated service. Include only those in 'surgery' or 'emergency' services. Order by type, then service, then name.

 SELECT
     p.patient_id AS identifier,
     p.name AS full_name,
     'Patient' AS type,
     p.service
 FROM patients p
 WHERE p.service IN ('surgery', 'emergency')

 UNION ALL
 SELECT
     s.staff_id AS identifier,
     s.staff_name AS full_name,
     'Staff' AS type,
     s.service
 FROM staff s
 WHERE s.service IN ('surgery', 'emergency')

 ORDER BY
     type,        -- Patient first, Staff second (alphabetical)
     service,
     full_name;
+--------------+------------------------+---------+-----------+
| identifier   | full_name              | type    | service   |
+--------------+------------------------+---------+-----------+
| PAT-0f8876bf | Aaron Parker           | Patient | emergency |
| PAT-0b1acdd9 | Aaron Schroeder        | Patient | emergency |
| PAT-09edc78a | Adam Jackson           | Patient | emergency |
| PAT-0c7c720b | Aimee Turner           | Patient | emergency |
| PAT-8b68dd5f | Alex Hernandez         | Patient | emergency |
| PAT-d8b5f75c | Alexander Brown        | Patient | emergency |
| PAT-06a8ea25 | Alexander Gomez        | Patient | emergency |
| PAT-69b43c9f | Allen Rosales          | Patient | emergency |
| PAT-de5fbcb3 | Allison Smith          | Patient | emergency |
| PAT-73381b12 | Alyssa Long            | Patient | emergency |
| PAT-8bfedd28 | Amanda Harvey          | Patient | emergency |
| PAT-a8b5abcb | Amanda Hill            | Patient | emergency |
| PAT-3ca8e0e7 | Amanda Terrell         | Patient | emergency |
| PAT-a01bf534 | Amber Vang             | Patient | emergency |
| PAT-85f8f76d | Amy Garcia             | Patient | emergency |
| PAT-686bfae7 | Amy Martinez           | Patient | emergency |
| PAT-532a2fa4 | Andrew Avila           | Patient | emergency |
| PAT-7472f19a | Andrew Harper          | Patient | emergency |
| PAT-6813152c | Angel May              | Patient | emergency |
| PAT-9da5da0c | Angela Vaughn          | Patient | emergency |
| PAT-6527c233 | Angelica Keith         | Patient | emergency |
| PAT-55a00f9c | Anthony Harmon         | Patient | emergency |
| PAT-c350b838 | Anthony Romero         | Patient | emergency |
| PAT-d9ea1df9 | Ariel Sandoval         | Patient | emergency |
| PAT-3b87b085 | Ashley Hicks           | Patient | emergency |
| PAT-ba4f5c45 | Bonnie Valencia        | Patient | emergency |
| PAT-93759aa2 | Brenda Wright          | Patient | emergency |
| PAT-bdf625cd | Brent Hernandez        | Patient | emergency |
| PAT-a34a64e2 | Brian Hunt             | Patient | emergency |
| PAT-e3ea41ad | Brian Lee              | Patient | emergency |
| PAT-b0c6f49d | Brittney Martin        | Patient | emergency |
| PAT-0812aab1 | Bryan Gomez            | Patient | emergency |
| PAT-8f6d5fa4 | Bryan Herrera          | Patient | emergency |
| PAT-5ee95f95 | Bryan Parker           | Patient | emergency |
| PAT-af65396f | Cameron Fisher         | Patient | emergency |
| PAT-30c0449b | Carlos Ryan            | Patient | emergency |
| PAT-41173daa | Carmen Preston         | Patient | emergency |
| PAT-3791a5a1 | Carolyn Miller         | Patient | emergency |
| PAT-5228b8e2 | Casey Chase            | Patient | emergency |
| PAT-6634a1f1 | Casey Perez            | Patient | emergency |
| PAT-137f292a | Cassie Brock           | Patient | emergency |
| PAT-3ef63f63 | Catherine Davis        | Patient | emergency |
| PAT-ca8b7c4f | Chelsea Nguyen         | Patient | emergency |
| PAT-e9c66e7a | Christian Vaughn       | Patient | emergency |
| PAT-b0296eda | Cody Reid              | Patient | emergency |
| PAT-c4848253 | Connie Brown           | Patient | emergency |
| PAT-4fbfd721 | Courtney Chapman       | Patient | emergency |
| PAT-cd63d937 | Courtney Gonzalez      | Patient | emergency |
| PAT-7d80fdf8 | Courtney Smith         | Patient | emergency |
| PAT-3dda2bb5 | Crystal Johnson        | Patient | emergency |
| PAT-366e54bd | Crystal Mann           | Patient | emergency |
| PAT-3a12adef | Dana Chapman           | Patient | emergency |
| PAT-5d40ae5a | Daniel Salinas         | Patient | emergency |
| PAT-268d03da | Danielle Watson        | Patient | emergency |
| PAT-af3e8f0a | David Grant            | Patient | emergency |
| PAT-27519dd1 | David Harris           | Patient | emergency |
| PAT-1def560a | David Lopez            | Patient | emergency |
| PAT-f801ed9b | Dawn Mullins           | Patient | emergency |      
