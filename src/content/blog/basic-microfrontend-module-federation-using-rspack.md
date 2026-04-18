---
title: "Basic Microfrontend Module Federation Using Rspack"
description: "Learn how to set up Module Federation with Rspack to build scalable microfrontend architectures."
pubDate: "Apr 18 2026"
---

Module Federation allows separately deployed applications to share code at runtime. Rspack has built-in support for it.

## Architecture

- **Remote**: Exposes components/modules to other apps
- **Host**: Consumes modules from remotes

## Remote App

```typescript
// remote/rspack.config.mts
import { rspack } from '@rspack/core';

export default {
  entry: './src/index.ts',
  plugins: [
    new rspack.container.ModuleFederationPlugin({
      name: 'remote',
      filename: 'remoteEntry.js',
      exposes: {
        './Button': './src/components/Button.tsx',
      },
      shared: {
        react: { singleton: true },
        'react-dom': { singleton: true },
      },
    }),
  ],
};
```

## Host App

```typescript
// host/rspack.config.mts
import { rspack } from '@rspack/core';

export default {
  entry: './src/index.ts',
  plugins: [
    new rspack.container.ModuleFederationPlugin({
      name: 'host',
      remotes: {
        remote: 'remote@http://localhost:3001/remoteEntry.js',
      },
      shared: {
        react: { singleton: true, eager: true },
        'react-dom': { singleton: true, eager: true },
      },
    }),
  ],
};
```

## Using Remote Modules

```typescript
// host/src/App.tsx
import React, { lazy, Suspense } from 'react';

const RemoteButton = lazy(() => import('remote/Button'));

function App() {
  return (
    <Suspense fallback="Loading...">
      <RemoteButton />
    </Suspense>
  );
}
```

## Running Locally

1. Start remote on port 3001
2. Start host on port 3000
3. Visit host app - remote component loads from remote server

## Key Options

| Option | Purpose |
|--------|---------|
| `exposes` | Modules to share (remote) |
| `remotes` | Remote entry URLs (host) |
| `shared` | Dependencies to deduplicate |
| `singleton` | Ensures single instance |

This is the basic pattern. From here you can add multiple remotes, shared state management, or explore Module Federation v2.0 features.
