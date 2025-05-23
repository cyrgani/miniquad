# Miniquad

[![Github Actions](https://github.com/not-fl3/miniquad/workflows/Cross-compile/badge.svg)](https://github.com/not-fl3/miniquad/actions?query=workflow%3A)
[![Docs](https://docs.rs/miniquad/badge.svg?version=0.3.13)](https://docs.rs/miniquad/0.3.13/miniquad/index.html)
[![Crates.io version](https://img.shields.io/crates/v/miniquad.svg)](https://crates.io/crates/miniquad)
[![Discord chat](https://img.shields.io/discord/710177966440579103.svg?label=discord%20chat)](https://discord.gg/WfEp6ut)
[![Matrix](https://img.shields.io/matrix/quad-general:matrix.org?label=matrix%20chat)](https://matrix.to/#/#quad-general:matrix.org)

Miniquad is a manifestation of a dream in a world where we do not need a deep dependencies tree and thousands lines of code to draw things with a computer.

Miniquad aims to provide a graphics abstraction that works the same way on any platform with a GPU, being as light weight as possible while covering as many machines as possible. 

## Supported Platforms

* Windows, OpenGL 3, OpenGL 2.2;
* Linux, OpenGL 2.2, OpenGL 3, GLES 2, GLES 3;
* macOS, OpenGL 3, Metal;
* iOS, GLES 2, GLES 3, Metal;
* WASM, WebGL 1 - tested on iOS Safari, Firefox, Chrome;
* Android, GLES 2, GLES 3.

## Examples

![Imgur](https://i.imgur.com/TRI50rk.gif)

[examples/quad.rs](https://github.com/not-fl3/miniquad/blob/master/examples/quad.rs): [web demo](https://not-fl3.github.io/miniquad-samples/quad.html)<br/>
[examples/offscreen.rs](https://github.com/not-fl3/miniquad/blob/master/examples/offscreen.rs): [web demo](https://not-fl3.github.io/miniquad-samples/offscreen.html)<br/>

[PonasKovas/miniquad-mandelbrot](https://github.com/PonasKovas/miniquad-mandelbrot): [web demo](https://ponaskovas.github.io/miniquad-mandelbrot-wasm-demo/)

# Building examples

## Linux

```bash
cargo run --example quad
```

On NixOS Linux you can use [`shell.nix`](shell.nix) to start a development
environment where Miniquad can be built and run.

## Windows

```bash
# both MSVC and GNU target is supported:
rustup target add x86_64-pc-windows-msvc
# or
rustup target add x86_64-pc-windows-gnu

cargo run --example quad
```

## WASM

```bash
rustup target add wasm32-unknown-unknown
cargo build --example quad --target wasm32-unknown-unknown
```

And then use the following .html to load .wasm:

<details><summary>index.html</summary>

```html
<html lang="en">

<head>
    <meta charset="utf-8">
    <title>TITLE</title>
    <style>
        html,
        body,
        canvas {
            margin: 0px;
            padding: 0px;
            width: 100%;
            height: 100%;
            overflow: hidden;
            position: absolute;
            background: black;
            z-index: 0;
        }
    </style>
</head>

<body>
    <canvas id="glcanvas" tabindex='1'></canvas>
    <!-- Minified and statically hosted version of https://github.com/not-fl3/miniquad/blob/master/native/sapp-wasm/js/gl.js -->
    <script src="https://not-fl3.github.io/miniquad-samples/gl.js"></script>
    <script>load("quad.wasm");</script> <!-- Your compiled wasm file -->
</body>

</html>
```
</details>

One of the ways to server static .wasm and .html:

```no-run
cargo install basic-http-server
basic-http-server .
```

## Android

Recommended way to build for android is using Docker.<br/>
miniquad uses slightly modifed version of `cargo-apk`

```no-run
docker run --rm -v $(pwd)":/root/src" -w /root/src notfl3/cargo-apk cargo quad-apk build --example quad
```

APK file will be in `target/android-artifacts/(debug|release)/apk`

With "log-impl" enabled all log calls will be forwarded to adb console.
No code modifications for Android required, everything should just works.

## iOS

To run on the simulator:

```no-run
mkdir MyGame.app
cargo build --target x86_64-apple-ios --release
cp target/release/mygame MyGame.app
# only if the game have any assets
cp -r assets MyGame.app
cat > MyGame.app/Info.plist << EOF
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
<key>CFBundleExecutable</key>
<string>mygame</string>
<key>CFBundleIdentifier</key>
<string>com.mygame</string>
<key>CFBundleName</key>
<string>mygame</string>
<key>CFBundleVersion</key>
<string>1</string>
<key>CFBundleShortVersionString</key>
<string>1.0</string>
</dict>
</plist>
EOF

xcrun simctl install booted MyGame.app/
xcrun simctl launch booted com.mygame
```

For details and instructions on provisioning for real iphone, check [https://macroquad.rs/articles/ios/](https://macroquad.rs/articles/ios/)

## Cross Compilation

```bash

# windows target from linux host:
# this is how windows builds are tested from linux machine:
rustup target add x86_64-pc-windows-gnu
cargo run --example quad --target x86_64-pc-windows-gnu
```

# Goals

* Fast compilation time. Right now it is ~5s from "cargo clean" for both desktop and web.

* Cross platform. Amount of platform specific user code required should be kept as little as possible.

* Low-end devices support.

* Hackability. Working on your own game, highly probable some hardware incompability will be found. Working around that kind of bugs should be easy, implementation details should not be hidden under layers of abstraction.

* Forkability. Each platform implementation is, usually, just one pure Rust file. And this file is very copy-paste friendly - it doesnt use any miniquad specific abstractions. It is very easy to just copy some part of miniquad's platform implementation and use it standalone.

# Non-goals

* Ultimate type safety. Library should be entirely safe in Rust's definition of safe - no UB or memory unsafety. But correct GPU state is not type guaranteed. Feel free to provide safety abstraction in the user code then!

* High-end API, like Vulkan/DirectX 12. Take a look on [gfx-rs](https://github.com/gfx-rs/gfx) or [vulkano](https://github.com/vulkano-rs/vulkano) instead!

# Platinum sponsors

Miniquad is supported by:

[SourceGear](https://www.sourcegear.com/)
