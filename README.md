# parcel2-react-types-bug

Reproduction of bugs generating typescript type in parcel 2 rc0.

## Bug 1: Error generating types for a library

The attached library is meant to be used as a design system, as such it contains a library entry point along with react and a single component. It's built in typescript.

When build is ran without a parcel-cache, provided a type entry point is provided, you will get the following errors:

```
@parcel/transformer-typescript-types: Cannot find name 'Set'. Do you need to change your target library? Try changing the 'lib' compiler option to 'es2015' or later.

  /code/personal/parcel2-react-types-bug/node_modules/@types/react/index.d.ts:415:23
    414 |         commitTime: number,
  > 415 |         interactions: Set<SchedulerInteraction>,
  >     |                       ^^^^ Cannot find name 'Set'. Do you need to change your target library? Try changing the 'lib' compiler option to 'es2015' or later.
    416 |     ) => void;
    417 |     interface ProfilerProps {

@parcel/transformer-typescript-types: Module '"/Users/steve/code/personal/parcel2-react-types-bug/node_modules/@types/react/index"' can only be default-imported using the 'esModuleInterop' flag

  /code/personal/parcel2-react-types-bug/src/Component.tsx:1:8
  > 1 | import React from "react";
  >   |        ^^^^^^ Module '"/code/personal/parcel2-react-types-bug/node\_modules/@types/react/index"' can only be default-imported using the 'esModuleInterop' flag
    2 |
    3 | export const Component: React.FC = () => (

```

You will get these errors even if you switch to using `@parcel/transformer-typescript-tsc` or explictly add a tsconfig with the above values set. Interestingly types do successfully generate.

## Bug 2 parcel seems to only check "dependancies" for the existence of transformer-react-refresh-wrap

If you run `yarn watch` you will get the following error:

```
@parcel/resolver-default: External dependency "@parcel/transformer-react-refresh-wrap" is not declared in package.json.

  /code/personal/parcel2-react-types-bug/package.json:14:3
    13 |   },
  > 14 |   "dependencies": {
  >    |   ^^^^^^^^^^^^^^
    15 |     "react": "17",
    16 |     "react-dom": "17"
```

even though it is present in "devDependencies". Moving the entry in package.json to "dependencies" fixes this error but this isn't where this package belongs.
