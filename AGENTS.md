# AGENTS.md

This file provides guidance to coding agents working in this repository.

## What this repository is

OpenBuds is an open-source companion app for true-wireless earbuds whose
vendor app is closed — first target: soundcore Liberty Buds 2 (model
D1206). It controls listening modes (noise cancelling, transparency and
their levels), shows battery, and puts those controls where they are
fastest: on Android a Quick Settings tile, home-screen widgets and
notification actions; on macOS a menu bar item, widgets and Shortcuts
actions, installable with Homebrew. The repository is **public** and
starts from an empty baseline: the ADR catalogue records the decisions,
and the plan queue holds the work.

## Picking up this repo

Read these, in order, before any tool calls:

1. `AGENTS.md` (this file) — the hard rules. Read in full.
2. `.docflow/CONVENTIONS.md` — authoring rules, ADR status semantics, the
   public-repository rule, the git contract.
3. `.docflow/plan/README.md` — how the work queue is used.
4. The newest `.docflow/plan/done/` entries and
   `git log --first-parent --oneline -n 20` — the shipped-work record.
5. `.docflow/INDEX.md` — the ADR catalogue: an ADR's filename and its
   dependency chain. Sorted by number, not by work order.
6. The queue item you are about to work, `.docflow/plan/todo/NNNN-<slug>.md`,
   and the ADR(s) it names — both in full.

## Repository structure

- `.docflow/adr/0000-template.md` — canonical ADR template.
- `.docflow/adr/NNNN-<kebab-slug>.md` — one ADR per decision, contiguous
  numbering, no gaps.
- `.docflow/INDEX.md` — table regenerated from every ADR's metadata block.
- `.docflow/CONVENTIONS.md` — authoring rules (read before editing anything).
- `.docflow/plan/todo/NNNN-<slug>.md` — pending work, lower numbers run first.
- `.docflow/plan/done/<YYYY-MM-DD>-<slug>.md` — shipped work, chronological.

## Hard rules for this public repository

- **Never commit personal or device data.** No Bluetooth MAC addresses,
  serial numbers, custom device names, account ids, signing keys or
  tokens — in code, fixtures, docs, screenshots or commit messages.
  Never commit raw Bluetooth captures (HCI snoop logs, pcap files, bug
  reports): commit only the decoded protocol facts, with identifiers
  replaced by obviously fake values. Before every commit, read the staged
  diff (`git diff --cached`) with this rule in mind. See
  `.docflow/CONVENTIONS.md` §Public Repository Rule.
- **Not affiliated with the vendor.** Vendor product names appear only to
  name supported devices — never as the project's name, logo or icon.
- **GPL-3.0-or-later.** Code taken from another project keeps its
  copyright notice, and its licence must be GPL-compatible.

## Hard rules when editing ADRs

These come from `.docflow/CONVENTIONS.md` and override default behaviour:

- **One decision per ADR.** Splits become new ADRs that supersede;
  never expand scope inside an existing one.
- **Status lifecycle:** `Proposed → Accepted → Implemented → (Superseded | Deprecated)`.
- **Capability ADR section order:** metadata → Context → Capability
  statement → User stories / scenarios → Acceptance criteria → Out of
  scope → Open questions → References → Revision History → Approvals.
- **Acceptance criteria are testable and numbered.**
- **ADRs are internal artefacts — never user-visible.** ADR numbers,
  ADR titles, and the existence of the ADR catalogue must NEVER appear
  in any string the product emits to users: UI copy, error messages,
  log lines, public documentation, release notes or store listings.
  References ARE allowed in: code comments (`// see adr/0042-foo.md`),
  commit messages, PR descriptions, `AGENTS.md`, `.docflow/CONVENTIONS.md`,
  `.docflow/INDEX.md`, and the `plan/` queue. Rule of thumb: if a
  non-builder could ever see the string, the ADR reference comes out.

## Implementation work

- Start from the ADRs. Identify which ADRs a code change implements or
  affects before changing behaviour.
- If implementation reveals a capability gap or changed decision, update
  the relevant ADR rather than silently diverging.
- Add or update tests for implemented behaviour. Map tests back to ADR
  acceptance criteria where practical.
- **Do not leak ADR identifiers into user-visible surfaces.** Refer to the
  behaviour by its product-level name in UI copy, errors, logs, the README
  and release notes.

## Audit trail and revision discipline

- Substantive ADR changes append a row to the Revision History table.
  Editorial changes (typos, formatting, link fixes) are excluded but
  flagged `editorial` in the commit message.
- Approvals table populates when an ADR is Accepted and updates on each
  later substantive revision.
- Regenerate `.docflow/INDEX.md` from ADR metadata after any ADR status
  change or new ADR.

## Multi-agent workflow

A single writer owns this repo — one human/agent integrates at a time,
so there is nothing to serialise. What happened is in git history and
`plan/done/`; what is in flight is the branch you are on and any open
pull request.

## Plan folder

- A pending item gets a `.docflow/plan/todo/NNNN-<slug>.md` file BEFORE
  work starts, naming the owning ADR(s), scope, and exit criteria.
- The completion event is: fast-forwarded to `main` + remote push
  succeeded. The completion commit moves the file to
  `.docflow/plan/done/<YYYY-MM-DD>-<slug>.md` with a footer naming the
  verified implementation HEAD SHA, then pushes.
- The owning ADR(s) advance `Accepted → Implemented` on the same
  commit. Regenerate `.docflow/INDEX.md`.

## Git contract

- Commit messages follow **Conventional Commits**.
- Mandatory `Rationale:` footer on any commit touching an ADR.
- Signed commits: no.
- ADR-revision tags `adr-NNNN-rN`: no.
- Co-Authored-By trailer on agent commits: yes.
- Cross-references between ADRs use relative paths (`adr/NNNN-*.md`).
- **Integration:** direct-to-main, **fast-forward only**. No merge
  commits on `main`. No verify gate is recorded yet; the first code item
  records one in `.docflow/CONVENTIONS.md`, and from then on it runs
  locally and must pass before push. Completion event: fast-forwarded to
  `main` + remote push succeeded.

## Reporting

End final results and persisted reports with **Status at a glance**:
**This run**, **Overall**, **Yet to do**, per `.docflow/CONVENTIONS.md`
§Reporting.
