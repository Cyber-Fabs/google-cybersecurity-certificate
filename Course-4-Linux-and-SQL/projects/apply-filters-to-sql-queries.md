# Apply Filters to SQL Queries

## Project Description

In this project, I investigated potential security issues within my organization by applying SQL filters to retrieve specific records from the `employees` and `log_in_attempts` tables. I used SQL filtering techniques including the `AND`, `OR`, `NOT`, `LIKE`, and `BETWEEN` operators, as well as comparison operators such as `>`, `>=`, `<`, `<=`, `=`, and `!=`, to examine login attempts and employee machine data. The goal was to identify suspicious activity, investigate security incidents, and support the organization's efforts to maintain a secure system.

---

## Retrieve After Hours Failed Login Attempts

My organization's business hours end at 18:00:00. I needed to investigate failed login attempts that occurred after business hours, as these could indicate unauthorized access attempts or brute force attacks targeting the system when staff are not present.

**SQL Query:**
```sql
SELECT *
FROM log_in_attempts
WHERE login_time > '18:00:00'
AND success = 0;
```

**Explanation:**

The query retrieves all columns from the `log_in_attempts` table. I applied two filters using the `AND` operator to ensure both conditions must be true for a record to be returned:

- `login_time > '18:00:00'` — filters for login attempts made after 18:00:00 (6PM), outside of normal business hours
- `success = 0` — filters for failed login attempts only, since `FALSE` is stored as `0` in MySQL

The `AND` operator ensures that only records meeting both conditions are returned — login attempts that are both after hours AND failed. This query helped identify suspicious activity occurring outside of normal working hours that warrants further investigation.

---

## Retrieve Login Attempts on Specific Dates

A suspicious security event was discovered on 2022-05-09. I needed to investigate all login attempts that occurred on that day and the day before (2022-05-08) to understand what activity led up to the incident and identify any patterns that could indicate the source of the breach.

**SQL Query:**
```sql
SELECT *
FROM log_in_attempts
WHERE login_date = '2022-05-09'
OR login_date = '2022-05-08';
```

**Explanation:**

The query retrieves all columns from the `log_in_attempts` table. I applied two date filters using the `OR` operator:

- `login_date = '2022-05-09'` — retrieves all login attempts on the day of the incident
- `login_date = '2022-05-08'` — retrieves all login attempts on the day before the incident

The `OR` operator ensures that records from either date are returned. I used `OR` instead of `AND` because a single login attempt can only occur on one date at a time — using `AND` would have returned zero results. Checking the day before the incident helped identify whether reconnaissance or initial access attempts occurred prior to the main event.

---

## Retrieve Login Attempts Outside of Mexico

My team needed to investigate login attempts that did not originate from Mexico. The `country` column contains two variations for Mexico — `'MEX'` and `'MEXICO'` — so I needed a pattern-based filter to exclude both variations accurately.

**SQL Query:**
```sql
SELECT *
FROM log_in_attempts
WHERE country NOT LIKE 'MEX%';
```

**Explanation:**

The query retrieves all columns from the `log_in_attempts` table. I used the `NOT LIKE` operator with the pattern `'MEX%'` to exclude all records where the country starts with MEX:

- `NOT` — reverses the result of the `LIKE` comparison, excluding matching records
- `LIKE 'MEX%'` — matches any value beginning with MEX, including both `MEX` and `MEXICO`
- `%` — wildcard that matches any number of characters after MEX

Using `NOT LIKE 'MEX%'` instead of `!= 'MEX'` was essential because a simple not-equal operator would have missed entries stored as `MEXICO`. This query returned all login attempts from every country except Mexico for further analysis.

---

## Retrieve Employees in Marketing

My team needed to update the computers of employees in the Marketing department who are located in the East building. I needed to identify these specific employees to target the update correctly without affecting employees in other departments or buildings.

**SQL Query:**
```sql
SELECT *
FROM employees
WHERE department = 'Marketing'
AND office LIKE 'East%';
```

**Explanation:**

The query retrieves all columns from the `employees` table. I applied two filters using the `AND` operator:

- `department = 'Marketing'` — exact match filter to return only Marketing department employees
- `office LIKE 'East%'` — pattern match filter using `%` wildcard to return all East building offices such as `East-170`, `East-320`, and `East-215`

The `AND` operator ensures only employees who are in the Marketing department AND located in the East building are returned. I used `=` for the department because it has one exact value, and `LIKE` for the office because there are multiple room variations within the East building that needed to be captured with a single filter.

---

## Retrieve Employees in Finance or Sales

My team needed to perform a security update on the computers of all employees in the Finance and Sales departments. I needed to retrieve all employees from both departments in a single query to obtain their information for the update.

**SQL Query:**
```sql
SELECT *
FROM employees
WHERE department = 'Finance'
OR department = 'Sales';
```

**Explanation:**

The query retrieves all columns from the `employees` table. I applied two department filters using the `OR` operator:

- `department = 'Finance'` — returns all Finance department employees
- `department = 'Sales'` — returns all Sales department employees

The `OR` operator ensures that employees from either department are included in the results. I used `OR` instead of `AND` because an employee can only belong to one department at a time — using `AND` would have returned zero results. This query returned all Finance and Sales employees together in a single result set for the update team.

---

## Retrieve All Employees Not in IT

The security update had already been applied to all computers in the Information Technology department. My team needed to apply the same update to all remaining employees in other departments. I needed to exclude IT employees from the results to avoid duplicating work already completed.

**SQL Query:**
```sql
SELECT *
FROM employees
WHERE NOT department = 'Information Technology';
```

**Explanation:**

The query retrieves all columns from the `employees` table. I used the `NOT` operator to exclude Information Technology department employees:

- `NOT` — reverses the condition, returning all records that do NOT match
- `department = 'Information Technology'` — the condition being reversed

The `NOT` operator ensured that every employee except those in the Information Technology department was returned. This gave the update team a complete list of all remaining employees whose machines still needed the security update applied.

---

## Summary

In this project, I used SQL filters to investigate security issues and retrieve targeted employee and login attempt data from the organization's database. I applied the following techniques across six queries:

| Task | Table Used | Operators Used | Purpose |
|------|------------|----------------|---------|
| After hours failed logins | `log_in_attempts` | `AND`, `>`, `=` | Find suspicious after hours activity |
| Logins on specific dates | `log_in_attempts` | `OR`, `=` | Investigate incident day and day before |
| Logins outside Mexico | `log_in_attempts` | `NOT`, `LIKE`, `%` | Exclude MEX and MEXICO variations |
| Marketing in East building | `employees` | `AND`, `=`, `LIKE`, `%` | Target specific department and building |
| Finance or Sales employees | `employees` | `OR`, `=` | Retrieve two departments at once |
| Employees not in IT | `employees` | `NOT`, `=` | Exclude already updated department |

The `AND` operator was used to narrow results by requiring multiple conditions to be true simultaneously. The `OR` operator was used to broaden results by returning records that matched either of two conditions. The `NOT` operator was used to exclude specific records from results. The `LIKE` operator combined with the `%` wildcard was used to match patterns where exact values had multiple variations. These SQL filtering techniques are essential tools for security analysts investigating incidents, auditing access, and managing employee data efficiently.
