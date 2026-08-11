# Where the sovereign euro goes

An opinionated, sourced trace of who actually receives European public money for AI and compute infrastructure -- and which definition of "sovereign" that money is being measured against.

Companion to [Europe's sovereign full stack](https://map.sovereigntymonitor.eu/), which maps *where* Europe is sovereign. This one follows the money.

It is a snapshot (**July 2026**), built from primary sources, with editorial judgement in every test verdict. The point of publishing it is to make those judgements correctable: if a figure or a verdict is wrong, [open a PR with a source](CONTRIBUTING.md).

**Live:** `https://ledger.sovereigntymonitor.eu/`
**Straight to the punchline:** add `#gap` to the URL.

## The two things it shows

**Follow the euro.** Twelve named cases, each traced across five stations: funder, instrument, recipient, supplier, and the jurisdiction where the profit finally books. The flag changes at the last two stations in almost every case. That change is the product.

**Which definition?** Eight tests that each claim to define sovereignty, held against five archetypes. They disagree with each other, and the disagreements are not accidents. Send an all-Nvidia estate from SEAL-4 to CADA level 4 and watch it go from failing to passing, because [CADA Annex II](https://data.consilium.europa.eu/doc/document/ST-10104-2026-ADD-1/en/pdf) puts hardware outside the scope in its own second sentence.

## "Not disclosed" is a finding, not a missing value

Fifty-five places in this dataset record an amount as `null` with `disclosed: false`. They render as dashed grey pipes, and they are the most important thing here.

A dashed pipe does not mean zero, and it does not mean nobody has filled in the spreadsheet. It means the public record stops at that point. JUPITER, MeluXina-AI and Mistral Compute all have fully documented funding chains that go dark at exactly the same station: the line item that would show what Nvidia was paid. Three separately-sourced chains, the same gap, the same place.

**Nothing in this repository is estimated, modelled or derived.** There is no "roughly 60% of a compute contract is silicon" anywhere in it, because that number is not published and inventing it would destroy the only thing the dataset is good for. Where a split cannot be inferred even in principle -- MeluXina-AI was tendered as a single undivided lot -- that is recorded as a fact about the tender, not filled in.

## How to read it

- **Status words are not interchangeable.** `pledged` is announced intent. `committed` is budgeted or board-approved. `contracted` is signed or adopted. `disbursed` is paid. `spent` is spent. A EUR 20bn programme is not EUR 20bn awarded, and a pledge running to 2040 is not capital expenditure.
- **Jurisdiction is where the parent is incorporated and taxed**, not where the site sits or the box was assembled. A fab in Dresden owned 70% from Taiwan books its profit accordingly.
- **Commission authorisation of national aid is not EU spending.** The EUR 5bn behind ESMC Dresden is German federal money the Commission approved under state-aid rules. Coding it as EU funding is the single most common error in this area and this ledger deliberately does not make it.
- **Test verdicts are this ledger's reading** of published criteria against public facts, not certifications. Where asserting a legal conclusion would be an overreach the verdict is `unassessed` rather than guessed, and where a test genuinely does not apply it is `n/a`. Those are different things and are drawn differently.
- **A warning triangle** marks something reported but unconfirmed -- registry-derived shareholdings, consultation drafts, figures that do not reconcile. Those never graduate to plain assertions without a primary source.

## The cases

MeluXina-AI · ESMC Dresden · VOLT Rotterdam · JUPITER · MareNostrum 5 AI upgrade · AI Gigafactories and InvestAI · AWS European Sovereign Cloud · Mistral Compute Essonne and Campus AI · Nscale and Sines · Chips Act aid to non-EU-parented projects, in aggregate · Polo Strategico Nazionale · Start Campus at Sines.

Sines appears twice on purpose, because two different money chains run through the same site: Nscale's financing as the tenant, and Start Campus's as the landlord. Keeping them apart is what stops the "no public money" finding being counted twice.

Polo Strategico Nazionale is the case that shows the whole pattern in one balance sheet. EU recovery money reaches Italy's national public-administration cloud as customer revenue rather than as a grant; 97.4 percent of the concessionaire's service costs in 2025 were paid to the four shareholders that own it; and the platforms underneath are Oracle, Microsoft, Google and AWS, with no published figure for any of them.

VOLT is in here precisely because it refutes the obvious hypothesis. No Dutch and no EU public money reaches it: the cabinet declined the compute offtake that EuroHPC rules require before a bid can be made. Confirmed private capital is "more than ten million euro" against announcements of EUR 5bn to 22.5bn. A ledger that only confirms its author's suspicions is not worth publishing.

## Viewing it locally

The viewer loads `data/flows.json` at runtime, so browsers block opening `index.html` straight off disk (`file://`). Either use the live link above, or run a local server in the repo folder:

```sh
python -m http.server 8000
# then open http://localhost:8000/
```

## Deep links

`#case-<id>` opens one chain, `#test-<id>` opens one sovereignty test, and `#gap` opens the undisclosed view. Case and test ids are in [`data/flows.json`](data/flows.json).

## Contributing

Corrections are welcome, and disagreement with a verdict is a legitimate contribution -- with a primary source behind it. The data lives in one file, [`data/flows.json`](data/flows.json), pretty-printed so diffs stay readable. Read [CONTRIBUTING.md](CONTRIBUTING.md) first: it covers the sourcing requirement, the status vocabulary, and the never-estimate rule that the whole dataset depends on.

This is a maintained snapshot, not a live register. The maintainer's call is final, and the rubric it is made against is written down so it can be argued with.

## Structure

```
index.html          the viewer (no build step, no dependencies)
data/flows.json     the data: cases, chains, tests, variants, contradictions
CONTRIBUTING.md     sourcing rules, status vocabulary, the never-estimate rule
LICENSE             MIT (the code)
LICENSE-DATA.txt    CC BY 4.0 (the data)
```

## Licence

The code (`index.html`) is [MIT](LICENSE). The data (`data/flows.json`) is [CC BY 4.0](LICENSE-DATA.txt) -- reuse it freely, with attribution. Each claim carries its own source link; those sources belong to their publishers.

**Fonts are self-hosted.** Caveat and Patrick Hand are [SIL Open Font License](fonts/OFL-Caveat.txt); the licence texts sit alongside them in `fonts/`. They are served from this repository rather than from a font CDN, so the page makes no third-party request of any kind. On an asset about technology sovereignty that seemed worth getting right.

## Maintainer

Built and maintained by Reidar Balstad, RB Consult AB. LinkedIn: https://www.linkedin.com/in/rbalstad/ Sources are public Commission, Council, EuroHPC and national government documents, company filings and announcements, and credible trade press as of July 2026. No individual is named anywhere in the data: entities and instruments only.
