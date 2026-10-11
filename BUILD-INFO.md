# MoxChat 1.3.2 build information

- Source revision: `1e6ea7f6457306e9a12aa478f27397127caf6e39` in the main Mox source repository.
- Build completed (UTC): `2026-10-11T17:09:46Z`.
- Build entry: `MoxChat/build.sh` via the existing Lazycat release configuration.
- Packages: independently deployable Web ZIP and Lazycat LPK v2.
- Web files: 508; the ZIP, LPK content, and fresh Web export match file for file.
- Toolchain: Node.js 24.18.1, pnpm 11.14.0, wasm-bindgen 0.2.126; existing dependencies and Rust cache reused in an isolated source checkout.
- Rust build: local source and toolchain paths remapped to generic `/build` paths using `--remap-path-prefix`; generated Web assets rebuilt before packaging.
- Source state: built from the identified commit. The uncommitted iOS project configuration in the main workspace was excluded and left untouched.
- Validation: production export, ZIP integrity, LPK version and content, required workers, MLS/SQLite WASM format, cross-origin isolation in the LPK manifest, artifact checksums, and absence of private key material and local developer paths. These are build and package checks; installation on Lazycat and browser functionality were not acceptance-tested.

| Artifact | SHA-256 |
| --- | --- |
| `web/moxchat-web-1.3.2.zip` | `1597d701da863c1a1d6a1ca24638a30e8a8632765d70bac76809513881d74020` |
| `lpk/moxchat-1.3.2.lpk` | `62fd1b52bbb522707eb1edc69cd3394e6bc8640face5f6acf291f4fe83c60070` |

Each artifact directory contains its own `SHA256SUMS`. GitHub Releases provides a combined `SHA256SUMS` for the downloaded release assets. Verify files before installing or deploying them.
