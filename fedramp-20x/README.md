# FedRAMP 20x

FedRAMP 20x is a fundamentally different assessment model from Rev5's
control baselines — authorizations are validated against a smaller set of
**Key Security Indicators (KSIs)**, many of which are meant to be verified
in a machine-readable way rather than through a traditional control
narrative.

**Status:** still actively evolving. Phase 2 pilot participants were
announced in December 2025, and FedRAMP has signaled 20x Low/Moderate wide
availability is targeted for early this year — but timelines have shifted
before. Before relying on anything in this folder, check the current
guidance at:

- https://www.fedramp.gov/updates/changelog
- https://www.fedramp.gov/docs/rev5/balance/intro/ (how 20x improvements
  are being folded into Rev5 in the meantime)
- https://github.com/FedRAMP (machine-readable FRMR docs and the public
  roadmap)

## Folders (by KSI category)

| Folder | KSI Category | Existing templates that already satisfy it |
|---|---|---|
| `ksi-cna/` | Cloud Native Architecture | `modules/eks-hardened/template.yaml`, `modules/ecs-fargate-hardened/template.yaml`, `modules/network-perimeter-vpc/template.yaml` |
| `ksi-iam/` | Identity and Access Management | `moderate/iam-access-control/access-control-baseline.yaml`, `modules/iam-password-policy.yaml`, `modules/org-scp-boundary/template.yaml` |
| `ksi-mla/` | Monitoring, Logging and Auditing | `modules/org-cloudtrail.yaml`, `modules/guardduty-org.yaml`, `modules/security-hub-org.yaml`, `moderate/logging-monitoring/cis-metric-alarms.yaml`, `modules/ecs-fargate-hardened/template.yaml` |
| `ksi-cnbc/` | Configuration and Network Boundary Controls | `modules/config-conformance-pack.yaml`, `moderate/network-boundary/vpc-flow-logs.yaml`, `moderate/network-boundary/default-security-group-lockdown.yaml`, `moderate/data-protection/s3-account-public-access-block.yaml`, `modules/fips-vpc-endpoints/template.yaml`, `modules/org-scp-boundary/template.yaml`, `modules/waf-hardened/template.yaml` |
| `ksi-svc/` | Service Configuration | `moderate/data-protection/s3-account-public-access-block.yaml`, `moderate/data-protection/kms-cmk-baseline.yaml`, `modules/account-baseline/template.yaml`, `modules/ecr-hardened/template.yaml`, `modules/rds-postgres-hardened/template.yaml`, `modules/ssm-patching-hardened/template.yaml` |
| `ksi-inr/` | Incident Response | `modules/guardduty-org.yaml`, `moderate/incident-response/incident-notifications.yaml` |

Many of `../modules/` templates already satisfy specific KSIs (see the
mapping table in each module's header comment and in
`../docs/control-mapping.md`) — check there before building something new.
This table is a starting point for which existing template to point to when
assembling KSI evidence; it is not a substitute for reading the actual KSI
definitions, since 20x's specific validation method for each indicator may
expect something more precise than "a relevant control exists."
