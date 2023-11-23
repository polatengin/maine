# Enable Content Security Policy (CSP) header on a Next.js app

Content Security Policy (CSP) is an added layer of security that helps to detect and mitigate certain types of attacks, including Cross Site Scripting (XSS) and data injection attacks. These attacks are used for everything from data theft to site defacement or distribution of malware.

CSP is designed to be fully backward compatible, so you can deploy it without breaking your existing application.

## How to enable CSP on a Next.js app

Generate a `nonce` and add it to the CSP header. The `nonce` is a random string that is generated for each request. It is used to ensure that the content of the script tag is not modified by an attacker.
