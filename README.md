# EasyPayment

EasyPayment is a responsive web-and-mobile-friendly **payment platform prototype** built as a static HTML/CSS/JavaScript app. It is designed to preview a product experience across dashboard, transfers, transaction history, cards, savings goals, and an illustrative admin console.

## Run locally
Open `index.html` in a modern browser. No build step or package install is required.

## Demo behavior
- Dashboard charts and sample balances are illustrative.
- Transfers are simulated in browser memory and update the sample transaction list/balance only.
- Card freeze/unfreeze is a visual demo control.
- Savings goals can be created and sample contributions can be added.
- The admin console contains fictional operational data and is not access-controlled.
- No real payment, bank, card, authentication, or identity-verification provider is connected.

## Deploy a preview
This repository can be published as a static site using GitHub Pages: repository **Settings → Pages → Deploy from a branch**, choose `main` and `/(root)`, then save. GitHub Pages can host the interface only; it does not provide a secure payments backend.

## Production requirements before handling real money
1. **Backend and ledger:** implement server-side APIs and an auditable double-entry ledger with database transactions, idempotency keys, reconciliation, and immutable transaction records. Never trust balances or fees calculated by the client.
2. **Payment rails:** integrate a regulated payment processor/bank partner (for example, a provider appropriate to the countries served). Verify provider webhooks using signatures, protect against replay, and reconcile settlement. Do not collect or store raw card data; use provider-hosted/tokenized card flows.
3. **Authentication and authorization:** add a real identity provider or secure server-side auth, MFA/passkeys, session rotation, secure HttpOnly/Secure/SameSite cookies, CSRF protection where applicable, rate limits, device/session management, and server-enforced role-based access control for admin routes.
4. **Security engineering:** TLS everywhere, secret manager (never commit secrets), input validation, output encoding, CSP and other security headers, dependency scanning, penetration testing, monitoring, alerting, backups, incident response, and least-privilege service credentials.
5. **Compliance and risk:** complete applicable licensing/legal review and KYC/AML/sanctions controls, transaction limits, fraud monitoring, privacy/data-retention policies, consumer disclosures, and dispute/refund processes. For Nigeria, obtain specialist advice on CBN requirements and applicable NDPA obligations before launch.
6. **Testing:** add automated unit/integration/end-to-end tests, sandbox payment tests, webhook failure/retry tests, ledger invariants, load testing, and disaster-recovery exercises.

## Important
This repository is a **UI prototype, not a production fintech service**. It does not provide real authentication, real account balances, real card issuance, real transfers, or secure admin permissions. Do not enter real banking credentials or personal financial data.


## User registration and login setup

The registration/sign-in screen now uses Supabase Auth, and the dashboard redirects visitors who are not signed in back to `auth.html`. The signed-in user's name/email appear in their dashboard and a Sign out button is available. **This is authentication plus a personalized dashboard shell; the balance, card, and sample transaction figures remain demo data. No real payments are processed.**

1. Create a project at https://supabase.com/.
2. In the Supabase project, open Project Settings / API and copy the project URL and publishable key (or legacy anon key).
3. Edit `supabase-config.js` and replace the two placeholder strings. Only use a publishable/anon key in frontend code; never expose a service-role/secret key.
4. In Supabase Authentication URL Configuration, set the Site URL to your published website and add both the website URL and `/auth.html` to allowed redirect URLs.
5. Run `supabase-schema.sql` in the Supabase SQL Editor to create user-owned profile, savings-goal, and transaction tables with row-level security.
6. Publish the static files to a supported static host and test signup, email confirmation, sign-in, refresh, sign-out, and password reset.

Supabase references: https://supabase.com/docs/reference/javascript/auth-signup and https://supabase.com/docs/reference/javascript/auth-signin.

**Important hosting note:** GitHub Pages is static hosting and its policy says it is not intended for commercial transaction or SaaS sites. Before launching EasyPayment publicly as a financial service, use appropriate hosting and build a trusted server/API, verified payment-provider integration, server-created ledger, fraud monitoring, privacy/compliance controls, and security testing. The current UI is not a bank or payment processor.
