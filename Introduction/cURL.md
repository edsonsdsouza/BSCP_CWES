**cURL**, which stands for _client URL_, is a command line tool that developers use to transfer data to and from a server. At the most fundamental, cURL lets you talk to a server by specifying the location (in the form of a URL) and the data you want to send.

cURL supports several different [protocols](https://curl.se/docs/url-syntax.html), including HTTP and HTTPS, and runs on almost every platform. This makes cURL ideal for testing communication from almost any device (as long as it has a command line and network connectivity) from a local server to most [edge devices](https://www.ibm.com/think/topics/edge-computing?utm_source=ibm_developer&utm_content=in_content_link&utm_id=articles_what-is-curl-command&cm_sp=ibmdev-_-developer-articles-_-ibmcom).

---


## Commands

**Basic HTTP request to any URL**

```shell
curl http://info.cern.ch/
```

- cURL does not render HTML, CSS, JS


**Download a page or a file and output the content into a file**

```shell
curl -O http://info.cern.ch/index.html
```

If we want to specify the output file name, we can use the `-o` flag and specify the name. Otherwise, we can use `-O` and cURL will use the remote file name, as mentioned above

```shell
curl -o myfile.zip https://example.com/file.zip
```

**Silent the status of the request using `-s` flag**

```shell
curl -s -O http://info.cern.ch/index.html
```

**Check what other options does cURL provide**

```shell
curl -h
```

we may use `--help all` to print a more detailed help menu, or `--help category` (e.g. `-h http`) to print the detailed help of a specific flag. If we ever need to read more detailed documentation, we can use `man curl` to view the full cURL manual page.

---
## cURL for HTTPS

```shell
curl https://inlanefreight.com
```

Note:
- If we ever contact a website with an invalid SSL certificate or an outdated one, then cURL by default would not proceed with the communication to protect against the MITM attacks.
- We may face such an issue when testing a local web application or with a web application hosted for practice purposes, as such web applications may not yet have implemented a valid SSL certificate. Skip the certificate check with cURL using `-k` flag.

```shell
curl -k https://www.inlanefreight.com
```

---

**To view full HTTP request and response, use `-v` flag**

```shell
curl inlanefreight.com -v
```

The `-vvv` flag shows an even more verbose output.

---
## HTTP Headers - Foundation

1. General Headers - describe the message rather than its contents
	1. Date
	2. Connection
2. Entity Headers - `describe the content` (entity) transferred by a message
	1. Content-Type
	2. Media-Type
	3. Boundary
	4. Content-Length
	5. Content-Encoding
3. Request Headers - used in an HTTP request and do not relate to the content
	1. Host
	2. User-Agent
	3. Referer
	4. Accept
	5. Cookie
	6. Authorization
4. Response Headers - used in an HTTP response and do not relate to the content
	1. Server
	2. Set-Cookie
	3. WWW-Authenticate
5. Security Headers - a class of response headers used to specify certain rules and policies
	1. Content-Security-Policy
	2. Strict-Transport-Security
	3. Referer-Policy

---

**To check only the response headers, use `-I` flag**

```shell
curl -I https://www.inlanefreight.com
```

use `-i` flag to display both the headers and the response body (e.g. HTML code)

We can use `-A` flag to set `User-Agent` header

```shell
curl https://www.inlanefreight.com -A 'Mozilla/5.0'
```

---

## Request Methods - Foundation

1. `GET` - Requests a specific resource
2. `POST` - Sends data to the server
3. `HEAD` - Requests the headers that would be returned if a GET request was made to the server
4. `PUT` - Creates new resources on the server
5. `DELETE` - Deletes an existing resource on the webserver
6. `OPTIONS` - Returns information about the server, such as the methods accepted by it.
7. `PATCH` - Applies partial modifications to the resource at the specified location

## Status Codes - Foundation

1. `1xx` - Provides information and does not affect the processing of the request.
2. `2xx` - Returned when a request succeeds.
3. `3xx` - Returned when the server redirects the client.
4. `4xx` - Signifies improper requests `from the client`. For example, requesting a resource that doesn't exist or requesting a bad format.
5. `5xx` - Returned when there is some problem `with the HTTP server` itself.

| Code                        | Description                                                                                                                                               |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `200 OK`                    | Returned on a successful request, and the response body usually contains the requested resource.                                                          |
| `302 Found`                 | Redirects the client to another URL. For example, redirecting the user to their dashboard after a successful login.                                       |
| `400 Bad Request`           | Returned on encountering malformed requests such as requests with missing line terminators.                                                               |
| `403 Forbidden`             | Signifies that the client doesn't have appropriate access to the resource. It can also be returned when the server detects malicious input from the user. |
| `404 Not Found`             | Returned when the client requests a resource that doesn't exist on the server.                                                                            |
| `500 Internal Server Error` | Returned when the server cannot process the request.                                                                                                      |

---

**To provide the credentials through cURL, use `-u` flag**

```shell
curl -u admin:admin http://<SERVER_IP>:<PORT>/
```

or

```shell
curl http://admin:admin@<SERVER_IP>:<PORT>/
```

**Check the responses**

```shell
curl -v http://admin:admin@<SERVER_IP>:<PORT>/
```

**Manually set the authorization**

```shell
curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<SERVER_IP>:<PORT>/
```

- We can add the `-H` flag multiple times to specify multiple headers

**`GET` using cURL**

```shell
curl 'http://<SERVER_IP>:<PORT>/search.php?search=le' -H 'Authorization: Basic YWRtaW46YWRtaW4='
```

- Here setting authorization is a must

**We can use the `-X POST` flag to send a `POST` request. Then, to add our POST data, we can use the `-d` flag and add the above data after it**

```shell
curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/
```

- Once authenticated we get authenticated cookie.

**We can set the cookie with the `-b` flag in cURL**

```shell
curl -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/
```

**It is also possible to specify the cookie as a header**

```shell
curl -H 'Cookie: PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/
```

JSON data

```shell
curl -X POST -d '{"search":"london"}' -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' -H 'Content-Type: application/json' http://<SERVER_IP>:<PORT>/search.php 

["London (UK)"]
```

## CRUD API

| Operation | HTTP Method | Description                                        |
| --------- | ----------- | -------------------------------------------------- |
| `Create`  | `POST`      | Adds the specified data to the database table      |
| `Read`    | `GET`       | Reads the specified entity from the database table |
| `Update`  | `PUT`       | Updates the data of the specified database table   |
| `Delete`  | `DELETE`    | Removes the specified row from the database table  |

### Read

```shell
curl http://<SERVER_IP>:<PORT>/api.php/city/london
```

**JSON format output**

```shell
curl -s http://<SERVER_IP>:<PORT>/api.php/city/london | jq
```

**search term**

```shell
curl -s http://<SERVER_IP>:<PORT>/api.php/city/le | jq
```

### Create

```shell
curl -X POST http://<SERVER_IP>:<PORT>/api.php/city/ -d '{"city_name":"HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'
```

### Update

```shell
curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london -d '{"city_name":"New_HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json
```

### Delete

```shell
curl -X DELETE http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City
```

**check once deleted**

```shell
curl -s http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City | jq

[]
```