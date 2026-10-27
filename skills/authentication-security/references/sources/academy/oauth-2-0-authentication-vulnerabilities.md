# OAuth 2.0 authentication vulnerabilities

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `76271babc3aa7e072e52b6c9f05e1a11b44548546f2cdc508e94e7574a723e4d`.
> Conversion: Local English OCR, all 7 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


> OAuth authentication

OAuth 2.0 authentication vulnerabilities interesting for attackers because it is both extremely common and inherently prone to implementation mistakes. This can result in authentication completely. In this section, we'll teach you how to identify and exploit some of the key vulnerabilities found in OAuth 2.0 authentication mecha background information to help you understand the key concepts you'll need. We'll also explore some vulnerabilities in OAuth's O applications against these kinds of attacks. This topic was written in collaboration with PortSwigger Research, alongside the paper Hidden OAuth Attack Vectors.

https : / / vulnerable -oauth-: horization?client idzinnoc Yi—evil - user. net &response

Authenticated Access token

If you're already familiar with the basic concepts behind OAuth vulnerabilities and just want to practice exploiting them on som below. View all OAuth labs

## What is OAuth?

OAuth is a commonly used authorization framework that enables websites and web applications to request limited access to a usi exposing their login credentials to the requesting application. This means users can fine-tune which data they want to share rathe The basic OAuth process is widely used to integrate third-party functionality that requires access to certain data from a user's acc that it can suggest people to connect with. However, the same mechanism is also used to provide third-party authentication servic

Note

---

Please be aware that the term "OAuth" refers exclusively to OAuth 2.0 throughout these materials.

## How does OAuth 2.0 work?

OAuth 2.0 was originally developed as a way of sharing access to specific data between applications. It works by defining a serie: the OAuth service provider.

- Client application - The website or web application that wants to access the user's data.

- Resource owner - The user whose data the client application wants to access.

- OAuth service provider - The website or application that controls the user's data and access to it. They support OAuth by p

There are numerous different ways that the actual OAuth process can be implemented. These are known as OAuth "flows" or "græ by far the most common. Broadly speaking, both of these grant types involve the following stages:

1. The client application requests access to a subset of the user's data, specifying which grant type they want to use and what I 2. The user is prompted to log in to the OAuth service and explicitly give their consent for the requested access. 3. The client application receives a unique access token that proves they have permission from the user to access the requeste 4. The client application uses this access token to make API calls fetching the relevant data from the resource server.

Before learning how OAuth is used for authentication, it's important to understand the fundamentals of this basic OAuth process. I the grant types we're going to cover before reading further.

OAuth grant types

OAuth authentication Although not originally intended for this purpose, OAuth has evolved into a means of authenticating users as well. For example, yt media account rather than having to register with the website in question. Whenever you see this option, there's a good chance it For OAuth authentication mechanisms, the basic OAuth flows remain largely the same; the main difference is how the client appli authentication is something that broadly resembles SAML-based single sign-on (SSO). In these materials, we'll focus exclusively OAuth authentication is generally implemented as follows:

1 . The user chooses the option to log in with their social media account. The client application then uses the social media site's email address that is registered with their account, for example. 2. After receiving an access token, the client application requests this data from the resource server, typically from a dedicated 3. Once it has received the data, the client application uses it in place of a username to log the user in. The access token that it

You can see a simple example of how this looks in the following lab. Just complete the "Log in with social media" option while pro log in using the credentials wiener : peter . Note that this implementation is deliberately vulnerable - we'll teach you how to exp

Authentication bypass via OAuth implicit flow

## How do OAuth authentication vulnerabilities arise?

OAuth authentication vulnerabilities arise partly because the OAuth specification is relatively vague and flexible by design. Althou! type, the vast majority of the implementation is completely optional. This includes many configuration settings that are necessary One of the other key issues with OAuth is the general lack of built-in security features. The security relies almost entirely on devel security measures on top, such as robust input validation. As you've probably gathered, there's a lot to take in and this i quiteea Depending on the grant type, highly sensitive data is also sent via the browser, which presents various opportunities for n a @ k'

---

Recognizing when an application is using OAuth authentication is relatively straightforward. If you see an option to log in using yo The most reliable way to identify OAuth authentication is to proxy your traffic through Burp and check the corresponding HTTP me first request of the flow will always be a request to the / authorization endpoint containing a number of query parameters the redirect uri , and response _ type parameters. For example, an authorization request will usually look something like this

```text
GET / authorization? client id=12345&redirect_uri=https : .
```

```text
Host: oauth-authorization server.com
```

## Recon

Doing some basic recon of the OAuth service being used can point you in the right direction when it comes to identifying vulnerab It goes without saying that you should study the various HTTP interactions that make up the OAuth flow - we'll go over some spec the specific provider from the hostname to which the authorization request is sent. As these services provide a public API, there is the exact names of the endpoints and which configuration options are being used. Once you know the hostname of the authorization server, you should always try sending a GET request to the following standard

/ . well—known/oauth authorization server / . well—known/ openid—configuration

These will often return a JSON configuration file containing key information, such as details of additional features that may be sup not be mentioned in the documentation.

## Exploiting OAuth authentication vulnerabilities

Vulnerabilities can arise in the client application's implementation of OAuth as well as in the configuration of the OAuth service itse these contexts. Vulnerabilities in the client application

- Improper implementation of the implicit grant type

Flawed CSRF protection Vulnerabilities in the OAuth service

- Leaking authorization codes and access tokens LABS

- Flawed scope validation

- Unverified user registration

Vulnerabilities in the OAuth client application Client applications will often use a reputable, battle-hardened OAuth service that is well protected against widely known exploits. I As we've already mentioned, the OAuth specification is relatively loosely defined. This is especially true with regard to the implem optional parameters and configuration settings in each grant type, which means there's plenty of scope for misconfigurations.

Improper implementation of the implicit grant type Due to the dangers introduced by sending access tokens via the browser, the implicit grant type is mainly recommended for single of its relative simplicity. In this flow, the access token is sent from the OAuth service to the client application via the user's browser as a URL fragment. Th to maintain the session after the user closes the page, it needs to store the current user data (normally a user ID and the access t To solve this problem, the client application will often submit this data to the server in a POST request and then assign the user a submission request that might be sent as part of a classic, password-based login. However, in this scenario, the server does not trusted. In the implicit flow, this POST request is exposed to attackers via their browser. As a result, this behavior can lead to a serious vu data in the request. In this case, an attacker can simply change the parameters sent to the server to impersonate any user.

---

Authentication bypass via OAuth implicit flow

Flawed CSRF protection Although many components of the OAuth flows are optional, some of them are strongly recommended unless there's an importan The state parameter should ideally contain an unguessable value, such as the hash of something tied to the user's session wh application and the OAuth service as a form of CSRF token for the client application. Therefore, if you notice that the authorizatior perspective. It potentially means that they can initiate an OAuth flow themselves before tricking a user's browser into completing il OAuth is being used by the client application. Consider a website that allows users to log in using either a classic, password-based mechanism or by linking their account to a s an attacker could potentially hijack a victim user's account on the client application by binding it to their own social media account

Forced OAuth profile linking

Note that if the site allows users to log in exclusively via OAuth, the state parameter is arguably less critical. However, not usin tricked into logging in to the attacker's account.

Leaking authorization codes and access tokens Perhaps the most infamous OAuth-based vulnerability is when the configuration of the OAuth service itself enables attackers to st code or token, the attacker may be able to access the victim's data. Ultimately, this can completely compromise their account - the this OAuth service. Depending on the grant type, either a code or token is sent via the victim's browser to the / callback endpoint specified in the URI properly, an attacker may be able to construct a CSRF-like attack, tricking the victim's browser into initiating an OAuth flow th In the case of the authorization code flow, an attacker can potentially steal the victim's code before it is used. They can then send to get access to the user's account. In this scenario, an attacker does not even need to know the client secret or the resulting acce simply complete the code/token exchange on the attacker's behalf before logging them in to the victim's account. Note that using state or nonce protection does not necessarily prevent these attacks because an attacker can generate new

OAuth account hijacking via redirect_uri

More secure authorization servers will require a redirect uri parameter to be sent when exchanging the code as well. The s and reject the exchange if not. As this happens in server-to-server requests via a secure back-channel, the attacker is not able to

Flawed redirect uri validation Due to the kinds of attacks seen in the previous lab, it is best practice for client applications to provide a whitelist of their genuine a new request, it can validate the redirect uri parameter against this whitelist. In this case, supplying an external URI will lik When auditing an OAuth flow, you should try experimenting with the redirect_uri parameter to understand how it is being va

- Some implementations allow for a range of subdirectories by checking only that the string starts with the correct sequence of

parameters, and fragments to see what you can change without triggering an error.

- If you can append extra values to the default redirect uri parameter, you might be able to exploit discrepancies betwee

try techniques such as: https://default—host.com . . evil—user . net/ If you're not familiar with these techniques, we recommend reading our content on how to circumvent common SSRF defenc You may occasionally come across server-side parameter pollution vulnerabilities. Just in case, you should try submitting dul https://oauth authorization server. corn/ ? client ld—123 & redirect uri=cLient app . com/ca

---

environment. This could allow you to bypass the validation by registering a domain name such as localhost . evil-user

It is important to note that you shouldn't limit your testing to just probing the redirect uri parameter in isolation. In the wild, y Sometimes changing one parameter can affect the validation of others. For example, changing the response mode from quer to submit URIs that would otherwise be blocked. Likewise, if you notice that the web _ message response mode is supported, thi:

Stealing codes and access tokens via a proxy page Against more robust targets, you might find that no matter what you try, you are unable to successfully submit an external domain By this stage, you should have a relatively good understanding of which parts of the URI you can tamper with. The key now is to other words, try to work out whether you can change the redirect uri parameter to point to any other pages on a whitelisted Try to find ways that you can successfully access different subdomains or paths. For example, the default URI will often be on an subdirectories. However, you may be able to use directory traversal tricks to supply any arbitrary path on the domain. Something https : / /clxent-app . com/oauth/callback/ . . / . . / example/ path

May be interpreted on the back-end as: https : / / client-app . com/example/path

Once you identify which other pages you are able to set as the redirect URI, you should audit them for additional vulnerabilities th find a vulnerability that gives you access to the query parameters, whereas for the implicit grant type, you need to extract the URL One of the most useful vulnerabilities for this purpose is an open redirect. You can use this as a proxy to forward victims, along wi like. Note that for the implicit grant type, stealing an access token doesn't just enable you to log in to the victim's account on the client make your own API calls to the OAuth service's resource server. This may enable you to fetch sensitive user data that you cannot

Stealing OAuth access tokens via an open redirect

In addition to open redirects, you should look for any other vulnerabilities that allow you to extract the code or token and send it to

Dangerous JavaScript that handles query parameters and URL fragments For example, insecure web messaging scripts can be great for this. In some scenarios, you may have to identify a longer gac your external domain. XSS vulnerabilities Although XSS attacks can have a huge impact on their own, there is typically a small time frame in which the attacker has ac is commonly used for session cookies, an attacker will often also be unable to access them directly using XSS. However, by browser. This gives them much more time to explore the user's data and perform harmful actions, significantly increasing the

- HTML injection vulnerabilities

In cases where you cannot inject JavaScript (for example, due to CSP constraints or strict filtering), you may still be able to u parameter to a page on which you can inject your own HTML content, you might be able to leak the code via the Referer When attempting to fetch this image, some browsers (such as Firefox) will send the full URL in the Referer header of the

Stealing OAuth access tokens via a proxy page

Flawed scope validation In any OAuth flow, the user must approve the requested access based on the scope defined in the authorization request. The resl But in some cases, it may be possible for an attacker to "upgrade" an access token (either stolen or obtained using a malicious cli for doing this depends on the grant type.

Scope upgrade: authorization code flow

---

achieve the same result by registering their own client application with the OAuth service. For example, let's say the attacker's malicious client application initially requested access to the user's email address using the o an authorization code. As the attacker controls their client application, they can add another scope parameter to the code/token

```text
POST / token
```

```text
Host: . corn
```

client secret —SECRET & redirect uri=https://client—app . corn/

If the server does not validate this against the scope from the initial authorization request, it will sometimes generate an access to

access token": "zOy9x8w7v6u5 't "token type" : "Bearer" , " expires in" • 3600, " scope": "openid email profile" ,

The attacker can then use their application to make the necessary API calls to access the user's profile data.

Scope upgrade: implicit flow For the implicit grant type, the access token is sent via the browser, which means an attacker can steal tokens associated with inr send a normal browser-based request to the OAuth service's / userinfo endpoint, manually adding a new scope parameter i Ideally, the OAuth service should validate this scope value against the one that was used when generating the token, but this is granted to this client application, the attacker can potentially access additional data without requiring further approval from the use

Unverified user registration When authenticating users via OAuth, the client application makes the implicit assumption that the information stored by the OAut Some websites that provide an OAuth service allow users to register an account without verifying all of their details, including thei provider using the same details as a target user, such as a known email address. Client applications may then allow the attacker t

## Extending OAuth with OpenID Connect

When used for authentication, OAuth is often extended with an OpenID Connect layer, which provides some additional features re more labs related to vulnerabilities that they can introduce, see our OpenID Connect topic.

OpenID Connect

## Preventing OAuth authentication vulnerabilities

For developers, we've provided some guidance on how you can avoid introducing these vulnerabilities into your own websites anc

How to prevent OAuth authentication vulnerabilities

---

Practise exploiting vulnerabilities on realistic targets.

Record your progression from Apprentice to Expert.

See where you rank in our Hall of Fame.

Enter your email

Already got an account? Login here
