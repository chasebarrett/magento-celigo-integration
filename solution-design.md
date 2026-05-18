# Solution Design – Customer Uniqueness Strategy

## Constraints
- No custom NetSuite development
- Preserve human-readable customer names
- Maintain existing CS workflows

## Data Sources Evaluated
- Magento customer internal ID
- Email address
- Combination keys

## Alternatives Considered

### NetSuite Native Auto-Numbering
NetSuite offers a native solution to the uniqueness constraint: enabling auto-generated, random Customer IDs for new records. This would have satisfied NetSuite's uniqueness requirement without any custom mapping.

**Why we declined:**
- Auto-generated IDs are not how Customer Service or Sales teams identify or search for records
- Reps look for customers by name first, ID second — a randomized identifier reverses that workflow
- Adopting it would have forced a behavioral change across two teams to solve a data problem, rather than solving the data problem in a way that matched existing behavior

The native solution was technically valid but operationally misaligned. The Magento-customer-ID approach preserved the way teams already worked while still resolving the uniqueness issue.

## Why Magento Customer ID Won
- Deterministic
- Immutable
- Exposed in Celigo
- Already trusted internally

## Mapping Logic (High Level)
- Source: Magento.customer_id
- Transform: append to display name
- Target: NetSuite entity ID
