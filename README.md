Overview
========
A simple REST service intended to be used as a service
stub during the early stages of the development, design or
discovery phases of a project.

The rationale here is that I often want a service that
will easily persist or return data for me while I'm
playing around with front-end interfaces and code.  I

It doesn't matter that it is not necessarily the way I 
want my back-end service to look ultimately but rather 
that it just holds on to and returns data.  I'm OK with
hitting a generic REST URL structure at first while I'm 
playing with an idea.

Another thing I wanted to have is the ability to hit
POST, PUT and DELETE from a browser's address bar. So
this service will have an optional path prefix that
will tell it to treat a GET request differently.


Features
========
  - responds to ANY http request
    * with what though (besides a 200)?
  - supports GET, POST, PUT, DELETE methods
  - TODO: support JSONP
  - persists abitrary key=value pairs as javascript objects.
    * in memory for now
    * TODO: I plan to persist to disk once the service is up and running 
  - optional GET syntax to access POST, PUT, DELETE methods for convenience (i.e. 
    allows using the browser address bar)



API documentation
=================
- all inputs expected to be key=value pairs
- general URL syntax:
/{_REST_COMMAND}/resource_name/:id?key1=value1&key2=value2&key3=value3

  * {_REST_COMMAND}
    Optional request type override. Treat request with the specified HTTP verb:
    * _REST_POST - treat request as a POST, query params treated as POST data
    * _REST_PUT - treat request as a PUT, query params treated as PUT data
    * _REST_DELETE - treate request as a DELETE

    This if for experimental development only (i.e. just trying things out)!

  * resource_name
    The type of object being accessed

  * :id
    The record id of a specific object

  * key=value&key2=value2
    Object data.  When the optional request type override is being used, the
    service will treat the GET parameters as request data



Methods:
  - GET /resource_name

    response:
    200 - return all records of the specified resource type
    404 - if the specified resource type does not exist (e.g. 
          hasn't been created yet).

    TODO: range and/or set parameters

  - GET /resource_name/:id

    response:    
    200 - return a single record of the specified resource type
    404 - if no record for the specified id was found

  - POST /resource
    PUT /resource
    create a new recoud of the type specified by resource

    response:
    201 - created new record (w/ JSON representation of the object)
    400 - validation error (w/ message WRT the error)

  - PUT /resource/:id
    create new or overwrite existing resource record

    response:
    201 - new record created (w/ JSON representation of the object)
    200 - existing record overwritten w/ JSON representation of the new version of the object
    400 - validation error (w/ message WRT the error)


  - POST /resource/:id
    update an existing resource record 

    response:
    200 - successful update
          (return entire object? or just the updated data? copy of the old data?)
    404 - record for specified id not found
    400 - validation error (w/ message WRT the error)

  - DELETE /resource/:id
    delete the specified resource record

    response:
    204 - record deleted sucessfully (w/ no message body in response)
    404 - specified resource record not found
