# API Security - Top 10 Best Practices

#### Introduction

I love drawing inspiration from real life and todays article is no different. I often get asked the question on how to hack an API but what some people don't realise is that almost everything is connected to an API these days, even the smart fridges in our homes that pass on our groceries to the store have to pass that data to somewhere. Today i would like to take a moment to show you my top 10 best practices in API testing. To do this however we first need to know exactly what an API is.

#### What is an API?

People often hear the story about how many invisible waves have come to cover us such as WiFi and microwaves but those stories often neglect that with the dawn of the internet, engineers needed a way to make our fridges play flappybird and pass along the highscore to a server so that everyone can see how little of a life we have.

All jokes aside, API’s are everywhere around us. API stands for Application Programming Interface, now i don’t know about you but those words needs some analysing for me. In the core an API seems to be something that allows two applications to talk to each-other and if we define it like that, we can see why API’s are so broadly distributed. Not only do all our websites talk to an API these days but pretty much anything that is connected to the internet is dynamic and thus needs to talk to an API.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e2ae6c12-11c4-40c7-9af6-52f9f8aa8396/What_is_an_API_(1).png](https://www.udemy.com/course/uncle-rats-api-security-testing-guide/learn/)

#### SOAP vs REST

It's important to know the distinction between the SOAP and REST as both desire a slightly different approach to security. SOAP stands for Simple Object Access Protocol whereas REST stands for Representational State Transfer. SOAP was designed first followed by REST and we can state that SOAP is a protocol whereas REST is an architectural pattern. An architectural pattern is a general, reusable solution to a commonly occurring problem in software architecture within a given context while a protocol is an established set of rules that determine how data is transmitted between different devices and the API.

This is a very technical explanation which boils down to the fact that SOAP only works with XML formats and REST will work with plain text, XML, HTML and JSON. We can also invoke the well known HTTP verbs of GET, POST, PUT and DELETE for working with the required components. In SOAP we have a WSDL document that will contain all the specifications we need to talk to the API whereas REST will rely on the URLs to find the correct data.

A big advantage of REST is that it will often use less bandwith as it usually uses JSON where SOAP is constraint to XML which contains a lot of bulky extra's describing the document.
```
1.  {"name":"Actor","Country":"Belgium"}

1.  <?xml version="1.0"?>
2.  <SOAP-ENV:Envelope 
3.  xmlns:SOAP-ENV
4.  ="<http://www.w3.org/2001/12/soap-envelope>" 
5.  SOAP-ENV:encodingStyle
6.  =" <http://www.w3.org/2001/12/soap-encoding>">
7.  <soap:Body>
8.   <Demo.thexssrat.com
9.   xmlns="<http://thexssrat.org/>">
10.   <name>Actor</name>
11.   <Country>Belgium</Country>
12.   </Demo.thexssrat.com>
13.   </soap:Body>
14.  </SOAP-ENV:Envelope>
```

It's very important to note that REST is a stateless architectural solution which means there can no proper information flow from one function to the other. If we need to have that we need to use a SOAP protocol.

#### Key API security issues facing organisations today

Companies have already made great steps in securing their infrastructure in recent years but they still have a long way to go. One of the key threats that loom over organisations these days is the lack of information itself. A program will often consist of many micro api's that all communicate with one another and often security gets pushed to the back or overlooked entirely. There is a choice that companies have to make and it can be a thin line they have to walk.

We have to make sure that we spend enough time on our security infrastructure but at the same time it can become a real big burden if we don't manage this properly. We need to keep a current overview of our full API structure and all of the possible attack avenues while also making sure that we don't put any more stress on the system and it's components than it can handle. We need to know which API's are active and what endpoints they expose so that we can keep an overview of the possible attack vectors into our systems. This is no easy task and it can easily overwhelm us if we need to do it manually.

#### Common Attacks Against Web APIs

#### Injections

One common type of attack against API's is injections. This can be anything from command injections to SQL injections and the impact can range from trivial to critical. Several different types of injection attacks can be very harmful for a server and while the API is usually not the weak point that executes the attack, rather passing it on to other systems where the attack is executed, it's often the first line of defence against an attack from the outside. Some things we have to watch out for are:

-   SQL injections
    
-   HTML injections
    
-   Code injections
    
-   Command injections
    
-   Content injections
    

#### Cross-site scripting (XSS)

While it's true that XSS is often executed on the clients computer, the API is often the first line of defence when it comes to stored XSS attacks which can be very damaging. These attacks allow the bad actor to execute malicious (often javascript) code and it's the API's job to filter out these malicious values but we all know how easy it is to forget one and the bad actors only need that 1 chance to strike.

#### Distributed denial-of-service (DDoS)

DDoS attacks can be devastating as they cripple your application, system or even whole network. A DDoS attack is executed by flooding the system with more requests than it can handle and usually by many different "zombies" as we call them. A zombie is an infected computer that is being controlled by the attacker and will be used to send out requests along with all the other "zombies" to our system. This can easily overwhelm even the most robust systems if enough traffic is generated.

#### Lack of Resources & Rate Limiting

This draws into our previous example which can be made even worse by not implementing proper rate limiting. This would allow the attacker to cripple our systems even faster than before by requesting unreasonable amounts of data in a very short timeframe.

#### Improper Assets Management

Some of the biggest threats come from that what is not known. A lot of organisations will put heavy emphasis on the security of the visible components in their system but completely loose sight of shadow API's. A shadow API is an API that is in production but is not known to anyone. These API's can cause great damage as they are not actively maintained and often overlooked when creating new features which could lead to unauthorised access of data for example or even worse, remote code execution.

#### Top 10 best practices

So after knowing all of this you might be a bit worried but with these best practices you can be sure that your security will increase by many factors.

#### Use a strong authentication and authorisation solution

It often occurs that broken API's do not properly check that the user is authorised to execute they action they want to take or that are even authenticated at all. This can cause major problems like data leaks by IDOR's and BAC. Since API's often form the gate to the companies data, which is arguably one it's most valuable assets, we need to be extra cautious in that we do not expose anything to the wrong people.

#### Prioritise security

I often hear the argument that security is not foreseen in the current budget but that makes me wonder what the budget is for a real data leak. The costs of a proper attack can reach many times that of the costs of proper security measures and it's important we also take security into consideration and even put it in the first place when designing our system's requirements or building up the code.

#### Inventory and manage your APIs

As we've talked before, shadow API's can be a real problem. To combat this, make sure that you have a proper inventory of your API's and that this is regularly updated. This is best done by automatic tools which can inspect traffic and perform enumeration scans to discover uncharted API's.

#### Practice the principle of least privilege

The principle of least privilege states that we should only give our API's exactly as much access as it needs and no more. This ensures that even if our API's get hacked, the damage will be limited to the scope of that API and will not spread to other parts of our system.

#### Encrypt traffic using TLS

TLS stands for Transport Layer Security and is the successor to SSL which you may be more familiar with. It allows us to encrypt traffic in a very secure manner which does not leave any more room for man-in-the-middle attacks where the bad actors try to read the traffic between our front-end systems and our back-end API's.

#### Remove information that’s not meant to be shared

When API's are developed we have to pay special attention to the fact they can contain secrets which are a much sought after commodity for hackers. Things such as API keys, usernames and passwords should always be put into secure storage solutions such as environment variables. This step is often overlooked which is why it pays off to automate this process. Besides scanning items before they are uploaded into code sharing solutions such as repositories we can also scan those repositories periodically to ensure no values got out from under our watchful eye.

#### Don’t expose more data than necessary

Developers will often expose more data than is strictly needed for debugging purposes in testing environments but we have to ensure all these extra values are removed before entering a production environment. As an ethical hacker i can ensure you that information is a very valuable commodity in our world and the less data is exposed, the less of an attack surface we leave exposed for bad actors.

#### Validate input

One of the biggest reasons vulnerabilities happen in production environments is because developers forget to validate and sanitise input. We should always have redundant validation systems and never rely on the front-end systems to block off invalid input. As an ethical hacker i do not care very much about front-end systems unless it comes to XSS or CSTI. I am talking to an API when I am hacking and i always say that front-end validations only serve to protect the users from their own mistaken input.

#### Use rate limiting

This issue type is by far one of the most reported issue types out there in bug bounties which means it is often missed in penetration testing. Improper rate limiting or lack of any rate limiting at all can cause our applications to go offline because a bad actor might be requesting too much data or they might overload our API's by having them process too much information. If we use third party services this can even lead to extremly high bills being racked up by bad actors trying to execute as many requests as possible to the third party systems which might bill per x number of requests.

#### Use a web application firewall

One of the most effective ways to stop bad actors is a WAF. This is a type of firewall that focusses on only inspecting web traffic and they are often based on rulesets which will filter out traffic that is not allowed. This can be requests with attack vectors in them for example. While a WAF is a very effective technique to stop bad actors, it should never be relied upon fully and we should still keep into account all of the things we have already talked about.

#### Best Practices in 2021

#### Secure backend data as well as frontend data

Organisations spend a lot of resources on securing their front-end data but they often neglect their back-end data. We need to ensure that we redundant checks in place that not only protects the visible parts of our application but we also need to ensure our backend systems are secure as bad actors might bypass our front-end completely and talk to an API directly.

#### Secure the request-response lifecycle through validation

Attackers will often target the discrepancies between requests and responses. For example a response might contain a field that the request does not and the attackers might be able to copy that field over to the request and manipulate its value. By doing this bad actors might cause serious damage when the manipulate values of high impact (for example isAdmin:false > isAdmin:true). For this reason we have to reject any requests that do not contain the exact bodies and headers that we expect on our API's and we have to be very strict about it.

#### Hash passwords

When an attacker does gain access to your system you want to put everything in your power into making sure they don't get to know the real password of the users. The best way to avoid this situation is to hash the password and even better is to add a salt before hashing the password. A salt is a simple string that will ensure attackers can not compare our hashes to existing rainbow tables. This technique is one of the techniques that helps preserve the data on your API to a great extend.