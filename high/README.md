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
parameters file instead of duplicating the template here.

See `../docs/control-mapping.md` for detail on what's diverged and why.
