# Document cleanup playbook

Agent-facing instructions for this repository. Read this before processing a source
document. The source itself is always the authority on content.

---

## 1. The rule that matters most

**The cleaned file must contain exactly the same text as the source document.**

This repository is an archive of legal texts. Their value depends on being a faithful,
verifiable copy. Whatever you do to the layout, the words, the order of clauses and the
numbering must survive untouched. The safest default is character-for-character
fidelity; the only tolerated deviations are the ones listed in section 7.

Everything else in this playbook exists to serve that one goal.

---

## 2. What this repository is

| Location                | Contents                                                         |
| ----------------------- | ---------------------------------------------------------------- |
| `STATUTEN.md`, `HHR.md` | Current version of each document                                 |
| `HISTORIE/`             | One file per adopted version, named `<DOCUMENT>-<YYYY-MM-DD>.md` |
| `README.md`             | Inventory of all files; update it when versions are added        |
| `.gitattributes`        | Locks text files to LF                                           |

`STATUTEN.md` and `HHR.md` are byte-identical copies of the newest file in
`HISTORIE/` (currently `STATUTEN-2025-07-18.md` and `HHR-2026-06-01.md`). Keep the
twins in sync, or tell the user they diverged.

**Why the formatting rules below are so strict:** the whole point of this archive is
that a diff between two versions shows *only* changes in the legal text. If files are
formatted canonically, the diff is text-only. If they are not, the diff drowns in
irrelevant layout noise. Structure and Markdown rendering are therefore free to change
- that is the entire reason the cleaning exists.

---

## 3. Priorities, in order

1. **Text fidelity** - the legal content, verbatim.
2. **Canonical structure** - so diffs between versions are text-only.
3. **Typographic consistency** - quotes, dashes, spacing.
4. **Cosmetics** - indentation, blank lines.

When a lower priority conflicts with a higher one, the higher one wins.

---

## 4. Golden rules

- **Text is the source's, not yours.** Never rewrite, reorder, merge, split, summarise
  or "improve" a sentence.
- **Structure is yours.** You may freely add/remove Markdown markers, re-wrap lines and
  normalise indentation to make the rendering canonical.
- **Follow the document's own skeleton.** Reproduce *its* hierarchy and numbering
  scheme. Do not impose a template, and do not assume a new source looks like the last
  one; formats vary (heading styles, numbering styles, page furniture, columns).
- **Never invent anything** - no text, articles, dates, numbers, titles or source links.
  If something is missing or ambiguous, leave it and flag it.
- **Typos are a conversation, not a decision.** See section 7.
- **Keep the empty-list-item placeholder** `N. .` (section 5.6).
- **Flag, don't guess.** An honest "I could not determine X" is far more useful than a
  confident wrong reconstruction.

---

## 5. Target style

This is the *output* style. The input may look nothing like it.

### 5.1 Skeleton

```markdown
# Statuten
Versie 2025-07-18

## Inhoudsopgave

- [Statuten](#statuten)
  - [Inhoudsopgave](#inhoudsopgave)
  - [Artikel 1 Begripsbepalingen](#artikel-1-begripsbepalingen)

## Artikel 1 Begripsbepalingen

1. Eerste lid, op één regel, niet hard-wrapped.
2. Tweede lid met subleden:
   - a. een sub-lid;
     - i. een sub-sub-lid;
```

- Title: `# Statuten` or `# Huishoudelijk Reglement`.
- Version line directly under the title, no blank line between: `Versie YYYY-MM-DD`,
  using the date of the deed/decision.
- `## Inhoudsopgave` with a nested anchor list. Article headings are
  `## Artikel <n> <Titel>`, with a blank line before and after.
- Anchor slugs: lowercase, drop punctuation (`.`, `/`, `!`), spaces to `-`.
  `Artikel 16 Afdelingen/werkgroepen van de vereniging` becomes
  `#artikel-16-afdelingenwerkgroepen-van-de-vereniging`.
- HHR sub-headings are their own paragraph in italics: `_schorsing en ontslag_`.

### 5.2 List levels

| Level              | Marker  | Indentation                            |
| ------------------ | ------- | -------------------------------------- |
| Numbered lid       | `N. `   | none                                   |
| Letter sub-lid     | `- a. ` | 3 spaces                               |
| Roman sub-sub-lid  | `- i. ` | 5 spaces                               |
| Unordered sub-item | `- `    | 5 spaces (3 when directly under a lid) |

One clause = one line. Do not hard-wrap at a column width.

### 5.3 Typography

- Straight quotes `'` and `"` - never curly quotes.
- Hyphen-minus `-` for dashes - never en/em dash.
- UTF-8 **without BOM**, Unicode **NFC**.
- LF line endings; exactly one trailing newline.
- No tabs, no trailing whitespace, no two consecutive blank lines.
- No double spaces *inside* a line. Leading indentation for nested list levels and
  padding inside Markdown tables are structural and therefore fine.

### 5.4 Emphasis

Mirror the source's emphasis, e.g. in the HHR:

```markdown
1. **De vereniging**: In dit reglement wordt verstaan onder ...
   - a. **moties**: tot uiterlijk veertien dagen voor de vergadering;
```

### 5.5 Numbering

Reproduce the adopted numbering, and keep numbered lists **sequential** (see 6). If the
source skips or repeats a number, that is a finding to report - not something to
silently fix.

### 5.6 Empty numbered items - `N. .`

Markdown has no syntax for an empty list item. When a numbered clause has no lead-in
text of its own and only introduces sub-items, write:

```markdown
2. .
   - a. Beslissing omtrent de toelating wordt genomen door het bestuur.
```

The lone `.` keeps the item alive so it renders as `2.` above its sub-items. **Never**
"tidy" this into a bare `2.` - that breaks the list.

---

## 6. Markdown must be formatter-stable

Because the diff quality is the product, the Markdown must be canonical enough that
running a standard formatter (Prettier, `mdformat`, `markdownlint --fix`) produces **no
changes**. A formatter that rewrites the file would create exactly the layout noise this
whole exercise is meant to eliminate.

To be formatter-stable:

- one consistent list-marker style and indentation per level (section 5.2);
- **sequential** numbering in ordered lists - formatters renumber them, so any
  non-sequential numbering will be rewritten (another reason to reproduce the source's
  numbering faithfully);
- exactly one blank line between blocks; a single blank line after a heading;
- no trailing spaces, tabs, or multiple consecutive blank lines;
- consistent emphasis markers;
- no raw HTML, no trailing-space hard breaks, no non-breaking spaces.

**Verify it, don't assume it:** run your formatter and confirm `git diff` is empty.
Check that the `N. .` placeholder survives your formatter; if it does not, adapt the
approach rather than dropping the convention.

---

## 7. What you may change, and what you must not

| You may change freely                                       | You must leave alone                                  |
| ----------------------------------------------------------- | ----------------------------------------------------- |
| Markdown markers, indentation, blank lines                  | the words themselves                                  |
| Line wrapping / re-flow                                     | the order of clauses                                  |
| Typographic punctuation (curly -> straight, en dash -> `-`) | clause and article numbering with legal meaning       |
| Removal of page furniture (section 8)                       | letter/roman sub-markers                              |
|                                                             | anything you suspect is a typo, until the user agrees |

**Typo's.** You may correct a spelling or grammar slip that is clearly accidental -
*after* the user has agreed. Report every candidate individually (what it says, what you
think it should say, why), and never bundle such fixes silently into a "cleanup". If the
user is not available, leave the text exactly as the source has it and list the
candidates in your report.

**Punctuation.** Normalising quote and dash characters is expected and does not change
legal meaning, so it may be done without asking. Anything beyond that (missing `;`,
extra words, doubled words) is text and follows the typo rule above.

---

## 8. Converting a source document

Sources vary a lot and change over time. Treat what follows as a **general method**, not
a checklist of known formats.

**Method**

1. **Preserve the raw source first.** Copy it somewhere safe before you touch anything;
   you may need to re-derive your work.
2. **Normalise the container:** to UTF-8 without BOM, CRLF -> LF, NBSP -> space, strip
   zero-width and control characters, and undo extraction artifacts (padding runs,
   ligatures, soft hyphens).
3. **Discover the document's own skeleton:** title, version/date, the top-level division
   (articles, sections, chapters...), and its numbering schemes for clauses and
   sub-clauses. Record it, then follow it. Do not force it into the shape of an earlier
   document.
4. **Remove page furniture and non-document matter:** page numbers, headers/footers,
   running titles, watermarks, scan noise, cover pages, preambles, signature and
   "issued as a copy" blocks. Decide by asking whether the text is part of the document
   itself.
5. **Re-flow:** join wrapped lines into one line per clause. Never merge two clauses, and
   never split one.
6. **Emit** the canonical Markdown of section 5.
7. **Verify** (section 9) before reporting.

**Watch out for** (examples, not an exhaustive list): page breaks that split a sentence,
a word, or a numbered clause mid-way; hyphenation across line breaks; multiple columns;
footnotes and marginalia; definitions lists (`term: description`); bullet glyphs other
than `-` (such as `*` or `o`); roman/letter/number mixtures; all-caps headings; tables;
inline bold/italic; unusual heading levels. Repair the *break*, never the text.

If a source's structure does not map cleanly onto the target style, stop and ask rather
than inventing a convention.

---

## 9. Verification

Prove the text is unchanged with a word-stream comparison between the source and your
output.

```python
import re

def strip_markers(text):
    prev = None
    while prev != text:
        prev = text
        text = re.sub(r"(?m)^[ \t]*#{1,6}[ \t]+", "", text)      # headings
        text = re.sub(r"(?m)^[ \t]*-[ \t]+", "", text)           # bullets
        text = re.sub(r"(?m)^[ \t]*\d{1,2}\.?[ \t]+", "", text)  # N. lids
        text = re.sub(r"(?m)^[ \t]*[a-z]\.[ \t]+", "", text)     # a. sub-lids
        text = text.replace("**", "").replace("_", " ")
    return text

def normalize(text):
    return re.sub(r"\s+", " ", strip_markers(text)).strip()

raw = open("RAW", encoding="utf-8").read().replace("\r\n", "\n")
out = open("OUT.md", encoding="utf-8").read()

a = normalize(raw[raw.index("Artikel 1"):])
b = normalize(out[out.index("## Artikel 1"):])
assert a == b, next(i for i, (x, y) in enumerate(zip(a, b)) if x != y)
```

- The `N. .` placeholder adds a `.` the source does not have - drop those lines before
  comparing, or ignore punctuation.
- Typographic changes (curly -> straight, en dash -> `-`) are expected differences.
- Also confirm the mechanical facts: no CRLF, no BOM, NFC, one trailing newline, no tabs
  or trailing spaces, and `git check-attr text eol -- <file>` reporting `text: auto` /
  `eol: lf`.
- Finally, run a formatter and confirm it changes nothing (section 6).

---

## 10. Finishing up

1. **Update `README.md`** - add the new file under `Inhoud` in chronological order, with
   a `[\[bron\]]` link *only* if you were actually given one. Never invent a URL.
2. **Sync the twin** if the new document is the current version (`STATUTEN.md` /
   `HHR.md`).
3. **Report** what changed, what you deliberately left alone, every typo candidate, and
   anything the user should verify.

### Definition of done

- [ ] Text proven identical to the source (word-stream check passes).
- [ ] Formatter-stable: a formatter run produces an empty diff.
- [ ] UTF-8, no BOM, NFC, LF, single trailing newline, no tabs/trailing spaces.
- [ ] Title + `Versie YYYY-MM-DD` + TOC present, anchors resolve.
- [ ] One clause per line; document's own numbering reproduced; `N. .` preserved.
- [ ] Typography normalised; page furniture removed.
- [ ] `README.md` updated; source link only if known.
- [ ] Report lists every deviation from the source, including typo candidates.