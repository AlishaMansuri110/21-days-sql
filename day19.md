Challenge Question: For each service, rank the weeks by patient satisfaction score (highest first). Show service, week, patient_satisfaction, patients_admitted, and the rank. Include only the top 3 weeks per service.

SELECT *
FROM (
    SELECT
        sw.service,
        sw.week,
        sw.patient_satisfaction,
        sw.patients_admitted,
        RANK() OVER (
            PARTITION BY sw.service
            ORDER BY sw.patient_satisfaction DESC
        ) AS satisfaction_rank
    FROM services_weekly sw
) ranked
WHERE satisfaction_rank <= 3
ORDER BY service, satisfaction_rank, week;
+------------------+------+----------------------+-------------------+-------------------+
| service          | week | patient_satisfaction | patients_admitted | satisfaction_rank |
+------------------+------+----------------------+-------------------+-------------------+
| emergency        |   19 |                   99 |                16 |                 1 |
| emergency        |   48 |                   99 |                20 |                 1 |
| emergency        |   11 |                   98 |                16 |                 3 |
| general_medicine |   45 |                   99 |                40 |                 1 |
| general_medicine |    1 |                   97 |                37 |                 2 |
| general_medicine |   47 |                   97 |                55 |                 2 |
| ICU              |   16 |                   98 |                15 |                 1 |
| ICU              |   26 |                   98 |                 8 |                 1 |
| ICU              |   48 |                   98 |                16 |                 1 |
| surgery          |   20 |                   99 |                31 |                 1 |
| surgery          |   47 |                   99 |                48 |                 1 |
| surgery          |   14 |                   97 |                33 |                 3 |
| surgery          |   36 |                   97 |                47 |                 3 |
+------------------+------+----------------------+-------------------+-------------------
