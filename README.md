# SQL Lab: SQL for Security Analysts

## Description
In this lab I will demonstrate how a security analyst working to secure a large organization’s system would use SQL queries with filters inside the organization’s database to identify potential security issues the organization is facing in regards to failed login attempts taken on their system after normal working hours, on specific dates, or outside a specific region.

I will also demonstrate how the analyst can use SQL to identify updates needed on employee computers by running operators that allow for filtering through the organization’s database in a much more efficient fashion across numerous departments.

As you work through this lab with me, it is important to know where the data is coming from. For this lab we will be examining the organization’s data in their employees and log_in_attempts tables. We will need to use SQL filters to retrieve records from different datasets and investigate the potential security issues, then note our findings and summarize our work.

## Bonus Material - Blog Post

Follow the link to read more on the strategy and various tools involved in executing SQL commands. There are also 5 helpful **SQL** searh queries given along with an explanation of how to use them and sample code to correspond:

[SQL Lab for Security Analysts: A Walkthrough](https://hispanictitanic.medium.com/sql-lab-for-security-analysts-63c59720b777)

## Table of Contents

   * [Table Formats](#Table-Formats)
   * [log_in_attempts](#log_in_attempts)
   * [employees](#employees)
   * [Failed Login Attempts - After Hours](#Failed-Login-Attempts-After-Hours)
   * [Failed Login Attempts - Specific Dates](#Failed-Login-Attempts-Specific-Dates)
   * [Failed Login Attempts - Outside of Mexico](#Failed-Login-Attempts-Outside-of-Mexico)
   * [Employee Updates - Marketing Department](#Employee-Updates-Marketing-Department)
   * [Employee Updates - Sales and Finance Department](#Employee-Updates-Sales-and-Finance-Department)
   * [Employee Updates - Not the Info Tech Department](#Employee-Updates-Not-the-Info-Tech-Department)
   * [Summary](#Summary)
   * [Key Takeaways](#Key-Takeaways)

### Table Formats
The organization database contains the following two tables:

* ```log_in_attempts```
* ```employees```

Here is a breakdown of each table and the columns they contain:

### log_in_attempts

The log_in_attempts table has the following columns:

* ```event_id```: The identification number assigned to each login event
* ```username```: The username of the employee
* ```login_date```: The date the login attempt was recorded
* ```login_time```: The time the login attempt was recorded
* ```country```: The country where the login attempt occurred
* ```ip_address```: The IP address of that employee’s machine
* ```success```: The success of the login attempt; FALSE indicates a failed attempt

In the MariaDB shell, these columns are returned as:
![image](https://github.com/resii-tech/SQL-Lab/assets/129999089/eb80e493-5648-4553-adb3-9efa951162f5)

### employees

The employees table has the following columns:

* ```employee_id```: The identification number assigned to each employee
* ```device_id```: The identification number assigned to each device used by the employee
* ```username```: The username of the employee
* ```department```: The department the employee is in
* ```office```: The office the employee is located in

In the MariaDB shell, these columns are returned as:

![image](https://github.com/resii-tech/SQL-Lab/assets/129999089/a1969dfe-5c8b-491e-b3be-87dd1fb35631)

## Failed Login Attempts — After Hours

The security analyst discovers a potential security incident that occurred after business hours (after 18:00). All after hours login attempts that fail need to be investigated. In order to investigate we will need to query the log_in_attempts table and review after hours login activity.

Using what we know, we identify two criteria for our search. The login attempt must have happened after 18:00 hours and the login attempt must have failed .

Screenshot

![image](https://github.com/resii-tech/SQL-Lab/assets/129999089/be7d03bf-7f86-476e-8c9f-1a94c4658760)

Command

```sql
SELECT *
FROM log_in_attempts
WHERE login_time > '18:00' AND success = 0;
```

Breakdown

* ```SELECT *``` — * is a wildcard which indicates you will select all
* ```FROM log_in_atttempts``` — specifies which dataset we will be pulling from
* ```WHERE login_time > ‘18:00’``` — using WHERE clause, specifying first condition as time of login to be greater than 18:00
* ```AND success = 0;``` — using AND operator to create a multi-condition query that specifies the second condition, which pulls only unsuccessful attempts. FALSE = 0 and TRUE = 1.

Query in three parts and the output it gives

![image](https://github.com/resii-tech/SQL-Lab/assets/129999089/4bd8b89b-a942-45b8-a313-561ad232cee9)

## Failed Login Attempts — Specific Dates

The security analyst identified a suspicious event that occurred on May 9, 2022. Any login attempts that happened on that date or on the day before need to be investigated. In order to investigate we will need to query the log_in_attempts table and review all attempts made on 5.8.22 or 5.9.22 (the date of the login attempt is found in the login_date column).

Screenshot

![image](https://github.com/resii-tech/SQL-Lab/assets/129999089/f9765d94-cd9b-45ac-b99f-2554b7faaa39)

Command

```sql
SELECT *
FROM log_in_attempts
WHERE login_date = '2022–05–08' OR login_date = '2022–05–09';
```

Breakdown

* ```SELECT *``` — * is a wildcard which indicates you will select all
* ```FROM log_in_atttempts``` — specifies which dataset we will be pulling from
* ```WHERE``` — clause used with OR operator for multi condition query
* ```login_date = ‘2022–05–08’``` — first condition as date of login to be May 8, 2022
* ```OR login_date = ‘2022–05–09’;``` — second condition as date of login to be May 9, 2022

Query in three parts and the output it gives

![image](https://github.com/resii-tech/SQL-Lab/assets/129999089/348dc87c-adb0-4a83-a170-6b1a2b39af6f)

## Failed Login Attempts — Outside of Mexico

The security analyst has determined that the suspicious activity did not originate in Mexico and needs to find out what other regions the login attempts came from. In order to investigate we will need to query the log_in_attempts table and review all attempts made outside of Mexico (the country column contains values of both MEX and MEXICO, and you need to use the LIKE keyword with % to make sure your query reflects this)

Screenshot

![image](https://github.com/resii-tech/SQL-Lab/assets/129999089/e937c391-29b6-4894-bfb8-3f65366efb01)

Command

```sql
SELECT *
FROM log_in_attempts
WHERE NOT country LIKE 'MEX%';
```

Breakdown

* ```SELECT *``` — * is a wildcard which indicates you will select all
* ```FROM log_in_atttempts``` — specifies which dataset we will be pulling from
* ```WHERE NOT``` — clause (WHERE) used with filter (NOT) for countries other than Mexico
* ```country``` — column we are pulling from
* ```LIKE``` — Operator used with WHERE to search for a pattern in a column
* ```‘MEX%’;``` — pattern using the percentage sign (%) which represents any number of unspecified characters when used with LIKE

Query in three parts and the output it gives

![image](https://github.com/resii-tech/SQL-Lab/assets/129999089/db84da20-4401-4910-926e-33c1bd7f2665)

## Employee Updates — Marketing Department

The security analyst wants to perform security updates on specific employee machines in the Marketing department located in the East Building. We will need to query the employees table using filters in SQL to create a filter that identifies all employees in the Marketing department for all offices in the East building.

We will need to pull from two columns — department column and office column. The office column lists examples of buildings like this — East-170, East-320, and North-434. We will need to use the LIKE keyword with % to filter for the East building.

Screenshot

![image](https://github.com/resii-tech/SQL-Lab/assets/129999089/ef057089-462e-4f39-ad96-e30d232b2cf6)

Command

```sql
SELECT *
FROM employees
WHERE department = 'Marketing' AND office LIKE 'East%';
```

Breakdown

* ```SELECT *``` — * is a wildcard which indicates you will select all
* ```FROM employees``` — specifies which dataset we will be pulling from
* ```WHERE department =``` — clause (WHERE) used with AND to filter for employees who meet both criteria (marketing and east building) from department column
* ```‘Marketing’ AND``` - first condition, specific department with operator AND
* ```office LIKE ‘EAST%’;``` - second condition, specific department (office) pattern matching filters (LIKE, EAST%) used together to identify employees in the East building

Query in three parts and the output it gives

![image](https://github.com/resii-tech/SQL-Lab/assets/129999089/a29b53cc-061e-4f55-8a1f-3147a6a2dc03)

## Employee Updates — Sales and Finance Departments

The security analyst needs to perform a different security update on machines for employees in the Sales and Finance departments. We will use filters in SQL to create a query that identifies all employees in the Sales or Finance departments. (The department of the employee is found in the department column, which contains values that include Sales and Finance.)

Screenshot

![image](https://github.com/resii-tech/SQL-Lab/assets/129999089/6b945e28-1740-421c-9142-cd1daf6b01e2)

Command

```sql
SELECT *
FROM employees
WHERE department = 'Finance' OR department = 'Sales';
```

Breakdown

* ```SELECT *``` — * is a wildcard which indicates you will select all
* ```FROM employees``` — specifies which dataset we will be pulling from
* ```WHERE``` — clause (WHERE) used with OR to filter for employees who meet either criteria (sales or finance) from department column
* ```department = ‘Finance’``` - first condition, specific department (finance)
* ```OR department = ‘Sales’;``` - OR operator and second condition, specific department (sales)

Query in three parts and the output it gives

![image](https://github.com/resii-tech/SQL-Lab/assets/129999089/f0a74c3e-aec3-4a0e-9be3-d70db7bd0125)

## Employee Updates — Not the Info Tech Department

The security analyst needs to make one more update to employee machines. The employees who are in the Information Technology department already had this update, but employees in all other departments need it.

We will use filters in SQL to create a query which identifies all employees not in the IT department. (The department of the employee is found in the department column, which contains values that include Information Technology.)

Screenshot

![image](https://github.com/resii-tech/SQL-Lab/assets/129999089/3ed82777-0fb1-4dfc-880b-59e52b71fd6d)

Command

```sql
SELECT *
FROM employees
WHERE NOT department = 'Information Technology'
```

Breakdown

* ```SELECT *``` — * is a wildcard which indicates you will select all
* ```FROM employees``` — specifies which dataset we will be pulling from
* ```WHERE NOT``` — clause (WHERE) used with NOT to filter for employees who are not in a specific department (information technology)
* ```department = ‘Information Technology’;``` - specifies department in query being used to be Information Technology

Query in three parts and the output it gives

![image](https://github.com/resii-tech/SQL-Lab/assets/129999089/fc57f634-fe1d-4690-b24b-06022bab3ccd)

## Summary

In this lab we were able to demonstrate how a security analyst would use SQL filters and operators to form queries that pulled from two tables in order to access records from different datasets and investigate potential security risks or update employee computers.

The two tables used were log_in_attempts and employees. The operators used to filter for specific information were AND, OR and NOT. The filters used to form patterns were LIKE and the % wildcard.

###  Takeaways

- [ ] SQL can be used for precise data extraction
- [ ] SQL can be used for data analysis
- [ ] Data analysis adds contect to logs and database information needing to be parsed and interpreted
- [ ] SQL maintains versatility and relevance as an important cybersecurity tool
