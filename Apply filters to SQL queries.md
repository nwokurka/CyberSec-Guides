---
up: "[[SQL MOC 🗺️]]"
---
## 📌 Project Description

In this note, you’ll see how to use the `AND`, `OR`, and `NOT` operators to create filters for SQL queries.

### Scenario

You are a security professional at a large organization. Part of your job is to investigate security issues to help keep the system secure. You recently discovered some potential security issues that involve login attempts and employee machines.

Your task is to examine the organization’s data in their **employees** and **log_in_attempts** tables. You’ll need to use SQL filters to retrieve records from different datasets and investigate the potential security issues.

---
## Task 1 | Retrieve after hours failed login attempts

You recently discovered a potential security incident that occurred after business hours. To investigate this, you need to query the **log_in_attempts** table and review after hours login activity. Use filters in SQL to create a query that identifies all failed login attempts that occurred after 18:00. (The time of the login attempt is found in the **login_time** column. The **success** column contains a value of **0** when a login attempt failed.
You can use either a value of **0** or **FALSE** in your query to identify failed login attempts.

``` SQL
SELECT *
FROM log_in_attempts
WHERE login_time > '18:00:00' AND success = 0;
```

![[Pasted image 20250319130121.png]]
#### Here’s a detailed breakdown of the query and its key components:

- **SELECT**: This part of the query specifies that all columns from the log_in_attempts table should be retrieved. The table contains columns such as event_id, username, login_date, login_time, country, ip_address, and success, which provide detailed information about each login attempt. By selecting all columns, the query ensures that the investigator has access to the full context of each failed login event for further analysis.

- **FROM log_in_attempts**: This indicates the data source, which is the log_in_attempts table in the database. This table likely stores historical login activity, making it a valuable resource for auditing and investigating security incidents.

- **WHERE login_time > '18:00:00' AND success = 0**: The WHERE clause is the core of the filtering mechanism in this query. It applies two conditions to narrow down the results to only the records that meet both criteria:
    - **login_time > '18:00:00'**: This condition filters for login attempts that occurred after 18:00 (6:00 PM). The login_time column stores the time of each login attempt in a time format (e.g., 20:27:27). By comparing it to '18:00:00', the query excludes any attempts made during regular business hours and focuses on after-hours activity, which is more likely to be suspicious.
    - **success = 0**: This condition filters for failed login attempts. The success column is a boolean-like field where a value of 0 (or FALSE) indicates that the login attempt was unsuccessful. Failed logins are often a red flag in cybersecurity, as they may indicate unauthorized access attempts, such as someone trying to guess a password.
    - The **AND** operator ensures that both conditions must be true for a record to be included in the results. This combination of filters isolates failed login attempts that occurred after 18:00, aligning with the goal of identifying potential security incidents during off-hours.

- **Query Results**: The output of the query includes 19 rows, each representing a failed login attempt after 18:00. For example, the first row shows a failed login attempt by the user apatel at 20:27:27 on 2022-05-10 from an IP address in Canada (192.168.205.12). The success column confirms the failure with a value of 0. This data can be used to identify patterns, such as repeated failed attempts from the same IP address or targeting specific users, which could indicate a targeted attack.

- **Performance Note**: The query executed in 0.019 seconds, indicating that it is efficient for the dataset size. This is important in a cybersecurity context, where quick analysis of logs can be critical during an active incident response.

#### Why This Query Matters in Cybersecurity

This query demonstrates a practical application of SQL in cybersecurity investigations. By filtering for failed login attempts after business hours, it helps identify potential security incidents, such as unauthorized access attempts or brute-force attacks. The ability to pinpoint suspicious activity using precise filters like login_time and success is a valuable skill for a cybersecurity professional, as it enables rapid detection and response to threats.

## Task 2 | Retrieve login attempts on specific dates

A suspicious event occurred on 2022-05-09. To investigate this event, you want to review all login attempts which occurred on this day and the day before. Use filters in SQL to create a query that identifies all login attempts that occurred on 2022-05-09 or 2022-05-08. (The date of the login attempt is found in the **login_date** column.)

``` SQL
SELECT * 
FROM log_in_attempts 
WHERE login_date = '2022-05-08' OR login_date = '2022-05-09';
```

![[Pasted image 20250319132647.png]]

This SQL query investigates a suspicious event by retrieving all login attempts from the **log_in_attempts** table that occurred on 2022-05-08 or 2022-05-09. Unlike the previous query, which focused on after-hours failed logins, this query targets a specific date range to analyze activity around the incident.

- **WHERE login_date = '2022-05-08' OR login_date = '2022-05-09'**: The WHERE clause now filters based on the login_date column, using the OR operator to include records from either 2022-05-08 or 2022-05-09. This allows the query to capture all login attempts—successful or failed—over the two-day period, providing a broader view of activity surrounding the suspicious event.

The query showing login attempts with details like usernames, timestamps, countries, and IP addresses. For example, a successful login by `jrafael` on `2022-05-09` at `04:56:27` from Canada (192.168.249.140) is included, alongside failed attempts like `dkot `on **`2022-05-08`** at `02:00:39`. This data helps identify patterns or anomalies tied to the incident.

## Task 3 | Retrieve login attempts outside of Mexico

There’s been suspicious activity with login attempts, but the team has determined that this activity didn't originate in Mexico. Now, you need to investigate login attempts that occurred outside of Mexico. Use filters in SQL to create a query that identifies all login attempts that occurred outside of Mexico. (When referring to Mexico, the **country** column contains values of both **MEX** and **MEXICO**, and you need to use the **LIKE** keyword with **%** to make sure your query reflects this.)

``` SQL
SELECT * 
FROM log_in_attempts
WHERE NOT country LIKE 'MEX%';
```

![[Pasted image 20250319133823.png]]

This SQL query investigates suspicious login activity by retrieving all login attempts from the log_in_attempts table that did not originate in Mexico. Building on the previous tasks, which filtered by time and date, this query introduces a new filter to exclude a specific country using pattern matching.

- **WHERE NOT country LIKE 'MEX%'**: The WHERE clause now filters based on the country column. The LIKE 'MEX%' condition matches any country value starting with "MEX" (e.g., "MEX" or "MEXICO"), where the % wildcard accounts for additional characters. The NOT operator inverts this, excluding those matches and returning only login attempts from other countries, such as the US or Canada.

The query showing login attempts from countries like the US, Canada, and others, but none from Mexico. For example, a successful login by `jrafael` on `2022-05-09` at `04:56:27` from Canada (`192.168.249.140`) is included, while any attempts from "MEX" or "MEXICO" are excluded. This helps focus the investigation on external activity, as the team determined the suspicious behavior did not originate in Mexico.

## Task 4 | Retrieve employees in Marketing

Your team wants to perform security updates on specific employee machines in the Marketing department. You’re responsible for getting information on these employee machines and will need to query the **employees** table. Use filters in SQL to create a query that identifies all employees in the Marketing department for all offices in the East building.

(The department of the employee is found in the **department** column, which contains values that include **Marketing**. The office is found in the office column. Some examples of values in this column are **East-170**, **East-320**, and **North-434**. You’ll need to use the **LIKE** keyword with **%** to filter for the East building.)

``` SQL
SELECT * 
FROM employees 
WHERE department = 'Marketing' AND office LIKE 'East%';
```

![[Pasted image 20250319134457.png]]

This SQL query retrieves employee information from the employees table to support security updates for machines in the Marketing department located in the East building. Unlike previous tasks that analyzed login attempts, this query targets employee data for a specific department and office location.

- **WHERE department = 'Marketing' AND office LIKE 'East%'**: The `WHERE` clause filters for employees in the Marketing department (department = 'Marketing') and in the East building (office `LIKE 'East%'`), where the `%` wildcard matches any office starting with "East" (e.g., `"East-170"`).

The query returns 7 rows, including employees like `elarson` in office `East-170` (device ID `a320b137c219`) and dellery in `East-417` (device ID `a184b775c707`). The results provide employee IDs, device IDs, usernames, and office locations, enabling the team to identify machines in the Marketing department within the East building for security updates.

## Task 5 | Retrieve employees in Finance or Sales

Your team now needs to perform a different security update on machines for employees in the Sales and Finance departments. Use filters in SQL to create a query that identifies all employees in the Sales or Finance departments. (The department of the employee is found in the **department** column, which contains values that include **Sales** and **Finance**.)

``` SQL
SELECT * 
FROM employees 
WHERE department = 'Finance' OR department = 'Sales';
```

![[Pasted image 20250319135501.png]]

This SQL query retrieves employee information from the employees table to support security updates for machines in the Sales and Finance departments. Compared to the previous task, which targeted Marketing in the East building, this query focuses on a different set of departments without location constraints.

- **WHERE department = 'Finance' OR department = 'Sales'**: The WHERE clause uses the OR operator to filter for employees in either the Finance or Sales department. This retrieves all matching records regardless of office location, unlike the previous query’s use of LIKE for office filtering.

The query returns rows, including employees like `sgilmore` from Finance in `South-153` (device ID `d394e816f943`) and jclark from Sales in `North-188` (device ID `r550b824t230`). The results provide employee IDs, device IDs, usernames, departments, and office locations, enabling the team to identify machines in the Sales and Finance departments for the security update.

## Task 6 | Retrieve all employees not in IT

Your team needs to make one more update to employee machines. The employees who are in the Information Technology department already had this update, but employees in all other departments need it. Use filters in SQL to create a query which identifies all employees not in the IT department. (The department of the employee is found in the **department** column, which contains values that include **Information Technology**.)

``` SQL 
SELECT * 
FROM employees 
WHERE NOT department = 'Information Technology';
```

![[Pasted image 20250319135948.png]]

This SQL query retrieves employee information from the employees table for a security update targeting all employees except those in the Information Technology (IT) department, as IT employees have already received the update. Unlike the previous task, which included specific departments (Sales or Finance), this query excludes a single department.

- **WHERE NOT department = 'Information Technology'**: The WHERE clause uses the NOT operator to exclude employees in the IT department (department = 'Information Technology'). This returns employees from all other departments, such as Marketing, Finance, and Sales, without additional filtering.

The query returns 20 rows, including employees like elarson from Marketing in East-170 (device ID a320b137c219) and sgilmore from Finance in South-153 (device ID d394e816f943). The results list employee IDs, device IDs, usernames, departments, and office locations, allowing the team to identify machines needing the update across all non-IT departments.


---
## 📝 Summary

#### 1. Investigate After-Hours Failed Login Attempts

- **Query**: SELECT * FROM log_in_attempts WHERE login_time > '18:00:00' AND success = 0;
- **Purpose**: Identifies failed login attempts after 18:00 to investigate potential unauthorized access during off-hours.
- **Key Filter**: login_time > '18:00:00' (after 18:00) and success = 0 (failed logins), combined with AND.
- **Result**: Returned 19 rows, e.g., a failed attempt by apatel at 20:27:27 from Canada (192.168.205.12).

#### 2. Retrieve Login Attempts on Specific Dates

- **Query**: SELECT * FROM log_in_attempts WHERE login_date = '2022-05-08' OR login_date = '2022-05-09';
- **Purpose**: Analyzes login activity on 2022-05-08 and 2022-05-09 to investigate a suspicious event.
- **Key Filter**: login_date = '2022-05-08' OR login_date = '2022-05-09' to include both dates.
- **Result**: Returned 44 rows, including a successful login by jrafael on 2022-05-09 at 04:56:27 from Canada (192.168.249.140).

#### 3. Retrieve Login Attempts Excluding Mexico

- **Query**: SELECT * FROM log_in_attempts WHERE NOT country LIKE 'MEX%';
- **Purpose**: Focuses on login attempts outside Mexico, as suspicious activity did not originate there.
- **Key Filter**: NOT country LIKE 'MEX%' excludes countries starting with "MEX" (e.g., "MEX" or "MEXICO").
- **Result**: Returned 36 rows, e.g., a successful login by jrafael on 2022-05-09 at 04:56:27 from Canada (192.168.249.140).

#### 4. Retrieve Employees in Marketing (East Building)

- **Query**: SELECT * FROM employees WHERE department = 'Marketing' AND office LIKE 'East%';
- **Purpose**: Identifies Marketing department employees in the East building for security updates.
- **Key Filter**: department = 'Marketing' and office LIKE 'East%' (e.g., "East-170") to target specific department and location.
- **Result**: Returned 7 rows, including elarson in East-170 (device ID a320b137c219).

#### 5. Retrieve Employees in Finance or Sales

- **Query**: SELECT * FROM employees WHERE department = 'Finance' OR department = 'Sales';
- **Purpose**: Identifies employees in the Finance or Sales departments for a different security update.
- **Key Filter**: department = 'Finance' OR department = 'Sales' to include both departments.
- **Result**: Returned 14 rows, e.g., sgilmore from Finance in South-153 (device ID d394e816f943).

#### 6. Retrieve All Employees Not in IT

- **Query**: SELECT * FROM employees WHERE NOT department = 'Information Technology';
- **Purpose**: Identifies employees in all departments except IT for a security update, as IT already received it.
- **Key Filter**: NOT department = 'Information Technology' to exclude IT employees.
- **Result**: Returned 20 rows, including elarson from Marketing in East-170 (device ID a320b137c219).

### Overall Takeaway

These queries showcase your ability to use SQL filters in cybersecurity tasks, such as investigating security incidents (log_in_attempts) and managing employee device updates (employees). You applied various filtering techniques, including AND, OR, NOT, and LIKE with wildcards (%), to target specific timeframes, dates, locations, departments, and exclusions. The results provide actionable data, like IP addresses for login analysis or device IDs for updates, demonstrating practical skills for a cybersecurity role.

