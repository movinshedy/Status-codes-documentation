# HTTP response status codes <br>
HTTP response status codes indicate whether a specific HTTP request has been successfully completed. Responses are grouped in five classes:<br>

## (A) Informational responses <br>
### 100 Continue
The HTTP 100 Continue informational response status code indicates that the initial part of a request has been received and has not yet been rejected by the server. The client should continue with a request or discard the 100 response if the request is already finished.<br>

When a request has an Expect: 100-continue header, the 100 Continue response indicates that the server is ready or capable of receiving the request content. Waiting for a 100 Continue response can be helpful if a client anticipates that an error is likely, for example, when sending state-changing operations without previously verified authentication credentials.<br>

_Example_<br>
PUT request with 100 Continue<br>
The following PUT request sends information to a server about a file upload. The client is indicating that it will proceed with the content if it receives a 100 response to avoid sending data over the network that could result in an error like 405, 401, or 403. At first, the client sends headers only, including an Expect: 100-continue header:<br>
<br>
PUT /videos HTTP/1.1<br>
Host: uploads.example.com<br>
Content-Type: video/h264<br>
Content-Length: 123456789<br>
Expect: 100-continue<br>
The server indicates that the request can proceed:
![alt url]()<br>
The client completes the request by sending the actual data:<br>
![alt url]()<br>

### 101 Switching Protocols<br>
The HTTP 101 Switching Protocols informational response status code indicates the protocol that a server has switched to. The protocol is specified in the Upgrade request header received from a client.

The server includes an Upgrade header in this response to indicate the protocol it has agreed to switch to<br>
Example<br>
GET /notifications HTTP/1.1
Host: example.com
Upgrade: websocket
Connection: Upgrade<br>

### 102 Processing<br>
The HTTP 102 Processing informational response status code indicates to client that a full request has been received and the server is working on it. This status code is only sent if the server expects the request to take significant time.<br>

## Successful responses <br>
### 200 ok <br>
The request succeeded. The result and meaning of "success" depends on the HTTP method:

GET: The resource has been fetched and transmitted in the message body.<br>
GET <request-target>["?"<query>] HTTP/1.1<br>
HEAD: Representation headers are included in the response without any message body.<br>
HEAD / HTTP/1.1
Host: example.com
User-Agent: curl/8.6.0
Accept: */*<br>
PUT or POST: The resource describing the result of the action is transmitted in the message body.<br>
POST /test HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 27

field1=value1&field2=value2<br>

TRACE: The message body contains the request as received by the server.<br>
### 201 create <br>
POST /users HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "firstName": "Brian",
  "lastName": "Smith",
  "email": "brian.smith@example.com"
}<br>
The request succeeded, and a new resource was created as a result. This is typically the response sent after POST requests, or some PUT requests.<br>
### 202 Accepted <br>
The request has been received but not yet acted upon. It is noncommittal, since there is no way in HTTP to later send an asynchronous response indicating the outcome of the request. It is intended for cases where another process or server handles the request, or for batch processing.<br>
HTTP/1.1 202 Accepted
Date: Wed, 26 Jun 2024 12:00:00 GMT
Server: Apache/2.4.1 (Unix)
Content-Type: application/json

{
  "message": "Request accepted. Starting to process task.",
  "taskId": "123",
  "monitorUrl": "http://example.com/tasks/123/status"
}<br>

## Redirection messages <br>

### 300 Multiple Choices<br>
In agent-driven content negotiation, the request has more than one possible response and the user agent or user should choose one of them. There is no standardized way for clients to automatically choose one of the responses, so this is rarely used.<br>
HTTP/1.1 300 Multiple Choices
Date: Fri, 30 Aug 2024 09:21:48 GMT
Server: Apache/2.4.59 (Unix)
Alternates: {"index.html.en" 1 {type text/html} {language en} {length 48}}, {"index.html.fr" 1 {type text/html} {language fr} {length 45}}
Vary: negotiate,accept-language
TCN: list
Content-Length: 419
Content-Type: text/html; charset=iso-8859-1<br>
### 301 Moved Permanently<br>
The URL of the requested resource has been changed permanently. The new URL is given in the response.<br>
HTTP/2 301
cache-control: max-age=2592000,public
location: /en-US/docs/Learn/JavaScript/Client-side_web_APIs/Fetching_data
content-type: text/plain; charset=utf-8
date: Fri, 19 Jul 2024 12:57:17 GMT
content-length: 97

Moved Permanently. Redirecting to /en-US/docs/Learn/JavaScript/Client-side_web_APIs/Fetching_data<br>

### 303 See Other<br>
The server sent this response to direct the client to get the requested resource at another URI with a GET request.<br>
HTTP/1.1 303 See Other
Location: https://www.example.com/confirmation/event/123
Content-Type: text/html; charset=UTF-8
Content-Length: 0<br>
### 304 Not Modified <br>
This is used for caching purposes. It tells the client that the response has not been modified, so the client can continue to use the same cached version of the response.<br>
HTTP/1.1 304 Not Modified
Date: Wed, 28 Aug 2024 09:52:35 GMT
Expires: Wed, 28 Aug 2024 10:01:53 GMT
Age: 3279
ETag: "b20a0973b226eeea30362acb81f9e0b3"
Cache-Control: public, max-age=3600
Vary: Accept-Encoding
X-cache: hit
Alt-Svc: clear<br>
## Client error responses <br>
### 400 Bad Request<br>
The server cannot or will not process the request due to something that is perceived to be a client error (e.g., malformed request syntax, invalid request message framing, or deceptive request routing).<br>
HTTP/1.1 400 Bad Request
Content-Type: application/json
Content-Length: 71

{
  "error": "Bad request",
  "message": "Request body could not be read properly.",
}<br>
### 401 Unauthorized <br>
Although the HTTP standard specifies "unauthorized", semantically this response means "unauthenticated". That is, the client must authenticate itself to get the requested response.<br>
HTTP/1.1 401 Unauthorized
Date: Tue, 02 Jul 2024 12:18:47 GMT
WWW-Authenticate: Bearer<br>
### 403 Forbidden
The client does not have access rights to the content; that is, it is unauthorized, so the server is refusing to give the requested resource. Unlike 401 Unauthorized, the client's identity is known to the server.
HTTP/1.1 403 Forbidden
Date: Tue, 02 Jul 2024 12:56:49 GMT
Content-Type: application/json
Content-Length: 88

{
  "error": "InsufficientPermissions",
  "message": "Deleting users requires the 'admin' role."
}<br>
### 404 Not Found
The server cannot find the requested resource. In the browser, this means the URL is not recognized. In an API, this can also mean that the endpoint is valid but the resource itself does not exist. Servers may also send this response instead of 403 Forbidden to hide the existence of a resource from an unauthorized client. This response code is probably the most well known due to its frequent occurrence on the web.

HTTP/1.1 404 Not Found
Age: 249970
Cache-Control: max-age=604800
Content-Type: text/html; charset=UTF-8
Date: Fri, 28 Jun 2024 11:40:58 GMT
Expires: Fri, 05 Jul 2024 11:40:58 GMT
Last-Modified: Tue, 25 Jun 2024 14:14:48 GMT
Server: ECAcc (nyd/D13E)
Vary: Accept-Encoding
X-Cache: 404-HIT
Content-Length: 1256

<!doctype html>
<head>
    <title>404 not found</title>
    ...<br>
    
### 405 Method Not Allowed
The request method is known by the server but is not supported by the target resource. For example, an API may not allow DELETE on a resource, or the TRACE method entirely.
HTTP/1.1 405 Method Not Allowed
Content-Length: 0
Date: Fri, 28 Jun 2024 14:30:31 GMT
Server: ECLF (nyd/D179)
Allow: GET, POST, HEAD<br>

### 406 Not Acceptable
This response is sent when the web server, after performing server-driven content negotiation, doesn't find any content that conforms to the criteria given by the user agent.

### 407 Proxy Authentication Required
This is similar to 401 Unauthorized but authentication is needed to be done by a proxy.

### 408 Request Timeout
This response is sent on an idle connection by some servers, even without any previous request by the client. It means that the server would like to shut down this unused connection. This response is used much more since some browsers use HTTP pre-connection mechanisms to speed up browsing. Some servers may shut down a connection without sending this message.

### 409 Conflict
This response is sent when a request conflicts with the current state of the server. In WebDAV remote web authoring, 409 responses are errors sent to the client so that a user might be able to resolve a conflict and resubmit the request.

### 410 Gone
This response is sent when the requested content has been permanently deleted from server, with no forwarding address. Clients are expected to remove their caches and links to the resource. The HTTP specification intends this status code to be used for "limited-time, promotional services". APIs should not feel compelled to indicate resources that have been deleted with this status code.

### 411 Length Required
Server rejected the request because the Content-Length header field is not defined and the server requires it.<br>

## Server error responses<br>
500 Internal Server Error
The server has encountered a situation it does not know how to handle. This error is generic, indicating that the server cannot find a more appropriate 5XX status code to respond with.

### 501 Not Implemented
The request method is not supported by the server and cannot be handled. The only methods that servers are required to support (and therefore that must not return this code) are GET and HEAD.

### 502 Bad Gateway
This error response means that the server, while working as a gateway to get a response needed to handle the request, got an invalid response.

### 503 Service Unavailable
The server is not ready to handle the request. Common causes are a server that is down for maintenance or that is overloaded. Note that together with this response, a user-friendly page explaining the problem should be sent. This response should be used for temporary conditions and the Retry-After HTTP header should, if possible, contain the estimated time before the recovery of the service. The webmaster must also take care about the caching-related headers that are sent along with this response, as these temporary condition responses should usually not be cached.

### 504 Gateway Timeout
This error response is given when the server is acting as a gateway and cannot get a response in time.

### 505 HTTP Version Not Supported
The HTTP version used in the request is not supported by the server.

### 506 Variant Also Negotiates
The server has an internal configuration error: during content negotiation, the chosen variant is configured to engage in content negotiation itself, which results in circular references when creating responses.

### 507 Insufficient Storage (WebDAV)
The method could not be performed on the resource because the server is unable to store the representation needed to successfully complete the request.

### 508 Loop Detected (WebDAV)
The server detected an infinite loop while processing the request.

### 510 Not Extended
The client request declares an HTTP Extension (RFC 2774) that should be used to process the request, but the extension is not supported.

### 511 Network Authentication Required
Indicates that the client needs to authenticate to gain network access.<br>






