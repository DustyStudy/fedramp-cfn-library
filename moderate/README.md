# FedRAMP Moderate

Templates here target the NIST SP 800-53 Rev5 **Moderate** baseline (~325
controls). Start with `../modules/` for the shared account-level baseline
(CloudTrail, Config, GuardDuty, Security Hub, IAM password policy) — those
underpin nearly every control family below.

| Folder | Control Family | Status |
|---|---|---|
| `logging-monitoring/` | AU | `cis-metric-alarms.yaml` — 14 CIS/Security Hub CloudWatch metric-filter + alarm pairs (root usage, unauthorized API calls, console sign-in without MFA, IAM/CloudTrail/Config/S3/security-group/NACL/gateway/route-table/VPC changes), built on `modules/org-cloudtrail.yaml`'s log group |
| `iam-access-control/` | AC, IA | `access-control-baseline.yaml` — Access Analyzer (external + unused access), permission boundary, enforced-MFA group, root usage alerting |
| `network-boundary/` | SC | `vpc-flow-logs.yaml` — VPC Flow Logs to encrypted S3; `default-security-group-lockdown.yaml` — strips all rules from the default SG |
| `data-protection/` | SC-13, SC-28, MP | `s3-account-public-access-block.yaml` — account-wide S3 Public Access Block; `kms-cmk-baseline.yaml` — reusable customer-managed key for encryption at rest |
| `incident-response/` | IR | `incident-notifications.yaml` — aggregated SNS topic for high-severity GuardDuty/Security Hub findings |
| `account-baseline/` | AC-2, IA-5, MP-2 | Standalone template with Moderate password-policy values baked in (duplicates `modules/account-baseline` logic rather than referencing it as a nested stack) |
| `org-governance/` | AC-2, AC-4, CP-9 | Standalone template with a 365-day backup retention baked in (same duplication caveat as above) |
| `org-scp-boundary/` | AC-3, AC-4, SC-7 | Standalone template with the Moderate policy name baked in (same duplication caveat as above) |

**Note:** unlike the Terraform version of this library, where High/Moderate
tier files genuinely call the shared module with different variables,
these three CloudFormation templates duplicate their module's logic
inline rather than referencing it via a nested stack
(`AWS::CloudFormation::Stack`). If you change the shared logic in
`modules/`, remember to update these copies too — they won't pick up
changes automatically the way the Terraform module calls do.

See `../docs/control-mapping.md` for template-to-control detail.
