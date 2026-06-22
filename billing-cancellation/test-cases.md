# Billing & Invoice Cancellation – Test Cases

## Test Data

| Field | Valid Value | Invalid Value |
|-------|------------|--------------|
| Invoice ID | INV-2026-0042 | 0, -1, ABC, NULL, 9999999 |
| Customer Ref | CUST-00123 | EMPTY, SPECIAL!@# |
| Net Amount | 100.00 | 0, -50, 1.999, ABC |
| VAT Rate | 19.00 | -5, 50, TEXT, 0.001 |
| Currency | EUR | XYZ, empty |
| Cancellation Reason | Customer request | null, special chars |
| User Role | admin, finance | guest, viewer |

## Test Cases (15)

| ID | Title | Precondition | Test Data | Steps | Expected Result | Type |
|----|-------|-------------|-----------|-------|----------------|------|
| CN-001 | Full cancellation – single invoice | Valid invoice INV-2026-0042 exists, status: paid | ID: INV-0042, Net: EUR 100.00, VAT: 19% | 1. Open invoice 2. Click cancel 3. Confirm 4. Verify credit note | Credit note created with ref INV-0042-CN, net EUR 100.00, VAT 19% | Positive |
| CN-002 | Credit note matches original invoice fields | Invoice with multiple line items | Net: 250.00, VAT: 19%, Ref: CUST-00123 | 1. Cancel invoice 2. Open credit note 3. Compare fields | Customer ref, net amount, VAT rate identical to original | Positive |
| CN-003 | Cancel already-cancelled invoice | Invoice INV-0042 already cancelled | Same invoice ID | 1. Find cancelled invoice 2. Cancel again | Error: "Invoice already cancelled" | Negative |
| CN-004 | Cancel with invalid invoice ID | System has valid invoices | Invoice ID: 0 | 1. API call with ID=0 | 400 Bad Request, "Invalid invoice ID" | Negative |
| CN-005 | Partial cancellation – one line item | Invoice with 3 line items (50+30+20) | Cancel line 2 (EUR 30) | 1. Select partial cancellation 2. Pick line 2 3. Confirm | Credit note for EUR 30 with proportional VAT | Positive |
| CN-006 | VAT rate preservation after rate change | Invoice from 2025 with 16% VAT, current rate 19% | INV-2025-001 (16%), cancel in 2026 | 1. Cancel old invoice 2. Check VAT on credit note | Credit note uses 16% (original rate), not 19% | Positive |
| CN-007 | Duplicate cancellation prevention | Invoice INV-0042, credit note CN-0042 already exists | Same invoice | 1. Try to cancel again 2. System check | Blocked: "Credit note already exists for this invoice" | Negative |
| CN-008 | Credit note PDF generation | Invoice INV-0042 cancelled, CN-0042 created | CN-0042 | 1. Generate PDF 2. Open file 3. Check fields | PDF contains: INV-0042 ref, correct net/VAT, date, CN number | Positive |
| CN-009 | Audit log after cancellation | Admin user cancels invoice | User: admin@test.de, INV-0042 | 1. Cancel invoice 2. Check audit table | Log entry: user, timestamp, invoice ID, action "cancellation", IP address | Positive |
| CN-010 | Cancel invoice with zero net amount | Invoice with EUR 0.00 (promo/free) | Net: 0.00 VAT: 0% | 1. Cancel zero invoice | Credit note created with 0.00, no rounding errors | Edge case |
| CN-011 | Multi-currency cancellation | Invoice in USD, rate: 1.10 | INV-0043, USD 200, rate locked | 1. Cancel 2. Check credit note currency | Credit note in USD with original exchange rate, no re-conversion | Positive |
| CN-012 | Unauthorised user tries cancellation | User with "viewer" role | viewer@test.de, INV-0042 | 1. Login as viewer 2. Navigate to cancel 3. Attempt | 403 Forbidden, "Insufficient permissions" | Security |
| CN-013 | Cancellation reason required | System configured to require reason | No reason provided | 1. Click cancel 2. Leave reason empty 3. Submit | Validation error: "Cancellation reason is required" | Negative |
| CN-014 | Batch cancellation – 10 invoices | 10 paid invoices selected | INV-0050 to INV-0059 | 1. Select all 10 2. Batch cancel 3. Confirm | 10 credit notes created in <5s, all fields correct | Performance |
| CN-015 | Credit note reversal | Credit note created in error, needs reversal | CN-0042 | 1. Reverse credit note 2. Check original invoice status | Original invoice restored to "paid" status, reversal logged | Positive |

## Possible Defects

| ID | Title | Steps | Actual | Expected | Severity | Root Cause |
|----|-------|-------|--------|----------|----------|-----------|
| BUG-CN-001 | Credit note shows current VAT rate instead of original | Cancel invoice from 2025 (16% VAT). Credit note shows 19% | VAT rate: 19% | VAT rate: 16% | **High** | System reads current rate instead of snapshot from invoice |
| BUG-CN-002 | Duplicate credit note created on network timeout | Cancel invoice. API times out. User retries. Two credit notes created | 2 credit notes for same invoice | 1 credit note only | **Critical** | Missing idempotency check; no unique constraint on invoice_id |
| BUG-CN-003 | Credit note PDF shows wrong date format | Cancel in German locale (DD.MM.YYYY). PDF shows MM/DD/YYYY | Date: 06/23/2026 | Date: 23.06.2026 | **Medium** | PDF template ignores locale setting |

## Traceability Matrix

| Requirement | Test Case IDs | Defect IDs | Coverage |
|-------------|--------------|-----------|----------|
| Credit note with original customer reference | CN-001, CN-002, CN-008 | – | ✅ |
| Credit note with original net amount | CN-001, CN-002, CN-006, CN-011 | BUG-CN-001 | ⚠️ Partial (VAT issue) |
| Credit note with original VAT rate | CN-001, CN-006, CN-008 | BUG-CN-001 | ⚠️ Partial |
| Prevent duplicate cancellations | CN-003, CN-007 | BUG-CN-002 | ⚠️ Partial |
| Audit trail for cancellations | CN-009, CN-014 | – | ✅ |
| PDF generation | CN-008, CN-015 | BUG-CN-003 | ⚠️ Partial |
| Role-based access | CN-012 | – | ✅ |
| Cancellation reason required | CN-013 | – | ✅ |
| Idempotency / no duplicates | CN-007 | BUG-CN-002 | ⚠️ Need fix |
