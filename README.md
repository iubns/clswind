# clswind

A utility for merging Tailwind CSS classes conditionally.

## Installation

```bash
npm install clswind
# or
yarn add clswind
# or
pnpm add clswind
```

## Usage

```typescript
import { cn } from 'clswind';

const className = cn("px-2 py-1", "bg-red-500", {
  "text-white": true,
  "text-black": false,
});
// => 'px-2 py-1 bg-red-500 text-white'
```

It supports merging Tailwind classes correctly:

```typescript
cn("px-2 py-1", "p-4");
// => 'p-4' (overrides correctly thanks to tailwind-merge)
```

## License

MIT © [iubns](https://github.com/iubns)

