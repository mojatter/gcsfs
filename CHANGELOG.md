# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.4.0]

### Fixed

- Upgraded `go.opentelemetry.io/otel/sdk` to v1.45.0, fixing
  [GO-2026-6505](https://pkg.go.dev/vuln/GO-2026-6505).
- Upgraded `golang.org/x/net` to v0.60.0, fixing
  [GO-2026-6603](https://pkg.go.dev/vuln/GO-2026-6603),
  [GO-2026-6610](https://pkg.go.dev/vuln/GO-2026-6610),
  [GO-2026-6611](https://pkg.go.dev/vuln/GO-2026-6611),
  [GO-2026-6612](https://pkg.go.dev/vuln/GO-2026-6612) and
  [GO-2026-6617](https://pkg.go.dev/vuln/GO-2026-6617) (HTTP/2). The
  same advisories also affect the standard library `net/http`; build
  with Go 1.26.9 / 1.27.2 or later to cover that side too.

### Changed

- Minimum Go version bumped to 1.26 (required by
  `google.golang.org/api` and `golang.org/x/net`).
- Upgraded `github.com/mojatter/wfs` to v0.7.1.
- Upgraded `cloud.google.com/go/storage` to v1.69.0 and
  `google.golang.org/api` to v0.300.0.
- Routine dependency updates via Dependabot.

## [0.3.1]

### Fixed

- Upgraded `google.golang.org/grpc` to v1.82.1, fixing
  [GO-2026-6061](https://pkg.go.dev/vuln/GO-2026-6061) (xDS RBAC
  authorization engine and HTTP/2 transport server vulnerabilities).

### Changed

- Minimum Go version bumped to 1.25 (required by
  `cloud.google.com/go/storage` v1.62.x).
- Routine dependency updates via Dependabot.

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

[Unreleased]: https://github.com/mojatter/gcsfs/compare/v0.4.0...HEAD
[0.4.0]: https://github.com/mojatter/gcsfs/compare/v0.3.1...v0.4.0
[0.3.1]: https://github.com/mojatter/gcsfs/compare/v0.3.0...v0.3.1
[0.3.0]: https://github.com/mojatter/gcsfs/compare/v0.2.0...v0.3.0
