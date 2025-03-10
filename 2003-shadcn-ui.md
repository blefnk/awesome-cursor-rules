---
description: Use Shadcn UI components when designing to ensure consistent styling
globs: src/**/*.tsx
alwaysApply: false
---

# Shadcn UI Rules

<author>blefnk/rules</author>
<version>1.0.0</version>

## Context

- For integrating Shadcn UI primitives
- Maintains consistency and design standards

## Requirements

- Import Shadcn primitives from `~/ui/primitives`.
- Keep app-specific components in `~/ui/components`.
- Match Shadcn design and naming conventions.
- Style <Link> using `cn()` and `buttonVariants` when you need a button-like style.
- Use <Button> only when you need to call a function.

## Available Shadcn UI Component Primitives

Accordion, Alert, Alert Dialog, Aspect Ratio, Avatar, Badge, Breadcrumb, Button, Calendar, Card, Carousel, Chart, Checkbox, Collapsible, Combobox, Command, Context Menu, Data Table, Date Picker, Dialog, Drawer, Dropdown Menu, Form, Hover Card, Input, Input OTP, Label, Menubar, Navigation Menu, Pagination, Popover, Progress, Radio Group, Resizable, Scroll Area, Select, Separator, Sheet, Sidebar, Skeleton, Slider, Sonner, Switch, Table, Tabs, Textarea, Toast, Toggle, Toggle Group, Tooltip

## Examples

<example>
  import { Button } from "~/ui/primitives/button";
  
  export function ConfirmButton() {
    return <Button>Confirm</Button>;
  }
</example>

<example type="invalid">
  import { Button } from "shadcn-ui";
  
  export function ConfirmButton() {
    return <Button>Confirm</Button>;
  }
</example>

<example>

  ```tsx
  import { Link } from "next/link";
  import { cn } from "~/lib/utils";
  import { buttonVariants } from "~/ui/primitives/button";
  
  export function HomeLink() {
    return (
      <Link
        href="/"
        className={cn(
          buttonVariants({
            variant: "default",
            className: "mx-auto mt-4 w-fit",
          }),
        )}
      >
        Home
      </Link>
    );
  }
  ```

</example>

<example type="invalid">
  
  ```tsx
  import { Link } from "next/link";
  import { Button } from "~/ui/primitives/button";
  
  export function HomeLink() {
    return (
      <Button
        className="mx-auto mt-4 w-fit"
      >
        <Link href="/">Home</Link>
      </Button>
    );
  }
  ```

</example>
