# cloud-itonami-lei-5493003uoetfyronlg31

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by RELIANCE INDUSTRIES LIMITED.**

This repository archives the publicly published Privacy Policy of **RELIANCE INDUSTRIES LIMITED** (IN), with source-url and retrieval-date provenance, per
ADR-2607110300 (`cloud-itonami-lei-corporate-tos-catalog`, `com-junkawasaki/root`).
Read-only reference/archive repository — not a governed Advisor/Governor actor.

- LEI: `5493003UOETFYRONLG31` (GLEIF entity status ACTIVE, registration ISSUED)
- Source: https://www.ril.com/privacy-policy
- Retrieved: 2026-07-25T05:18:31Z
- SHA-256 of archived text: `7658ab1d172bd1b748494bc4ac6ddfe0946eb6cfbe4e1ca6f60b91fdb13ff578`

Acquired by `scripts/lei-acquire.cljs` as part of the worldwide-broadening
continuation that followed the 2026-07-25 coverage audit, which found the
catalog's real reach was 27 countries with the United States at 55%.

## Verified public-register citations

`facts/catalog.edn` records 58 citations for this legal entity, drawn from **four
independent authorities**: GLEIF, Companies House (United Kingdom), the National
Stock Exchange of India, and the issuer's own website. Every row was retrieved
before it was written down; nothing in that file is asserted from memory.

Check them against the live web:

```sh
nbb tools/verify_citations.cljs facts/catalog.edn --min 50
```

Exit codes are three, not two — "nothing was checked" must not look like
"nothing was wrong":

| exit | meaning |
|---|---|
| 0 | every citation fetched, every substring present, floor met |
| 1 | answered, and at least one citation is wrong (`DRIFT <id> <why>`) |
| 2 | **could not answer** — catalog missing/unparseable, zero rows, or fewer rows than `--min` |

### Read `:cite/row-kind` before you read the claims

Not every row identifies this company, and the file says so per row:

- **`:identity`** (29) — names the entity, its identifiers, or a specific related
  legal entity. Repoint the URL at another company and the row fails.
- **`:attribute`** (21) — true of this entity but **not only** of it (country `IN`,
  status `ACTIVE`, legal form `DDKQ`). Such a row can survive a swap to a similar
  company. Recorded because it is checked and true, not because it identifies.
- **`:definition`** (8) — resolves a code another row uses (`RA000394`, `DDKQ`) or
  fixes a CSV's column labels. Says nothing about Reliance.

Treating all 58 as identifying would overstate what the catalog proves. Two rows
were reclassified from `:identity` to `:attribute` because the swap test below
**measured** them surviving — the taxonomy is checked, not asserted.

### The home register is not cited, and that is the main limitation

GLEIF names `RA000394` — the Companies Register of India's Ministry of Corporate
Affairs — as both the register of record and the validation authority. It answers
HTTP 403, so **CIN `L17110MH1973PLC019786` is attested by GLEIF alone.** What the
catalog corroborates independently is the *group* (Companies House prints the UK
subsidiary's name, number and registered office that GLEIF gives) and the *listed
company* (NSE's equity list names it, with a domestic ISIN GLEIF does not carry).
Five further sources were tested and rejected — each answered 2xx while carrying
no register data — and are named in `:catalog/unreachable` rather than dropped
quietly.

### How this gate was shown to discriminate

Measured, not assumed:

| break | result |
|---|---|
| unmodified | exit 0, `CHECKED 58 OK 58 FAIL 0` |
| one substring falsified (CIN `…786`→`…787`) | exit 1 naming **only** `gleif-registered-as` |
| GLEIF URL repointed at INFOSYS LIMITED, a real Indian near-twin | exit 1: **all 11** `:identity` rows fail, 10 of 15 `:attribute` rows survive — exactly what the two kinds claim |
| Companies House URL repointed at TESCO PLC | exit 1, all 8 UK rows fail |
| host made unresolvable | exit 1 `fetch-error`, not a silent pass |
| catalog missing / empty / unparseable | exit 2 |
| `--min 100` against 3 rows / `--min 3` against the same 3 | exit 2 `FLOOR`, then exit 0 |
