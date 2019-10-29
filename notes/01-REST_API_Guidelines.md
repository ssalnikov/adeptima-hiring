### Note
This is API sample optimized for React/Redux application ... usually discussed with back-end team prior to coding APIs.

### Reactima API Spec v0.1
* [OWASP 10 2017](OWASP%20Top%2010%20-%202017%20RC1-English.pdf)
# ACl
### ACL Roles
* User
  * Can't delete records such Companies, Contacts, but can Archive except ...
* Admin
  * User/IP Blacklisting
  * User Suspension
  * Can do everything expect maintaince tasks
* Superadmin 
  * Manage Backup
  * Monitor CPU, API, traffic usage, etc
  * Unrecovarble Actions
    * Delete User/Admin
    * Clean Duplicates
    * Free old back-ups
    
### ACL Cases
* Users can't see Security Stats. Visible for Admin & Superadmin

### ACL Scheme   
```json
{
   "user":[
      {
         "resource":"/api/controller",
         "methods":[
            "POST",
            "GET",
            "PATCH",
            "ARCHIVE"
         ]
      },
      {
         "resource":"/api/users",
         "methods":[
            "ME"
         ]
      },
      {
         "resource":"/api/lists",
         "methods":[
            "*"
         ]
      }
   ]
}
```  

### API test generation
* external 
* internal
  
# Relationships
* Relationships objects can't include other relationships. Only Parent object or Parent array of objects can include relationship.

### Relationship Permissions
 * Restricted list of models and fields 
   * contacts -> list but without full job description. Basically prevention from "SELECT * " 

```http
GET /companies?include=lists,users&fields[lists]=title,created&fields[users]=first,last&page[number]=3&page[size]=10 HTTP/1.1
```

```json
{ 
  "relationships": {
    "authors": [42],
    "likes": [1, 2, 3, 4, 5]
  }
}
```
 
### Relationship map example
* /companies can only include users no other relationship 
* for /companies user can only have fields first,last,editDate but not password
* LATER admin can have access to all fields "*"
* ??? Warning for wrong, missed, unauthorized fields
     
# Time
Unix time 03/15/2017 @ 1:46pm (UTC) - 1489585595, stored in Postgres with timezone

# CRUD

### Create
* Request
  * POST with single object in JSON body
* Response
  * ok -> status code 201
  * id of a newly created item in json payload
  ```json
    {"users": 1}
  ```


### Read
* Request
  * Example 

    ```GET /companies?include=lists,users&fields[lists]=title,created&fields[users]=first,last&page[number]=3&page[size]=10 HTTP/1.1```

  * Use [SUPERAGENT](https://github.com/visionmedia/superagent/blob/master/lib/client.js#L103) style
  
    ```fields[crawl]=profile_url&fields[crawl]=email&fields[crawl]=id&include=full_contact&include=empty```
    See ```pushEncodedKeyValuePair``` function for more details

  * Dont use [QS STRINGIFY](https://github.com/ljharb/qs) approach with indexes

    ```fields[crawl][0]=profile_url&fields[crawl][1]=email&fields[crawl][2]=id&include[0]=full_contact&include[1]=empty```

* Response
  * json body
  ```json
  {
    "pages": {
      "total": 13,
      "size": 4,
      "first": 1,
      "prev": 2,
      "self": 3,
      "next": 4,
      "last": 13
    },
    "articles": [
      {
        "id": 1,
        "title": "JSON API paints my bikeshed!",
        "body": "The shortest article. Ever.",
        "created": 1489585595,
        "updated": 1489585595,
        "relationships": {
          "authors": [42],
          "likes": [1, 2, 3, 4, 5]
        }
      }
    ],
    "authors": [
      {
        "id": 42,
        "name": "John",
        "age": 80,
        "gender": "male"
      }
    ],
    "likes": [{
      "id": 21,
      "articleId": 1,
      "userId": 42
    }]
  }
  ```

### Update/Patch
* Request
  * PATCH with 1 or more fields in JSON body
  * TODO discuss empty fields, e.g. ["", 0, null, undefined]
* Response
  * ok -> status code 204 (no content)

### Delete
* Request
  * DELETE to route /entity with list of ids(one or more) in json body for removing multiple items
    * preferred
  * DELETE to route /entity/:id for removing single item
    * implemented, but hard to handle in redux
* Response
  * ok -> status code 204 (no content)

#### Pagination
* [Pagination JSON API](http://jsonapi.org/format/#fetching-pagination)

  ```page[number], page[size], page[offset], page[limit]```

* Request
  number defaults to 1 and size to 50
  max size limited to 500
  ```http
  GET /articles?page[number]=3&page[size]=1 HTTP/1.1
  ```
  
#### Sorting
The sort order for each sort field MUST be ascending unless it is prefixed with a minus  “-“, in which case it MUST be descending.

```http
GET /articles?sort=-created,title HTTP/1.1
```

#### Object vs Array
topKey: { singleObj } - ok, but we don't implement it till Stable version

#### Reserved Keywords
Reserved 1st level top keys: ids, errors, removed, pages, patch, data, new, relationships, type

#### Wrong!
Don't use **data** or **type**. Otherwise will increase nesting and increase implementation complexity.

```
{
data: [{
    type: "articles",
    id: 1,
    title: "JSON API paints my bikeshed!"
  }, {
    type: "articles",
    id: 2,
    title:  "Rails is Omakase"
  }]
}
```

```
{
data: [{
    id: 1,
    title: "JSON API paints my bikeshed!"
  }, {
    id: 2,
    title:  "Rails is Omakase"
  }],
type: "articles"
}
```


### Errors
[JSON API error objects](http://jsonapi.org/format/#error-objects)

* See Golang net/http codes
* See /utils/responce_code.go

| Code | Problem                                                   |
|------|-----------------------------------------------------------|
|  123 | Value too short                                           |
|  225 | Password lacks a letter, number, or punctuation character |
|  226 | Passwords do not match                                    |
|  227 | Password cannot be one of last five passwords             |

Multiple errors on `"password"` attribute, with error `code`:

```http
HTTP/1.1 422 Unprocessable Entity
Content-Type: application/json

{
  "errors": [
    {
      "code":   "123",
      "source": { "pointer": "form.first-name" },
      "title":  "Value is too short",
      "detail": "First name must contain at least three characters."
    },
    {
      "code":   "225",
      "source": { "pointer": "form.password" },
      "title": "Passwords must contain a letter, number, and punctuation character.",
      "detail": "The password provided is missing a punctuation character."
    },
    {
      "code":   "226",
      "source": { "pointer": "form.password" },
      "title": "Password and password confirmation do not match."
    }
  ]
}
```
* [ ] Unify code numbers, etc
* [ ] Specification for "source" convension. Should be Redux friendly
 
 
#### Codes from REST cookbooks
 - HTTP Status Codes, http://www.restapitutorial.com/httpstatuscodes.html
 - REST CookBook, http://restcookbook.com
 - HTTP Status Codes, http://www.restpatterns.org/HTTP_Status_Codes

#### API Examples
* Twitter Codes & Responses HTTP Status Codes, https://dev.twitter.com/overview/api/response-codes
* Force.com REST API Status Codes and Error Responses, https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/errorcodes.htm
* Stripe API https://stripe.com/docs/api#list_transfers 
* Github API https://developer.github.com/v3/

#### WebSockets
A Google Chrome Extension for construct custom Web Socket requests and handle responses to directly test your Web Socket service
* https://github.com/hakobera/Simple-WebSocket-Client
* Why you don’t need Socket.IO https://medium.com/@ivanderbyl/why-you-don-t-need-socket-io-6848f1c871cd
* https://developer.mozilla.org/en-US/docs/Web/API/WebSocket
* https://echo.labstack.com/cookbook/websocket
* https://github.com/gorilla/websocket