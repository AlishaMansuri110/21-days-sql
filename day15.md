Challenge Question: Create a comprehensive service analysis report for week 20 showing: service name, total patients admitted that week, total patients refused, average patient satisfaction, count of staff assigned to service, and count of staff present that week. Order by patients admitted descending

SELECT
     sw.service AS service_name,
     sw.patients_admitted,
     sw.patients_refused,
     sw.patient_satisfaction AS avg_satisfaction,
     COUNT(DISTINCT st.staff_id) AS staff_assigned,
     SUM(CASE WHEN ss.present = 1 THEN 1 ELSE 0 END) AS staff_present
 FROM services_weekly sw
 LEFT JOIN staff st
     ON sw.service = st.service
 LEFT JOIN staff_schedule ss
     ON sw.service = ss.service
     AND ss.week = 20
 WHERE
     sw.week = 20
 GROUP BY
     sw.service,
     sw.patients_admitted,
     sw.patients_refused,
     sw.patient_satisfaction
 ORDER BY
     sw.patients_admitted DESC;
+------------------+-------------------+------------------+------------------+----------------+---------------+
| service_name     | patients_admitted | patients_refused | avg_satisfaction | staff_assigned | staff_present |
+------------------+-------------------+------------------+------------------+----------------+---------------+
| general_medicine |                40 |               30 |               64 |             27 |           648 |
| surgery          |                31 |                8 |               99 |             22 |           506 |
| emergency        |                21 |               24 |               93 |             29 |          1073 |
| ICU              |                10 |                4 |               85 |             48 |          1488 |
+------------------+-------------------+------------------+------------------+----------------+---------------+
