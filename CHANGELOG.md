# Changelog

All notable changes to this repo are documented here. Format loosely
follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

Significant infrastructure changes to a live FedRAMP-authorized system
require notifying agencies under FedRAMP's Significant Change
Notification (SCN) rules — see `docs/CONTINUOUS-MONITORING.md`. Keeping this changelog current is
good practice regardless of whether you're tracking against a live
authorization, since it mirrors the change-documentation discipline
FedRAMP expects.

## [Unreleased]

### Security
- `moderate/org-scp-boundary`: the Moderate SCP was missing most of the deny
  list the High tier and `modules/org-scp-boundary` already carry. Member
  accounts could leave the Organization (which detaches every SCP), turn off
  S3 Block Public Access or default EBS encryption, and disassociate GuardDuty
  or delete KMS keys. It now matches the shared module.
- All three SCP variants also deny the current-name GuardDuty and Security
  Hub administrator-disassociation actions (`...FromAdministratorAccount`,
  distinct IAM actions from the legacy `...FromMasterAccount`), member
  deletion / stop-monitoring, `securityhub:BatchDisableStandards`, and
  `cloudtrail:PutEventSelectors`.
- `modules/guardduty-org`, `moderate/incident-response`,
  `moderate/logging-monitoring`, `moderate/iam-access-control`: SNS topics
  moved off the AWS-managed `alias/aws/sns` key, which EventBridge and
  CloudWatch cannot publish to (notifications were silently dropped), onto
  customer-managed keys. Topic policies now carry `aws:SourceArn` /
  `aws:SourceAccount` conditions.
- `moderate/iam-access-control`: the root-usage rule now also matches root
  console sign-ins and skips service-initiated events.
- `modules/org-cloudtrail`: the CloudWatch Logs statement in the key policy
  now grants the documented action set and is limited to this trail's log
  group via the encryption context.
- `modules/config-conformance-pack`: Config bucket-policy statements are
  pinned to this account (`aws:SourceAccount`).
- `modules/ssm-patching-hardened`: TLS-only bucket policy on patch logs.
- `modules/waf-hardened`: WAF logging redacts the `authorization` and
  `cookie` headers; `Scope` is restricted to `REGIONAL` / `CLOUDFRONT`.
- `modules/eks-hardened`: control-plane logs go to a template-managed,
  KMS-encrypted log group with bounded retention (new `LogRetentionDays`,
  default 365). Previously EKS auto-created an unencrypted, never-expiring
  group.
- `modules/rds-postgres-hardened`, `modules/fips-vpc-endpoints`: security
  groups no longer get CloudFormation's implicit allow-all egress rule.

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
  (network boundary) and `SVC` (configuration/drift).
- Remapped every template's 20x KSI reference (`docs/control-mapping.md` and
  the header comments in 12 templates) from the pilot-era numbered IDs
  (e.g. `KSI-MLA-01`) to CR26's lettered IDs (e.g. `KSI-MLA-LET`). This is a
  judgment-based mapping against the official indicator text, not an
  assessor-validated one. GuardDuty and incident-notification templates no
  longer cite `KSI-INR-*`, since CR26's INR family covers after-action and
  procedure reviews rather than detection.
- Removed the empty `fedramp-20x/ksi-cnbc/` placeholder; the `CNBC` KSI
  family no longer exists in CR26.
- Rewrote `docs/CONTINUOUS-MONITORING.md` for CR26: Collaborative
  Continuous Monitoring, Vulnerability Detection and Response, and
  Significant Change Notification, with applicability dates. Updated
  `docs/COVERAGE-GAPS.md`, `README.md`, and `CONTRIBUTING.md` to match.

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
