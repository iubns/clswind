# clswind

A tiny, fast utility for merging Tailwind CSS classes without conflicts.

[![npm version](https://img.shields.io/npm/v/clswind.svg)](https://www.npmjs.com/package/clswind)
[![License](https://img.shields.io/npm/l/clswind.svg)](https://github.com/iubns/clswind/blob/main/LICENSE)

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

## License

MIT © [iubns](https://github.com/iubns)
