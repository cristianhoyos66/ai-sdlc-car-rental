# PRD - Marketing Features

## Document Information
- **Product / Feature Name:** Marketing Features for Car Rental System
- **Author:** cristianhoyos66
- **Date:** 
- **Version:** 

## Table of Contents
- [Document Information](#document-information)
- [Table of Contents](#table-of-contents)
- [Overview](#overview)
  - [Background](#background)
  - [Objective](#objective)
  - [Goals](#goals)
- [Problem Statement](#problem-statement)
- [Functional Requirements](#functional-requirements)
  - [FR-1: Car Rental Percentage Tracking](#fr-1-car-rental-percentage-tracking)
  - [FR-2: Seasonal Pricing Overlay](#fr-2-seasonal-pricing-overlay)
  - [FR-3: Pricing Simulation on Confirmation Page](#fr-3-pricing-simulation-on-confirmation-page)
  - [FR-4: Customer Segmentation](#fr-4-customer-segmentation)
  - [FR-5: Approval Workflow with Role-Based Limits](#fr-5-approval-workflow-with-role-based-limits)
  - [FR-6: Performance Reporting Dashboard](#fr-6-performance-reporting-dashboard)
  - [FR-7: Competitor Price Monitoring](#fr-7-competitor-price-monitoring)
  - [FR-8: Customer Identity Validation Consent](#fr-8-customer-identity-validation-consent)
  - [FR-9: Pricing Audit History](#fr-9-pricing-audit-history)
- [Non-Functional Requirements](#non-functional-requirements)
- [Dependency & Constraints](#dependency--constraints)
- [Success Metrics](#success-metrics)

## Overview

### Background
The company is expanding from car sales into the car rental business. This is a new business line requiring dedicated marketing capabilities to support customer acquisition, retention, and revenue optimization. Marketing needs to track specific KPIs, manage pricing strategies, and establish approval workflows to ensure operational efficiency and risk management.

### Objective
Build marketing features that enable the marketing team to:
1. Monitor rental fleet utilization and customer acquisition metrics
2. Manage pricing tiers and seasonal pricing overlays
3. Support customer segmentation for targeted strategies
4. Implement role-based approval workflows for rental transactions
5. Generate performance reports for data-driven decision making

### Goals
- Achieve and maintain at least 75% car rental percentage (rented cars vs. total available fleet)
- Enable brand awareness and customer acquisition through web and marketplace aggregator channels
- Mitigate payment risk through appropriate approval workflows
- Provide pricing transparency and simulation capabilities to customers
- Establish a foundation for data-driven marketing decisions

## Problem Statement

The company is entering a new car rental business without existing systems or processes tailored to rental operations. Marketing needs:

1. **Visibility into fleet utilization**: Without tracking rental percentages, the company cannot assess whether they are meeting the 75% utilization target.

2. **Pricing flexibility**: Static pricing across three vehicle categories (small regular, medium regular, medium luxury) with daily, weekly, and monthly rates needs to be managed, with the ability to apply seasonal overlays during peak holidays.

3. **Risk management**: The primary concern is customers not paying on time or after extending rentals. Role-based approval limits are needed to manage exposure.

4. **Performance tracking**: Marketing lacks visibility into daily, weekly, and monthly performance metrics including rental percentage, income, new customers, and repeat customers.

**Who is affected:**
- Marketing team (staff, supervisors, department heads, directors)
- Customers (local, tourist, insurance replacement segments)
- Operations team (dependent on pricing and approval workflows)

**Why is this important now:**
Without these capabilities at launch, the company risks:
- Poor fleet utilization leading to lost revenue
- Inability to adapt pricing to demand patterns
- Uncontrolled payment exposure
- Lack of data to optimize marketing strategies

## Functional Requirements

### FR-1: Car Rental Percentage Tracking

**Title:** Track and Display Car Rental Percentage

**Statement:** **As a** marketing manager, **I want** to track the percentage of rented cars versus total available fleet in real-time, **so that** I can ensure we maintain at least 75% utilization and identify opportunities to improve fleet performance.

**Requirement Detail:**
The system must calculate and display the car rental percentage, defined as (number of rented cars / total available cars) × 100, across daily, weekly, and monthly time periods. This metric is critical for assessing business health and marketing effectiveness.

**Acceptance Criteria:**

**Given** the system has rental data for the current period  
**When** a marketing user accesses the rental percentage dashboard  
**Then** the system displays:
- Current day rental percentage
- Current week rental percentage  
- Current month rental percentage
- A visual indicator when percentage falls below 75% threshold

**Given** rental or fleet inventory changes occur  
**When** a car is rented, returned, or added/removed from inventory  
**Then** the rental percentage is recalculated automatically within 5 minutes

**Given** a marketing user views historical data  
**When** they select a past date range  
**Then** the system displays rental percentage for that period with daily granularity

---

### FR-2: Seasonal Pricing Overlay

**Title:** Apply Seasonal Pricing During Peak Holiday Periods

**Statement:** **As a** marketing department head, **I want** to apply seasonal pricing overlays during peak holiday seasons, **so that** we can optimize revenue during high-demand periods.

**Requirement Detail:**
The system must support time-limited pricing adjustments that overlay the standard pricing structure. Standard pricing is based on three vehicle categories (small regular car, medium regular car, medium luxury car) with daily, weekly, and monthly rate options. Seasonal overlays allow percentage-based or fixed-amount adjustments during specified date ranges.

**Acceptance Criteria:**

**Given** a marketing department head creates a seasonal pricing rule  
**When** they specify date range, vehicle categories, and pricing adjustment (percentage or fixed amount)  
**Then** the system stores the rule and activates it during the specified period

**Given** a seasonal pricing overlay is active  
**When** a customer searches for vehicles during the overlay period  
**Then** the system displays the adjusted pricing instead of standard rates

**Given** multiple seasonal overlays exist  
**When** date ranges do not overlap  
**Then** each overlay is applied during its respective period

**Given** a seasonal pricing period ends  
**When** the end date is reached  
**Then** the system automatically reverts to standard pricing

---

### FR-3: Pricing Simulation on Confirmation Page

**Title:** Display Pricing Simulation Before Booking Confirmation

**Statement:** **As a** customer, **I want** to see simulated pricing scenarios on the confirmation page, **so that** I can understand the cost implications of different rental periods before finalizing my booking.

**Requirement Detail:**
Before a customer confirms their booking, the system must display a pricing breakdown showing the cost for daily, weekly, and monthly rental options based on their selected vehicle category. This helps customers make informed decisions and understand potential savings from longer rental periods.

**Acceptance Criteria:**

**Given** a customer completes vehicle selection  
**When** they reach the confirmation page  
**Then** the system displays:
- Their selected rental duration and calculated price
- Alternative pricing scenarios (daily rate for same period, weekly rate equivalent, monthly rate equivalent)
- Potential savings highlighted for longer-term options

**Given** the customer's rental period spans multiple rate tiers  
**When** the pricing simulation is displayed  
**Then** the system calculates the optimal pricing combination (e.g., 1 month + 5 days)

**Given** seasonal pricing is active  
**When** the pricing simulation is displayed  
**Then** the system includes seasonal adjustments in all scenarios

---

### FR-4: Customer Segmentation

**Title:** Categorize Customers by Segment Type

**Statement:** **As a** marketing analyst, **I want** to categorize customers into segments (local, tourist, insurance replacement), **so that** I can analyze performance by segment and develop targeted strategies in the future.

**Requirement Detail:**
The system must allow manual classification of customers into predefined segments during or after the booking process. While segmentation will not drive differentiated pricing or offers at launch, capturing this data establishes a foundation for future targeted marketing initiatives.

**Acceptance Criteria:**

**Given** a new customer creates a booking  
**When** they complete their profile information  
**Then** the system captures their segment classification (local, tourist, insurance replacement) either through self-selection or staff assignment

**Given** a marketing analyst generates reports  
**When** they filter by customer segment  
**Then** the system displays metrics (rental count, revenue, repeat rate) broken down by segment

**Given** segment definitions need updating  
**When** an administrator modifies segment criteria  
**Then** the system allows reassignment of existing customers through manual review process

---

### FR-5: Approval Workflow with Role-Based Limits

**Title:** Implement Role-Based Rental Approval Limits

**Statement:** **As a** marketing director, **I want** to establish configurable approval limits based on staff roles, **so that** we can manage payment risk by ensuring higher-value rentals receive appropriate authorization levels.

**Requirement Detail:**
To mitigate the risk of customers not paying on time, the system must implement an approval workflow where rental transactions require authorization based on their value. Four roles exist with escalating approval limits: Staff (max X), Supervisor (max Y), Marketing Department Head (max Z), and Marketing Director (unlimited). The values X, Y, and Z must be configurable by system administrators.

**Acceptance Criteria:**

**Given** a rental transaction is initiated  
**When** the transaction value is calculated  
**Then** the system determines the minimum required approver role based on configured limits

**Given** a staff member attempts to approve a rental  
**When** the rental value exceeds their configured limit (X)  
**Then** the system prevents approval and routes the request to a supervisor

**Given** a supervisor attempts to approve a rental  
**When** the rental value exceeds their configured limit (Y)  
**Then** the system prevents approval and routes the request to the department head

**Given** a marketing department head attempts to approve a rental  
**When** the rental value exceeds their configured limit (Z)  
**Then** the system prevents approval and routes the request to a director

**Given** a marketing director reviews a rental  
**When** they approve regardless of value  
**Then** the system processes the approval (unlimited authority)

**Given** an administrator updates approval limits  
**When** they change the X, Y, or Z values  
**Then** the system applies the new limits to all pending and future transactions

---

### FR-6: Performance Reporting Dashboard

**Title:** Generate Daily, Weekly, and Monthly Performance Reports

**Statement:** **As a** marketing manager, **I want** to access daily, weekly, and monthly performance reports, **so that** I can monitor key business metrics and make data-driven decisions.

**Requirement Detail:**
The system must automatically generate reports showing:
1. Rental car percentage (daily/weekly/monthly)
2. Income reports (daily/weekly/monthly)
3. New customer count within the month
4. Customers with repeated orders

Historical data must be retained for 3 years to support trend analysis and compliance requirements.

**Acceptance Criteria:**

**Given** the reporting dashboard is accessed  
**When** a marketing user selects a date range  
**Then** the system displays:
- Rental car percentage for the period
- Total income broken down by day/week/month
- Count of new customers acquired in the selected month
- Count and list of customers with 2+ bookings

**Given** a report is generated  
**When** the data includes multiple time periods  
**Then** the system provides comparison metrics (e.g., "15% increase from previous month")

**Given** historical data exists beyond 3 years  
**When** the data retention policy is enforced  
**Then** the system archives data older than 3 years but retains aggregated summaries for trend analysis

**Given** a marketing user wants to export data  
**When** they select export function  
**Then** the system generates downloadable reports in CSV or PDF format

---

### FR-7: Competitor Price Monitoring

**Title:** Track Regular Competitor Pricing Information

**Statement:** **As a** marketing analyst, **I want** to regularly monitor competitor pricing, **so that** I can ensure our pricing remains competitive in the market.

**Requirement Detail:**
The system must provide a mechanism to input and track competitor pricing data on a regular basis. This can be manual entry or automated data collection. The data should be viewable alongside the company's own pricing to facilitate comparison and strategic decisions.

**Acceptance Criteria:**

**Given** a marketing analyst accesses competitor pricing tools  
**When** they enter competitor pricing data  
**Then** the system stores the data with timestamp, competitor name, vehicle category, and price point

**Given** competitor pricing data exists  
**When** a marketing user views pricing comparisons  
**Then** the system displays the company's pricing alongside competitor pricing for equivalent vehicle categories

**Given** competitor pricing is updated  
**When** new data is entered  
**Then** the system maintains historical pricing trends for analysis

---

### FR-8: Customer Identity Validation Consent

**Title:** Capture Customer Consent for ID Card Validation

**Statement:** **As a** compliance officer, **I want** to capture and record customer consent for ID card validation, **so that** we comply with data privacy regulations when verifying customer identity.

**Requirement Detail:**
The system must present a consent mechanism during the booking process where customers explicitly agree to ID card validation. This consent must be recorded with timestamp and linked to the customer's account.

**Acceptance Criteria:**

**Given** a customer initiates a booking  
**When** they reach the identity verification step  
**Then** the system displays a clear consent statement explaining ID card validation and requests explicit consent (checkbox or similar mechanism)

**Given** a customer provides consent  
**When** they submit the consent form  
**Then** the system records the consent with timestamp and allows booking to proceed

**Given** a customer does not provide consent  
**When** they attempt to proceed without consenting  
**Then** the system prevents booking completion and displays an explanation

**Given** an audit is performed  
**When** customer consent records are reviewed  
**Then** the system provides a complete audit trail showing who consented, when, and for which bookings

---

### FR-9: Pricing Audit History

**Title:** Maintain Audit Log of Pricing Changes

**Statement:** **As a** marketing department head, **I want** to track who changed pricing rules, when they made changes, and what the previous values were, **so that** I can maintain accountability and investigate pricing issues.

**Requirement Detail:**
The system must automatically log all changes to pricing tiers, seasonal overlays, and approval limits. The audit log must capture the user who made the change, timestamp, affected pricing element, previous value, and new value.

**Acceptance Criteria:**

**Given** an authorized user modifies a pricing tier  
**When** they save the changes  
**Then** the system logs:
- User ID and name
- Timestamp
- Pricing element modified (e.g., "Medium Luxury Car - Weekly Rate")
- Previous value
- New value

**Given** an authorized user creates or modifies a seasonal overlay  
**When** they save the changes  
**Then** the system logs all configuration details with user and timestamp

**Given** a marketing department head reviews audit history  
**When** they access the audit log  
**Then** the system displays a searchable, filterable list of all pricing changes with full details

**Given** an investigation requires historical pricing  
**When** a specific date is queried  
**Then** the system can reconstruct the exact pricing configuration active at that time

## Non-Functional Requirements

(Leave blank)

## Dependency & Constraints

1. **Channel Limitations:** At launch, the system supports web and marketplace aggregator channels only. Mobile app support is not included in the initial release.

2. **Language Support:** The system will support English only at launch. Multilingual support is deferred to future releases.

3. **Pricing Structure:** Static pricing model with three vehicle categories (small regular car, medium regular car, medium luxury car) and three rate periods (daily, weekly, monthly). Dynamic pricing based on demand, location, or time-of-day is not supported at launch.

4. **Promotion Capabilities:** Advanced promotion features (promo codes, automatic discounts, tiered discounts, loyalty programs, add-on bundles) are not included at launch. Focus is on base pricing and seasonal overlays only.

5. **Customer Journey:** The booking funnel follows a fixed sequence: search → vehicle selection → confirmation → payment. Abandoned cart recovery, personalization, and upsell features are not included at launch.

6. **Analytics Integration:** External analytics tools, BI platforms, and real-time dashboards are not integrated at launch. Reporting is limited to the built-in performance dashboard.

7. **Data Retention:** Marketing performance data must be retained for 3 years minimum to support trend analysis and compliance.

8. **Approval Workflow:** The system requires manual approval by authorized personnel. Automated approval based on credit scoring or other criteria is not supported.

## Success Metrics

The success of marketing features will be measured by:

1. **Fleet Utilization Target:**
   - Achieve and maintain ≥75% car rental percentage on a weekly basis
   - Track daily, weekly, and monthly rental percentages to identify trends

2. **Customer Acquisition:**
   - Track number of new customers acquired per month
   - Establish baseline in first 3 months for future comparison

3. **Customer Retention:**
   - Measure repeat customer rate (customers with 2+ bookings)
   - Target: Establish baseline in first 6 months; aim for 20% repeat rate by end of year 1

4. **Revenue Performance:**
   - Monitor daily, weekly, and monthly income reports
   - Track seasonal pricing impact on revenue during peak holiday periods

5. **Payment Risk Mitigation:**
   - Measure percentage of rentals with on-time payment
   - Track escalation rate for approvals exceeding role limits
   - Target: Reduce payment defaults through appropriate approval workflows

6. **Operational Efficiency:**
   - Reduce time to approve rental transactions by establishing clear approval limits
   - Achieve 100% compliance with ID validation consent requirements

7. **Pricing Competitiveness:**
   - Regular competitor price monitoring and documentation
   - Maintain pricing within 10% of market average for equivalent vehicle categories
