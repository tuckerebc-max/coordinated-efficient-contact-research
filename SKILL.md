---
name: coordinated-efficient-contact-research
description: Build accurate, current, source-backed names, roles, emails, phones, and official contact routes for organizations in a roster. Use when researching school systems, boards, agencies, associations, companies, nonprofits, or other organization lists at scale; when a 5.6 Luna-style coordinator should delegate disjoint batches to efficient Mini workers; or when outputs must be deduplicated, verified, resumable, and honest about missing information.
---

# Coordinated Efficient Contact Research

## Purpose

Turn an organization roster into a practical contact dataset through coordinated, time-boxed public-source research. Use a strong coordinator/integrator (for example, 5.6 Luna at medium effort), efficient independent workers (for example, 5.4 Mini), and a final verification pass that resolves conflicts and strengthens evidence. Optimize for useful verified coverage, not fabricated completeness.

## Operating model

1. Inspect the roster, prior outputs, and any handoff notes. Identify the authoritative organization key: a supplied ID is preferred; otherwise use a stable jurisdiction/state ID plus normalized organization name. Never merge on organization name alone when a stronger key exists.
2. Normalize the input without destroying original values. Preserve source row numbers, original names, IDs, state/jurisdiction, and any existing status or contact fields.
3. Create a manifest of disjoint batches, normally 10-25 organizations per worker. Give every worker a unique batch ID and a unique temporary output path. Workers must not edit a shared workbook or another worker's file.
4. Send routine batches to Mini workers. Keep the coordinator focused on routing, schema enforcement, deduplication, conflict review, queue management, and synthesis. Escalate only conflicts, high-value organizations, ambiguous identity matches, partial rosters, and final QA to higher reasoning effort.
5. Merge worker outputs only after structural and evidence validation. Keep the raw worker files, manifest, progress log, source/evidence ledger, unresolved queue, and final outputs separately.
6. Freeze a resumable checkpoint at the deadline even when some organizations remain queued, blocked, or unresolved.

## Public-source and privacy boundaries

- Use public official websites, official directories, current staff or leadership pages, public rosters, agendas/minutes, public filings, and attributable public PDFs.
- Do not email people, submit contact forms, log in, pay, bypass CAPTCHAs or paywalls, or use private data.
- Do not use social profiles as evidence unless the user explicitly changes the policy and the source is appropriate for the task.
- Workers may generate an inferred email candidate from a naming pattern, domain convention, guessed initials, or another person's address, but must store it separately as an unverified candidate.
- Never present an inferred candidate as a confirmed direct address. Promote it to `directEmail` only after a current public source confirms that the exact address is formally used and attributable to the named person. Confirmation may come from an official staff/leadership page, official PDF, official directory, official correspondence, or a current public page on the organization's domain that visibly uses the exact address in a way that identifies the person.
- Google/search results are discovery aids, not evidence by themselves. Open the underlying public source, record its URL, and explain the confirmation in notes. A syntactically valid address or a match to another employee's pattern is not confirmation.
- A mailto link is usable only when the link target exposes the exact address or the page clearly attributes that exact address to the named person.
- Keep a general office, department, clerk, seat, or board route in a separate route field. Label it as an office/department/seat route; never present it as the person's direct email.
- If the evidence is insufficient, use `Not publicly located` or the task's approved unresolved value. Missing information is an acceptable result.

## Research order for each organization

Use the shortest path to reliable contactability:

1. Official organization leadership, directory, board, staff, or contact page.
2. Official roster or management platform linked from the organization (for example, BoardDocs, Simbli, eBoards, or an official directory).
3. Official agenda, minutes, packet, filing, or PDF with visible names, roles, and/or exact addresses.
4. Official state, regulator, association, or parent-organization directory for identity context or a lead; corroborate against a current organization source before treating a contact as current.

Time-box routine research. Capture exact emails and obvious official routes first. For a difficult organization, use at most two targeted searches or about 90 seconds on the first pass, then place it in the unresolved or recheck queue instead of spiraling.

## Worker contract

Each worker receives only its assigned organizations and the minimum project context needed to research them. It returns one JSON or CSV artifact per batch, with one row per verified named contact and, when available, one separate office/department route row.

Use the field set in [references/evidence-and-output-schema.md](references/evidence-and-output-schema.md), adapting labels to the roster while retaining the required concepts. At minimum, preserve:

- batch ID and source row number;
- authoritative organization ID, jurisdiction, and name;
- contact category, name, title/role;
- exact direct email, if visibly published;
- best public office/department route;
- official phone and URLs;
- email-source URL and other evidence URLs;
- verification status, research date, and concise notes.

Workers must return explicit empty/unresolved values rather than omitting required fields. They must not overwrite prior adequate evidence except to resolve a documented conflict.

## Coordinator QA gates

Run these checks after every worker and again before export:

1. Schema: required fields exist; IDs and source row numbers are valid; dates are parseable; URLs are complete and plausible.
2. Identity: the source organization matches the roster key; do not silently merge similar names, campuses, branches, or jurisdictions.
3. Evidence: every exact email has an attributable source URL; office routes are not placed in direct-email fields; old or secondary sources are not treated as current without corroboration.
4. Inference audit: every candidate email has its own candidate field, inference basis, confirmation status, and evidence URL. Reject candidates that cannot be confirmed or are supported only by pattern matching.
5. Status: distinguish current named contact, office-only route, partial evidence, role not identified, queued, and blocked. Do not upgrade status because an email merely looks syntactically valid.
6. Duplicates: deduplicate by authoritative organization ID + normalized contact name + role/category. Preserve genuinely distinct roles and sources.
7. Contradictions: compare source dates, official-domain evidence, title/role agreement, and direct attribution. Keep both sources in the ledger and explain the resolution in notes.
8. Coverage: compare the manifest, roster, worker outputs, and unresolved queue. Every input organization must end as researched, queued, blocked, or explicitly unresolved.
9. Integrity: count exact emails, office routes, named contacts, partial rows, and unresolved organizations independently. Never claim that office-route coverage equals direct-email coverage.

Requeue only empty, contradictory, especially valuable, or office-only batches that are likely to improve materially. Do not re-research every row merely to polish wording.

## Deliverables

For a workbook request, produce a resumable workbook with the closest available equivalent of:

- `Contacts` or `Organization Contacts`: one row per named contact or office route;
- `Organization Summary`: one row per input organization with coverage and best route;
- `Source Review` or source/evidence ledger: source URL, source type, accessed date, claims supported, and review notes;
- `Progress`: batch status, worker, counts, timestamps, and retry/requeue reason;
- `Audit`: validation results, duplicate decisions, unresolved queue, and final counts.

Keep source URLs as visible plain-text cells. Keep raw worker artifacts and manifests alongside the final workbook. If the user requests CSV/JSON instead, provide the same separation of row data, evidence, progress, and unresolved records.

## Handoff and resume protocol

At start, read existing manifest, progress, source ledger, and checkpoint outputs. At resume, process queued and partial organizations first, then only unresolved/conflicted records. Preserve stable IDs and prior evidence. Write a new checkpoint rather than silently replacing the prior one.

At cutoff, export the merged result even if work remains. Report: input organizations, researched organizations, named-contact rows, exact direct emails, office routes, queued organizations, partial/unresolved rows, blocked rows, and the date/source policy used.

## References

Read [references/evidence-and-output-schema.md](references/evidence-and-output-schema.md) when defining a new roster schema, validating worker output, or building the final workbook. Use the main workflow above for all other cases.
