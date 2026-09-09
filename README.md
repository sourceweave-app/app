# SourceWeave — Health record provenance & source explorer

Trace healthcare sources, returned record categories and supplied sync details with SourceWeave. Understand provenance and the limits of an authorized snapshot.

**Site:** https://sourceweave.onrender.com/  
**Repository:** https://github.com/sourceweave-app/app

## Production integration

The previous synthetic demo integration has been removed. This client requests real patient-authorized records using [FinchNode production](https://finchnode.com/openapi.yaml) through the [shared connection service](https://github.com/visitquill/finchapps-connect). It never calls the public demo API or falls back to fixture data.

Production activation is pending operator legal/privacy details and a server-side live key plus webhook signing secret. Until that is complete, connecting fails closed with an explicit setup message. Deployment of this code alone is not evidence of a completed real EHR connection.


## Website and import flow

The public home route presents a complete, individually designed website with navigation, a product explanation, and a primary import button. Import and private records live at `/#/import`; opening the homepage never starts a record request. The import button opens category selection before any external connection. Back to home clears records from the rendered page. Returning from Hosted Connect opens the import route. Existing sessions can be resumed via the homepage button.

## Use

Choose record categories, click **Connect my EHR**, and complete FinchNode Hosted Connect and your own provider sign-in. Consent identifies **FinchApps Personal Health Tools**, the shared application behind these eleven sites. Return here to view the authorized record. Each visitor session is isolated to this site's origin and expires after 30 minutes; free service restarts can end it earlier. Reconnect if necessary.

Source provenance. Source names, dates, units, missing categories and partial sync warnings come from the production response. No patient identity, provider, measurement or connection is invented. FHIR Trail shows FinchNode's normalized records derived from FHIR, not an untouched FHIR bundle. ConsentLoom displays actual consent metadata. SourceWeave lists only the sources returned with the authorized record.

End this session removes local access. Revoke sharing or request deletion through [FinchNode data controls](https://finchnode.com/me). Sharing consent is for the common application, so revocation can affect all eleven tools. Exported or printed copies remain on the user's device.

## Local development

Node 22.13+:

```sh
npm ci
npm run dev
npm test
npm run build
```

The build outputs `dist/`. This is a React/Vite static frontend with responsive layouts, keyboard controls, visible focus styles and reduced-motion support. Development runs do not bypass production origin restrictions. To exercise authentication locally, run the backend's injected mock tests; do not relax its production allowlist or embed keys in the client.

## Render

Create a free Static Site from this repository, build with `npm ci && npm run build`, and publish `dist`. The supplied `render.yaml` documents the service and security headers. Public-repository deployments require a manual deploy after pushing a commit. The separate Node connection service runs on Render's free plan and may sleep.

CSP `connect-src` must allow only `https://finchapps-connect.onrender.com`. Deploy the backend and configure its secret environment before enabling live connections. **Never add API keys to Vite variables, source, browser storage, logs, README examples or Git.** No frontend environment secret is required.

## Privacy and verification

Read [the data-handling notice](https://sourceweave.onrender.com/privacy.html). No clinical record is saved in browser storage; visible data is held in memory, cleared on hiding the page, and periodically revalidated. No browser agent tools expose medical data. Unit tests validate production envelopes and preserve source values. Backend tests cover origin/session isolation, scope checks, invalid environments, expiration and signed revocation without using real medical data.

A real patient must perform their own EHR authentication and consent; these tests do not claim successful patient connectivity. Availability varies by healthcare organization.

## Domain candidate

`sourceweaveapp.com` was available on September 8, 2026; Porkbun displayed $11.08 for initial registration and renewal. No domain was purchased. Availability and price can change. Add it to Render and the backend's explicit origin allowlist before use.

<!-- public-discovery -->
## Public guide and project context

[Trace a record back to its source](https://sourceweave.onrender.com/guides/trace-a-record-back-to-its-source.html) — How SourceWeave separates organization identity, synchronization time and category coverage when presenting provenance.

[Search SourceWeave guides](https://sourceweave.onrender.com/guides/) · [About the site](https://sourceweave.onrender.com/about.html) · [Sitemap](https://sourceweave.onrender.com/sitemap.xml)

SourceWeave is a standalone product with its own interface, documentation and repository, prepared for independent business operation and continued development. Its FinchNode integration is documented in the code. Live production activation remains pending.

## Public-page build and discoverability

Edit `content/seo.json` for reviewed article text and site metadata. `npm run build` generates public HTML pages, a sitemap, social metadata and structured data, then prerenders the actual React homepage. `npm run test:seo` checks the built crawl surface after a build. Public guide search filters only public text in the browser; no patient data or search analytics enter the index.

Keep canonical URLs on the deployed origin until a custom domain is registered and configured. Add only public, canonical pages to the sitemap. Validate links, mobile layout and the built HTML after editorial changes. Search engine indexing and rich results are not guaranteed.

## Independent business handoff

[Business handoff](HANDOFF.md) covers product identity, the receiving business’s production setup, domain migration, search verification and ongoing editorial maintenance.
