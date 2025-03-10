---
description: Apply these testing rules when verifying or writing new test suites to ensure quality and coverage
globs: **/*.test.ts
alwaysApply: false
---

# Testing Standards

<author>blefnk/rules</author>
<version>1.0.0</version>

## Context

- Ensures consistent testing conventions for unit and integration tests
- Uses `vitest` for all test files matching `**/*.test.ts`

## Requirements

- Use Vitest as the testing framework.
- Group related tests logically (describe blocks).
- Write clear, atomic assertions; avoid chaining multiple, unrelated checks in one test.
- Test only critical logic or components; avoid trivial or redundant tests.
- Give each test a descriptive name that clarifies its purpose and expected outcome.

## Examples

<example>
  import { describe, it, expect } from "vitest";

  describe("Button component", () => {
    it("renders with default props", () => {
      const result = renderButton(); // Pseudocode
      expect(result).toContain("<button>");
    });
  });
</example>

<example type="invalid">
  test("Test all app logic in one big function", () => {
    // Overly broad testing, unclear assertions
  });
</example>
