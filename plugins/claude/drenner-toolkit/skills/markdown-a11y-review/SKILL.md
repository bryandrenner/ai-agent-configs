---
name: markdown-a11y-review
description: Accessibility review checklist for Markdown covering image alt text, heading hierarchy, link text, lists and emoji, and plain language. Use only when the user asks for an accessibility or a11y review of a Markdown file, e.g. "check this README for accessibility". Not for routine Markdown writing or editing.
---

# Markdown accessibility review

Checks Markdown against GitHub's [5 tips for making your GitHub profile page accessible](https://github.blog/developer-skills/github/5-tips-for-making-your-github-profile-page-accessible/). For each issue, name who is affected (e.g. screen reader users, people with cognitive disabilities, non-native speakers) and give a specific fix.

## Checks

Sections are in priority order. Report higher-priority issues first.

### Image alt text

- Empty alt text (`![](image.png)`), unless the image is decorative.
- Alt text that is a filename (`img_1234.jpg`) or a placeholder (`screenshot`, `image`).
- Alt text that leaves out text visible in the image.
- Alt text that starts with "image of". Screen readers already announce the image. "Screenshot of" is fine.
- Complex images (charts, infographics): summarize the data in the alt text and put the longer description in nearby text or linked content. Use `<details>` only where the document's Markdown flavor allows it (see `drenner-toolkit:markdown`).

### Heading hierarchy

- More than one H1. Where the platform generates the H1 from frontmatter, content starts at H2.
- Skipped levels, e.g. `##` followed by `####`.
- Bold text used instead of a heading.

### Link text

- Generic text ("click here", "here", "this", "read more", "link"). Assistive technology can list links out of context, so the text has to make sense alone.
- The same link text pointing to different destinations.
- Bare URLs in prose.

### Lists and emoji

- Emoji or special characters used as bullets instead of list syntax.
- A sequence of items written as prose that should be a list.
- Several emoji in a row. Screen readers read each emoji's name aloud.
- Meaning carried only by an emoji and not by the text.

### Plain language

- Jargon, long sentences, passive voice, or dense paragraphs that could be split into sections or lists.

## Tone

- Present alt-text and plain-language changes as recommendations. Only the author knows the image content, the audience, and the tone they want.
- Keep the author's voice and personality.
