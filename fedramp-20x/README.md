# FedRAMP 20x

FedRAMP 20x is a fundamentally different assessment model from Rev5's
control baselines — authorizations are validated against a smaller set of
**Key Security Indicators (KSIs)**, many of which are meant to be verified
in a machine-readable way rather than through a traditional control
narrative.

**Status (September 2026):** FedRAMP finalized the **Consolidated Rules
for 2026 ("CR26")** on 2026-06-24, moving 20x from pilot to a generally
available certification path. "FedRAMP Authorization" is now "FedRAMP
Certification," and certifications are organized into Classes A–D. Rev5 Classes B/C/D loosely align with the old Low/Moderate/High baselines, but FedRAMP states there is no direct correlation between a Class and an impact level. See
`../docs/FEDRAMP-20X-CHEAT-SHEET.md` for a plain-language rundown of what
changed and why.

## The current KSI catalog

CR26 finalized the KSI structure at **46 individual indicators across 10
top-level families**, confirmed directly against FedRAMP's own
machine-generated reference doc
([`reference/key-security-indicators.md`](https://github.com/FedRAMP/2026-markdown/blob/main/reference/key-security-indicators.md)
in [github.com/FedRAMP/2026-markdown](https://github.com/FedRAMP/2026-markdown),
last KSI-level changelog entry dated 2026-06-24 as of this update). That
repo — not `github.com/FedRAMP/docs`, which has been renamed to
`docs-legacy` and is no longer current — is FedRAMP's actual source of
truth for the finalized rule text. Always check it directly before relying
on anything below for real KSI evidence-mapping; family scope and
individual indicator wording can still be revised.

| Family | Code | Individual KSIs | Folder in this repo |
|---|---|---|---|
| Cybersecurity Education | `CED` | 1 (`RAT`) | — (training records, not infrastructure; see `../docs/COVERAGE-GAPS.md`) |
| Change Management | `CMT` | 4 (`LMC`, `RMV`, `RVP`, `VTD`) | — (process/procedure, not infrastructure) |
| Cloud Native Architecture | `CNA` | 8 (`DFP`, `EIS`*, `IBP`, `MAT`, `OFA`, `RNT`, `RVP`, `ULN`) | `ksi-cna/` |
| Identity and Access Management | `IAM` | 6 (`AAM`, `APM`, `ELP`, `JIT`, `SNU`, `SUS`) | `ksi-iam/` |
| Incident Response | `INR` | 3 (`AAR`, `RIR`, `RPI`) | `ksi-inr/` |
| Monitoring, Logging, and Auditing | `MLA` | 5 (`ALA`*, `EVC`, `LET`, `OSM`, `RVL`) | `ksi-mla/` |
| Policy and Inventory | `PIY` | 5 (`GIV`, `RES`, `RIS`, `RSD`, `RVD`) | — (governance/policy, not infrastructure) |
| Recovery Planning | `RPL` | 4 (`ABO`, `ARP`, `RRO`, `TRC`) | — (plans/procedures, not infrastructure) |
| Supply Chain Risk | `SCR` | 2 (`MIT`, `MON`) | — (process, not infrastructure) |
| Service Configuration | `SVC` | 8 (`ACM`, `ASM`, `EIS`, `PRR`*, `RUD`*, `SIN`, `VCM`*, `VRI`) | `ksi-svc/` |

\* Marked "Class B, Optional" in the official reference at time of writing
(`KSI-CNA-EIS`, `KSI-MLA-ALA`, `KSI-SVC-PRR`, `KSI-SVC-RUD`,
`KSI-SVC-VCM`) — not required for every Certification Class. Verify current
applicability per Class before treating one as mandatory.

### What changed from the pre-CR26 structure

This folder was originally organized around an earlier six-category
description of KSIs (`CNA`, `IAM`, `MLA`, `CNBC`, `SVC`, `INR`) from before
the structure was finalized. Five of those six family codes carried
through into CR26 unchanged (`CNA`, `IAM`, `MLA`, `SVC`, `INR`) — the
folders below are still valid. The sixth, **`CNBC` (Configuration and
Network Boundary Controls), does not exist as a top-level family in the
finalized CR26 catalog.** Its scope split:

- Network-boundary/traffic-flow indicators → now under **Cloud Native
  Architecture** (`KSI-CNA-RNT`, `KSI-CNA-ULN`, `KSI-CNA-RVP`)
- Configuration-drift/management indicators → now under **Service
  Configuration** (`KSI-SVC-ACM`)

The old `ksi-cnbc/` folder was an empty placeholder (`.gitkeep` only) and
has been removed, so nothing needed migrating — new evidence for that
scope should go under `ksi-cna/` or `ksi-svc/` per the split above. See
`../docs/control-mapping.md` for the template-level crosswalk, which uses
CR26's lettered IDs (e.g. `KSI-MLA-LET`). It was remapped in September 2026
from the pilot-era numbered IDs (e.g. `KSI-MLA-01`) by matching each
template's behavior to the official indicator wording — a judgment-based
mapping, not an assessor-validated one, so confirm before citing a row.

CR26 also formalized four families with no infrastructure-template
equivalent in this repo — `CED` (training), `PIY` (policy/inventory/SDLC),
`RPL` (recovery planning), and `SCR` (supply chain risk) — because they're
evidenced by process, documentation, and organizational practice rather
than CloudFormation resources. They're listed above for completeness; see
`../docs/COVERAGE-GAPS.md` for what this repo can't automate.

## Folders (by KSI family)

| Folder | KSI Family | Existing templates that already satisfy it |
|---|---|---|
| `ksi-cna/` | Cloud Native Architecture | `modules/eks-hardened/template.yaml`, `modules/ecs-fargate-hardened/template.yaml`, `modules/network-perimeter-vpc/template.yaml` |
| `ksi-iam/` | Identity and Access Management | `moderate/iam-access-control/access-control-baseline.yaml`, `modules/iam-password-policy/template.yaml`, `modules/org-scp-boundary/template.yaml` |
| `ksi-mla/` | Monitoring, Logging and Auditing | `modules/org-cloudtrail/template.yaml`, `modules/guardduty-org/template.yaml`, `modules/security-hub-org/template.yaml`, `moderate/logging-monitoring/cis-metric-alarms.yaml`, `modules/ecs-fargate-hardened/template.yaml` |
| `ksi-svc/` | Service Configuration | `moderate/data-protection/s3-account-public-access-block.yaml`, `moderate/data-protection/kms-cmk-baseline.yaml`, `modules/account-baseline/template.yaml`, `modules/ecr-hardened/template.yaml`, `modules/rds-postgres-hardened/template.yaml`, `modules/ssm-patching-hardened/template.yaml`, `modules/config-conformance-pack/template.yaml` (config/drift scope formerly under CNBC), `moderate/network-boundary/vpc-flow-logs.yaml`, `modules/fips-vpc-endpoints/template.yaml`, `modules/org-scp-boundary/template.yaml`, `modules/waf-hardened/template.yaml` (network-boundary scope formerly under CNBC — see split note above; some of these also support `ksi-cna/`) |
| `ksi-inr/` | Incident Response | `modules/guardduty-org/template.yaml`, `moderate/incident-response/incident-notifications.yaml` |

Many of `../modules/` templates already satisfy specific KSIs (see the
mapping table in each module's header comment and in
`../docs/control-mapping.md`) — check there before building something new.
This table is a starting point for which existing template to point to when
assembling KSI evidence; it is not a substitute for reading the actual KSI
definitions, since 20x's specific validation method for each indicator may
expect something more precise than "a relevant control exists."

## Where to check for current, authoritative information

- [fedramp.gov/2026](https://www.fedramp.gov/2026/) — the CR26 rules site
- [github.com/FedRAMP/2026-markdown](https://github.com/FedRAMP/2026-markdown) —
  human-readable generated docs (what this update was sourced from)
- [github.com/FedRAMP/rules](https://github.com/FedRAMP/rules) — the
  underlying machine-readable JSON ruleset (`fedramp-consolidated-rules.json`)
- https://www.fedramp.gov/updates/changelog — plain-language change log
