# Example High-tier parameter overrides

These are illustrative examples of the "reuse the Moderate template, override
the parameters" pattern described in `../README.md` — not asserted FedRAMP
High requirements. Retention windows and similar numeric thresholds are set
by your agency's SSP, not by a single universal FedRAMP figure, so treat the
values here as a starting point to adapt, not a target to copy verbatim.

## Usage

Deploy the Moderate template with the High parameter file instead of the
default parameters, e.g.:

```
aws cloudformation deploy \
  --template-file ../../modules/org-cloudtrail.yaml \
  --stack-name org-cloudtrail-high \
  --parameter-overrides file://org-cloudtrail.high.json \
  --capabilities CAPABILITY_NAMED_IAM
```

## Files

- `org-cloudtrail.high.json` — extends CloudWatch Logs retention (365 → 1096
  days / ~3 years) and S3 archive retention (2555 days / ~7 years, same as
  Moderate's default — S3 was already generous, so this file mainly bumps
  the CloudWatch side). Adjust both figures to match what your SSP commits
  to for High.
