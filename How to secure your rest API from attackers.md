# How to secure your rest API from attackers

#### Introduction

API stands for application programming interface which basically means that we have an exposed interface that can be adressed programatically. As the internet becomes available in more and more locations around the world, the types of interfaces will become ever more prevalent. Securing an API can easily cost as much as the feature development itself and it can even cost more which is why are decided to write this article in the hopes of guiding you throughout this wild landscape and even though we are well aware that REST API's are not the only kind, they serve a specific purpose and we want to make sure the security risks are understood when implementing such a REST API.

#### What is a REST API

REST stands for Representational state transfer which means that it defines a programming architecture which uses the HTTP Methods. (GET/POST/DELETE/PATCH/...). Using these HTTP-method allows us to create our APIs independant of what host OS it will run on. The following poperties have to be fulfilled before we can speak of a RESTful API:

-   Works on the CLIENT-SERVER architecture where the clients use HTTP calls to communicate with the APIs.
    
-   RESTful APIs are stateless which means they do not account for already known information of the object that is being processed. These so called states will never be passed between requests
    
-   A RESTful API has to allow for caching of often used resources
    
-   RESTful APIs have a uniform way of communicating with other APIs
    
-   The variables are URI based and have to be identified with the URI, for example /invoices/1/print
    
-   They will contain Content-type headers describing the meta data of the API
    

#### Eight security principals of secure REST APIs

Securing these REST API endpoints is no easy feat and it will often require some custom solutions but we can define a general list of API design principles which will help keep your code more secure.

-   **Least Privilege:** A system should be analysed very carefully and should only be passed rights to an application if those rights are absolutely needed. Permissions should be removed when they are no longer needed either.
    
-   **Fail-Safe Defaults**: This means that a user should not have access to any resources by default (fail safe) and they should only be allowed to view a resources after receiving explicit permissions.
    
-   **Economy of Mechanism:** The more complex a system and it's interfaces is, the more chances there are for something to go wrong and when it does go wrong, the chances are also bigger that the severity will be higher which is why we should strive to keep our complexity down as much as possible
    
-   **Complete Mediation:** On every action that is executed by the system, it should verify whether or not the user is allowed to do that action using a list of user rights that's created on the spot and not using the cached data
    
-   **Open Design:** While some companies might be convinced that keeping as much source code a secret is a good idea, you and I both know better. Having the guts to expose software that was written for internal usage up to bug bounties is a thing of beauty and any serious manager should consider it to be honest
    
-   **Separation of Privilege:** specific users should never gain access to a specified object based on 1 critirium but ideally multiple selection criteria should be used to define 1 users access. This way we can have more fine grained control
    
-   **Least Common Mechanism:** We have to think about sharing states between calls carefully as if we can corrupt the state of the object at any point, the flow that is predefined might no longer work as expected
    
-   **Psychological Acceptability:** Any security mechanism should not add any perceiveable time to a webapps loading time or the process of the users as they will start looking for ways around our limitations for example if they have to change their password every 30 days they might just change the past 1 character
    

#### **Some principles for securing REST API?**

#### Keep it Simple

The more functionality and complexity we add to a system, the easier it will be to overlook certain aspects of our APIs security which is why we need to pay special attention to only leave as much complexity as we really need.

#### Use HTTPS

This will prevent attacks such as Man-In-The-Middle attacks which aim to listen in on your traffic between the browser and the API in the back-end. If you always communicate over TLS, you will also give your customers more confidence which will lead to higher conversions.

#### Password Hash

When storing passwords they should always be hashed because in the occasion of a complete database leak, the passwords should still be impossible to read. Instead when the user enters a password to login, the same password is encrypted with the same encryption algorithm while registering. These hashes should then be compared and if they match, the system should login the user.

#### Never expose information on URLs

We have to ensure that sensitive information such as API keys, usernames and passwords are nowhere to be found in the URL of our web application. These types of requests can be viewed from within the access logs which would compromise the security of these endpoints.

#### API Access Control

Our API endpoints should always check whether the user is allowed to perform the action they are about for perform and it should be blocked off if it turns out the user has no rights to that functionality. This can be on an endpoint in it's whole or to a certain object on an endpoint.

#### _Response Security Headers_

There are a number of [security related headers](https://owasp.org/www-project-secure-headers/) that can be returned in the HTTP responses to instruct browsers to act in specific ways. However, some of these headers are intended to be used with HTML responses, and as such may provide little or no security benefits on an API that does not return HTML.

The following headers should be included in all API responses:

  

#### Adding Timestamp in Request

To prevent replay attacks we night consider adding a timestamp to our requests in the form of a custom header. Next we will make sure the server only processes the requests that are within a reasonable amount of time from the current time (1- to 2 minutes). If the attacker will now try to replay for example the login system to brute force it, this will offer some very basic protection.

#### Input Parameter Validation

We have to put strong validation checks in place even before our data reaches our back-end logic to ensure it gets implemented properly and also logged so we do not forget. User defined input should be upheld to the strictest standards as it can often be unpredictable.

#### Why REST API security is important

In short i would to leave you with the thought that APIs are often points of integration and know from experience this is where the biggest issues always arise. It does not mean that we don't have a UI that it is therefore impossible to directly talk to our API and we should be well aware of the security risks this can hold.