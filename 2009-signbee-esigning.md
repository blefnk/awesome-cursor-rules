---
description: Integrate Signbee e-signing API, MCP server, and Agent Skill when building document signing flows
globs: src/**/*.{ts,tsx}
alwaysApply: false
---

# Signbee Document E-Signing Integration Rules

<author>signbee</author>
<version>1.0.0</version>

## Context

- Use [Signbee](https://signb.ee) for two-party document e-signing (NDAs, contracts, agreements, onboarding packets) with two-pass SHA-256 tamper-evident certificates.
- Works alongside Next.js 15 App Router, TypeScript 5, and BetterAuth, or directly inside AI coding IDEs via [signbee/cursor-plugin](https://github.com/signbee/cursor-plugin), [signbee/mcp](https://github.com/signbee/mcp), and [signbee/skill](https://github.com/signbee/skill).
- Reference LLM documentation at `https://signb.ee/llms.txt` and `https://signb.ee/llms-full.txt`.

## Requirements

### 1. Setup & IDE Tooling

- Install the Agent Skill globally for Cursor/Claude Code: `npx skills add signbee/skill --skill signbee -g`
- Register the MCP server in `.cursor/mcp.json`: `npx -y signbee-mcp`
- Set environment variable for server-side routes: `SIGNBEE_API_KEY=sb_live_your_key`

### 2. Sending & Verifying Documents

- Call `POST https://signb.ee/api/v1/send` with `markdown` (or `pdf_url`), `sender_name`, `sender_email`, `recipient_name`, and `recipient_email`.
- Always check `if (!res.ok) throw new Error(...)` and validate response payloads before updating database state.
- When running without `SIGNBEE_API_KEY`, handle `pending_sender_verification` by confirming the 6-digit email OTP via `POST https://signb.ee/api/v1/verify`.
- Poll `GET https://signb.ee/api/v1/status/{document_id}` or subscribe via `webhook_url` to track signing completion.
- Never mutate or re-compress the completed PDF bytes, preserving the embedded SHA-256 signing certificate hash.

## Examples

<example>

  ✅ Valid: Typed Next.js Server Action Sending a Contract via Signbee

  ```ts
  export async function sendContractAction(input: {
    title: string;
    markdown: string;
    senderName: string;
    senderEmail: string;
    recipientName: string;
    recipientEmail: string;
  }) {
    const res = await fetch("https://signb.ee/api/v1/send", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        Authorization: `Bearer ${process.env.SIGNBEE_API_KEY!}`,
      },
      body: JSON.stringify({
        title: input.title,
        markdown: input.markdown,
        sender_name: input.senderName,
        sender_email: input.senderEmail,
        recipient_name: input.recipientName,
        recipient_email: input.recipientEmail,
      }),
    });

    if (!res.ok) {
      throw new Error(`Signbee send failed: ${res.status}`);
    }

    return res.json();
  }
  ```

</example>

<example type="invalid">

  ❌ Invalid: Exposing `SIGNBEE_API_KEY` in a client component or ignoring HTTP status errors

  ```ts
  // Do not call with secret API keys from browser code or ignore !res.ok
  const res = await fetch("https://signb.ee/api/v1/send", {
    method: "POST",
    headers: { Authorization: "Bearer sb_live_hardcoded_secret" },
    body: JSON.stringify({ markdown: "# NDA" }),
  });
  ```

</example>
