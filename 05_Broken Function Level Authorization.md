# API5:2019 Broken Function Level Authorization

#### **What is Broken Function Level Authorization?**

From the summary you might have been able to gather this vulnerability can be quite complex and varied. Humans are predictable in nature and they tend to follow several patterns when it comes to hosting endpoints, for example it usually occurs that higher priviledge endpoints are hosted on the same relative path, which makes it easier for the users to guess these endpoints but that alone does not make the vulnerability though. We have to be able to access to endpoints as a lower priviledged users as well of course. This access can occur by executing the provided HTTP method (for example POST) but it can also occur on other methods that the developers left enabled (un)intentionally (for example DELETE). These issues are often made worse by how predictable the endpoints are (for example /books?id=0 might indicate the presence of an endpoint /books/all which may print out books the user should not be able to access.).

#### **Example Attack Scenarios**

In our first example we will be looking at a target which contains two types of accounts: Franchise holder and a company looking to exploit the franchise. Both log into the same URL but the Franchise holder type of account is much more expensive. However by going to the account settings and saving, we notice a hidden parameter "Account type" and we change it from "exploitant" to "franchise_holder". This allows users to buy a lower priced type of account and change it to higher priced account type themselves later on.

Example:
```
1.  POST /api/saveSettings.php

3.  [
4.   {
5.   "Name": "677", 
6.   "...": "AAAAA...AAAA"
7.   "AccountType":"exploitant"
8.   }
9.  ]

11.  We change this body to 

13.  POST /api/saveSettings.php

15.  [
16.   {
17.   "Name": "677", 
18.   "...": "AAAAA...AAAA"
19.   "AccountType":"franchise_holder"
20.   }
21.  ]
22.  and by sending this request, the users can upgrade their own accounts to a more expensive account type.
```
A second example we can add is where users try to view documents however they can only view their own. By exporting their own documents though, the user is presented a URL /api/documents/export. This URL does return the proper documents and only the only belonging to the user but by simply guessing, the user discovers a URL that returns all the books on /api/documents/export/all.

```
1.  GET /api/documents

3.  Will return the following response

5.  [
6.   {
7.   "id": "1", 
8.   "content": "AAAAA...AAAA"
9.   "creationDate":"10-10-10"
10.   },
11.   {
12.   "id": "4", 
13.   "content": "AAAAA...AAAA"
14.   "creationDate":"10-10-10"
15.   }
16.  ]

18.  Since this user can only see their own documents, they can only see those 2, however if they export the documents, the user notices they can change the path and export all the documents.

20.  GET /api/documents/export/

22.  will return the same as before, however the following:

24.  GET /api/documents/export/all

26.  exports all the documents.
```

#### **Preventive measures against Excessive Data Exposure**

It's always better to seperate the authentication and authorization module from the main code and make it easy to implement where needed. By default all access should be denied to make it safer, we can also work on blocking the traffic we do not want but this would make it easier to miss certain paths of attack.

Besides this we should conduct regular and thorough testing and analysis of the autorization module while we keep the logic of the application and the different user levels in mind.

To make things extra secure we can implement a master admin class that implements the main authorization modules as mentioned above. We can use this master admin module to inherent from all other administrative functions to limit the chance of administrative functions being executed by lower priviledge users.

Lastely we always need to keep 2 factors in mind:

-   The users group
    
-   The users authorization level
    

And we have to make sure that with those two factors checked, only administrators can access administrative functions.

To test for this, it is wise to at least partially rely on test automation due to the preditcable nature of APIs and how they are structured. A potential test could be to run the OPTIONS method on every API endpoint and to make sure those API endpoints do not allow for any unexpected method. We can also automate the checks of user roles and groups, this requires a proper definition of all the user roles and groups with their access levels.

All parameters returned by the API should be indexed on a regular basis and tested to ensure we can edit properties we are not supposed to as a user which could elevate the accounts priviledge level.

#### Conclusion

There are three main ways to abuse this vulnerability, being by manipulating the URL or by manipulating certain parameters and even by manipulating the HTTP method. The consequences of this vulnerability are almost always severe and lead to vertical priviledge escalation on the API level. With all these factors in mind, we should pay close attention to not let any of these issues slip through the net and land in production.

It is wise to at least partially automate this search but to keep manual testing involved as it's not easy have a robot guess certain URLs. Test automation however can perform directory brute forcing to ensure no API endpoints have been missed that allow for administrative functions.