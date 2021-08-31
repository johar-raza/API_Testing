# API7:2019 Security Misconfiguration

#### **What is Security Misconfiguration?**

There are several factors that might indicate a Security Misconfiguration. We should be very careful with handling configurations because if the correct security measures are not in place to protect our APIs, an attacker might be able to take over the full infrastructure.

We have to ensure all our systems are always up to date to avoid old exploits working on our systems. Following up on this, if our systems are up to date, we have to disable any unused functions like http PUT calls.

All our data should always be transported over a TLS channel to avoid that an attacker can perform a MitM attack.

Make sure all your security headers such as CSP are working correctly and configured wherever needed. With CSP enabled we should also run a CORS policy and configure it properly or you might open yourself up to **Security Misconfiguration** vulnerabilities.

Furthermore as a last point we can claim that security misconfigurations happen when the end user is able to see error messages or warnings. These should only be logged and viewed internally.

#### **How to detect Security Misconfiguration?**

First of all, it is really important that we look get an overview of our entire application architecutre. We need to gain visibility by creating a mindmap or a schema.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/46cc7f42-f6dc-49ba-a69b-9c527a8ebbcc/Untitled_Diagram_(7).png](https://www.udemy.com/course/uncle-rats-api-security-testing-guide/learn/)

Make sure you include everything in here, from printers to smart thermostats and mobile phones. These days you should even include watches if your company allows smartwatches to connect to the network.

Now that we have everything mapped, we need to scan it for security misconfigurations, we can do this manually or with a scanner. To do this manually, go over all the assets, workflows and everything else you gathered and confirm they are configured correctly. You are looking for things like passwords sent over plain text or unencrypted communication on server x which would indicate **Security Misconfiguration**.

#### **Hybrid-cloud environments and Security Misconfiguration**

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0e343f23-b9b2-4af6-8c7c-3a84098e78c0/Untitled_Diagram_(8).png](https://www.udemy.com/course/uncle-rats-api-security-testing-guide/learn/)

A lot of people are against hybrid environments but it's been given a bad rep due to the misconfigurations that are often present on these environments with improper network protection and execution. This can often lead to vulnerabilities, we will go over a few of them.

First of all it is really important to encrypt all the traffic that moves over the network to give attackers who perform a man-in-the-middle attack no chance to eavesdrop on the data being transmitted.

If we want to set up a complex environment like this, we have to perform a really thorough risk analysis as well to make but this is often overlooked or not executed due to budget constraints. It's really important to have rigerious risk assesment in place and evaluate it from time to time.

What is also vital is proper security management is implemented. Sometimes shortcuts will be built in for testing purposes that make it to the production environment, this should never happen but also the intended authentication methods should be strong and should not allow for attacks.

Our different cloud envirnoments need to be coordinated carefully and we need to assure the fall within compliance rules. This is extra important in hybrid-cloud environments where two clusters might need to communicate at any time.

There are many more things we can do to protect our environment, make sure you always define proper SLAs and do not leak data that should not be leaked.

#### **Example Attack Scenarios**

I am again going to draw from experience here as i think it's an interesting avenue that should give you a better idea of how this issue manifests in a real production environment.

I was testing and i found a manual that told me i could not disable the super admin accounts, that's enough for me to go and try if this actually is true. As a tester, i have to verify everything and i can not make assumptions
```
1.  DELETE /users/
2.  {
3.   "id":123
4.  }

6.  Where user 123 is a super admin
```
That was the easiest 125 euro's i ever made.

Another example that comes to mind is when i found a server which had their HTTP configured improperly which had disasterous consequences. Due to anonymous PUT file upload, i was able to upload an exploit that gave me a reverse shell via PHP. POST was disabled properly but they forgot the PUT method.
```
1.  POST /files
2.  {
3.   "title":"test.php"
4.   "binary":"BINARY FOR FILE"
5.  }

7.  Returns
8.  403 - forbidden

10.  PUT /files
11.  {
12.   "title":"test.php"
13.   "binary":"BINARY FOR FILE"
14.  }

16.  Returns
17.  200 - ok
```
I can then easily call my file from the browser.

GET /files/test.php

Where test.php is a reverse shell to the server. HTTP Put method should have been disabled but it was not. This is a clear **Security Misconfiguration.**

#### **Preventive measures against Security Misconfiguration?**

-   To be safe, we have to judge and adjust the security on a regular basis both on it's own and when put into a production environment to prevent **Security Misconfiguration**
    
-   We have to perform a regular review of our API's configuration files while keeping the whole technology stack that has been used in mind.
    
-   Never use insecure channels of communication and make sure you judge this keeping all communications in mind, especially in **Hybrid-cloud environments**
    
-   Automation can help us in detecting common **Security Misconfiguration,** Design and implement a proper strategy to automatically scan your APIs.
    
-   We can use scheme based validation on the responses of the API to validate they meet the criteria and do not send out misconfigured information like full error messages.
    
-   Don't allow any HTTP method that is not needed for your API, you should work with whitelist based filtering wherever possible.
    
-   If your API needs to be accessed from browser-based clients we have to consider implementing a proper Cross-Origin Resource Sharing (CORS) policy.
    

#### **Conclusion**

**Security Misconfiguration** vulnerabilites are really easy to overlook while testing manually so it's always adviced to combine the manual testing with the automated testing because popular scanners can detect and report on common **Security Misconfiguration** vulnerabilites.