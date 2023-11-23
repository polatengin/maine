# Enable Content Security Policy (CSP) header on a Next.js app

Content Security Policy (CSP) is an added layer of security that helps to detect and mitigate certain types of attacks, including Cross Site Scripting (XSS) and data injection attacks. These attacks are used for everything from data theft to site defacement or distribution of malware.

CSP is designed to be fully backward compatible, so you can deploy it without breaking your existing application.

## How to enable CSP on a Next.js app

Generate a `nonce` and add it to the CSP header. The `nonce` is a random string that is generated for each request. It is used to ensure that the content of the script tag is not modified by an attacker.

```typescript
// ./src/middleware.ts
const nonce = Buffer.from(crypto.randomUUID()).toString('base64');
const cspHeader = `
  default-src 'self';
  script-src 'self' 'nonce-${nonce}' 'unsafe-eval' https://www.googletagmanager.com/;
  style-src 'self' 'unsafe-inline';
  img-src 'self' blob: data:;
  font-src 'self';
  object-src 'none';
  base-uri 'self';
  form-action 'self';
  frame-ancestors 'none';
  block-all-mixed-content;
  upgrade-insecure-requests;
`;

// Replace newline characters and spaces
const contentSecurityPolicyHeaderValue = cspHeader.replace(/\s{2,}/g, ' ').trim();

const headers = new Headers(request.headers);
headers.set('x-nonce', nonce);
headers.set('Content-Security-Policy', contentSecurityPolicyHeaderValue);

const response = NextResponse.next({ request: { headers } });
```

`nonce` value can be read in pages by reading the `x-nonce` header.

```typescript
// ./pages/page.tsx
const nonce = headers().get('x-nonce');
```
```
