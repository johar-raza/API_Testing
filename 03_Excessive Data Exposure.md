# API3:2019 Excessive Data Exposure

#### **What is Excessive Data Exposure?**

An API is only supposed to return the required data to the front-end clients but sometimes developers will make a mistake or take the easy route and implement generic API's that return all data to the client. When these API's return too much data, we can speak of **Excessive Data Exposure.**

#### **Example Attack Scenarios**

A simple example we can give is an application which makes a call to grab the credit card details. The user does not see the CCV because it will be filtered out by the front-end client but the API still returns too much data.

Example:
```
1.  GET /api/v1/cards?id=0

3.  [
4.   {
5.   "CVV": "677", 
6.   "creditCard": "1234567901234", 
7.   "id": 0, 
8.   "user": "API", 
9.   "validUntil": "1992"
10.   }
11.  ]

13.  As you can see here, we made the call to grab the credit card details and while the end user might not be able to see the CVV but since the API returns it, we are speaking of Excessive Data Expsoure.
```
![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d98d88f3-e672-47fb-9bdb-bb9dc8178813/Untitled_Diagram_(2).png](https://www.udemy.com/course/uncle-rats-api-security-testing-guide/learn/)

Let's add another example to make things more clear. In this scenario we have a mobile application that sends a request to /api/articles/{articleId}/comments/{commentId} and gets metadata about the comment as well, including the author. However when the attacker is sniffing the data, he can also see PII data from the author.
```
1.  GET /api/v1/comments?id=0

3.  [
4.   {
5.   "comment": "1234567901234", 
6.   "id": 0, 
7.   "user": "testUser", 
8.   "user adress": "testlane, testing - 340043 testing in testland", 
9.   "user email": "test@bla.com"
10.   }
11.  ]
API3:2019 Excessive Data Exposure
```

#### **What is Excessive Data Exposure?**

An API is only supposed to return the required data to the front-end clients but sometimes developers will make a mistake or take the easy route and implement generic API's that return all data to the client. When these API's return too much data, we can speak of **Excessive Data Exposure.**

#### **Example Attack Scenarios**

A simple example we can give is an application which makes a call to grab the credit card details. The user does not see the CCV because it will be filtered out by the front-end client but the API still returns too much data.

Example:
```
1.  GET /api/v1/cards?id=0

3.  [
4.   {
5.   "CVV": "677", 
6.   "creditCard": "1234567901234", 
7.   "id": 0, 
8.   "user": "API", 
9.   "validUntil": "1992"
10.   }
11.  ]

13.  As you can see here, we made the call to grab the credit card details and while the end user might not be able to see the CVV but since the API returns it, we are speaking of Excessive Data Expsoure.
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d98d88f3-e672-47fb-9bdb-bb9dc8178813/Untitled_Diagram_(2).png](https://www.udemy.com/course/uncle-rats-api-security-testing-guide/learn/)

Let's add another example to make things more clear. In this scenario we have a mobile application that sends a request to /api/articles/{articleId}/comments/{commentId} and gets metadata about the comment as well, including the author. However when the attacker is sniffing the data, he can also see PII data from the author.

```
1.  GET /api/v1/comments?id=0

3.  [
4.   {
5.   "comment": "1234567901234", 
6.   "id": 0, 
7.   "user": "testUser", 
8.   "user adress": "testlane, testing - 340043 testing in testland", 
9.   "user email": "test@bla.com"
10.   }
11.  ]

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7c725a53-fadd-4271-b537-d4d8001bffac/Untitled_Diagram_(3).png](https://www.udemy.com/course/uncle-rats-api-security-testing-guide/learn/)
```

#### **Preventive measures against Excessive Data Exposure**

-   We should never rely on the client to filter out data
    
-   We should review all the responses coming from the back-end to see if they include sensitive data
    
-   When exposing a new API endpoint, engineers should always be wondering who the consumers of that data will be and exactly what data they need
    
-   There are certain generic methods such as to_json() and to_string(). These will return all the data that is fed into the function and can produce undesireable effects. We should opt for only returning specific properties of an object and never the full object itself fed into a to_json() or to_string() function.
    
-   All PII data your application works with should be classified and re-indexed on a regular basis. You should review all the API call responses and see if they do not contain any of this data without reason.
    
-   As an extra layer of security we can implement a scheme-based response validation, we need to ensure this validation defines and enforces all the data that's returned by the API.
    
      
    

#### Conclusion

The deceptive simple nature of this vulnerability makes it very easy to overlook and our automation is not very likely to pick this issue type up either so it's very easy to slip under the radar. It's highly recommended that you judge all data leaving API's on their sensitive nature and whether or not that data should be filtered client side of server side.
![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7c725a53-fadd-4271-b537-d4d8001bffac/Untitled_Diagram_(3).png](https://www.udemy.com/course/uncle-rats-api-security-testing-guide/learn/)

#### **Preventive measures against Excessive Data Exposure**

-   We should never rely on the client to filter out data
    
-   We should review all the responses coming from the back-end to see if they include sensitive data
    
-   When exposing a new API endpoint, engineers should always be wondering who the consumers of that data will be and exactly what data they need
    
-   There are certain generic methods such as to_json() and to_string(). These will return all the data that is fed into the function and can produce undesireable effects. We should opt for only returning specific properties of an object and never the full object itself fed into a to_json() or to_string() function.
    
-   All PII data your application works with should be classified and re-indexed on a regular basis. You should review all the API call responses and see if they do not contain any of this data without reason.
    
-   As an extra layer of security we can implement a scheme-based response validation, we need to ensure this validation defines and enforces all the data that's returned by the API.
    
      
    

#### Conclusion

The deceptive simple nature of this vulnerability makes it very easy to overlook and our automation is not very likely to pick this issue type up either so it's very easy to slip under the radar. It's highly recommended that you judge all data leaving API's on their sensitive nature and whether or not that data should be filtered client side of server side.