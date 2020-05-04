# Rust, Neon, Electron

Electron running native Rust code is a very appealing idea: we get a decent cross-platform user interface with all the performance of native Rust underneath it, and the convenience of Node.js in the middle.

Neon? It's a really useful project that permits us to write native Rust modules that work with Node.js. 

I wanted to create a working example, particularly one that works on Windows as well as Linux. Probably works on Mac too.

* This repo is forked from https://github.com/electron/electron-quick-start
* I have added to it following the instructions on the Neon site: https://neon-bindings.com/docs/electron-apps/

## Testing

* `npm i` installs the dependencies
* `npm run build` runs native Rust builds targeting Electron specifically
* `npm start` starts the app

## Details

For a coder not familiar with npm or Node, there are some things to be aware of: 

* Node integration needs to be enabled in `main.js`: `nodeIntegration: true`
* Neon bindings are not currently context aware or NAPI, so there are deprecation warnings. I'd expect this to improve at some point. 
* I had to comment out `<meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'">` to enable Electron to run the native packages; there might be a better way to do this.

On the Rust side, 

* Don't despair; the Rust build only takes a while on the first run. Incremental builds after changes are quick.
* `npm i` installs the dependencies, 
    * Our local `hello` package is configured to build correctly on installation:
        * [package.json](hello/package.json) contains `"install": "electron-build-env neon build --release"` 
* The main build script is in the root `packages.js`
* `npm run build` will do the Rust build of the native libraries
    * Local `hello` is already correctly built as noted above, but this will re-build it if there are changes
    * Imported `@amilajack/neon-hello` is *not* correctly built, as it is built for Node without Electron; the build call here re-builds it targeting electron; it's not an electron-specific example: https://github.com/amilajack/neon-hello
    * `electron-build-env` sets up the correct environment variables to target Electron, rather than normal Node
    * `neon build -p {path} --release` uses `neon-cli` to run the Rust build; Note that it needs to be `--release` on Windows
* Local `hello` was created using neon-cli by doing `neon new hello`
    * on Windows, it's a build dependency, so accessible in `node_modules\.bin\neon` 
    * `hello` was modified to export the native module with `module.exports = require('../native');` in `hello/lib/index.js`

## Tooling

This project is working on Windows 10 and Ubuntu 20.04 with the following tooling:

* Rust 1.43 (stable)
* npm 6.14.4
* Visual Studio 2019 Community Edition, with MSVC v142 (v14.25) installed (only on Windows of course)

# Original template

Based on this: https://github.com/electron/electron-quick-start; I've left the original documentation below. Refer to the original repo for updates.

# electron-quick-start

**Clone and run for a quick way to see Electron in action.**

This is a minimal Electron application based on the [Quick Start Guide](https://electronjs.org/docs/tutorial/quick-start) within the Electron documentation.

**Use this app along with the [Electron API Demos](https://electronjs.org/#get-started) app for API code examples to help you get started.**

A basic Electron application needs just these files:

- `package.json` - Points to the app's main file and lists its details and dependencies.
- `main.js` - Starts the app and creates a browser window to render HTML. This is the app's **main process**.
- `index.html` - A web page to render. This is the app's **renderer process**.

You can learn more about each of these components within the [Quick Start Guide](https://electronjs.org/docs/tutorial/quick-start).

## To Use

To clone and run this repository you'll need [Git](https://git-scm.com) and [Node.js](https://nodejs.org/en/download/) (which comes with [npm](http://npmjs.com)) installed on your computer. From your command line:

```bash
# Clone this repository
git clone https://github.com/electron/electron-quick-start
# Go into the repository
cd electron-quick-start
# Install dependencies
npm install
# Run the app
npm start
```

Note: If you're using Linux Bash for Windows, [see this guide](https://www.howtogeek.com/261575/how-to-run-graphical-linux-desktop-applications-from-windows-10s-bash-shell/) or use `node` from the command prompt.

## Resources for Learning Electron

- [electronjs.org/docs](https://electronjs.org/docs) - all of Electron's documentation
- [electronjs.org/community#boilerplates](https://electronjs.org/community#boilerplates) - sample starter apps created by the community
- [electron/electron-quick-start](https://github.com/electron/electron-quick-start) - a very basic starter Electron app
- [electron/simple-samples](https://github.com/electron/simple-samples) - small applications with ideas for taking them further
- [electron/electron-api-demos](https://github.com/electron/electron-api-demos) - an Electron app that teaches you how to use Electron
- [hokein/electron-sample-apps](https://github.com/hokein/electron-sample-apps) - small demo apps for the various Electron APIs

## License

[CC0 1.0 (Public Domain)](LICENSE.md)
