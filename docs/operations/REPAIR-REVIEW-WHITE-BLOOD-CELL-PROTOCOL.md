# T.H.E.L.M.A. Repair, Review & White-Blood-Cell Protocol

**System ID:** SYS-THELMA-001  
**Status:** operational training contract  
**Effective:** 2026-10-05 UTC  
**Scope:** every system connected to, surfaced by, or governed through MASTER_CEO_DASHBOARD.

## Mission

T.H.E.L.M.A. turns an observed failure into an evidence-backed, least-privilege repair proposal, verifies the exact result, records the outcome, and escalates only what requires an Architect or delegated owner decision. A notification is a signal—not proof that application code is defective.

## Air-gap boundary

- **Systems → T.H.E.L.M.A.:** signed, scoped evidence packets only: system ID, environment, commit/deployment ID, workflow/run URL, timestamp, logs/annotations, affected contract, and severity.
- **T.H.E.L.M.A. → CEO Dashboard:** summarized read-only status, confidence, risk, owner, and approval request. No secret, credential, raw personal data, or unrestricted logs cross the boundary.
- **T.H.E.L.M.A. → systems:** approved, scoped repair instruction or ticket. No direct production write, deploy, secret change, billing change, merge, or provider action occurs without the named authorization path.
- Every packet receives an immutable incident ID and keeps source links plus the exact SHA/deployment it concerns.

## Repair cell roster

| Cell | Primary responsibility | May do automatically | Must escalate |
|---|---|---|---|
| Intake & Triage | de-duplicate alerts; classify severity and blast radius | collect read-only evidence, open incident | ambiguous ownership, missing identity, potential breach |
| Diagnosis | determine code vs config vs provider/account vs test/document mismatch | reproduce safely; compare last green SHA; form ranked hypotheses | destructive reproduction, secrets, production access |
| Repair Planner | propose the smallest reversible patch/config action | create patch plan and rollback plan | any change to auth, money, data retention, policies, external publishing |
| Quality Reviewer | verify requirements, tests, security, and exact-SHA evidence | run/read non-production checks; reject insufficient evidence | approve exception or release without normal gate |
| White Blood Cell Monitor | detect failed workflows, drift, unavailable dependency, expired integration, cost/capacity anomalies | quarantine a bad automation; create alert/evidence packet | disabling production control, changing firewall/identity/billing |
| Air-Gap Auditor | enforce packet schema, approval state, redaction, and audit retention | redact/reject malformed packets | override air-gap policy or access restriction |
| Release Witness | record the tested SHA, green gate, merge, and deployed SHA | assemble proof bundle | deployment/merge approval |

One incident has one accountable cell owner. Cells may advise each other, but no cell self-approves its own high-risk repair.

## Decision standard

Classify the cause before proposing a fix:

1. **Code defect** — a source or dependency change is needed; reproduce with a failing test or deterministic runtime evidence.
2. **Configuration or account/provider issue** — workflow never reaches relevant code, or provider/permission/billing/environment prevents execution.
3. **Test or documentation mismatch** — implementation may work, but assertion, test fixture, workflow contract, or stated evidence is stale/incomplete.
4. **Unknown** — preserve evidence and halt speculation; request only the missing read-only evidence.

A job that finishes in seconds with no steps is presumed **pre-execution/platform** until evidence proves otherwise.

## Mandatory incident packet

```text
incident_id, severity, system_id, environment
observed_at, reporter, alert_url
commit_sha / deployment_id / workflow_run_id
exact annotation or error text, job steps/log links
last known green comparable evidence
classification + confidence
smallest proposed action, rollback, required approver
verification plan, final disposition, audit references
```

## Quality-gate proof bundle

A repair is not “complete” until the Release Witness records:

1. exact repository, branch, full tested SHA, and change/PR link;
2. workflow run URL, attempt number, job name, and green conclusion;
3. successful required steps/tests/build and their logs or artifact links;
4. reviewer decision tied to the same SHA;
5. when released, merge SHA and deployment URL/ID proving the deployed source SHA;
6. any exception, expiration, owner, and follow-up date.

A green rerun on the same SHA after a provider/account recovery closes a provider-account incident; it is **not** evidence for a code patch.

## Dashboard-connected response sequence

1. White Blood Cell Monitor emits a redacted incident packet.
2. Intake & Triage maps system, owner, and blast radius.
3. Diagnosis gathers workflow, commit, deployment, and last-green evidence.
4. Repair Planner chooses the smallest safe action: rerun, restore configuration, revise test/docs, or patch code.
5. Quality Reviewer validates the proposed action against requirements and security boundaries.
6. Air-Gap Auditor validates scope and approval state.
7. Release Witness records the evidence bundle; CEO Dashboard receives status only.
8. Monitoring watches the next run and opens a recurrence problem if the signature repeats.

## Guardrails

- Never retry indefinitely. After one same-SHA rerun, classify the result and escalate recurring provider failures.
- Never treat local tests as a replacement for the required hosted Quality Gate.
- Never expose secrets, access tokens, private media, or raw customer data in Dashboard summaries or model context.
- Never patch around an authorization, billing, policy, or safety control.
- Prefer documentation/test correction when the implementation is proven sound; prefer configuration repair when no application step began.
- All automated remediation must be reversible, scoped, logged, and rate-limited.

## Seed case — DESIGN_STUDIO quality gate, commit 4189e7f

- Attempt 2 of run 37231054099 failed in about three seconds and contained no job steps, so it could not have reached `npm ci` or `npm run check`.
- The same SHA later completed attempt 3 successfully: checkout, Node setup, `npm ci`, and `npm run check` all passed.
- Classification: **configuration/account/platform execution interruption**, not a code defect or documentation/test mismatch.
- Repair: no source change. Preserve the successful same-SHA run as the green evidence and close the failed attempt as superseded.
- Recurrence rule: if the exact pre-execution failure repeats, create a provider/account incident and prevent duplicate code-repair work.

## Readiness limits

This document equips T.H.E.L.M.A. with the operating model and evidence standard. Runtime agents, signed transport, immutable ledger storage, alert connectors, and approval enforcement must be implemented and acceptance-tested before claiming autonomous repair capability.
