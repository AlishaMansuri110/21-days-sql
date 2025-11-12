Challenge Question: Create a service performance report showing service name, total patients admitted, and a performance category based on the following: 'Excellent' if avg satisfaction >= 85, 'Good' if >= 75, 'Fair' if >= 65, otherwise 'Needs Improvement'. Order by average satisfaction descending.

 SELECT
     service AS service_name,
     COUNT(patient_id) AS total_patients_admitted,
     AVG(satisfaction) AS avg_satisfaction,
     CASE
         WHEN AVG(satisfaction) >= 85 THEN 'Excellent'
         WHEN AVG(satisfaction) >= 75 THEN 'Good'
         WHEN AVG(satisfaction) >= 65 THEN 'Fair'
         ELSE 'Needs Improvement'
     END AS performance_category
 FROM
     patients
 GROUP BY
     service
 ORDER BY
     avg_satisfaction DESC;
+------------------+-------------------------+------------------+----------------------+
| service_name     | total_patients_admitted | avg_satisfaction | performance_category |
+------------------+-------------------------+------------------+----------------------+
| surgery          |                     254 |          80.3150 | Good                 |
| ICU              |                     241 |          79.9212 | Good                 |
| emergency        |                     263 |          79.5475 | Good                 |
| general_medicine |                     242 |          78.5744 | Good                 |
+------------------+-------------------------+------------------+----------------------+
