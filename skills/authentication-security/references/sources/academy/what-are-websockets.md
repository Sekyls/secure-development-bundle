# What are WebSockets_

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `7a40ced42be3d6d61002aa7d0cc8924d9b8719cc966aa3c45edc4b39b85e107c`.
> Conversion: Local English OCR, all 3 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


> What are WebSockets?

WebSockets and other asynchronous traffic. In this section, we'll explain the difference between HTTP and WebSockets, describe how WebSocket connections are establishe messages look like.

## What is the difference between HTTP and WebSockets?

Most communication between web browsers and web sites uses HTTP. With HTTP, the client sends a request and the server retu response occurs immediately, and the transaction is complete. Even if the network connection stays open, this will be used for a s and a response. Some modern web sites use WebSockets. WebSocket connections are initiated over HTTP and are typically long-lived. Message: any time and are not transactional in nature. The connection will normally stay open and idle until either the client or the server is WebSockets are particularly useful in situations where low-latency or server-initiated messages are required, such as real-time fe

## How are WebSocket connections established?

WebSocket connections are normally created using client-side JavaScript like the following:

var new WebSocket ("wss : / / normal-website . com/chat") ;

Note The wss protocol establishes a WebSocket over an encrypted TLS connection, while the protocol uses an unencrypted

To establish the connection, the browser and server perform a WebSocket handshake over HTTP. The browser issues a WebSoc following:

```text
GET /chat HTTP/ 1.1
```

```text
Host: normal—website . com
```

Sec—WebSocket—Version: 13 Sec—WebSocket —Key: wDqumtseNBJdhkihL6PW7w=— Connection: keep—alive, Upgrade Cookie: session—KOsEJNufIw4Rd9BDNrVmvwBF9rEij eE2 Upgrade: websocket

If the server accepts the connection, it returns a WebSocket handshake response like the following:

---

Connection: Upgrade Upgrade: websocket Sec-WebSocket-Accept: OFFP+2nmN1f/h+4BP36k9uzrYGk=

At this point, the network connection remains open and can be used to send WebSocket messages in either direction.

Note Several features of the WebSocket handshake messages are worth noting:

- The Connection and Upgrade headers in the request and response indicate that this is a WebSocket handshake.

- The Sec-WebSocket-Version request header specifies the WebSocket protocol version that the client wishes to use

- The Sec-WebSocket-Key request header contains a Base64-encoded random value, which should be randomly gene

request.

- The Sec-WebSocket-Accept response header contains a hash of the value submitted in the sec-WebSocket-Key

with a specific string defined in the protocol specification. This is done to prevent misleading responses resulting from mi: proxies.

## What do WebSocket messages look like?

Once a WebSocket connection has been established, messages can be sent asynchronously in either direction by the client or se A simple message could be sent from the browser using client-side JavaScript like the following: ws.send ("Peter Wiener") ;

In principle, WebSocket messages can contain any content or data format. In modern applications, it is common for JSON to be u WebSocket messages. For example, a chat-bot application using WebSockets might send a message like the following: {"user" : "Hal Pline" , "content" : "I wanted to be a Playstation growing up, not a device to answe

---

Find WebSocket vulnerabilities using
