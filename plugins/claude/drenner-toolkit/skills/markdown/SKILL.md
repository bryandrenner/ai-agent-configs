---
name: markdown
description: Conventions for writing and editing Markdown files, covering which flavor and frontmatter to use, when to add summary or framing sections, maintainer notes, and formatting. Use when creating or substantially editing a .md file such as a README, doc, report, memo, or Obsidian note. Not for chat replies.
---

# Markdown conventions

These conventions keep Markdown documents consistent across code repositories and knowledge projects, and keep notes intended for maintainers separate from content intended for readers.

## Considering the purpose

Documents are either **digests** (living documents that track or accumulate knowledge) or **dispatches** (finalized and sent, e.g. a letter, resume, email, or publication).

If you can't reasonably infer the target audience, or whether the document is a digest or dispatch, then ask before writing.

## Flavor and frontmatter

Take the first branch that matches:

- If it's in an Obsidian vault:
  - Use [Obsidian Flavored Markdown](https://obsidian.md/help/obsidian-flavored-markdown).
  - Apply frontmatter per the vault's conventions.
- Else if it's adjacent to a software implementation or in a primarily code repository:
  - Use [GFM (GitHub Flavored Markdown)](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).
  - No digest frontmatter.
- Else if it's a dispatch:
  - Use portable Markdown: no platform-specific extensions (alerts, wikilinks, embeds, `<details>`).
  - Don't add frontmatter.
- Else regard it as a digest:
  - Use portable Markdown: no platform-specific extensions (alerts, wikilinks, embeds, `<details>`).
  - Add frontmatter with `updated` and `targetAudience`, like this:

    ```md
    ---
    updated: 2026-09-11T14:15
    targetAudience: engineers new to the billing service
    ---
    ```

    - `updated` uses ISO 8601 local time (no offset). Refresh it on every substantive edit. Get the time from the environment (e.g. run `date`). If you can't, write the date only (`2026-09-11`).
    - `targetAudience` names who the document is for.
    - Use camel case for new keys.

## General rules

- Don't manually number headings, because the document already orders them and the numbers are painful to update when sections are inserted or moved.
  - Exception: the section represents a thing with its own number, e.g. a story or epic with an ID.
- Don't render a table of contents unless the document already has one.
- Don't duplicate details, e.g. a lengthy summary table that restates the body. A second representation of the same information must be kept in sync.
- Don't add editorial history or rationale (timestamps of changes, lists of decisions) unless it will prevent a plausible future regression. The document should plainly reflect the current changes and decisions. The frontmatter `updated` key is the one exception.

### Formatting

- Separate sections with headings, not horizontal rules or other decorations.
- Don't hand-format: don't pad table columns, and don't hard-wrap prose (one paragraph per line).
- If the repo configures a Markdown formatter (rumdl, Prettier, markdownlint), run it on only the files you changed. If none is configured, don't run one.

## Framing sections

Framing sections (a summary, background, problem statement, objectives, significance, scope, or definitions) come before the body. Add **none** when any of these holds:

- The document is under ~300 words. Frame it in the first sentence or two instead.
- The reader's request already defines the problem, the questions, and the stakes.
- The document is a list, reference, or lookup material that's already scannable.
- A summary would repeat more than half the body.

Otherwise:

- Add at most one summary section (plus an optional Key Takeaways list for long, data-heavy reports).
- Add each other framing section only if its failure mode would actually occur, and skip it silently otherwise:
  - **Background:** the reader may lack context the body assumes.
  - **Problem Statement:** the reader couldn't state the problem themselves, or it's contested.
  - **Objectives:** there are 2+ distinct questions, or scope could be misread.
  - **Significance:** the audience might doubt the topic deserves attention. Never when they commissioned the work.
  - **Scope:** what's covered could plausibly be confused.
  - **Definitions:** a term could be misread. Otherwise define it inline.
- For an email or memo, open with a bold **Bottom line** sentence and use no section labels.

## Maintainer notes

**Maintainer notes** give rationale and context that prevent future maintainers from making plausible mistakes, and aren't relevant to the target audience. Write them as HTML comments:

```md
<!-- Kept the legacy term here because the API still uses it. -->
```

If a note matters to the target audience, it's an editorial comment and belongs in the visible text. Examples are to-do items or unresolved questions, which tell the reader the document is unfinished. In a WIP document, collect items for editorial consideration in a section at the very end.

Don't leave maintainer notes in finalized dispatches.
