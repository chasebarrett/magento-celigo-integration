# Solution Design – Customer Uniqueness Strategy

## Constraints
- No custom NetSuite development
- Preserve human-readable customer names
- Maintain existing CS workflows

## Data Sources Evaluated
- Magento customer internal ID
- Email address
- Combination keys

## Why Magento Customer ID Won
- Deterministic
- Immutable
- Exposed in Celigo
- Already trusted internally

## Mapping Logic (High Level)
- Source: Magento.customer_id
- Transform: append to display name
- Target: NetSuite entity ID
