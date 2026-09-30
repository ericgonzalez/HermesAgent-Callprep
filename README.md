### Sales Call Preparation and Research

**Give it a company name. Get a sourced, citation-tagged PDF brief ready for the call.**

`sales-call-prep` is a [Hermes Agent](https://hermes-agent.nousresearch.com/docs) skill that runs the ten-stage *call-prep route* end to end: it researches the account, pulls pain points from earnings calls, maps the buying committee, and renders the whole thing as a formatted PDF brief — account brief, discovery questions, a specific two-sentence opener, likely objections, competitor landscape, a five-objection battle card, and a follow-up plan.

**No invented facts.** Every claim in the brief is tagged `sourced` (with a citation) or `inferred` (reasoning, labeled as such). Anything that cannot be verified becomes an explicit gap with an ask the rep can act on ("paste her last 3 posts") — never a plausible-sounding guess. The skill never bypasses logins or paywalls.

![The ten-stage call-prep route](skills/sales-call-prep/sales-call-prep-flowchart.png)

## The route

| # | Stage | What you get |
|---|-------|--------------|
| 01 | Account brief | One-pager: business, customers, priorities, dated recent changes |
| 02 | Pain from earnings calls | 3 challenges with short verbatim quotes (speaker + call) and where your solution fits |
| 03 | Contact's role | What the title cares about *now*, with public evidence |
| 04 | Discovery questions | 5 open questions + follow-ups, each traced to a finding |
| 05 | A specific opener | Two sentences anchored on one real, dated, sourced fact |
| 06 | Buying committee | 4–6 roles with how each judges you; names only when publicly verified |
| 07 | Likely objections | Top 3 with responses and their basis |
| 08 | Competitor landscape | 2–3 competitors (status quo counts), pricing, and gaps |
| 09 | Battle card | 5 objections: real concern, response, proof point, ask-back |
| 10 | Follow-up plan | One draft email (<120 words) + 3 value-adding touches tied to findings |

Plus a **call-day snapshot** written last and shown first: the 3–5 takeaways most likely to change how the call goes.

[Sample output — fictional company, 8 pages](skills/sales-call-prep/sample-call-prep-brief.pdf)

## Install

### Option A: Hermes skills tap (recommended)

```bash
hermes skills tap add ericgonzalez/HermesAgent-Callprep
hermes skills search "sales call prep"   # find the identifier
hermes skills install <identifier>
```

### Option B: manual

```bash
git clone https://github.com/ericgonzalez/HermesAgent-Callprep /tmp/callprep
cp -R /tmp/callprep/skills/sales-call-prep ~/.hermes/skills/
pip install reportlab
```

**Requirements:** Python 3 and `reportlab` (used only to render the PDF). No network calls from the script itself.

## First run

Say, for example:

> Prep me for a call with Acme Corp.

The skill asks **one batched question** to fill in `references/seller-profile.md` (your company, solution, approved proof points, pricing rules, discovery framework) and offers to save the answers so every future brief reads them. Until then, solution-fit content is tagged `inferred` and proof points say "Needs rep input".

## How to use it

- `/sales-call-prep Acme Corp` — or just a bare company name in a sales context
- "Research Acme for a sales call", "brief me on Acme", "build a battle card for Acme"

Output lands in `~/call-prep/<company-slug>/`:

```
~/call-prep/acme-corp/
├── payload.json                     # machine-readable brief, all sections
└── acme-corp-call-prep-2026-09-30.pdf
```

The reply highlights the three findings most likely to change the call, the gaps that need your input, and states that the follow-up email is a **draft** — nothing is ever sent automatically.

## Repository layout (skill tap)

```
├── README.md
├── LICENSE
└── skills/
    └── sales-call-prep/
        ├── SKILL.md                  # the skill (frontmatter + procedure)
        ├── references/
        │   ├── prompts.md            # the ten stages: prompts, fallbacks, "done" criteria
        │   └── seller-profile.md     # YOUR profile — fill in on first run
        ├── scripts/
        │   └── build_brief_pdf.py    # JSON payload → formatted PDF (validate + build)
        ├── assets/
        │   └── sample_payload.json   # complete fictional payload, exact schema shape
        ├── sales-call-prep-flowchart.png
        └── sample-call-prep-brief.pdf
```

This repo is a [Hermes skill tap](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills#publishing-a-custom-skill-tap): add it with `hermes skills tap add ericgonzalez/HermesAgent-Callprep` and install any skill under `skills/` — more skills can be added to this repo over time.

## Design principles

- **Stop rule:** gated or missing source → recorded in `gaps[]` with a concrete ask. Never guessed, never paywalled.
- **Sourced vs inferred:** every claim carries a tag; inference is never dressed up as fact.
- **Quotes are short and exact:** at most two sentences verbatim, with speaker, call, and link — or they're dropped and tagged.
- **No invented proof points:** battle-card proof points come only from the seller profile or cited sources.

## License

MIT — see [LICENSE](LICENSE).
