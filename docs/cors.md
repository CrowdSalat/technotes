# Cross-Origin Resource Sharing (CORS)

 CORS is an HTTP-header based mechanism that allows a server to indicate any origins (domain, scheme, or port) other than its own from which a browser should permit loading resources. 
 
 [Source](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

## preflight

Send an http request before the acutal request to check if the actual request would be allowed CORS-wise.

## Header

The Access-Control-Allow-Origin will be set server-side and need to contain the domain of the website which calls the service. Is not needed if both client and server are raeachable under the same url.
