Challenge Question: Create a staff utilisation report showing all staff members (staff_id, staff_name, role, service) and the count of weeks they were present (from staff_schedule). Include staff members even if they have no schedule records. Order by weeks present descending.
 
 SELECT
     s.staff_id,
     s.staff_name,
     s.role,
     s.service,
     COUNT(ss.week) AS weeks_present
 FROM staff s
 LEFT JOIN staff_schedule ss
     ON s.staff_id = ss.staff_id
 GROUP BY
     s.staff_id,
     s.staff_name,
     s.role,
     s.service
 ORDER BY
     weeks_present DESC;
+--------------+----------------------+-------------------+------------------+---------------+
| staff_id     | staff_name           | role              | service          | weeks_present |
+--------------+----------------------+-------------------+------------------+---------------+
| STF-5ca26577 | Allison Hill         | doctor            | emergency        |            52 |
| STF-02ae59ca | Noah Rhodes          | doctor            | emergency        |            52 |
| STF-d8006e7c | Angie Henderson      | doctor            | emergency        |            52 |
| STF-212d8b31 | Daniel Wagner        | doctor            | emergency        |            52 |
| STF-107a58e4 | Cristian Santos      | doctor            | emergency        |            52 |
| STF-ca932dea | Connie Lawrence      | nurse             | emergency        |            52 |
| STF-39082289 | Abigail Shaffer      | nurse             | emergency        |            52 |
| STF-702887af | Gina Moore           | nurse             | emergency        |            52 |
| STF-249f63bb | Gabrielle Davis      | nurse             | emergency        |            52 |
| STF-094f410b | Ryan Munoz           | nurse             | emergency        |            52 |
| STF-d7ab0c23 | Monica Herrera       | nurse             | emergency        |            52 |
| STF-e51bdcb4 | Jamie Arnold         | nurse             | emergency        |            52 |
| STF-8cabb71f | Lisa Hensley         | nurse             | emergency        |            52 |
| STF-34c0563d | Michele Williams     | nurse             | emergency        |            52 |
| STF-130577e6 | Dylan Miller         | nurse             | emergency        |            52 |
| STF-768c9d6f | Brian Ramirez        | nurse             | emergency        |            52 |
| STF-abec8336 | Holly Wood           | nurse             | emergency        |            52 |        
