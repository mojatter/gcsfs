# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.3.0]

### Added

- `wfs.RenameFS` support via GCS server-side copy (CopierFrom) + Delete.
- `wfs.SyncWriterFile` support (no-op — GCS writes atomically on Close).
- CI: GitHub Actions with Go version matrix (`1.24`, `stable`),
  golangci-lint, govulncheck.
- Dependabot for weekly Go module and GitHub Actions updates.
- `CHANGELOG.md`.

### Changed

- Upgraded `wfs` dependency from v0.4.0 to v0.5.0.
- Upgraded `cloud.google.com/go/storage` from v1.33.0 to v1.60.0.
- Upgraded `google.golang.org/api` from v0.141.0 to v0.265.0.
- Removed `io2` dependency (replaced with internal `lazyWriteCloser`).
- Minimum Go version set to 1.24.

### Fixed

- Unchecked `Close` and `SetAttrSelection` errors flagged by errcheck.

## [0.2.0] and earlier

See the git log.

[Unreleased]: https://github.com/mojatter/gcsfs/compare/v0.3.0...HEAD
[0.3.0]: https://github.com/mojatter/gcsfs/compare/v0.2.0...v0.3.0
