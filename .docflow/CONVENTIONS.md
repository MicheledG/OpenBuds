# Conventions

## Project

Project name: OpenBuds.

Artefact root: `.docflow/` — `adr/`, `plan/`, `INDEX.md`, and this file live
under this root; `AGENTS.md` and `CLAUDE.md` always stay at the repository
root. Every lifecycle skill resolves paths against this root.

Discovery: a `.docflow/` **directory** at the repository root is itself the
artefact root; any other root is named by a one-line `.docflow` **file** at
the repository root (`root: <path>`). Tools resolve the root in that order,
falling back to probing `docs/` then the repository root for this file.
The pointer and the record above must agree.

Assessment depth: `express` — the depth chosen at bootstrap. Skill
assessments pre-select it as the recommended depth; the depth selector
appears when depth is unresolved and material choices remain. Reuse
applicable answers already supplied in the request or session, including
depth; defaults never replace explicit choices. A recorded preference alone
steers the recommendation and is never applied silently or treated as an
execution grant. Change this line to change the recommendation.

Language: en-US throughout — code, comments, docs, ADRs, plan items and
commit messages. User-facing strings may be localised through the
platforms' resource files.

## ADR Files

ADR filenames use `NNNN-kebab-case-slug.md`, zero-padded to 4 digits,
with contiguous numbering and no reserved gaps.

The number is an **integer**; the four-digit zero-padding is a display
convention only — tools sort ADRs **numerically**, not lexically. Related
ADRs may be **grouped** under `domains/<slug>/README.md` without changing
their numbers — grouping is a curated view, not a separate namespace; the
contiguous number stays the single identity.

Each ADR describes one decision. If a decision splits, supersede the
original ADR and create new ADRs rather than expanding scope inside a
single document.

Status lifecycle: `Proposed → Accepted → Implemented → (Superseded | Deprecated)`.

| Status | Meaning |
|---|---|
| Proposed | Draft. Decision authored but not yet approved. |
| Accepted | Decision approved; implementation authorised. Work item lives in `plan/todo/`. |
| Implemented | Code shipped per the completion event. Work item moved to `plan/done/`. ADR is the authoritative spec the running system matches. |
| Superseded | Replaced by another ADR. The successor is named in `superseded-by:` metadata. |
| Deprecated | Was real; the world moved on; no successor. Capability is not being rebuilt. |

Terminal states (Superseded / Deprecated) are reachable from any prior
state.

The first **persisted** status is `Proposed` — there is no separate `Draft`
state and no `brainstorming/`/`drafts/` folder. Work-in-progress lives in
the brainstorm conversation; only an approved decision is written, as a
numbered `Proposed` ADR.

Cross-references link by relative path to `adr/NNNN-*.md`.

## ADR Shapes

This project uses a single ADR shape. ADRs use `adr/0000-template.md`
and contain these sections in order: Context, Capability statement,
User stories / scenarios, Acceptance criteria, Out of scope, Open
questions, References, Revision History, Approvals. No `shape:` field
is written. The template is not a decision: it is excluded from the
catalogue and from `INDEX.md`.

## Public Repository Rule

This repository is public. Nothing that identifies a person, a device or
an account is ever committed — in code, docs, ADRs, plan items, test
fixtures, screenshots or commit messages:

- No Bluetooth MAC addresses, device serial numbers, firmware build ids
  tied to one unit, or custom device names (e.g. "<owner>'s buds").
- No raw Bluetooth captures: HCI snoop logs (`btsnoop_hci.log`),
  `.pcap`/`.pcapng`/`.cfa` files, `adb bugreport` archives. Only the
  decoded protocol facts (opcodes, field layouts, byte meanings) are
  committed, with addresses and identifiers replaced by obviously fake
  values such as `00:11:22:33:44:55`.
- No credentials or account identifiers: signing keys, keystores, Apple
  team ids, notarization passwords, API tokens, cloud account ids.
  Release secrets live in the CI secret store, never in the tree.
- Screenshots are cropped or edited so no personal name, notification or
  address is visible.

`.gitignore` blocks the capture and key formats above; the ignore list is
a backstop, not the rule.

## Trademark and Licence Rule

OpenBuds is an independent project, not affiliated with or endorsed by
Anker Innovations. Product names (Anker, soundcore, Liberty) are used only
to say which devices are supported, never as the project's name, logo or
icon. The code is GPL-3.0-or-later; code derived from another project keeps
its original copyright notice and must have a GPL-compatible licence.

## ADR Privacy

ADRs are internal artefacts. ADR numbers, ADR titles, and the
existence of the ADR catalogue must never appear in any string the
product emits to users: UI copy, API response bodies, error messages,
customer-visible log lines, public documentation, release notes,
marketing copy, or support communications.

Allowed references:
- Inline code comments tying a non-obvious choice to its ADR
  (`// see adr/0042-foo.md`).
- Commit messages and PR descriptions.
- Internal documents: `AGENTS.md`, `INDEX.md`, the `plan/` queue,
  internal runbooks.

Rule of thumb: if a non-builder could ever read the string, the ADR
reference comes out. Refer to the behaviour by its product-level name
instead.

## Multi-Agent Rules

A single writer owns this repo — one human/agent integrates at a time,
so there is nothing to serialise: no roles list, no lock ledger. What
happened is git history and `plan/done/`; what is in flight is the
current branch and any open pull request. There is no `_agent/`
directory: it would hold only `_agent/prompts/autonomous.md`, which is
written once a verify gate is recorded below.
Regenerate `INDEX.md` after any ADR status change or new ADR.

## Plan Folder

Pending and shipped work live in `plan/` under the artefact root:

- `plan/todo/NNNN-<slug>.md` — pending work, lower numbers run first.
  Each file names the owning ADR(s), scope, and exit criteria.
- `plan/done/<YYYY-MM-DD>-<slug>.md` — shipped work, chronological.
  The completion commit moves the item here and its footer names the
  verified implementation HEAD recorded before the completion commit.

The completion event is: fast-forwarded to `main` + remote push succeeded.

The bootstrap seed's adoption record is created with the complete scaffold.
For that seed alone, a direct-integration footer may use this form, with its
actual repository-relative path and host-appropriate quoting:

```text
Shipped by bootstrap introduction: `<repository-relative done path>`.
Resolve: `git log --follow --diff-filter=A --format=%H -- "<repository-relative done path>"`
```

Resolve it after committing to one reachable introducing commit containing
the record, Implemented seed and verified scaffold. A generic bootstrap
label or future/self SHA is not evidence. Ordinary completion footers name
an existing verified implementation SHA.

When a `plan/todo/` item ships, the file moves to `plan/done/`, any
plan-item Status section is removed, the owning ADR(s)' `status:`
advances from `Accepted` to `Implemented`, and `INDEX.md` is
regenerated to match. These changes are committed before the final push.

## Audit Trail Policy

Every substantive change to an Accepted ADR appends a new row to its
Revision History table.

Editorial changes — typos, formatting, link fixes — are excluded from
Revision History but must be flagged "editorial" in the commit message.

The Approvals table is populated when an ADR transitions to Accepted
and updated on every subsequent substantive revision.

## Git Contract

Commit messages follow Conventional Commits with a mandatory
`Rationale:` footer for any commit that touches an ADR.

- Signed commits: no (no signing key on the authoring machine).
- ADR-revision tags `adr-NNNN-rN`: no.
- Co-Authored-By trailer on agent commits: yes.

Changes are fast-forwarded onto `main`; no merge commits. No verify gate
is recorded yet: the first code item records one here (build + unit
tests for the Rust core and each app). Until then a change is "shipped"
when it is on `main` and pushed.

## Reporting

Final skill results and persisted verification, PR, wave and stop reports
end with a section headed exactly **Status at a glance**, with three labels:

- **This run** — what was attempted, actual outcomes, exact gate output and exit code.
- **Overall** — implemented, partially verified, verified, blocked, failed or unknown.
- **Yet to do** — all remaining work, checks, findings, cleanup and required input; None only when the complete task is verifiably finished.

1. Report exact process outcomes, including timeouts and interruptions.
2. A passing sub-step is not an overall pass; require complete evidence.
3. Missing returns or incomplete evidence remain unknown or partially verified.
4. Do not omit remaining work when a budget or session ends.

The reader must be able to distinguish what was achieved from what is
missing. Routine progress updates remain concise and need no closing block.

## Item status

Every new queue item carries this section, initially empty:

```markdown
## Status

- Claimed by:
- Blockers:
- Stopped:
```

At start, record actor, date and actual branch in Claimed by. The owner
maintains Blockers. On stop, record date and reason in Stopped with the
three Status at a glance labels. Commit status with the work. Remove the
section in the completion move to plan/done; the shipped footer replaces it.
