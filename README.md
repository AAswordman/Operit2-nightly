# Operit2 Release and Preview Updates

This repository contains documentation for Operit2 release packages and byte-exact
incremental updates. The application uses two existing update channels: **release**
and **preview**. Both resolve versions and download assets from the main project:

- [Source code](https://github.com/AAswordman/Operit2)
- [Release and preview downloads](https://github.com/AAswordman/Operit2/releases)
- [Issues and support](https://github.com/AAswordman/Operit2/issues)
- [Package and incremental-update format](RELEASES.md)
- [GNU AGPL v3 license](LICENSE)

This repository does not define an additional application update channel. Its
existing repository name is retained; the updater and publishing tools use
`AAswordman/Operit2` for both release and preview distribution.

## Incremental updates

Packages identify product, platform, architecture, distribution and version.
Android APK, Windows ZIP/MSI, macOS distribution-specific PKG and Linux/CLI archive
updates use the same binary patch protocol. Each package has an authenticated
`.update.json` manifest and can have direct `.patch.gz` deltas from older exact
packages. Complete installation packages remain available on every release.

An exact base SHA-256 match is required. The reconstructed package must match the
original published bytes, including installer signatures, before the existing
platform installer is invoked. Android can read a monolithic installed APK;
desktop applications retain verified original packages for subsequent updates.
A baseline extraction or patch reconstruction/verification failure is reported
explicitly and makes subsequent attempts for that exact target use the full package.
The decision persists across restarts; newer targets and other package types are
evaluated independently. Cancelled requests and ordinary network interruptions can
retry the delta. Only a delta smaller than the full package is selected.
System installation permissions
and platform confirmations continue to apply.

## Source and licensing

Development and issue tracking belong in the main source repository. Release notes
identify the corresponding source revision. Operit2 is licensed under GNU AGPL v3;
third-party components retain their own licenses. No personal user data is published
here. This repository may contain documentation only while release builds are prepared.
