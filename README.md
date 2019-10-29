# READ.md
CHAPTER 9
Instance variables live within the object they are assigned to.
If the instance variable is a reference to an object, both the reference and the object it refers to are on the heap. 
Instance variables are declared in a class not a method. 
Local variables are placed with the method. 


Chapter 10
A static method means "behavior not dependent on a instance variable, so no instance /object is required. 
Static variables are shared 
All instances are of the same class share a single copy of the static variables.


SpringBootBookProject
Resources:
Fundamental to REST is the concept of resource. A resource is anything that can be accessed or manipulated. Examples of resources include “videos,” “blog entries,” “user profiles,” “images,” and even tangible things such as persons or devices. Resources are typically related to other resources. For example, in an ecommerce application, a customer can place an order for any number of products. In this scenario, the product resources are related to the corresponding order resource. It is also possible for a resource to be grouped into collections. Using the same ecommerce example, “orders” is a collection of individual “order” resources.


URI Templates:
When working with REST and a REST API, there will be times where you need to represent the structure of a URI rather than the URI itself. For example, in a blog application, the URI http://blog.example. com/2014/posts would retrieve all the blog posts created in the year 2014. Similarly, the URIs http://blog. example.com/2013/posts, http://blog.example.com/2012/posts, and so forth would return blog posts corresponding to the years 2013, 2012, and so on. In this scenario, it would be convenient for a consuming client to know the URI structure http://blog.example.com/year/posts that describes the range of URIs rather than individual URIs. URI templates, defined in RFC 6570 (http://tools.ietf.org/html/rfc6570), provide a standardized mechanism for describing URI structure. The standardized URI template for this scenario could be:
http://blog.example.com/{year}/posts




Representations:
RESTful resources are abstract entities. The data and metadata that make a RESTful resource needs to be serialized into a representation before it gets sent to a client. This representation can be viewed as a snapshot of a resource’s state at a given point in time. Consider a database table in an ecommerce application that stores information about all the available products. When an online shopper uses their browser to buy a product and requests its details, the application would provide the product details as a Web page in HTML. Now, when a developer writing a native mobile application requests product details, the ecommerce application might return those details in XML or JSON format. In both scenarios, the clients didn’t interact with the actual resource—the database record-holding product details. Instead, they dealt with its representation.
 
 
 
 Safety:
 A HTTP method is said to be safe if it doesn’t cause any changes to the server state. Consider methods such as GET or HEAD, which are used to retrieve information/resources from the server. These requests are typically implemented as read-only operations without causing any changes to the server’s state and, hence, considered safe. Safe methods are used to retrieve resources. However, safety doesn’t mean that the method must return the same value every time. For example, a GET request to retrieve Google stock might result in a different value for each call. But as long as it didn’t alter any state, it is still considered safe. In real-world implementations, there may still be side effects with a safe operation. Consider the implementation in which each request for stock prices gets logged in a database. From a purist perspective we are changing the state of the entire system. However, from a practical standpoint, because these side effects were the sole responsibility of the server implementation, the operation is still considered to be safe.


Indempotency:
An operation is considered to be idempotent if it produces the same server state whether we apply it once or any number of times. HTTP methods such as GET, HEAD (which are also safe), PUT, and DELETE are considered to be idempotent, guaranteeing that clients can repeat a request and expect the same effect as making the request once. The second and subsequent requests leave the resource state in exactly the same state as the first request did. Consider the scenario in which you are deleting an order in an ecommerce application. On successful completion of the request, the order no longer exists on the server. Hence, any future requests to delete that order would still result in the same server state. By contrast, consider the scenario in which you are creating an order using a POST request. On successful completion of the request, a new order gets created. If you were to re“POST” the same request, the server simply honors the request and creates a new order. Because a repeated POST request can result in unforeseen side effects, POST is not considered to be idempotent.


Get:
The GET method is used to retrieve a resource’s representation. For example, a GET on the URI http:// blog.example.com/posts/1 returns the representation of the blog post identified by 1. By contrast, a GET on the URI http://blog.example.com/posts retrieves a collection of blog posts. Because GET requests don’t modify server state, they are considered to be safe and idempotent. A hypothetical GET request to http://blog.example.com/posts/1 and the corresponding response are shown here.


Head:
On occasions, a client would like to check if a particular resource exists and doesn’t really care about the actual representation. In another scenario, the client would like to know if a newer version of the resource is available before it downloads it. In both cases, a GET request could be “heavyweight” in terms of bandwidth and resources. Instead, a HEAD method is more appropriate. The HEAD method allows a client to only retrieve the metadata associated with a resource. No resource representation gets sent to the client. This metadata represented as HTTP headers will be identical to the information sent in response to a GET request. The client uses this metadata to determine resource accessibility and recent modifications. Here is a hypothetical HEAD request and the response.


Delete:
The DELETE method, as the name suggests, requests a resource to be deleted. On receiving the request, a server deletes the resource. For resources that might take a long time to delete, the server typically sends a confirmation that it has received the request and will work on it. Depending on the service implementation, the resource may or may not be physically deleted. On successful deletion, future GET requests on that resource would yield a “Resource Not Found” error via HTTP status code 404. We will be covering status codes in just a minute. In this example, the client requests a post identified by 1 to be deleted. On completion, the server could return a status code 200 (OK) or 204 (No Content), indicating that the request was successfully processed.


Put:
The PUT method allows a client to modify a resource state. A client modifies the state of a resource and sends the updated representation to the server using a PUT method. On receiving the request, the server replaces the resource’s state with the new state. In this example, we are sending a PUT request to update a post identified by 1. The request contains an updated blog post’s body along with all of the other fields that make up the blog post. The server, on successful processing, would return a status code 200, indicating that the request was processed 


Post:
The POST method is used to create resources. Typically, it is used to create resources under subcollections— resource collections that exist under a parent resource. For example, the POST method can be used to create a new blog entry in a blogging application. Here, “posts” is a subcollection of blog post resources that reside under a blog parent resource.


Patch:
As we discussed earlier, the HTTP specification requires the client to send the entire resource representation as part of a PUT request. The PATCH method proposed as part of RFC 5789 (http://tools.ietf.org/html/ rfc5789) is used to perform partial resource updates. It is neither safe nor idempotent. Here is an example that uses PATCH method to update a blog post title

HTTP Status Codes:
Informational Codes—Status codes indicating that the server has received the request but hasn’t completed processing it. These intermediate response codes are in the 100 series. •	Success Codes—Status codes indicating that the request has been successfully received and processed. These codes are in the 200 series. •	Redirection Codes—Status codes indicating that the request has been processed, but the client must perform an additional action to complete the request. These actions typically involve redirecting to a different location to get the resource. These codes are in the 300 series. •	Client Error Codes—Status codes indicating that there was an error or a problem with client’s request. These codes are in the 400 series. •	Server Error Codes—Status codes indicating that there was an error on the server while processing the client’s request. These codes are in the 500 series. 


Bulding A Restful API:
Designing and implementing a beautiful RESTful API is no less than an art. It takes time, effort, and several iterations. A well-designed RESTful API allows your end users to consume the API easily and makes its adoption easier. At a high level, here are the steps involved in building a RESTful API: 1= Identify Resources—Central to REST are resources. We start modeling different resources that are of interest to our consumers. Often, these resources can be the application’s domain or entities. However, a one-to-one mapping is not always required. 2= Identify Endpoints—The next step is to design URIs that map resources to endpoints. In Chapter 4, we will look at best practices for designing and naming endpoints. 3= Identify Actions—Identify the HTTP methods that can be used to perform operations on the resources. 4= Identify Responses—Identify the supported resource representation for the request and response along with the right status codes to be returned. 


Model View Controller:
The Model View Controller, or MVC, is an architectural pattern for building decoupled Web applications. This pattern decomposes the UI layer into the following three components: 


Controller:
Controllers in Spring Web MVC are declared using the stereotype org.springframework.stereotype. Controller. A stereotype in Spring designates roles or responsibilities of a class or an interface. Listing 2-1 shows a basic controller


View:
Spring Web MVC supports a variety of view technologies such as JSP, Velocity, Freemarker, and XSLT. Spring Web MVC uses the org.springframework.web.servlet.View interface to accomplish this. The View interface has two methods

Resource Identification:
We begin the resource identification process by analyzing requirements and extracting nouns. At a high level, the QuickPoll application has users that create and interact with polls. From the previous statement, you can identify User and Poll as nouns and classify them as resources. Similarly, users can vote on polls and view the voting results, making Vote another resource. This resource modeling process is similar to database modeling in that it is used to identify entities or object-oriented design that identifies domain objects. 

Resource Representation:
The next step in the REST API design process is defining resource representations and representation formats. REST APIs typically support multiple formats such as HTML, JSON, and XML. The choice of the format largely depends on the API audience. For example, a REST service that is internal to the company might only support JSON format, whereas a public REST API might speak XML and JSON formats. In this chapter and in the rest of the book, JSON will be the preferred format for our operations.


Endpoint Identification:
REST resources are identified using URI endpoints. Well-designed REST APIs should have endpoints that are understandable, intuitive, and easy to use. Remember that we build REST APIs for our consumers to use. Hence, the names and the hierarchy that we choose for our endpoints should be unambiguous to consumers. We design the endpoints for our service using best practices and conventions widely used in the industry. The first convention is to use a base URI for our REST service. The base URI provides an  entry point for accessing the REST API. Public REST API providers typically use a subdomain such as http://api.domain.com or http://dev.domain.com as their base URI. Popular examples include GitHub’s https://api.github.com and Twitter’s https://api.twitter.com. By creating a separate subdomain,  you prevent any possible name collisions with webpages. It also allows you to enforce security policies that are different from the regular website. To keep things simple, we will be using http://localhost:8080 as our base URI in this book. 

Action Identification:
HTTP Verbs allow clients to interact and access resources using their endpoints. In our QuickPoll application, the clients must be able to perform one or more CRUD operations on resources such as Poll and Vote. Analyzing the use cases from the “Introducing QuickPoll” section, Table 4-2 shows the operations allowed on Poll/Polls collection resources along with the success and error responses. Notice that on the Poll collection resource we allow GET and POST operations but deny PUT and Delete operations. A POST on the collection resource allows the client to create new polls. Similarly, we allow GET, PUT, and Delete operations on a given Poll resource but deny POST operation. The service returns a 404 status code for any GET, PUT, and DELETE operation on a Poll resource that doesn’t exist. Similarly, any server errors would result in a status code of 500 sent to the client.


QuickPoll Architecture:
The QuickPoll application will be made of a Web or REST API layer and a Repository layer with a domain layer crosscutting those two, as depicted in Figure 4-3. A layered approach provides a clear separation of concerns, making applications easy to build and maintain. Each layer interacts with the layer below using a well-defined contract. As long as the contract is maintained, it is possible to swap out underlying implementations without any impact to the overall system

Repository Implementation:
Repositories, or Data Access Objects (DAO), provide an abstraction for interacting with datastores. Repositories traditionally include an interface that provides a set of finder methods such as findById, findAll for retrieving data, and methods to persist and delete data. Repositories also include a class that implements this interface using datastore-specific technologies. For example, a repository dealing with a database uses technology such as JDBC or JPA, and a repository dealing with LDAP would use JNDI. It is also customary to have one Repository per domain object.

Embedded Database:
In the previous section, we created repositories, but we need a relational database for persisting data. The relational database market is full of options ranging from commercial databases such as Oracle, SQL Server to open source databases such as MySQL, and PostgreSQL. To speed up our QuickPoll application development, we will be using HSQLDB, a popular in-memory database. In-memory, aka embedded, databases don’t require any additional installations and can simply run as a JVM process. Their quick startup and shutdown capabilities make them ideal candidates for prototyping and integration testing. At the same time, they don’t usually provide a persistent storage and the application needs to seed the database every time it bootstraps. 

API Implementation:
In this section, we will create Spring MVC controllers and implement our REST API endpoints. We begin by creating the com.apress.controller package under src\main\java to house all of the controllers.

PollController Implementation:
The PollController provides all of the necessary endpoints to access and manipulate the Poll and Polls resources. Listing 4-14 shows a barebones PollController class.

VoteController Implementation:
Following the principles used to create PollController, we implement the VoteController class. Listing 4-21 gives the code for the VoteController class along with the functionality to create a vote. The VoteController uses an injected instance of VoteRepository to perform CRUD operations on Vote instances

ComputeResultController Implementation:
The final piece remaining for us is the implementation of the ComputeResult resource. Because we don’t have any domain objects that can directly help generate this resource representation, we implement two Data Transfer Objects or DTOs—OptionCount and VoteResult. The OptionCount DTO contains the ID of the option and a count of votes casted for that option. The VoteResult DTO contains the total votes cast and a collection of OptionCount instances. These two DTOs are created under the com.apress.dto package, and their implementation is given in Listing 4-24.

InputField Validation:
As a famous proverb goes, “Garbage in Garbage Out”; input field validation should be another area of
emphasis in every application. Consider the scenario in which a client requests a new poll to be created but
doesn’t include the poll question in the request. Figure 5-7 shows a Postman request with a missing question
and the corresponding response. Make sure that you set the Content-Type header to “application/json”
before firing the Postman request. From the response, you can see that the poll still gets created. Creating a
poll with a missing question can result in data inconsistencies and other bugs.

Externalizing Error Messages:
We have made quite a bit of progress with our input validation and provided the client with descriptive error
messages that can help them troubleshoot and recover from those errors. However, the actual validation
error message may not be very descriptive and API developers might want to change it. It would be even
better if they were able to pull this message from an external properties file. The property file approach not
only simplifies Java code but also makes it easy to swap the messages without making code changes. It also
sets the stage for future internationalization/localization needs. To achieve this, create a messages.properties
file under the src\main\resources folder and add the following two messages:

