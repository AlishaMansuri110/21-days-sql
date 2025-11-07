Challenge Question: Calculate the total number of patients admitted, total patients refused, and the average patient satisfaction across all services and weeks. Round the average satisfaction to 2 decimal places.

SELECT
     SUM(patients_admitted) AS total_patients_admitted,
     SUM(patients_refused) AS total_patients_refused,
     ROUND (AVG(patient_satisfaction), 2) AS average_satisfaction
FROM services_weekly;
+-------------------------+------------------------+----------------------+
| total_patients_admitted | total_patients_refused | average_satisfaction |
+-------------------------+------------------------+----------------------+
|                    5851 |                   7642 |                80.00 |
+-------------------------+------------------------+----------------------+
