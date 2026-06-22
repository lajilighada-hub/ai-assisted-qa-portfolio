# Billing & Invoice Cancellation – Risk Analysis

## Risk Assessment

| ID | Risk Description | Likelihood | Impact | Risk Level | Mitigation |
|----|-----------------|-----------|--------|-----------|------------|
| R-01 | Credit note created with wrong customer reference | Medium | High | **High** | Auto-populate reference from original invoice; validate before save |
| R-02 | VAT rate mismatch between invoice and credit note | Medium | High | **High** | Lock VAT rate from original invoice; disallow manual override |
| R-03 | Net amount rounding difference after cancellation | Medium | Medium | **Medium** | Use same rounding logic as invoice generation; compare with original |
| R-04 | Credit note posted to wrong accounting period | Low | High | **Medium** | Default to current period; allow override with warning |
| R-05 | Duplicate credit note for same invoice | Low | High | **Medium** | Check for existing credit note before creation; block duplicates |
| R-06 | Credit note created for already-cancelled invoice | Low | Medium | **Low** | Invoice status check before credit note generation |
| R-07 | PDF generation failure for credit note | Low | Medium | **Low** | Queue for retry; admin notification on failure |
| R-08 | Currency conversion mismatch (multi-currency) | Low | High | **Medium** | Lock original exchange rate; no re-conversion allowed |
| R-09 | Missing audit trail for cancellation action | Medium | Medium | **Medium** | Log all cancellation events with user ID, timestamp, IP |
| R-10 | Partial cancellation (line items) miscalculated | Medium | High | **High** | Calculate proportionally; validate line-item totals match original |

## Test Strategy

1. **Positive tests**: Full invoice cancellation, verify all fields match
2. **Negative tests**: Cancel already-cancelled invoice, invalid invoice ID
3. **Edge cases**: Zero-value invoices, multi-currency, partial cancellations
4. **Audit tests**: Verify log entries, user tracking
5. **PDF tests**: Credit note PDF generation, data accuracy
