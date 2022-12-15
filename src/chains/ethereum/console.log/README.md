# `@ganache/console.log`

A Solidity library and EVM hooks for using `console.log` from Solidity
contracts.

## Usage:

`@ganache/console.log` was designed to be used with the [`@ethereumjs/vm` npm
package](https://www.npmjs.com/package/@ethereumjs/vm).

Install `@ganache/console.log`:

```console
> npm install @ganache/console.log --save-exact
```

`@ganache/console.log` has two parts, a `console.sol` Solidity library file, and
a method to capture logs generated by the use of the `console.sol` library.

### `console.sol` Solidity Library

The `console.sol` library is available at `@ganache/console.log/console.sol`.
There are various ways to make this file available to your contracts and
Solidity build pipelines, so this part is left up to the user.

Use it in Solidity just like any other library, e.g.,

```solidity
// SPDX-License-Identifier: MIT
pragma solidity >= 0.4.22 <0.9.0;

import "console.sol";

contract MyContract {
    constructor() {
        console.log("got here");
    }
}
```

### `console.log` parser (`maybeGetLogs`):

To use `@ganache/console.log` directly within an instance of the
`@ethereumjs/vm` VM:

```typescript
import { ConsoleLogs, maybeGetLogs } from "@ganache/console.log";

// ... initialize vm ...

// vm should be an instance of @ethereumjs/vm
vm.evm.events.on("step", event => {
  const logs: ConsoleLogs | null = maybeGetLogs(event); // (string | bigint | boolean)[] | null
  // do something with `logs`
});
```

Note: Ganache CLI, as well as `Ganache.server` and `Ganache.provider`, already
implement the logger parser and will automatically log calls to `console.sol` to
standard output just like JavaScript's `console.log`.