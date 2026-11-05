# LiCai agent rules

Before acting, read this file and the relevant document in `docs/`.

## Roles

- **Decision agent** owns the recommendation or artifact. It must provide the task requirements, assumptions, evidence, validation performed, and known limits.
- **Adversarial reviewer** independently checks the submitted artifact and reports findings only. Do not edit, repair, or silently rewrite the artifact under review. Return proposed remedies to the decision layer.
- **Decision layer** may challenge a finding by naming it and supplying counter-evidence or explaining the evidence gap. A challenge is not resolved until the reviewer rechecks it and records support, correction, or withdrawal.
- **Execution layer** acts only after an explicit reviewer pass and the required decision-layer approval. Do not treat a draft, unresolved review, or missing evidence as approval.

## Review rules

- Use the format in [docs/review-gate.md](docs/review-gate.md): each finding must state the problem, evidence, impact, proposed solution, and who must correct it.
- Check the request against the artifact item by item. For code, inspect callers, input boundaries, errors, and relevant validation. For data, trace sources, timestamps, units, definitions, and transformations. For research, distinguish facts, opinions, and inference.
- Use authoritative and current sources for facts that can change. Record the source URL and the time checked. A skill description or third-party summary is not evidence that a live endpoint currently works.
- If a source, calculation, code path, or requirement cannot be checked, label it unverified and keep the result from passing when it is material.
- Do not claim that code is bug-free or that a review proves zero risk. A pass is limited to the stated scope, inspected evidence, and recorded validations.
- Never fabricate a citation, test result, dataset, account state, or GitHub setting.
- Keep credentials and personal account data out of commits, PRs, logs, and reports.

## Handoff

Every submitted decision artifact goes to the reviewer before execution. The reviewer returns a scoped report to the decision layer. Only a review marked `PASS — scoped`, with no unresolved finding or material item awaiting verification, may be handed to execution. Findings cannot be waived by a risk-acceptance note. The decision layer may dispute a finding and ask the reviewer to recheck it; if the issue stands, the artifact must be revised or abandoned before handoff.

GitHub branch protection must be configured separately. These repository instructions and PR templates guide agents but do not, by themselves, enforce a server-side merge block.
