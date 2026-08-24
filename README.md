# FedRAMP CloudFormation Library

Reusable AWS CloudFormation templates that implement common controls and
security patterns for organizations pursuing **FedRAMP Moderate**, **FedRAMP
High**, or **FedRAMP 20x** authorization.

## ⚠️ Disclaimer

These templates support the *implementation* of security controls. They do
**not**, by themselves, constitute a FedRAMP authorization. An Authority to
Operate (ATO) requires an agency sponsor, a 3PAO (Third Party Assessment
Organization) assessment, and an approved System Security Plan (SSP). Treat
this repo as a starting point for your control implementation evidence — not
a substitute for the assessment process.

Templates are provided as-is with no warranty. Review every parameter,
resource, and IAM policy before deploying to any environment, and validate
against your organization's current SSP and your 3PAO's expectations.

## Why three separate tracks?

- **`moderate/`** and **`high/`** map to the NIST SP 800-53 Rev5 control
  baselines. High reuses most of Moderate's templates with tighter
  parameters (longer log retention, stricter crypto, broader MFA
  enforcement) rather than duplicating logic.
- **`fedramp-20x/`** is *not* a control baseline. FedRAMP 20x authorizations
  are validated against a smaller set of machine-readable **Key Security
  Indicators (KSIs)** — a fundamentally different assessment model that is
  still being piloted (Phase 2 pilot participants were announced in December
  2025; wide availability of 20x Low/Moderate is targeted for early this
  year). Templates here are organized by KSI category instead of control
  family, and this track will change as FedRAMP finalizes 20x guidance.

## Structure

```
moderate/            Rev5 Moderate baseline, by control family
high/                Rev5 High baseline (supersets moderate, tighter params)
fedramp-20x/         KSI-based templates (CNA, IAM, MLA, CNBC, SVC, INR)
modules/             Shared nested-stack building blocks used across tracks
docs/                Control-to-template cross-reference
```

## Templates

| Template | What it does |
|---|---|
| `org-cloudtrail` | Organization-wide CloudTrail, KMS-encrypted, with a dedicated access-log bucket |
| `config-conformance-pack` | AWS Config recorder + delivery channel + FedRAMP Moderate conformance pack |
| `guardduty-org` | GuardDuty with organization auto-enrollment, findings routed to SNS |
| `security-hub-org` | Security Hub with default standards + organization auto-enrollment |
| `iam-password-policy` | Account-wide IAM password policy |
| `account-baseline` | EBS default encryption, S3 account public access block, optional default-VPC/SG lockdown |
| `ecr-hardened` | KMS-encrypted ECR repository, tag immutability, scan-on-push |
| `ecs-fargate-hardened` | ECS cluster with Container Insights and KMS-encrypted logging (incl. ECS Exec) |
| `eks-hardened` | EKS cluster with KMS secrets envelope encryption, full control-plane logging, private-only endpoint |
| `fips-vpc-endpoints` | VPC interface endpoints — see the template's comments for which services genuinely have FIPS-suffixed endpoints and which don't |
| `network-perimeter-vpc` | 3-tier VPC with KMS-encrypted Flow Logs and a locked-down default security group |
| `org-governance` | Workload-perimeter SCP, AI-services opt-out policy, centralized backup policy |
| `org-scp-boundary` | Region-lock SCP, security-service protection, insecure-transport deny |
| `rds-postgres-hardened` | Multi-AZ PostgreSQL with enforced TLS, KMS encryption, managed master password |
| `ssm-patching-hardened` | Automated patch baseline, weekly maintenance window, KMS-encrypted patch logs |
| `waf-hardened` | Regional WAFv2 with AWS-managed rule groups, rate limiting, KMS-encrypted logging |
| `moderate/iam-access-control` | Access Analyzer, permission boundary, enforced-MFA group, root usage alerting |
| `moderate/logging-monitoring` | 14 CIS/Security Hub CloudWatch metric-filter + alarm pairs |
| `moderate/network-boundary` | VPC Flow Logs, default security group lockdown |
| `moderate/data-protection` | Account-wide S3 Public Access Block, reusable general-purpose KMS CMK |
| `moderate/incident-response` | Aggregated SNS topic for high-severity GuardDuty/Security Hub findings |

See `docs/control-mapping.md` for the NIST 800-53/20x KSI mapping per
template, and `docs/NIST-800-53-REV5-MATRIX.md` for the same information
organized by control ID instead.

## Compliance documentation beyond control mapping

Passing a FedRAMP audit takes more than deployed infrastructure. These
docs are aimed at that gap directly:

- **`docs/CUSTOMER-RESPONSIBILITY-MATRIX.md`** — what AWS already covers,
  what this repo automates, what's still a manual process
- **`docs/COVERAGE-GAPS.md`** — control families and requirements this
  repo genuinely cannot address (personnel security, training, the SSP
  itself, tested IR/contingency plans, and more), stated plainly rather
  than left implicit
- **`docs/CONTINUOUS-MONITORING.md`** — how this repo's templates feed
  FedRAMP's monthly/annual ConMon deliverables, and what ConMon requires
  that nothing here automates
- **`docs/FEDRAMP-20X-CHEAT-SHEET.md`** — a plain-language explainer of
  FedRAMP 20x and the 2026 Consolidated Rules (CR26) overhaul, for anyone
  who's heard people worrying about it and wants the short version
- **`docs/POAM-TEMPLATE.md`** — a starting point for tracking findings;
  get FedRAMP's official POA&M workbook for actual submissions
- **`CHANGELOG.md`** — change history, in the spirit of the documentation
  discipline FedRAMP's Significant Change Request process expects

## Security scanning

Every push and PR to `main` runs automatically via GitHub Actions
(`.github/workflows/ci.yml`), in three jobs:

- **Gitleaks** — secret/credential scanning
- **cfn-lint** — CloudFormation syntax validation and AWS best practices
- **Checkov** (blocking) and **Trivy** (reporting to the Security tab) —
  two independent security/compliance scanners against the templates
  themselves

Check the **Actions** tab on GitHub after your first push — new templates
sometimes get flagged for things that are intentional design choices in a
security baseline (for example, the permission boundary's `NotAction`
wildcard is deliberate, not an oversight). Where a finding is an accepted
risk rather than a bug, add a `checkov: skip:` metadata block above the
resource so the justification travels with the code — see
`CONTRIBUTING.md` for the pattern.

## Getting started

1. Start with `modules/` — these are the foundational building blocks
   (organization CloudTrail, AWS Config, GuardDuty, Security Hub, IAM
   password policy) that nearly every control family in Moderate, High, and
   every KSI category in 20x depends on.
2. Pick your target (`moderate/`, `high/`, or `fedramp-20x/`) and deploy the
   relevant nested stacks, referencing the modules above.
3. Check `docs/control-mapping.md` to see which NIST 800-53 control IDs (or
   KSI IDs) each template addresses, and use it to build your control
   implementation evidence for your SSP.

## Contributing

See `CONTRIBUTING.md`. PRs that add control mapping documentation alongside
new templates are especially welcome.

## License

Apache License 2.0 — see `LICENSE`.
