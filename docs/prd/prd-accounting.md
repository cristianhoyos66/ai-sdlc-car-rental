# PRD - Accounting System for Car Rental

## Document Information

- **Product / Feature Name:** Accounting System for Car Rental
- **Author:** 
- **Date:** 
- **Version:** 

## Table of Contents

1. [Overview](#overview)
   - Background
   - Objective
   - Goals
2. [Problem Statement](#problem-statement)
3. [Functional Requirements](#functional-requirements)
4. [Non-Functional Requirements](#non-functional-requirements)
5. [Dependency & Constraints](#dependency--constraints)
6. [Success Metrics](#success-metrics)

## Overview

### Background

Our company is expanding from car sales into the car rental business. This new business line requires a dedicated accounting system to manage the unique financial operations of car rentals, including revenue recognition, cost tracking, depreciation management, and integration with our existing Oracle General Ledger.

Unlike our existing car sales business, car rental operations involve:
- Recurring revenue streams from vehicle rentals
- Complex cost structures including depreciation, maintenance, and operational costs
- Different revenue recognition patterns (prepayment, late charges, adjustments)
- Location-based revenue segmentation
- Daily cash flow management and reconciliation

### Objective

Build an accounting system that supports the complete financial lifecycle of car rental operations, from booking payment reconciliation to period-end closing, while maintaining compliance with financial reporting standards and integrating seamlessly with Oracle General Ledger.

### Goals

1. Enable accurate and timely revenue recognition for rental transactions
2. Provide real-time visibility into daily cash flow and profitability per vehicle
3. Automate payment reconciliation from multiple sources (CSV bank reports and online payments)
4. Maintain a comprehensive and immutable Chart of Accounts specific to car rental operations
5. Support location-based revenue segmentation for business performance analysis
6. Achieve zero-friction month-end close with automated calculations and minimal manual adjustments (< 1% of total ledger entries)
7. Integrate seamlessly with Oracle General Ledger via CSV export

## Problem Statement

The accounting team needs a specialized financial management system to handle car rental operations, which have fundamentally different financial characteristics than our existing car sales business.

**Key pain points:**
- No existing system to track rental-specific revenue streams (base rental, late charges, penalties)
- Manual reconciliation processes are time-consuming and error-prone
- Lack of visibility into per-vehicle profitability and location-based performance
- No automated depreciation calculation for fleet vehicles
- Inability to reconcile payments from multiple sources (online and bank CSV) efficiently
- Risk of delayed period close without automated accruals and reconciliation

**Affected users:**
- Accounting staff (daily transaction recording and reconciliation)
- Finance managers (period-end close and reporting)
- Location managers (location-based revenue analysis)
- Executive management (financial KPI monitoring)

**Why this is important now:**
As we launch the car rental business line, we need proper financial controls and reporting from day one to:
- Ensure accurate financial statements
- Monitor business profitability and cash flow
- Comply with financial regulations and audit requirements
- Make data-driven business decisions
- Support scaling to multiple locations

## Functional Requirements

### FR-1: Chart of Accounts Management

**Title:** Separate Chart of Accounts for Rental Operations

**Statement:** As an **Accounting Manager**, I want **a separate Chart of Accounts dedicated to rental operations**, so that **I can clearly segregate rental finances from car sales and track rental-specific revenue and cost categories**.

**Requirement Detail:**

The system must maintain a separate Chart of Accounts (CoA) structure for the car rental business with the following account categories:

**Revenue Accounts:**
- Base Rental Income
- Late Charge Income
- Delivery/Drop-off Fee Income

**Cost Accounts:**
- Maintenance Cost (Workshop)
- Fuel Cost
- Vehicle Installment
- Vehicle Insurance
- Depreciation Expense
- Promotion Cost
- Rental Cancellation Cost (No-shows)
- Customer Retention Cost (Early return refunds)
- Chargeback Cost
- Manual Adjustment (Credit/Debit)

**Tax Account:**
- VAT (Value Added Tax)

The CoA must be extensible to add new accounts in the future without system modifications.

**Acceptance Criteria:**

- **Given** the accounting system is initialized, **when** I access the Chart of Accounts module, **then** I should see a separate CoA structure for rental operations with all specified revenue, cost, and tax accounts
- **Given** I need to add a new account type, **when** I create a new account in the CoA, **then** the system should accept and store the new account for future transactions
- **Given** I am viewing any transaction, **when** I look at the account mapping, **then** I should see it categorized under the rental-specific CoA

### FR-2: Revenue Recognition at Payment Reconciliation

**Title:** Revenue Recognition Timing

**Statement:** As an **Accountant**, I want **revenue to be recognized at payment reconciliation after booking**, so that **financial records accurately reflect when money is received and reconciled**.

**Requirement Detail:**

The system must recognize revenue at specific points in the transaction lifecycle:

1. **Base Rental Revenue:** Recognized at payment reconciliation after booking is confirmed
2. **Late Charge Revenue:** Recognized at payment reconciliation after customer returns the vehicle and pays the late charge
3. **Delivery/Drop-off Fees:** Recognized at payment reconciliation with the base rental

The system must NOT use accrual accounting for in-progress rentals. Revenue recognition is strictly tied to payment reconciliation events.

**Acceptance Criteria:**

- **Given** a customer completes a booking and payment is reconciled, **when** the reconciliation process runs, **then** the system should create a journal entry recognizing base rental revenue and any associated delivery/drop-off fees
- **Given** a customer returns a vehicle late and pays late charges, **when** the late charge payment is reconciled, **then** the system should create a separate journal entry recognizing late charge revenue
- **Given** a rental is in progress at month-end, **when** period close occurs, **then** the system should NOT create any accrual entries for unrecognized revenue

### FR-3: Payment Reconciliation - Multiple Sources

**Title:** Multi-Source Payment Reconciliation

**Statement:** As an **Accountant**, I want **to reconcile payments from both CSV bank reports and online payment systems**, so that **all revenue is accurately recorded regardless of payment method**.

**Requirement Detail:**

The system must support two payment reconciliation methods:

1. **CSV Bank Report Reconciliation:**
   - Import daily CSV files from bank
   - Process on daily basis (excluding holidays)
   - Match payments to booking records
   - Flag discrepancies (amount mismatches, missing bookings)

2. **Online Payment Reconciliation:**
   - Receive real-time payment notifications from payment gateway
   - Automatically match to booking records
   - Process immediately upon receipt

For both methods:
- Automated discrepancy alerts sent via email
- Manual reconciliation interface for unmatched items
- Complete audit trail of all reconciliation actions

**Acceptance Criteria:**

- **Given** I have a CSV bank report file, **when** I upload it to the system, **then** the system should parse the file, match payments to bookings, and flag any discrepancies via email alert
- **Given** an online payment is received, **when** the payment gateway sends notification, **then** the system should automatically match the payment to the corresponding booking and create a journal entry in real-time
- **Given** there are payment discrepancies, **when** I view the reconciliation dashboard, **then** I should see all unmatched payments with details (amount, date, reference) and have the ability to manually match or adjust them
- **Given** payment reconciliation occurs daily, **when** excluding holidays, **then** the system should skip processing on configured holiday dates

### FR-4: Location-Based Revenue Segmentation

**Title:** Revenue Tracking by Location

**Statement:** As a **Finance Manager**, I want **all revenue to be segmented by rental location**, so that **I can analyze financial performance per location and make location-specific business decisions**.

**Requirement Detail:**

The system must:
- Associate every transaction (revenue, cost, adjustment) with a specific location
- Generate financial reports filtered by location
- Support multi-location comparison views
- Track location-specific profitability

All revenue accounts in the CoA must have location dimension for reporting purposes.

**Acceptance Criteria:**

- **Given** a rental transaction occurs, **when** the system records the journal entry, **then** it must include the location identifier from the booking
- **Given** I want to view financial performance, **when** I generate a revenue report, **then** I should be able to filter by one or multiple locations
- **Given** multiple locations exist, **when** I view the location comparison dashboard, **then** I should see revenue, costs, and profitability metrics side-by-side for each location

### FR-5: Vehicle Depreciation Management

**Title:** Straight-Line Depreciation Calculation

**Statement:** As an **Accountant**, I want **automatic depreciation calculation for each vehicle using 60-month straight-line method**, so that **vehicle costs are accurately allocated over their useful life without manual calculations**.

**Requirement Detail:**

The system must:
- Calculate monthly depreciation as: (Vehicle Purchase Price) / 60 months
- Apply same depreciation schedule for both tax and management reporting
- Track depreciation per vehicle
- Create automated journal entries for monthly depreciation
- Support annual revaluation/impairment adjustments

Each vehicle in the fleet must have:
- Purchase date
- Purchase price
- Accumulated depreciation
- Current book value
- Remaining depreciation months

**Acceptance Criteria:**

- **Given** a new vehicle is added to the fleet with purchase price $60,000, **when** the system calculates monthly depreciation, **then** it should determine $1,000 per month ($60,000 / 60)
- **Given** month-end close is triggered, **when** the depreciation calculation runs, **then** the system should create journal entries for each vehicle's monthly depreciation amount
- **Given** I view a vehicle's financial record, **when** I access the depreciation details, **then** I should see purchase price, monthly depreciation amount, accumulated depreciation, current book value, and remaining depreciation months
- **Given** annual revaluation occurs, **when** I adjust a vehicle's book value, **then** the system should recalculate the remaining depreciation based on new value and remaining months

### FR-6: Per-Vehicle Profitability Tracking

**Title:** Vehicle-Level Profit and Loss

**Statement:** As a **Finance Manager**, I want **to track profit and loss for each individual vehicle**, so that **I can identify which vehicles are profitable and make informed fleet management decisions**.

**Requirement Detail:**

The system must calculate and report per-vehicle metrics:
- Total rental revenue generated
- Depreciation expense allocated
- Maintenance costs incurred
- Fuel costs
- Insurance costs
- Net profit/loss

Reports must be available for:
- Individual vehicle view
- Fleet-wide comparison
- Time-period filtering (monthly, quarterly, yearly)
- Location filtering

**Acceptance Criteria:**

- **Given** a vehicle has generated rental transactions, **when** I view the vehicle profitability report, **then** I should see total revenue, all allocated costs (depreciation, maintenance, fuel, insurance), and calculated net profit/loss
- **Given** I manage a fleet of vehicles, **when** I view the fleet profitability dashboard, **then** I should see a ranked list of vehicles by profitability with key metrics
- **Given** I want to analyze trends, **when** I select a time period filter, **then** the system should recalculate profitability metrics for only that period
- **Given** I operate multiple locations, **when** I filter by location, **then** I should see per-vehicle profitability only for vehicles at that location

### FR-7: Early Return and Adjustment Handling

**Title:** Revenue Adjustments for Early Returns

**Statement:** As an **Accountant**, I want **to properly account for early returns with no refund, or as customer retention cost in exceptional cases**, so that **revenue adjustments are properly categorized and auditable**.

**Requirement Detail:**

The system must support:
1. **Standard Early Return (No Refund):**
   - Keep revenue already recognized
   - No journal entry adjustments needed
   - Update booking status to completed early

2. **Exceptional Early Return with Refund:**
   - Create contra-entry to original revenue
   - Record amount refunded as "Customer Retention Cost"
   - Require approval based on hierarchy
   - Maintain audit trail with approval details

**Acceptance Criteria:**

- **Given** a customer returns a vehicle early, **when** the return is processed with no refund, **then** the system should update booking status without creating any revenue adjustment entries
- **Given** an exceptional early return requires refund, **when** the accountant initiates the refund, **then** the system should prompt for approval and upon approval create journal entries debiting Customer Retention Cost and crediting Cash/Bank
- **Given** a refund is processed, **when** I audit the transaction, **then** I should see the approval chain, timestamp, and reason notes

### FR-8: No-Show Handling

**Title:** Financial Treatment of No-Shows

**Statement:** As an **Accountant**, I want **no-shows to be recorded as rental cancellation cost**, so that **I can track lost revenue opportunities and assess their financial impact**.

**Requirement Detail:**

When a customer does not show up for a rental:
- Original payment (if collected) is retained
- Transaction recorded under "Rental Cancellation Cost" account
- No revenue recognized
- No-show metrics tracked for business analysis

**Acceptance Criteria:**

- **Given** a customer with a confirmed booking does not show up, **when** the no-show is recorded in the system, **then** a journal entry should be created crediting Rental Cancellation Cost and debiting Cash (if payment was collected)
- **Given** no-shows occur over time, **when** I generate a financial report, **then** I should be able to see total rental cancellation costs by period and location

### FR-9: Manual Adjustments and Audit Trail

**Title:** Manual Journal Entries with Audit Logging

**Statement:** As an **Accountant**, I want **to make manual adjustment entries with required notes and audit trail**, so that **any manual changes to the ledger are traceable and explainable during audits**.

**Requirement Detail:**

The system must:
- Provide interface for manual journal entries
- Require:
  - Account(s) to debit/credit
  - Amount
  - Description/notes (mandatory)
  - Approval (based on amount threshold)
- Use special CoA accounts: "Manual Adjustment - Debit" and "Manual Adjustment - Credit"
- Record complete audit trail:
  - User who created entry
  - Creation timestamp
  - Approver (if required)
  - Approval timestamp

Manual adjustments should represent less than 1% of total ledger entries (success metric).

**Acceptance Criteria:**

- **Given** I need to make a manual adjustment, **when** I create a manual journal entry, **then** the system should require account selection, amount, description notes, and show approval workflow if amount exceeds threshold
- **Given** a manual entry is created, **when** I view the ledger, **then** the entry should be marked as "Manual Adjustment" with the creator's name and timestamp
- **Given** manual entry requires approval, **when** the entry is submitted, **then** it should be routed to the direct reporting line supervisor for approval before posting to ledger
- **Given** I audit manual adjustments, **when** I generate an audit report, **then** I should see all manual entries with full details (creator, approver, timestamps, notes)

### FR-10: Immutable Ledger with Reversal Entries

**Title:** Immutable Journal with Correction Mechanism

**Statement:** As an **Accountant**, I want **an immutable ledger where corrections are made via reversal entries**, so that **financial records maintain complete history and comply with accounting standards**.

**Requirement Detail:**

The system must enforce:
- No deletion of posted journal entries
- No editing of posted journal entries
- All corrections via new entries that reverse/adjust original entry
- Clear linkage between original and reversal entries
- Complete audit trail of all corrections

**Acceptance Criteria:**

- **Given** a journal entry is posted, **when** I attempt to delete or edit it, **then** the system should prevent the action and prompt me to create a reversal entry instead
- **Given** I need to correct an error, **when** I create a reversal entry, **then** the system should link it to the original entry and clearly mark it as a reversal
- **Given** I view a reversed entry, **when** I look at the details, **then** I should see both the original entry and the reversal entry with clear indication of the relationship

### FR-11: Oracle General Ledger Integration

**Title:** CSV Export to Oracle GL

**Statement:** As a **Finance Manager**, I want **automated CSV export of ledger transactions to Oracle General Ledger**, so that **rental operations are integrated with our corporate financial system**.

**Requirement Detail:**

The system must:
- Generate CSV files in Oracle GL import format
- Include all required fields: date, account code, debit/credit, amount, description, location
- Support two export frequencies:
  - Real-time: For payment reconciliation transactions
  - End-of-day: For all other transactions
- Maintain export log (what was exported, when, by whom)
- Support manual export trigger for ad-hoc needs

**Acceptance Criteria:**

- **Given** payment reconciliation occurs in real-time, **when** revenue is recognized, **then** the system should immediately generate a CSV export file for Oracle GL
- **Given** end-of-day processing runs, **when** the export process executes, **then** all non-payment transactions should be compiled into a CSV file formatted for Oracle GL import
- **Given** I need to re-export transactions, **when** I trigger a manual export for a specific date range, **then** the system should generate the CSV file and log the manual export action
- **Given** an export completes, **when** I view the export log, **then** I should see timestamp, file name, number of transactions, and user who initiated (if manual)

### FR-12: Hierarchical Approval Workflow

**Title:** Multi-Level Approval for Financial Actions

**Statement:** As a **Finance Manager**, I want **hierarchical approval workflows based on transaction thresholds**, so that **high-value financial actions receive appropriate oversight**.

**Requirement Detail:**

The system must support:
- Configurable approval thresholds by transaction type
- Three approval levels:
  - Supervisor
  - Department Head
  - Director
- Approval routing based on poster's reporting line
- Approval notifications via email
- Pending approval queue for approvers
- Timeout/escalation rules for overdue approvals

Transaction types requiring approval:
- Refunds above threshold
- Manual adjustments above threshold
- Write-offs
- Large cost entries

**Acceptance Criteria:**

- **Given** a refund request is submitted, **when** the amount exceeds the supervisor threshold but is below department head threshold, **then** the system should route for supervisor approval only
- **Given** a manual adjustment exceeds the director threshold, **when** it is submitted, **then** it should require approval from supervisor, then department head, then director in sequence
- **Given** I am an approver, **when** transactions are pending my approval, **then** I should receive email notification and see them in my approval queue
- **Given** an approval is overdue, **when** the timeout period expires, **then** the system should escalate to the next level in the hierarchy

### FR-13: Tax Management - VAT Only

**Title:** VAT Calculation and Recording

**Statement:** As an **Accountant**, I want **automated VAT calculation on all taxable transactions**, so that **tax reporting is accurate and consistent across locations**.

**Requirement Detail:**

The system must:
- Apply VAT to all rental revenue (base rental, late charges, delivery/drop-off fees)
- Use configurable VAT rate (same rate for all locations)
- Make VAT calculation rule-based and configurable by Accounting
- Generate VAT liability journal entries
- Support VAT reporting by period

No multi-currency support needed (USD only).
No variable tax rates by location.
No cross-border tax complications.

**Acceptance Criteria:**

- **Given** a rental transaction is recorded, **when** the system calculates the total, **then** it should apply the configured VAT rate and create separate journal entries for revenue and VAT liability
- **Given** the VAT rate changes, **when** an accountant updates the rate configuration, **then** all subsequent transactions should use the new rate
- **Given** period-end occurs, **when** I generate a VAT report, **then** I should see total VAT collected by period with transaction-level detail
- **Given** VAT rules need modification, **when** accounting configures the tax logic, **then** the system should apply the new rules without requiring system development

### FR-14: Financial Reporting and KPIs

**Title:** Key Financial Reports and Dashboards

**Statement:** As a **Finance Manager**, I want **automated financial reports showing key rental business metrics**, so that **I can monitor business performance and make informed decisions**.

**Requirement Detail:**

The system must provide:

**Key Performance Indicators:**
- Cash Flow (Revenue per Day)
- Profit/Loss per Vehicle
- Revenue by Location
- Cost breakdown by category

**Standard Reports:**
- Income Statement (by period, by location)
- Revenue Detail Report (by transaction type)
- Cost Analysis Report (by category)
- Fleet Profitability Report
- Payment Reconciliation Summary
- Manual Adjustment Report
- VAT Report

**Report Features:**
- Exportable to CSV/PDF
- Date range filtering
- Location filtering
- Scheduled email delivery

**Acceptance Criteria:**

- **Given** I access the dashboard, **when** the page loads, **then** I should see current period KPIs including daily revenue, total profit/loss, and revenue by location
- **Given** I need an income statement, **when** I generate the report for a specific month and location, **then** I should see all revenue categories, cost categories, and net income for that period and location
- **Given** I want to review reconciliation, **when** I generate the payment reconciliation report, **then** I should see summary of total payments reconciled, discrepancies, and outstanding items
- **Given** I need regular reports, **when** I schedule a report for weekly email delivery, **then** the system should automatically generate and email the report every week

### FR-15: Period Close Automation

**Title:** Zero-Friction Month-End Close

**Statement:** As an **Accountant**, I want **automated month-end close processes**, so that **I can close the accounting period within T+2 days with minimal manual work**.

**Requirement Detail:**

Month-end close automation must include:
- Verify all payment reconciliations complete (no outstanding items)
- Calculate and post monthly depreciation for all vehicles
- Generate automated reports:
  - Ledger completeness check
  - Manual adjustment summary (should be < 1% of entries)
  - Payment reconciliation status
  - Exception report (any unreconciled items)
- Mark period as closed (prevent backdated entries)
- Generate Oracle GL export for entire period

Close is successful when:
- All transactions exist in ledger
- All payment reconciliations done with no mismatches
- Depreciation auto-calculated
- Manual adjustments < 1% of total entries
- Process completes within T+2 days from month-end

**Acceptance Criteria:**

- **Given** month-end arrives, **when** I initiate the close process, **then** the system should run all automated checks and calculations, flagging any outstanding issues
- **Given** all reconciliations are complete, **when** the close process runs, **then** depreciation should be calculated and posted for all active vehicles automatically
- **Given** the close process completes, **when** I view the close summary, **then** I should see confirmation that all transactions are in ledger, reconciliations complete, and manual adjustments are less than 1% of total entries
- **Given** a period is closed, **when** I attempt to post a transaction to that period, **then** the system should prevent the entry and require a period reopening action
- **Given** close completes successfully, **when** T+2 days from month-end, **then** all close activities including Oracle GL export should be done

### FR-16: Data Retention and Audit Requirements

**Title:** Financial Record Retention

**Statement:** As a **Compliance Officer**, I want **financial records retained for 10 years with audit logs**, so that **we meet regulatory requirements and support audits**.

**Requirement Detail:**

The system must:
- Retain all financial records for minimum 10 years
- Store audit logs for all transactions including:
  - Creation date
  - User who created/modified
  - Approval chain (if applicable)
- Prevent deletion of records within retention period
- Support audit queries by date range, user, transaction type
- Archive older records while maintaining accessibility

**Acceptance Criteria:**

- **Given** a transaction is created, **when** I view its audit log, **then** I should see creation date, creator's username, and any modification history
- **Given** records are 10 years old, **when** the retention period completes, **then** the system should flag them for potential archival (but not auto-delete)
- **Given** I need to audit activities, **when** I query for all transactions by a specific user in a date range, **then** the system should return all matching records with complete audit details
- **Given** a record is within retention period, **when** I attempt to delete it, **then** the system should prevent deletion and display the retention policy

### FR-17: Penalties and Fees - Separate Tracking

**Title:** Distinct Revenue Categories for Fees

**Statement:** As an **Accountant**, I want **late fees, damage fees, and cleaning fees tracked in separate revenue accounts**, so that **I can analyze non-rental revenue streams and understand penalty income**.

**Requirement Detail:**

The system must create separate CoA accounts for:
- Late Return Fee Income
- Damage Fee Income  
- Cleaning Fee Income

Each must:
- Be tracked separately in reporting
- Have own revenue recognition rules
- Support location segmentation
- Include in vehicle profitability calculations (as vehicle-related income)

**Acceptance Criteria:**

- **Given** a customer incurs a late return fee, **when** payment is reconciled, **then** the system should record revenue in the "Late Return Fee Income" account separately from base rental
- **Given** damage fees are charged, **when** I generate a revenue report, **then** I should see damage fee income as a distinct line item from rental income
- **Given** I analyze fee revenue, **when** I view the fee summary report, **then** I should see breakdown by fee type (late, damage, cleaning) with totals and location segmentation

### FR-18: Cost Accounting - Cash Basis

**Title:** Cash-Based Cost Recording

**Statement:** As an **Accountant**, I want **costs recorded on cash basis when incurred**, so that **cost recognition matches actual cash outflows**.

**Requirement Detail:**

All costs except depreciation are recorded when paid:
- Maintenance costs: Recorded when workshop invoice is paid
- Fuel costs: Recorded when fuel is purchased
- Insurance: Annual premium recorded when paid (not prorated monthly)
- Overhead: Allocated monthly

No accruals for expected damages or insurance claims.
No provisions for doubtful accounts.

Depreciation is the only non-cash expense, calculated systematically.

**Acceptance Criteria:**

- **Given** a maintenance invoice is paid, **when** the payment is processed, **then** the system should immediately create a journal entry debiting Maintenance Cost and crediting Cash
- **Given** annual insurance is paid, **when** the payment is recorded, **then** the full annual amount should be expensed immediately, not spread over 12 months
- **Given** month-end occurs with unpaid invoices, **when** the close process runs, **then** no accrual entries should be created for those costs

### FR-19: System Performance and Transaction Latency

**Title:** Acceptable Transaction Processing Delay

**Statement:** As a **Finance Manager**, I want **transactions reflected in accounting within T+2 days**, so that **financial data is current enough for decision-making while allowing reasonable processing time**.

**Requirement Detail:**

The system must support:
- Real-time recording: Payment reconciliation (online) → immediately in ledger
- Next-day recording: Bank CSV import → T+1 day in ledger
- Period close: Must complete within T+2 days from month-end

Exception handling:
- Discrepancies can be resolved within T+2 window
- Manual adjustments must be approved within T+2 timeline

**Acceptance Criteria:**

- **Given** an online payment is received, **when** the gateway notification arrives, **then** the transaction should appear in the ledger within seconds (real-time)
- **Given** a bank CSV is uploaded, **when** reconciliation completes, **then** all matched transactions should be in the ledger by next business day
- **Given** month-end close is initiated, **when** T+2 days have elapsed, **then** all close activities including reconciliation, depreciation, and reporting should be complete

## Non-Functional Requirements

(To be defined)

## Dependency & Constraints

### Technical Dependencies
- **Oracle General Ledger Integration:** The system must integrate with existing Oracle GL via CSV export. Real-time sync capability for payment transactions required.
- **Payment Gateway Integration:** Must support real-time notifications from existing payment gateway(s) for online payment reconciliation.
- **Bank CSV Format:** System must parse specific CSV format(s) from bank(s) used by the company.

### Business Constraints
- **Currency:** USD only, no multi-currency support required
- **Tax Jurisdiction:** Single VAT rate across all locations, no variable tax rates
- **Locations:** System must support multiple locations, with all data segmented by location
- **Desktop Only:** Initial implementation for desktop web only (not mobile)
- **No Cross-Border:** Cross-border rentals handled with delivery/drop-off fees only, no complex tax jurisdictions

### Operational Constraints
- **Data Retention:** All financial records must be retained for 10 years minimum
- **Immutable Ledger:** Posted entries cannot be deleted or modified, only reversed with new entries
- **Processing Timeline:** Transactions must be reflected in accounting within T+2 days maximum
- **Period Close:** Month-end close must complete within T+2 days from period end
- **Manual Adjustments:** Must represent less than 1% of total ledger entries

### Future Considerations (Out of Scope)
- B2B detailed invoicing (current scope: B2C simplified only)
- Corporate account delayed payment terms
- Aging reports for receivables
- Automated chargeback journal entries
- Dual reporting (IFRS vs. local GAAP)
- Fleet utilization dashboard integration
- Demand forecasting integration
- Scenario modeling capabilities
- Split billing (customer vs. corporate sponsor)
- Mid-rental vehicle swapping
- Prepaid package unused portion refunds

## Success Metrics

### Primary Success Metrics

1. **Month-End Close Efficiency**
   - Target: Achieve period close within T+2 days from month-end, 100% of the time
   - Measure: Average days to close and % of periods closed on time

2. **Manual Adjustment Ratio**
   - Target: Manual adjustments represent less than 1% of total ledger entries
   - Measure: (Count of manual adjustment entries / Total ledger entries) × 100

3. **Payment Reconciliation Accuracy**
   - Target: 99.5% of payments automatically matched with zero discrepancies
   - Measure: (Successfully auto-matched payments / Total payments) × 100

4. **Ledger Completeness**
   - Target: 100% of rental transactions recorded in ledger within T+2 days
   - Measure: Daily audit of transaction coverage

### Secondary Success Metrics

5. **Real-Time Processing Performance**
   - Target: Online payment transactions reflected in ledger within 60 seconds
   - Measure: Average latency from payment notification to ledger entry

6. **Depreciation Automation**
   - Target: 100% of monthly depreciation calculated and posted automatically
   - Measure: % of vehicles with automated depreciation entries vs. manual

7. **Approval Workflow Efficiency**
   - Target: 90% of approvals completed within 24 hours
   - Measure: Average time from submission to final approval

8. **Oracle GL Integration Reliability**
   - Target: 99.9% successful CSV export rate
   - Measure: (Successful exports / Total export attempts) × 100

### Business Outcome Metrics

9. **Daily Cash Flow Visibility**
   - Target: Finance team can view accurate daily cash flow by 9 AM every business day
   - Measure: % of days with current cash flow data available on time

10. **Per-Vehicle Profitability Tracking**
    - Target: 100% of fleet vehicles have accurate profit/loss reporting
    - Measure: % of vehicles with complete cost and revenue allocation

11. **Location Performance Transparency**
    - Target: Location managers can access their location's financial performance within 1 day of transaction
    - Measure: User satisfaction score for location-based reporting

12. **Audit Readiness**
    - Target: Zero audit findings related to missing documentation or incomplete audit trails
    - Measure: Number of audit findings per annual audit
