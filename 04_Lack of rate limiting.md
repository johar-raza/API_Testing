# API4:2019 Lack of rate limiting

#### **What is a lack of rate limiting?**

Whenever an API is served a request it will have to respond, to generate this response the API requires resources (CPU, RAM, network and at times even disk space) but how much are required highly depends on the task at hand. The more logic processing happens or data is returned, the more resources are taken up by a single call and this can stack on quick. If we do not rate limit our API endpoints. This issue is made even worse by the fact that most API's reside on shared hosts which means they are all fighting for the same resources which could mean the attacker is disabling a secondary unrelated API by consuming all the resources.

#### **Example Attack Scenarios**

There are simple examples of attacks related to lack of rate limiting on endpoint but those are easy enough, a somewhat deeper attack could be a user who discovers the endpoint to create a file which does have rate limiting and an endpoint to copy a file does not have rate limiting. At first this might seem hard to abuse but if we create a document on the system that has a large file size and then copy it over, we might trigger the server to run out of resources.

Example:
```
1.  POST /createDocument.php

3.  [
4.   {
5.   "Name": "677", 
6.   "Content": "AAAAA...AAAA"
7.   "fileSize":"21343243242343kb"
8.   }
9.  ]

11.  With a response of the ID:

13.  id=12

15.  If we try to trigger this call multiple times we will notice rate limiting on the endpoint.

17.  GET /cloneDocument.php?id=12

19.  But there might be a GET call which is not rate limited and by triggering it multiple times we might consume all of the server's resources.
```

Let's add another example to make things more clear. We might be trying to recall the last 100 posts to a blog with the following URL

GET /api/v1/posts?limit=100

By executing this request with a parameter of limit=99999 we might trigger a lack of resources as well and this is also counted as lack of endpoint rate limiting.

#### **Preventive measures against Excessive Data Exposure**

-   Using a docker container can be very handy as it has built in flags such as -m to determine exactly how much memory every container may consumer. This also allows for a much easier seperation of different APIs.
    
-   We should make sure the client can only make a certain amount of request over a certain period, If we do this however we have to make sure that the client is notified when a rate limit is triggered or about to be triggered. This message should contain the amount of remaining requests before the limit is hit and/or the remaining time of the rate limit
    
-   We need to verify on the client and server side that the request body and response are not too big. This is especially true for endpoints that return an amount of records specified by the client. The client can manipulate the amount of requests and cause a severe issue to occur if they request too many records at a time.
    
-   Every data type that the endpoint accepts should have an upper limit, for example integers should be limited to 5999. This ensure we never expose too great of a volume of data.
    

#### Conclusion

This again deceptive vulnerability is easy to overlook but can be a bit easier to automate as all we have to do is check all the API endpoints and see if they enfore a maximum size to the input or ouput, this requires a good understanding of what the APIs should accept or return. Better yet, good documenation helps idenitfy issues easier which costs less in the long run.