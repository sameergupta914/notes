# Key HTTP Methods

# GET: Used to retrieve or read data from the server.

    When you type a URL in your browser (e.g., searching for something or accessing a homepage), the browser defaults to a GET request.

    The server's job is to read data from the database and send it back to the client.

# POST: Used to send data to the server, typically to create or mutate (change/add) a new entry in the database.

    This method is used when submitting forms, such as sign-up, login, or feedback forms.

    The form data is sent in the body of the POST request.

    The server uses this data to perform an insert operation into the database.

# PUT: Used to put a complete resource on the server, often for file uploads.

    For example, uploading a profile picture or any large file.

    PATCH: Used to update or modify an existing entry in the database.

    If you wanted to change your existing name on a social media profile, you would typically use a PATCH request.

# DELETE: Used to delete a resource from the server or database.

    This is used for actions like deleting a user account or a post.

# Handling Methods in a Server

    The request method can be accessed using request.method.

    A server must check the request method to decide what action to perform, even if the URL path is the same.

    For example, a request to /signup might:

    Handle a GET request by rendering the sign-up form.

    Handle a POST request by processing the form data and creating a new user in the database.

    The video notes that while using if/else or switch statements works for simple examples, frameworks like Express are necessary to easily structure and maintain method-based routing in large applications.

    The most common methods used are GET (to get data) and POST (to send data).