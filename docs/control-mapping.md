# Control Mapping

Cross-reference of templates to the NIST SP 800-53 Rev5 control IDs
(Moderate/High) or FedRAMP 20x Key Security Indicator IDs they help
implement. This is a starting point for your control implementation
narrative — always verify against your current SSP language and your 3PAO's
expectations.

## modules/ (shared baseline, used by all three tracks)

| Template | Rev5 Controls | 20x KSI |
|---|---|---|
| `org-cloudtrail.yaml` | AU-2, AU-3, AU-6, AU-11, AU-12 | KSI-MLA-01, KSI-MLA-02 |
| `config-conformance-pack.yaml` | CM-2, CM-6, CM-8, CA-7 | KSI-CNBC-01, KSI-CNBC-02 |
| `guardduty-org.yaml` | SI-4, IR-4 | KSI-MLA-03, KSI-INR-01 |
| `security-hub-org.yaml` | CA-7, RA-5, SI-4 | KSI-MLA-04 |
| `iam-password-policy.yaml` | IA-5, AC-2, AC-7 | KSI-IAM-01 |

**Note:** `iam-password-policy.yaml` and `guardduty-org.yaml` both use a
small Lambda-backed custom resource internally. Neither the account
password policy nor GuardDuty organization auto-enrollment has a native
CloudFormation resource type — both can only be set via a direct API call
(`iam:UpdateAccountPasswordPolicy` and `guardduty:UpdateOrganizationConfiguration`
respectively), so a custom resource is the standard way to manage them as
code instead of a manual console/CLI step.

## moderate/

| Folder | Rev5 Control Family |
|---|---|
| `logging-monitoring/` | AU (Audit and Accountability) |
| `iam-access-control/` | AC (Access Control), IA (Identification and Authentication) |
| `network-boundary/` | SC (System and Communications Protection) |
| `data-protection/` | SC-13, SC-28, MP (Media Protection) |
| `incident-response/` | IR (Incident Response) |

### logging-monitoring/cis-metric-alarms.yaml

All 14 filter patterns are copied verbatim from AWS's Security Hub CSPM
documentation — the corresponding Security Hub checks (CloudWatch.1 through
CloudWatch.14) fail if the exact prescribed pattern isn't used, so none of
these are paraphrased.

| Alarm | Rev5 Controls | 20x KSI |
|---|---|---|
| Root account usage | AC-6(5), AU-6 | KSI-IAM-02, KSI-MLA-01 |
| Unauthorized API calls | AU-6, SI-4 | KSI-MLA-04 |
| Console sign-in without MFA | IA-2(1), AU-6 | KSI-IAM-01 |
| IAM policy changes | AC-6, AU-6 | KSI-IAM-03 |
| CloudTrail config changes | AU-12, AU-6 | KSI-MLA-01 |
| Console auth failures | AU-6, SI-4 | KSI-MLA-04 |
| CMK disable/scheduled deletion | SC-12, SC-28 | KSI-CNBC-02 |
| S3 bucket policy changes | AC-3, AU-6 | KSI-CNBC-01 |
| Config configuration changes | CM-6, AU-6 | KSI-CNBC-01 |
| Security group changes | CM-6, SC-7 | KSI-CNBC-02 |
| NACL changes | CM-6, SC-7 | KSI-CNBC-02 |
| Network gateway changes | SC-7, AU-6 | KSI-CNBC-02 |
| Route table changes | SC-7, AU-6 | KSI-CNBC-02 |
| VPC changes | SC-7, CM-6 | KSI-CNBC-02 |

*(Fill in remaining folder mappings as templates are added.)*

### iam-access-control/access-control-baseline.yaml

| Resource | Rev5 Controls | 20x KSI |
|---|---|---|
| IAM Access Analyzer (external access) | AC-3, AC-6 | KSI-IAM-03 |
| IAM Access Analyzer (unused access) | AC-2(3) | KSI-IAM-03 |
| Developer permission boundary | AC-6, AC-6(1) | KSI-IAM-01 |
| Require-MFA IAM group policy | IA-2(1), AC-7 | KSI-IAM-01 |
| Root usage EventBridge rule + SNS | AC-6(5), AU-6 | KSI-IAM-02, KSI-MLA-01 |

Note: the require-MFA group governs IAM *users* only — SSO/Identity Center
users authenticate through your IdP, so their MFA enforcement lives there,
not in this template.

### network-boundary/

| Template | Rev5 Controls | 20x KSI |
|---|---|---|
| `vpc-flow-logs.yaml` | SC-7, AU-2, AU-12 | KSI-CNBC-02, KSI-MLA-01 |
| `default-security-group-lockdown.yaml` | SC-7, CM-7 | KSI-CNBC-02 |

### data-protection/

| Template | Rev5 Controls | 20x KSI |
|---|---|---|
| `s3-account-public-access-block.yaml` | AC-3, AC-6, SC-7 | KSI-CNBC-01, KSI-SVC-01 |
| `kms-cmk-baseline.yaml` | SC-12, SC-13, SC-28 | KSI-SVC-02 |

**Note:** `s3-account-public-access-block.yaml` and
`default-security-group-lockdown.yaml` both use a Lambda-backed custom
resource, for the same reason as the modules noted above: neither
account-level S3 Public Access Block nor default-security-group rule
management has a native CloudFormation resource type.

### incident-response/

| Template | Rev5 Controls | 20x KSI |
|---|---|---|
| `incident-notifications.yaml` | IR-4, IR-5, IR-6 | KSI-INR-01, KSI-INR-02 |

## high/

High reuses the Moderate folder structure with tighter parameters. Only list
a template here if it diverges structurally from its Moderate counterpart
(not just parameter values) — e.g. FIPS 140-3 validated endpoint enforcement,
extended log retention, additional audit event types required at High.
See `high/README.md` for example parameter override files.

## fedramp-20x/

FedRAMP 20x KSI categories (subject to change as FedRAMP finalizes guidance —
see https://www.fedramp.gov/updates/changelog for the current status):

| Folder | KSI Category |
|---|---|
| `ksi-cna/` | Cloud Native Architecture |
| `ksi-iam/` | Identity and Access Management |
| `ksi-mla/` | Monitoring, Logging and Auditing |
| `ksi-cnbc/` | Configuration and Network Boundary Controls |
| `ksi-svc/` | Service Configuration |
| `ksi-inr/` | Incident Response |

*(Fill in specific template-to-KSI mappings here as templates are added.)*
