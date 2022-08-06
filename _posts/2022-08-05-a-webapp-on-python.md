# Introduction
There are various web framework out there. They are included but not all, Django, Flask, etc.
The Python itself also provides Internet Protocols and Support library. 
This writing intends to use the library provided by Python only.

The application is to make the restful api for serving database.
It does not depend on third library.
It depends only on the python standard library 

For rest api:
+ Use python built-in library

For database:
+ The python has specified the database interface at PEP 249. Each instant database
such as PostgresSQL, MySQL, etc have its own implementation.


Talks is always cheaper than the some demonstration

# Implementaion
## Rest API
Handle getting a post
``` python
def get_posts_handle(self, post_id):
    """
    This endpoint is used to process an GET /post endpoind
    """
    msg = "content of : " + post_id
    self.send_response(200)
    self.send_header("Content-Length", len(msg.encode()))
    self.end_headers()

    self.wfile.write(msg.encode())
```

Create a new post
```python
# TBU 

```


## database
1. Initializatino
Create a database if it is not created.
In case it is development environment, add test data
``` python
def db_init():
    """
    Create a database if it does not exists
    """
    # If the db has not there, create the database
    if not os.path.isfile(DB_NAME):
        print ("Create database ")
        db_execute_schema()

    # check if it is the prod or dev environment
    if 'prod' in g_db:
        if g_db['prod'] == False:
            db_add_test_data()
```


The entire source code can be found [here](https://github.com/avble/py-demo-blog)

# Reference
[1] https://docs.python.org/3.9/library/http.server.html, 