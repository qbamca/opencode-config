# Architect-to-Coder Delivery Proof

These cases specify fresh-process behavioral checks for the depth-one primary-to-leaf workflow. Run each case in a unique fixture below `/tmp/opencode`; never target a real repository.

Assertions must inspect persisted OpenCode `session`, `message`, and `part` evidence read-only, deduplicate copied fork history by task `call_id`, and correlate each run with a unique textual marker and start timestamp. Do not query credential, account, provider configuration, permission, share, or environment data. Agent prose and task transport completion are not sufficient proof.

For every case assert that all delegated sessions are direct children of the selected main agent, no leaf emits a `task` call, and maximum depth is one.

## Cases

- `01-required-architecture.md`: Architect plan acceptance and complete inline transfer to Coder.
- `02-optional-fast-path.md`: bounded change proceeds directly to Coder.
- `03-plan-contradiction.md`: same Architect and Coder are resumed around an amendment.
- `04-validation-failure.md`: failed command cannot become semantic success.
- `05-review-correction.md`: same Coder and Reviewer are resumed after a finding.
- `06-claim-only-rejection.md`: unsupported verification prose is not accepted as evidence.
- `07-browser-blocked.md`: unavailable browser capability becomes `blocked`.
- `08-lifecycle-reconciliation.md`: unresolved required children prevent success.

Configuration changes are loaded only by fresh OpenCode processes. Record exact session and call IDs for every assertion.
