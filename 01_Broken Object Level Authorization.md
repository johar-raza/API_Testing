# API1:2019 Broken Object Level Authorization

#### **What is Broken Object Level Authorisation?**

Broken Object Level Authorisation all starts with an object. Objects should be looked at in the context of "Object Oriented Programming", what I mean with that is objects are the things you think about first in designing a program and they are also the units of code that are eventually derived from the process. This can be anything, ranging from an account to an invoice to a credit note and everything in between. Usually these objects are marked with an identifier because we need to address these objects directly and this is where issue can arise because we need to always check if the user is allowed to access that object. This might seem simple at first but i can assure you it's not, more about that later in the "testing" section of this article.

A broken Object issue occurs when the server does not properly check if the currently logged in user or if a logged out user can read, update or delete an object to which they do not have the rights.

#### **Two main types of Broken Object Level Authorization**

There are two types of broken authorisation on object, these can occur because either a userID is passed on to the server or an objectID, we will look into both.

#### **Based on user ID**

Sometimes a userID is passed on to the server, this can happen when we request all the resources for a certain user for example:

1.  <https://google.com/get_users_search_history?userID=1234>

If we replace the userID with someone else's userID, we should not be able to get the search history of other users but when a **Broken Object Level Authorization** issue arises, we can view the search history of other users.

This issue is simple to solve as a developer. We simply need to check if the currently logged in user is allowed to access those objects. In this example we need to check if the userID from the GET parameter is the same as the userID of the object's owner.

```
1.  Psuedo code:
2.
3.  if($_GET['userID'] === object.ownerID){
4.   ShowData()
5.  }else{
6.   echo "You are not allowed to view this data"
7.   wait(5s)
8.   Redirect(Homepage)
9.  }

11.  This is the safest way to handle the exception, DO NOT USE THE FOLLOWING CODE AS IT IS NOT SAFE:

13.  if(!$_GET['userID'] === object.ownerID){
14.   echo "You are not allowed to view this data"
15.   wait(5s)
16.   Redirect(Homepage)
17.  }
18.   ShowData()

20.  The data does not get rendered because of the redirect but a simple push of the back button can still enable the data to be displayed
```

#### **Based on object ID**

This vulnerability type can also exist because objectID's are passed to the server when the server does not properly check if the user is authorised for that object. This can happen very easily for example when developer needs to secure some resources and some not. They might forget to secure on of the objects which should be secured.

#### **Example of an attack**

Whatever type we are dealing with, some properties are the same across both.

These issues can arise more easily in two situations but of course they can arise any time that a developer forgets to add authorisation. We will zoom into two examples:

-   When a functionality is being developed and needs a change or an addition after a long time. Often sufficient documentation does not exist and by the time development begins, the developers might have forgotten part of the functionality. If this happens, it's easy to forget initial authorisation requirements and they might get overwritten or simply removed.
    
    -   For example if you build a function to add products but users should only be able to edit their own products. We have 3 calls we can make to a product:
        
        -   GET product.php?id=12 for getting the details
            
        -   POST product.php?id=12 for updating a product's details
            
        -   DELETE product.php?id=12 to delete a product
            
        -   OPTIONS
            
    -   These calls are secure and users can only execute them for their own objects
        
    -   After a year we want to add an option to import products via CSV
        
        -   POST /import.php? with a CSV body containing id,name and price: {1,"test",12}
            
    -   The problem is that the developers forgot to check if the user is allowed to write to that product with id=1 and the attacker can overwrite product details of products that do not belong to them
        
    ```
    1.  POST updateProduct.php?productID?id=1
    
    3.  if($_GET['productID'].owner === object.ownerID){
    4.   UpdateData()
    5.  }else{
    6.   echo "You are not allowed to view this data"
    7.   wait(5s)
    8.   Redirect(Homepage)
    9.  }
    
    11.  POST /import.php? 
    
    13.  StartUpdateData()
    ```
    In this pseudo code example we can see the described vulnerability in action
    
-   When a functionality integrates with another functionality, it's easy to overlook certain security considerations because the two functionalities are often developed separately and sometimes even in separate teams. If software designers and developers are not careful, they can easily make a mistake and forget to implement some required checks that prevent **Broken Object Level Authorization** vulnerabilities.
    
    -   For example we are building a function to update the prices of our products based on the products of our supplier every 10 minutes if a user is using the website but we are only allowed to update the prices of our own products. A call might get implemented that checks the prices in the background. An attacker can abuse this call to check the prices if they simply replace the objectID but it goes unnoticed because the call happens in the background.
        
    ```
    1.  TimerForProducts(){
    2.   wait(24H)
    3.   UpdatePrices($_GET[UsersProducts[])
    4.  }
    
    6.  UpdatePrices(products[]){
    7.   for each product in products{
    8.   getPrice()
    9.   }
    10.  }
    
    12.  GET /updatePrices?UsersProducts=1,2,3,4
    13.  An attacker can easily abuse this background call by passing any productID'
    14.  GET /updatePrices?UsersProducts=1,2,3,4,5,6,7,8,9,...
    ```

#### **How to Detect and Prevent Broken Object Level Authorization**

#### Detection

To detect these vulnerabilities we need to test read, update and delete actions on all objects that we are not authorised for. We can do this on two ways:

-   Replace every objectID that we encounter with one that we are not authorised for and see if we can execute the call succesfully
    
-   Replace our authentication token with the token from another user and browse our own objects. The advantage of doing this we can automate this approach, it's really hard to automate replacing every objectID because it can be named differently (i.e. adressID, productID,...) but the authorisation header is always the same.
    

Whatever approach we decide to take, it is vital that we check all objects and that we check them for read, update and delete actions. We need to check every functionality that has access to these objects, even if it's via a secondary route (example importing products instead of adding them manually).

#### Prevention

We can form some general tips for preventing Broken Object Level Authorization defects. These will help prevent the vulnerability or will lower the impact if one occurs.

-   Instead of sending the userID as a parameter, we should use an auth token such as JWT and extract the userID from there
    
-   We should always use GUIDs as id's, these are long and random strings of numbers and letters that make it a lot harder to guess other users' identifiers
    
-   Create a centralised authorisation solution that you can re-use for every sensitive object. This will prevent your code from becoming a mess of different authorisation mechanisms
    
-   We should use that authorisation mechanism to verify read, delete and update functions on objects that should be private
    
-   We need to make sure that tests are in place to ensure the existing authorisation mechanisms keep functioning as intended
    

#### **Conclusion**

Broken Object Level Authorization defects are becoming ever more prevelent as functionalities of applications increase and more and more API's are built. This requires more and more ethical hackers as it can be a severe vulnerability and it can be very easy to notice, all someone has to do is replace a number in a request if the server has not been configured properly.

Every endpoint that handles objects and receives and ID should properly enfore Object Level Authorization. The Object Level Authorization should check that the user who is trying to read or manipulate an object has the correct authorisation for it.

It is really important to have proper authorization checks are in place and we should always stop users from performing actions on objects they are not allowed to perform.