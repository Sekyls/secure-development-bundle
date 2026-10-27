# Email addresses

> Converted source material; attribution: pilcrow, Pilcrow's auth book. Snapshot processed 2026-10-27.
> Source SHA-256: `d61de4da4b8dfaac31c9f7aabf7fd9fcfebddcf72b89b038491b0f160472a93c`.
> Conversion: HTML converted to Markdown; text and code preserved. Source opinions and historical examples are preserved, not automatically endorsed.


Email addresses work great as an account identifier as they're something users remember and globally unique. By verifying ownership of the domain, you also can avoid squatting and typos. However, keep in mind that this effectively makes the email address a public-facing identifier. Avoid it if you (or your users) prefer not to expose which individuals are using your website.

The structure of an email address is simple. There is the username part, the "@" sign, and the domain. But specification and rules defining the exact pattern of each part are complex and building strict validation logic to enforce them isn't worth the effort. For example, the local part can contain spaces and "@" characters if they're enclosed in double quotes. The domain can include non-Latin characters (IDNs) or even be a literal IP address. The local part can be case-sensitive depending on the hosting mail server.

If you just need the email address to send emails, a simple check for the "@" character is sufficient.

However, if the email address is an account identifier, we should be more careful. It's going to be part of your system and will be displayed to users as part of the website's UI. That means like usernames, we need to be strict with them. As such, I recommend putting the following restrictions:

	
- A maximum length of 100 characters.

	
- The email address must contain exactly one "@" character to separate the username and domain.

	
- Both the username and the domain must be at least 1 character long.

	
- The username can only include lowercase letters, numbers, periods (.), plus signs (+), underscores (_), and hyphens (-).

	
- The domain must include at least one period (.).

	
- The domain can only include lowercase letters, numbers, hyphens (-), and periods (.).

I strongly discourage silently modifying the user's input, such as automatically lowercasing email addresses or stripping the plus sign to block aliases. As I mentioned, the local part of an email address can be case-sensitive. Furthermore, the plus sign (+) is not universal and only has a special meaning in certain mail servers (e.g., Gmail).
