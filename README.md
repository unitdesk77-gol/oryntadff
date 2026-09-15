# Orynta reconciliation workspace

A responsive bank/Tally reconciliation application. Production audience must remain owner-private unless the owner explicitly approves sharing.

## Run and test

Requires Node 20+ for core tests and Python 3 for the development web server. No runtime third-party JavaScript dependencies.

```
npm test
npm start
```

Open http://localhost:3000.

## Implemented

- Bank CSV/TSV import with explicit column mapping, preview, strict calendar-date and integer-paise validation, and duplicate-candidate exclusion. Invalid rows block the import rather than silently dropping errors.
- Financial-year filters, transaction search, categories, tenant names, exact Tally ledger mapping, individual and bulk edits, and review approval.
- Preserved original bank references, dates and amounts; exported tenant labels suffix the original reference in parentheses.
- XML import of actual Tally bank ledger entries; unique reference/date/amount matching with explicit confirmation and one-to-one match controls.
- Receipt/payment XML export for reviewed records. Matched and previously exported records are blocked from export, and exported records are locked against edits. Tally-side reimport of a downloaded file is outside this control.
- Browser-local persistence, CSV export, JSON backup/restore and a local activity history.
- Responsive desktop/mobile layout and keyboard-accessible controls.

## Material boundaries

This is a single-owner browser-local application, not yet a multi-user accounting SaaS. Data does not sync across devices. Browser data must be backed up before clearing storage. The activity log is useful operational history, not tamper-proof audit evidence. Do not use synthetic data as financial evidence.

The app supports CSV/TSV bank statements, not Excel/PDF directly. Save Excel statements as CSV first. No OCR, automatic rent allocation, receipt splitting or financial-year shifting is performed.

There is no live connection to the user's Tally desktop. XML export is not reported as successful sync. Validate generated vouchers in a backed-up test company and use existing exact ledger names. Tally-side import acceptance has not been verified against an actual installed Tally instance. Integrating a secure desktop bridge with acknowledgments and duplicate-safe retries is a subsequent milestone.

For several bank accounts or companies, carefully review the selected account and exact company/bank ledger before export; this release has one active Tally mapping. Import fingerprints identify duplicate candidates; identical genuine payments need source review.

## Next production milestones

1. Authenticated database persistence with company-level isolation, role permissions and server audit trails.
2. Bank-specific XLSX/PDF import parsers and balance reconciliation.
3. Desktop Tally bridge with company verification, voucher acknowledgment, conflict resolution and idempotent retries.
4. Automated browser regression suite in CI and recovery testing.

## Deployment

Static assets live in `public/`. The hosted URL must use owner-only access. Source code may be stored in the existing GitHub repository; never commit bank statements, backups, credentials or real transaction records.
