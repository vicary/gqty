# @gqty/cli

## 3.2.0

### Minor Changes

- dd47986: New option `"fetchOptions"`, added to the [`resolved`](https://gqty.dev/docs/client/fetching-data#resolved) client function, that allows for giving extra configurations to the expected [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) call.

  This enables, for example, the customization of the headers sent for a specific query or to pass an [AbortSignal](https://developer.mozilla.org/en-US/docs/Web/API/AbortSignal).

  ```ts
  import { resolved, query } from '../gqty';

  // ...

  const controller = new AbortController();

  await resolved(() => query.currentUser?.email, {
    fetchOptions: {
      headers: {
        'Content-Type': 'application/json',
        authorization: 'secret_token',
      },
      signal: controller.signal,
    },
  });
  ```

  For already generated clients to be able to use this new option, it is required manually modify the existing query fetcher, to do for example:

  ```ts
  const queryFetcher: QueryFetcher = async function (
    query,
    variables,
    fetchOptions
  ) {
    const response = await fetch(endpoint, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        query,
        variables,
      }),
      ...fetchOptions,
    });

    const json = await response.json();

    return json;
  };
  ```

- b7c8710: Fix generator: Argument is required only if it is non-null and does not have default value. Previously only checking if it is non-null.

### Patch Changes

- Updated dependencies [dd47986]
  - gqty@2.2.0

## 3.1.0

### Minor Changes

- 4c43e7a: New "transformSchema" configuration

### Patch Changes

- 46efcd2: Use `undici` for network schema introspection
- 18e719b: Fix interop graphql import

## 3.0.1

### Patch Changes

- 3ac7bee: Use "import \* as" to prevent esm-cjs interop issues

## 3.0.0

### Major Changes

- c7b80c8: Minimize npm dependencies

### Minor Changes

- f9fd0b9: Added the ability to use fast-glob patterns in the schema generate endpoint to be able to use and combine multiple gql/graphql schema files

## 2.3.1

### Patch Changes

- 96ab370: Change export syntax to fix bundling issues, show a warning on existent generated clients with `export const {` syntax.

  Closes #292

## 2.3.0

### Minor Changes

- 0b1860a: lexicographic sort GraphQL Schema before generating
- 82fd259: Use ESM for CLI

### Patch Changes

- Updated dependencies [6df0318]
  - gqty@2.1.0

## 2.2.0

### Minor Changes

- cba5c43: Don't skip config read on NODE_ENV === "test"
- a4fc294: New [Envelop](https://www.envelop.dev/) / [graphql-ez](https://www.graphql-ez.com/) plugin that automatically generates gqty code based on schema and gqty.config.cjs

  ```ts
  // graphql-ez

  // ...
  import { useGenerateGQty } from '@gqty/cli/envelop';

  const ezApp = CreateApp({
    // ...
    envelop: {
      plugins: [
        // ...
        useGenerateGQty({
          // ...
        }),
      ],
    },
  });
  ```

  ```ts
  // Envelop

  import { envelop } from '@envelop/core';
  import { useGenerateGQty } from '@gqty/cli/envelop';

  //...

  const getEnveloped = envelop({
    plugins: [
      // ...
      useGenerateGQty({
        // ...
      }),
    ],
  });
  ```

### Patch Changes

- aab8e48: Fix: don't put invalid default generate endpoint
- f72aa23: GraphQL v16 compatibility

  closes #268

## 2.1.2

### Patch Changes

- 2b33e6a: Add missing useSubscription in generated react code

## 2.1.1

### Patch Changes

- f4ddac9: Enforce `"importsNotUsedAsValues"` & `"preserveValueImports"` using `import type`
- ff821ef: default config react enabled only if "react" dependency is found

## 2.1.0

### Minor Changes

- c993f2e: Sort alphabetically generated code

### Patch Changes

- 0a30558: Fix misplaced semi-colons in input types

## 2.0.1

### Patch Changes

- 28e2c09: [Bug fixing breaking change] Fix types and retrieval of unions/interfaces of different object types
- Updated dependencies [28e2c09]
  - gqty@2.0.1

## 2.0.0

### Major Changes

- 3586c45: Change previous unstable `Unions` support with new `"$on"` property with support for both `Unions` & `Interfaces`

### Patch Changes

- Updated dependencies [3586c45]
- Updated dependencies [3586c45]
  - gqty@2.0.0

## 1.1.4

### Patch Changes

- 5446d83: Fix/Improve config loading
- Updated dependencies [1fc2672]
  - gqty@1.1.2

## 1.1.3

### Patch Changes

- 9faebb6: fix dynamic import transpilation

## 1.1.2

### Patch Changes

- d097ae3: fix ESM build

## 1.1.1

### Patch Changes

- cad0d92: fix false positive error after second call on inspectWriteGenerate

## 1.1.0

### Minor Changes

- f860c01: change \_\_typename undefined to optional undefined

### Patch Changes

- Updated dependencies [a216972]
  - gqty@1.1.0

## 1.0.4

### Patch Changes

- bd31ab8: fix cli package json read

## 1.0.3

### Patch Changes

- f0315c4: fix js target

## 1.0.2

### Patch Changes

- 0f9e0e8: fix build

## 1.0.1

### Patch Changes

- 3784c2b: release
- Updated dependencies [3784c2b]
  - gqty@1.0.1

## 2.0.18

### Patch Changes

- a88d4e8: fix generated schema use scoped gqty

## 2.0.17

### Patch Changes

- 3f08372: publish fork
- Updated dependencies [3f08372]
  - gqty@2.0.15

## 2.0.16

### Patch Changes

- 422eb9a: fix importsNotUsedAsValues error on generated schema
- Updated dependencies [422eb9a]
- Updated dependencies [422eb9a]
  - gqty@2.0.14

## 2.0.15

### Patch Changes

- 4a3d5ef: allow introspection json without "data" field
- af6a437: - Rename `gqtyConfig` to `GQtyConfig` (so it's consistent with the new logo)
  - Rename `gqtyError` to `GQtyError`
  - Remove `endpoint` option from the configuration, and instead always defaults to introspection one
    - It's confusing why theres two of them, and the user can change it later by modifying the file anyway
- 4a3d5ef: disable config file write if no cli usage
- Updated dependencies [4a3d5ef]
- Updated dependencies [af6a437]
  - gqty@2.0.13

## 2.0.14

### Patch Changes

- 7e084fb: accept json introspection schema result

## 2.0.13

### Patch Changes

- 9b9d127: add "usePaginatedQuery" hook
- Updated dependencies [85a389c]
- Updated dependencies [cca9d02]
- Updated dependencies [0904297]
  - gqty@2.0.11

## 2.0.12

### Patch Changes

- Updated dependencies [65c4d32]
  - gqty@2.0.10

## 2.0.11

### Patch Changes

- 5d89cbd: fix generated interfaces
- Updated dependencies [6a9269f]
  - gqty@2.0.9

## 2.0.10

### Patch Changes

- Updated dependencies [c74442e]
- Updated dependencies [d78f2ab]
- Updated dependencies [0ffaa9d]
  - gqty@2.0.8

## 2.0.9

### Patch Changes

- Updated dependencies [ff66195]
- Updated dependencies [63fd3ea]
- Updated dependencies [40d2101]
  - gqty@2.0.7

## 2.0.8

### Patch Changes

- 1eaa4b4: fix not null args & nullable scalar fields

## 2.0.7

### Patch Changes

- eb45ca2: improve generate config conflict warning
- Updated dependencies [173e11d]
- Updated dependencies [c613410]
  - gqty@2.0.6

## 2.0.6

### Patch Changes

- 8f1a329: add "ignoreArgs" schema transform
- Updated dependencies [6fef085]
  - gqty@2.0.5

## 2.0.5

### Patch Changes

- 940883a: fix yarn berry compatibility

## 2.0.4

### Patch Changes

- 2bf4ce2: improve config validation & add javascript output
- Updated dependencies [2bf4ce2]
  - gqty@2.0.4

## 2.0.3

### Patch Changes

- 27f9ece: set "graphql" as optional peerDependency
- Updated dependencies [27f9ece]
  - gqty@2.0.3

## 2.0.2

### Patch Changes

- 7d932f8: fix import gqty
- 6f9416d: set "gqty" as direct dependency
- Updated dependencies [c06ef80]
  - gqty@2.0.2

## 2.0.1

### Patch Changes

- a57cab4: official beta v2 publish
- Updated dependencies [a57cab4]
  - gqty@2.0.1
