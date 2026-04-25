<!-- This is an EXAMPLE of what your CLAUDE.md will look like after vsync runs. -->
<!-- Yours will reflect your actual project state. -->

<!-- VerifiedState managed section — auto-updated by vsync. Do not edit. -->
<!-- For instructions managed by VerifiedState, edit at https://verifiedstate.ai/dashboard -->

# Project Memory (VerifiedState)

Auto-updated 2026-04-25T05:23:09Z. Regenerated every 5 minutes.

## Active project

**Repo:** my-saas-app
**Current goal:** Ship the Stripe checkout flow for the Pro tier
**Next action:** Wire success webhook to user upgrade endpoint
**Active branch:** feature/stripe-pro-checkout

## Recent decisions (last 7 days)

- Use Stripe Checkout (hosted) over Elements for v1 — faster to ship, less PCI surface
- Webhook signature verification via `stripe.webhooks.constructEvent` not custom HMAC
- User upgrade is idempotent (keyed on `subscription_id`) to handle webhook retries
- Pro tier price ID stored in env var, not database (rotation strategy: redeploy)

## Open loops

- Webhook idempotency table needs migration before deploy
- Need to test 3DS flow on European test card
- Pricing page copy still says "$19/mo" — update to match Stripe price object

## Active constraints

- Must support both monthly and annual billing
- No PII in webhook logs (regulatory)
- Failover: if Stripe webhooks fail, fall back to polling subscription status hourly

## Behavioral guidelines

### Think Before Coding
- State assumptions explicitly before starting
- Surface real tradeoffs instead of hiding them behind chosen-one answers
- Push back when the ask is unclear, contradictory, or likely wrong

### Simplicity First
- Write the minimum viable code that solves the task
- No speculative abstractions; no future-proofing for needs that don't exist yet
- Don't add error handling for errors that can't happen

### Surgical Changes
- Only touch what the task requires
- Do not rename, reformat, or "clean up" code you were not asked to change

### Goal-Driven Execution
- Define verifiable success criteria before starting
- Loop until the criteria are met; do not stop early and declare success
- If criteria cannot be met, report why — don't silently narrow the goal

## What changed since last session

- Webhook signature verification merged to main (commit `a8f3e21`)
- Switched from manual price IDs to env var pattern (Cursor session, 2h ago)

<!-- /VerifiedState managed section -->

<!-- Add your own project instructions below this line -->
