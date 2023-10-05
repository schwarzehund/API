# API
Learning API (postman)

Interface for software to talk to hardware.
Example: How your phone's camera talks to the operating system. 

**Software Library APIs**# API
Learning API (postman)
-----
INTRODUCTION

Networking term	        Description	                                                    Restaurant analogy
Client	                The requester. Ex: browser, web app, mobile app	                Customer
API	                    Simplified interface for interacting with the backend          	Waiter
Server	                The backend where the processing happens	                      Kitchen

-----
**Hardware APIs**
Interface for directly consuming code from another code base.
Example: Using methods from a library you import into your application.

**Web APIs**
Interface for communicating across code bases over a network.
Example: Fetching current stock prices from a finance API over the internet.

------
There is more than one way to build and consume APIs. Some architecture types you may come across are:

REST (Representational State Transfer)
GraphQL
WebSockets
webhooks
SOAP (Simple Object Access Protocol)
gRPC (Google Remote Procedure Call)
MQTT (MQ Telemetry Transport)

----- 

Before curl method in command line was used to call for API like:

**curl **https://api.github.com/users/postmanlabs

-----

**Request methods**
When we make an HTTP call to a server, we specify a request method that indicates the type of operation we are about to perform. These are also called HTTP verbs.

Some common HTTP request methods correspond to the CRUD operations mentioned earlier. You can see a list of more methods here.

Method name	Operation
**GET**	Retrieve data (Read)
**POST**	Send data (Create)
**PUT/PATCH**	Update data (Update)

* PUT usually replaces an entire resource, whereas PATCH usually is for partial updates
DELETE	Delete data (Delete)


-----
**Request URL**
In addition to a request method, a request must include a request URL that indicates where to make the API call. A request URL has three parts: a protocol (such as http:// or https://), host (location of the server), and path (route on the server). In REST APIs, the path often points to a reference entity, like "books".

Protocol	Host	                            Path
https://	library-api.postmanlabs.com	      /books


-----

**Response status codes**
The Postman Library API v2 has returned a response status code of "200 OK". Status codes are indicators of whether a request failed or succeeded.

Status codes have conventions. For example, any status code starting with a "2xx" (a "200-level response") represents a successful call. Get familiar with other status code categories:

Code range	Meaning	Example
**2xx  	Success	    200 - OK**
201 - Created
204 - No content (silent OK)


**3xx	Redirection	  301 - Moved (path changed**)
**
4xx	Client error	400 - Bad request**
401 - Unauthorized
403 - Not Permitted
404 - Not Found

**5xx	Server error	500 - Internal server error**
502 - Bad gateway
504 - Gateway timeout

------

**Query parameter syntax**
Query parameters are added to the end of the path. They start with a question mark ?, followed by the key-value pairs in the format: <key>=<value>. For example, this request might fetch all photos that have landscape orientation:

GET https://some-api.com/photos?orientation=landscape

If there are multiple query parameters, each is separated by an ampersand &. Below two query parameters to specify the orientation and size of the photos to be returned:

GET https://some-api.com/photos?orientation=landscape&size=500x400

----

Path Variable syntax
The path variable comes immediately after a slash in the path. For example, the GitHub API allows you to search for GitHub users by providing a username in the path in place of {username} below: 

GET https://api.github.com/users/{username}



Making this API call with a value for {username} will fetch data about that user:

GET https://api.github.com/users/postmanlabs

You can have multiple path variables in a single request, such as this endpoint for getting a user's GitHub code repository:

GET https://api.github.com/repos/{owner}/{repoName}

For example, to get information about the newman code repository from postmanlabs:

GET https://api.github.com/repos/postmanlabs/newman

Path vs. query parameters
At first, it is easy to confuse these two parameter types. Let's compare them side by side. 

Path Variable	                                                              Query parameters
ex: /books/abc123	                                                          ex: /books?search=borges&checkedOut=false
Located directly after a slash in the path. It can be anywhere on the path	Located only at the end of a path, right after a question mark ?
Accepts dynamic values	                                                    Accepts defined query keys with potentially dynamic values.
* Often used for IDs or entity names	                                      * Often used for options and filters

----
![image](https://github.com/schwarzehund/API/assets/82609240/566e0250-aa06-4b45-a14f-c9ea6d85b544)

----
POST {{baseURL}}/books 

Body:
    {
        "title": "Walden",
        "author": "Henry David Thoreau",
        "genre": "philosophical",
        "yearPublished": 1854

    }


**Headers:** 
api-key

----

**Authorization**
Think about why you might not want an API to have completely open endpoints that anyone can access publicly. It would allow unauthorized people to access data they shouldn't see, or allow bots to flood an API with thousands of calls per second and shut it down. 

There are multiple methods for authorizing a request. Some examples are Basic Auth (username and password), OAuth (delegated authorization), and API Keys (secret strings registered to a developer from an API portal). 

---
**Scripting in Postman**
Postman allows you to add automation and dynamic behaviors to your collections with scripting.

Postman will automatically execute any provided scripts during two events in the request flow:

Immediately before a request is sent: pre-request script (Pre-request Script tab of request).
Immediately after a response comes back: test script (Tests tab of request).
In this lesson, we will focus on writing scripts in the Tests tab, which are executed when a response comes back from an API.

**PM_Object**

Postman has a helper object named pm that gives you access to data about your Postman environment, requests, responses, variables and testing utilities. 

For example, you can access the JSON response body from an API with: 

pm.response.json()

You can also programmatically get collection variables like the value of baseUrl with:

pm.collectionVariables.get(“baseUrl”)

In addition to getting variables, you can also set them with pm.collectionVariables.set("variableName", "variableValue") like this:

pm.collectionVariables.set(“myVar”, “foo”)

**SCRIPTS**

Saving a value as a variable allows you to use it in other requests. Using a Test script, let's grab the id of a newly added book and save it so we can use it in future requests.

The pm object allows you to set and get collection variables.

To set a collection variable, use the .set() method with two parameters: the variable name and the variable value

pm.collectionVariables.set("variableName", value)

To get a collection variable use the .get() method and specify the name of the variable you want to retrieve:

pm.collectionVariables.get("variableName")
