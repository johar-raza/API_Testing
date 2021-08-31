# API8:2019 Injection

#### **What is Injection?**

API's with the following properties are open to injection flaws:

-   When we don't sanitize the input from the front-end we are opening ourselves to a world of problems, this would allow the user to input anything which could intervene with later processes.
    
-   The same goes for validation and verification of the API's request, these have to be done before the data enters any kind of processes, this is where it could start causing problems for example on the login calls, make sure the API is validating, verifying and sanitizing any input.
    
-   Make sure that you pay attention to not just the input from the user directly, this can also come from 3rd party services or file uploads for example.
    
-   Take into account there might be processes such a batch jobs running that could trip over the data.
    

#### **Example Attack Scenarios**

I can also draw from experience on this vulnerability type as i have and reported on it often. The form i prefer is SQL injections via the import feature and OS command injection from an unexpected source. Let's start with the SQL injection.

I found this issue while doing bug bounties on a private program and it happend because the developers did sanitise all the direct input very well, except they did not think to include the import functionality because the base of the application was built a year prior to building the import functionality.
```
1.  Upload.csv looked like this: 
2.  name,adress,email,phone
3.  ',',','

5.  And while uploading i selected the comma as field seperator, this displayed a SQL error and from there on i dug in deeper.

7.  The error was:
8.  Expects paremeter 1 to be string, null given in /var/www/html/import.php
9.  This made me close the query and start a new one of my own
10.  name,adress,email,phone
11.  ';select * from users;--,',','
```
This made the application dump the entire uses table in an error message, that was enough for me to report this issue and collect my bounty.

The second example i have is a little bit less complicated, i noticed a parameter litteraly called "osParam" which seemed to have some flags in it, i rushed to start up burp suite intruder with a list of command injections i had prepared before and had a hit on the 9th request that burp suite made. The command seperate was a newline character '\n' and my ping command delayed the response.
```
1.  index.php?osParam=\\nping -c 10 127.0.0.1 
```
So i quickly tried a whoami, reported the result and awaited approval which cames 2 days later.

#### **Preventive measures against Injections?**

-   We need to treat any input as being compromised and we should filter, validate en verify every input to our API through all ways, this includes third party inputs or non-direct inputs such as importing files.
    
-   We have to make sure to create 1 system that will be able to perform these steps and that we implement and use that same system across all of our endpoints.
    
-   Prefereably use a well known library that has been tested instead of creating your own.
    
-   We have to take care in filtering special characters, often the language we use has a specific syntax and way to handle these special characters and it's adviced to implement that syntax.
    
-   Whener multiple records are being selected, limit the number of records per query to avoid mass disclosure
    
-   Preferably use a specification that specifies how the API works such as OpenAPI and that you only allow requests that match the filters, verification and validation rules
    
-   Define what the API can expect for all the string endpoints and in terms of variable types.
    
```
1.  An example in php which is badly implemented and leaves the app open for things such as XSS 
2.   $id = $row['id'];
3.   $title = $row['title'];
4.   $des = $row['description'];
5.   $time = $row['date'];
6.  This is how it's supposed to be done
7.   $id = htmlentities($row['id']);
8.   $title = htmlentities($row['title']);
9.   $des = htmlentities($row['description']);
10.   $time = htmlentities($row['date']);
```
![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8c795b8c-5106-491d-89e3-5549d2b37bb7/Untitled.png](https://www.udemy.com/course/uncle-rats-api-security-testing-guide/learn/)

#### **Conclusion**

This issue type is diverse and not always easy to automatically test, we have to keep a good overview of every endpoint, including the ones that are less obvious. Manual testing is still adviced to compliment the automated tests in areas where an automated test is falling short.

Taking every endpoint into account can be quite confusing as it includes finding all the hidden parameters as well. New API endpoints should already be added to the documentation and old endpoints should be indexed whenever possible.