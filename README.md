# Bun - Vite - Svelte - Rust - WebAssembly - Github Pages template project
A template for a very specific case when one would want for no good reason to have Bun as a package manager, Vite as a transpiler, Svelte as the framework, Rust as a language for functionality, WebAssembly to bring Rust to Svelte and deploy it as a static website on GitHub Pages

This hack arose primarily due to vite-plugin-wasm not having support for Bun currently and secondarily due to the inconsistent way Vite builds WebAssembly with Svelte

# TL;DR
- Create new directory for Rust
    - You need to change the directory name in
    - `.github/workflows/deploy.yml`
    - `.prettierignore`
    - `.eslintignore`
    - `vite.config.ts`
- Change repo name in `.github/workflows/deploy.yml`

# How to use
## Requirements
- git
- Bun (^1.0.7)
- rustc (^1.73.0)
- wasm-pack (^0.12.1)
## Steps
1. `git clone --depth=1`
2. `cd <directory name>`
3. `bun install`
4. `cargo new --lib <your Rust directory> --vcs none`
6. `wasm-pack build ./<your Rust dir> --target web && bun --bun run dev` to initialize
5. Walk through Templating

# Templating
Things to look out for, in the order of relevance
1. There is an `src/routes/Example.svelte` file that showcases how one exports WebAssembly to TypeScript (or JavaScript) in Svelte
2. You need an `onMount` in order for `bun --bun run dev` build to fetch WebAssembly properly. 
3. Unfortunately, all the functions you import from WebAssembly have to be declared as variables
4. That means, because `.wasm` is imported asynchronously, you need to satisfy `CallableFunction` interface on declaration, otherwise Vite server complains
5. You need to add `server.fs.allow: [<your Rust directory>/pkg]` to `vite.config.ts`, see the file
6. In `.github/workflows/deploy.yml` file to deploy to GitHub Pages you need to change the repository name and the name of the folder with the Rust source code
7. You would want to add the folder with Rust code to `.prettierignore` and `.eslintignore`
8. Currently, Playwright and Vitest don't seem to be supported by Bun, but they are included anyway, maybe to hack them together in the future
    - There are also issues with ESLint but it's likely to be just my skill issue

# Things that have changed from the skeleton Svelte project
0. `README.md`
1. `.nojekyll` in `static`
2. `.github/workflows/deploy.yml`
3. `bun.lockb` to `.gitignore`, `.eslintignore`, `.prettierignore`
4. "allowImportingTsExtensions" in `tsconfig.json`
5. static adapter and else in `svelte.config.js`
6. scripts in `package.json`
7. added "tabWidth" and removed "pluginSearchDirs" in `.prettierrc`
8. added `target/` and `Cargo.lock` from Rust directory to `.gitignore`
9. added `[dependencies] wasm-bindgen = "0.2"` and `[lib] crate-type = ["cdylib"]` to `Cargo.toml`
10. added `build.target: "esnext"` to `vite.config.ts`
11. `src/routes/+layout.js`
12. examples in `src/routes/Example.svelte`, `example-rs/`
13. added `.github/` to `.prettierignore` and `.eslintignore`
