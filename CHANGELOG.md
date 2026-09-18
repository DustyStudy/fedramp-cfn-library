# Changelog

All notable changes to this repo are documented here. Format loosely
follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

Significant infrastructure changes to a live FedRAMP-authorized system
require going through FedRAMP's Significant Change Request (SCR) process
— see `docs/CONTINUOUS-MONITORING.md`. Keeping this changelog current is
good practice regardless of whether you're tracking against a live
authorization, since it mirrors the change-documentation discipline
FedRAMP expects.

## [Unreleased]

### Added
- Compliance documentation: Customer Responsibility Matrix
  (`docs/CUSTOMER-RESPONSIBILITY-MATRIX.md`), coverage gap analysis
  (`docs/COVERAGE-GAPS.md`), POA&M starter template
  (`docs/POAM-TEMPLATE.md`), and continuous monitoring mapping
  (`docs/CONTINUOUS-MONITORING.md`)

### Changed
- Refreshed all FedRAMP 20x / CR26 references against FedRAMP's finalized
  Consolidated Rules for 2026 (confirmed 2026-06-24 via
  [github.com/FedRAMP/2026-markdown](https://github.com/FedRAMP/2026-markdown),
  the current source of truth — `github.com/FedRAMP/docs` has been renamed
  to `docs-legacy` and is no longer current). The KSI catalog is now
  confirmed at 46 indicators across 10 families (was previously flagged as
  "unsettled" pending finalization): `fedramp-20x/README.md`,
  `docs/FEDRAMP-20X-CHEAT-SHEET.md`, `README.md`,
  `docs/control-mapping.md`. Noted that the `CNBC` KSI family from the
  pre-CR26 pilot structure no longer exists — its scope split into `CNA`
  (network boundary) and `SVC` (configuration/drift). Added a callout to
  `docs/CONTINUOUS-MONITORING.md` for CR26's new Collaborative Continuous
  Monitoring model (Ongoing Certification Reports every 3 months +
  Quarterly Reviews, required by 2027-01-01 for 20x). Flagged that
  `docs/control-mapping.md`'s existing numbered KSI IDs (e.g.
  `KSI-MLA-01`) predate CR26's finalized lettered IDs (e.g.
  `KSI-MLA-LET`) and haven't been re-verified 1:1 against the new catalog.

## 2026-08-23

### Added
- 11 additional templates: `account-baseline`, `ecr-hardened`,
  `ecs-fargate-hardened`, `eks-hardened`, `fips-vpc-endpoints`,
  `network-perimeter-vpc`, `org-governance`, `org-scp-boundary`,
  `rds-postgres-hardened`, `ssm-patching-hardened`, `waf-hardened`
- `docs/NIST-800-53-REV5-MATRIX.md` — control-ID-oriented mapping across
  all templates
- Hardened CI pipeline (`ci.yml`): Gitleaks secret scanning, cfn-lint,
  Checkov (blocking) + Trivy (reporting)

### Fixed
- KMS key policies on `ecs-fargate-hardened`, `network-perimeter-vpc`, and
  `ssm-patching-hardened` were missing the service-principal (or
  writer-role) grant needed for the encrypted resource to actually
  function — these are functional bugs, not just hardening gaps, and
  would have failed at deploy/runtime
- `waf-hardened` was missing WAF logging entirely — no log group, no KMS
  key, no `LoggingConfiguration` resource
- `fips-vpc-endpoints` defaulted to services with no genuine FIPS-suffixed
  VPC endpoint name (`secretsmanager`, `logs`), meaning the template
  silently created ordinary interface endpoints while being labeled FIPS.
  Corrected to only use `-fips` suffixes for the services that actually
  have one (`kms`, `ec2`, `sts`), with the others clearly labeled as
  standard endpoints
- `docs/NIST-800-53-REV5-MATRIX.md` contained three verified-inaccurate
  claims (S3 Object Lock enforcement, backup vault immutability, non-root
  container UID enforcement) that didn't match any template's actual
  logic — rewritten using only claims checked directly against the
  implementation

## Initial release

- Core templates: `org-cloudtrail`, `config-conformance-pack`,
  `guardduty-org`, `security-hub-org`, `iam-password-policy`
- `moderate/iam-access-control`, `moderate/logging-monitoring`,
  `moderate/network-boundary`, `moderate/data-protection`,
  `moderate/incident-response`
- `high/example-params` illustrating the parameter-override pattern
- `fedramp-20x/` KSI cross-reference
- Initial CI (cfn-lint, Checkov)
