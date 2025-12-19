# SOP – Customer Sync Error Handling

## Scope
Magento → Celigo → NetSuite customer and sales order flows

## Expected Behavior
- Customers sync automatically
- Sales orders attach to correct customer records

## Common Failure Modes
- Missing customer_id
- Mapping changes

## Troubleshooting Checklist
1. Verify Magento customer record exists
2. Confirm customer_id present in Celigo export
3. Validate mapping logic
4. Re-run flow

## Ownership
- Systems Admin
- Escalation: eCommerce / PM
