Challenge Question: Analyze the event impact by comparing weeks with events vs weeks without events. Show: event status ('With Event' or 'No Event'), count of weeks, average patient satisfaction, and average staff morale. Order by average patient satisfaction descending.

SELECT
     CASE
     WHEN event IS NOT NULL AND event <> '' AND event <> 'none'
             THEN 'With Event'
         ELSE 'No Event'
     END AS event_status,

     COUNT(*) AS week_count,
     AVG(patient_satisfaction) AS avg_patient_satisfaction,
     AVG(staff_morale) AS avg_staff_morale
 FROM services_weekly

 GROUP BY event_status
 ORDER BY avg_patient_satisfaction DESC;
+--------------+------------+--------------------------+------------------+
| event_status | week_count | avg_patient_satisfaction | avg_staff_morale |
+--------------+------------+--------------------------+------------------+
| With Event   |         44 |                  81.0227 |          70.4091 |
| No Event     |        164 |                  79.7256 |          73.1463 |
+--------------+------------+--------------------------+------------------+
