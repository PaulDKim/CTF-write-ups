# Lab12: Visible error-based SQL injection
* url: `https://portswigger.net/web-security/sql-injection/blind/lab-time-delays`
* vulnerability: `Time-Based Blind SQL Injection`
* cheatsheet: `https://portswigger.net/web-security/sql-injection/cheat-sheet`


## Description 
This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows or causes an error. However, since the query is executed synchronously, it is possible to trigger conditional time delays to infer information.

To solve the lab, exploit the SQL injection vulnerability to cause a 10 second delay. 

## Proof of Concept
1. I have no idea what kind of DBMS I am dealing with, so it's useful to look at Portswigger's SQLi Cheat Sheet
to look at the different payloads for Time-Based Injections and fuzz the application until we're able to see a 
delay in the web application response. 
2. It's good to have a general idea of what the backend query looks like. From playing around with the web 
application I can assume that the query looks like: `SELECT * FROM tracking-table WHERE tracking-id='q6C7mS2AyYUMiYKb'`
3. So, like before I can try to create a payload that fits the query that's used in the backend: 
`' || (SELECT SLEEP(10)))-- -`
4. I can test out each of the payloads listed in the `notes` section. It looks like `' || (SELECT pg_sleep(10))`
works to solve the lab!

## Notes
* Oracle: `dbms_pipe.receive_message(('a'),10)`
* Microsoft: `WAITFOR DELAY '0:0:10'`
* PostgreSQL: `SELECT pg_sleep(10)`
* MySQL: `SELECT SLEEP(10)`

* Even though `q6C7mS2AyYUMiYKb` is a string and `pg_sleep(10)` is not, the injection still works because PostgreSQL
allows the `||` operator to concatenate a `string` with any `expression`, and it doesn't require the concatenated
expression to be a string. 
  * `pg_sleep(10)` does not return any data to the query, so it's effectively a no-op that causes the database 
  to sleep for the specified time but doesn't `alter the logic` of the SQL query directly. 
