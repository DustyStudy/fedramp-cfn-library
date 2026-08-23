# FedRAMP Moderate

Templates here target the NIST SP 800-53 Rev5 **Moderate** baseline (~325
controls). Start with `../modules/` for the shared account-level baseline
(CloudTrail, Config, GuardDuty, Security Hub, IAM password policy) — those
underpin nearly every control family below.

| Folder | Control Family | Status |
|---|---|---|
| `logging-monitoring/` | AU | Uses `modules/org-cloudtrail.yaml` as its base |
| `iam-access-control/` | AC, IA | Planned |
| `network-boundary/` | SC | Planned |
| `data-protection/` | SC-13, SC-28, MP | Planned |
| `incident-response/` | IR | Planned |

See `../docs/control-mapping.md` for template-to-control detail.
