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
