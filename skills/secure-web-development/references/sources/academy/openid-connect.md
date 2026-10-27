# OpenID Connect

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `8a65e21adc44e89da12038e908e70bf294e64113c233d5718aed7c3e06aab536`.
> Conversion: Local English OCR, all 4 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


> OpenID Connect

OAuth 2.0 authentication vulnerabilities of the vulnerabilities we're going to cover. If you're new to OAuth and OpenID Connect specifically, we recommend reading this section before attempting to complete our OpenID-based labs.

## What is OpenID Connect?

OpenID Connect extends the OAuth protocol to provide a dedicated identity and authentication layer that sits on top of the basic OAuth implementation. It adds some simple functionality that enables better support for the authentication use case of OAuth. OAuth was not initially designed with authentication in mind; it was intended to be a means of delegating authorizations for specific resources between applications. However, many websites began customizing OAuth for use as an authentication mechanism. To achieve this, they typically requested read access to some basic user data and, if they were granted this access, assumed that the user authenticated themselves on the side of the OAuth provider. These plain OAuth authentication mechanisms were far from ideal. For a start, the client application had no way of knowing when, where, or how the user was authenticated. As each of these implementations was a custom workaround of sorts, there was also no standard way of requesting user data for this purpose. To support OAuth properly, client applications would have to configure separate OAuth mechanisms for each provider, each with different endpoints, unique sets of scopes, and so on. OpenID Connect solves a lot of these problems by adding standardized, identity-related features to make authentication via OAuth work in a more reliable and uniform way.

## How does OpenID Connect work?

OpenID Connect slots neatly into the normal OAuth flows. From the client application's perspective, the key difference is that there is an additional, standardized set of scopes that are the same for all providers, and an extra response type: id token

OpenID Connect roles The roles for OpenID Connect are essentially the same as for standard OAuth. The main difference is that the specification uses slightly different terminology.

- Relying party - The application that is requesting authentication of a user. This is synonymous with the OAuth client

application.

- End user - The user who is being authenticated. This is synonymous with the OAuth resource owner.

OpenID provider - An OAuth service that is configured to support OpenID Connect.

OpenID Connect claims and scopes

---

example ofa claim could be "family name" : "Montoya" Unlike basic OAuth, whose scopes are unique to each provider, all OpenID Connect services use an identical set of scopes. In order to use OpenID Connect, the client application must specify the scope open id in the authorization request. They can then include one or more of the other standard scopes:

profile email • address phone

Each of these scopes corresponds to read access for a subset of claims about the user that are defined in the OpenID specification. For example, requesting the scope open id profile will grant the client application read access to a series of claims related to the user's identity, such as family name , given name , birth date , and so on.

ID token The other main addition provided by OpenID Connect is the id token response type. This returns a JSON web token (JWT) signed with a JSON web signature (JWS). The JWT payload contains a list of claims based on the scope that was initially requested. It also contains information about how and when the user was last authenticated by the OAuth service. The client application can use this to decide whether or not the user has been sufficiently authenticated. The main benefit of using id token is the reduced number of requests that need to be sent between the client application and the OAuth service, which could provide better performance overall. Instead of having to get an access token and then request the user data separately, the ID token containing this data is sent to the client application immediately after the user has authenticated themselves. Rather than simply relying on a trusted channel, as happens in basic OAuth, the integrity of the data transmitted in an ID token is based on a JWT cryptographic signature. For this reason, the use of ID tokens may help protect against some man-in-the-middle attacks. However, given that the cryptographic keys for signature verification are transmitted over the same network channel (normally exposed on / . well-known/j wks . j son ), some attacks are still possible. Note that multiple response types are supported by OAuth, so it's perfectly acceptable for a client application to send an authorization request with both a basic OAuth response type and OpenID Connect's id token response type: response type=id token token response_type=id token code

In this case, both an ID token and either a code or access token will be sent to the client application at the same time.

## Identifying OpenID Connect

If OpenID connect is actively being used by the client application, this should be obvious from the authorization request. The most foolproof way to check is to look for the mandatory open id scope. Even if the login process does not initially appear to be using OpenID Connect, it is still worth checking whether the OAuth service supports it. You can simply try adding the openid scope or changing the response type to id token and observing whether this results in an error. As with basic OAuth, it's also a good idea to take a look at the OAuth provider's documentation to see if there's any useful information about their OpenID Connect support. You may also be able to access the configuration file from the standard endpoint / . well known/ openid configuration .

## OpenID Connect vulnerabilities

The specification for OpenID Connect is much stricter than that of basic OAuth, which means there is generally less potential for quirky implementations with glaring vulnerabilities. That said, as it is just a layer that sits on top of OAuth, th

---

you might have noticed that all of our OAuth authentication labs also use OpenID Connect. In this section, we'll look at some additional vulnerabilities that may be introduced by some of the extra features of OpenID Connect.

Unprotected dynamic client registration The OpenID specification outlines a standardized way of allowing client applications to register with the OpenID provider. If dynamic client registration is supported, the client application can register itself by sending a POST request to a dedicated / registration endpoint. The name of this endpoint is usually provided in the configuration file and documentation. In the request body, the client application submits key information about itself in JSON format. For example, it will often be required to include an array of whitelisted redirect URIs. It can also submit a range of additional information, such as the names of the endpoints they want to expose, a name for their application, and so on. A typical registration request may look something like this:

```text
POST / openid/register HTTP/ 1.1
```

Content—Type: application/ j son Accept: application/ j son

```text
Host: oauth-authorization—server . com
```

Authorization: Bearer ab12cd34ef56gh89

" application type" : web " "redirect ur is" : "https : / / client—app . com/callback" "https : / / client-app . com/callback2 "

"client name" : "My Application" , " logo ur i' "https : / / client-app . com/logo . png" , "token endpoint auth method" : "client secret basic" , "j wks ur i": "https : // client—app . com/my public _ keys .jwks" , "user info_encrypted response alg • ' 'RSAI 5", "user info encrypted response enc" • "A128CBC-HS256"

The OpenID provider should require the client application to authenticate itself. In the example above, they're using an HTTP bearer token. However, some providers will allow dynamic client registration without any authentication, which enables an attacker to register their own malicious client application. This can have various consequences depending on how the values of these attacker-controllable properties are used. For example, you may have noticed that some of these properties can be provided as URIs. If any of these are accessed by the OpenID provider, this can potentially lead to second-order SSRF vulnerabilities unless additional security measures are in place.

SSRF via OpenID dynamic client registration

Allowing authorization requests by reference Up to this point, we've looked at the standard way of submitting the required parameters for the authorization request i.e. via the query string. Some OpenID providers give you the option to pass these in as a JSON web token (JWT) instead. If this feature is supported, you can send a single request_uri parameter pointing to a JSON web token that contains the rest of the OAuth parameters and their values. Depending on the configuration of the OAuth service, this request ur parameter is another potential vector for SSRF.

---

validate the query string in the authorization request, but may fail to adequately apply the same validation to parameters in a JWT, including the redirect uri To check whether this option is supported, you should look for the request ur i _ parameter supported option in the configuration file and documentation. Alternatively, you can just try adding the request uri parameter to see if it works. You will find that some servers support this feature even if they don't explicitly mention it in their documentation.
