---
description: Integrate Polar payments for checkout, subscriptions, and webhooks in ecommerce flows
globs: src/**/*.{ts,tsx}
alwaysApply: false
---

# Polar Payments Integration Rules

<author>blefnk/rules</author>
<version>1.0.0</version>

## Context

- Use Polar for checkouts, subscriptions, and webhooks in ecommerce flows.
- Works in combination with Next.js and BetterAuth for secure payments.

## Requirements

### 1. Setup & Installation

- Test with Polar Sandbox before switching to production.
- Set environment variables: `POLAR_ACCESS_TOKEN=your_token POLAR_WEBHOOK_SECRET=your_secret`
- Install dependencies: `bun add @polar-sh/sdk @polar-sh/nextjs`
- Configure the Polar API client: `import { Polar } from '@polar-sh/sdk'; export const api = new Polar({ accessToken: process.env.POLAR_ACCESS_TOKEN!, server: 'sandbox', // Change to 'production' when ready });`
- Retrieve active products: `const products = await api.products.list({ isArchived: false });`
- Create a checkout endpoint: `import { Checkout } from '@polar-sh/nextjs'; export const GET = Checkout({ accessToken: process.env.POLAR_ACCESS_TOKEN!, successUrl: '/confirmation', server: 'sandbox', });`
- Pass query parameters (e.g., `productId`, `productPriceId`) for dynamic product selection.
- Redirect users to a confirmation page (`/confirmation`) after checkout.
- Handle checkout, subscription, and payment events via webhooks: `import { Webhooks } from '@polar-sh/nextjs'; export const POST = Webhooks({ webhookSecret: process.env.POLAR_WEBHOOK_SECRET!, onPayload: async (payload) => { // Handle events: checkout.created, subscription.active, etc. }, });`
- Use ngrok for local webhook testing.
- Enable customer portal if user requests self-service: `import { CustomerPortal } from '@polar-sh/nextjs'; export const GET = CustomerPortal({ accessToken: process.env.POLAR_ACCESS_TOKEN!, getCustomerId: (req) => "", // Implement customer ID resolution server: 'sandbox', });`
- Use BetterAuth integration: `bun add better-auth @polar-sh/better-auth`
- Automate customer creation, checkout, and webhook handling.
- Query Parameters: Pass product/customer data (e.g., `productId`, `customerEmail`) via URL.
- Product Display: Create UI components for product listings; handle multiple pricing plans.
- Testing & Deployment: Use Sandbox for testing; update tokens for production.
- Webhook Handlers: Update database or user entitlements based on events.

## Examples

<example>

  ✅ Minimal Next.js Checkout Setup

  ```ts
  import { Checkout } from '@polar-sh/nextjs';
  export const GET = Checkout({
    accessToken: process.env.POLAR_ACCESS_TOKEN!,
    successUrl: '/confirmation',
    server: 'sandbox',
  });
  ```
  
</example>

<example>

  ❌ Invalid: Missing `POLAR_ACCESS_TOKEN`

  ```ts
  export const GET = Checkout({});
  // Checkout will fail due to missing access token.
  ```

</example>
