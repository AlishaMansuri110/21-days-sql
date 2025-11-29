1. Identify where and when the crime happened
 FROM evidence
    WHERE room = 'CEO Office'
    ORDER BY found_time DESC;
+-------------+------------+-----------------------------+---------------------+
| evidence_id | room       | description                 | found_time          |
+-------------+------------+-----------------------------+---------------------+
|           2 | CEO Office | Keycard swipe logs mismatch | 2025-10-15 21:10:00 |
|           1 | CEO Office | Fingerprint on desk         | 2025-10-15 21:05:00 |
+-------------+------------+-----------------------------+---------------------+
SELECT room, description, found_time
   FROM evidence
   WHERE room = 'CEO Office'
   AND found_time BETWEEN '2025-10-15 20:00:00' AND '2025-10-15 23:59:59';
+------------+-----------------------------+---------------------+
| room       | description                 | found_time          |
+------------+-----------------------------+---------------------+
| CEO Office | Fingerprint on desk         | 2025-10-15 21:05:00 |
| CEO Office | Keycard swipe logs mismatch | 2025-10-15 21:10:00 |
+------------+-----------------------------+---------------------+

2. Analyze who accessed the critical area (CEO Office) at the time
  SELECT k.log_id, k.employee_id, e.name, k.room, k.entry_time, k.exit_time
    FROM keycard_logs k
    JOIN employees e USING (employee_id)
    WHERE k.room = 'CEO Office'
    AND (k.entry_time BETWEEN '2025-10-15 20:30:00' AND '2025-10-15 21:30:00'
        OR k.exit_time  BETWEEN '2025-10-15 20:30:00' AND '2025-10-15 21:30:00')
    ORDER BY k.entry_time;
+--------+-------------+-------------+------------+---------------------+---------------------+
| log_id | employee_id | name        | room       | entry_time          | exit_time           |
+--------+-------------+-------------+------------+---------------------+---------------------+
|     11 |           4 | David Kumar | CEO Office | 2025-10-15 20:50:00 | 2025-10-15 21:00:00 |
+--------+-------------+-------------+------------+---------------------+---------------------+
SELECT k.log_id, k.employee_id, emp.name, k.entry_time, k.exit_time
    FROM keycard_logs k
    JOIN employees emp USING (employee_id)
    WHERE k.room = 'CEO Office'
      AND k.entry_time <= '2025-10-15 21:00:00'
      AND (k.exit_time IS NULL OR k.exit_time >= '2025-10-15 21:00:00')
    ORDER BY k.entry_time;
+--------+-------------+-------------+---------------------+---------------------+
| log_id | employee_id | name        | entry_time          | exit_time           |
+--------+-------------+-------------+---------------------+---------------------+
|     11 |           4 | David Kumar | 2025-10-15 20:50:00 | 2025-10-15 21:00:00 |
+--------+-------------+-------------+---------------------+---------------------+


3. Cross-check alibis with actual logs (who lied)
 WITH actual_presence AS (
    ->   SELECT employee_id, 'CEO Office' AS room, entry_time, exit_time
    ->   FROM keycard_logs
    ->   WHERE room = 'CEO Office'
    ->     AND entry_time BETWEEN '2025-10-15 00:00:00' AND '2025-10-15 23:59:59'
    -> ),
    -> claimed AS (
    ->   SELECT a.alibi_id, a.employee_id, a.claimed_location, a.claim_time
    ->   FROM alibis a
    ->   WHERE a.claim_time BETWEEN '2025-10-15 20:00:00' AND '2025-10-15 22:00:00'
    -> )
    -> SELECT c.alibi_id, c.employee_id, emp.name,
    ->        c.claimed_location, c.claim_time,
    ->        ap.entry_time, ap.exit_time
    -> FROM claimed c
    -> LEFT JOIN actual_presence ap USING (employee_id)
    -> JOIN employees emp USING (employee_id)
    -> WHERE ap.employee_id IS NOT NULL
    ->   -- example conflict: they claimed to be elsewhere at claim_time but keycard shows they were in CEO Office then
    ->   AND (c.claim_time BETWEEN ap.entry_time AND COALESCE(ap.exit_time, '9999-12-31'))
    -> ORDER BY c.claim_time;
+----------+-------------+-------------+------------------+---------------------+---------------------+---------------------+
| alibi_id | employee_id | name        | claimed_location | claim_time          | entry_time          | exit_time           |
+----------+-------------+-------------+------------------+---------------------+---------------------+---------------------+
|        2 |           4 | David Kumar | Server Room      | 2025-10-15 20:50:00 | 2025-10-15 20:50:00 | 2025-10-15 21:00:00 |
+----------+-------------+-------------+------------------+---------------------+---------------------+---------------------+

4. Investigate suspicious calls made/received around 20:50–21:00
 SELECT c.call_id, c.caller_id, caller.name AS caller_name,
    ->        c.receiver_id, receiver.name AS receiver_name,
    ->        c.call_time, c.duration_sec
    -> FROM calls c
    -> JOIN employees caller ON c.caller_id = caller.employee_id
    -> JOIN employees receiver ON c.receiver_id = receiver.employee_id
    -> WHERE c.call_time BETWEEN '2025-10-15 20:50:00' AND '2025-10-15 21:00:00'
    -> ORDER BY c.call_time;
+---------+-----------+-------------+-------------+---------------+---------------------+--------------+
| call_id | caller_id | caller_name | receiver_id | receiver_name | call_time           | duration_sec |
+---------+-----------+-------------+-------------+---------------+---------------------+--------------+
|       1 |         4 | David Kumar |           1 | Alice Johnson | 2025-10-15 20:55:00 |           45 |
+---------+-----------+-------------+-------------+---------------+---------------------+--------------+


5. Match evidence with movements and claims
 SELECT
    ->     ev.evidence_id,
    ->     ev.description,
    ->     ev.found_time,
    ->     k.employee_id,
    ->     emp.name,
    ->     k.entry_time,
    ->     k.exit_time
    -> FROM evidence ev
    -> LEFT JOIN keycard_logs k
    ->       ON k.room = ev.room
    ->      AND k.entry_time <= ev.found_time
    ->      AND (k.exit_time IS NULL OR
    ->           k.exit_time >= ev.found_time - INTERVAL 1 HOUR)
    -> JOIN employees emp
    ->       ON emp.employee_id = k.employee_id
    -> WHERE ev.room = 'CEO Office'
    ->   AND ev.found_time BETWEEN '2025-10-15 20:30:00' AND '2025-10-15 22:00:00'
    -> ORDER BY ev.found_time;
+-------------+-----------------------------+---------------------+-------------+-------------+---------------------+---------------------+
| evidence_id | description                 | found_time          | employee_id | name        | entry_time          | exit_time           |
+-------------+-----------------------------+---------------------+-------------+-------------+---------------------+---------------------+
|           1 | Fingerprint on desk         | 2025-10-15 21:05:00 |           4 | David Kumar | 2025-10-15 20:50:00 | 2025-10-15 21:00:00 |
|           2 | Keycard swipe logs mismatch | 2025-10-15 21:10:00 |           4 | David Kumar | 2025-10-15 20:50:00 | 2025-10-15 21:00:00 |
+-------------+-----------------------------+---------------------+-------------+-------------+---------------------+---------------------+

6. Combine all findings to identify suspect(s)
WITH in_office AS (
    ->   -- anyone inside CEO Office at the time of murder (present at 21:00)
    ->   SELECT DISTINCT k.employee_id
    ->   FROM keycard_logs k
    ->   WHERE k.room = 'CEO Office'
    ->     AND k.entry_time <= '2025-10-15 21:00:00'
    ->     AND (k.exit_time IS NULL OR k.exit_time >= '2025-10-15 21:00:00')
    -> ),
    ->
    -> lied_alibi AS (
    ->   -- anyone whose alibi claim time overlaps with being in CEO Office (claimed elsewhere but presence shows otherwise)
    ->   SELECT DISTINCT a.employee_id
    ->   FROM alibis a
    ->   JOIN keycard_logs k
    ->     ON a.employee_id = k.employee_id
    ->    AND k.room = 'CEO Office'
    ->    AND a.claim_time BETWEEN k.entry_time AND COALESCE(k.exit_time, '9999-12-31 23:59:59')
    ->   WHERE a.claim_time BETWEEN '2025-10-15 20:00:00' AND '2025-10-15 22:00:00'
    -> ),
    ->
    -> suspicious_calls AS (
    ->   -- anyone who made or received a call in the suspicious window
    ->   SELECT DISTINCT caller_id AS employee_id
    ->   FROM calls
    ->   WHERE call_time BETWEEN '2025-10-15 20:50:00' AND '2025-10-15 21:00:00'
    ->   UNION
    ->   SELECT DISTINCT receiver_id AS employee_id
    ->   FROM calls
    ->   WHERE call_time BETWEEN '2025-10-15 20:50:00' AND '2025-10-15 21:00:00'
    -> ),
    ->
    -> evidence_link AS (
    ->   -- employees who were in CEO Office within 1 hour before an evidence found time
    ->   SELECT DISTINCT k.employee_id
    ->   FROM evidence ev
    ->   JOIN keycard_logs k ON k.room = ev.room
    ->     AND k.entry_time <= ev.found_time
    ->     AND (k.exit_time IS NULL OR k.exit_time >= ev.found_time - INTERVAL 1 HOUR)
    ->   WHERE ev.room = 'CEO Office'
    ->     AND ev.found_time BETWEEN '2025-10-15 20:30:00' AND '2025-10-15 22:00:00'
    -> )
    ->
    -> -- Final shortlist: those who meet ALL criteria
    -> SELECT emp.employee_id, emp.name
    -> FROM employees emp
    -> JOIN in_office i ON emp.employee_id = i.employee_id
    -> JOIN lied_alibi l ON emp.employee_id = l.employee_id
    -> JOIN suspicious_calls s ON emp.employee_id = s.employee_id
    -> JOIN evidence_link el ON emp.employee_id = el.employee_id;
+-------------+-------------+
| employee_id | name        |
+-------------+-------------+
|           4 | David Kumar |
+-------------+-------------+


Final — Case Solved (single-column killer as required)
 WITH in_office AS (
    ->   SELECT DISTINCT k.employee_id
    ->   FROM keycard_logs k
    ->   WHERE k.room = 'CEO Office'
    ->     AND k.entry_time <= '2025-10-15 21:00:00'
    ->     AND (k.exit_time IS NULL OR k.exit_time >= '2025-10-15 21:00:00')
    -> ),
    -> lied_alibi AS (
    ->   SELECT DISTINCT a.employee_id
    ->   FROM alibis a
    ->   JOIN keycard_logs k
    ->     ON a.employee_id = k.employee_id
    ->    AND k.room = 'CEO Office'
    ->    AND a.claim_time BETWEEN k.entry_time AND COALESCE(k.exit_time, '9999-12-31 23:59:59')
    ->   WHERE a.claim_time BETWEEN '2025-10-15 20:00:00' AND '2025-10-15 22:00:00'
    -> ),
    -> suspicious_calls AS (
    ->   SELECT DISTINCT caller_id AS employee_id
    ->   FROM calls
    ->   WHERE call_time BETWEEN '2025-10-15 20:50:00' AND '2025-10-15 21:00:00'
    ->   UNION
    ->   SELECT DISTINCT receiver_id AS employee_id
    ->   FROM calls
    ->   WHERE call_time BETWEEN '2025-10-15 20:50:00' AND '2025-10-15 21:00:00'
    -> ),
    -> evidence_link AS (
    ->   SELECT DISTINCT k.employee_id
    ->   FROM evidence ev
    ->   JOIN keycard_logs k
    ->     ON k.room = ev.room
    ->    AND k.entry_time <= ev.found_time
    ->    AND (k.exit_time IS NULL OR k.exit_time >= ev.found_time - INTERVAL 1 HOUR)
    ->   WHERE ev.room = 'CEO Office'
    ->     AND ev.found_time BETWEEN '2025-10-15 20:30:00' AND '2025-10-15 22:00:00'
    -> )
    -> SELECT emp.name AS killer
    -> FROM employees emp
    -> JOIN in_office i ON emp.employee_id = i.employee_id
    -> JOIN lied_alibi l ON emp.employee_id = l.employee_id
    -> JOIN suspicious_calls s ON emp.employee_id = s.employee_id
    -> JOIN evidence_link el ON emp.employee_id = el.employee_id;
+-------------+
| killer      |
+-------------+
| David Kumar |
+-------------+

**A short explanation**
Scene & time — evidence rows show physical items in CEO Office discovered during the evening of 2025-10-15, establishing the crime scene and time window.
Presence — keycard logs show which employees entered and were inside the CEO Office at 21:00.
Contradicted alibi — the alibis table contains a claim where an employee said they were elsewhere at a time that overlaps with their keycard presence in the CEO Office, indicating a false alibi.
Suspicious call — calls reveal someone who made or received a call between 20:50–21:00, immediately prior to the murder time.
Evidence link — timestamps tie the found evidence to people whose keycard logs put them in the room within one hour before evidence was logged.
Intersection — the final query finds the person who meets all conditions: in the CEO Office at 21:00, contradicted their alibi, had a call in the suspicious window, and whose presence temporally links to the discovered evidence. That intersection yields David Kumar.

