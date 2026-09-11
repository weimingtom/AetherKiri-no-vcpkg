# AetherKiri-no-vcpkg
[WIP] My AetherKiri 0.2.4 fork for linux, without vcpkg

## Build and run for Ubuntu 25.04 64bit VMware
* sudo apt update
* sudo apt install gedit lftp make gcc g++ pkg-config
* sudo apt install libfmt-dev libonig-dev libspdlog-dev libwebp-dev libfreetype-dev libopencv-core-dev libopencv-imgproc-dev liblz4-dev libvorbis-dev libopusfile-dev libopenal-dev libsdl2-dev libgtk2.0-dev libtinyxml2-dev libuchardet-dev libglfw3-dev
* sudo apt install libgles2-mesa-dev libegl1-mesa-dev mesa-utils 
* sudo apt install libboost-locale-dev libavcodec-dev libswscale-dev libavformat-dev
* cd godot-cpp-10.0.0-rc1/
* make clean
* make -j8
* cd ..
* make clean
* make -j8
* cd ..
* unzip Godot_v4.7-stable_linux.x86_64.zip
* ./Godot_v4.7-stable_linux.x86_64
* (Import ./apps/godot_app/project.godot, and run)  

## weibo record
```
我测试过KrKr2-Next的非官方发布版（在AetherKiri的release页面有apk），
确定在某些手机设备（例如红米13C）上可以运行，我猜测可能只能在Android 14，如果是Android 12也有可能黑屏 ​​​​

转，gh上有个开源项目AetherKiri/AetherKiri，继承自flutter版的reAAAq/KrKr2-Next的代码，
我看到它好像在整大活，把代码移植到godot引擎上，至于是什么原理就不清楚了（可能这位作者想在ios上运行？
基于GDExtension‌？），它没有把最新的安卓版发布出来，我是从github actions页那里下载的，
运行效果如图，当然我不确保所有安卓设备都能运行。另外这个安卓版中滚动的控件可能会滚动不了，
我觉得这个的bug比flutter版更多。现在没时间研究这个，可能也要等年底

我可能会研究一下AetherKiri，因为这项目基于Godot C++ GDExtensions，而C++ GDExtensions的意思
是指用C++实现godot的类（通常是用gdscript而非C++），然后可以用类名字符串实例化这个类
（通常是类名.instantiate()，但这里用ClassDB.instantiate(类名)），在gdscript中调用
这个Node节点类的子类进行显示——我觉得跑通C++ GDExtensions例子比跑通这个项目可能还会更好玩，
所以准备有时间试一下，看能不能在Linux下编译运行

AetherKiri研究。我暂时未跑AetherKiri这个项目的代码，我先试试跑godotengine/godot-cpp-template。
scons可以很轻松编译出动态库，so文件输出到project\bin\linux目录下（对应example.gdextension里面的相对路径），
然后用godot 4.4加载project/project.godot即可。不过这里有几个问题，首先编译会很慢，其次用godot 4.1会运行失败，
看错误提示会提示一定要用godot 4.4才能正常运行。接下来有空试试换成cmake看能不能编译（对应godot-cpp的顶层目录，
也可以用cmake编译）

AetherKiri研究。测试cmake编译godot-cpp-template。上次忘记说开发环境，是ubuntu 25.04，清除项目是scons -c，
然后用cmake编译也可以mkdir build;cd build;cmake ..;make -j8然后不需要install，编译完成后会把.so文件输出到
project/bin/linux目录下（补注：so文件需要自己重命名，添加lib前缀），然后用godot 4.4.1加载project目录即可

我记录下的krkr2项目，有6个还没研究，其中有2个是活跃的，AetherKiri和kirikiroid2-web，我可能先研究这两个，
其他就随意了。不要再来了，再多我就受不了，不过代码库越多，也是好事，我可以参考的代码就更多

AetherKiri研究。Ubuntu 25.04 VMware 运行效果如下 ​​​

AetherKiri研究。在Ubuntu 25.04 VMware下用官方介绍的cmake+vcpkg方法去编译Linux版。
首先关于git仓库的子模块，无视就行，这里为了简单，我只编译了0.2.4这个tag的代码git checkout -f 0.2.4
（只有一个闭源子模块）。然后用cmake编译，它是用vcpkg编译依赖的，基本就是用这两个来编译：
./build.sh和./tools/setup_linux.sh，如果你不安装vcpkg直接执行build.sh，它会提示执行setup_linux.sh，
可以认为setup_linux.sh就是下载安装vcpkg的（还有godot ide和template），不过setup_linux.sh的最后
会触发template解压失败，忽略就可以了（后面打包godot就会报错，但我只需要用godot ide导入即可），
然后用./build.sh执行cmake+vcpkg配置和编译，最后的godot template报错可以忽略，编译出很多个so文件输出
到./apps/godot_app/bin/linux/debug/目录下（包括libaether_kiri_godot.so），然后用godot ide（linux版，
版本是v4.7，我怀疑4.4和以上都支持）导入这个目录./apps/godot_app/project.godot然后运行即可

AetherKiri研究。我现在是基于0.2.4版本来修改（未改好）。这开源项目基于KrKr2-Next
（还有另外一批开源项目是基于另外一个krkr2），但改动非常大（连注释也改，还有很底层的脚本引擎也改），
我都不指望我的修改版编译出来能运行了——例如ScriptMgnIntf.cpp里面加了很多兼容代码。TextStream.cpp则是编码。
总体来说这个开源项目应该是插件实现比较齐全（另一个类似情况则是krkrsdl3），当然能不能用，我不好说，
我跑它官方打包的安卓版就跑不通，也许它的修改重心在ios上而非安卓。但改动那么多是不是好事，我不敢断言。
如果笼统来说，krkr2, krkr2-next, krkrsdl3这三个主要分支都很值得研究，如果说最完美，可能目前只有krkrsdl3会比较好，
当然我不太想研究插件，我觉得很多情况都用不上，和ffmpeg一样，我觉得不看这些代码会更好，
最好能用某种方法跳过来实现一个超级轻量级的实现（我以前甚至连音频输出也去掉）

AetherKiri研究。我暂时试试用ubuntu 25.04和KrKr2-Next-no-vcpkg的Makefile编译一下
（以前好像用xubuntu 20会好一些）。但实际上还需要对一下cpp文件，而且这个要编译godot扩展
的c++头文件，我先想好makefile再试。暂时还没有遇到太大的问题（因为我还没开始编译它核心的动态库）。
非常花时间，我都不想改

AetherKiri研究。把godot-cpp的静态库编译出来了，总共1000多个cpp文件——gcc参数不知道是什么，
乱填，我是参考ps aux的命令行参数，如果直接看scons或cmake的配置文件可能比较麻烦，暂时先这样，等后面编译失败了再看 ​​​​

AetherKiri研究。试试链接动态库（差不多最后的工作了），但有些符号未实现。按理来说动态库允许符号未实现，
但如果被godot加载会失败，所以我打算还是先解决这个问题。大概有300多处，因为我改了它源代码，我要逐个链接失败的地方看，
短时间解决不了，我觉得可能要一两个月时间，我可能会先研究别的

AetherKiri研究。Makefile编译版做好了，虽然我有点心急，有些地方可能还不是很完善（例如有些第三方库可能不需要，
但我还是引入了那些库的头文件）。不管了，以后再想办法去掉，有时间会放到gh上继续修改。目前只能编译ubuntu 25的版本，
其他版本还不能编译，如果想编译安卓版可能还要继续研究下去 ​​​
```

## Original README.md

<p align="center">
  <img src="apps/godot_app/assets/icon.png" width="112" alt="AetherKiri app icon">
</p>

<h1 align="center">AetherKiri</h1>

<p align="center">
  A Godot-hosted KiriKiri2 runtime with a C++ engine core and native mobile/desktop exports.
</p>

<p align="center">
  <a href="README.md">English</a> |
  <a href="README.zh-CN.md">简体中文</a>
</p>

<p align="center">
  <a href="https://github.com/AetherKiri/AetherKiri/actions/workflows/build.yml"><img alt="macOS Build" src="https://img.shields.io/github/actions/workflow/status/AetherKiri/AetherKiri/build.yml?branch=main&amp;job=Build%20macOS%20App&amp;label=macOS%20Build"></a>
  <a href="https://github.com/AetherKiri/AetherKiri/actions/workflows/build.yml"><img alt="iOS Build" src="https://img.shields.io/github/actions/workflow/status/AetherKiri/AetherKiri/build.yml?branch=main&amp;job=Build%20iOS%20App&amp;label=iOS%20Build"></a>
  <a href="https://github.com/AetherKiri/AetherKiri/actions/workflows/build.yml"><img alt="Android Build" src="https://img.shields.io/github/actions/workflow/status/AetherKiri/AetherKiri/build.yml?branch=main&amp;job=Build%20Android%20App&amp;label=Android%20Build"></a>
  <a href="https://github.com/AetherKiri/AetherKiri/actions/workflows/build.yml"><img alt="Web Build" src="https://img.shields.io/github/actions/workflow/status/AetherKiri/AetherKiri/build.yml?branch=main&amp;job=Build%20Web%20App&amp;label=Web%20Build"></a>
</p>

<p align="center">
  <a href="https://github.com/AetherKiri/AetherKiri/blob/main/LICENSE"><img alt="GitHub License" src="https://img.shields.io/github/license/AetherKiri/AetherKiri?logo=gnu&label=license"></a>
  <a href="https://github.com/AetherKiri/AetherKiri/commits/main"><img alt="GitHub Last Commit" src="https://img.shields.io/github/last-commit/AetherKiri/AetherKiri?logo=github"></a>
  <a href="https://github.com/AetherKiri/AetherKiri/issues"><img alt="GitHub Issues" src="https://img.shields.io/github/issues/AetherKiri/AetherKiri?logo=github"></a>
  <a href="https://github.com/AetherKiri/AetherKiri/pulls"><img alt="GitHub Pull Requests" src="https://img.shields.io/github/issues-pr/AetherKiri/AetherKiri?logo=github"></a>
  <a href="https://github.com/AetherKiri/AetherKiri"><img alt="GitHub Repository Size" src="https://img.shields.io/github/repo-size/AetherKiri/AetherKiri?logo=github"></a>
  <a href="https://github.com/AetherKiri/AetherKiri"><img alt="GitHub Top Language" src="https://img.shields.io/github/languages/top/AetherKiri/AetherKiri?logo=github"></a>
</p>

## Overview

AetherKiri runs KiriKiri2 content inside a Godot 4.7 application shell. The
runtime is split into a native C++17 engine core, a C ABI bridge, and a Godot
GDExtension host that owns the product UI, render resources, settings, export
presets, and platform packaging.

The default product renderer is **Godot Native**: engine frames are rendered
through Godot-owned `RenderingDevice` resources. **GPU Bridge** remains an
explicit compatibility and performance comparison backend for external native
GPU render targets imported by Godot. **Debug CPU** is available as a visible
diagnostic fallback only.

```text
Godot App Shell
  -> GDExtension Host
    -> C ABI Engine API
      -> C++ Engine Core
        -> KiriKiri Runtime / Plugins
```

## Highlights

- Godot 4.7 app shell with native GDExtension integration.
- C++17 KiriKiri2 runtime core with visual, audio, storage, VM, and plugin
  support.
- Export paths for macOS, iOS/iPadOS, Android, and Web.
- Runtime-selectable render backend with persisted settings.
- Bundled multilingual KAG3 demo that can be played from the library and
  deleted by the player.
- Probe scripts for smoke, render, interaction, performance, and manual repro
  sessions.
- Manual compatibility notes for tested titles in
  [`doc/verified_games.md`](doc/verified_games.md).
- GPL-3.0-or-later source distribution.

## Repository Layout

| Path | Purpose |
| --- | --- |
| `apps/godot_app/` | Godot project, scenes, settings UI, performance/log panel, icons, and export presets. |
| `bridge/godot_extension/` | Godot native host library entry points. |
| `bridge/engine_api/` | C ABI used by the host layer to drive the C++ engine. |
| `cpp/core/` | KiriKiri2 runtime, visual system, audio, storage, VM, and plugin support. |
| `cpp/plugins/` | Bundled native plugin implementations and compatibility stubs. |
| `packages/AetherInternal/` | Optional private E-mote package submodule; public builds work without it. |
| `demos/aetherkiri-kag3/` | Source tree for the built-in AetherKiri KAG3 demo. |
| `tests/profiles/` | Per-game probe profiles. Committed profiles must not contain machine-local game paths. |
| `tools/` | Developer and compatibility tools built outside iOS/Android targets. |
| `doc/development.md` | Full developer guide for architecture, file roles, build, testing, probes, and debugging. |
| `doc/diagnostics.md` | In-app debugger, one-command collection, bundle contract, and evidence-first investigation guide. |
| `doc/verified_games.md` | Manual list of games that have been smoke-tested with the current runtime. |

## Built-in Demo

The product package includes the multilingual AetherKiri KAG3 demo at
`apps/godot_app/builtin_demos/aetherkiri-kag3/data.xp3`. On first launch the
app atomically stages a writable copy under `user://builtin_games/` and adds it
to the normal game library, so it uses the same launch and play-time flow as an
imported game.

Deleting this library entry removes the staged archive and its local saves,
including the browser's separate persistent save directory, and records the
player's choice. Refreshing or upgrading the app does not restore the demo;
instead, it retries any unfinished cleanup. The signed seed remains part of
the application package, so deleting the writable copy cannot shrink the
installed app bundle.
The editable source and rebuild instructions are in
[`demos/aetherkiri-kag3/`](demos/aetherkiri-kag3/).

## Render Backends

| Backend | Role | Status |
| --- | --- | --- |
| Godot Native | Godot-owned GPU rendering path. | Default product path |
| GPU Bridge | External GPU render-target bridge for comparison and compatibility. | Optional backend |
| Debug CPU | RGBA readback/upload fallback. | Debugging only |

The Godot settings UI persists the selected backend and warns when changing it
while a game session is active, because render resources must be recreated.

## Icon and Assets

The app icon shown above is the same icon configured by the Godot project:

- App icon: `apps/godot_app/assets/icon.png`
- SVG source used by the Godot project: `apps/godot_app/assets/icon.svg`
- Export icon set: `apps/godot_app/assets/icons/`
iOS and Android export presets reference the generated PNG sizes under
`apps/godot_app/assets/icons/`, including App Store and launcher sizes.

## Runtime Platform Requirements

| Platform | Minimum version | Notes |
| --- | --- | --- |
| macOS | macOS 13.0 (Ventura) | The Godot app export is universal, but the current native build triplet is `arm64`; Intel support needs an `x86_64` native build. |
| iOS / iPadOS | iOS / iPadOS 16.0 | `arm64` devices; `arm64` and `x86_64` simulator builds are available for development. |
| Android | Android 7.0 (API 24) | The product export currently packages `arm64-v8a` only. |
| Web | No OS version floor | Requires a browser with WebAssembly SIMD, WebAssembly threads, and `SharedArrayBuffer`, served with cross-origin isolation (COOP/COEP). |
| Linux | Build from source | No official prebuilt product package; compile the `x86_64` export locally. |
| Windows | Build from source | No official prebuilt product package; compile the native targets locally. |

## Requirements

- CMake 3.28+
- Ninja
- vcpkg in `.devtools/vcpkg` or available through `VCPKG_ROOT`
- Godot at `/Applications/Godot.app` or `GODOT_BIN=/path/to/Godot`
- Xcode for macOS/iOS exports
- Android SDK/NDK for Android exports. The script uses
  `ANDROID_HOME`/`ANDROID_SDK_ROOT` when set, otherwise
  `$HOME/Library/Android/sdk`, and prefers an installed NDK 28.x release.
  Set `ANDROID_NDK_HOME` (or `ANDROID_NDK_VERSION`) to select a specific NDK;
  Android builds for the Godot 4.7 export template should use NDK 28.1.13356709.
- Emscripten/emsdk for Web exports, with `emcc`, `em++`, and `emar` on `PATH`.
- Godot Web GDExtension/dlink export templates installed as
  `web_dlink_debug.zip` and `web_dlink_release.zip`.
- Node.js and npm for the TypeScript/Vite local Web server.

### Linux

The Linux development environment is project-local. It keeps Godot, vcpkg,
vcpkg downloads/binaries, assembler tools, npm, and Godot XDG data in
`.aetherkiri-cache/` (override with `AETHERKIRI_CACHE_DIR`). Bootstrap it with:

```bash
./tools/setup_linux.sh
```

On Arch Linux, the bootstrap uses the system packages when available and can
place the tool-only `zip`, `nasm`, and `yasm` packages under the project cache
when it cannot elevate. Install the broader system prerequisites
once for the normal host-managed setup:

```bash
sudo pacman -S --needed base-devel cmake ninja pkgconf git curl unzip ccache nasm yasm
```

Delete `.aetherkiri-cache/` to reclaim all reusable local build caches and
tool downloads. The Linux build links `apps/godot_app/.godot` to this cache
root and recreates its cache target on the next build. Build outputs remain
under `out/`; remove a Linux configuration with
`./build.sh --clean linux debug` or `./build.sh --clean linux release`.

## Build

The public repository builds and runs CI without access to private packages.
Maintainers with access to the complete E-mote and native Live2D
implementations can initialize the optional package before building:

```bash
git submodule update --init packages/AetherInternal
```

CMake enables it automatically when present. Use
`-DAETHERKIRI_ENABLE_INTERNAL=OFF` to test the public fallback, or
`-DAETHERKIRI_INTERNAL_DIR=/absolute/path/to/AetherInternal` to use a separate
checkout. Trusted runs of the `Build` GitHub Actions workflow use the
`AETHERSECRET` repository secret as a read-only SSH key and initialize the
private submodule recursively. Fork and Dependabot pull requests cannot access
repository secrets, so those untrusted runs use the public fallback.

The internal package extends the existing public `motionplayer` and
`krkr2plugin` targets; it does not replace either target or copy their public
source trees. For E-mote, it registers a small versioned controller extension.
For native Live2D, it contributes the Cubism SDK, `.l2d` loader, motion player,
and renderer while the public repository keeps the script-compatible fallback
and generic GPU bridge. Both configurations run the same public tests. A
package/API mismatch stops configuration instead of silently building an
incompatible combination.

Common builds:

```bash
./build.sh macos debug
./build.sh macos release
./build.sh ios debug --simulator
./build.sh ios release
./build.sh android debug --abi=arm64-v8a
./build.sh android release --abi=arm64-v8a
./build.sh web debug
./build.sh web release
./build.sh linux debug
./build.sh linux release
```

The scripts build the native engine and Godot host library, stage them under
`apps/godot_app/bin/`, then run the matching Godot export preset when Godot is
available. Linux exports include the required vcpkg shared libraries beside
the executable, so their engine runtime does not depend on the local build
tree. Android is currently wired for `arm64-v8a`.

## Tagged Releases

Pushing a SemVer tag such as `0.3.0` or `0.3.0-beta.1` starts the
`Release` workflow. The tag, without an optional leading `v`, becomes the
version shown inside Aether and the Android `versionName`. The numeric SemVer
core becomes the iOS and macOS marketing version, while the GitHub run number
and attempt produce a monotonically increasing Apple build number and Android
`versionCode`.

The Apple jobs use the GitHub-hosted `macos-latest` image. Tagged iOS builds
fail early unless the selected Xcode contains the iOS 26 SDK or newer. Regular CI
continues to produce an unsigned IPA for validation. Tagged releases require
App Store signing for both Apple platforms. Set `AETHERID` to the registered
Bundle ID `com.liuyu.aether.aether` and configure these repository Actions
secrets:

- `IOS_DISTRIBUTION_CERTIFICATE_BASE64`: base64-encoded Apple Distribution
  `.p12` certificate and private key.
- `IOS_DISTRIBUTION_CERTIFICATE_PASSWORD`: password for that `.p12`.
- `IOS_PROVISIONING_PROFILE_BASE64`: base64-encoded App Store provisioning
  profile for `com.liuyu.aether.aether` and team `3JL7FE9XQT`.
- `MACOS_INSTALLER_CERTIFICATE_BASE64`: base64-encoded Mac Installer
  Distribution `.p12` certificate and private key.
- `MACOS_INSTALLER_CERTIFICATE_PASSWORD`: password for the Mac installer
  `.p12`.
- `MACOS_PROVISIONING_PROFILE_BASE64`: base64-encoded Mac App Store
  provisioning profile for `com.liuyu.aether.aether` and team `3JL7FE9XQT`.
- `APP_STORE_CONNECT_API_KEY_ID`: App Store Connect API key ID.
- `APP_STORE_CONNECT_API_ISSUER_ID`: App Store Connect issuer ID.
- `APP_STORE_CONNECT_API_PRIVATE_KEY_BASE64`: base64-encoded App Store
  Connect `AuthKey_*.p8` file.

The macOS App Store package is Apple Silicon-only, enables App Sandbox, and
allows read-write access to files selected by the user. The Release workflow
validates and uploads the signed iOS IPA and macOS installer package to App
Store Connect before publishing the GitHub Release. Apple processes accepted
uploads asynchronously; selecting the processed builds and submitting them to
App Review remains a separate App Store Connect operation. Missing or partial
signing and API credentials fail the tagged release instead of silently
publishing an unsigned store artifact.

## Run and Test Artifacts

Web builds produce an Emscripten GDExtension side module at
`apps/godot_app/bin/web/<debug|release>/aether_kiri_godot.wasm`, then export
the Godot Web app to `out/godot/web/<debug|release>/index.html` when the dlink
template is installed. The Web export is built for threaded, SIMD-enabled
WebAssembly, so the served app needs cross-origin isolation headers. Cloud
deployments import local games through the browser's file/directory picker and
mount the authorized `File`/`Blob` objects with on-demand Range reads instead
of copying multi-GB game packages into the Emscripten virtual filesystem.

### macOS

Build and launch the exported app:

```bash
./build.sh macos release
open out/godot/macos/release/AetherKiri.app
```

Run the debug build from a terminal to inspect logs:

```bash
./build.sh macos debug
out/godot/macos/debug/AetherKiri.app/Contents/MacOS/AetherKiri
```

Add a game through the app UI, or pass a local test game only for the current
run:

```bash
AETHERKIRI_GAME_PATH="/path/to/game" \
out/godot/macos/debug/AetherKiri.app/Contents/MacOS/AetherKiri
```

### iOS Simulator

Build the simulator export:

```bash
./build.sh ios debug --simulator
```

Open the generated Xcode project, or install the built app with `simctl` after
building it from Xcode:

```bash
xcrun simctl boot "iPad Pro 11-inch (M4)"
xcrun simctl install booted /path/to/AetherKiri.app
xcrun simctl launch booted com.example.aetherkiri
```

The bundle identifier depends on the export preset and signing configuration.

### iOS Device

Build the iOS export project:

```bash
./build.sh ios release
```

Then build and install with Xcode or command line tools:

```bash
xcodebuild \
  -project out/godot/ios/release/AetherKiri.xcodeproj \
  -scheme AetherKiri \
  -configuration Release \
  -destination 'generic/platform=iOS' \
  -allowProvisioningUpdates \
  build
```

After Xcode creates `AetherKiri.app`, install it to a paired device:

```bash
xcrun devicectl list devices
xcrun devicectl device install app \
  --device <device-identifier> \
  /path/to/AetherKiri.app
```

On iOS/iPadOS, copy games through the Files app into:

```text
On My iPhone/iPad -> AetherKiri -> Games
```

Return to AetherKiri and tap refresh.

### Android

Build the debug APK:

```bash
./build.sh android debug --abi=arm64-v8a
```

The APK is written to:

```text
out/godot/android/debug/Aether-debug.apk
```

Install and launch it on a connected device or emulator:

```bash
adb install -r out/godot/android/debug/Aether-debug.apk
adb shell monkey -p org.github.krkr2.aetherkiri \
  -c android.intent.category.LAUNCHER 1
```

Build the release APK:

```bash
./build.sh android release --abi=arm64-v8a
```

The release APK is written to:

```text
out/godot/android/release/Aether-release.apk
```

The release preset is intentionally unsigned until a project release keystore is
configured. Sign it before installing or distributing it:

```bash
apksigner sign --ks /path/to/release.keystore \
  out/godot/android/release/Aether-release.apk
```

On Android, import games through the app UI on platforms with file-system
access. On restricted devices, copy the game directory into the app's
documents/storage location and use refresh.

### Web

Activate Emscripten before building:

```bash
source /path/to/emsdk/emsdk_env.sh
./build.sh web debug
```

The export is written to:

```text
out/godot/web/debug/index.html
```

Run it from an HTTP server, not by opening the file directly:

```bash
npm install
npm run web:dev:debug
```

Vite serves the exported static files with the COOP/COEP headers required by
`SharedArrayBuffer` and Godot's threaded Web export. For a release export, run
`npm run web:dev:release`.

Cloud deployments do not configure server-side game paths. Users click Import
in the browser and authorize a local game directory or XP3 file. Imported game
files are mounted as read-only inputs; saves, game configuration, and other
runtime writes are persisted in the current site's IndexedDB-backed `/userfs`,
not written back to the user's original game directory. For local development
only, Vite can expose a read-only test game root:

```bash
AETHERKIRI_GAME_ROOT=/absolute/path/to/game \
AETHERKIRI_WEB_AUTO_START=1 \
npm run web:dev:release
```

Use `AETHERKIRI_GAME_ROOTS` with the platform path delimiter for multiple roots.
`AETHERKIRI_WEB_AUTO_START_INDEX=1` or `AETHERKIRI_WEB_AUTO_START_NAME=title`
selects a specific configured root. These environment variables are developer
shortcuts, not the product import path.

## Validation

Useful migration checks:

```bash
rg "F[l]utter|f[l]utter|A[N]GLE|Platform[ ]Graphics" README.md README.zh-CN.md apps bridge build CMakeLists.txt
rg "u[n]official-angle|l[i]bEGL|l[i]bGLESv2" CMakeLists.txt bridge cpp build vcpkg.json
./build.sh macos debug
./build.sh ios debug --simulator
./build.sh android debug --abi=arm64-v8a
./build.sh web debug
build/validate_godot_native.sh
build/validate_gpu_bridge.sh
```

Godot script checks:

```bash
/Applications/Godot.app/Contents/MacOS/Godot \
  --headless \
  --path apps/godot_app \
  --check-only \
  --quit
```

## Per-Game Probe Profiles

Probe scripts can be driven by `AETHERKIRI_TEST_CONFIG`. Keep committed profiles
generic; do not commit local absolute game paths. Use `AETHERKIRI_SMOKE_GAME` or
an untracked local profile for machine-specific paths.

Example smoke test:

```bash
AETHERKIRI_TEST_CONFIG="$PWD/tests/profiles/kr37s.json" \
AETHERKIRI_SMOKE_GAME="/path/to/game" \
/Applications/Godot.app/Contents/MacOS/Godot \
  --path apps/godot_app \
  --script res://scripts/smoke_test.gd
```

Example render/interaction probe:

```bash
AETHERKIRI_TEST_CONFIG="$PWD/tests/profiles/kr37s.json" \
AETHERKIRI_SMOKE_GAME="/path/to/game" \
/Applications/Godot.app/Contents/MacOS/Godot \
  --path apps/godot_app \
  --script res://scripts/step_render_probe.gd
```

Manual render probe for clicking through a repro:

```bash
AETHERKIRI_TEST_CONFIG="$PWD/tests/profiles/kr37s.json" \
AETHERKIRI_SMOKE_GAME="/path/to/game" \
/Applications/Godot.app/Contents/MacOS/Godot \
  --path apps/godot_app \
  --script res://scripts/manual_render_probe.gd
```

The manual probe forwards mouse, wheel, touch, and keyboard input to the game.
Press `F12` to save `/tmp/aetherkiri-manual-*.png`; press `Esc` to exit.

Profile fields:

- `game_path`: optional game directory or XP3 path. Keep blank in committed
  profiles unless the path is portable.
- `backend`: render backend, usually `Godot Native`.
- `surface_size`: engine render surface, for example `[1280, 720]`.
- `window_size`: probe window size.
- `coord_size`: coordinate space used by recorded click points.
- `startup_timeout_frames`, `warmup_frames`, `after_click_frames`,
  `measure_frames`: timing knobs.
- `clicks`: ordered interaction steps with `name`, `x`, `y`, and optional
  `after_frames`.
- `perf_input`: compatibility settings for `perf_input_probe.gd`.

Acceptance requires startup, rendering, input, menu operations, audio, save
paths, clean exit, and performance parity from Godot Native or GPU Bridge. Debug
CPU is only a diagnostic fallback.

## Documentation

- Developer guide: `doc/development.md`
- Plugin notes: `doc/krkr2_plugins.md`
- Tools: `tools/README.md`

## License

AetherKiri is distributed under GPL-3.0-or-later. See `LICENSE` for the full
license text. Third-party notices are preserved in `THIRD_PARTY_LICENSES.md`.
For Apple App Store distribution, `COPYING.iOS` records the limited additional
permission granted by approving copyright holders solely for the official
Aether iOS or macOS release, or a distributor they authorize in writing.
Third-party forks and derivative apps may not rely on that additional
permission. It does not grant rights in upstream or third-party material on
behalf of other copyright holders, or revoke rights already granted by the GPL.
