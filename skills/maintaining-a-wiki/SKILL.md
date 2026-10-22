---
name: maintaining-a-wiki
description: Keeps a research wiki in markdown (customer interviews, desk research, competitor notes) by ingesting sources into cited pages, answering questions from those pages, and linting for contradictions and stale claims. Based on Karpathy's LLM Wiki. Use when sharing a source to file, asking a question the wiki should answer, or asking to check the wiki, or run /maintaining-a-wiki.
disable-model-invocation: true
---

# maintaining-a-wiki

**What it does:** Compiles your sources into a wiki once and keeps it current, so questions get answered from cited pages instead of re-reading raw files every time. You curate sources and ask questions; the skill does the bookkeeping. Based on Karpathy's [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).
**Not in scope:** finding sources on its own, editing a source after it's saved, publishing the wiki anywhere.

**Rule: never write or change a page before the user has seen the takeaways (ingest) or the punch list (lint) and said yes.**

Where your wiki and its sources live, and what pages may name, live in [setup.md](setup.md), next to this file. Step 1 of every job fills it on first run.

## Layers

- **Sources** are immutable snapshots, one file per source, named `YYYY-MM-DD-<slug>.md` and dated by when the thing happened or was published. Frontmatter: `source:` (URL or original location), `fetched:` (today), `type:` (article / talk / interview / desk-research / notes). The body is the source text, unedited. Whatever is wrong gets corrected in a page, never in the snapshot. Each source gets an ID in `index.md` (`S1`, `S2`, …).
- **Pages** are what you write: flat in the wiki root, kebab-case names, one idea each. Every claim ends with a citation to its source, e.g. `[S2 00:23:09]` for a timestamp or `[S5 p.3]` for a page, as reference-style links. Link related pages to each other.
- **`index.md`** is the schema plus the catalogue. Top section: scope, page types, any rule specific to this wiki. Below it: the sources table, then one line per page (`- [Title](page.md) — one-line summary`), grouped by page type. Read it before touching pages. It is how you find things.
- **`log.md`** is append-only. One entry per job: `## [YYYY-MM-DD] ingest | <title>`, `query | <question>` or `lint`, with one line on which pages changed.

**Three layers at most:** source, then page, then an optional synthesis page. A synthesis page combines several pages into something none of them holds, cites those pages, and quotes the source where the wording matters. Anything new starts from pages or sources, never from a synthesis page.

**Build from the primary text.** When a source comes with an AI summary (meeting-notes summaries, a search tool's answer), read the full transcript or page and write from that. Use the summary only as a table of contents: summaries misattribute speakers and drop the sharpest lines. Keep short quotes in the speaker's own words, so a page carries how things were said, not just what.

**Mark judgment.** Anything that's your interpretation rather than a source's claim gets a **J** marker. Readers must be able to tell the two apart.

## Steps

### 1. Check setup (every job)

Read `setup.md`. If any field is empty, ask the user for it in one message, show the filled file, and write it on a yes. If the wiki root doesn't exist yet, create `index.md` and `log.md` there on the first ingest, not before.
Done when: every field in `setup.md` is filled or marked "none".

### 2a. Ingest

1. **Snapshot.** Save the source as described under Layers, in the sources location from setup. If it already lives somewhere you can link (a shared drive, a doc), give it an ID in `index.md` with its link instead of copying it. Done when: the source has an ID and a link that opens.
2. **Takeaways.** Give the user 3–5 takeaways, plus which pages you'll create or change. Wait for a yes or a redirect: the user's steer on what matters is the point of this step.
3. **Integrate.** Write the pages. Every page the source touches gets updated, not just a new summary page. Where the source contradicts an existing page, keep both claims with their citations and mark it `**Contradiction:**`. The user resolves it. Done when: every takeaway lands on a page with a citation.
4. **Bookkeeping.** Update `index.md` and append to `log.md`.

### 2b. Query

Read `index.md`, open the pages it points to, and answer with page and source citations. Go back to raw sources only when the pages can't answer, and name that gap: it's a finding. Offer to file the answer as a page only when it adds something no page holds; otherwise link the pages.
Done when: every claim in the answer has a citation, or is marked **J**.

### 2c. Lint

Report a numbered punch list. Change nothing until the user says yes, by number. Check every page for:

- **contradictions** between pages
- **stale** claims: superseded by a newer source, a dated fact that has passed, or a claim about the present resting on an old source. Propose removing these. Dated history ("2021 revenue") stays.
- **orphans:** pages with no inbound link and missing from `index.md`
- **uncited** claims
- **names** of people, where setup says pages name roles only
- **gaps:** a concept mentioned on several pages with no page of its own
- **depth:** a page built on a synthesis page or an AI summary instead of the source

Done when: every page was checked against every item. After the approved fixes, append a `lint` entry to `log.md`.

## Example

Hivelist customer research, wiki at `research/`, pages name roles only.

**User:** "Ingest this interview" (transcript of a call with Hivelist's ops lead at a logistics customer).

**Step 2a.1:** saved as `sources/2026-09-25-ops-lead-interview.md`, ID S3.

**Step 2a.2 takeaways:**
1. Exports are rebuilt by hand every Friday, about 2 hours a week ("we copy it into Excel and fix the dates") [S3 00:12:40]
2. Notifications are muted by most of the team, so overdue tasks go unseen [S3 00:21:05]
3. **Contradiction:** the `export-needs` page says CSV is enough; this customer needs XLSX with formatting [S3 00:14:10] vs [S1 00:08:30]

Pages: update `export-needs` and `notifications`, new page `manual-workarounds`. Go ahead?

**User:** "Yes, but skip the notifications point."

*(Pages written, `index.md` and `log.md` updated, contradiction left marked for the user.)*
