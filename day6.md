Question: For each hospital service, calculate the total number of patients admitted, total patients refused, and the admission rate (percentage of requests that were admitted). Order by admission rate descending.

SELECT
     service,
     SUM(patients_admitted) AS total_admitted,
     SUM(patients_refused) AS total_refused,
     ROUND(
         (SUM(patients_admitted) * 100.0 / (SUM(patients_admitted) + SUM(patients_refused))),
         2
     ) AS admission_rate
 FROM
     services_weekly
 GROUP BY
     service
 ORDER BY
     admission_rate DESC;
+------------------+----------------+---------------+----------------+
| service          | total_admitted | total_refused | admission_rate |
+------------------+----------------+---------------+----------------+
| ICU              |            648 |           141 |          82.13 |
| surgery          |           1686 |           555 |          75.23 |
| general_medicine |           2332 |          1938 |          54.61 |
| emergency        |           1185 |          5008 |          19.13 |
+------------------+----------------+---------------+----------------+
