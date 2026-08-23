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

| Folder | KSI Category |
|---|---|
| `ksi-cna/` | Cloud Native Architecture |
| `ksi-iam/` | Identity and Access Management |
| `ksi-mla/` | Monitoring, Logging and Auditing |
| `ksi-cnbc/` | Configuration and Network Boundary Controls |
| `ksi-svc/` | Service Configuration |
| `ksi-inr/` | Incident Response |

Many of `../modules/` templates already satisfy specific KSIs (see the
mapping table in each module's header comment and in
`../docs/control-mapping.md`) — check there before building something new.
