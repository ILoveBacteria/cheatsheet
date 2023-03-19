# HTTP Cheatsheet

## Table Of Contents

- [HTTP Cheatsheet](#http-cheatsheet)
  - [Table Of Contents](#table-of-contents)
  - [Handwrite Notes](#handwrite-notes)
  - [Important Headeres](#important-headeres)
    - [Response Headers](#response-headers)
    - [Request Headers](#request-headers)
  - [HTTP Methods](#http-methods)
  - [HTTP Status Codes](#http-status-codes)
  - [Cookie](#cookie)
  - [`curl`](#curl)
  - [`dig`](#dig)

## Handwrite Notes

| #     | Key           | Description                                                                 | Reference |
| ----- | ------------- | --------------------------------------------------------------------------- | --------- |
| **1** | `http.server` | Create a simple fantastic HTTP server with Python. `python3 -m http.server` |           |

## Important Headeres

### Response Headers

| Header                       | Description                                                  |
| ---------------------------- | ------------------------------------------------------------ |
| Age                          | The age the object has been in a proxy cache in seconds      |
| Access-Control               |                                                              |
| Content-Type                 | The MIME type of this content                                |
| Location                     | Used in redirection, or when a new resource has been created |
| Server                       | A name for the server                                        |
| Content-Security-Policy(CSP) | From which domains the website assets can be loaded.         |
| X-Content-Type-Options       | Prevents payloads that hides in files                        |
| X-Powered-By                 | Specifies the technology (e.g. ASP.NET, PHP, JBoss)          |
| Set-Cookie                   | An HTTP cookie                                               |
| X-Frame-Options              | Prevent clickjacking. iframe                                 |

### Request Headers

| Header                                                          | Description                                                                 |
| --------------------------------------------------------------- | --------------------------------------------------------------------------- |
| Accept                                                          | Media types that accept in respone                                          |
| Referer                                                         | The previous address                                                        |
| Origin                                                          | Initiate a requst for CORS                                                  |
| Host                                                            | The domain name od the server                                               |
| Cookie                                                          | Who is this client                                                          |
| Transfer-Encoding                                               | The form of encoding used to safely transfer the entity to the user.        |
| Content-Length                                                  | The length of the request                                                   |
| Connection                                                      | keep-alive: Do not hand shake again - Upgrade: Change HTTP protocol to TCP. |
| Websocket is a persistent connection - Can be sent both of them |
| Content-Type                                                    | The media type of the body in request(used with POST and PUT)               |
| Authorization                                                   | HTTP basic authentication. Also is used for API JWT                         |
| User-Agent                                                      | The browser of the client                                                   |

## HTTP Methods

| Method  | Purpose     | Request Body |
| ------- | ----------- | ------------ |
| GET     | API         | No           |
| POST    | API - Forms | Yes          |
| PUT     | API         | Yes          |
| DELETE  | API         | No           |
| OPTIONS | CORS - XHR  | No           |
| PATCH   | API         | Yes          |
| HEAD    | API         | No           |

## HTTP Status Codes

| Code | Description                                                                                                           |
| ---- | --------------------------------------------------------------------------------------------------------------------- |
| 200  | **OK**                                                                                                                |
| 201  | **Created** The request has been fulfilled, resulting in the creation of a new resource                               |
| 202  | **Accepted** The request has been accepted for processing, but the processing has not been completed                  |
| 204  | **No Content** The server successfully processed the request, and is not returning any content                        |
| 301  | **Moved Permanently**                                                                                                 |
| 302  | **Found** Tells the client to look at (browse to) another URL                                                         |
| 304  | **Not Modified** Indicates that the resource has not been modified since the version specified by the request headers |
| 400  | **Bad Request**                                                                                                       |
| 401  | **Unauthorized**                                                                                                      |
| 403  | **Forbidden** The request contained valid data and was understood by the server, but the server is refusing action    |
| 404  | **Not Found**                                                                                                         |
| 405  | **Method Not Allowed**                                                                                                |
| 500  | **Internal Server Error**                                                                                             |
| 502  | **Bad Gateway** The server was acting as a gateway or proxy and received an invalid response from the upstream server |
| 504  | **Gateway Timeout** The server was acting as a gateway or proxy and did not receive a timely response                 |

## Cookie

| Parameter | Sample Value     | Description                                                         |
| --------- | ---------------- | ------------------------------------------------------------------- |
| Name      | myCookie         | The name of the cookie                                              |
| Value     | eijv45xbr21      | The value of the cookie                                             |
| Domain    | .example.com     | Which domains the cookie can be used. Also include other subdomains |
| Path      | /                | Where can this cookie be used                                       |
| Expires   | Thu, 16 Feb 2023 | When the cookie will expire                                         |
| Secure    | True             | Only send the cookie over HTTPS                                     |
| HttpOnly  | True             | If this is false, we can get the cookie with JavaScript             |


## `curl`

| Command                            | Description                       |
| ---------------------------------- | --------------------------------- |
| `$ curl <addr>`                    | Send a GET request to the address |
| `$ curl -I <addr>`                 | Send a HEAD request to the addr   |
| `$ curl <addr> -X POST`            | Send a POST request               |
| `$ curl <addr> -d "param1:value1"` | Send a POST request with data     |
| `$ curl <addr> -H "key:value"`     | Add headers                       |

## `dig`

| Command          | Description         |
| ---------------- | ------------------- |
| `$ dig <addr>`   | request dns records |
| `$ dig A <addr>` | request A record    |