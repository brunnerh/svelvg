# svelvg

> Convert SVG files into Svelte components with TypeScript definitions.

> Successor to [svg-to-svelte](https://github.com/metonym/svg-to-svelte)

This library transforms SVG files into Svelte components through the following:

- convert the `svg` file name into a valid module name (e.g., `alarm-fill` --> `AlarmFill`)
- forward `$$restProps` to the `svg` element while preserving attributes from the original `svg`
- add a default `slot` as child element to `svg`
- generate a TypeScript definition by extending the `SvelteComponentTyped` interface

## Usage

### CLI

The easiest way to use this library is through [npx](https://nodejs.dev/learn/the-npx-nodejs-package-runner).

The `glob` value represents the relative path to the folder containing SVG files inside `node_modules/`.

For example, say you have [bootstrap-icons](https://github.com/twbs/icons) installed as a development dependency, which contains icon SVG files in the "icons" folder.

```sh
npx svelvg glob=bootstrap-icons/icons
```

By default, the output directory is `"lib"`. Customize the directory using the `outDir` flag:

```sh
npx svelvg glob=bootstrap-icons/icons outDir=dist
```

Optionally, generate an index of icons (a list of all module names) by enabling the `iconIndex` flag. It will default to creating a `ICON_INDEX.md` file in your working directory.

```sh
npx svelvg glob=bootstrap-icons/icons iconIndex
```

Customize the icon index file name like so:

```sh
npx svelvg glob=bootstrap-icons/icons iconIndex=ICONS.md
```

### Node.js

Alternatively, install `svelvg` from NPM to use it programmatically.

```sh
# Yarn
yarn add -D svelvg

# NPM
npm i -D svelvg

# pnpm
pnpm i -D svelvg
```

Note that the top-level await requires Node.js v14 or greater.

```js
const svelvg = require("svelvg");

// Emits components to the `lib` folder
svelvg.createLibrary("bootstrap-icons/icons");

// Customize the output directory to be `dist`
svelvg.createLibrary("bootstrap-icons/icons", { outDir: "dist" });
```

### API

#### `createLibrary`

```ts
interface CreateLibraryOptions {
  /** @default "lib" */
  outDir: string;

  /** @default false */
  iconIndex: boolean | string;

  /**
   * Callback to add a list of classes to the SVG element
   * provided the original filename and module name
   * @example
   * filename: "alarm-fill"
   * moduleName: "AlarmFill"
   */
  appendClassNames: (filename: string, moduleName: string) => string[];

  /**
   * Override the default module name
   */
  toModuleName: (params: {
    path: path.ParsedPath;
    moduleName: string;
  }) => string;
}
```

## Template

Scaffold a new project using the [template](template):

```sh
npx degit metonym/svelvg/template <folder-name>
```

### `IconLibrary.svelte`

`svelvg` exports an `IconLibrary.svelte` to make listing and searching icons easier.

See [template/index.html](template/index.html) for an example.

```html
<script type="module">
  import IconLibrary from "svelvg/lib/IconLibrary.svelte";
  import * as icons from "./lib";
  import pkg from "./package.json";

  const app = new IconLibrary({
    target: document.getElementById("svelte"),
    props: {
      pkg,
      icons,
    },
  });
</script>
```

## License

[MIT](LICENSE)
