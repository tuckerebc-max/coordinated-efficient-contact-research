# Evidence and Output Schema

## Required concepts

Adapt field names to the user's roster, but retain these concepts in the final data:

| Concept | Guidance |
|---|---|
| `batchId` | Unique worker/coordinator assignment. |
| `sourceRowNumber` | Original roster row for traceability. |
| `organizationId` | Authoritative supplied ID; use a jurisdiction ID if available. |
| `jurisdiction` | State, country, region, or other governing area. |
| `organizationName` | Formal roster name plus normalized comparison form in audit data. |
| `contactCategory` | Named contact, board member, executive, office, department, clerk, etc. |
| `name` | Person's full name, or a literal office label for a route row. |
| `title` | Exact current title when published. |
| `directEmail` | Exact attributable email only; otherwise `Not publicly located`. |
| `candidateEmail` | Optional inferred address retained for verification; never treat as a confirmed contact. |
| `candidateBasis` | Pattern, domain, initials, or colleague-match explanation for the candidate. |
| `candidateConfirmationStatus` | `UNCONFIRMED`, `CONFIRMED`, or `REJECTED`. |
| `bestPublicRoute` | Office/department/seat route, with its type explicitly labeled. |
| `phone` | Official organization or office phone, not an unverified personal number. |
| `organizationUrl` | Official organization/contact page. |
| `leadershipUrl` | Official leadership, roster, or membership page. |
| `emailSourceUrl` | Page or PDF visibly publishing the exact direct email. Blank when none. |
| `additionalEvidenceUrl` | Supporting official source, if needed. |
| `verificationStatus` | Controlled status from the table below. |
| `researchedAt` | Date the evidence was checked. |
| `notes` | Short evidence statement, limitations, and route labeling. |

## Recommended verification statuses

- `VERIFIED - CURRENT`: current official source visibly identifies the named contact and directly attributes the exact email, or a current named contact is verified with no email when the task's policy allows that distinction.
- `CANDIDATE - UNCONFIRMED`: an inferred email is retained as a lead, but it is excluded from usable-email and coverage counts.
- `VERIFIED - OFFICE CONTACT ONLY`: official source supports an office, department, clerk, or seat route, but not the named person's direct email.
- `PARTIALLY VERIFIED`: organization or role context is official/current, but the named contact, title, or exact email is incomplete or link-obscured.
- `ROLE NOT IDENTIFIED`: an official organization route was found, but a current named contact or role could not be established.
- `UNRESOLVED`: no sufficient current official evidence was found during the time-box.
- `BLOCKED`: access or source conditions prevented completion; explain the reason without bypassing controls.

## Evidence note pattern

Use concise, auditable notes such as:

`Official current leadership page identifies [name] as [title] and visibly publishes [email].`

`Official organization page publishes [office/department/seat route]; no direct address for [name] is displayed.`

`Official domain and organization identity confirmed; current named contact/email not publicly located in first pass.`

Do not write notes that imply an email was tested, delivered, or confirmed by sending a message.

## Inferred-email confirmation

An inferred candidate can guide targeted searching, but it is not evidence. Search the exact candidate in quotation marks with the person's name and organization, then inspect the underlying public result. Confirm only when a current public source formally uses that exact address and ties it to the person. If confirmed, copy the exact address into `directEmail`, retain the candidate and confirmation URL in the audit fields, and use `VERIFIED - CURRENT`. If no confirmation is found, retain the candidate only as `CANDIDATE - UNCONFIRMED` or mark it `REJECTED`; leave `directEmail` as `Not publicly located`.

## School-board adaptation

For school boards or school committees, use `Board Member`, `Board Office`, `Chair`, `President`, `Vice Chair`, or the locally used equivalent. A board-office or clerk address remains an office route even when the page says it distributes messages to the full board. A source that displays a person's name and a clickable email without exposing the exact address supports the person/role but not an exact direct email unless the target is attributable and visible.

## Final audit counts

Report these independently:

1. input organizations;
2. organizations with at least one researched record;
3. named-contact rows;
4. exact direct emails;
5. office/department/seat routes;
6. queued, partial, unresolved, and blocked organizations;
7. rows missing source evidence or failing schema checks.
