---
id: typescript
title: TypeScript
sidebar_label: TypeScript
---

NodeCG is natively written in TypeScript and has type definitions for all APIs.

## Setup {#setup}

Install TypeScript as your bundle's dev dependency.

```bash
npm install -D typescript
```

## Typing Replicants {#typing-replicants}

Optionally, you can define types of replicants using replicants' JSON schema.

1. Define schema for replicants
1. Use CLI's `nodecg schema-types` command to convert JSON schema to TypeScript type definitions
1. Import the type and pass it to type parameter like this:

```ts
import { ExampleReplicant } from '../types/schemas/example_replicant';
const rep = nodecg.Replicant<ExampleReplicant>('example_replicant');
```

## Using Type Definitions {#type-definitions}

If you have not installed `nodecg` in your project:

```bash
npm install -D nodecg
```

The majority of the types are used by importing the types package and referencing the types that way, but to access the browser globals (`window.nodecg` and `window.NodeCG`), an extra step is required. You'll need to modify your bundle's `tsconfig.json` files(s) in one of two ways achieve this:

The first approach is to use [`include`](https://www.typescriptlang.org/tsconfig#include) to reference the file that augments the window object:

```json
{
    "include": ["src/**/*.ts", "src/**/*.tsx", "node_modules/nodecg/types/augment-window.d.ts"]
}
```

If you use Vue, be sure to include your `*.vue` files as well:

```json
{
    "include": ["src/**/*.ts", "**/*.vue", "node_modules/nodecg/types/augment-window.d.ts"]
}
```

The second approach is to use [`types`](https://www.typescriptlang.org/tsconfig#types):

```json
{
    "compilerOptions": {
        "types": ["node", "express", "nodecg/types/augment-window"]
    }
}
```

Both of these approaches have pros and cons, so be sure to read their corresponding [TypeScript tsconfig.json docs](https://www.typescriptlang.org/tsconfig) thoroughly.

### extension {#extension}

```ts
import NodeCG from 'nodecg/types';

export = (nodecg: NodeCG.ServerAPI) => {
    nodecg.sendMessage('message');
}
```

### dashboard/graphics {#dashboard}

```ts
// Some types get automatically injected into the global scope by our tsconfig.json.
// For everything else, you can `import NodeCG from 'nodecg/types'` just as in our extension example.
nodecg.listenFor('message', () => {
    // ...
})
```
