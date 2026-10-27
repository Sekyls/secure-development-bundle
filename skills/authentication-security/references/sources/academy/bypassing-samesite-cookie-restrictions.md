# Bypassing SameSite cookie restrictions

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `1ebfbc1b18e1050ca45d224ff5a17f7e21b7c22c9cf506ad250bf52308e90c10`.
> Conversion: Local English OCR, all 5 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


> Bypassing SameSite cookie restrictions

Cross-site request forgery (CSRF) restrictions provide partial protection against a variety of cross-site attacks, including CSRF, cross-site leaks, and some CORS ex Since 2021, Chrome applies Lax SameSite restrictions by default if the website that issues the cookie doesn't explicitly set its ov proposed standard, and we expect other major browsers to adopt this behavior in the future. As a result, it's essential to have solit restrictions work, as well as how they can potentially be bypassed, in order to thoroughly test for cross-site attack vectors. In this section, we'll first cover how the SameSite mechanism works and clarify some of the related terminology. We'll then look at ways you may be able to bypass these restrictions, enabling CSRF and other cross-site attacks on websites that may initially app

## What is a site in the context of SameSite cookies?

In the context of SameSite cookie restrictions, a site is defined as the top-level domain (T LD), usually something like . com or .1 of the domain name. This is often referred to as the TLD+I When determining whether a request is same-site or not, the URL scheme is also taken into consideration. This means that a link http: / / app . example . com to https : / / app . example . com is treated as cross-site by most browsers.

scheme +1

## https://

## example.com

site

Note You may come across the term "effective top-level domain" (eTLD). This is just a way of accounting for the reserved multipart top-level domains in practice, such as . co . uk

## What's the difference between a site and an origin?

---

closely related, it's important not to use the terms interchangeably as conflating the two can have serious security implications. Two URLs are considered to have the same origin if they share the exact same scheme, domain name, and port. Although note tt from the scheme.

## origin

## https://

## example.com

site

As you can see from this example, the term "site" is much less specific as it only accounts for the scheme and last part of the dory means that a cross-origin request can still be same-site, but not the other way around. Request from Request to Same-site? Same-origin? https : / / example . corn https : / / example . com Yes Yes https : / / app . example . https : / / intranet . example . com Yes No: mismatched domain name com https : / /example . corn https://examp e.com: 8080 Yes No: mismatched port https : / / example . com https : / / example . co . uk No: mismatched eTLD No: mismatched domain name https : / / example . com http : / / example . com No: mismatched scheme No: mismatched scheme This is an important distinction as it means that any vulnerability enabling arbitrary JavaScript execution can be abused to bypass domains belonging to the same site. We'll see an example of this in one of the labs later.

## How does SameSite work?

Before the SameSite mechanism was introduced, browsers sent cookies in every request to the domain that issued them, even if an unrelated third-party website. SameSite works by enabling browsers and website owners to limit which cross-site requests, if a cookies. This can help to reduce users' exposure to CSRF attacks, which induce the victim's browser to issue a request that trigge vulnerable website. As these requests typically require a cookie associated with the victim's authenticated session, the attack will include this. All major browsers currently support the following SameSite restriction levels:

Strict Lax •

Developers can manually configure a restriction level for each cookie they set, giving them more control over when these cookies have to include the SameSite attribute in the Set-cookie response header, along with their preferred value: Set—cookie: session—OE8tgdOhi9ynRIM9wa30Da; SameSite—Strict

Although this offers some protection against CSRF attacks, none of these restrictions provide guaranteed immunity, as we'll demc vulnerable, interactive labs later in this section.

Note

---

the cookie is only sent in cross-site requests that meet specific criteria, even though the developers never configured this behi new standard, we expect other major browsers to adopt this behavior in future.

Strict If a cookie is set with the SameSite Strict attribute, browsers will not send it in any cross-site requests. In simple terms, this the request does not match the site currently shown in the browser's address bar, it will not include the cookie. This is recommended when setting cookies that enable the bearer to modify data or perform other sensitive actions, such as acce only available to authenticated users. Although this is the most secure option, it can negatively impact the user experience in cases where cross-site functionality is des

Lax Lax SameSite restrictions mean that browsers will send the cookie in cross-site requests, but only if both of the following conditü

- The request uses the GET method.

The request resulted from a top-level navigation by the user, such as clicking on a link.

This means that the cookie is not included in cross-site POST requests, for example. As POST requests are generally used to pe or state (at least according to best practice), they are much more likely to be the target of CSRF attacks. Likewise, the cookie is not included in background requests, such as those initiated by scripts, iframes, or references to images al

None If a cookie is set with the sameSite=None attribute, this effectively disables SameSite restrictions altogether, regardless of the t will send this cookie in all requests to the site that issued it, even those that were triggered by completely unrelated third-party site With the exception of Chrome, this is the default behavior used by major browsers if no SameSxte attribute is provided when se There are legitimate reasons for disabling SameSite, such as when the cookie is intended to be used from a third-party context ar access to any sensitive data or functionality. Tracking cookies are a typical example. If you encounter a cookie set with sameSite=None or with no explicit restrictions, it's worth investigating whether it's of any use behavior was first adopted by Chrome, this had the side-effect of breaking a lot of existing web functionality. As a quick workarour to simply disable SameSite restrictions on all cookies, including potentially sensitive ones. When setting a cookie with SameSite=None , the website must also include the Secure attribute, which ensures that the cook messages over HTTPS. Otherwise, browsers will reject the cookie and it won't be set. Set —Cookie: tracking ld—OF8tgdOhi 9ynRIM9wa30Da; SameSite=None; Secure

## Bypassing SameSite Lax restrictions using GET requests

In practice, servers aren't always fussy about whether they receive a GET or POST request to a given endpoint, even those that submission. If they also use Lax restrictions for their session cookies, either explicitly or due to the browser default, you may still attack by eliciting a GET request from the victim's browser. As long as the request involves a top-level navigation, the browser will still include the victim's session cookie. The following is on to launching such an attack:

```text
<scr ipt>
```

document. location = https : / / vulnerable—website . com/ account/ transfer—payment ? recipient—ha &lt; / script &gt;

Even if an ordinary GET request isn't allowed, some frameworks provide ways of overriding the method specified in the request li supports the method parameter in forms, which takes precedence over the normal method for routing purposes:

---

&lt; input type— "hidden" name=" method" value—" POST &lt; input type="hidden" value="hacker"&gt; &lt; input name=" amount" &lt; / form&gt;

Other frameworks support a variety of similar parameters.

SameSite Lax bypass via method override

## Bypassing SameSite restrictions using on-site gadgets

If a cookie is set with the SameSite—Strict attribute, browsers won't include it in any cross-site requests. You may be able to can find a gadget that results in a secondary request within the same site. One possible gadget is a client-side redirect that dynamically constructs the redirection target using attacker-controllable input like examples, see our materials on DOM-based open redirection. As far as browsers are concerned, these client-side redirects aren't really redirects at all; the resulting request is just treated as ar Most importantly, this is a same-site request and, as such, will include all cookies related to the site, regardless of any restrictions If you can manipulate this gadget to elicit a malicious secondary request, this can enable you to bypass any SameSite cookie rest

SameSite Strict bypass via client-side redirect

Note that the equivalent attack is not possible with server-side redirects. In this case, browsers recognize that the request to follov cross-site request initially, so they still apply the appropriate cookie restrictions.

## Bypassing SameSite restrictions via vulnerable sibling domains

Whether you're testing someone else's website or trying to secure your own, it's essential to keep in mind that a request can still t cross-ongn. Make sure you thoroughly audit all of the available attack surface, including any sibling domains. In particular, vulnerabilities that secondary request, such as XSS, can compromise site-based defenses completely, exposing all of the site's domains to cross-sit( In addition to classic CSRF, don't forget that if the target website supports WebSockets, this functionality might be vulnerable to cr (CSWSH), which is essentially just a CSRF attack targeting a WebSocket handshake. For more details, see our topic on WebSoc

SameSite Strict bypass via sibling domain

---

Practise exploiting vulnerabilities on realistic targets.

Record your progression from Apprentice to Expert.

See where you rank in our Hall of Fame.

Enter your email

Already got an account? Login here
