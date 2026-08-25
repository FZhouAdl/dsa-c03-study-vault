# DSA-C03 Study Vault

An Obsidian-based study vault for the **SnowPro Advanced: Data Scientist (DSA-C03)** certification. It combines downloaded official Snowflake material with distilled concept notes, practice questions, and exam-trap cheatsheets.

## What's inside

```
dsa-c03-study-vault/
├── INDEX.md                  # Index of every crawled source, mapped to exam domains
├── sources/                  # Raw source material (study guide + crawled pages)
│   ├── SnowProDataScientistStudyGuide.pdf   # Official exam guide
│   ├── snowpro-data-scientist-study-guide.txt
│   ├── crawl-report.txt      # Crawl log: status per URL (OK / GATED / PDF / ipynb)
│   └── downloads/            # Crawled official Snowflake pages, per domain
└── StudyVault/               # Main study notes (open this folder in Obsidian)
    ├── 00-dashboard/         # MOC, exam traps, quick-reference cheat sheet
    ├── 01-data-science-concepts/                    # Domain 1 (17%)
    ├── 02-data-preparation-and-feature-engineering/ # Domain 2 (27%)
    ├── 03-model-development/                        # Domain 3 (31%)
    └── 04-model-deployment/                         # Domain 4 (25%)
```

- **23 notes**: concept notes per topic, 4 practice sets (44 questions), 5 official sample questions.
- **Tag system**: `#dsa-c03` + `#domain-N` on every note, one registered detail tag per concept note. See the [Tag Index](StudyVault/00-dashboard/moc.md) — don't invent new tags.
- **Weak areas** tracked in the [MOC](StudyVault/00-dashboard/moc.md#weak-areas).

## Getting started

1. Clone the repo.
2. Open **`StudyVault/`** as an Obsidian vault (the `.obsidian/` config tracks vault settings).
3. Start at the [Map of Content](StudyVault/00-dashboard/moc.md).
4. Drill into a domain folder → concept note → its practice set.

## How sources were gathered

Official Snowflake material only — crawled at depth 1 from `*.snowflake.com` and `github.com/Snowflake-Labs`, following links referenced in the official study guide.

- `sources/crawl-report.txt` logs per-URL status: `OK` (saved), `GATED` (login-walled, stub saved), `PDF` (binary), `ipynb` (converted notebook).
- Email/blog-gated stubs (`resources.snowflake.com`, ILT training pages) are excluded from study notes; see the [Non-core Topic Policy](StudyVault/00-dashboard/moc.md#non-core-topic-policy).
- Third-party links in the guide (medium, scikit-learn, etc.) are **not** downloaded and are referenced only as supplementary knowledge.

## Study plan

- Exam weight: Domain 1 17% · Domain 2 27% · Domain 3 31% · Domain 4 25%.
- Guide effort: ~10–13 hours plus practice.
- Use `00-dashboard/exam-traps.md` to avoid common wrong answers and `00-dashboard/quick-reference.md` for a last-pass formula cheat sheet.

## License / legal

All source material is Snowflake's official documentation and the published exam study guide; it is stored here for personal study. Third-party content is not redistributed.