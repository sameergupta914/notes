
# Getting Started with Express.js and Node.js

This video introduces Express.js, a popular, fast, unopinionated, and minimalist framework for Node.js. It explains the problems Express solves with the native Node.js HTTP module and shows how to set up and use basic routing.

1. The Problem with Native HTTP Module
When building a server using the native Node.js http module, managing application logic quickly becomes complex:

Routing Complexity: Developers must manually create a separate switch case for every single URL path (/, /about, /contact).

Method Handling: Within each route, separate if/else statements are needed to handle different HTTP methods (GET, POST, PUT).

Manual Parsing: Essential tasks, like parsing URL query parameters or request body data (JSON), require importing and using extra, dedicated modules (like the built-in url module).

Code Clutter: The combination of manual routing, method handling, and parsing leads to unclean, unmaintainable, and pain-inducing code.

2. What Express.js Does
Express acts as an application layer that handles the low-level concerns of the native HTTP module, effectively abstracting away the pain points.

Simplified Routing: Express provides clean, built-in methods to handle routes and HTTP methods simultaneously.

Built-in Parsing: It handles the parsing of query parameters and other request data automatically, eliminating the need for external URL parsing packages.

Structure: It gives your code a clear, modular structure that is easier to scale and maintain.

3. Basic Express Server Setup
Setting up an Express application is significantly simpler than using the raw HTTP module:

Step	Native HTTP Module (Painful)	Express.js (Easy)
1. Installation	(Not needed, it's native)	Use npm install express to add the framework.
2. Initialization	Require http and manually use http.createServer(myHandler).	Require Express, then call it to create an application instance: const app = express();
3. Routing	Use a manual switch case on request.url and if/else on request.method.	Use the dedicated method on the app instance: app.get('/', handler) or app.post('/submit', handler).
4. Listening	Manually call server.listen(port).	Express's app instance includes a built-in listener: app.listen(8000, callback).

Export to Sheets
Example of Routing in Express
In Express, the handler function for a route only runs when both the path and the method match, which cleans up the code immensely.