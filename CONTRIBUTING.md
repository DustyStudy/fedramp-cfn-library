# Contributing

Thanks for considering a contribution. This repo is meant to be a practical,
trustworthy reference — please keep a few things in mind.

## Ground rules

- **Map every template to its control(s) or KSI(s).** A template without a
  mapping in `docs/control-mapping.md` isn't useful to someone building an
  SSP. State the NIST 800-53 Rev5 control ID (e.g. `AC-2`, `AU-6`) or the
  FedRAMP 20x KSI ID (e.g. `KSI-MLA-01`) in a comment block at the top of the
  template and in the docs table.
- **No hardcoded secrets, account IDs, or ARNs.** Use CloudFormation
  parameters, SSM parameters, or Secrets Manager references.
- **Least privilege by default.** IAM policies should scope to specific
  resources/actions, not `*:*`. If a wildcard is genuinely required, explain
  why in a comment.
- **Prefer AWS Config managed rules / conformance packs** over reinventing
  detection logic where AWS already publishes one.
- **Test before submitting a PR.** Deploy the stack in a sandbox account and
  confirm it creates successfully and the resources match intent.
- **Note any assumptions** (e.g. "assumes AWS Organizations is already set
  up", "assumes a delegated administrator account for security services").

## PR checklist

- [ ] Template validated with `aws cloudformation validate-template`
- [ ] Control/KSI mapping added to `docs/control-mapping.md`
- [ ] No hardcoded account IDs, ARNs, or credentials
- [ ] README in the relevant folder updated if this changes scope
- [ ] Deployed successfully in a test account

## Reporting issues

If you find a template that's out of date with current FedRAMP guidance
(especially anything in `fedramp-20x/`, since that program is still
evolving), please open an issue with a link to the current FedRAMP.gov
guidance so it can be corrected quickly.
