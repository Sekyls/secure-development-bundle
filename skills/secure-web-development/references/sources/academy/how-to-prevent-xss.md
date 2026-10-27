# How to prevent XSS

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `f9fa49be7396c2f7b46f88ee0dd412a2d4e31d3b9d39a836c42be303e4520d41`.
> Conversion: Local English OCR, all 5 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


Cross-site scripting (XSS) common technologies for protecting against XSS attacks. Cross-site scripting prevention can generally be achieved via two layers of defense:

- Encode data on output

- Validate input on arrival

## Encode data on output

Encoding should be applied directly before user-controllable data is written to a page, because the context you're writing into determines what kind of encoding you need to use. For example, values inside a JavaScript string require a different type of escaping to those in an HTML context. In an HTML context, you should convert non-whitelisted values into HTML entities:

. &lt; converts to: &amp;lt; . &gt; converts to: &gt; In a JavaScript string context, non-alphanumeric values should be Unicode-escaped:

. &lt; converts to: \u003c . &gt; converts to: \u003e Sometimes you'll need to apply multiple layers of encoding, in the correct order. For example, to safely embed user input inside al event handler, you need to deal with both the JavaScript context and the HTML context. So you need to first Unicode-escape the input, and then HTML-encode it:

```text
<a 'This string needs two layers of escaping'
```

## Validate input on arrival

Encoding is probably the most important line of XSS defense, but it is not sufficient to prevent XSS vulnerabilities in every context You should also validate input as strictly as possible at the point when it is first received from a user. Examples of input validation include:

If a user submits a URL that will be returned in responses, validating that it starts with a safe protocol such as HTT HTTPS. Otherwise someone might exploit your site with a harmful protocol like javascript or data

---

- Validating that input contains only an expected set of characters.

Input validation should ideally work by blocking invalid input. An alternative approach, of attempting to clean invalid input to make it valid, is more error prone and should be avoided wherever possible.

Whitelisting vs blacklisting Input validation should generally employ whitelists rather than blacklists. For example, instead of trying to make a list of all harmfu protocols ( javascript , data , etc.), simply make a list of safe protocols (HTTP, HTTPS) and disallow anything not on the list. This will ensure your defense doesn't break when new harmful protocols appear and make it less susceptible to attacks that seek to obfuscate invalid values to evade a blacklist.

## Allowing "safe" HTML

Allowing users to post HTML markup should be avoided wherever possible, but sometimes it's a business requirement. For example, a blog site might allow comments to be posted containing some limited HTML markup. The classic approach is to try to filter out potentially harmful tags and JavaScript. You can try to implement this using a whitelist of safe tags and attributes, but thanks to discrepancies in browser parsing engines and quirks like mutation XSS, this approach is extremely difficult to implement securely. The least bad option is to use a JavaScript library that performs filtering and encoding in the user's browser, such as DOMPurify. Other libraries allow users to provide content in markdown format and convert the markdown into HTML. Unfortunately, all these libraries have XSS vulnerabilities from time to time, so this is not a perfect solution. If you do use one you should monitor closely for security updates.

Note In addition to JavaScript, other content such as CSS and even regular HTML can be harmful in some situations. Attacks using malicious CSS

## How to prevent XSS using a template engine

Many modern websites use server-side template engines such as Twig and Freemarker to embed dynamic content in HTML. These typically define their own escaping system. For example, in Twig, you can use the e ( ) filter, with an argument defining context: { { user. firstname I e ('html')

Some other template engines, such as Jinja and React, escape dynamic content by default which effectively prevents most occurrences of XSS. We recommend reviewing escaping features closely when you evaluate whether to use a given template engine or framework.

Note If you directly concatenate user input into template strings, you will be vulnerable to server-side template injection which is often more serious than XSS

## How to prevent XSS in PHP

In PHP there is a built-in function to encode entities called htmlentities . You should call this function to escape your input when inside an HTML context. The function should be called with three arguments:

. Your input string.

---

- The character set, which in most cases should be UTF-8.

For example: echo htmlentities ($input, ENT QUOTES, 'UTF-8') ;

When in a JavaScript string context, you need to Unicode-escape input as already described. Unfortunately, PHP doesn't provide an API to Unicode-escape a string. Here is some code to do that in PHP:

< ? php

```text
function j s Escape ($str) {
```

$output — $str = str split ($str) ; for ($str) ;$i++) $ chrNum = ord($str[$i]); $chr = $str[$i]• if ($chrNum === 226) if (is set ($str[$i+l)) && — 128) if (isset($strC$i+2]) && ord($str[$i+2]) — 168) $output = '\u2028'

continue;

if (isset ($strC$i+2)) && 169) $output '\u2029',

continue;

switch ($chr) case case case case case case case case $output $chrNum) ; sprxntf ( break; default : $output $str[$i]; break;

return $output;

Here is how to use the jsEscape function in PHP: echo jsEscape($ GET C' ; &lt; / script &gt;

Alternatively, you could use a template engine.

---

To escape user input in an HTML context in JavaScript, you need your own HTML encoder because JavaScript doesn't provide ar API to encode HTML. Here is some example JavaScript code that converts a string to HTML entities:

```text
function htmlEncode (str) {
```

return String (str) . replace ( / [A \ w. ]/gi, function (c) { return ' (0)+' •

You would then use this function as follows:

```text
<script>document.body. innerHTML =
```

htmlEncode (untrustedVa1ue) &lt; / script &gt;

If your input is inside a JavaScript string, you need an encoder that performs Unicode escaping. Here is a sample Unicode- encoder:

```text
function jsEscape (str) {
```

return String (str) . replace ( / CA \ w. ] / g i, function (c) { return u ' + ( ' 0000 charCodeAt (O) .toString (16)) . slice

You would then use this function as follows: . write ( ' &lt;script&gt;x=" ' +j sEscape (untrustedVaIue) + ' " ; &lt; \ / script &gt; ) &lt;/ script &gt;

## How to prevent XSS in jQuery

The most common form of XSS in jQuery is when you pass user input to a jQuery selector. Web developers would often use location . hash and pass it to the selector which would cause XSS as jQuery would render the HTML. jQuery recognized this issue and patched their selector logic to check if input begins with a hash. Now jQuery will only render HTML if the first character isa < . If you pass untrusted data to the jQuery selector, ensure you correctly escape the value using the jsEscape function above.

## Mitigating XSS using content security policy (CSP)

Content security policy (CSP) is the last line of defense against cross-site scripting. If your XSS prevention fails, you can use CSF to mitigate XSS by restricting what an attacker can do. CSP lets you control various things, such as whether external scripts can be loaded and whether inline scripts will be executed. deploy CSP you need to include an HTTP response header called content Security Policy with a value containing your policy. An example CSP is as follows: default—src ' self' • script-src self' • object-src ' none'; frame-src 'none' base-uri ' none'

This policy specifies that resources such as images and scripts can only be loaded from the same origin as the main page. So even if an attacker can successfully inject an XSS payload they can only load resources from the current origin. This greatly reduces the chance that an attacker can exploit the XSS vulnerability. If you require loading of external resources, ensure you only allow scripts that do not aid an attacker to exploit your site. For example, if you whitelist certain domains then an attacker can load any script from those domains. Where possible, try to host resources on your own domain. If that is not possible then you can use hash- or nonce-based policy to allow scripts on different domains. A nonce is a random string that is added as an attribute of a script or resource, which will only be executed if the random string matches the server— generated one. An attacker is unable to guess the randomized string and therefore cannot invoke a script or resource wi h a valid nonce and so the resource will not be executed.

---

- Mitigating XSS attacks using CSP


## Verified transcription examples

These literal samples were visually compared with the source. They illustrate different contexts; prefer framework text insertion rather than constructing event-handler markup.

| Context | Source example |
| --- | --- |
| HTML text | `<` becomes `&lt;`; `>` becomes `&gt;` |
| JavaScript string | `<` becomes `\u003c`; `>` becomes `\u003e` |

```html
<a href="#" onclick="x='This string needs two layers of escaping'">test</a>
```
