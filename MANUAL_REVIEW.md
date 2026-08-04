# Reviewing what the bot excludes

`update_library.py` pulls every OpenAlex work matching *spiny mouse* / *spiny mice* /
*Acomys* in the title or abstract, then applies relevance heuristics. Those heuristics
are conservative and will sometimes drop a paper that belongs.

Every exclusion is written to **`excluded_report.json`** on each run, with the reason.
Nothing is dropped silently.

## Adding a paper the filter rejected

1. Open `excluded_report.json` and find the entry.
2. Copy its `doi` into the `dois` list in `manual_include.json`.
3. Commit. The next run ingests it and it stays in from then on.

A DOI in `manual_include.json` bypasses **all** relevance heuristics, so it is also the
way to keep something the filter would otherwise re-reject every week.

## Why papers get excluded

| Reason | Meaning |
|---|---|
| `only N mention(s) in abstract, none in title` | The genus appears once. Usually a passing citation, occasionally a central paper — **this is the bucket worth reviewing.** |
| `no abstract available anywhere (unevaluatable)` | Neither OpenAlex, Crossref, nor PubMed had an abstract, so relevance could not be judged. |
| `neacomys (different genus)` | *Neacomys* is a South American genus, unrelated. |
| `data repository (figshare/zenodo)` | Dataset deposit, not a paper. |
| `GBIF occurrence download` | Biodiversity record export. |
| `cyrillic title` / `CJK title` | Non-Latin metadata, generally OpenAlex duplicates. |
| `off-topic title (likely metadata mixup)` | Title unrelated to biology; OpenAlex metadata error. |

## Worked example

*Type III Collagen Regulates Matrix Architecture and Mechanosensing during Wound
Healing* (J Invest Dermatol 2024) names *Acomys* once in its abstract, so it was
excluded as `only 1 mention(s)`. It is a core spiny-mouse wound-healing paper, so its
DOI now sits in `manual_include.json`.
