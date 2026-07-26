# Contributing

Corrections are welcome. So is disagreement with a verdict, as long as a primary source comes with it.

All data lives in one file, [`data/flows.json`](data/flows.json), pretty-printed so pull-request diffs stay readable. The viewer reads it at runtime; there is no build step and no dependencies.

## The short version

1. Every amount carries a currency, a date, a status and a primary-source URL.
2. **Never estimate, model or derive a figure.** If it is not published it is `null` with `disclosed: false`.
3. Status words are not interchangeable: `pledged`, `committed`, `contracted`, `disbursed`, `spent`.
4. EU money, member-state money and private capital are different things. Commission *authorisation* of national aid is not EU spending.
5. `jurisdiction` is parent incorporation, not site location.
6. Watch for double-counting in headline investment figures.
7. A warning marker means reported but unconfirmed, and it stays until a primary source removes it.
8. No individual is named anywhere in the data. Entities and instruments only.

## The one rule: never estimate

This is the rule the whole dataset depends on, and it is the one most likely to be broken with good intentions.

There is no "roughly 60% of a compute contract is silicon" in this repository, and there must never be. That figure is not published by anyone. Inventing it, or deriving it from a comparable contract, or scaling it from a GPU list price, would turn a sourced ledger into a model with a citation stapled to it -- and the whole value of the thing is that it is not that.

When an amount is not public:

```json
{ "role": "supplier", "entity": "Nvidia", "jurisdiction": "US", "amount": null, "disclosed": false }
```

That renders as a dashed grey pipe reading "not disclosed". It is a first-class state. It must never render as zero, and it must never be quietly omitted to make a chain look tidy.

### A confirmed zero is not an undisclosed amount

The two look similar in a spreadsheet and mean opposite things. `amount: null` with `disclosed: false` says the public record stops here. `amount: 0` with `disclosed: true` says a source positively establishes that nothing flows -- and that is frequently the most interesting cell in the row.

```json
{ "role": "funder", "entity": "Dutch State", "amount": 0, "currency": "EUR",
  "disclosed": true, "status": "declined", "date": "2026-03-31", "src": "Rijksoverheid" }
```

Seven nodes currently carry a confirmed zero: the Dutch State and EuroHPC against VOLT, German public funds against the AWS European Sovereign Cloud, Portuguese and EU funds against Nscale at Sines, and EU money, Portuguese state money and the PIN fast-track against Start Campus as landlord. Each is sourced. They render in red as `EUR 0` with a "confirmed zero" marker, never as a blank or a dash, because "we checked and the answer is none" is a finding and "we do not know" is not.

Sometimes a split is not merely unpublished but **unknowable by construction** -- MeluXina-AI was tendered as a single undivided lot, so no hardware-versus-integration split exists to be disclosed. Record that as a fact in `not_public`, and still leave the amount `null`.

## Status vocabulary (`status`)

| Value | Means |
|---|---|
| `pledged` | Announced intent. No contract, no budget line. |
| `committed` | Budgeted or board-approved, not yet contracted. |
| `contracted` | Signed contract, or an adopted state-aid decision. |
| `disbursed` | Money confirmed paid. |
| `spent` | Money confirmed spent. |
| `none` | A source positively establishes that no money flows. Amount is `0`. |
| `declined` | The funder was asked and refused, or let the route lapse. Amount is `0`. |
| `not applicable` | No award was possible, so there is nothing to disclose. Amount is `0`. |

A EUR 20bn programme with EUR 1bn actually programmed is `pledged` at 20bn or `committed` at 1bn, depending on which figure you are recording -- never `contracted` at 20bn. A pledge running to 2040 is not capital expenditure and must not be totalled with capex.

## Whose money (`jurisdiction`, and the funder node)

`jurisdiction` is **where the parent is incorporated and taxed.** Not where the datacentre stands, not where the server was screwed together, not where the sales office is.

Three separate things get confused constantly and this ledger keeps them apart:

- **EU money** -- the Union budget, Digital Europe, EuroHPC's Union share.
- **Member-state money** -- national treasuries. Aid the Commission *authorises* under Article 107(3)(c) TFEU is member-state money. The EUR 5bn behind ESMC Dresden is German federal money, not EU spending, and coding it otherwise is the most common error in this subject area.
- **Private capital** -- including state-linked foreign capital, which is neither EU nor member-state money and should be labelled for what it is.

## Test verdicts (`tests`, `test_notes`)

| Verdict | Means |
|---|---|
| `pass` | The published criteria, read against public facts, are met. |
| `fail` | They are not met, on a specific identifiable criterion. |
| `conditional` | Met only where a derogation or implementing act applies. |
| `n/a` | The test does not apply to this kind of case at all. |
| `unassessed` | It would be an overreach to assert a verdict on the public record. |

`n/a` and `unassessed` are **not** the same and must not be swapped. `n/a` says the question is not meaningful here; `unassessed` says the question is meaningful and we are declining to answer it without better evidence. Absence of a key altogether means no reading has been made, which is also not a pass.

Verdicts are this ledger's reading of published criteria. They are not certifications, and nothing here should be represented as one. Prefer `unassessed` over a clever inference. Where a verdict turns on a specific clause, put the reasoning in `test_notes` and quote the clause in the test's `deciding_clause`.

## Quoting legal text

Verbatim clauses are quoted **character for character** against the primary document, not against a news summary or a secondary analysis. Check the quote again before it ships.

The CADA Annex II hardware carve-out does more work than any other sentence in this dataset. If one word of it is wrong the argument built on it collapses, so it gets re-checked against Council document ST-10104-2026-ADD-1 on every change that touches it.

## Double-counting and inflated headlines

Announced investment figures are routinely inflated by construction, and the inflation is usually documented in the announcement itself. Known traps:

- Microsoft's UK "USD 30bn" is USD 15bn capital expenditure plus USD 15bn operating expenditure.
- Amazon's UK GBP 40bn explicitly includes an earlier GBP 8bn. Do not add them.
- Grid availability figures get quoted as if they were committed IT load. They are not the same thing.
- Programme ambition, authorised aid and disbursed aid are three different numbers and headlines mix them freely.

## Warning markers (`caveat`)

A `caveat` on a node, a test or a related entry means **reported but unconfirmed**. Registry-derived shareholdings, consultation drafts, second-hand accounts of restricted documents, and figures that do not reconcile all get one.

A caveat is removed only by a primary source, never by repetition in the trade press. Current examples: registry-derived Campus AI shareholdings, SecNumCloud's capital caps which are verified only in a consultation draft, and the unreconciled JUPITER contract figures.

## Data shape

```
_meta            title, snapshot month, the standing note on undisclosed amounts, status vocabulary
VARIANTS         the six things called sovereignty, and which tests measure each
TESTS            the eight sovereignty tests, each with its deciding clause and legal status
CASES            the traced money flows
CONTRADICTIONS   archetypes where the tests disagree with each other
```

A case carries `chain` (the nodes, in role order: `funder`, `instrument`, `recipient`, `supplier`), `profit_lands`, `not_public`, `tests`, and `confidence`. The aggregate case additionally carries `line_items`, `excluded` and `never_approved`, because a total that does not show its arithmetic is not worth publishing.

Before opening a pull request, check that every `tests` key resolves to a `TESTS[].id`, that arithmetic still reconciles where a total is asserted, and that no em-dash or en-dash has crept in (the house style is a double hyphen).

## No individuals

Entities and instruments only. Ownership, control and money are the subject; named people are not, even where they are public figures and even where a public source names them. If a person's role is load-bearing to a claim, describe the office rather than the holder.

## How calls are made

The maintainer's call is final and is made against this document. That is not a claim to be right -- it is so that the basis for a call is written down and can be argued with. A pull request that shows the rubric was applied wrongly, or that the rubric itself is wrong, is the most useful kind.
