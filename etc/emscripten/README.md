Code to allow building gap to WASM using Emscripten.

Files:

- `build.sh`: Run as `etc/emscripten/build.sh` from a fresh copy of GAP.

- `web-template`: Uses 'xterm-pty' to create a "nice" interface to the Wasm GAP.

- `build_startup_manifest.js`: Run it in the web root directory to build `startup_manifest.json` that contains resources to preload.

See `run-web-demo.sh` as an example on how to set up a working website.

Note that this demo uses xterm-pty, a library which provides a terminal interface
for emscripten-compiled programs. This uses a javascript feature called 
"SharedArrayBuffer", which requires some headers are returned by the server:

```
Cross-Origin-Opener-Policy: same-origin
Cross-Origin-Embedder-Policy: require-corp
```

For more details, see for [this article](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer).

The file "coi-serviceworker.js" works around this problem on Github pages. Alternatively, "server.rb" is a simple ruby script, which just starts a web-server which returns the required headers.

Setup Instructions:

- install emscripten:
```
git clone https://github.com/emscripten-core/emsdk.git
cd emsdk
./emsdk install 3.1.23
./emsdk activate 3.1.23
source ./emsdk_env.sh
```
- compile gap to WASM:
Fromm root directory of this git repository, run 
```
bash etc/emscripten/build.sh
```
- setup website:
Fromm root directory of this git repository, run 
```
bash etc/emscripten/run-web-demo.sh
```
- build `startup_manifest.json` for faster preloading
Fromm root directory of this git repository, run 
```
cp etc/emscripten/build_startup_manifest.js web-example/
cd web-example
node build_startup_manifest.js
```
Then go to localhost:9999, toggle the web developer console and wait for gap to initialize, then run the following command in gap web shell:
```
SizeScreen([100000, 100000]);
??a
```
You may remove `build_startup_manifest.js` after the loading is finished.
