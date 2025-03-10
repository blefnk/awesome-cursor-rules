---
description: Use better-auth patterns when implementing authentication to ensure secure and consistent user identity management
globs: src/**/*.{ts,tsx}
alwaysApply: false
---

# Better-Auth Implementation Standards

<author>blefnk/rules</author>
<version>1.0.0</version>

## Context

- Implement authentication in Next.js applications using better-auth.
- Ensure secure and consistent user identity management.
- Apply these standards to auth configuration, client components, and database schema.

## Requirements

- Define all user schema customizations in `src/lib/auth.ts` and regenerate the schema with `bun db:auth`.
- Implement social providers with proper profile mapping functions to ensure consistent user data.
- Structure auth routes in `src/app/auth/` with dedicated client components.
- Enable two-factor authentication and manage backup codes securely.
- Use the auth client from `~/lib/auth-client` for all client-side authentication operations.
- Maintain proper account linking configuration for social providers.
- Handle authentication errors and redirect users appropriately.
- Follow secure token handling and session management practices.
- Log authentication events for auditing and troubleshooting.
- Separate client and server logic by applying `"use client"` and `"use server"` directives as needed.
- Implement robust profile mapping functions for each social provider.
- Configure secure cookies and token storage for managing sessions.
- Enforce HTTPS and validate all authentication inputs to prevent CSRF and injection attacks.
- Structure all auth routes and components consistently to streamline maintenance and debugging.
- Use secure methods for generating and storing backup codes for two-factor authentication.
- Always use the designated auth client; server-side: `src/lib/auth.ts`; client-side: `~/lib/auth-client`.

## Examples

<example>

```typescript
// src/lib/auth.ts
export const auth = betterAuth({
  baseURL: process.env.NEXT_SERVER_APP_URL,
  secret: process.env.AUTH_SECRET,
  
  database: drizzleAdapter(db, {
    provider: "pg",
    schema: {
      user: userTable,
      session: sessionTable,
      account: accountTable,
      verification: verificationTable,
      twoFactor: twoFactorTable,
    },
  }),
  
  user: {
    additionalFields: {
      firstName: {
        type: "string",
        required: false,
        input: true,
      },
      // Add additional fields as needed
    },
  },
  
  // Enable email/password authentication
  emailAndPassword: {
    enabled: true,
  },
  
  // Configure social providers with proper mapping
  socialProviders: {
    github: {
      clientId: process.env.AUTH_GITHUB_ID ?? "",
      clientSecret: process.env.AUTH_GITHUB_SECRET ?? "",
      scope: ["user:email", "read:user"],
      mapProfileToUser: (profile) => ({
        firstName: profile.name?.split[" "](0) ?? "",
        // Map additional fields as needed
      }),
    },
  },
  
  // Enable plugins like two-factor authentication
  plugins: [twoFactor()],
});
```

</example>

<example>

```typescript
// src/app/auth/sign-in/client.tsx
"use client";

import { signIn } from "~/lib/auth-client";

export function SignInPageClient() {
  // Initialize form state hooks

  const handleEmailLogin = async (e: React.FormEvent) => {
    e.preventDefault();
    setLoading(true);

    try {
      await signIn.email({
        email,
        password,
      });
      router.push("/dashboard");
    } catch (err) {
      setError("Invalid email or password");
    } finally {
      setLoading(false);
    }
  };
  
  const handleSocialLogin = (provider: string) => {
    setLoading(true);
    try {
      void signIn.social({ provider });
    } catch (err) {
      setError(`Failed to sign in with ${provider}`);
      setLoading(false);
    }
  };
  
  // Render the component JSX
}
```

</example>

<example type="invalid">

```typescript
// Incorrect: Directly modifying schema without regenerating
// Edit users.ts directly instead of src/lib/auth.ts
export const userTable = pgTable("user", {
  // Custom fields added directly here
  age: integer("age"),
  firstName: text("first_name"),
});
```

</example>

<example type="invalid">

```typescript
// Incorrect: Using inconsistent auth client or methods
import { signIn } from "next-auth/react"; // Wrong import
// or
fetch("/api/auth/signin", { // Direct API call instead of using the auth client
  method: "POST",
  body: JSON.stringify({ email, password }),
});
```

</example>
