Challenge Question: Calculate the average length of stay (in days) for each service, showing only services where the average stay is more than 7 days. Also show the count of patients and order by average stay descending.

 SELECT
     service,
     COUNT(*) AS patient_count,
     AVG(DATEDIFF(departure_date, arrival_date)) AS avg_stay
 FROM
     patients
 GROUP BY
     service
 HAVING
     avg_stay > 7
 ORDER BY
     avg_stay DESC;
+-----------+---------------+----------+
| service   | patient_count | avg_stay |
+-----------+---------------+----------+
| surgery   |           254 |   7.8661 |
| ICU       |           241 |   7.6058 |
| emergency |           263 |   7.1597 |
+-----------+---------------+----------+
