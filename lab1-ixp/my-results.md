# Lab 1 — IXP Vendor Invoice Model

## Project

| | |
|---|---|
| Title | Vendor Invoice Carlos |
| Project name | `vendor-invoice-carlos-89ac9c10-ixp` |
| Tenant | staging.uipath.com · customersuccessamer · Training |
| Live version | 6 (published and tagged `live`) |
| Documents | 5 vendor invoices (006–010), all fields confirmed |

## Taxonomy

Field group **Vendor Invoice**: Extract the vendor identity and tax registration, invoice and purchase-order references, total payable amount, currency, and dates needed to match and approve a vendor invoice.

| # | Field | Type | Instruction |
|---|---|---|---|
| 1 | Vendor Name | Exact Text | Extract the supplier's full legal name exactly as shown in the vendor, supplier, from, remit-to, letterhead, or supplier-of-record area. |
| 2 | Vendor Tax ID | Exact Text | Extract the vendor's US EIN in NN-NNNNNNN format. Preserve leading zeros; do not capture a phone or account number. |
| 3 | Invoice Number | Exact Text | Extract the invoice or document number, including prefixes and punctuation. |
| 4 | Invoice Date | Date | Extract the invoice issue date only, not the purchase-order, ship, or service date. Normalize it to MM/DD/YYYY. |
| 5 | PO Number | Exact Text | Extract the complete purchase-order reference, including its prefix and leading zeros. Watch for OCR confusion between O and 0. |
| 6 | Total Amount | Number | Extract the final grand total payable including sales tax: the last, bold row of the totals block at the bottom right of the invoice, labeled 'TOTAL DUE', 'BALANCE DUE' or 'TOTAL (USD)' and followed by the currency code. It equals Subtotal plus Sales Tax. Do NOT extract the 'Subtotal' row, the 'Sales Tax' row, any line-item AMOUNT or LINE TOTAL, or the blank 'Amount enclosed' on a remittance stub. Example: on invoice GLS-2026-0442 the value is 17,988.20, not the Subtotal 16,970.00. |
| 7 | Currency | Exact Text | Extract the three-letter currency code. When only a dollar sign is present, return USD. |
| 8 | Due Date | Date | Extract the payment due date, or derive it from the invoice date and explicit payment terms. Normalize it to MM/DD/YYYY. |

## Live version vs. expected extractions

Predictions from live version 6 compared with `reference/expected-extractions.json`. IXP returns dates as `YYYY-MM-DDT00:00:00Z`; they were converted to MM/DD/YYYY before comparing. Totals were compared as two-decimal numbers.

| Invoice | Vendor Name | Vendor Tax ID | Invoice Number | Invoice Date | PO Number | Total Amount | Currency | Due Date |
|---|---|---|---|---|---|---|---|---|
| 006 Great Lakes Steel | ✅ Great Lakes Steel Supply Inc. | ✅ 38-4410927 | ✅ GLS-2026-0442 | ✅ 03/05/2026 | ✅ PO-2026-0470 | ✅ 17988.20 | ✅ USD | ✅ 04/04/2026 |
| 007 Liberty Print | ✅ Liberty Print & Signage LLC | ✅ 36-5127744 | ✅ LPS-55210 | ✅ 03/09/2026 | ✅ PO-2026-0476 | ✅ 4946.40 | ✅ USD | ✅ 04/08/2026 |
| 008 Summit Facilities | ✅ Summit Facilities Group | ✅ 45-6612093 | ✅ SFG-2026-0771 | ✅ 03/12/2026 | ✅ PO-2026-0468 | ✅ 3699.26 | ✅ USD | ✅ 04/11/2026 |
| 009 Vertex Analytics | ✅ Vertex Analytics Corp. | ✅ 88-2245107 | ✅ VA-INV-40592 | ✅ 03/02/2026 | ✅ PO-2026-0405 | ✅ 22700.00 | ✅ USD | ✅ 04/16/2026 |
| 010 Pacific Timber | ✅ Pacific Timber Company | ✅ 93-1180446 | ✅ PTC-2026-2287 | ✅ 03/13/2026 | ✅ PO-2026-0489 | ✅ 9902.58 | ✅ USD | ✅ 04/12/2026 |

**Result: 40 / 40 fields match.** These five invoices are also the documents the model was trained on, so this is not a test on unseen invoices.
