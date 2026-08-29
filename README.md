# CrewApply — public website

The public site for **crewapply.com**: what the product is, what it costs, and the
policy pages that payment providers and app stores require.

Plain static HTML with one stylesheet. No framework, no build step, no server.
That is deliberate — this site has to stay up when the API does not. It was
built when the API VPS went down and took `api.crewapply.com` with it; hosting
it separately means the legal pages and payment-gateway verification never
depend on that server again.

## Pages

| Path | Purpose |
|------|---------|
| `/` | What CrewApply does, subscription plans |
| `/terms` | Terms and Conditions |
| `/privacy` | Privacy Policy |
| `/refunds` | Cancellation and Refunds |
| `/shipping` | Shipping and Delivery |
| `/contact` | Business details and support |

The last five are the set required for Razorpay merchant verification.

## Terms and Privacy are generated, not written here

Those two pages are derived from the copy the mobile app already ships in
`src/screens/Settings/TermsConditionsScreen.jsx` and `PrivacyPolicyScreen.jsx`.

Keeping one source matters: a reviewer comparing the app's policy against the
website's is exactly who notices when they have drifted apart. If a policy
changes, change it in the app and regenerate — do not hand-edit `terms.html` or
`privacy.html`.

## Before publishing — placeholders

Anything highlighted yellow on the rendered page is a placeholder and must be
replaced with real details:

- registered business name, address, phone (`/contact`)
- refund window and support hours (`/refunds`, `/contact`)
- "last updated" dates

A missing or unverifiable business address is one of the most common reasons a
payment-gateway application is rejected.

## Deploying

Hosted on Vercel. Pushing to `main` deploys.

`vercel.json` sets `cleanUrls` (so `/terms` and `/terms.html` both resolve),
security headers, long-lived caching for `/assets`, and redirects from the URL
spellings reviewers commonly guess — `/refund-policy`, `/privacy-policy`,
`/terms-of-service` and similar — onto the right page.

## DNS

`crewapply.com` and `www` point at Vercel. `api.crewapply.com` is a separate
record pointing at the API server and is unaffected by anything in this repo.
