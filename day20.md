Challenge Question: Create a trend analysis showing for each service and week: week number, patients_admitted, running total of patients admitted (cumulative), 3-week moving average of patient satisfaction (current week and 2 prior weeks), and the difference between current week admissions and the service average. Filter for weeks 10-20 only.

SELECT
     sw.service,
     sw.week,
     sw.patients_admitted,
     SUM(sw.patients_admitted) OVER (
         PARTITION BY sw.service
         ORDER BY sw.week
         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
     ) AS cumulative_admissions,

         AVG(sw.patient_satisfaction) OVER (
         PARTITION BY sw.service
         ORDER BY sw.week
         ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
     ) AS moving_avg_satisfaction,

     sw.patients_admitted -
         (SELECT AVG(patients_admitted)
          FROM services_weekly
          WHERE service = sw.service) AS difference_from_avg

 FROM services_weekly sw
 WHERE sw.week BETWEEN 10 AND 20
 ORDER BY sw.service, sw.week;
+------------------+------+-------------------+-----------------------+-------------------------+---------------------+
| service          | week | patients_admitted | cumulative_admissions | moving_avg_satisfaction | difference_from_avg |
+------------------+------+-------------------+-----------------------+-------------------------+---------------------+
| emergency        |   10 |                17 |                    17 |                 81.0000 |             -5.7885 |
| emergency        |   11 |                16 |                    33 |                 89.5000 |             -6.7885 |
| emergency        |   12 |                28 |                    61 |                 82.3333 |              5.2115 |
| emergency        |   13 |                23 |                    84 |                 81.0000 |              0.2115 |
| emergency        |   14 |                15 |                    99 |                 72.0000 |             -7.7885 |
| emergency        |   15 |                17 |                   116 |                 80.6667 |             -5.7885 |
| emergency        |   16 |                23 |                   139 |                 75.6667 |              0.2115 |
| emergency        |   17 |                21 |                   160 |                 80.0000 |             -1.7885 |
| emergency        |   18 |                18 |                   178 |                 75.6667 |             -4.7885 |
| emergency        |   19 |                16 |                   194 |                 88.0000 |             -6.7885 |
| emergency        |   20 |                21 |                   215 |                 91.0000 |             -1.7885 |
| general_medicine |   10 |                35 |                    35 |                 95.0000 |             -9.8462 |
| general_medicine |   11 |                35 |                    70 |                 85.0000 |             -9.8462 |
| general_medicine |   12 |                33 |                   103 |                 80.0000 |            -11.8462 |
| general_medicine |   13 |                33 |                   136 |                 72.6667 |            -11.8462 |
| general_medicine |   14 |                47 |                   183 |                 76.3333 |              2.1538 |
| general_medicine |   15 |                53 |                   236 |                 83.0000 |              8.1538 |
| general_medicine |   16 |                39 |                   275 |                 87.0000 |             -5.8462 |
| general_medicine |   17 |                33 |                   308 |                 83.3333 |            -11.8462 |
| general_medicine |   18 |                25 |                   333 |                 85.3333 |            -19.8462 |
| general_medicine |   19 |                43 |                   376 |                 85.3333 |             -1.8462 |
| general_medicine |   20 |                40 |                   416 |                 81.6667 |             -4.8462 |
| ICU              |   10 |                 8 |                     8 |                 91.0000 |             -4.4615 |
| ICU              |   11 |                14 |                    22 |                 76.0000 |              1.5385 |
| ICU              |   12 |                13 |                    35 |                 77.0000 |              0.5385 |
| ICU              |   13 |                12 |                    47 |                 68.6667 |             -0.4615 |
| ICU              |   14 |                10 |                    57 |                 79.0000 |             -2.4615 |
| ICU              |   15 |                11 |                    68 |                 83.3333 |             -1.4615 |
| ICU              |   16 |                15 |                    83 |                 94.0000 |              2.5385 |
| ICU              |   17 |                14 |                    97 |                 92.3333 |              1.5385 |
| ICU              |   18 |                 9 |                   106 |                 82.6667 |             -3.4615 |
| ICU              |   19 |                14 |                   120 |                 72.6667 |              1.5385 |
| ICU              |   20 |                10 |                   130 |                 72.0000 |             -2.4615 |
| surgery          |   10 |                38 |                    38 |                 68.0000 |              5.5769 |
| surgery          |   11 |                48 |                    86 |                 75.0000 |             15.5769 |
| surgery          |   12 |                24 |                   110 |                 76.3333 |             -8.4231 |
| surgery          |   13 |                41 |                   151 |                 78.6667 |              8.5769 |
| surgery          |   14 |                33 |                   184 |                 83.6667 |              0.5769 |
| surgery          |   15 |                31 |                   215 |                 79.0000 |             -1.4231 |
| surgery          |   16 |                28 |                   243 |                 81.3333 |             -4.4231 |
| surgery          |   17 |                23 |                   266 |                 80.6667 |             -9.4231 |
| surgery          |   18 |                36 |                   302 |                 86.0000 |              3.5769 |
| surgery          |   19 |                42 |                   344 |                 84.6667 |              9.5769 |
| surgery          |   20 |                31 |                   375 |                 86.0000 |             -1.4231 |
+------------------+------+-------------------+-----------------------+-------------------------+---------------------+
