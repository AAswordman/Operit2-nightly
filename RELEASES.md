# Release packages and incremental updates

Operit2 publishes both release and preview assets in
[the main repository Releases](https://github.com/AAswordman/Operit2/releases).
This documentation repository does not provide a third update channel.

## Versions

Stable tags use `v2.0.0` and GitHub's prerelease flag is false. Preview tags use
`v2.0.0-preview.1` and the prerelease flag is true. Existing rc/dev identifiers use
the preview channel. Stable installations select stable releases; preview
installations can select previews and subsequent stable versions, using SemVer.
Published packages are immutable: rebuilds require a new product version.

## Packages

```text
operit2-<product>-<platform>-<architecture>[-<distribution>]-<version>.<extension>
<package>.update.json
<package>.from-<base-sha256>.patch.gz
```

The update manifest uses schema 1 and contains `version`, `assetName`, `sha256`,
`size` and `patches`. Each patch declares `baseVersion`, `baseSha256`, `baseSize`,
`assetName`, `sha256` and `size`. Different architectures, macOS sandbox/unsandbox
distributions and Windows MSI/ZIP formats have independent base packages.

## Binary patch protocol

The gzip payload starts with the nine bytes `O2PATCH1` followed by a zero byte,
then little-endian u64 base and target sizes and their raw 32-byte SHA-256 hashes.
Instructions are END=0, COPY=1 followed by u64 offset and length, and ADD=2
followed by u64 length and literal bytes. Every generated delta is independently
reconstructed and only included when smaller than the complete package.

Clients authenticate the manifest using GitHub's asset digest, bind the target
hash to the full package digest, verify the exact base and patch, and verify the
reconstructed package before installation. They preserve existing native
installation and permission flows. Desktop updates without a retained original
package use a full transfer; this seeds a base for later updates.

## Tooling and publication

The source repository contains `tools/updates/patch.py`, `tools/updates/prepare.py`,
`tools/release/incremental.py` and `tools/release/publish_dist.py`. Publication uses
the authenticated GitHub CLI. The collector retains stable and preview histories
independently and can produce direct preview-to-release patches. A first release
has an empty patch list when no older package exists.

The publisher uploads a hidden draft, validates all GitHub asset digests and the
complete asset set, then publishes it. Existing public releases cannot be overwritten.
Signing remains part of each platform's normal package build.

## Transfer priority and retry policy

Each update selects one direct patch from an exact base to the target version.
The manifest's other base patches are alternative routes and are not downloaded.
A matching patch is used only when its size is strictly smaller than the full
package; equal or larger transfers use the full package.

If baseline extraction or patch reconstruction/verification fails, the current
attempt reports the error. Subsequent attempts for the same target version and
package family download the full package directly. This local decision persists
across application restarts and cache cleanup; newer target versions and other
architectures or installer formats are evaluated independently. User cancellation
and ordinary network interruption do not disable delta retries. Full transfers
always retain package digest and container verification.

Report problems in [the main repository](https://github.com/AAswordman/Operit2/issues)
with the full version, platform, package format and relevant logs.
