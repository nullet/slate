# Introduction

> `curl` is a great way to explore using the API. Here's an example:

```shell
curl -H 'Content-Type: application/xml' -d '<user>...</user>' http://URL/memberships.xml
```

> Authentication is managed using HTTP authentication. Every request must include the Authorization HTTP header:

```shell
curl -u zach:changeme http://URL/trades.xml
```

> The above will return an XML-formatted response, if it succeeds. If a request fails, the error information is returned with an HTTP status code. For example, a curl request to create a user that already exists will return a 422:

```shell
  curl -H 'Content-Type: application/xml' \ 
  -u admin:password -d ' \
  <user> \
    <login>new_user</login> \ 
    <password>somepassword</password> \ 
    <email>new_user@yahoo.com</email> \
  </user> ' \
  http://subdomain.example.com/memberships.xml -v
```

> This might output something like:

```shell
  > POST /memberships.xml HTTP/1.1
  > Authorization: Basic 
  > User-Agent: curl/7.16.3 (powerpc-apple-darwin9.0) libcurl/7.16.3 OpenSSL/0.9.7l zlib/1.2.3
  > Host: subdmain.example.com:80
  > Accept: */*
  > Content-Type: application/xml
  > Content-Length: 134
  > 
  < HTTP/1.1 422 
  <?xml version="1.0" encoding="UTF-8"?>
  <errors>
    <error>Login has already been taken</error>
    <error>Email has already been registered</error>
  </errors>
```

The Inkling API is vanilla XML over HTTP (secure HTTPS is also possible if your account has it enabled.) In order to request that the data be returned as XML, merely append ".xml" at the end of the request URL. For any requests to a service that is sending data (e.g. the membership creation service posts XML data representing the new user) the `Content-Type` header should be set to `application/xml`.


### A few things to keep in mind:

  * Any quantity related to money (e.g. price, worth, performance, etc.) is reported in cents.
  * All "datetimes" of the xmlschema format `CCYY-MM-DDThh:mm:ssTZD` of a date. Where `TZD` is `Z` or `[+-]hh:mm`. If `self` is a UTC time, `Z` is used as `TZD`. `[+-]hh:mm` is used otherwise.
  * If your account has SSL enabled, make sure you use the HTTPS in your request URLs.

## Pagination

> Let's say we want to to retrieve the second page of recent markets from a site:

```shell
curl -u username:password http://subdomain.example.com/markets.xml?page=2
```

Any requests that return a list of objects will be paginated. To retrieve subsequent pages after the first one, simply append the "page" parameter following the request URL.

A common strategy to retrieve all the markets on your site is for your client program to loop until a subsequent request no longer returns any records.