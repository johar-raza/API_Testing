# API6:2019 Excessive Data Exposure

#### **What is Mass Assignment?**

Applications these days often rely an objects (For example user, product, ...) and these objects have properties (for example product.stock). As a user, we have the authorization to edit and view specific properties of the objects but we might also be limited and not able to edit or view some specific properties (For example user can view product.stock but user should not be able to edit product.stock). These properties are then matched to parameters on the front-end and if these conversion happen automatically, they might convert parameters to properties the attacker should not be able to access (For example, the user should never be able to edit product.title but the front-end might convert a paramter "title" to product.title if the user sends a PUT request).

Here are some more examples of properties the user should not be able to edit:

-   Account.AccountType or Account.discountsEnable. These are properties that relate to permissions.
    
-   Account.wallet This property should never be editable be the attacker
    
-   product.title These are internal properties the user should never be able to edit
    

#### **Example Attack Scenarios**

These attack scenario's come from my personal experience as a bug bounty hunter. This happens to be my favourite issue types because it's really to miss them and you can not automate the search easily due to the required logic knowledge.

The first example is from an application that allows you to make an appointment which last 1 hour. In the UI I am able to see timeslots that last 1 hour and i can select one of them.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/114e92b5-67b2-4797-972b-4cc063c2e1e6/Untitled_Diagram_(6).png](https://www.udemy.com/course/uncle-rats-api-security-testing-guide/learn/)

I was staring at the request and did not notice it at first, it took me a good hour to realise it:
```
1.  {
2.  "startDate":"29/04/2022 11:00",
3.  "endDate":"29/04/2022 12:00",
4.  "userID":"123",
5.  "consultantID":"123"
6.  }
```
If we change the start or end date we can fully book the agenda of the consultant for years if we wish. This is very easy to miss which is why i like this issue type so much!
```
1.  {
2.  "startDate":"29/04/2022 11:00",
3.  "endDate":"29/04/2099 12:00",
4.  "userID":"123",
5.  "consultantID":"123"
6.  }
```
A second example is that i love to look at my account settings when i am hacking to see if i can't find any properties in the api responses that I should not be able to edit to make myself admin for example but this is about a much more subtle bug.
```
1.  Request:
2.  {
3.  "endDate":"29/04/2099 12:00",
4.  "userID":"123",
5.  "consultantID":"123"
6.  }

8.  Response from API:

10.  {
11.  "AccountType":"airport",
12.  "endDate":"29/04/2099 12:00",
13.  "userID":"123",
14.  "consultantID":"123"
15.  }
```
There are two account types in here, these account types can not be found in the requests themselves but only in the respones however if i copy over that parameter, i might be able to change it as the API might automatically map the object.
```
1.  Request:
2.  {
3.  "endDate":"29/04/2099 12:00",
4.  "userID":"123",
5.  "consultantID":"123"
6.  "AccountType":"airline",
7.  }

9.  Response from API:

11.  {
12.  "AccountType":"airline",
13.  "endDate":"29/04/2099 12:00",
14.  "userID":"123",
15.  "consultantID":"123"
16.  }
```
A second subtilty that comes into play is that we need know that it's a pretty bad thing to change account types but looking at the website (which was not in scope but it's a public asset, [www.FAKEPAGE.com](http://www.FAKEPAGE.com) ) on that page we found that one account type costs a lot more than the other thus increasing the impact.

#### **Preventive measures against** Mass Assignment

-   You should try to disable the automatic mapping of properties whenever possible, all properties should be mapped manually and others should be ignored.
    
-   You should not rely on a blacklist to block the data that the user is not allowed to edit but instead you should rely on a whitelist that enables users to edit specific objects. This will be more resource intensive though as you have to have a really good overview of the logic and properties of the application but knowing this brings a lot more security to the table is a strong argument for the added cost.
    
-   Rely on the functions of the framework when blacklisting or whitelisting instead of writing your own. These functions have been tested many times over so they are more likely to be safe than a custom built solution.
    
-   If you have the resources, you should investigate the request and responses that reach the API and what properties they can have. Test if parameters the user can not edit are actually read only by sending them in a request and trying to edit them.
    

#### Conclusion

As you can see you need to think very carefully about the function of every parameter and make sure you understand what it means and what all the options are with that specific property. Make sure you investigate all the properties the objects have on the API and that unused properties do not get sent to production where they might get abused.