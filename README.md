# WebSec
Something to talk
## Table of Content
* [OAuth 2.0](#OAuth-2.0)
* [HTTP Request Smuggling](#HTTP-Request-Smuggling)
## OAuth 2.0 authentication vulnerabilities
### What is OAuth?
OAuth is a token-based authentication.
![oauth-authorization-code-flow](https://user-images.githubusercontent.com/85674719/184623917-ba76c0e3-7730-43ee-9c4b-57236df5a25b.jpg)
#### CL.TE
Cause the front-end server doesnt support chunked encoding. So request will forward all the request to backend server.
```
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Length: 13
Transfer-Encoding: chunked

0

SMUGGLED
```
And back-end server use chunked to determine the request length so when issued `0` back-end know its end of request so the rest turn into other request 
```
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Length: 13
Transfer-Encoding: chunked

0

```
```
SMUGGLED
```

### Prevent OAuth authentication vulnerabilities?
#### For OAuth service providers
* Require other application to register a whitelist of valid `redirect_uris`. Wherever posible, use strict byte-for-byte compare to validate the URI in any incoming requests.
* Use state parameter, its should bound to user's session by including ungessable, session-specific data. This helps protect users from CSRF. Also harder for hacker use stolen authorazition code.
* On resource server, make sure verify that the access token was issued to same `client_id` making request. Check scope being requested to make sure this matches the scope for which token was origially granted.
#### For OAuth client application
* Use `state` parameter even though it's not mandatory.
* Send a `recirect_uri` param not only to the `/authorization` endpoint but also to the `/token` endpoint
* If use OpenID `id_token`, make sure it's validate to JWS, JWE and OpenID specifications.
## HTTP Request Smuggling
### Possible Attacks
* Web cache poisoning
* Web cache deception
* Session hijacking
* XSS
* Bypass WAF

