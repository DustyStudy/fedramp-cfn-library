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

## moderate/

| Folder | Rev5 Control Family |
|---|---|
| `logging-monitoring/` | AU (Audit and Accountability) |
| `iam-access-control/` | AC (Access Control), IA (Identification and Authentication) |
| `network-boundary/` | SC (System and Communications Protection) |
| `data-protection/` | SC-13, SC-28, MP (Media Protection) |
| `incident-response/` | IR (Incident Response) |

*(Fill in specific template-to-control mappings here as templates are added.)*

## high/

High reuses the Moderate folder structure with tighter parameters. Only list
a template here if it diverges structurally from its Moderate counterpart
(not just parameter values) — e.g. FIPS 140-3 validated endpoint enforcement,
extended log retention, additional audit event types required at High.

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
