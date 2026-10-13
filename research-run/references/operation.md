# Operation: run folder, JSON records, handoffs and failures

## Run folder

Root: the runs root resolved as described under **Where runs live** in `SKILL.md`.

```text
research-runs/
  registry.json                        index of runs, shared by all runs
  <run-folder>/                        one descriptive folder per run
    frame.json                         approved understanding, token budget plan, approval
    frame-v1.json, frame-v2.json, ...  snapshot of each approved frame revision
    state.json                         status, work queue, budget, rounds, handoffs, continuations
    ledger.json                        cumulative evidence, findings, questions and change history
    round-01-discovery.json, ...       one record per round, including its review
    publication-source-v1.json, ...    frozen findings for each report version
    report-v1.html, ...                the HTML report for each version
    inputs/                            copies of files the user supplied
```

- Name the folder from the clarified question: lowercase words joined by hyphens, at most 60 characters. If the name exists, append `-2`, `-3` and so on.
- Stage keys used in file names and records: `discovery`, `problem`, `evidence`, `alternatives`, `falsify`, `synthesis`.

## Writing records

- The coordinator is the only writer of `frame.json`, `state.json`, `ledger.json` and `registry.json`. Delegated agents return a round payload, or write only their own round file, and the coordinator merges one round at a time.
- Keep every file valid JSON (UTF-8, two-space indentation). Where the environment allows, write to a temporary file in the same folder and rename it over the original, then re-read the file to confirm it parses.
- Save after every merge, handoff, budget check and status change, and before a session ends. At any moment the records must show the current evidence, findings, review decisions, remaining work and budget, not only the final result.
- Append; never erase. Claims, sources, rounds and reviews are superseded or given a new status, never deleted. Every change to a claim, proposition, finding, unknown, alternative, verdict or decision adds a `history` entry saying what changed and why.
- One active performer per work item. Quiet progress is not permission to start duplicate work; record a handoff before another agent takes an item over.
- If a write's result is unclear, re-read the file and reconcile once. Never regenerate research because a write's outcome is unknown.

## IDs

- Round-local IDs (`c1` claim, `s1` source, `q1` query, `u1` unknown, `x1` alternative, `a1` attack) become ledger IDs prefixed with the round number when merged, for example `r03.c7`.
- Propositions are `P1`, `P2`…; findings `F1`…; work items `W1`…; handoffs `H1`…; review findings `RF1`…; questions for the user `B1`….
- IDs never change and are never reused. A superseded claim keeps its ID and links to its successor.

## frame.json

The approved understanding of the request. When the user approves a revision, update `frame.json` and save a copy as `frame-vN.json`.

```json
{
  "interface": "research-run-frame-v2",
  "slug": "",
  "revision": 1,
  "revision_reason": "",
  "created_at": "",
  "topic": "the user's request, verbatim",
  "title": "",
  "question_answered": "",
  "problem_investigated": "",
  "target_end_state": "",
  "intended_audience": "",
  "decisions_supported": [],
  "actor": "",
  "scope_in": [],
  "scope_out": [],
  "horizon": "",
  "freshness_window_days": null,
  "segment_or_geo": "",
  "evidence_bar": "",
  "topic_kind": "general | product-opportunity",
  "verdict_dimensions": [{"id": "", "name": "", "statuses": []}],
  "information_to_explain": [],
  "expected_output": "",
  "definition_of_done": "",
  "constraints": [],
  "known_facts": [{"text": "", "source": "user"}],
  "clarifications": [{"id": "Q1", "dimension": "", "question": "", "answer": "", "source": "user | inferred | assumed"}],
  "supplied_materials": [{"name": "", "kind": "file | link | note", "location": "", "ledger_source_id": ""}],
  "token_budget": {
    "cap_tokens": null,
    "set_by": "user",
    "allocations": {"discovery": null, "problem": null, "evidence_and_alternatives": null, "falsify": null, "review_and_repair": null, "synthesis": null},
    "report_reserve": null,
    "sizing_note": ""
  },
  "plan": {"summary": "", "stages": [{"stage": "", "depth": "", "estimated_tokens": null}], "delegation": ""},
  "approval": {"approved_at": "", "source": "user", "summary_shown": ""}
}
```

## state.json

The run's live status. `budget` describes the active allowance (the original run, or the current continuation); when a continuation starts, move the previous budget into `budget_history`.

```json
{
  "interface": "research-run-state-v2",
  "slug": "",
  "title": "",
  "topic": "the user's request, verbatim",
  "created_at": "",
  "updated_at": "",
  "status": "awaiting-clarification | researching | awaiting-user | reporting | complete | stopped-at-budget | stopped-by-blocker | failed",
  "research_status": "not-started | in-progress | complete | stopped-at-budget | stopped-by-blocker",
  "report_status": "none | current | outdated",
  "phase": "clarify | discovery | problem | evidence | alternatives | falsify | synthesis | report",
  "frame_revision": 1,
  "version": 0,
  "next_round": 1,
  "budget": {
    "cap_tokens": null,
    "allocations": {},
    "report_reserve": null,
    "used": {"measured": null, "measured_source": "", "estimated": null, "estimate_method": ""},
    "remaining_before_reserve": {"tokens": null, "basis": "measured | estimated | mixed"},
    "status": "on-plan | tight | reserve-only",
    "adjustments": [{"at": "", "change": "", "reason": ""}]
  },
  "budget_history": [],
  "work_queue": [{"id": "W1", "stage": "", "assignment": "", "reason": "", "depends_on": [], "priority": 1, "status": "open | in-progress | done | deferred | dropped", "outcome": ""}],
  "rounds": [{"index": 1, "stage": "discovery", "file": "round-01-discovery.json", "work_ids": [], "repair_of": null, "status": "in-progress | merged | abandoned", "performer": "", "reviewer": "", "material_delta": true, "usage_tokens": null, "usage_basis": "measured | estimated"}],
  "handoffs": [{"id": "H1", "at": "", "to": "", "round": null, "work_ids": [], "budget_slice_tokens": null, "returned_at": "", "usage_tokens": null, "usage_basis": "", "notes": ""}],
  "continuations": [{"at": "", "intent": "answer-questions | new-direction | presentation-only | reframe", "user_input": "", "cap_tokens": null, "frame_revision": null, "result_version": null}],
  "blockers_open": [],
  "degraded": [{"code": "", "detail": "", "since": ""}],
  "failures": [{"at": "", "operation": "", "error": "", "affected_work_ids": [], "last_saved": "", "resolved_at": ""}],
  "versions": [{"version": 1, "publication_source": "publication-source-v1.json", "report": "report-v1.html", "frozen_at": "", "report_built_at": "", "frame_revision": 1, "research_status": ""}]
}
```

`degraded` records limitations that affect how far the result can be trusted, such as assumed clarifications or an unavailable independent reviewer.

## ledger.json

The cumulative evidence base. Records are keyed by ID; the shapes below show one entry of each.

```json
{
  "interface": "research-run-ledger-v2",
  "slug": "",
  "active_frame_revision": 1,
  "checked_through": "",
  "topic_kind": "",
  "territory": {"scope_in": [], "scope_out": [], "population_context": "", "sampling_frames": [], "query_families": {}, "source_classes_by_claim_type": {}, "surfaces": [], "exclusions": []},
  "surfaces": {"direct-product": {"status": "searched | inaccessible | not-applicable | pending", "query_ids": [], "note": ""}},
  "query_families_used": [],
  "search_log": [{"id": "r01.q1", "round": 1, "date": "", "query": "", "purpose": "orientation | confirm | disconfirm | freshness | substitute", "surface": "", "tool": "", "results_note": "", "used_source_ids": []}],
  "sources": {"r01.s1": {"id": "r01.s1", "title": "", "publisher": "", "location": "", "source_class": "", "publication_date": null, "accessed_date": "", "supplied": false, "opened": true, "access_note": "", "first_seen_round": 1}},
  "claims": {"r01.c1": {"id": "r01.c1", "round": 1, "statement": "", "claim_type": "", "evidence_type": "fact | estimate | interpretation | assumption", "stance": "supports | refutes | neutral", "support_level": "supports | suggests", "excerpt": "", "locator": "", "source_ids": [], "corroborating_source_ids": [], "proposition_ids": [], "publication_date": null, "event_date": null, "accessed_date": "", "behavioral_signal_tier": "", "freshness_status": "unchecked | current | stale | superseded | unverifiable", "confidence": "high | medium | low", "confidence_basis": "", "review_status": "pending | accepted | repair | insufficient-support | contradicted | contested", "state": "established | probable | mixed | contested | insufficient-support | contradicted | superseded", "contradicts_claim_ids": [], "superseded_by": null, "review_finding_ids": []}},
  "problem_records": [{"actor": "", "context_trigger": "", "job_outcome": "", "current_process": "", "breakdown": "", "consequence": "", "workaround": "", "uncertainty": ""}],
  "propositions": {"P1": {"id": "P1", "statement": "", "falsifier": "", "support_evidence": "", "qualify_evidence": "", "refute_evidence": "", "surfaces_to_search": [], "status": "open | supported | refuted | mixed | narrowed | unknown", "confidence": "high | medium | low | none", "supports": [], "refutes": [], "narrowed_from": null, "falsification_verdict": "stands | narrowed | contradicted", "open_unknowns": []}},
  "verdict_dimensions": [],
  "unknowns": {"r02.u1": {"id": "r02.u1", "question": "", "why": "not-searched | needs-first-party | needs-user-decision | no-public-evidence | contradictory | inaccessible-surface | not-resolved-within-budget", "what_would_resolve": "", "proposition_id": null, "status": "open | resolved", "resolved_by_claim_ids": []}},
  "contradictions": [{"id": "", "claim_ids": [], "nature": "direct | scope | date | method", "description": "", "resolution": "open | resolved", "resolution_note": ""}],
  "alternatives": {"r05.x1": {"id": "r05.x1", "name": "", "category": "", "status": "current | upcoming | abandoned", "adequacy": "adequate | inadequate | unknown", "constraints": "", "claim_ids": [], "source_ids": []}},
  "negative_evidence_appendix": {"searched_through": "", "date_ranges": [], "terms_used": [], "source_classes": [], "found_direct": [], "found_substitute_classes": [], "inaccessible": []},
  "attacks": [{"id": "", "round": null, "proposition_id": "", "kind": "", "worked": false, "summary": "", "evidence_claim_ids": []}],
  "revalidations": [{"claim_id": "", "round": null, "status": "current | stale | superseded | unverifiable", "source_id": "", "superseding_claim_id": null, "note": ""}],
  "verdicts": {"problem-reality": {"status": "", "confidence": "", "basis_claim_ids": [], "explanation": ""}},
  "findings": {"F1": {"id": "F1", "text": "", "evidence_type": "fact | estimate | interpretation | assumption | unknown", "information_items": [], "proposition_ids": [], "claim_ids": [], "confidence": "", "qualifications": [], "status": "current | narrowed | corrected | retired", "since_version": 1}},
  "synthesis": {"title": "", "clearest_answer": "", "coverage": [{"requirement": "", "conclusion": "", "claim_ids": [], "limitation": ""}], "confidence_basis": "", "methods_note": "", "change_note": ""},
  "decisions": [{"subject": "", "decision": "advance | revise | park", "reason": "", "reopen_when": "", "date": ""}],
  "validation_plan": {},
  "blockers": [{"id": "B1", "question": "", "why_it_matters": "", "if_unanswered": "", "kind": "decision | access | first-party | assumption-to-confirm", "proposition_ids": [], "status": "open | answered | withdrawn", "answer": ""}],
  "review_findings": [{"id": "RF1", "round": null, "reviewer": "", "severity": "blocking | major | minor", "target_ids": [], "rule": "", "text": "", "evidence_checked": "", "disposition": "accepted | repair | insufficient-support | contradicted | contested | frame-conflict | note", "resolution": "open | resolved | unresolved", "resolved_in_round": null}],
  "history": [{"at": "", "round": null, "object": "claim | proposition | finding | unknown | alternative | source | verdict | decision", "id": "", "change": "added | corrected | narrowed | qualified | superseded | contradicted | marked-insufficient | contested | resolved | retired | restored", "from": null, "to": null, "reason": "", "evidence_ids": []}]
}
```

`blockers` holds the questions for the user. `contradictions` holds conflicts between sources or claims only; disagreement between agents belongs in `review_findings`.

## Round files

One file per round, `round-NN-<stage>.json`, written when the round starts and completed as it is reviewed and merged. A repair or revisit is a new round that names what it addresses. Stage-specific outputs from `principles.md` (territory, propositions, alternatives, attacks, findings and so on) go in `payload` under their ledger names.

```json
{
  "interface": "research-run-round-v2",
  "slug": "",
  "index": 1,
  "stage": "discovery | problem | evidence | alternatives | falsify | synthesis",
  "frame_revision": 1,
  "status": "in-progress | merged | abandoned",
  "repair_of": null,
  "addresses_review_findings": [],
  "revisit_reason": "",
  "assignment": {"work_ids": [], "proposition_ids": [], "task": "", "done_test": ""},
  "handoff_id": null,
  "performer": {"role": "", "delegated": false, "identity": "as reported, or unknown", "identity_basis": "reported | declared | unknown"},
  "started_at": "",
  "finished_at": "",
  "provenance": {"query_ids": [], "opened_sources": [{"source_id": "", "location": "", "observed": ""}], "access_failures": [{"location": "", "error": ""}]},
  "payload": {"claims": [], "sources": [], "search_log": [], "unknowns": [], "proposition_updates": [], "contradictions": [], "notes": ""},
  "review": {
    "reviewer": {"identity": "", "independence": "different-provider | separate-session | self-review", "unavailable_preferences": [{"level": "", "reason": ""}]},
    "sources_opened": [],
    "checks": [{"name": "", "found_defect": false, "evidence": ""}],
    "frame_alignment": {"answers_approved_question": true, "within_scope": true, "preserves_exclusions": true, "supports_end_state": true, "material_conflict": false, "explanation": ""},
    "outcome_delivered": {"delivered": true, "evidence": ""},
    "finding_ids": []
  },
  "dispositions": [{"target_id": "", "disposition": "accepted | repair | insufficient-support | contradicted | contested | frame-conflict", "finding_ids": [], "note": ""}],
  "delta": {"material": true, "summary": "", "claims_added": [], "claims_changed": [], "propositions_changed": [], "unknowns_resolved": [], "unknowns_added": [], "sources_added": []},
  "usage": {"tokens": null, "basis": "measured | estimated", "source_or_method": ""}
}
```

Only record a source in `opened_sources`, or mark it `opened: true` in the ledger, when a tool actually opened it in this run. Record failed opens, such as access denied or a paywall, in `access_failures`.

## publication-source-vN.json

The frozen snapshot the report is generated from. Include every claim and source that the findings, coverage, verdicts, falsification results, contested or rejected items, alternatives and unknowns reference, and the ledger `history` entries that explain changes to them.

```json
{
  "interface": "research-run-publication-source-v2",
  "slug": "",
  "version": 1,
  "frozen_at": "",
  "frame_revision": 1,
  "frame": {"topic": "", "question_answered": "", "target_end_state": "", "intended_audience": "", "scope_in": [], "scope_out": [], "evidence_bar": "", "information_to_explain": [], "clarifications": [], "token_budget": {}, "approval": {}},
  "research_status": "complete | stopped-at-budget | stopped-by-blocker",
  "clearest_answer": "",
  "findings": [],
  "coverage": [],
  "verdicts": [],
  "falsification": {"stands": [], "narrowed": [], "contradicted": [], "attacks": [], "revalidations": []},
  "decisions": [],
  "questions_for_user": [],
  "validation_plan": {},
  "unknowns": [],
  "contested": [],
  "rejected_or_contradicted": [],
  "alternatives": [],
  "negative_evidence_appendix": {},
  "claims": {},
  "sources": {},
  "reviews": [{"round": null, "stage": "", "reviewer": "", "independence": "", "checks": [], "findings": [], "dispositions": []}],
  "execution": {"rounds": [], "revisits": [], "handoffs": [], "degraded": [], "failures": []},
  "budget": {},
  "budget_history": [],
  "possible_next_directions": [{"direction": "", "why": "", "estimated_tokens": null}],
  "change_history": [],
  "change_note": ""
}
```

## registry.json

The shared index at the root: `{"interface": "research-run-registry-v1", "entries": [...]}`. Add this run's entry when its folder is created and replace it, matched by `slug`, whenever the run's status or version changes. Keep entries sorted by slug. Never delete or edit other runs' entries.

```json
{"slug": "", "title": "", "statement": "", "status": "", "version": 1, "updated_at": "", "last_verified": "", "report": "report-v1.html", "aliases": [], "alternatives": [], "park_reasons": [], "verdicts": []}
```

## Handoff packet

Every delegated or resumed step receives a packet like this. Include the content the receiver needs (claim statements, excerpts, source details) whenever it cannot read the ledger itself. Every packet carries these approved frame fields verbatim, including the intended outcome. The adversary's packet leaves out earlier authors' reasoning notes and tells it to break claims whatever outcome would be welcome.

```json
{
  "handoff_id": "H1",
  "run_folder": "",
  "role": "",
  "frame": {"question_answered": "", "target_end_state": "", "intended_audience": "", "decisions_supported": [], "scope_in": [], "scope_out": [], "evidence_bar": "", "freshness_window_days": null, "verdict_dimensions": [], "information_to_explain": [], "definition_of_done": "", "constraints": []},
  "assignment": {"work_ids": [], "proposition_ids": [], "task": "", "done_test": "", "stage_checks": []},
  "build_on": {"finding_ids": [], "claim_ids": [], "source_ids": [], "unknown_ids": [], "review_finding_ids": [], "query_families_used": [], "completed_work": ""},
  "budget": {"slice_tokens": null, "run_cap_tokens": null, "run_used_tokens": null, "run_used_basis": "", "remaining_before_reserve": null, "report_reserve": null},
  "return": "a round payload in the round-file shape, with usage; report any conflict with the frame instead of changing direction"
}
```

The receiver builds on the recorded work: it does not restart the investigation, widen the scope, redo work another agent owns, or plan as though the full cap were unspent.

Record a handoff in `state.handoffs` only when a sub-agent or separate session was actually started, setting `at` when it starts and `returned_at` when its work comes back. Work the coordinator does itself is recorded in its round file with `delegated: false`, even when it was briefed with a packet.

## Budget accounting

- Record usage for every round, handoff, review and the report phase. Prefer measured figures from the environment and name where they came from; otherwise estimate, name the method, and err high.
- Keep measured and estimated totals separate in `state.json`; `remaining_before_reserve.basis` says which kind the figure rests on.
- Add delegated agents' reported usage to the run totals when their work returns. When an agent cannot report usage, estimate it and label it.
- Record every change of plan in `budget.adjustments`: what was deferred, cut or reprioritized, and why.
- These figures are accounting, not enforcement. Do not describe them as limits the environment applies.

## Continuation records

- Add an entry to `state.continuations` with the intent, the user's input, the continuation's cap and the resulting version.
- A reframe updates `frame.json` as a new revision, saves `frame-vN.json`, and records the reason.
- Record the user's answers as claims with source class `user-first-party` (confidence at most medium) linked to the questions they answer, or as `known_facts` in the frame. Mark answered questions in `blockers`.
- Keep earlier snapshots and reports. The next snapshot and report use the next version number.

## Runs made by earlier tooling

Older runs in the same root use the earlier `-v1` record forms and may also contain `reality-vN.md`, `frame-approval-vN.json`, `round-NN-<stage>-judge.json`, `presentation-vN.json`, `report-vN.json`, `packet-vN.html`, `README.md` and a `native/` folder. Leave all of those files as they are.

When continuing such a run, convert its three working records before the first write, so the run keeps one current record set:

1. Copy `frame.json`, `state.json` and `ledger.json`, unchanged, to `frame-legacy.json`, `state-legacy.json` and `ledger-legacy.json` in the same folder.
2. Update each working file in place to the `-v2` form, keeping every existing field and value and adding what is new:
   - `frame.json`: add `token_budget`, `plan` and `approval`, taking the approval from the latest `frame-approval-vN.json` and `reality-vN.md`.
   - `state.json`: keep the existing `rounds` rows and `versions` entries; add `budget`, `budget_history`, `work_queue`, `handoffs`, `continuations`, `failures`, `research_status` and `report_status`.
   - `ledger.json`: keep every record and every `*_history` array. Give each claim a `review_status` derived from `judge_status`. Give each source `opened: true` only where a round's provenance lists its URL as opened; otherwise set `opened: null` and put the earlier `fetched` value in `access_note`. Copy `judge_findings` into `review_findings` with disposition `note`, and start `history` for new changes.
3. Earlier tooling marked every claim filed by a round contested once that round's single repair was rejected. That status reflects the repair limit, not the evidence: set those claims' `review_status` to `pending`, record why in `history`, and re-examine each one on its own evidence, including the review findings against it, before a finding relies on it.
4. New round files, snapshots and reports use the current forms and the next free round and version numbers.

## Execution failure

- Before each new round, confirm that the capabilities it needs still work.
- If search or page access fails across the board, the run folder cannot be read or written, or a delegated agent cannot be reached, stop starting research. Keep the exact run, round and work IDs, save the work in hand, and record the failure in `state.json`.
- Tell the user the failed operation, the observed error, the affected work, the last saved record and what needs repair. Leave out secrets.
- Do not continue with another provider, in another location, or through repeated unattended retries.
- After a confirmed repair, resume the same run and the same work IDs. Check any round that was in progress when the failure happened, and do not redo merged work.
- A source that cannot be opened, a disputed claim or a review rejection is a research outcome, not a failure. Elapsed time alone never triggers this procedure.
