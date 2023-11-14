
# Goals

- use latest Node LTS (20.9)
- use typescript
- import of local files with no extension
- use pnpm as package manager
- prefer tsx over ts-node

# Highlights

- Requires `ts-node` configuration in `tsconfig.json` for first execution
- Manually set tsx loader through `node-args` because the plugin uses a deprecated version of esbuild loaders
- List `@tapjs/test` and `@isaacs/ts-node-temp-fork-for-pr-2009` as dependencies because pnpm tries to be annoyingly secure

# Usage

```
pnpm install
pnpm test # first pass
pnpm test # second pass, for comparison
```

> NOTE:
> 
> because of `tshy`, prefer clean executions: delete `node_modules` after each test

# Problems

---

After a clean `pnpm install`, running `tap` gives:

```
node:internal/process/esm_loader:40
      internalBinding('errors').triggerUncaughtException(
                                ^

[Error: Cannot find package '@tapjs/test' imported from /Users/abianco/Desktop/test/x] {
  code: 'ERR_MODULE_NOT_FOUND'
}
```

> Solution
> 
> pnpm only, npm does not need this
> 
> add `@tapjs/test` to dependencies and do a clean install
> 
> could also be resolved by using `shamefully-hoist=true` to get access to every sub-dependency

---

After a clean `pnpm install`, running `tap` gives:


```
node:internal/process/esm_loader:40
      internalBinding('errors').triggerUncaughtException(
                                ^

Error [ERR_MODULE_NOT_FOUND]: Cannot find package '@isaacs/ts-node-temp-fork-for-pr-2009' imported from /Users/abianco/Desktop/test/
Did you mean to import @isaacs+ts-node-temp-fork-for-pr-2009@10.9.5_@types+node@20.9.0_typescript@5.2.2/node_modules/@isaacs/ts-node-temp-fork-for-pr-2009/import.mjs?
    at new NodeError (node:internal/errors:406:5)
    at packageResolve (node:internal/modules/esm/resolve:789:9)
    at moduleResolve (node:internal/modules/esm/resolve:838:20)
    at defaultResolve (node:internal/modules/esm/resolve:1043:11)
    at ModuleLoader.defaultResolve (node:internal/modules/esm/loader:383:12)
    at ModuleLoader.resolve (node:internal/modules/esm/loader:352:25)
    at ModuleLoader.getModuleJob (node:internal/modules/esm/loader:228:38)
    at ModuleLoader.import (node:internal/modules/esm/loader:315:34)
    at node:internal/process/esm_loader:26:84
    at node:internal/per_context/primordials:544:39 {
  code: 'ERR_MODULE_NOT_FOUND'
}

Node.js v20.9.0
/Users/abianco/Desktop/test/node_modules/.pnpm/@tapjs+run@1.4.15_@tapjs+core@1.4.5_@types+node@20.9.0_react-dom@18.2.0_react@18.2.0/node_modules/@tapjs/run/src/build.ts:29
          Object.assign(new Error('build failed'), {
                        ^


Error: build failed
    at <anonymous> (/Users/abianco/Desktop/test/node_modules/.pnpm/@tapjs+run@1.4.15_@tapjs+core@1.4.5_@types+node@20.9.0_react-dom@18.2.0_react@18.2.0/node_modules/@tapjs/run/src/build.ts:29:25)
    at ChildProcess.<anonymous> (/Users/abianco/Desktop/test/node_modules/.pnpm/foreground-child@3.1.1/node_modules/foreground-child/src/index.ts:163:20)
    at ChildProcess.emit (node:events:514:28)
    at maybeClose (node:internal/child_process:1105:16)
    at ChildProcess._handle.onexit (node:internal/child_process:305:5) {
  code: 1,
  signal: null
}

Node.js v20.9.0
```

> Solution
> 
> pnpm only, npm does not need this
> 
> add `@isaacs/ts-node-temp-fork-for-pr-2009` to dependencies and do a clean install
> 
> could also be resolved by using `shamefully-hoist=true` to get access to every sub-dependency

---

After a clean `pnpm install`, running `tap` gives:

```
> test@1.0.0 test /Users/abianco/Desktop/test
> tap


> @tapjs/test-built@0.0.0 prepare
> tshy

 SKIP  index.spec.ts 0 OK 610ms
 ~ no tests found

2> index.spec.ts

node:internal/process/esm_loader:40
      internalBinding('errors').triggerUncaughtException(
                                ^
Error: Cannot find module '/Users/abianco/Desktop/test/index' imported from /Users/abianco/Desktop/test/index.spec.ts
    at finalizeResolution (/Users/abianco/Desktop/test/node_modules/.pnpm/@isaacs+ts-node-temp-fork-for-pr-2009@10.9.5_@types+node@20.9.0_typescript@5.2.2/node_modules/@isaacs/ts-node-temp-fork-for-pr-200
9/dist-raw/node-internal-modules-esm-resolve.js:366:11)
    at moduleResolve (/Users/abianco/Desktop/test/node_modules/.pnpm/@isaacs+ts-node-temp-fork-for-pr-2009@10.9.5_@types+node@20.9.0_typescript@5.2.2/node_modules/@isaacs/ts-node-temp-fork-for-pr-2009/dis
t-raw/node-internal-modules-esm-resolve.js:801:10)
    at Object.defaultResolve (/Users/abianco/Desktop/test/node_modules/.pnpm/@isaacs+ts-node-temp-fork-for-pr-2009@10.9.5_@types+node@20.9.0_typescript@5.2.2/node_modules/@isaacs/ts-node-temp-fork-for-pr-
2009/dist-raw/node-internal-modules-esm-resolve.js:912:11)
    at <anonymous>
(/Users/abianco/Desktop/test/node_modules/.pnpm/@isaacs+ts-node-temp-fork-for-pr-2009@10.9.5_@types+node@20.9.0_typescript@5.2.2/node_modules/@isaacs/ts-node-temp-fork-for-pr-2009/src/esm.ts:249:65)
    at entrypointFallback
(/Users/abianco/Desktop/test/node_modules/.pnpm/@isaacs+ts-node-temp-fork-for-pr-2009@10.9.5_@types+node@20.9.0_typescript@5.2.2/node_modules/@isaacs/ts-node-temp-fork-for-pr-2009/src/esm.ts:207:34)
    at <anonymous>
(/Users/abianco/Desktop/test/node_modules/.pnpm/@isaacs+ts-node-temp-fork-for-pr-2009@10.9.5_@types+node@20.9.0_typescript@5.2.2/node_modules/@isaacs/ts-node-temp-fork-for-pr-2009/src/esm.ts:249:14)
    at addShortCircuitFlag
(/Users/abianco/Desktop/test/node_modules/.pnpm/@isaacs+ts-node-temp-fork-for-pr-2009@10.9.5_@types+node@20.9.0_typescript@5.2.2/node_modules/@isaacs/ts-node-temp-fork-for-pr-2009/src/esm.ts:417:10)
    at resolve
(/Users/abianco/Desktop/test/node_modules/.pnpm/@isaacs+ts-node-temp-fork-for-pr-2009@10.9.5_@types+node@20.9.0_typescript@5.2.2/node_modules/@isaacs/ts-node-temp-fork-for-pr-2009/src/esm.ts:229:12)
    at nextResolve (node:internal/modules/esm/hooks:833:28)
    at resolve (/Users/abianco/Desktop/test/node_modules/.pnpm/@tapjs+mock@1.2.14_@tapjs+core@1.4.5/node_modules/@tapjs/mock/src/hooks.mts:71:7)

Node.js v20.9.0
```

Running again immediately after, goes in timeout and leaves a node process hanging and **consuming all system resources**.

> 
> Solution:
> 
> uninstall `@tapjs/esbuild-kit` and remove it from plugins, since `esbuild-kit` has been deprecated in favor of `tsx`
> install `tsx` and add it to the `node-arg` config of taprc file
> redefine inclusion of ts files (since disabling `@tapjs/typescript`removes them from the default list

---

Fist execution (after a clean install) fails to process imports, spawned processes seem to use `@isaacs/ts-node-temp-fork-for-pr-2009` for resolution, event if it should not be needed

```
TAP 58848 TAP:     spawnargs: [
TAP 58848 TAP:       '/Users/abianco/.asdf/installs/nodejs/20.9.0/bin/node',
TAP 58848 TAP:       '--import=file:///Users/abianco/Desktop/test/node_modules/.pnpm/@isaacs+ts-node-temp-fork-for-pr-2009@10.9.5_@types+node@20.9.0_typescript@5.2.2/node_modules/@isaacs/ts-node-temp-fork
-for-pr-2009/import.mjs',
TAP 58848 TAP:       '--import=file:///Users/abianco/Desktop/test/node_modules/.pnpm/@tapjs+mock@1.2.14_@tapjs+core@1.4.5/node_modules/@tapjs/mock/dist/esm/import.mjs',
TAP 58848 TAP:       '--enable-source-maps',
TAP 58848 TAP:       '--import=file:///Users/abianco/Desktop/test/node_modules/.pnpm/@tapjs+processinfo@3.1.6/node_modules/@tapjs/processinfo/dist/esm/import.mjs',
TAP 58848 TAP:       '--import=tsx',
TAP 58848 TAP:       '/Users/abianco/Desktop/test/src/test/foo.spec.ts'
TAP 58848 TAP:     ],
```

A second execution completes successfully and `@isaacs/ts-node-temp-fork-for-pr-2009` is not used as expected

```
TAP 59455 TAP:     spawnargs: [
TAP 59455 TAP:       '/Users/abianco/.asdf/installs/nodejs/20.9.0/bin/node',
TAP 59455 TAP:       '--import=file:///Users/abianco/Desktop/test/node_modules/.pnpm/@tapjs+mock@1.2.14_@tapjs+core@1.4.5/node_modules/@tapjs/mock/dist/esm/import.mjs',
TAP 59455 TAP:       '--enable-source-maps',
TAP 59455 TAP:       '--import=file:///Users/abianco/Desktop/test/node_modules/.pnpm/@tapjs+processinfo@3.1.6/node_modules/@tapjs/processinfo/dist/esm/import.mjs',
TAP 59455 TAP:       '--import=tsx',
TAP 59455 TAP:       '/Users/abianco/Desktop/test/src/test/foo.spec.ts'
TAP 59455 TAP:     ],
```

Second execution is about 40% faster

> Solution:
> 
> add `ts-node` config to `tsconfig.json` to set `experimentalSpecifierResolution` flag to `node`
