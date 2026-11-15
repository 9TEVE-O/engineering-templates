# Independent agent execution gate: {project or action name}

Version {n}, {YYYY-MM-DD}. Owner: {name}. Candidate commit: {full SHA}.
Status: PROPOSED / EXECUTED-PASS / EXECUTED-FAIL / BLOCKED.

Use this template for **one consequential agent action path at a time**. An agent's
confidence or explanation is not proof that the action is correct or authorised.
Prediction, evidence checking, permission and external execution have separate owners.

> **Good:** the reader can identify the exact external effect, the independent gate
> that controls it, the source revision the decision used and the receipt proving
> what happened. If any link in that chain is missing, the action stays blocked.

## 1. Execution scope

| Field | Record |
| --- | --- |
| Project or service | {repository, deployment, revision} |
| Exact action | {tool name, parameters and consequential external effect} |
| Source of authority | {record ID, access method, revision or hash} |
| Agent access | {read and propose only, permitted resources} |
| Prohibited direct route | {API, credential or execution path the agent must not reach} |
| Independent gate | {service and its separate identity or trust boundary} |
| Privileged executor | {only service holding write credentials, or gap} |
| Human authoriser | {identity provider and role, never agent self-approval} |
| Reviewer and date | {person and date} |
| Current assessment | {PROPOSED / EXECUTED-PASS / EXECUTED-FAIL / BLOCKED} |

## E. Evidence

**Failure statement:** {How a confident but substantively wrong proposal could
produce a consequential external effect.}

**Claim under test:** {Exact independently testable claim, without overstating the evidence.}

| Required item | Recorded evidence | State |
| --- | --- | --- |
| Authoritative source, revision or hash | {link and SHA or revision} | {VERIFIED / SUPPLIED / UNKNOWN} |
| Read method and timestamp | {independent gate retrieval and observed time} | {state} |
| Independently derived parameters | {formula, expected result or source comparison} | {state} |
| Agent proposal | {exact tool, scope, parameters and effect hash} | {state} |
| Human approval | {actor, action hash, source revision, scope, expiry, nonce} | {state} |
| Executor identity and credential isolation | {code, configuration and independent check} | {state} |
| Outbox or durable action record | {storage and transaction or receipt} | {state} |
| Deployment revision | {commit, environment and deployment receipt} | {state} |

**Evidence conflicts:** {Contradictions, unavailable records and assumptions.
An authenticated document can still contain a false assertion. Compare its
substantive claim with independent evidence where available.}

**Evidence quality:** {SUPPLIED / VERIFIED / ASSUMED / UNKNOWN; say which
original sources and tests have actually been opened.}

**Independent corroboration:** {An authentic source or a matching checksum does
not prove its substantive claim. Record independent counter-evidence, what can
be verified without circular reliance on the same origin, and HOLD if the
applicable source cannot be resolved.}

## A. Corrective action

- [ ] Remove external write credentials and direct execution routes from the
      agent. Leave read and propose capabilities only.
- [ ] Route the exact external effect through a separately controlled,
      deny-by-default gate and privileged executor.
- [ ] Have the gate retrieve original authoritative evidence independently;
      check source identity, current revision, freshness, claim correctness,
      exact parameters, tool and target.
- [ ] Bind independently issued human approval to actor identity, action hash,
      source revision, permitted scope, expiry and single-use nonce. Keep
      signing keys outside agent reach.
- [ ] Persist nonce, outbox and effect state durably, including across restart
      and concurrent requests. Reconcile an indeterminate effect before retry.
- [ ] On contradictory, stale, absent or malformed evidence, invalid approval,
      unavailable verification, unknown tool or bypass attempt: DENY and log.
- [ ] Verify there is no other working privileged route around the gate.

**Smallest change proposed:** {Exact modification, owner and evidence required.}

## A. Acceptance record

One row per test. Record **actual** evidence links; do not pre-fill pass results.
A synthetic policy-unit test cannot certify a deployed privileged executor.

| Scenario | Expected | Actual result / evidence |
| --- | --- | --- |
| Confidently wrong proposal | DENY | {NOT RUN or result and link} |
| Correct proposal and valid approval | ALLOW ONCE | {NOT RUN or result and link} |
| Forged approval | DENY | {NOT RUN or result and link} |
| Approval replay | DENY | {NOT RUN or result and link} |
| Source revision changes | DENY | {NOT RUN or result and link} |
| Authentic but false source | DENY OR HOLD FOR INDEPENDENT CHECK | {NOT RUN or result and link} |
| Conflicting source versions | HOLD UNTIL APPLICABLE REVISION RESOLVED | {NOT RUN or result and link} |
| Action parameters changed after approval | DENY | {NOT RUN or result and link} |
| Approval missing or expired | DENY | {NOT RUN or result and link} |
| Independent verifier unavailable | DENY | {NOT RUN or result and link} |
| Unknown tool or direct bypass | DENY | {NOT RUN or result and link} |
| Concurrent attempt and restart | NO DUPLICATE EFFECT | {NOT RUN or result and link} |
| Timeout after possible external effect | HOLD AND RECONCILE | {NOT RUN or result and link} |

**Execution environment:** {synthetic, isolated integration or deployed system}.
**Exact tested revision:** {SHA or build ID}.
**Assertions and actual effects:** {test count, output, external executor receipt}.
**Limits:** {which controls and outcomes this evidence does not establish}.

**Adversarial counterexamples:** {Recorded failures of the reference test
harness, including source data that is internally consistent but false, or a
privileged effect route the test harness cannot isolate. A test detecting
such a weakness is not a production gate PASS.}

Keep the exact run results in the
[test evidence record](../07-release/test-evidence.md). Track unresolved
controls in [hardening status](hardening-status.md) and trust boundaries in
the [threat model](threat-model.md).

## Release decision

**Default:** HOLD. No release solely because an agent, a simulation or a
policy-unit test says the gate passed. Verify the actual privileged executor,
credential isolation, independently retrieved evidence, approval issuance,
durable persistence, replay resistance and bypass control at the deployment
revision. Retain the human release decision.

| Item | Record |
| --- | --- |
| Decision | {HOLD / RELEASE} |
| Authorised by | {human identity, never the agent} |
| Date and deployment revision | {date and SHA} |
| Independent executor evidence | {receipts, audit links and tested effects} |
| Outstanding gate | {single highest-value remaining proof} |

See the [release checklist](../07-release/release-checklist.md) for broader
release requirements. The execution gate is one dependency, not a replacement
for the full release review.

## Worked example

A synthetic supplier-notice agent claims a contract requires 30 calendar days'
notice, while the authoritative contract states 60 days. It proposes sending
notice using an external mail tool. Even if the agent reports 99% confidence,
the independent gate must deny the conflicting action. An approval for the
wrong action does not repair the underlying evidence contradiction.

This is an **illustrative fixture**, not proof that any production executor
or credential boundary was tested. The corresponding deployment stays on HOLD
until independent integration evidence and human release approval exist.
