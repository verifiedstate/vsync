# Before vs. after vsync

## Before (without vsync)

You: I want to add Stripe to my app

Claude Code: Sure! What's your tech stack? Are you using Next.js? Express? What's your database? Do you want subscriptions or one-time payments? Do you have an existing user model? What payment methods do you need to support? Will you handle webhooks?

You: [explains everything for the third time this week]

Claude Code: Great, here's a Stripe integration. I'll add a `users` table...

You: We already have a users table. From last session.

Claude Code: My apologies. Could you share the schema?

[10 minutes later, you switch to Cursor]

Cursor: I'll set up Stripe Checkout for you. What approach do you want?

You: I literally just decided this with Claude Code 5 minutes ago.

Cursor: I don't have access to that context.

---

## After (with vsync)

You: I want to add Stripe to my app

Claude Code: Looking at the project memory — you decided to use Stripe Checkout (hosted) over Elements for v1, with `stripe.webhooks.constructEvent` for signature verification. I see you're on branch `feature/stripe-pro-checkout` and the next action is wiring the success webhook to the user upgrade endpoint. The webhook idempotency table migration is still open. Should I start with that migration?

You: Yes, that.

[Claude Code does the work]

[You switch to Cursor]

You: Continue from where Claude Code left off

Cursor: Looking at the project memory — Claude Code just merged the webhook signature verification (commit a8f3e21). The idempotency migration was applied. Next up was testing 3DS flow on European test cards. Want me to set that up?

---

The difference: vsync captured the architecture decision, the next action, and the open loops in shared memory. Both tools see them. No re-explaining. No context loss.
