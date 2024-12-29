# Basic Injection
Vulnerability Found: SQL Injection (SQLi)

## Proof of Concept
1. On the default page of this lab, we can observe that it takes user input.  
It also presents the `Original Query` as well as the `Resulting Query`  
![Image description](images/basic-injection-home.png)

2. A good habit to have is to always check the source code for any hints.
We can see in the source code that it gives a helpful hint in the form of a comment.
Similar to the home page of the application, we can see the query used by the web app
It looks like it's searching for data in the `webfour` database in the `webfour` table
![Image-description](images/basic-injection-source.png)
