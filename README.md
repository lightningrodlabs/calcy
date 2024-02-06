# Griffy!

Holochain hApp for collaborative diagramming.

Real-time colloaboration delivered by [syn](https://github.com/holochain/syn).

## Environment Setup

> PREREQUISITE: set up the [holochain development environment](https://developer.holochain.org/docs/install/).

Outside of nix shell you will need rust installed:

https://www.rust-lang.org/tools/install or https://rustup.rs/

Enter the nix shell by running this in the root folder of the repository: 

```bash
nix develop
npm install
```

**Run all the other instructions in this README from inside this nix-shell, otherwise they won't work**.

## Running 2 agents
 
```bash
npm start
```

This will create a network of 2 nodes connected to each other and their respective UIs.
It will also bring up the Holochain Playground for advanced introspection of the conductors.

## Running solo dev environment

```bash
npm run dev
```

This will not launch a UI, you will have to open a browser window and navigate to the Local address provided by VITE. It can take a moment for the UI to come up after visiting the URL, let it load. 

## Running the backend tests

```bash
npm test
```

## Bootstrapping a network

Create a custom network of nodes connected to each other and their respective UIs with:

```bash
AGENTS=3 npm run network
```

Substitute the "3" for the number of nodes that you want to bootstrap in your network.
This will also bring up the Holochain Playground for advanced introspection of the conductors.

## Packaging

To package the web happ:
``` bash
npm run package
```

You'll have the `griffy.webhapp` in `workdir`. This is what you should distribute so that the Holochain Launcher can install it.
You will also have its subcomponent `griffy.happ` in the same folder`.

## Documentation

This repository is using these tools:
- [NPM Workspaces](https://docs.npmjs.com/cli/v7/using-npm/workspaces/): npm v7's built-in monorepo capabilities.
- [hc](https://github.com/holochain/holochain/tree/develop/crates/hc): Holochain CLI to easily manage Holochain development instances.
- [@holochain/tryorama](https://www.npmjs.com/package/@holochain/tryorama): test framework.
- [@holochain/client](https://www.npmjs.com/package/@holochain/client): client library to connect to Holochain from the UI.
- [@holochain-playground/cli](https://www.npmjs.com/package/@holochain-playground/cli): introspection tooling to understand what's going on in the Holochain nodes.


## License

[![License: CAL 1.0](https://img.shields.io/badge/License-CAL%201.0-blue.svg)](https://github.com/holochain/cryptographic-autonomy-license)

Copyright (C) 2023, Holochain Foundation

This program is free software: you can redistribute it and/or modify it under the terms of the license
provided in the LICENSE file (MIT). This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
