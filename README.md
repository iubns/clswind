# clswind

A tiny, fast utility for merging Tailwind CSS classes without conflicts.

[![npm version](https://img.shields.io/npm/v/clswind.svg)](https://www.npmjs.com/package/clswind)
[![License](https://img.shields.io/npm/l/clswind.svg)](https://github.com/iubns/clswind/blob/main/LICENSE)

## Why clswind?

Every Tailwind CSS project seems to start with the same ritual: creating a `lib/utils.ts` file and copy-pasting the `cn` helper function.

**Stop copy-pasting.**

`clswind` provides this standard utility as a package, so you can:

- 🚀 **Save time**: No more boilerplate files in every project.
- 📦 **Keep it clean**: One less file to maintain.
- 🛠 **Stay updated**: Get improvements and fixes automatically.

## Introduction

`clswind` combines the power of [clsx](https://github.com/lukeed/clsx) and [tailwind-merge](https://github.com/dcastil/tailwind-merge) into a single, easy-to-use function.

- **Conditional**: Apply classes based on boolean logic (like `clsx`).
- **Conflict-free**: Automatically resolves Tailwind CSS class conflicts (like `tailwind-merge`).

## Installation

```bash
npm install clswind
# or
yarn add clswind
# or
pnpm add clswind
```

## Usage

### Basic Usage

It works just like `clsx`, but handles Tailwind conflicts intelligently.

```typescript
import { cn } from "clswind";

// Basic conditional classes
cn("px-2 py-1", "bg-red-500", {
  "text-white": true,
  "text-black": false,
});
// => 'px-2 py-1 bg-red-500 text-white'

// Merging conflicting Tailwind classes
// 'p-4' overrides 'px-2 py-1' because padding utilities conflict
cn("px-2 py-1", "p-4");
// => 'p-4'
```

### With React Components

This is especially useful when building reusable UI components that accept `className` props.

```tsx
import { cn } from "clswind";

interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: "primary" | "secondary";
  className?: string; // Explicitly define className prop if needed, though included in ButtonHTMLAttributes
}

export function Button({
  className,
  variant = "primary",
  ...props
}: ButtonProps) {
  return (
    <button
      className={cn(
        // Base styles
        "px-4 py-2 rounded font-medium transition-colors",
        // Variant styles
        variant === "primary" && "bg-blue-500 text-white hover:bg-blue-600",
        variant === "secondary" &&
          "bg-gray-200 text-gray-800 hover:bg-gray-300",
        // External classes (allows overriding base/variant styles)
        className,
      )}
      {...props}
    />
  );
}
```

### With class-variance-authority (CVA)

`clswind` is the perfect companion for [CVA](https://cva.style/docs). CVA handles your component variants, while `clswind` handles the class merging and conflict resolution.

```tsx
import { cva, type VariantProps } from "class-variance-authority";
import { cn } from "clswind";

const badgeVariants = cva(
  "inline-flex items-center rounded-full border px-2.5 py-0.5 text-xs font-semibold transition-colors focus:outline-none focus:ring-2 focus:ring-ring focus:ring-offset-2",
  {
    variants: {
      variant: {
        default:
          "border-transparent bg-primary text-primary-foreground hover:bg-primary/80",
        secondary:
          "border-transparent bg-secondary text-secondary-foreground hover:bg-secondary/80",
        destructive:
          "border-transparent bg-destructive text-destructive-foreground hover:bg-destructive/80",
        outline: "text-foreground",
      },
    },
    defaultVariants: {
      variant: "default",
    },
  },
);

export interface BadgeProps
  extends
    React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof badgeVariants> {}

function Badge({ className, variant, ...props }: BadgeProps) {
  return (
    <div className={cn(badgeVariants({ variant }), className)} {...props} />
  );
}
```

## License

MIT © [iubns](https://github.com/iubns)
