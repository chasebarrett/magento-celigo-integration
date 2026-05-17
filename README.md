# Magento–Celigo–NetSuite Customer Mapping Optimization
**eCommerce Systems Case Study | Integration Design | Data Governance**

This repository documents a production integration problem at **SCARPA North America**, where customer and order sync failures between **Magento, Celigo, and NetSuite** were creating significant operational overhead — and how a data-modeling–first solution reduced those errors by **90%+** without custom development.

---

## Table of Contents
- [The Problem](#-the-problem)
- [My Approach](#-my-approach)
- [Key Insight](#-key-insight)
- [The Solution](#%EF%B8%8F-the-solution)
- [Results](#-results)
- [Why This Matters](#-why-this-matters)
- [Solution Design](solution-design.md)
- [SOP](sop.md)

---

## 🧭 The Problem

SCARPA’s eCommerce stack consisted of:

- **Magento** — Frontend eCommerce platform  
- **NetSuite** — ERP & system of record  
- **Celigo (integrator.io)** — Integration layer  

### ⚠️ Core Issue
NetSuite enforces **global uniqueness on customer records**.

- Works well for wholesale
- Breaks down in **direct-to-consumer (DTC)** environments

### 📉 Impact
- Orders errored when customers shared common names (e.g. *John Doe*)
- Celigo failed during customer creation
- Orders required manual recreation in NetSuite
- Customer Service bandwidth shifted from support to data cleanup
- Payroll costs and technical debt increased

This wasn’t a bug — it was a **misalignment between system design and business model**.

---

## 🏗️ Existing Workflow (Before)

```mermaid
flowchart TD
  A["Customer places order"]
  B["Magento<br/>Customer + Sales Order created"]
  C["Celigo flow runs<br/>Customer → NetSuite"]
  D["NetSuite<br/>Create or Match Customer"]
  E["Error in Celigo or NetSuite"]
  F["Customer Service manual fix<br/>Create customer and order in NetSuite"]

  A --> B
  B --> C
  C --> D
  D -->|Duplicate customer name detected| E
  E --> F
```

---

## 🧠 My Approach

Before applying a workaround, I focused on deeply understanding:

- Magento customer data structure  
- Celigo field availability and mappings  
- NetSuite customer uniqueness constraints  
- How CS and Sales teams actually use customer records  

Rather than forcing a system-native solution that degraded usability, I looked for a way to introduce **deterministic uniqueness** while preserving human readability.

---

## 🔑 Key Insight

Magento assigns **every customer a unique internal customer ID**.

- John Doe → Customer ID `3456`  
- John Doe → Customer ID `3459`  

This internal ID:
- Is deterministic  
- Is exposed in Magento exports  
- Is accessible inside Celigo mappings  

That made it an ideal solution for enforcing uniqueness **without sacrificing usability**.

---

## 🛠️ The Solution

We updated the Celigo customer mapping to:

- Pull Magento’s internal `customer_id`
- Append it to the customer name when creating the NetSuite record

### ✅ Resulting Format

```
Before: John Doe
After:  John Doe 3456
```

This preserved:
- Familiar customer naming conventions  
- Existing reporting and workflows  
- System-wide uniqueness in NetSuite  

No custom NetSuite development was required.

---

## 🗺️ Updated Workflow (After)

```mermaid
flowchart TD
  A["Customer places order"]
  B["Magento<br/>Customer + Sales Order created"]
  C["Magento assigns unique Customer ID"]
  D["Celigo mapping appends ID<br/>Name = First Last + CustomerID"]
  E["NetSuite<br/>Create customer (unique)"]
  F["NetSuite<br/>Create Sales Order under customer"]
  G["Sync succeeds<br/>No manual CS work"]

  A --> B --> C --> D --> E --> F --> G
```
## 🔁 Broader Integration Context (Bi-Directional)

```mermaid
flowchart LR
  M["Magento<br/>eCommerce Frontend"]
  C["Celigo<br/>integrator.io"]
  N["NetSuite<br/>ERP / Source of Truth"]

  M <-->|Customers + Orders| C
  C <-->|Create/Update Customers + Sales Orders| N
  N -->|Inventory + Item Data| C
  C -->|Sync Inventory Availability| M
```

## 🧩 Root Cause → Fix Summary

```mermaid
flowchart TD
  A["NetSuite requires unique Customer Name"]
  B["Common names collide<br/>(John Doe)"]
  C["Celigo customer create fails"]
  D["Order fails to import"]
  E["Manual CS intervention required"]

  A --> B --> C --> D --> E

  F["Fix: Use Magento internal customer ID"]
  G["Append ID to customer display name"]
  H["Customer record becomes unique"]
  I["Orders import successfully"]
  J["Errors reduced 90%+"]

  F --> G --> H --> I --> J
```

---

## 📈 Results

- **90%+ reduction** in customer-related sync errors  
- Significant reduction in manual Customer Service intervention  
- Improved trust in system data  
- Lower operational overhead  
- Scalable solution aligned with DTC growth  

---

## 📦 Deliverables

- **New Celigo customer mappings**  
- **Integration logic documentation**  
- **Standard Operating Procedure (SOP)** for maintenance and troubleshooting  
- **Knowledge transfer** to Systems and Customer Service teams  

---

## 🧭 Why This Matters

This case study highlights that:

- Integration failures are often **data-modeling problems**, not tooling problems  
- Small mapping decisions can have massive operational impact  
- Systems should be designed for how people actually work — not just how software expects them to  

---

## 📁 Repository Structure

- [`solution-design.md`](solution-design.md) — Constraints, data sources evaluated, and rationale for the customer ID approach
- [`sop.md`](sop.md) — Standard operating procedure for ongoing customer sync error handling

