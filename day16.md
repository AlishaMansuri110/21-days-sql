Challenge Question: Find all patients who were admitted to services that had at least one week where patients were refused AND the average patient satisfaction for that service was below the overall hospital average satisfaction. Show patient_id, name, service, and their personal satisfaction score.

SELECT 
    p.patient_id,
    p.name,
    p.service,
    p.satisfaction AS personal_satisfaction
FROM patients p
JOIN services_weekly sw
    ON p.service = sw.service
WHERE 
    sw.patients_refused > 0
    AND sw.patient_satisfaction < (
        SELECT AVG(patient_satisfaction)
        FROM services_weekly
    )
GROUP BY 
    p.patient_id,
    p.name,
    p.service,
    p.satisfaction;
+--------------+-------------------------+------------------+-----------------------+
| patient_id   | name                    | service          | personal_satisfaction |
+--------------+-------------------------+------------------+-----------------------+
| PAT-09484753 | Richard Rodriguez       | surgery          |                    61 |
| PAT-f0644084 | Shannon Walker          | surgery          |                    83 |
| PAT-ac6162e4 | Julia Torres            | general_medicine |                    83 |
| PAT-3dda2bb5 | Crystal Johnson         | emergency        |                    81 |
| PAT-08591375 | Garrett Lin             | ICU              |                    76 |
| PAT-f4b29bae | Diana May               | emergency        |                    81 |
| PAT-283cda07 | William Herrera         | emergency        |                    66 |
| PAT-5b61868c | Ashley Waller           | ICU              |                    82 |
| PAT-f9c8afa6 | Victor Baker            | general_medicine |                    91 |
| PAT-5290be70 | Jeffrey Chandler        | emergency        |                    88 |
| PAT-003ce690 | Larry Dixon             | ICU              |                    60 |
| PAT-18f78014 | Kenneth Scott           | general_medicine |                    61 |
| PAT-69dc0dc1 | April Frost             | general_medicine |                    60 |
| PAT-95afe21e | Michelle Harmon         | emergency        |                    62 |
| PAT-cc50ad71 | Helen Jones             | surgery          |                    92 |
| PAT-0beb4754 | Erin Edwards            | general_medicine |                    66 |
| PAT-3223420e | Michelle Evans          | emergency        |                    62 |
| PAT-f0682772 | Jason Powell            | general_medicine |                    67 |
| PAT-af65396f | Cameron Fisher          | emergency        |                    62 |
| PAT-dbbf6d1f | Megan Orr               | emergency        |                    93 |
| PAT-5e3f8747 | Elizabeth Kelley        | ICU              |                    91 |
| PAT-ba5529f4 | Dustin Jordan           | general_medicine |                    80 |
| PAT-208fd478 | Mary Marshall           | general_medicine |                    66 |
| PAT-127ca40e | Daniel Kennedy          | general_medicine |                    80 |
