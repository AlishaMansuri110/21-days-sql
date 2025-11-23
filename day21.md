Challenge Question: Create a comprehensive hospital performance dashboard using CTEs. Calculate: 1) Service-level metrics (total admissions, refusals, avg satisfaction), 2) Staff metrics per service (total staff, avg weeks present), 3) Patient demographics per service (avg age, count). Then combine all three CTEs to create a final report showing service name, all calculated metrics, and an overall performance score (weighted average of admission rate and satisfaction). Order by performance score descending.

WITH
service_metrics AS (
    SELECT
        service,
        SUM(patients_admitted) AS total_admissions,
        SUM(patients_refused) AS total_refusals,
        AVG(patient_satisfaction) AS avg_satisfaction
    FROM services_weekly
    GROUP BY service
),

staff_metrics AS (
    SELECT
        s.service,
        COUNT(DISTINCT s.staff_id) AS total_staff,
        AVG(weekly_presence.weeks_present) AS avg_weeks_present
    FROM staff s
    LEFT JOIN (
        SELECT 
            staff_id,
            COUNT(week) AS weeks_present
        FROM staff_schedule
        WHERE present = 1
        GROUP BY staff_id
    ) AS weekly_presence
        ON s.staff_id = weekly_presence.staff_id
    GROUP BY s.service
),

patient_metrics AS (
    SELECT
        service,
        AVG(age) AS avg_age,
        COUNT(*) AS patient_count
    FROM patients
    GROUP BY service
)
SELECT
    sm.service,
    sm.total_admissions,
    sm.total_refusals,
    sm.avg_satisfaction,
    st.total_staff,
    st.avg_weeks_present,
    pm.avg_age,
    pm.patient_count,

    (
        (sm.total_admissions * 0.6) + 
        (sm.avg_satisfaction * 0.4)
    ) AS performance_score

FROM service_metrics sm
LEFT JOIN staff_metrics st
    ON sm.service = st.service
LEFT JOIN patient_metrics pm
    ON sm.service = pm.service
ORDER BY performance_score DESC;
+------------------+------------------+----------------+------------------+-------------+-------------------+---------+---------------+-------------------+
| service          | total_admissions | total_refusals | avg_satisfaction | total_staff | avg_weeks_present | avg_age | patient_count | performance_score |
+------------------+------------------+----------------+------------------+-------------+-------------------+---------+---------------+-------------------+
| general_medicine |             2332 |           1938 |          81.2308 |          27 |           30.6667 | 46.1942 |           242 |        1431.69232 |
| surgery          |             1686 |            555 |          79.2692 |          22 |           31.6818 | 45.5945 |           254 |        1043.30768 |
| emergency        |             1185 |           5008 |          77.8846 |          29 |           31.3103 | 45.3270 |           263 |         742.15384 |
| ICU              |              648 |            141 |          81.6154 |          48 |           31.1875 | 44.2158 |           241 |         421.44616 |
+------------------+------------------+----------------+------------------+-------------+-------------------+---------+---------------+-------------------+
