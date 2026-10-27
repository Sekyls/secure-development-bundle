# OAuth grant types

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `0a1baca7e157ee01e7e0d01ed2fcbbd8e7aeee65003f193f1ffb1f6c3a17db27`.
> Conversion: Local English OCR, all 6 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


> OAuth grant types

OAuth 2.0 authentication vulnerabilities

## What is an OAuth grant type?

The OAuth grant type determines the exact sequence of steps that are involved in the OAuth process. The grant type also affects including how the access token itself is sent. For this reason, grant types are often referred to as "OAuth flows". An OAuth service must be configured to support a particular grant type before a client application can initiate the corresponding fli authorization request it sends to the OAuth service. There are several different grant types, each with varying levels of complexity and security considerations. We'll focus on the "autl

## OAuth scopes

For any OAuth grant type, the client application has to specify which data it wants to access and what kind of operations it wants t sends to the OAuth service. For basic OAuth, the scopes for which a client application can request access are unique to each OAuth service. As the name of I providers. Some even use a full URI as the scope name, similar to a REST API endpoint. For example, when requesting read acc depending on the OAuth service being used: scope—contacts scope=contacts . read

scope=https://oauth-authorization server.com/auth/scopes/user/contacts.readonly

When OAuth is used for authentication, however, the standardized OpenID Connect scopes are often used instead. For example, predefined set of basic information about the user, such as their email address, username, and so on. We'll talk more about Open

## Authorization code grant type

The authorization code grant type initially looks quite complicated, but it's actually simpler than you think once you're familiar with In short, the client application and OAuth service first use redirects to exchange a series of browser-based HTTP requests that ini accept, the client application is granted an "authorization code". The client application then exchanges this code with the OAuth st relevant user data. All communication that takes place from the code/token exchange onward is sent server-to-server over a secure, preconfigured b. established when the client application first registers with the OAuth service. At this time, a client secret is also generated, ' to-server requests. As the most sensitive data (the access token and user data) is not sent via the browser, this grant type is arguably the most secur

---

Client Browser Authorization application (User-Agent) request

User (resource User log owner) and cons /callback Authorization code grant

Access token request

Access token grant

API call

User data

User logged in

1. Authorization request The client application sends a request to the OAuth service's / authorization endpoint asking for permission to access speci- use the endpoint / auth for this purpose. However, you should always be able to identify the endpoint based on the parameters

```text
GET / authorization? client id=12345&redirect_uri=https : // client—app. corn/ callback & response _ type
```

```text
Host: . corn
```

This request contains the following noteworthy parameters, usually provided in the query string:

- client id

Mandatory parameter containing the unique identifier of the client application. This value is generated when the client applicæ

- redirect uri

The URI to which the user's browser should be redirected when sending the authorization code to the client application. This based on exploiting flaws in the validation of this parameter.

- response _ type

Determines which kind of response the client application is expecting and, therefore, which flow it wants to initiate. For the al

- scope

Used to specify which subset of the user's data the client application wants to access. Note that these may be custom scope specification. We'll cover OpenID Connect in more detail later.

Stores a unique, unguessable value that is tied to the current session on the client application. The OAuth service should ret serves as a form of CSRF token for the client application by making sure that the request to its / callback endpoint is fror

2. User login and consent When the authorization server receives the initial request, it will redirect the user to a login page, where they will be prompted to media account. They will then be presented with a list of data that the client application wants to access. This is based on the scopes defined in tt access. It is important to note that once the user has approved a given scope for a client application, this step will be completed utomatic the first time the user selects "Log in with social media", they will need to manually log in and give their consent, but if th y re 't

---

If the user consents to the requested access, their browser will be redirected to the / callback endpoint that was specified in tt request will contain the authorization code as a query parameter. Depending on the configuration, it may also send the state p;

```text
GET / HTTP/ 1.1
```

```text
Host: client—app. com
```

4. Access token request Once the client application receives the authorization code, it needs to exchange it for an access token. To do this, it sends a serv communication from this point on takes place in a secure back-channel and, therefore, cannot usually be observed or controlled t

```text
POST / token HTTP/ 1.1
```

```text
Host: oauth-authorization server . corn
```

client secret=SECRET&redirect_uri=https://client—app .

In addition to the client id and authorization code , you will notice the following new parameters:

- client secret

The client application must authenticate itself by including the secret key that it was assigned when registering with the OAut grant _ type Used to make sure the new endpoint knows which grant type the client application wants to use. In this case, this should be

5. Access token grant The OAuth service will validate the access token request. If everything is as expected, the server responds by granting the client

"access token". "zOy9x8w7v6u5" , "token type" : "Bearer" , "expires in" • 3600, " scope": "open id profile" ,

6. API call Now the client application has the access code, it can finally fetch the user's data from the resource server. To do this, it makes ar in the Authorization: Bearer header to prove that the client application has permission to access this data.

```text
GET / user info HTTP/ 1.1
```

```text
Host: . com
```

Authorization: Bearer zOy9x8w7v6u5

7. Resource grant The resource server should verify that the token is valid and that it belongs to the current client application. If so, it will respond by token.

"username " : " carlos " carlos@carlos-montoya . net " ,

The client application can finally use this data for its intended purpose. In the case of OAuth authentication, it will typically be usec

## Implicit grant type

---

their consent. You may be wondering why client applications don't always use the implicit grant type. The answer is relatively simple - it is far Ies redirects - there is no secure back-channel like in the authorization code flow. This means that the sensitive access token and the The implicit grant type is more suited to single-page applications and native desktop applications, which cannot easily store the c authorization code grant type.

Client Browser Authorization application (User-Agent) request

User (resource User log owner) and cons /callback Access token grant o API c

User data

User logged in

I. Authorization request The implicit flow starts in much the same way as the authorization code flow. The only major difference is that the response_ty

```text
GET / authorization? client id—12345&redirect_uri—https://client—app.corn/callback&response_type
```

```text
Host: oauth-authorization—server . com
```

2. User login and consent The user logs in and decides whether to consent to the requested permissions or not. This process is exactly the same as for the

3. Access token grant If the user gives their consent to the requested access, this is where things start to differ. The OAuth service will redirect the user'! instead of sending a query parameter containing an authorization code, it will send the access token and other token-specific date

```text
GET / callback#access type—bearer &expires
```

```text
Host: client app.com
```

As the access token is sent in a URL fragment, it is never sent directly to the client application. Instead, the client application mus

4. API call Once the client application has successfully extracted the access token from the URL fragment, it can use it to make API calls to t also happens via the browser.

```text
GET / user info HTTP/ 1.1
```

```text
Host: oauth-resource—server. corn
```

Authorization: Bearer zOy9x8w7v6u5

5. Resource grant The resource server should verify that the token is valid and that it belongs to the current client application. If so, it will respond by

## Io

with the access token.

---

"username " : " car los " , email • carlos@ car los-montoya . net "

The client application can finally use this data for its intended purpose. In the case of OAuth authentication, it will typically be usec

Now that you know a bit more about how the different flows work, you should be able to follow our learning materials on how t OAuth authentication vulnerabilities

---
