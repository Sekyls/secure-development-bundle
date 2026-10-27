# Access control vulnerabilities and privilege escalation

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `22d1dcada962326fa021d03d70ddae40cac1b542421df1d0e0f857e411f710bf`.
> Conversion: Local English OCR, all 7 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


Access control

Access control

Privilege escalation. The types of vulnerabilities that can arise with access control.

- How to prevent access control vulnerabilities.

If you're familiar with the basic concepts behind access control vulnerabilities and want to practice exploiting them on some re vulnerable targets, you can access labs in this topic from the link below.

- View all access control labs

## What is access control?

Access control is the application of constraints on who or what is authorized to perform actions or access resources. In the contex access control is dependent on authentication and session management:

- Authentication confirms that the user is who they say they are.

- Session management identifies which subsequent HTTP requests are being made by that same user.

- Access control determines whether the user is allowed to carry out the action that they are attempting to perform.

Broken access controls are common and often present a critical security vulnerability. Design and management of access controk dynamic problem that applies business, organizational, and legal constraints to a technical implementation. Access control design made by humans so the potential for errors is high.

---

Admin Delete users: Balance:

amount: Transfer Delete

https : / / insecure—webs ite . com/ https : / / insecure— website . com/admin accounts / 12345 / accountmenu . j sp

- Access control security models

Vertical access controls Vertical access controls are mechanisms that restrict access to sensitive functionality to specific types of users. With vertical access controls, different types of users have access to different application functions. For example, an administrato modify or delete any user's account, while an ordinary user has no access to these actions. Vertical access controls can be more implementations of security models designed to enforce business policies such as separation of duties and least privilege.

Horizontal access controls Horizontal access controls are mechanisms that restrict access to resources to specific users. With horizontal access controls, different users have access to a subset of resources of the same type. For example, a banking al user to view transactions and make payments from their own accounts, but not the accounts of any other user.

Context-dependent access controls Context-dependent access controls restrict access to functionality and resources based upon the state of the application or the us Context-dependent access controls prevent a user performing actions in the wrong order. For example, a retail website might pre\ modifying the contents of their shopping cart after they have made payment.

## Examples of broken access controls

Broken access control vulnerabilities exist when a user can access resources or perform actions that they are not supposed to be

Vertical privilege escalation If a user can gain access to functionality that they are not permitted to access then this is vertical privilege escalation. For exampl administrative user can gain access to an admin page where they can delete user accounts, then this is vertical privilege escalati(

Unprotected functionality At its most basic, vertical privilege escalation arises where an application does not enforce any protection for sensitive functionalit administrative functions might be linked from an administrator's welcome page but not from a user's welcome page. However, a u access the administrative functions by browsing to the relevant admin URL. For example, a website might host sensitive functionality at the following URL:

---

This might be accessible by any user, not only administrative users who have a link to the functionality in their user interface. In sc administrative URL might be disclosed in other locations, such as the robots . txt file: https : / / insecure—website . com/robots . txt

Even if the URL isn't disclosed anywhere, an attacker may be able to use a wordlist to brute-force the location of the sensitive fun

Unprotected admin functionality

In some cases, sensitive functionality is concealed by giving it a less predictable URL. This is an example of so-called "security b) hiding sensitive functionality does not provide effective access control because users might discover the obfuscated URL in a nun Imagine an application that hosts administrative functions at the following URL: https : / / insecure—website .

This might not be directly guessable by an attacker. However, the application might still leak the URL to users. The URL might be that constructs the user interface based on the user's role. &lt; script &gt;

```text
var isAdmin —
```

false; if (isAdmin)

```text
var adminPaneITag
```

document . createEIement ( ; adminPaneITag . setAttribute ( ' href ' 'https://insecure website.com/administrato adminPaneITag. innerText = ' Admin panel' •

&lt; / script &gt;

This script adds a link to the user's UI if they are an admin user. However, the script containing the URL is visible to all users rega

Unprotected admin functionality with unpredictable URL

Parameter-based access control methods Some applications determine the user's access rights or role at login, and then store this information in a user-controllable locatiot

- A hidden field.

- A cookie.

A preset query string parameter.

The application makes access control decisions based on the submitted value. For example: https : / / insecure—website . com/login/home . j https : / / insecure—website . com/login/home . j

This approach is insecure because a user can modify the value and access functionality they're not authorized to, such as admini

User role controlled by request parameter

User role can be modified in user profile

---

Some applications enforce access controls at the platform layer. they do this by restricting access to specific URLs and HTTP me user's role. For example, an application might configure a rule as follows: DENY: POST, / admin/de1eteUser, managers

This rule denies access to the POST method on the URL / admin/de1eteUser , for users in the managers group. Various thing situation, leading to access control bypasses. Some application frameworks support various non-standard HTTP headers that can be used to override the URL in the original re Original-URL and X-Rewrite-URL . If a website uses rigorous front-end controls to restrict access based on the URL, but th URL to be overridden via a request header, then it might be possible to bypass the access controls using a request like the followi

```text
POST / HTTP/ 1.1
```

X-OriginaI-URL: / admin/deIeteUser

URL-based access control can be circumvented

An alternative attack relates to the HTTP method used in the request. The front-end controls described in the previous sections re the URL and HTTP method. Some websites tolerate different HTTP request methods when performing an action. If an attacker ca another) method to perform actions on a restricted URL, they can bypass the access control that is implemented at the platform

Method-based access control can be circumvented

Broken access control resulting from URL-matching discrepancies Websites can vary in how strictly they match the path of an incoming request to a defined endpoint. For example, they may tolerai capitalization, so a request to /ADMTN/DELETEUSER may still be mapped to the / admin/de1eteUser endpoint. If the access less tolerant, it may treat these as two different endpoints and fail to enforce the correct restrictions as a result. Similar discrepancies can arise if developers using the Spring framework have enabled the useSuffixPatternMatch option. an arbitrary file extension to be mapped to an equivalent endpoint with no file extension. In other words, a request to / admin/de1eteUser . anything would still match the / admin/de1eteUser pattern. Prior to Spring 5.3, this option is enab On other systems, you may encounter discrepancies in whether / admin/de1eteUser and / admin/deleteUser/ are treate In this case, you may be able to bypass access controls by appending a trailing slash to the path.

Horizontal privilege escalation Horizontal privilege escalation occurs if a user is able to gain access to resources belonging to another user, instead of their own For example, if an employee can access the records of other employees as well as their own, then this is horizontal privilege escæ Horizontal privilege escalation attacks may use similar types of exploit methods to vertical privilege escalation. For example, a ust own account page using the following URL: https : / / insecure—website . com/myaccount?id—123

If an attacker modifies the id parameter value to that of another user, they might gain access to another user's account page, ar and functions.

Note This is an example of an insecure direct object reference (IDOR) vulnerability. This type of vulnerability arises where user-con values are used to access resources or functions directly.

---

User ID controlled by request parameter

In some applications, the exploitable parameter does not have a predictable value. For example, instead of an incrementing numt use globally unique identifiers (GUIDs) to identify users. This may prevent an attacker from guessing or predicting another user's i GUIDs belonging to other users might be disclosed elsewhere in the application where users are referenced, such as user messa

User ID controlled by request parameter, with unpredictable user IDs

In some cases, an application does detect when the user is not permitted to access the resource, and returns a redirect to the log response containing the redirect might still include some sensitive data belonging to the targeted user, so the attack is still succes

User ID controlled by request parameter with data leakage in redirect

Horizontal to vertical privilege escalation Often, a horizontal privilege escalation attack can be turned into a vertical privilege escalation, by compromising a more privilegec horizontal escalation might allow an attacker to reset or capture the password belonging to another user. If the attacker targets an and compromises their account, then they can gain administrative access and so perform vertical privilege escalation. An attacker might be able to gain access to another user's account page using the parameter tampering technique already descril privilege escalation: https : / / insecure—website . com/myaccount?id=456

If the target user is an application administrator, then the attacker will gain access to an administrative account page. This page m administrator's password or provide a means of changing it, or might provide direct access to privileged functionality.

User ID controlled by request parameter with password disclosure

Insecure direct object references Insecure direct object references (IDORs) are a subcategory of access control vulnerabilities. IDORs occur if an application uses access objects directly and an attacker can modify the input to obtain unauthorized access. It was popularized by its appearance Top Ten. It's just one example of many implementation mistakes that can provide a means to bypass access controls.

Insecure direct object references -5

Insecure direct object references (IDOR)

Access control vulnerabilities in multi-step processes Many websites implement important functions over a series of steps. This is common when:

A variety of inputs or options need to be captured. The user needs to review and confirm details before the action is performed.

For example, the administrative function to update user details might involve the following steps:

---

2. Submit the changes. 3. Review the changes and confirm.

Sometimes, a website will implement rigorous access controls over some of these steps, but ignore others. Imagine a website wh correctly applied to the first and second steps, but not to the third step. The website assumes that a user will only reach step 3 if tl completed the first steps, which are properly controlled. An attacker can gain unauthorized access to the function by skipping the directly submitting the request for the third step with the required parameters.

Multi.step process with no access control on one step

Referer-based access control Some websites base access controls on the Referer header submitted in the HTTP request. The Referer header can be ad browsers to indicate which page initiated a request. For example, an application robustly enforces access control over the main administrative page at / admin , but for sub-pages st / admin/de1eteUser only inspects the Referer header. If the Referer header contains the main / admin URL, then the In this case, the Referer header can be fully controlled by an attacker. This means that they can forge direct requests to sensiti supplying the required Referer header, and gain unauthorized access.

Referer-based access control

Location-based access control Some websites enforce access controls based on the user's geographical location. This can apply, for example, to banking applic services where state legislation or business restrictions apply. These access controls can often be circumvented by the use of wel manipulation of client-side geolocation mechanisms.

## How to prevent access control vulnerabilities

Access control vulnerabilities can be prevented by taking a defense-in-depth approach and applying the following principles:

- Never rely on obfuscation alone for access control.

- Unless a resource is intended to be publicly accessible, deny access by default.

- Wherever possible, use a single application-wide mechanism for enforcing access controls.

- At the code level, make it mandatory for developers to declare the access that is allowed for each resource, and deny acces:

- Thoroughly audit and test access controls to ensure they work as designed.

---

Practise exploiting vulnerabilities on realistic targets.

Record your progression from Apprentice to Expert.

See where you rank in our Hall of Fame.

Enter your email

Already got an account? Login here
