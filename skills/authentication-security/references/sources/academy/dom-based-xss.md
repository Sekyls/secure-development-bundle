# What is DOM-based XSS (cross-site scrip...rial & Examples

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `ca4c32c1a909bf53ff4a23cac1d727d5e068a176512b7f9a6d862aaf761efc83`.
> Conversion: Local English OCR, all 6 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


DOM-based

Cross-site scripting (XSS) to exploit DOM XSS with different sources and sinks.

## What is DOM-based cross-site scripting?

DOM-based XSS vulnerabilities usually arise when JavaScript takes data from an attacker-controllable source, such as the URL, passes it to a sink that supports dynamic code execution, such as eval ( ) or innerHTML . This enables attackers to execute JavaScript, which typically allows them to hijack other users' accounts. To deliver a DOM-based XSS attack, you need to place data into a source so that it is propagated to a sink and causes execution arbitrary JavaScript. The most common source for DOM XSS is the URL, which is typically accessed with the window. location object. An attacke construct a link to send a victim to a vulnerable page with a payload in the query string and fragment portions of the URL. In certa circumstances, such as when targeting a 404 page or a website running PHP, the payload can also be placed in the path. For a detailed explanation of the taint flow between sources and sinks, please refer to the DOM-based vulnerabilities page.

## How to test for DOM-based cross-site scripting

The majority of DOM XSS vulnerabilities can be found quickly and reliably using Burp Suite's web vulnerability scanner. To test fol based cross-site scripting manually, you generally need to use a browser with developer tools, such as Chrome. You need to each available source in turn, and test each one individually.

Testing HTML sinks To test for DOM XSS in an HTML sink, place a random alphanumeric string into the source (such as location . search ), then developer tools to inspect the HTML and find where your string appears. Note that the browser's "View source" option won't work XSS testing because it doesn't take account of changes that have been performed in the HTML by JavaScript. In Chrome's devel( you can use Control+F (or Command+F on MacOS) to search the DOM for your string. For each location where your string appears within the DOM, you need to identify the context. Based on this context, you need to your input to see how it is processed. For example, if your string appears within a double-quoted attribute then try to inject double your string to see if you can break out of the attribute. Note that browsers behave differently with regards to URL-encoding, Chrome, Firefox, and Safari will URL-encode location . s and location . hash , while IEII and Microsoft Edge (pre-Chromium) will not URL-encode these sources. If your data gets URL before being processed, then an XSS attack is unlikely to work.

Testing JavaScript execution sinks Testing JavaScript execution sinks for DOM-based XSS is a little harder. With these sinks, your input doesn't necessarily appear within the DOM, so you can't search for it. Instead you'll need to use the JavaScript debugger to determine whether and sent to a sink.

---

referenced. In Chrome's developer tools, you can use Control+Shift+F (or Command+A1t+F on MacOS) to search all the p JavaScript code for the source. Once you've found where the source is being read, you can use the JavaScript debugger to add a break point and follow how the value is used. You might find that the source gets assigned to other variables. If this is the case, you'll need to use the search func to track these variables and see if they're passed to a sink. When you find a sink that is being assigned data that originated from t source, you can use the debugger to inspect the value by hovering over the variable to show its value before it is sent to the sink. with HTML sinks, you need to refine your input to see if you can deliver a successful XSS attack.

Testing for DOM XSS using DOM Invader Identifying and exploiting DOM XSS in the wild can be a tedious process, often requiring you to manually trawl through complex, r JavaScript. If you use Burp's browser, however, you can take advantage of its built-in DOM Invader extension, which does a lot of work for you.

- DOM Invader documentation

## Exploiting DOM XSS with different sources and sinks

In principle, a website is vulnerable to DOM-based cross-site scripting if there is an executable path via which data can propagate source to sink. In practice, different sources and sinks have differing properties and behavior that can affect exploitability, and detc what techniques are necessary. Additionally, the website's scripts might perform validation or other processing of data that must b accommodated when attempting to exploit a vulnerability. There are a variety of sinks that are relevant to DOM-based vulnerabiliti Please refer to the list below for details. The document . write sink works with script elements, so you can use a simple payload, such as the one below: document. write ... (document . domain) &lt; / script&gt;

AppRENTlCE DOM in document . write Sink using source location . search

Note, however, that in some situations the content that is written to document . write includes some surrounding context that y to take account of in your exploit. For example, you might need to close some existing elements before using your JavaScript pay

DOM XSS in document . write sink using source location . search inside a select element

The innerHTML sink doesn't accept script elements on any modern browser, nor will svg onload events fire. This means need to use alternative elements like img or i frame . Event handlers such as onload and onerror can be used in conjunc these elements. For example: element . inner HTML=' &lt; img src—l onerror=alert (document . domain) &gt;

DOM in inner HTML Sink using source location. search

Sources and sinks in third-party dependencies Modern web applications are typically built using a number of third-party libraries and frameworks, which often provide a ditio al i and capabilities for developers. It's important to remember that some of these are also potential sources and sinks for D

---

If a JavaScript library such as jQuery is being used, look out for sinks that can alter DOM elements on the page. For instance, jQu attr ( ) function can change the attributes of DOM elements. If data is read from a user-controlled source like the URL, then pa the attr ( ) function, then it may be possible to manipulate the value sent to cause XSS. For example, here we have some Jave that changes an anchor element's href attribute using data from the URL: $ (function ( ) $ ( #backLink') . attr ("href" , (new URLSearchParams (window . location . search) ) . get ( returnU

You can exploit this by modifying the URL so that the location . search source contains a malicious JavaScript URL. After the JavaScript applies this malicious URL to the back link's href , clicking on the back link will execute it: ? returnUr1=j avascript : alert (document . domain)

DOM XSS in jQuery anchor href attribute sink using location . search source

Another potential sink to look out for is jQuery's $ ( ) selector function, which can be used to inject malicious objects into the Dor jQuery used to be extremely popular, and a classic DOM XSS vulnerability was caused by websites using this selector in conjunct the location . hash source for animations or auto-scrolling to a particular element on the page. This behavior was often impler using a vulnerable hashchange event handler, similar to the following: $ (window) . on ( 'hashchange', function ( ) $ (location . hash) ; element [O] . scroll IntoView ( )

As the hash is user controllable, an attacker could use this to inject an XSS vector into the $ ( ) selector sink. More recent vers jQuery have patched this particular vulnerability by preventing you from injecting HTML into a selector when the input begins with character ( # ). However, you may still find vulnerable code in the wild. To actually exploit this classic vulnerability, you'll need to find a way to trigger a hashchange event without user interaction. One simplest ways of doing this is to deliver your exploit via an i frame : &lt; i frame src="https://vulnerable-website.com#" .src+= '&lt;img src=l onerror=alert (1) &gt;

In this example, the src attribute points to the vulnerable page with an empty hash value. When the i frame is loaded, an XSE appended to the hash, causing the hashchange event to fire.

Note Even newer versions of jQuery can still be vulnerable via the $ ( ) selector sink, provided you have full control over its input fi source that doesn't require a # prefix.

DOM XSS in jQuery selector sink using a hashchange event

DOM XSS in AngularJS If a framework like AngularJS is used, it may be possible to execute JavaScript without angle brackets or events. When a site use app attribute on an HTML element, it will be processed by AngularJS. In this case, AngularJS will execute JavaScript inside dout braces that can occur directly in HTML or inside attributes.

---

## DOM XSS combined with reflected and stored data

Some pure DOM-based vulnerabilities are self-contained within a single page. If a script reads some data from the URL and write dangerous sink, then the vulnerability is entirely client-side. However, sources aren't limited to data that is directly exposed by browsers - they can also originate from the website. For examp websites often reflect URL parameters in the HTML response from the server. This is commonly associated with normal XSS, but lead to reflected DOM XSS vulnerabilities. In a reflected DOM XSS vulnerability, the server processes data from the request, and echoes the data into the response. The ref data might be placed into a JavaScript string literal, or a data item within the DOM, such as a form field. A script on the page then processes the reflected data in an unsafe way, ultimately writing it to a dangerous sink. eval ( 'var data = "reflected string" ;

Reflected DOM XSS

Websites may also store data on the server and reflect it elsewhere. In a stored DOM XSS vulnerability, the server receives data i request, stores it, and then includes the data in a later response. A script within the later response contains a sink which then proc data in an unsafe way. element. innerHTML = comment . author

Stored DOM XSS

## Which sinks can lead to DOM-XSS vulnerabilities?

The following are some of the main sinks that can lead to DOM-XSS vulnerabilities: document . write ( ) document . write In ( ) document . domain element . innerHTML element . outerHTML element . insertAdj acentHTML element . onevent The following jQuery functions are also sinks that can lead to DOM-XSS vulnerabilities:

---

after ( ) append ( ) animate ( ) insertAfter ( ) insertBefore ( ) before ( ) html ( ) prepend ( ) replaceA11 ( ) rep aceWith ( ) wrap ( ) wraplnner ( ) wrapAII ( ) has ( ) constructor ( ) init ( ) index ( ) jQuery . parseHTML ( ) $ . parseHTML ( )

## How to prevent DOM-XSS vulnerabilities

In addition to the general measures described on the DOM-based vulnerabilities page, you should avoid allowing data from any u source to be dynamically written to the HTML document.

---
