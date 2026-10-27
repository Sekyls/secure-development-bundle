# Preventing prototype pollution vulnerabilities

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `41ea51b33ef915b4adcaf53c478384dc22fbb40b1641f924a63ba96e6176036c`.
> Conversion: Local English OCR, all 3 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


Prototype pollution are coupled with exploitable gadgets. Even if you're confident that you haven't missed any, there's no guarantee that future updates to your own code or any libraries you use won't introduce new gadgets, paving the way for viable exploits. In this section, we'll provide some high-level advice on some of the measures you can take to protect your own websites from the threats we've covered in our labs. We'll also cover some common pitfalls to avoid.

## Sanitizing property keys

One of the more obvious ways to prevent prototype pollution vulnerabilities is to sanitize property keys before merging them into existing objects. This way, you can prevent an attacker from injecting keys such as proto , which reference the object's prototype. Using an allowlist of permitted keys is the most effective way to do this. However, as this is not feasible in many cases, it's common to use a blocklist instead, removing any potentially dangerous strings from user input. Although this is a quick fix to implement, truly robust blocklisting is inherently tricky, as demonstrated by sites that successfully block proto but fail to account for an attacker polluting an object's prototype via its constructor. Likewise, weak implementations can also be bypassed using simple obfuscation techniques. For this reason, we only recommend this as a stopgap rather than a long-term solution.

## Preventing changes to prototype objects

A more robust approach to preventing prototype pollution vulnerabilities is to prevent prototype objects from being changed at all. Invoking the Obj ect . freeze ( ) method on an object ensures that its properties and their values can no longer be modified, and no new properties can be added. As prototypes are just objects themselves, you can use this method to proactively cut off any potential sources: Obj ect. freeze (Obj ect . prototype) ;

The Obj ect . seal ( ) method is similar, but still allows changes to the values of existing properties. This may be a good compromise if you're unable to use Obj ect . freeze ( ) for any reason.

## Preventing an object from inheriting properties

While you can use Obj ect . freeze ( ) to block potential prototype pollution sources, you can also take measures to eliminate gadgets. This way, even if an attacker identifies a prototype pollution vulnerability, it is likely to be unexploitable. By default, all objects inherit from the global Obj ect . prototype either directly or indirectly via the prototype chain. However, you can also manually set an object's prototype by creating it using the Obj ect . create ( ) method. Not onl does this let you assign any object you like as the new object's prototype, you can also create the object with a null prototype, which ensures that it won't inherit any properties at all.

---

Obj ect . get PrototypeOf (myObj ect) ; // null

## Using safer alternatives where possible

Another robust defense against prototype pollution is to use objects that provide built-in protection. For example, when defining an options object, you could use a Map instead. Although a map can still inherit malicious properties, they have a built-in get ( ) method that only returns properties that are defined directly on the map itself: Object . prototype . evil — 'polluted' •

```text
let options —
```

new Map ( ) ; options. set ( 'transport url https : / / normal—website . corn' )

options . evil; // 'polluted' options. get ( 'evil ' ) ; // undefined options . get ('transport ur 1 ' ) • // 'https://normal—website . corn'

A set is another alternative if you're just storing values rather than key: value pairs. Just like maps, sets provide built- in methods that only return properties that were defined directly on the object itself: Obj ect . prototype . evil = 'polluted',

```text
let options =
```

new Set(); options . add ( ' safe');

options . evil; // 'polluted' option . has ('evil'); options . has ( ' safe'); // true

---

pollution vulnerabilities using

TRY FOR FREE
