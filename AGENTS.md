# MoxChat Release Repository Rules

1. This repository contains distribution documentation and release artifacts for Android APKs, Web packages, Lazycat LPKs, and macOS, Linux, and Windows desktop clients only.
2. Client source, product specifications, and build tools are maintained in the main Mox source repository. Do not add application source or dependencies here.
3. Never publish signing keys, keystores, certificates with private keys, real `.env` values, credentials, user databases, or local build caches.
4. Publish only artifacts built from an identified source revision. Record the version, supported platform, source revision, build time, installation instructions, and SHA-256 checksums with each release.
5. Keep `README.md` and `README.zh-CN.md` synchronized. Keep `BUILD-INFO.md` in English and its Chinese version in `BUILD-INFO.zh-CN.md`, with matching release facts and reciprocal language links. Do not advertise unpublished artifacts as available.
6. Store large client packages as GitHub Release assets rather than committing them when they exceed GitHub's repository file size limit.
7. Organize artifacts under `web/`, `android/`, `lpk/`, `macos/`, `linux/`, and `windows/`; retain empty directories with `.gitkeep`. Keep the bilingual READMEs synchronized with the repository's purpose, platform and package descriptions, artifact directories, publication availability, and the iOS App Store and online Web client links.
