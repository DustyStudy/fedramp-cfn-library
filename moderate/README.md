# FedRAMP Moderate

Templates here target the NIST SP 800-53 Rev5 **Moderate** baseline (~325
controls). Start with `../modules/` for the shared account-level baseline
(CloudTrail, Config, GuardDuty, Security Hub, IAM password policy) — those
underpin nearly every control family below.

| Folder | Control Family | Status |
|---|---|---|
| `logging-monitoring/` | AU | `cis-metric-alarms.yaml` — 14 CIS/Security Hub CloudWatch metric-filter + alarm pairs (root usage, unauthorized API calls, console sign-in without MFA, IAM/CloudTrail/Config/S3/security-group/NACL/gateway/route-table/VPC changes), built on `modules/org-cloudtrail.yaml`'s log group |
| `iam-access-control/` | AC, IA | `access-control-baseline.yaml` — Access Analyzer (external + unused access), permission boundary, enforced-MFA group, root usage alerting |
| `network-boundary/` | SC | Planned |
| `data-protection/` | SC-13, SC-28, MP | Planned |
| `incident-response/` | IR | Planned |

See `../docs/control-mapping.md` for template-to-control detail.
