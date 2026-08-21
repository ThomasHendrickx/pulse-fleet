# Observed export formats, from two real statements (2026-08-17)

Structural facts only, extracted by the orchestrator from two examples the
owner provided in session. The raw documents are NOT committed anywhere:
they carry personal data and both repositories are public. This note is the
durable, anonymized record for plan review and for M1-P2's
verification-first step.

## Example 1: Belfius current account statement (PDF)

- Bank: Belfius. Product: BEATS STAR current account. Language: Dutch.
- Both provided examples are PDF statements, not CSV. Whether CSV exports
  are also available and preferred is an open owner question at the time
  of writing.
- Header per statement: account IBAN and BIC, statement date, page as
  statement-number/page (for example 7/1 .. 7/5), opening balance with
  date (SALDO OP dd-mm-yyyy, sign and amount), closing balance with date
  and time on the last page.
- One transaction block per movement: a four-digit per-account sequence
  number (observed 0319 through 0357, continuous), booking date
  dd-mm-yyyy, value date in parentheses (VAL. dd-mm-yyyy), signed amount
  with comma decimals and dot thousands (observed -30.941,50 and
  +42.000,00 shapes), then a multi-line free-text description.
- The statement-plus-sequence natural key HOLDS for this format: statement
  number plus per-account sequence number identifies a row; sequence
  numbers alone appear continuous across statements within a year.
- Description sub-patterns observed: debit card payments (with embedded
  merchant, city, country, amount repeat, masked card number, cardholder
  name), instant and ordinary credit transfers (counterparty IBAN and
  name, free text), European direct debits (creditor reference, mandate,
  period), standing orders, account fees, interest settlement with a
  separate annex page (BIJLAGE BIJ VERRICHTING nnn), and a card
  settlement debit line naming the card statement number (MASTERCARD
  AFREKENING NUMMER nnn), which is the account-side leg of the card
  settlement correction.

## Example 2: KBC-Mastercard expense statement (PDF)

- Bank: KBC, a SECOND bank next to Belfius. Product: credit card
  (Uitgavenstaat). Language: Dutch.
- Header: customer reference, statement number (Uitgavenstaatnummer), card
  limit, masked card number, period of the statement (from/to dates).
- Rows: transaction date, settlement date (datum verrekening),
  description, EUR amount. NO per-row sequence number exists in this
  format: the statement-plus-sequence natural key FAILS here and the
  content-hash fallback is mandatory.
- Non-EUR transactions carry sub-lines with the original amount and the
  exchange rate (observed USD and GBP). Amounts settle in EUR; original
  currency is description metadata, not a second currency in totals.
- The statement carries the previous balance, a positive settlement row
  (DOMICILIERING VIA JE BANK) mirroring the account-side settlement
  debit, and a closing total with the future settlement date. Duplicate
  same-day same-amount same-merchant rows occur legitimately (observed
  twice in one statement), which any dedup key must not collapse.

## Additional facts verified by the M0-P2 scout (2026-08-21)

Structure only, measured in-container against the same two statements
(pdfplumber text extraction to the session scratchpad; nothing copied
out). Recorded for the v0.2 plan and its reviewers.

1. BOTH PDFs carry a full machine-readable text layer. No OCR is needed
   for either format; a text-extraction library suffices.
2. Belfius: 5 pages; the fingerprint header text appears on every page;
   transaction blocks span pages 1 to 4; page 5 is the interest annex
   (body starts with the BIJLAGE marker) and contributes zero
   transactions. The balance identity holds EXACTLY in integer cents:
   opening SALDO OP + sum of all 39 parsed transaction amounts equals
   closing SALDO OP. Sequence numbers are consecutive within the
   statement. Four band lines carry the IBAN between dashes.
3. Belfius amount shapes: spaced sign ("- 3,55" shape) observed only on
   amounts without a thousands dot; tight sign observed BOTH with and
   without a thousands dot. The addendum's "space disappears on large
   amounts" is directionally right but not a strict rule; a parser must
   accept every combination.
4. KBC: extraction is TOLERANCE-SENSITIVE. Default/layout-mode text
   extraction glues words together (labels like the previous-balance and
   period lines come out with no spaces); a tight word-gap tolerance
   restores real spacing. Any extractor must reconstruct lines from
   positioned text items deliberately rather than trusting library
   default text assembly, and template matching must not depend on exact
   interior spacing of labels.
5. KBC balance semantics: the "Totaal bedrag van de kaartverrichtingen
   op <date>" line carries NO amount; the settlement amount sits on the
   following "Afrekening via je bank op <date>" line, and that amount
   uses a SPACE as thousands separator (unlike Belfius dot-thousands).
   The identity that holds exactly in integer cents is: previous balance
   (Vorig saldo) + sum of ALL transaction rows including the
   DOMICILIERING credit = the Afrekening amount.
6. KBC: the DOMICILIERING VIA JE BANK credit row equals the NEGATED
   previous balance exactly (it is the settlement of the prior statement
   arriving on the card). 22 transaction rows in the observed statement;
   4 FX transactions each followed by two continuation lines (original
   amount, exchange rate); exactly one identical duplicate row pair
   (same transaction date, description and amount) occurs legitimately.
7. KBC masked card number appears as four groups ("9999 99XX XXXX
   9999" shape) in both the header and the per-card sub-heading.

## Consequences already visible

1. The plan's M1-P2 hash-fallback stance is confirmed as the guaranteed
   path; the sequence key is a Belfius-account optimization only.
2. Two banks means at least two source profiles from the first real
   import, and the card-versus-account settlement pairing appears on both
   sides with linkable references.
3. RESOLVED by the owner (2026-08-17): both CSV and PDF are available
   from the same sources; the PDFs were mobile exports, the PC export is
   CSV. CSV remains the v1 input and the charter premise stands. Two
   consequences kept: the CSV column layouts still need verification
   against real PC exports during M1-P2 (the PDFs verify content and
   keys, not CSV columns), and PDF import is a parked idea only, since
   the owner's habitual mobile export produces PDFs the v1 importer will
   not accept.
