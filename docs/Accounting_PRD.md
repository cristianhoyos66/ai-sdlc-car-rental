# Product Requirements Document (PRD)
## Accounting Module - Car Rental System

**Version:** 1.0  
**Date:** 2026-02-12  
**Document Owner:** Accounting Department  
**Status:** Draft

---

## 1. Executive Summary

This Product Requirements Document outlines the accounting module requirements for the car rental system. The requirements are based on comprehensive interviews with the accounting department and focus on enabling accurate financial management, compliance with accounting standards, and seamless integration with existing Oracle General Ledger systems.

### 1.1 Key Objectives
- Enable daily cash flow monitoring and profitability tracking
- Support location-based revenue segmentation
- Provide accurate financial reporting per vehicle
- Integrate seamlessly with Oracle General Ledger
- Maintain compliance with 10-year data retention requirements

---

## 2. Financial Objectives & Scope

### 2.1 Primary Financial Metrics
The system must track and report on the following key financial outcomes:

| Metric | Description | Success Criteria |
|--------|-------------|------------------|
| Daily Cash Flow | Daily income tracking | Positive daily cash flow |
| Profitability | Revenue minus cost per vehicle and location | Monthly profit reports available |
| Revenue per Day | Daily revenue tracking | Real-time visibility |
| Profit-Loss per Vehicle | Individual vehicle profitability | Per-vehicle reports available |

### 2.2 Revenue Segmentation
- **Primary Segmentation:** By location
- **Secondary Analysis:** Vehicle class, corporate vs. retail (future consideration)
- **Reporting Granularity:** Location-level with drill-down capability

---

## 3. Chart of Accounts

### 3.1 Account Structure
The system must support a **separate Chart of Accounts** specifically for the rental business.

### 3.2 Required Account Categories

#### 3.2.1 Revenue Accounts
| Account Code | Account Name | Description |
|--------------|--------------|-------------|
| 4000 | Base Rental Income | Primary rental revenue |
| 4100 | Late Charge Income | Revenue from late returns |
| 4200 | Delivery/Drop-off Income | Cross-location delivery fees |

#### 3.2.2 Cost/Expense Accounts
| Account Code | Account Name | Description |
|--------------|--------------|-------------|
| 5000 | Maintenance Cost | Workshop and vehicle maintenance |
| 5100 | Fuel Cost | Fuel expenses |
| 5200 | Vehicle Installment | Vehicle purchase installments |
| 5300 | Vehicle Insurance | Insurance premiums (annual basis) |
| 5400 | Depreciation Expense | Monthly depreciation charges |
| 5500 | Promotion Cost | Discounts and promotional expenses |
| 5600 | Rental Cancellation Cost | No-show financial impacts |
| 5700 | Customer Retention Cost | Early return refunds (exceptions) |
| 5800 | Chargeback Cost | Payment chargebacks with notes |

#### 3.2.3 Tax Accounts
| Account Code | Account Name | Description |
|--------------|--------------|-------------|
| 2100 | VAT Payable | Value Added Tax liability |
| 5900 | Tax Expense | Tax expenses |

#### 3.2.4 Adjustment Accounts
| Account Code | Account Name | Description |
|--------------|--------------|-------------|
| 6000 | Manual Adjustment (Credit) | Manual correction entries |
| 6001 | Manual Adjustment (Debit) | Manual correction entries |

### 3.3 Chart of Accounts Flexibility
- The system must allow **additional accounts to be added later**
- Each account must support detailed notes and descriptions
- Accounts must be configurable by authorized accounting personnel

---

## 4. Revenue Recognition

### 4.1 Recognition Timing

#### 4.1.1 Base Rental Revenue
- **Recognition Point:** At payment reconciliation after booking
- **Booking Flow:** Customer books → Pays → Revenue recognized
- **No accruals** for in-progress rentals straddling reporting periods

#### 4.1.2 Late Charge Revenue
- **Recognition Point:** At payment reconciliation after vehicle return
- **Flow:** Customer returns late → Late charge calculated → Customer pays → Revenue recognized

### 4.2 Early Returns
- **Standard Policy:** No refund to customer
- **Exception Cases:** May be classified as "Customer Retention Cost" (expense)
- **Financial Treatment:** Requires manual approval and journal entry

### 4.3 Extensions
- Treated as new rental transaction
- Revenue recognized upon payment for extension period

### 4.4 No-Shows
- **Financial Treatment:** Accounted as "Rental Cancellation Cost"
- **Account:** 5600 - Rental Cancellation Cost

---

## 5. Deferred & Prepaid Items

### 5.1 Deposits
- **Treatment:** Not held as liabilities
- **Policy:** Payment required upfront; no deposit system

### 5.2 Insurance Packages
- **Vehicle Insurance:** Annual basis per vehicle
- **Recognition:** Prepaid expense, amortized monthly
- **Allocation:** Cost allocated to specific vehicles

---

## 6. Cost Accounting

### 6.1 Vehicle Depreciation

#### 6.1.1 Depreciation Method
- **Method:** Straight-line depreciation
- **Period:** 60 months (5 years)
- **Formula:** Monthly Depreciation = Vehicle Purchase Price ÷ 60
- **Example:** $30,000 vehicle = $500/month depreciation

#### 6.1.2 Depreciation Schedule
- **Reporting:** Same schedule for tax and management reporting (no separate schedules)
- **Revaluation:** Yearly basis for asset impairment review
- **Tracking:** Per-vehicle depreciation tracking required

### 6.2 Other Costs
- **Recognition Basis:** Cash basis, on-demand
- **Categories:**
  - Maintenance (workshop repairs)
  - Fuel costs
  - Cleaning fees
  - Ad-hoc operational expenses

### 6.3 Overhead Allocation
- **Allocation Method:** Monthly allocation
- **Categories:** Facilities, fleet management
- **Distribution:** Can be allocated across locations or vehicles

### 6.4 Per-Vehicle Profitability
- **Requirement:** System must calculate profitability per vehicle
- **Formula:** Vehicle Revenue - (Depreciation + Maintenance + Insurance + Fuel + Allocated Overhead)
- **Reporting:** Monthly per-vehicle profitability reports

---

## 7. Billing & Invoicing

### 7.1 Invoice Format
- **Primary Format:** B2C simplified invoices
- **Customer Type:** Individual consumers (B2C focus)
- **Complexity:** Simple, customer-friendly format

### 7.2 Invoice Content
**Required Fields:**
- Invoice number
- Date
- Customer information
- Rental period
- Vehicle information
- Base rental amount
- Late charges (if applicable)
- Delivery/drop-off fees (if applicable)
- VAT amount
- Total amount
- Payment status

**Not Required:**
- Itemized surcharges
- Environmental fees
- Detailed tax breakdowns

### 7.3 Currency
- **Single Currency:** USD only
- **No multi-currency** support required
- **No FX handling** required

### 7.4 Invoice Versioning
- **Versioned History:** Not required
- **Adjustments:** Handled via credit notes and manual adjustment entries

---

## 8. Taxation

### 8.1 Tax Types
- **Applicable Tax:** VAT (Value Added Tax) only
- **No other taxes:** No luxury tax, rental-specific levies

### 8.2 Tax Rates
- **Uniformity:** Single tax rate across all locations
- **No Variable Rates:** No location-specific rates or exemptions

### 8.3 Cross-Border Rentals
- **Delivery/Drop-off Charges:** Customer charged for cross-location delivery
- **Tax Treatment:** Standard VAT applies to all charges
- **Revenue Recognition:** Delivery fees recognized as separate revenue line

### 8.4 Tax Configuration
- **Rule-Based System:** Tax logic must be configurable by Accounting
- **Authority:** Only accounting personnel can modify tax rules
- **Audit Trail:** All tax configuration changes must be logged

---

## 9. Adjustments & Corrections

### 9.1 Post-Invoice Adjustments

#### 9.1.1 Credit Notes
- Required for partial refunds (exceptional cases)
- Must reference original invoice
- Require approval workflow

#### 9.1.2 Customer Retention Costs
- Used for exceptional early return refunds
- Account: 5700 - Customer Retention Cost
- Require supervisor/department head approval

### 9.2 Manual Adjustments

#### 9.2.1 Audit Trail Requirements
- **Entry Details:** Each adjustment must include:
  - Creation date and time
  - Person In Charge (PIC) who performed the update
  - Detailed notes explaining the reason
  - Reference to original transaction (if applicable)
  
#### 9.2.2 Chart of Account
- **Account:** 6000/6001 - Manual Adjustment (Credit/Debit)
- **Ledger Entry:** Must create new ledger entry (no modification of existing entries)
- **Reversal:** To correct errors, create reversing entry + correct entry

### 9.3 Ledger Immutability
- **Immutable Journal:** Ledger entries cannot be modified or deleted
- **Append-Only:** All corrections via new entries
- **Correction Process:**
  1. Identify incorrect entry
  2. Create reversing entry
  3. Create correct entry
  4. Link all three entries with reference notes

---

## 10. Payment Reconciliation

### 10.1 Payment Methods

#### 10.1.1 Manual Reconciliation (Bank CSV)
- **Source:** Bank report in CSV format
- **Frequency:** Daily basis (excluding holidays)
- **Process:** Manual import and reconciliation
- **File Format:** CSV with standardized columns

#### 10.1.2 Automatic Online Reconciliation
- **Source:** Online payment gateway
- **Frequency:** Real-time
- **Process:** Automated matching to booking records
- **Integration:** API-based payment gateway integration

### 10.2 Reconciliation Workflow

```
Manual Process:
1. Download daily bank CSV report
2. Import into reconciliation system
3. Match payments to booking records
4. Flag discrepancies
5. Update ledger (end-of-day batch)
6. Generate reconciliation report

Automatic Process:
1. Payment gateway notification received
2. Automatic matching to booking
3. Immediate ledger update (real-time)
4. Generate transaction confirmation
```

### 10.3 Discrepancy Alerts
- **Requirement:** Automated alerts for payment mismatches
- **Delivery Method:** Email notification
- **Alert Triggers:**
  - Amount mismatches between payment and booking
  - Payment delays exceeding expected timeline
  - Unmatched payments (no corresponding booking)
  - Duplicate payment attempts

### 10.4 Expected Latency
- **Real-time (Online):** Immediate ledger reflection
- **CSV Import:** T+2 days maximum
- **Period Close:** T+2 days for month-end close

---

## 11. Outstanding Balances & Accounts Receivable

### 11.1 Payment Policy
- **Requirement:** Pay first - no delayed payments allowed
- **Customer Types:** No corporate account extensions
- **Insurance Partners:** No special payment terms

### 11.2 Overdue Balances
- **Standard:** Should be zero (all prepaid)
- **Accumulated Balances:** Only for late charges post-return
- **Collections:** Third-party debt collectors for significant unpaid late charges

### 11.3 Aging Reports
- **Requirement:** Not required (no AR aging needed)
- **Rationale:** Prepayment model eliminates traditional AR

---

## 12. Refunds & Chargebacks

### 12.1 Refund Approval Workflow

| Amount Threshold | Approver Level |
|------------------|----------------|
| < $500 | Marketing Supervisor |
| $500 - $2,000 | Marketing Department Head |
| > $2,000 | Marketing Director |

### 12.2 Chargeback Handling
- **Account:** 5800 - Chargeback Cost
- **Categorization:** Must include notes on ledger entry
  - Fraud-related
  - Service dispute
  - Processing error
- **Automated Journal Entries:** Not required
- **Process:** Manual entry with detailed documentation

---

## 13. Integration Requirements

### 13.1 Oracle General Ledger Integration

#### 13.1.1 System Information
- **Target System:** Oracle General Ledger
- **Integration Method:** CSV file-based export/import
- **No API Integration:** File-based only

#### 13.1.2 Sync Latency Requirements

| Transaction Type | Sync Requirement | Batch Process |
|------------------|------------------|---------------|
| Payment Reconciliation (Online) | Real-time | Immediate export |
| Payment Reconciliation (CSV) | End-of-day | Daily batch |
| Depreciation Calculations | End-of-day | Daily batch |
| Manual Adjustments | End-of-day | Daily batch |
| Other Transactions | End-of-day | Daily batch |

#### 13.1.3 CSV Export Format
The system must generate Oracle GL-compatible CSV files with the following structure:

```csv
Date,Account_Code,Debit,Credit,Description,Reference,Location,Vehicle_ID,Transaction_Type,PIC
```

**Required Fields:**
- Date: Transaction date (YYYY-MM-DD)
- Account_Code: Chart of Accounts code
- Debit: Debit amount (USD)
- Credit: Credit amount (USD)
- Description: Transaction description
- Reference: Reference number (booking ID, invoice number)
- Location: Location code
- Vehicle_ID: Vehicle identifier (if applicable)
- Transaction_Type: Category of transaction
- PIC: Person In Charge

### 13.2 Data Export Schedule
- **Real-time Transactions:** Exported immediately to staging
- **Batch Transactions:** Daily export at end of business day
- **Manual Export:** On-demand export capability for accounting team
- **Retention:** Export files retained for 10 years

---

## 14. Reporting & Compliance

### 14.1 Statutory Reports

The system must generate the following reports:

#### 14.1.1 Rental-Specific Reports
1. **Rental Agreement/Contract Reports**
   - All active rentals
   - Revenue by contract
   - Contract modifications

2. **Income and Tax Reports**
   - Revenue by category
   - VAT collected and payable
   - Tax reconciliation

3. **Vehicle Maintenance and Safety Reports**
   - Maintenance costs per vehicle
   - Safety compliance costs
   - Preventive maintenance schedules

4. **Fleet and Vehicle Availability Reports**
   - Vehicle utilization rates
   - Revenue per available vehicle
   - Idle vehicle costs

5. **Accident/Damage Reports**
   - Damage costs by incident
   - Insurance claim tracking
   - Vehicle downtime costs

6. **Driver's License Verification Reports**
   - Verification compliance tracking
   - Associated administrative costs

### 14.2 Financial Accounting Standards
- **Primary Standard:** Local GAAP only
- **IFRS:** Not required
- **Dual Reporting:** Not needed

### 14.3 Key Performance Indicators (KPIs)

The system must calculate and display:

| KPI | Calculation | Reporting Frequency |
|-----|-------------|---------------------|
| Revenue per Day | Total Daily Revenue ÷ Days | Daily, Monthly aggregate |
| Profit-Loss per Vehicle | Vehicle Revenue - Vehicle Costs | Monthly |
| Cash Flow | Daily Income - Daily Expenses | Daily |
| Fleet Utilization | Rental Days ÷ Available Days | Monthly |
| Average Daily Rate | Total Revenue ÷ Total Rental Days | Monthly |

### 14.4 Dashboard Requirements
- **Fleet Utilization Integration:** Not required in Phase 1
- **Financial Performance:** Standalone financial dashboards
- **Real-time Metrics:** Daily cash flow and revenue visible in real-time

---

## 15. Internal Controls & Governance

### 15.1 Approval Hierarchy

Financial approvals must follow hierarchical structure based on transaction threshold:

| Transaction Amount | Approval Level |
|-------------------|----------------|
| < $1,000 | Supervisor |
| $1,000 - $5,000 | Department Head |
| > $5,000 | Director |

### 15.2 Role Separation
- **Poster Role:** Creates and enters financial transactions
- **Approver Role:** Must be the direct reporting line of the poster
- **System Enforcement:** System must prevent self-approval
- **Audit Trail:** All approval actions logged with timestamp and approver ID

### 15.3 Automated Control Checks
- **Requirement:** Not required in Phase 1
- **Future Consideration:** 
  - Negative revenue detection
  - Duplicate invoice prevention
  - Threshold breach alerts

---

## 16. Data Retention & Audit

### 16.1 Record Retention
- **Retention Period:** 10 years for all financial records
- **Scope:** All transactions, ledger entries, invoices, reports
- **Format:** Original format plus archived format
- **Compliance:** Must comply with local jurisdiction requirements

### 16.2 Audit Log Requirements

#### 16.2.1 Minimum Log Fields
- **Creation Date:** Date and time of record creation
- **PIC (Person In Charge):** User who created or modified the record
- **Action Type:** Create, Update, Delete attempt
- **Before/After Values:** For updates, log previous and new values

#### 16.2.2 Immutable Ledger
- **Ledger Journal:** Append-only, immutable storage
- **Corrections:** Only via reversing entries
- **Deletion:** Not permitted
- **Modification:** Not permitted - must create new correcting entry

### 16.3 Audit Trail Access
- **Retention:** 10 years
- **Access Control:** Audit logs accessible only to:
  - Accounting Department
  - Internal Audit
  - External Auditors (with approval)
- **Export:** Audit logs must be exportable for external audit

---

## 17. Penalties & Fees

### 17.1 Fee Categories

Each fee type must be tracked in separate revenue/cost accounts:

| Fee Type | Account | Recognition Timing |
|----------|---------|-------------------|
| Late Return Fees | 4100 - Late Charge Income | Upon payment after return |
| Damage Fees | Cost of repair (5000 - Maintenance) | Cash basis |
| Cleaning Fees | 5000 - Maintenance Cost | Cash basis |
| Delivery Fees | 4200 - Delivery/Drop-off Income | Upon payment |

### 17.2 Separate Chart of Accounts
- **Requirement:** Each fee category in separate COA line
- **Rationale:** Enable granular revenue and cost analysis
- **No Offset Accounts:** Direct revenue or cost classification

---

## 18. Accruals & Provisions

### 18.1 Damage and Claims
- **Accrual Policy:** No accruals for expected damages
- **Recognition:** Cash basis only
- **Insurance Claims:** Recognized when received

### 18.2 Doubtful Accounts
- **Provision:** None required in Phase 1
- **Rationale:** Prepayment model minimizes bad debt risk
- **Corporate Customers:** Not applicable (no credit terms)

---

## 19. Seasonal & Forecasting

### 19.1 Rolling Forecasts
- **Requirement:** Not required in Phase 1
- **Future Consideration:** Weekly or monthly forecasting capability

### 19.2 Cross-Functional Inputs
- **Demand Forecasts:** Not required in Phase 1
- **Fleet Planning:** Not required in Phase 1
- **Scenario Modeling:** Not required in Phase 1

---

## 20. Compliance & Regulatory

### 20.1 Regulatory Audits
- **Current Requirement:** Manual handling
- **Automated Support:** Not required in Phase 1
- **Future:** System must support audit data extraction

### 20.2 Compliance Checklists
- **Requirement:** Not required in Phase 1
- **Manual Process:** Compliance managed outside system initially

---

## 21. Performance & SLA

### 21.1 Transaction Latency
- **Maximum Delay:** T+2 days between transaction occurrence and system reflection
- **Standard:** Most transactions processed within 24 hours
- **Real-time:** Payment reconciliation (online) immediate

### 21.2 Period Close
- **Target:** T+2 days for month-end close
- **Requirements for Zero-Friction Close:**
  1. All accountable transactions exist in ledger
  2. All payment reconciliation complete with no mismatches
  3. Accruals and depreciation auto-calculated
  4. Manual adjustment entries < 1% of total ledger entries
  5. Close process completed by end of month + 2 days

### 21.3 System Performance
- **Report Generation:** < 30 seconds for standard reports
- **CSV Export:** < 2 minutes for daily export files
- **Dashboard Loading:** < 5 seconds for financial dashboards

---

## 22. Exceptions & Edge Cases

### 22.1 Out of Scope (Phase 1)

The following scenarios are **not required** in Phase 1:

1. **Split Billing:** Customer vs. corporate sponsor payment splitting
2. **Mid-Rental Vehicle Swap:** No vehicle swapping during active rental
3. **Prepaid Package Refunds:** No prorated refunds for unused portions

### 22.2 Future Considerations

These scenarios may be addressed in future phases based on business needs:

- Multi-currency support
- Corporate credit accounts
- Advanced forecasting and scenario modeling
- Automated compliance checklists
- Real-time API integration with Oracle GL

---

## 23. Security & Data Access

### 23.1 Sensitive Data (Phase 1)
- **Access Restrictions:** Not required in Phase 1
- **Future:** Cost and margin data may require role-based restrictions

### 23.2 Data Anonymization
- **Requirement:** Not required in Phase 1
- **Operational Data:** Full access for finance stakeholders

---

## 24. Automation Opportunities

### 24.1 Current Manual Processes
- **Phase 1 Focus:** Minimal automation
- **Future:** Opportunity to automate Excel-based reconciliation processes

### 24.2 Month-End Close Automation

**Goal:** Zero-friction month-end close

**Automation Requirements:**
1. **Automated Depreciation:**
   - Daily calculation of depreciation per vehicle
   - Automatic journal entry generation
   - Monthly aggregation

2. **Payment Reconciliation:**
   - Automatic matching (online payments)
   - Batch import with validation (CSV payments)
   - Discrepancy alerts

3. **Accrual Calculation:**
   - Automated prepaid insurance amortization
   - Other prepaid expense allocation

4. **Validation Rules:**
   - Balance checks (debits = credits)
   - Account balance validations
   - Missing transaction detection

5. **Performance Target:**
   - Manual adjustments < 1% of total entries
   - Close by T+2 days
   - Zero unreconciled transactions

---

## 25. Technical Specifications

### 25.1 System Architecture Requirements

#### 25.1.1 Data Storage
- **Ledger Database:** Append-only, immutable storage
- **Transaction Database:** Relational database with ACID compliance
- **Audit Logs:** Separate audit log database with write-once storage
- **Archive Storage:** Long-term storage for 10-year retention

#### 25.1.2 Integration Points
- **Oracle GL Export:** Daily CSV file generation
- **Bank CSV Import:** Manual upload interface
- **Payment Gateway:** Real-time webhook/callback integration
- **Reporting Engine:** SQL-based reporting with export capabilities

### 25.2 Data Validation Rules

| Field | Validation Rule |
|-------|----------------|
| Transaction Date | Must be within valid fiscal period |
| Account Code | Must exist in Chart of Accounts |
| Amount | Must be positive, max 2 decimal places |
| Debit/Credit | Total debits must equal total credits |
| Reference | Required for all transactions |
| Vehicle ID | Must exist in fleet database (if applicable) |

### 25.3 User Interface Requirements

#### 25.3.1 Core Screens
1. **Dashboard:** Key financial metrics and KPIs
2. **Chart of Accounts Management:** View/edit COA structure
3. **Transaction Entry:** Manual journal entry interface
4. **Payment Reconciliation:** Import and match payments
5. **Reports:** Generate and export financial reports
6. **Approval Queue:** Review and approve pending transactions

#### 25.3.2 Usability
- Simple, intuitive interface for accounting staff
- Bulk operations support (import, export, approval)
- Error messages with clear guidance
- Confirmation dialogs for irreversible actions

---

## 26. Success Metrics

### 26.1 Implementation Success Criteria

| Metric | Target | Measurement |
|--------|--------|-------------|
| Month-end close time | ≤ T+2 days | Actual days to close |
| Manual adjustment rate | < 1% of entries | Manual entries ÷ total entries |
| Payment reconciliation accuracy | > 99% | Matched payments ÷ total payments |
| System uptime | > 99.5% | Available hours ÷ total hours |
| User satisfaction | > 4.0/5.0 | User survey score |

### 26.2 Business Value Metrics

| Metric | Baseline | Target | Timeline |
|--------|----------|--------|----------|
| Time to close month-end | Manual (5+ days) | 2 days | 3 months post-launch |
| Daily cash flow visibility | Next day | Real-time | Launch |
| Per-vehicle profitability | Manual calculation | Automated daily | Launch |
| Reconciliation errors | ~5% manual error rate | < 1% | 6 months post-launch |

---

## 27. Implementation Phases

### 27.1 Phase 1: Core Accounting (MVP)
**Duration:** 3-4 months

**Scope:**
- Chart of Accounts setup
- Basic transaction recording
- Manual journal entries
- CSV export to Oracle GL
- Basic reporting (P&L, cash flow)

**Deliverables:**
- Functional accounting module
- Oracle GL integration (CSV)
- Core financial reports

### 27.2 Phase 2: Payment Reconciliation
**Duration:** 2-3 months

**Scope:**
- Bank CSV import
- Online payment gateway integration
- Automated reconciliation
- Discrepancy alerts

**Deliverables:**
- Automated payment matching
- Reconciliation reports
- Email alert system

### 27.3 Phase 3: Automation & Optimization
**Duration:** 2-3 months

**Scope:**
- Automated depreciation calculation
- Month-end close automation
- Enhanced reporting
- Per-vehicle profitability tracking

**Deliverables:**
- Automated month-end processes
- Advanced financial dashboards
- Depreciation automation

### 27.4 Phase 4: Advanced Features (Future)
**Scope (TBD):**
- Multi-currency support
- Corporate account management
- Real-time Oracle API integration
- Advanced forecasting

---

## 28. Risks & Mitigation

### 28.1 Identified Risks

| Risk | Impact | Probability | Mitigation Strategy |
|------|--------|-------------|---------------------|
| Oracle GL integration complexity | High | Medium | Start with simple CSV format; validate early |
| Data migration from manual systems | High | High | Phased migration; validate accuracy at each step |
| User adoption resistance | Medium | Medium | Training program; change management support |
| Payment reconciliation errors | High | Low | Automated validation; manual review workflow |
| Period close delays | High | Low | Automated processes; clear runbook |

### 28.2 Dependencies

**Critical Dependencies:**
- Oracle GL CSV format specification
- Bank CSV file format specification
- Payment gateway API documentation
- Existing manual process documentation

---

## 29. Assumptions

1. **Technology Stack:** Modern web-based application with relational database
2. **User Count:** <20 concurrent accounting users initially
3. **Transaction Volume:** Scalable to handle growth in rental transactions
4. **Network:** Stable internet connectivity for cloud-based deployment
5. **Oracle GL:** CSV import process is well-established and documented
6. **Bank Files:** Standardized CSV format available from banking partner
7. **Payment Gateway:** Webhook/API integration is available and documented

---

## 30. Out of Scope

The following items are explicitly **out of scope** for this PRD:

1. Tax filing automation
2. Payroll processing
3. Vendor payment management (non-rental related)
4. Fixed asset management (non-vehicle assets)
5. Budget planning and approval
6. Cash forecasting models
7. Multi-entity consolidation
8. Customer credit management
9. Purchase order management
10. Inventory management (parts, supplies)

---

## 31. Glossary

| Term | Definition |
|------|------------|
| B2C | Business to Consumer |
| COA | Chart of Accounts |
| CSV | Comma-Separated Values |
| FX | Foreign Exchange |
| GAAP | Generally Accepted Accounting Principles |
| GL | General Ledger |
| IFRS | International Financial Reporting Standards |
| KPI | Key Performance Indicator |
| P&L | Profit and Loss |
| PIC | Person In Charge |
| PRD | Product Requirements Document |
| T+N | Transaction date plus N days |
| VAT | Value Added Tax |

---

## 32. Approval & Sign-off

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Accounting Department Head | | | |
| IT Director | | | |
| CFO | | | |
| Product Owner | | | |

---

## 33. Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-02-12 | Accounting Department | Initial version based on requirement gathering interviews |

---

## 34. Appendices

### Appendix A: Sample Chart of Accounts Structure
```
4000 - Revenue
  4000 - Base Rental Income
  4100 - Late Charge Income
  4200 - Delivery/Drop-off Income

5000 - Cost of Sales
  5000 - Maintenance Cost
  5100 - Fuel Cost
  5200 - Vehicle Installment
  5300 - Vehicle Insurance
  5400 - Depreciation Expense

5500 - Operating Expenses
  5500 - Promotion Cost
  5600 - Rental Cancellation Cost
  5700 - Customer Retention Cost
  5800 - Chargeback Cost

5900 - Taxes
  5900 - Tax Expense

2000 - Liabilities
  2100 - VAT Payable

6000 - Adjustments
  6000 - Manual Adjustment (Credit)
  6001 - Manual Adjustment (Debit)
```

### Appendix B: Oracle GL CSV Export Sample
```csv
Date,Account_Code,Debit,Credit,Description,Reference,Location,Vehicle_ID,Transaction_Type,PIC
2026-02-12,4000,0.00,500.00,"Rental payment for booking #12345",BOOK-12345,LOC-01,VEH-456,Rental Revenue,john.doe
2026-02-12,2100,0.00,50.00,"VAT on booking #12345",BOOK-12345,LOC-01,VEH-456,Tax,john.doe
2026-02-12,1000,550.00,0.00,"Payment received via gateway",PAY-67890,LOC-01,,Payment,system
```

### Appendix C: Depreciation Calculation Example
```
Vehicle Purchase Price: $30,000
Depreciation Period: 60 months
Monthly Depreciation: $30,000 ÷ 60 = $500/month

Journal Entry (Monthly):
Debit: Depreciation Expense (5400) - $500
Credit: Accumulated Depreciation (Asset) - $500
```

### Appendix D: Payment Reconciliation Process Flow
```
1. Payment Received
   ├─ Online Payment Gateway
   │  ├─ Webhook received
   │  ├─ Match to booking (auto)
   │  ├─ Create GL entry (real-time)
   │  └─ Send confirmation
   │
   └─ Bank CSV
      ├─ Download daily file
      ├─ Import to system
      ├─ Match to bookings
      ├─ Flag discrepancies
      ├─ Manual review (if needed)
      └─ Create GL entries (batch)

2. Discrepancy Handling
   ├─ Amount mismatch → Email alert
   ├─ No matching booking → Email alert
   ├─ Duplicate payment → Email alert
   └─ Manual investigation required
```

---

**END OF DOCUMENT**
