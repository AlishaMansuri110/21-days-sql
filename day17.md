Challenge Question: Create a report showing each service with: service name, total patients admitted, the difference between their total admissions and the average admissions across all services, and a rank indicator ('Above Average', 'Average', 'Below Average'). Order by total patients admitted descending.

SELECT 
    sw.service AS service_name,
    SUM(sw.patients_admitted) AS total_admitted,
    SUM(sw.patients_admitted) 
        - (SELECT AVG(sa.total_admitted)
           FROM (
                SELECT SUM(patients_admitted) AS total_admitted
                FROM services_weekly
                GROUP BY service
           ) sa
          ) AS admission_difference,
    CASE 
        WHEN SUM(sw.patients_admitted) > (
                SELECT AVG(sa.total_admitted)
                FROM (
                    SELECT SUM(patients_admitted) AS total_admitted
                    FROM services_weekly
                    GROUP BY service
                ) sa
             ) THEN 'Above Average'
        WHEN SUM(sw.patients_admitted) = (
                SELECT AVG(sa.total_admitted)
                FROM (
                    SELECT SUM(patients_admitted) AS total_admitted
                    FROM services_weekly
                    GROUP BY service
                ) sa
             ) THEN 'Average'
        ELSE 'Below Average'
    END AS rank_indicator
FROM services_weekly sw
GROUP BY sw.service
ORDER BY total_admitted DESC;
+------------------+----------------+----------------------+----------------+
| service_name     | total_admitted | admission_difference | rank_indicator |
+------------------+----------------+----------------------+----------------+
| general_medicine |           2332 |             869.2500 | Above Average  |
| surgery          |           1686 |             223.2500 | Above Average  |
| emergency        |           1185 |            -277.7500 | Below Average  |
| ICU              |            648 |            -814.7500 | Below Average  |
+------------------+----------------+----------------------+----------------+
