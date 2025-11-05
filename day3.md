Challenge Question: Retrieve the top 5 weeks with the highest patient refusals across all services, showing week, service, patients_refused, and patients_request. Sort by patients_refused in descending order.

SELECT
     week,
     service,
     patients_refused,
      patients_request
 FROM
     services_weekly
  ORDER BY
     patients_refused DESC
  LIMIT 5;
+------+------------------+------------------+------------------+
| week | service          | patients_refused | patients_request |
+------+------------------+------------------+------------------+
|    5 | emergency        |              363 |              388 |
|   12 | emergency        |              319 |              347 |
|    8 | emergency        |              214 |              240 |
|   51 | general_medicine |              211 |              285 |
|    1 | general_medicine |              164 |              201 |
+------+------------------+------------------+------------------+
