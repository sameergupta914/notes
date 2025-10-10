
Handling URLs in Node.js
This video explains the components of a Uniform Resource Locator (URL) and demonstrates how to handle and parse different URL elements in a basic Node.js server to implement routing.

1. Components of a URL
A URL is composed of several key parts that define how to access a resource on the internet (e.g., https://www.example.com/about?user=1&page=2):

Component Description Example Purpose
Protocol A set of rules for communication.	https	Specifies how the client and server should talk (e.g., HTTP is unsecured, HTTPS is secured).
Domain Name	A human-friendly name that maps to the server's IP address.	www.example.com	Easier for users to remember than an IP address.
Path The specific location of the resource on the server.	/about	Directs the request to a specific "page" or resource handler. Can be nested (e.g., /projects/1).
Query Parameters Extra information sent to the server as key-value pairs.	?user=1&page=2	Used to pass data to the server (e.g., search terms, user IDs, sorting options). Starts with ? and pairs are separated by &.

Export to Sheets
2. URL Handling in Node.js
A basic Node.js HTTP server receives the full URL string, including the path and query parameters, in the request.url property.

The Problem with Simple Routing
The built-in Node.js HTTP module does not automatically separate the path from the query parameters.

If a request is made to /about?user=1, the request.url will literally be /about?user=1.

A simple switch case looking for a path of /about will fail because it won't match the full URL string.

The Solution: The url Package
To properly handle the different parts of a URL, the built-in url package (or module) is used to parse the string into an object:

Install: Use npm install url to get the package.

Import: Require the module: const url = require('url');

Parsing: Use url.parse(request.url, true) to break down the URL string.

The true argument is crucial as it instructs the parser to separate the query parameters into a usable object.

Accessing Components:

Path: Access the path using myURL.pathname (e.g., /about). This is used for routing in the switch statement.

Query Parameters: Access the parameters as an object using myURL.query (e.g., { user: '1', page: '2' }).

Example of Query Parameter Handling
After parsing, the server can easily access and use the query data:

JavaScript

// Parse the URL, setting the second argument to true to parse query strings
const myURL = url.parse(request.url, true);

// Get the specific value of a query parameter (e.g., "myname")
const userName = myURL.query.myname;

// Use the data to customize the response
response.end("Hey " + userName);
By using the url package, the server can use the clean path (/about) for its main routing logic and independently use the query parameters (user=1, page=2) to customize the data it fetches and sends back to the client. This is the foundation of building a functional backend server.