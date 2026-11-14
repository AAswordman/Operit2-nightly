# Release artifacts

This repository is the dedicated nightly distribution channel for
[Operit2](https://github.com/AAswordman/Operit2).

## Versions and source

Tags use `v<major>.<minor>.<patch>-nightly.<N>`. Each published snapshot is a
GitHub prerelease. The release notes must link the source commit and identify the
builds used to produce its packages. Published package bytes are immutable: a
rebuild uses a new nightly number.

## Packages

Package filenames identify product, platform, architecture, distribution when
applicable, and full release version:

```text
operit2-<product>-<platform>-<architecture>[-<distribution>]-<version>.<extension>
```

Full packages remain downloadable from each release. Incremental updates are
additional transfer artifacts, not replacements for system installers.

## Incremental metadata

For a full package named `<package>`, the release can provide:

- `<package>.update.json`: schema version, target version, exact package name,
  target byte size, target SHA-256, and supported base packages.
- `<package>.from-<base-sha256>.patch.gz`: a compressed COPY/ADD stream for one
  exact base package. It includes base and target sizes and SHA-256 values.

The client authenticates the manifest with the SHA-256 digest returned by the
GitHub Releases API, verifies the selected patch, reconstructs the complete
package, and checks the result before handing it to the platform installer.
Different architectures and macOS distributions have different packages and
cannot share patches.

Android can read a monolithic installed APK as a base. Desktop installations use
a retained, previously verified package. A first desktop installation without a
retained package uses a full transfer. An invalid or interrupted selected patch
reports an error; it does not silently start another download.

## Reporting problems

Open issues in the [main source repository](https://github.com/AAswordman/Operit2/issues).
Include the full version, platform, package type, and relevant logs. Do not attach
tokens, signing keys, personal conversations, or other private user data.
