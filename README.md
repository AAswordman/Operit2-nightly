# Operit2 Nightly Releases

Operit2 的 nightly 安装包和增量更新发布仓库。

This repository distributes development builds of **Operit2**, an open-source,
cross-platform AI assistant. It is maintained alongside the main project; it is
not a general-purpose file host.

- **Source code / 源码**: [Operit2](https://github.com/AAswordman/Operit2)
- **Nightly downloads / 下载**: [Releases](https://github.com/AAswordman/Operit2-nightly/releases)
- **Issues / 问题反馈**: [Main project issues](https://github.com/AAswordman/Operit2/issues)
- **Release format / 发布格式**: [RELEASES.md](RELEASES.md)
- **License / 许可证**: [GNU AGPL v3](LICENSE)

## What is published here?

Nightly builds are development snapshots and may contain unfinished changes.
Release notes identify the source revision used to build each snapshot. The
source repository remains the place for development, review, and issue tracking.

The planned desktop and mobile release formats include Android APKs, Windows
portable ZIPs and MSI installers, macOS distribution-specific packages, Linux
archives, and platform-specific CLI packages. A release may include only a subset
of these targets. Download the package matching your platform and distribution.

Nightly versions use `major.minor.patch-nightly.N` and are kept separate from the
main project's stable and preview releases. This repository may initially have
no downloadable release while the first build is being prepared.

## Incremental updates

The updater can use an incremental patch only when the exact installed or
retained package matches the patch's base SHA-256. Patches reconstruct the
original complete installer, including its original signature. The reconstructed
package must pass size and SHA-256 checks before installation.

Each package's update manifest and patch assets are documented in
[RELEASES.md](RELEASES.md). Platform installers still control installation and any
required system confirmation. User data is not distributed in this repository.

## Source availability

Operit2 source is available under the GNU Affero General Public License v3 in the
[source repository](https://github.com/AAswordman/Operit2). Release notes
link the exact corresponding source revision. Third-party components retain
their respective licenses and notices in the source tree and distributions.
