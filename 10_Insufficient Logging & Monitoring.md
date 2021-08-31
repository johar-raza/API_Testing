# API10:2019 Insufficient Logging & Monitoring

#### **What is Insufficient Logging & Monitoring?**

The title already says a lot but this vulnerability is a bit more complext than it was at first sight, of course the API is vulnerable if it does not create any log entries or when the log level is set incorrectly but we should not neg lect to also check whether or not the contents of the log messages are what is expected and that they contain enough detail.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aac804b9-e464-499d-9b2a-56ecf5924cd5/Untitled_(1).png](https://www.udemy.com/course/uncle-rats-api-security-testing-guide/learn/)

A second aspect that is a bit harder to control is the log integrity, for example a malicious user might insert a special character that when written to the log breaks it. This is known as log injection and it could nullify all the logging efforts made.

When we finally have a clear plan of what to log, when and on what environment we need to ensure we are monitoring these logs, failure to do so would result in a massive decrease in the efficiency of our logging efforts.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f778f845-0916-4ac2-90d5-bc0655bf4af3/Untitled_(2).png](https://www.udemy.com/course/uncle-rats-api-security-testing-guide/learn/)

Monitor everything, not just the logs but make you monitor the APIs and the infrastructure as well.

#### How does Insufficient Logging **&** Monitoring affect business?

The impact from a business perspective can be seen in several aspects. To start with confidantiality since often logs contain personal information that the victin would rather not share with the world. You really want to protect the logs from any attackers as this can have a severe impact.

For that same reason we should ensure that any data that goes into the logs is sanitised to the same level of scrutiny as other vulnerability types like XSS for example. A log pollution attack can nullify our attempts to log proper, sanitised and sufficient data.

In any case if we do not have sufficient logging and an attack does happen, we have no real way of tracing the call.

Though it may be important to log things, we should not go overboard as every log entry has a cost to it in the form of resources, this is why it's so important to pick exactly the right amount of logging. If you log too much or don't put rate limiting on logging endpoints an attacker could perform a DoS attack by consuming all the resources the server has to log their calls.

Lastely, should an external audit ever happen, we would have no way of proving our true intentions due to missing audit trails.

#### **Example Attack Scenarios**

A log entry might be in CSV form, if the attacker knows this, they might insert a malicious character such as a comma character ( ,) which the CSV log will interpret as a new column thus breaking the code and the logs. Especially if the logs are being monitored since this might break the monitoring tool as well which is expecting a certain format but it will have lines with too many columns.
```
1.  name,lastname,email
2.  test,test,test@test.com
3.  test2,test2,test@test.com
4.  test3,te,st3,test@test.com
```
A second example would be an attacker gaining access to a system and being able to get customers data for 9 years without alerting any of the monitoring tools. What's really scary is this happened a while ago ([https://healthitsecurity.com/news/insurer-dominion-national-reports-server-hack-that-began-august-2010](https://healthitsecurity.com/news/insurer-dominion-national-reports-server-hack-that-began-august-2010)).

#### **Preventive measures against** Insufficient Logging **&** Monitoring?

-   We should log any authetication calls and at a minimum failed calls. This can be a 403 forbidden status code but also any input validation.
    
-   How we log is also partially decided by the log management systems since the logs we create should be able to be consumed by said systems. The formats of our logs should match the expected format for the management solution.
    
-   Log should be treated with the utmost respect towards peoples privacy in mind, they should be kept safe and stranspoted safe.
    
-   Logging is needed but we should also set up a 24/7 monitoring system that monitors our logs, infrastructure and API endpoints. We should get an alert from this system if a breach occurs.
    
-   Security Information and Event Management (SIEM) systems can be used to aggregate logs from all components of the API technology stack and the virtual hosts.
    
-   The catch an attacker earlier, set up custom dashboards and alerting fit for your environment.
    

#### Conclusion

Insufficient Logging **&** Monitoring may not seem to be impactful at first but like with any issue type, if we look under the hood there is much more to be found. If there is not sufficient logging an attacker can roam freely and take their time and even if there was enough logging, that is no guarantee that there will also be monitoring to watch those logs. Given the severity of this type and what it can do though, it would not be wise to ignore logging and monitoring until the last minute and to bake it into the designs of the software that's created.