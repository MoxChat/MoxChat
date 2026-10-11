# MoxChat 1.3.2 build information

[中文构建信息](./BUILD-INFO.zh-CN.md)

## Web and Lazycat

- Source revision: `1e6ea7f6457306e9a12aa478f27397127caf6e39` in the main Mox source repository.
- Build completed (UTC): `2026-10-11T17:09:46Z`.
- Build entry: `MoxChat/build.sh` via the existing Lazycat release configuration.
- Packages: independently deployable Web ZIP and Lazycat LPK v2.
- Web files: 508; the ZIP, LPK content, and fresh Web export match file for file.
- Toolchain: Node.js 24.18.1, pnpm 11.14.0, wasm-bindgen 0.2.126; existing dependencies and Rust cache reused in an isolated source checkout.
- Rust build: local source and toolchain paths remapped to generic `/build` paths using `--remap-path-prefix`; generated Web assets rebuilt before packaging.
- Source state: built from the identified commit. The uncommitted iOS project configuration in the main workspace was excluded and left untouched.
- Validation: production export, ZIP integrity, LPK version and content, required workers, MLS/SQLite WASM format, cross-origin isolation in the LPK manifest, artifact checksums, and absence of private key material and local developer paths. These are build and package checks; installation on Lazycat and browser functionality were not acceptance-tested.

## Android

- Base source revision: `1e6ea7f6457306e9a12aa478f27397127caf6e39`; the isolated Android build also applies the two-line note-editor correction described below. The original workspace remains unchanged.
- Build/package verification completed (UTC): `2026-10-11T18:24:51Z`.
- Build entry: `node MoxChat/scripts/build-android-release.mjs` (`assembleRelease`); the JavaScript bundle is embedded and does not require a Metro development server.
- Package: `com.ponzs.moxchat`, version `1.3.2`, version code `2`, target SDK `36`.
- Requirements: Android **8.0 or later** (minimum SDK `26`). Included ABIs: `arm64-v8a`, `armeabi-v7a`, `x86`, and `x86_64`.
- Signing: release build signed with the project's existing **Android debug/test key**, not a production distribution key. The filename explicitly marks it as a test package.
- Toolchain: Temurin Java 17.0.20.1, Gradle 8.14.3, Android SDK 36, app Build Tools 36.0.0, NDK 27.1.12297006, and CMake 3.22.1.
- Build-only compatibility settings: minimum SDK raised to 26 because the current WebGPU dependency uses Android 26 hardware-buffer APIs; three dependencies without an explicit NDK version aligned with the project's NDK 27.1. Gradle workers and Ninja jobs limited to two on the 16 GB build host.
- Android source correction: in `MoxNoteEditorModule.kt`, the override and its `super` call use Android's `onSelectionChanged` instead of the misspelled `onNoteSelectionNoteChanged`. Patch SHA-256: `5d75b47a231de1a82a5cefafff7b0a71a17674424db1adf47242d5ab81de0b88`. The patch and full build report are retained with the local APK; this correction was applied only to the isolated Android build copy.
- Validation: successful release build and its required lint checks, APK ZIP integrity, APK Signature Scheme v2 verification, package/version/minimum SDK, all four ABIs, embedded JavaScript bundle, artifact checksum, and absence of keystores, environment files, source maps, and original workspace paths in native libraries. No physical Android device or emulator installation/functionality was acceptance-tested.
- Installation: verify the checksum, download the APK to an Android 8.0+ device, open it, and allow installation from that source when prompted. Updating an existing installation requires the same signing key.
- Distribution: the APK exceeds GitHub's repository file size limit and is stored as a [Release asset](https://github.com/MoxChat/MoxChat/releases/download/v1.3.2/moxchat-android-1.3.2-test.apk); `android/SHA256SUMS` records its checksum.

| Artifact | SHA-256 |
| --- | --- |
| `web/moxchat-web-1.3.2.zip` | `1597d701da863c1a1d6a1ca24638a30e8a8632765d70bac76809513881d74020` |
| `lpk/moxchat-1.3.2.lpk` | `62fd1b52bbb522707eb1edc69cd3394e6bc8640face5f6acf291f4fe83c60070` |
| `moxchat-android-1.3.2-test.apk` (Release asset) | `c1c5d7946854c302ae5b06cf946867c2719b0741f1a0fbcbace9f907be595723` |

Each package directory contains its own `SHA256SUMS`. GitHub Releases provides a combined `SHA256SUMS` for the downloaded release assets. Verify files before installing or deploying them.
