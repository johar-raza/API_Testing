# API9:2019 Improper Assets Management

#### **What is Improper Assets Management?**

We should always wonder for every API if all the current endpoint should even be aviable and if we maybe can't do with only allowing the whole API to communicate internally. To aid us we can ask ourself if this API even needs to be in production at all and who should have access to the API. If the specific API does needs to be in production, we can also ask ourselves if we are not using an outdated version of the API, if there is sensitive data exposed or how the data flows throughout the application and APIs.

A lack of documentation is a problem that plagues many companies and this only makes things worse because an undocumented API that does not send out traffic can remain undetected for a long time.

To ease the removal of older APIs we might consider a retirement plan for APIs that are no longer needed. Another tool we have at our disposal are inventory management systems which index every API and it's version, this can be used to perform a regular inspection of the API plan.

As long as these older APIs and their libraries remain unpatched, the system will be vulnerable to **Improper Assets Management.**

#### **Example Attack Scenarios**

Our first example will be a scenario that i encountered in production. I was scanning the APIs with a directory brute forcer to see if i could not find anything and unsurprisngly i could not find a thing. I noticed however that i was working on version 2 of the API from the URL:

1.  <http://WEBSITE:5000/api/v2/resources/books/all>

After replacing the v2 in the URL to v1 and restarting my directory brute forcing attempts i did find an admin URL

1.  <http://WEBSITE:5000/api/v1/admin>

The admin interface was protected in that it could only be accessed from the internal network but this was easily fooled by adding an "x-forwarded-from" header and setting it to 127.0.0.1.

Our second example comes from a third party vendor. For a new feature to work, the target had to include certain libraries which were using outdated APIs but due to imperfect asset management, our target did not have an overview of these APIs existance. These endpoints ultimately led to more functionality being exposed which had an RCE in it.

#### **Preventive measures against Improper Assets Management?**

We need to ensure we have a proper inventory management system set up that includes all the API endpoints with what is special about them, their environment and from which networks they are reachable. Also inventory third party integrations for these APIs, so we can have an overview of critical infrastructre that should be easy to consult by the people who need it.

We all know software is becoming more and more dynamic but with that also comes that we will have to review our inventory system on a regular basis and evaluate it.

Some interesting things to document could be:

-   Errors
    
-   Authentication flows
    
-   Rate limiting
    
-   redirects
    
-   CORS policy triggers
    

These days we can automatically generate the documentation from adopting an API specification such as Open API. This will make it much harder to miss a rogue API, however still not impossible. This documentation should be available to users of the API.

Besides what we just talked about, it really helps if you make use of external security measures such as an API firewall and we need to ensure that we install it everywhere that is exposed to production, not just the production APIs themselves; but also testing envirnments as they can sometimes be exposed to the internet, especially with the current pandemic and home work becoming more normal.

When it comes to data, it is important not to use production data on non-production enviornments and if it must be done, that we treat the endpoints with the same dilligent standard as the production APIs.

When newer version of an API are released, we should always ensure that a thourough risk analysis is done if that version contains security updates. We can wonder to ourselves what our next steps should be and how to implement them, for example if an update is really required, we might have to get it out to our customers immediatly or if there is no client impact to leaving out the security fix, if it can not be postponed.

#### Conclusion

It is incredibly easy to lose track of your APIs and what versions you are running where. I can not stress the importance of keeping track of every API enough as often attackers will be looking for an entry point which could easily get deployed or that might even still be deployed from a long time ago. These entry points could lead to a much bigger issue so it's wise to only enable what is needed in a production environment.