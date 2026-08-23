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
