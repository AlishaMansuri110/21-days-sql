Question: Find all unique combinations of service and event type from the services_weekly table where events are not null or none, along with the count of occurrences for each combination. Order by count descending.

 SELECT
     service,
     event,
     COUNT(*) AS occurrence_count
 FROM
     services_weekly
 WHERE
     event IS NOT NULL
     AND event <> 'none'
 GROUP BY
     service, event
 ORDER BY
     occurrence_count DESC;
+------------------+----------+------------------+
| service          | event    | occurrence_count |
+------------------+----------+------------------+
| general_medicine | flu      |                6 |
| ICU              | flu      |                5 |
| emergency        | flu      |                5 |
| surgery          | donation |                5 |
| surgery          | strike   |                4 |
| emergency        | donation |                4 |
| emergency        | strike   |                4 |
| surgery          | flu      |                3 |
| general_medicine | donation |                3 |
| general_medicine | strike   |                3 |
| ICU              | donation |                2 |
+------------------+----------+------------------+
