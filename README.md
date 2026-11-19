# Scan results — unranked

Actual scans of the simplified excerpts and their dependency context, **2026-11-13 UTC**. The order below is by tool, **not remediation priority**. No custom detection rules were added.

| Tool | Version | Scope | Observed result |
| --- | --- | --- | --- |
| Semgrep Community Edition | 1.185.0 | Go/Rust excerpts; registry `p/default`, `p/security-audit`, `p/rust` | 3 code findings; no scan errors |
| Gitleaks | 8.30.1 | Excerpt directory, default rules; no Git-history scan | 2 secret findings, values redacted |
| Trivy | 0.76.2 | Excerpts for secrets; dependency manifests/lockfiles for vulnerabilities | 2 dependency findings and 2 secret findings |
| OSV-Scanner | 2.7.1 | Dependency manifests/lockfiles; no call/reachability analysis | 2 affected dependency versions; 3 advisory records |

These counts overlap across tools and source copies. They are **not** a total number of independent problems. No application images or OS packages were scanned for this excerpt-only exercise.

## Semgrep

| Source location | Tool severity | Rule | Reported concern |
| --- | --- | --- | --- |
| `go.go:27–30` | WARNING | `problem-based-packs.insecure-transport.go-stdlib.bypass-tls-verification.bypass-tls-verification` | TLS certificate verification disabled |
| `go.go:52` | ERROR | `go.lang.security.audit.dangerous-exec-command.dangerous-exec-command` | Non-static input in a command-execution call |
| `rust.rs:46–48` | WARNING | `rust.lang.security.reqwest-accept-invalid.reqwest-accept-invalid` | Invalid TLS certificates accepted |

[Original text](raw/semgrep.txt) · [JSON](raw/semgrep.json) · [Log](raw/semgrep.log)

## Gitleaks

| Source location | Rule | Reported concern |
| --- | --- | --- |
| `go.go:40` | `github-pat` | GitHub-shaped personal access token in source |
| `rust.rs:25` | `github-pat` | GitHub-shaped personal access token in source |

Gitleaks did not assign a severity in this JSON output. No live validation was performed; the exercise credentials are synthetic.

[Original JSON](raw/gitleaks.json) · [Log](raw/gitleaks.log)

## Trivy

| Location / dependency | Tool severity | ID / rule | Reported concern | Fixed version reported |
| --- | --- | --- | --- | --- |
| `go/go.mod`: `github.com/golang-jwt/jwt/v4` 4.5.1 | HIGH | `CVE-2025-30204` | Excessive memory allocation during JWT header parsing | 4.5.2 |
| `rust/Cargo.lock`: `jsonwebtoken` 9.3.0 | MEDIUM | `CVE-2026-25537` | Type confusion in JWT claim validation; potential authorization bypass | 10.3.0 |


[Original JSON](raw/trivy.json) · [Log](raw/trivy.log)

## OSV-Scanner

| Dependency | Advisory IDs / aliases | Reported maximum score | Reported concern |
| --- | --- | --- | --- |
| `github.com/golang-jwt/jwt/v4` 4.5.1 | `GO-2025-3553`, `GHSA-mh63-6h87-95cp`, `CVE-2025-30204` | 8.7 | Excessive allocation when parsing malformed tokens |
| `jsonwebtoken` 9.3.0 | `GHSA-h395-gr6q-cpjc`, `CVE-2026-25537` | 5.5 | Incorrect handling of malformed temporal-claim types under certain validation settings |

The maximum scores and advisory groups are taken from OSV-Scanner's output. Its three raw advisory records include two identifiers for the Go issue. The omitted authentication middleware's types, limits, and validation settings are not established by this package-version scan.

[Original JSON](raw/osv-scanner.json) · [Log](raw/osv-scanner.log)

## Your answer

Return to [the question and blank top-five table](README.md). Supply your own priority order, reasons, and first actions.

Exact commands, timestamps, image identities, database information, and input/report hashes are in [scan-details.json](scan-details.json). Paths prefixed `/code` or `code/` map to `candidate/`; dependency paths map to `scan-inputs/`. All four runs exited 1 to signal findings, not execution errors. Future databases and rule registries may produce different results.
