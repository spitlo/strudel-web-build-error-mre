This is a minimal reproducible example of a build bug in `@strudel/web ^1.1.0`.

The moving parts (`index.html`, `package.json`) are copied from the Strudel Github repo [headless-repl example](https://github.com/tidalcycles/strudel/tree/main/examples/headless-repl).

The only change I’ve made is in `package.json`:

```diff
- "@strudel/web": "workspace:*"
+ "@strudel/web": "^1.1.0"
```

When running `vite build`, this build error appears:

```
vite v5.3.1 building for production...
✓ 3 modules transformed.
x Build failed in 140ms
error during build:
[vite:worker-import-meta-url] Could not resolve entry module "public/assets/clockworker--4w5RvGG.js".
file: <FOLDER_REDACTED>/strudel-web-build-error/node_modules/@strudel/web/dist/index.mjs
    at getRollupError (file://<FOLDER_REDACTED>/strudel-web-build-error/node_modules/rollup/dist/es/shared/parseAst.js:396:41)
    at error (file://<FOLDER_REDACTED>/strudel-web-build-error/node_modules/rollup/dist/es/shared/parseAst.js:392:42)
    at ModuleLoader.loadEntryModule (file://<FOLDER_REDACTED>/strudel-web-build-error/node_modules/rollup/dist/es/shared/node-entry.js:19123:20)
    at async Promise.all (index 0)
```

The error appears to come from this line:

<https://github.com/tidalcycles/strudel/blob/f514cd85b17fb5c16431d04f7ac4e25e3e2ac72c/packages/core/neocyclist.mjs#L23>

```js
this.worker = new SharedWorker(new URL('./clockworker.js', import.meta.url));
```
