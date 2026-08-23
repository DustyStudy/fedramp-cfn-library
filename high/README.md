# FedRAMP High

Reuses the `moderate/` templates and folder structure with tighter
parameters rather than duplicating logic. Only add a template here if it
diverges *structurally* from its Moderate counterpart — for example:

- FIPS 140-3 validated cryptographic endpoint enforcement
- Extended audit log retention (High commonly expects longer retention
  windows than Moderate — confirm current figures against your SSP)
- Additional audit event types or more restrictive network segmentation

Where a Moderate template only needs different **parameter values** (e.g.
`LogRetentionDays`), deploy the same Moderate template with a High
parameters file instead of duplicating the template here. See
`example-params/` for illustrative examples of this pattern — the exact
figures in those files are starting points, not universal FedRAMP High
requirements; retention windows and other numeric thresholds vary by
agency SSP, so confirm them against yours before using them as-is.

## What's here

| Folder | Status |
|---|---|
| `account-baseline/` | Standalone template with a dedicated High-tier KMS key and a 16-character minimum password length baked in |
| `org-governance/` | Standalone template with a 1095-day (3-year) backup retention baked in |
| `org-scp-boundary/` | Standalone template with the High policy name baked in |
| `example-params/` | Illustrative parameter overrides — see the caveat above |

**Note:** these three templates duplicate their `modules/` counterpart's
logic inline rather than referencing it via a nested stack — see the
same note in `../moderate/README.md` for why that matters if you change
the shared logic later.

See `../docs/control-mapping.md` for detail on what's diverged and why.
