# Introduction
In web application, some web-pages require authenticated-user.
User accesses to the web-pages which requires authentication. If the user has not been
authenticated, it will redirect to log-in page for user authencition. 
In case the user authentication is correct, it will redirec to the web page that user
had requested before.

This writing is to look into detail how the client submits an authentication (in form of form data)
And the backend logic to process the authentication. 

The implementation is written based on the Python built-in library.


# Submit a form data from client server
HTML example
```html
<form method="POST" action="/submit_form" >
    <label> username </label>
    <input name="username" type="text"> 
    <input name="email" type="text"> 
    <button> send my username </button>
</form>
```

The example of received http request on server.
```text
POST / HTTP/1.1
Host: foo.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 13

username=Harry&email=harry@aaa.com
```

# handle the form data in backend
At the backend, the common logic of handling this kind of request is as below.

```python
if the content-type is 'application/x-www-form-urlencoded':
    # read username and e-mail from body
    
    # if the username and e-mail is matched with app's credential
    # response by sending a `redirect` message to client.

    # Otherwise, return the login page so that user can continue do login

```

# implementation in python
The implementation is naively based on the python built-in library. 
(TBD)

# Reference
[1] https://www.ietf.org/rfc/rfc1867.txt
[2] https://developer.mozilla.org/en-US/docs/Learn/Forms/Sending_and_retrieving_form_data