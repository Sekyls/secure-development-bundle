# CL.0 request smuggling

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `c9e10edc2414f7d503a42ba85c664ce8ca36b0133ca207acec1ff317977b63d8`.
> Conversion: Local English OCR, all 3 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


CL.O request smuggling

HTTP request smuggling starts and ends. This is typically due to inconsistent header parsing, leading to one server using a request's content- Length and the other treating the message as chunked. However, it's possible to perform many of the same attacks without relying on either of these issues. In some instances, servers can be persuaded to ignore the Content-Length header, meaning they assume that each request finishes at the end of the headers. This is effectively the same as treating the Content-Length as 0 . If the back-end server exhibits this behavior, but the front-end still uses the content-Length header to determine where the request ends, you can potentially exploit this discrepancy for HTTP request smuggling. We've decided to call this a "CL.O" vulnerability.

PortSwigger Research The materials and labs in this section are based on Browser-Powered Desync Attacks: A New Frontier in HTTP Request Smuggling by PortSwigger Research.

## Testing for CL.O vulnerabilities

To probe for CL.O vulnerabilities, first send a request containing another partial request in its body, then send a normal follow-up request. You can then check to see whether the response to the follow-up request was affected by the smuggled prefix. In the following example, the follow-up request for the home page has received a 404 response. This strongly suggests that the back-end server interpreted the body of the POST request ( GET / hopefully404 ... ) as the start of another request.

```text
POST / vulnerable—endpoint HTTP/ 1.1
```

```text
Host: vulnerable—website. corn
```

Connection: keep—alive Content—Type: application/x-www-form-urlencoded Content —Length: 34

```text
GET /hopefu11y404 HTTP/ 1.1
```

Foo: xGET / HTTP/ 1.1

```text
Host: vulnerable—website . corn
```

HTTP/ 1.1 200 0K

---

HTTP/ 1.1 404 Not Found

Crucially, notice that we haven't tampered with the headers in any way - the length of the request is specified by a perfect normal, accurate Content-Length header. To try this yourself using Burp Repeater:

1. Create one tab containing the setup request and another containing an arbitrary follow-up request. 2. Add the two tabs to a group in the correct order. 3. Using the drop-down menu next to the Send button, change the send mode to Send group in sequence (single connection). 4. Change the Connection header to keep-alive 5. Send the sequence and check the responses.

In the wild, we've mostly observed this behavior on endpoints that simply aren't expecting POST requests, so they implicitly assume that no requests have a body. Endpoints that trigger server-level redirects and requests for static files are prime candidates.

## Eliciting CL.O behavior

If you can't find any endpoints that appear vulnerable, you can try eliciting this behavior instead. When a request's headers trigger a server error, some servers issue an error response without consuming the request body off the socket. If they don't close the connection afterwards, this can provide an alternative CL.O desync vector. You can also try using GET requests with an obfuscated Content-Length header. If you're able to hide this from the back-end server but not the front-end, this also has the potential to cause a desync. We looked at some header obfuscation techniques when we covered TE.TE request smuggling.

## Exploiting CL.O vulnerabilities

You can exploit CL.O vulnerabilities for the same server-side request smuggling attacks we've covered in our previous request smuggling materials. Try this for yourself with the following lab.

CL.O request smuggling

## H2.O vulnerabilities

Websites that downgrade HTTP/2 requests to HTTP/I may be vulnerable to an equivalent "H2.O" issue if the back-end server ignores the content-Length header of the downgraded request.

## How to prevent CL.O vulnerabilities

For some high-level measures you can take to prevent CL.O vulnerabilities, and other forms of desync attack, see How to prevent HTTP request smuggling vulnerabilities.

What next?

You can expand on what you've learned in this section to perform client-side variations of a number of request smuggling attacks. To learn how, check out Client-side desync attacks

---

Practise exploiting vulnerabilities on realistic targets.

Record your progression from Apprentice to Expert.

See where you rank in our Hall of Fame.

Enter your email

Already got an account? Login here
