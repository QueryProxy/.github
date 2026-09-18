<!--
  Thanks for opening a pull request.

  Fill in the sections below. Sections you do not need can be removed, but
  please keep at least Summary and Test plan.

  If this PR touches a guard, a policy, webhook verification, masking or
  credential handling, the "Security impact" section is not optional.
-->

## Summary

<!-- One to three sentences describing what this PR does and why. -->

## Related issues

<!-- e.g. Fixes #123, Refs #456. Remove this section if not applicable. -->

## Type of change

<!-- Check all that apply. -->

- [ ] Bug fix (non-breaking)
- [ ] New feature (non-breaking)
- [ ] Breaking change (behavior, configuration, or database schema)
- [ ] Documentation only
- [ ] Refactor / cleanup (no behavior change)
- [ ] Build / release / CI

## Test plan

<!--
  How did you verify this change works, and what did you check that it does
  not break? Be specific: commands run, scenarios tested, edge cases
  considered.
-->

- [ ] `php artisan test` passes locally
- [ ] `./vendor/bin/pint` reports no changes
- [ ] Added or updated tests for the new behavior
- [ ] Manually verified the affected screens / flows in a running instance

## Security impact

<!--
  Required if this PR touches any of:
    - the SQL guards (SqlInspector): statement classification, WHERE
      enforcement, LIMIT clamping, forbidden statements
    - authorization: roles, policies, team isolation, connection grants,
      self-approval prevention
    - webhook verification: Slack signatures, the Teams HMAC endpoint,
      replay windows
    - masking (Masker) and the point at which results are written
    - credential storage, logging, audit records or error formatting

  Otherwise write "None — does not touch guards, policies, webhooks, masking
  or credentials."
-->

- [ ] This change does **not** weaken an existing guard, policy or verification step.
- [ ] Every new code path that executes against a target database is covered by an audit record.
- [ ] No credentials, tokens or unmasked result values can reach logs, exceptions or audit metadata through this change.

## Database and configuration changes

<!-- Remove if not applicable. -->

- [ ] Adds a migration (one migration per feature)
- [ ] Adds or changes an environment variable — documented in `.env.example` and in the configuration docs
- [ ] Requires an action from existing deployments — described below

## Notes for reviewers

<!--
  Anything reviewers should pay extra attention to? Trade-offs you considered?
  Decisions you would like a second opinion on? Remove if not applicable.
-->

## Checklist

- [ ] I have read the [Contributing guide](../CONTRIBUTING.md).
- [ ] My contribution is licensed under AGPL-3.0-or-later (inbound = outbound).
- [ ] `CHANGELOG.md` is updated under *Unreleased* if this change is user-visible.
- [ ] Documentation (README, `queryproxy.com/docs/`) is updated if user-visible behavior changed.
