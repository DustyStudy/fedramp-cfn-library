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
- [ ] cfn-lint and Checkov pass in CI (or findings are explicitly skipped
      with justification — see below)
- [ ] Control/KSI mapping added to `docs/control-mapping.md`
- [ ] No hardcoded account IDs, ARNs, or credentials
- [ ] README in the relevant folder updated if this changes scope
- [ ] Deployed successfully in a test account

## Handling a Checkov finding you disagree with

Security baseline templates sometimes need patterns Checkov flags by
default — a deliberately broad `NotAction` in a permission boundary, an
AWS-managed KMS key where a CMK isn't warranted, etc. Don't silence these
by disabling the whole check repo-wide. Instead, skip it at the specific
resource with a reason, so the next reader (including future-you) can see
why:

```yaml
Resources:
  MyResource:
    Type: AWS::Some::Resource
    Metadata:
      checkov:
        skip:
          - id: "CKV_AWS_XXX"
            comment: "Why this is intentional, not an oversight"
    Properties:
      ...
```

If you're not sure whether a finding is a real issue or an accepted
trade-off, open the PR anyway and flag it in the description — that's a
useful discussion to have in the open.

## Reporting issues

If you find a template that's out of date with current FedRAMP guidance
(especially anything in `fedramp-20x/`, since that program is still
evolving), please open an issue with a link to the current FedRAMP.gov
guidance so it can be corrected quickly.
