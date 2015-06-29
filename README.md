# LeaseWeb API Design Standards

This guide describes a set of API Design standards used at [LeaseWeb](www.leaseweb.com).
When designing a new API, it's important to respect the HTTP interaction patterns
and resource models described below.

In case you need to defer from these standards or if conflicts are found, please contact the Developer Platform.

##Foundation

### Require TLS

Require TLS to access the API, without exception. Ideally, simply reject any non-TLS 
requests by not responding to requests for http or port 80 to avoid any insecure data 
exchange. Respond with a `403 Forbidden` if this is not possible for your environment.


Redirects are discouraged since they allow sloppy/bad client behavior without providing 
any clear gain. Clients that rely on redirects double up on server traffic and render TLS 
useless since sensitive data will already have been exposed during the first call.

### Versioning

APIs are subject to version control. Versioning your API is important as it helps developers 
and existing clients to slowly transition from a version two another with enough time to plan 
and adapt on changes.

Keep your API version connected to interfaces changes rather than implementations. If you fix 
a bug on version 1, is just easier to leave existing clients on version 1 rather than ask all of them 
to switch to 1.1 because you fixed something.
Should you change the interface, perhaps returning a completely different type in a response or 
having different mandatory parameters, then is good time to inform developers that they need to 
switch soon or later to version 2.

The version number of an API should appear in its URI as `/vN` with the major version (`N`) as prefix. 

#### Example

`/v1/bareMetals`

### Trace requests with Correlation-Ids

Each API response through the API Gateway will include a `X-LSW-CORRELATION-ID` header 
populated with a UUID value. Both the server and client can log these values, which will be helpful 
for tracing and debugging requests.


If you make subsequent calls to other APIs due to an API request, it is advised to add the 
`X-LSW-CORRELATION-ID` header to your subsequent call. This way it is possible to trace an 
API call from start to end throughout our application landscape.

When the API Gateway isn’t receiving a `X-LSW-CORRELATION-ID` it will add the header to the call.


## Requests and responses

### HTTP Status codes

Successful responses should be coded according to this guide:

* `200 OK`: Request succeeded for a `GET` call, for `PUT` or `DELETE` call that completed synchronously
* `201 Created`: Request succeeded for a `POST` call that completed synchronously
* `202 Accepted`: Request accepted for a `POST`, `PUT` or `DELETE` call that will be processed asynchronously 

Use the following HTTP status codes for errors:

* `400 Bad Request`: Request failed because client submitted invalid data and needs to check his data before submitting again 
* `401 Unauthorized`: Request failed because user is not authenticated
* `403 Forbidden`: Request failed because user does not have authorization to access the resource
* `404 Not Found`: Request failed because the resource does not exists
* `405 Method Not Allowed`: Request failed because the requested method is not allowed
* `500 Internal Server Error`: Request failed on server side, user should check status site or report the issue (preferably we track 500 errors and get notified automatically)
* `503 Service Unavailable`: API is unavailable, check status site for details

### Provide full resources

Provide the full resource representation in the response. Always provide the full resource on `200` and `201` responses, except 
for `DELETE` requests.

In case of asynchronous calls (`POST/PUT/DELETE`) you should use `202 Accepted`. `202` Responses will not include the full resource representation. You can provide a task ID which can be queried, or create a temporary resource until the asynchronous calls has 
finished and the resource is created.

*NOTE: see the examples for single resources and collections. In a single resource there is no need for a root-identifier.*

### Error messages

### Form validation


## Resources


## Pagination and partial Responses


## Search



