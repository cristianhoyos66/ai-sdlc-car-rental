# PRD - Car Management

## Document Information

- **Product / Feature Name:** Car Management
- **Author:** copilot-swe-agent[bot]
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
  - [FR-1: Vehicle Fleet Onboarding](#fr-1-vehicle-fleet-onboarding)
  - [FR-2: Vehicle Lifecycle Management](#fr-2-vehicle-lifecycle-management)
  - [FR-3: Location and Inventory Management](#fr-3-location-and-inventory-management)
  - [FR-4: Vehicle Availability Management](#fr-4-vehicle-availability-management)
  - [FR-5: Vehicle Status and Telemetry](#fr-5-vehicle-status-and-telemetry)
  - [FR-6: Pickup and Delivery Operations](#fr-6-pickup-and-delivery-operations)
  - [FR-7: Return and Check-In Process](#fr-7-return-and-check-in-process)
  - [FR-8: Maintenance Scheduling](#fr-8-maintenance-scheduling)
  - [FR-9: Damage and Incident Handling](#fr-9-damage-and-incident-handling)
  - [FR-10: Fuel and Energy Management](#fr-10-fuel-and-energy-management)
  - [FR-11: Vehicle Utilization Analytics](#fr-11-vehicle-utilization-analytics)
  - [FR-12: Vehicle Swaps and Substitutions](#fr-12-vehicle-swaps-and-substitutions)
  - [FR-13: Reservation Extensions and Early Returns](#fr-13-reservation-extensions-and-early-returns)
  - [FR-14: Alerts and Notifications](#fr-14-alerts-and-notifications)
  - [FR-15: Operational Safeguards](#fr-15-operational-safeguards)
- [Non-Functional Requirements](#non-functional-requirements)
- [Dependency & Constraints](#dependency--constraints)
- [Success Metrics](#success-metrics)

## Overview

### Background

Our company is expanding from car sales into the car rental business. This is a new business line requiring a comprehensive car management system to handle the complete lifecycle of rental vehicles from acquisition to disposal. Unlike car sales, rental operations require real-time tracking, continuous maintenance scheduling, and operational workflows for pickup, delivery, and return processes.

### Objective

Build a car management system that enables efficient operations of a car rental fleet, including vehicle onboarding, location-based inventory management, maintenance scheduling, pickup/delivery operations, damage handling, and utilization analytics to support the new car rental business line.

### Goals

1. Enable efficient onboarding and tracking of rental vehicles with complete lifecycle management
2. Support real-time and planned vehicle availability for reservations across multiple locations
3. Automate maintenance scheduling based on mileage to minimize vehicle downtime
4. Streamline pickup, delivery, and return operations with proper documentation and inspection workflows
5. Provide visibility into fleet utilization and operational metrics for data-driven decision making
6. Ensure operational safeguards to prevent vehicle assignment with critical issues (maintenance overdue, insurance expiration)
7. Support geofencing and telemetry monitoring for fleet security

## Problem Statement

As our company transitions from car sales to car rental, we lack the operational systems needed to manage a rental fleet effectively. Car rental requires fundamentally different capabilities than car sales:

- **Vehicle Lifecycle Management:** Unlike sales inventory that has a linear path (acquisition → sale), rental vehicles have complex lifecycles involving active rental periods, maintenance windows, damage incidents, and location transfers
- **Real-Time Availability:** Rental operations need instant visibility into which vehicles are available, where they are located, and their condition status
- **Continuous Maintenance:** Vehicles require predictable maintenance scheduling based on usage patterns to prevent operational disruptions
- **Operational Workflows:** Pickup, delivery, and return processes require standardized inspection procedures, documentation capture, and damage assessment
- **Multi-Location Coordination:** Vehicles need to be tracked across locations with proper inventory management and transfer workflows

**Who is affected:**
- Fleet managers responsible for vehicle lifecycle and utilization
- Operations staff handling pickup, delivery, and return processes
- Maintenance crew scheduling and performing vehicle service
- Business managers making decisions based on fleet performance metrics

**Why is this important now:**
The company is launching a new car rental business line and requires these foundational capabilities from day one to operate efficiently, maintain customer satisfaction, and achieve business viability. Without proper car management systems, we risk operational chaos, poor customer experience, and business failure.

## Functional Requirements

### FR-1: Vehicle Fleet Onboarding

**Title:** Onboard New Vehicles to Rental Fleet

**Statement:** **As a** fleet manager, **I want** to register new vehicles entering the rental fleet with complete information, **so that** I can track and manage vehicles throughout their lifecycle.

**Requirement Detail:**

When a new vehicle is acquired for the rental fleet, the system must capture comprehensive vehicle information to enable proper tracking, maintenance scheduling, insurance management, and operational assignments. The vehicle data forms the foundation for all subsequent fleet management operations.

The system must support vehicle classification into categories (Economy small/medium, Luxury small/medium) and capture technical specifications (seats, fuel type) that influence rental pricing and customer matching.

**Acceptance Criteria:**

- **Given** a fleet manager is adding a new vehicle to the system  
  **When** they enter the vehicle information  
  **Then** the system must capture and store:
  - VIN (Vehicle Identification Number)
  - License plate
  - Purchase date
  - Purchase cost
  - Insurance details (policy number, provider, expiration date)
  - Odometer reading at acquisition
  - Vehicle type (brand, model, manufacturing year)
  - Size classification (small, medium)
  - Class (luxury, economy)
  - Number of seats (4 or 7)
  - Fuel type (gas, electric, hybrid)
  - Ownership information (must be owned by company)
  - Home location assignment

- **Given** a fleet manager submits vehicle information  
  **When** all required fields are completed  
  **Then** the vehicle is created with initial status "incoming"

- **Given** a vehicle is being registered  
  **When** the VIN or license plate already exists in the system  
  **Then** the system displays an error preventing duplicate registration

### FR-2: Vehicle Lifecycle Management

**Title:** Manage Vehicle Lifecycle Status

**Statement:** **As a** fleet manager, **I want** to track and update vehicle lifecycle status, **so that** I can manage the operational state of each vehicle from acquisition to disposal.

**Requirement Detail:**

Vehicles progress through distinct lifecycle stages that determine their availability for rental and operational handling. The system must support manual status transitions to reflect the vehicle's current operational state.

Status transitions control vehicle availability - only vehicles in "active" status can be assigned to rentals, while other statuses indicate the vehicle is not available for customer assignment.

**Acceptance Criteria:**

- **Given** a vehicle exists in the system  
  **When** a fleet manager views the vehicle  
  **Then** the system displays the current lifecycle status

- **Given** a fleet manager needs to change vehicle status  
  **When** they select a new status  
  **Then** the system supports these lifecycle statuses:
  - Incoming: Vehicle acquired but not yet operational
  - Active: Vehicle operational and available for rental
  - Maintenance: Vehicle undergoing service or repair
  - Decommissioning: Vehicle being prepared for disposal
  - Sold: Vehicle disposed/sold

- **Given** a vehicle's lifecycle status changes  
  **When** the status is updated  
  **Then** the system records the status change with timestamp

- **Given** a vehicle status is not "active"  
  **When** the reservation system checks availability  
  **Then** the vehicle is excluded from available inventory

### FR-3: Location and Inventory Management

**Title:** Track Vehicle Location and Transfers

**Statement:** **As a** fleet manager, **I want** to assign vehicles to specific home locations and track location transfers, **so that** I can manage inventory distribution across our rental network.

**Requirement Detail:**

Each vehicle belongs to a specific "home location" at any given time. Location assignment determines where the vehicle is available for rental pickup and influences operational planning.

When business needs require moving vehicles between locations, the system must track the transfer including updated home location and associated costs charged to company pool.

**Acceptance Criteria:**

- **Given** a new vehicle is onboarded  
  **When** the vehicle is registered  
  **Then** the system requires assignment to a home location

- **Given** a vehicle exists in the system  
  **When** viewing the vehicle details  
  **Then** the system displays the current home location

- **Given** a fleet manager needs to transfer a vehicle  
  **When** they initiate a location transfer  
  **Then** the system:
  - Updates the vehicle's home location
  - Records transfer in home location history with timestamp
  - Captures transfer cost (charged to company pool)
  - Maintains historical record of all previous locations

- **Given** a vehicle has location transfer history  
  **When** a fleet manager views the history  
  **Then** the system displays chronological list showing:
  - Previous locations
  - Transfer dates
  - Transfer costs

### FR-4: Vehicle Availability Management

**Title:** Calculate Real-Time and Planned Vehicle Availability

**Statement:** **As a** reservation system, **I want** to determine vehicle availability in real-time and for future dates, **so that** customers can only book vehicles that are actually available.

**Requirement Detail:**

Vehicle availability is complex calculation considering multiple factors: current lifecycle status, existing reservations, maintenance schedules, and insurance validity. The system must provide two availability views:

1. **Real-time availability**: Vehicles available 2 hours from current time
2. **Planned availability**: Vehicles available from tomorrow (H+1) up to 30 days out (H+30)

Availability is location-specific - vehicles are only available at their current home location. Automatic allocation operates on first-come-first-serve basis without prioritization or overbooking.

**Acceptance Criteria:**

- **Given** the reservation system requests available vehicles  
  **When** checking real-time availability  
  **Then** the system returns vehicles available 2 hours from current time

- **Given** the reservation system requests future availability  
  **When** specifying a date range  
  **Then** the system returns vehicles available from H+1 to H+30 days

- **Given** multiple factors affect availability  
  **When** calculating available vehicles  
  **Then** the system excludes vehicles that:
  - Have lifecycle status other than "active"
  - Have existing reservations overlapping the requested period
  - Have maintenance scheduled during the requested period
  - Have insurance expiring within 7 days of rental start
  - Have critical faults preventing operation

- **Given** a customer searches for vehicles at a location  
  **When** the system calculates availability  
  **Then** only vehicles with home location matching the search are included

- **Given** multiple reservation requests arrive  
  **When** allocating vehicles  
  **Then** the system assigns vehicles on first-come-first-serve basis

- **Given** all vehicles of a requested type are unavailable  
  **When** checking availability  
  **Then** the system returns no vehicles (no overbooking allowed)

### FR-5: Vehicle Status and Telemetry

**Title:** Monitor Vehicle Location via GPS Telemetry

**Statement:** **As a** fleet manager, **I want** to receive real-time GPS location updates and geofencing alerts, **so that** I can monitor fleet security and prevent unauthorized vehicle use.

**Requirement Detail:**

The system integrates with vehicle GPS devices to track location and detect when vehicles leave permitted areas. This capability supports fleet security and helps identify potential theft or misuse.

GPS data updates frequently to support operational dashboards showing current fleet location distribution.

**Acceptance Criteria:**

- **Given** a vehicle has GPS telemetry enabled  
  **When** the vehicle is in operation  
  **Then** the system receives GPS location updates at maximum 5-minute intervals

- **Given** GPS location data is received  
  **When** the data is processed  
  **Then** the system updates the vehicle's current location coordinates

- **Given** a vehicle has defined geofencing boundaries  
  **When** GPS indicates the vehicle has left the permitted area  
  **Then** the system generates an alert notification

- **Given** fleet managers need visibility  
  **When** accessing operational dashboard  
  **Then** the system displays real-time vehicle locations on a map view

### FR-6: Pickup and Delivery Operations

**Title:** Manage Vehicle Pickup and Delivery Service

**Statement:** **As an** operations staff member, **I want** to schedule and document vehicle pickup and delivery to customers, **so that** customers receive convenient service with proper handover documentation.

**Requirement Detail:**

The system supports optional pickup/delivery service at customer-specified locations (home, hotel) for additional cost. Service is available daily between 06:00-19:00 local time with manual route planning by operations staff.

Handover requires comprehensive documentation including photos, electronic form with signature, timestamp, geolocation, and identity verification. This ensures proper proof-of-handover and protects both company and customer.

**Acceptance Criteria:**

- **Given** a customer requests pickup/delivery service  
  **When** scheduling the service  
  **Then** the system:
  - Accepts delivery address
  - Validates requested time is between 06:00-19:00 local time
  - Calculates additional delivery cost
  - Assigns the request for manual route planning

- **Given** operations staff performs delivery  
  **When** completing the handover  
  **Then** the system requires capture of:
  - Multiple photos of vehicle exterior and interior
  - Electronic form with customer signature
  - Timestamp of handover
  - GPS geolocation of handover
  - Driver license scan
  - Customer selfie for identity verification match
  - Current odometer reading
  - Current fuel level

- **Given** handover documentation is incomplete  
  **When** operations staff attempts to complete delivery  
  **Then** the system prevents completion until all required items are captured

- **Given** identity verification is performed  
  **When** comparing driver license and selfie  
  **Then** the system validates against national identifier or passport

- **Given** delivery is completed  
  **When** all documentation is captured  
  **Then** the system updates vehicle status to "on-rent" and records handover details

### FR-7: Return and Check-In Process

**Title:** Inspect and Check-In Returned Vehicles

**Statement:** **As an** operations staff member, **I want** to conduct standardized vehicle inspection during return, **so that** I can identify damage, assess charges, and prepare the vehicle for next rental.

**Requirement Detail:**

Vehicle return requires thorough inspection following standardized checklist to identify any damage or issues. The inspection determines whether additional charges apply (damage repair, fuel difference) and whether the vehicle is ready for re-rental.

Inspection must be completed during pickup/delivery hours (06:00-19:00 local time) - after-hours returns are not supported. Chargeable damage requires visible evidence (body dents, torn seats); normal engine/electrical wear is not charged.

After return inspection, vehicles require 1-day turnaround before becoming available for next rental.

**Acceptance Criteria:**

- **Given** a vehicle is being returned  
  **When** operations staff initiates return inspection  
  **Then** the system provides standardized inspection form with:
  - Photo capture points (multiple angles)
  - Exterior damage checklist
  - Interior condition checklist
  - Engine check
  - Electrical instruments check
  - Fuel level measurement
  - Odometer reading

- **Given** inspection photos are taken  
  **When** damage is identified  
  **Then** staff marks damage on vehicle diagram showing positional mapping

- **Given** damage assessment is completed  
  **When** determining charge status  
  **Then** the system applies chargeable damage rules:
  - Chargeable: Visible body dent, torn leather seats, accident damage
  - Normal wear: Engine wear, electrical instrument wear

- **Given** fuel level at return differs from delivery level  
  **When** calculating charges  
  **Then** the system calculates fuel charge for the difference

- **Given** vehicle return is completed  
  **When** inspection form is submitted  
  **Then** the system:
  - Records inspection details and photos
  - Calculates total charges (damage + fuel + any late fees)
  - Updates vehicle status
  - Sets vehicle availability to 1 day after return date (turnaround time)

- **Given** return occurs between 06:00-15:00 local time  
  **When** staff assignment is needed  
  **Then** the system enables manual task assignment same day

- **Given** return occurs after 15:00 local time  
  **When** staff assignment is needed  
  **Then** the system enables manual task assignment for next day

- **Given** customer attempts after-hours return  
  **When** validating return time  
  **Then** the system prevents returns outside 06:00-19:00 window

### FR-8: Maintenance Scheduling

**Title:** Schedule Mileage-Based Vehicle Maintenance

**Statement:** **As a** maintenance crew member, **I want** to schedule routine maintenance based on vehicle mileage, **so that** vehicles receive timely service and remain operationally safe.

**Requirement Detail:**

Routine maintenance follows mileage-based schedule every 10,000-12,000 kilometers. Maintenance crew creates schedule at 10,000 km increments, prioritizing maintenance over existing reservations.

When maintenance is scheduled for a specific day, the vehicle becomes unavailable for rental that day. Maintenance blocks automatically remove the vehicle from available inventory.

**Acceptance Criteria:**

- **Given** a vehicle's odometer approaches maintenance threshold  
  **When** crew reviews maintenance scheduling  
  **Then** the system highlights vehicles within 1,000 km of 10,000 km multiples

- **Given** maintenance crew schedules service  
  **When** creating a maintenance block  
  **Then** the system:
  - Accepts maintenance date
  - Accepts maintenance type description
  - Sets vehicle status to "maintenance"
  - Removes vehicle from availability for the scheduled date

- **Given** a maintenance schedule is created  
  **When** customers search for available vehicles  
  **Then** vehicles with maintenance scheduled during requested period are excluded

- **Given** a maintenance block conflicts with existing reservation  
  **When** validating the maintenance schedule  
  **Then** the system displays a warning but allows maintenance to proceed (maintenance priority)

- **Given** maintenance is completed  
  **When** crew updates the maintenance record  
  **Then** the system:
  - Records completion date
  - Records updated odometer reading
  - Returns vehicle to "active" status
  - Makes vehicle available for rental

### FR-9: Damage and Incident Handling

**Title:** Report and Track Vehicle Damage Incidents

**Statement:** **As an** operations staff member, **I want** to document vehicle damage incidents with photos and repair estimates, **so that** damage is properly tracked and repair costs are recovered.

**Requirement Detail:**

During return inspection, staff identifies damage and documents it via web form for the specific vehicle. Documentation includes photos with positional mapping on vehicle diagram and repair cost estimation from internal price list.

Damaged vehicles are marked as either "drivable" (can continue operation) or "in-maintenance" (requires repair before rental). No parts inventory tracking is required - focus is on incident documentation and cost estimation.

**Acceptance Criteria:**

- **Given** operations staff identifies damage during return  
  **When** documenting the incident  
  **Then** the system provides damage report form with:
  - Vehicle identification
  - Incident date/time
  - Multiple photo upload capability
  - Vehicle diagram for positional mapping
  - Damage description field
  - Damage type classification
  - Cost estimation field

- **Given** photos are uploaded  
  **When** marking damage location  
  **Then** the system displays vehicle diagram allowing staff to pin damage locations

- **Given** damage requires cost estimation  
  **When** entering repair cost  
  **Then** the system references internal price list for common repair types

- **Given** damage report is completed  
  **When** submitting the report  
  **Then** the system requires staff to flag vehicle as:
  - Drivable: Vehicle can continue operation
  - In-maintenance: Vehicle unavailable until repaired

- **Given** vehicle is flagged "in-maintenance" due to damage  
  **When** reservation system checks availability  
  **Then** the vehicle is excluded from available inventory

- **Given** damage repairs are completed  
  **When** updating the incident record  
  **Then** the system:
  - Records actual repair cost
  - Updates vehicle status back to "drivable/active"
  - Links repair completion to incident

### FR-10: Fuel and Energy Management

**Title:** Enforce Fuel Level Policy and Charges

**Statement:** **As an** operations staff member, **I want** to measure and charge for fuel level differences, **so that** customers return vehicles with the same fuel level as delivery.

**Requirement Detail:**

Company policy requires vehicles returned with same fuel level as at delivery (same-to-same). If customer returns vehicle with less fuel, additional charges apply.

At delivery, system records fuel level. At return, system compares fuel levels and calculates charges for any difference. This ensures fair fuel cost distribution and maintains vehicle readiness.

**Acceptance Criteria:**

- **Given** a vehicle is being delivered to customer  
  **When** recording handover details  
  **Then** the system captures and stores delivery fuel level (as percentage or liters)

- **Given** a vehicle is being returned  
  **When** conducting return inspection  
  **Then** the system captures return fuel level

- **Given** return fuel level differs from delivery fuel level  
  **When** calculating final charges  
  **Then** the system:
  - Calculates fuel difference
  - Applies internal fuel pricing
  - Adds fuel charge to customer's total amount due

- **Given** fuel charges are calculated  
  **When** completing return process  
  **Then** the system displays itemized charges including fuel difference

- **Given** return fuel level matches or exceeds delivery level  
  **When** calculating charges  
  **Then** no fuel charge is applied

### FR-11: Vehicle Utilization Analytics

**Title:** Track and Report Fleet Utilization Metrics

**Statement:** **As a** fleet manager, **I want** to view vehicle utilization metrics per location and globally, **so that** I can identify underutilized vehicles and optimize fleet efficiency.

**Requirement Detail:**

The system tracks daily vehicle usage (on-rent vs. idle) to calculate utilization metrics. Both location-specific and global views are needed to support operational and strategic decisions.

Key metric is consecutive idle time - vehicles idle for 5+ consecutive days represent inefficiency and potential for fleet optimization (redistribution or disposal).

**Acceptance Criteria:**

- **Given** vehicles are tracked over time  
  **When** calculating utilization  
  **Then** the system records daily status for each vehicle:
  - On-rent: Vehicle assigned to active reservation
  - Idle: Vehicle available but not rented
  - Unavailable: Vehicle in maintenance or other non-rental status

- **Given** daily status is recorded  
  **When** generating utilization reports  
  **Then** the system calculates metrics:
  - Utilization percentage (on-rent days / available days)
  - Revenue per available vehicle day
  - Idle time statistics
  - Consecutive idle days per vehicle

- **Given** fleet manager requests location-specific utilization  
  **When** selecting a location  
  **Then** the system displays utilization metrics for vehicles at that location

- **Given** fleet manager requests global utilization  
  **When** accessing overall dashboard  
  **Then** the system displays aggregated metrics across all locations

- **Given** vehicles have extended idle periods  
  **When** reviewing utilization report  
  **Then** the system highlights vehicles with 5+ consecutive idle days as inefficiency alerts

### FR-12: Vehicle Swaps and Substitutions

**Title:** Handle Vehicle Unavailability with Substitution Rules

**Statement:** **As a** reservation system, **I want** to apply substitution rules when promised vehicles become unavailable, **so that** customer reservations can be fulfilled with appropriate alternatives.

**Requirement Detail:**

When a reserved vehicle becomes unexpectedly unavailable (damage, breakdown), the system must handle substitution following priority rules: 1) same vehicle type, 2) upgrade to better vehicle, 3) refund if no alternatives exist.

No automatic compensation is offered, and customer impact history is not tracked - focus is on finding suitable substitution following the defined priority rules.

**Acceptance Criteria:**

- **Given** a reserved vehicle becomes unavailable  
  **When** the system detects the conflict  
  **Then** the system applies substitution rules in priority order:
  1. Find available vehicle of same type (size and class match)
  2. If no match, find vehicle of better type (upgrade)
  3. If no alternatives, flag for refund processing

- **Given** substitution rule 1 is applied  
  **When** same-type vehicle is found  
  **Then** the system:
  - Updates reservation with new vehicle
  - Maintains original pricing
  - Notifies customer of vehicle change (same category)

- **Given** substitution rule 2 is applied  
  **When** upgrade is necessary  
  **Then** the system:
  - Assigns better vehicle
  - Maintains original pricing (customer gets free upgrade)
  - Notifies customer of upgrade

- **Given** no suitable vehicle available  
  **When** substitution rules are exhausted  
  **Then** the system flags reservation for manual refund processing

### FR-13: Reservation Extensions and Early Returns

**Title:** Manage Rental Extensions and Early Returns

**Statement:** **As a** reservation system, **I want** to handle rental extension requests and early returns, **so that** customers can modify their rental period with appropriate charges and availability updates.

**Requirement Detail:**

Customers may request to extend rental beyond original return date or return vehicle early. Extensions require checking vehicle availability for the extended period and calculating additional charges (base rental cost + late charges on daily basis).

Early returns make vehicle available for new rentals after 1-day turnaround period. System must warn when extensions conflict with upcoming reservations.

**Acceptance Criteria:**

- **Given** a customer requests rental extension  
  **When** processing the extension request  
  **Then** the system checks if vehicle is available for the extended period

- **Given** the vehicle is available for extension  
  **When** approving the extension  
  **Then** the system:
  - Auto-approves the extension
  - Calculates charges: base rental rate + daily late charge
  - Updates reservation end date
  - Blocks vehicle availability through new end date

- **Given** the vehicle has upcoming reservation  
  **When** extension is requested  
  **Then** the system displays warning to operations staff about the conflict

- **Given** extension conflicts with confirmed reservation  
  **When** staff reviews the conflict  
  **Then** manual decision is required (cannot auto-approve)

- **Given** customer returns vehicle early  
  **When** processing early return  
  **Then** the system:
  - Completes normal return inspection process
  - Updates reservation actual end date
  - Makes vehicle available after 1-day turnaround
  - Opens vehicle for new customer reservations

### FR-14: Alerts and Notifications

**Title:** Generate Operational Alerts via Multiple Channels

**Statement:** **As an** operations staff member, **I want** to receive critical alerts about fleet issues, **so that** I can respond quickly to security threats and operational problems.

**Requirement Detail:**

System monitors critical conditions and generates alerts through multiple channels (email, SMS, dashboard) to ensure timely staff response. Critical alerts include geofencing violations and maintenance overdue.

Multi-channel delivery ensures alerts reach staff regardless of their current work context (office, field, off-hours).

**Acceptance Criteria:**

- **Given** vehicle GPS indicates geofencing violation  
  **When** the violation is detected  
  **Then** the system:
  - Generates "Vehicle Outside Geofencing" alert
  - Sends email notification to fleet managers
  - Sends SMS notification to on-call staff
  - Displays alert in operations dashboard
  - Includes vehicle details, last known location, and timestamp

- **Given** vehicle maintenance is overdue  
  **When** odometer reading exceeds scheduled maintenance threshold  
  **Then** the system:
  - Generates "Maintenance Overdue" alert
  - Sends email notification to maintenance crew
  - Sends SMS notification to fleet manager
  - Displays alert in operations dashboard
  - Includes vehicle details, current mileage, and overdue amount

- **Given** alerts are generated  
  **When** staff views dashboard  
  **Then** active alerts are displayed with severity indicators

- **Given** alert condition is resolved  
  **When** the underlying issue is addressed  
  **Then** the system automatically clears the alert and logs resolution

### FR-15: Operational Safeguards

**Title:** Prevent Vehicle Assignment with Critical Issues

**Statement:** **As a** reservation system, **I want** to block vehicle assignment when critical issues exist, **so that** customers are not given vehicles that are unsafe or non-compliant.

**Requirement Detail:**

System enforces operational safeguards preventing vehicles from being assigned to customers when critical conditions exist. These safeguards protect both customer safety and company liability.

Critical conditions include active faults preventing operation and insurance about to expire (within 7 days threshold). Vehicles with critical issues are automatically excluded from available inventory until issues are resolved.

**Acceptance Criteria:**

- **Given** a vehicle has active critical fault  
  **When** reservation system calculates availability  
  **Then** the vehicle is excluded from available inventory

- **Given** fleet manager marks vehicle with critical fault  
  **When** updating vehicle status  
  **Then** the system:
  - Sets availability flag to "unavailable"
  - Prevents new reservation assignments
  - Displays reason: "Critical fault - not available for rent"

- **Given** a vehicle's insurance expiration date is checked  
  **When** expiration is within 7 days  
  **Then** the system:
  - Blocks vehicle from reservation assignment
  - Generates alert to fleet manager
  - Displays reason: "Insurance expiring soon"

- **Given** insurance is renewed  
  **When** new expiration date is updated  
  **Then** the system automatically removes the block if new expiration is >7 days out

- **Given** critical fault is repaired  
  **When** vehicle status is updated to operational  
  **Then** the system:
  - Removes availability block
  - Returns vehicle to available inventory
  - Records resolution in vehicle history

## Non-Functional Requirements

## Dependency & Constraints

**Technical Constraints:**
- Vehicle status updates must complete within 5 minutes maximum latency
- GPS telemetry data must update at maximum 5-minute intervals, real-time preferred
- System must support manual workflows - no requirement for AI-based features (damage detection, repositioning recommendations)
- No integration required with external systems (DMS, telematics providers, repair shops) in initial release

**Operational Constraints:**
- Pickup and delivery service limited to 06:00-19:00 local time only (no after-hours support)
- 1-day turnaround time required between rentals for vehicle preparation
- Maintenance scheduling follows fixed mileage intervals (10,000-12,000 km)
- Route planning for delivery remains manual process
- Staff task assignment (cleaning, inspection) is manual process

**Business Constraints:**
- Target zero double-booking incidents - overbooking not permitted
- Vehicle fleet initially focused on Economy class (expandable to Luxury)
- Real-time availability shows vehicles 2+ hours from current time
- Planned availability limited to 30-day window (H+1 to H+30)
- All vehicles must be owned by company (no third-party fleet)
- Each vehicle assigned to single home location at any time

**Scope Limitations:**
- No mobile app for staff task management in initial release
- No task productivity metrics tracking
- No predictive maintenance capabilities
- No automated repositioning recommendations
- No support for peer-to-peer car sharing models
- No staged cleaning levels (basic/deep/disinfection) tracking
- No SLA monitoring for turnaround times
- Compliance and regulatory requirements deferred (annual inspections, emissions)

## Success Metrics

**Operational Efficiency Metrics:**
- Reduce vehicle double-booking to zero incidents
- Achieve <5 minute average system response time for vehicle status updates
- Maintain 95%+ GPS telemetry uptime with <5 minute update intervals
- Reduce average vehicle idle time by tracking and highlighting 5+ consecutive idle days
- Complete vehicle turnaround within 1-day standard for 90%+ of returns

**Fleet Utilization Metrics:**
- Achieve 70%+ vehicle utilization rate (on-rent days / available days) within first 6 months
- Reduce consecutive idle time to <5 days for 90%+ of active fleet
- Track revenue per available vehicle day trending upward quarter-over-quarter

**Process Compliance Metrics:**
- Achieve 100% completion rate for required handover documentation (photos, signatures, identity verification)
- Complete standardized return inspection checklist for 100% of returns
- Execute scheduled maintenance within +/- 500 km of target mileage for 95%+ of services

**Risk Mitigation Metrics:**
- Zero vehicle assignments with expired or expiring insurance (<7 days)
- Zero vehicle assignments with active critical faults
- <30 minute average response time to geofencing violation alerts
- Maintenance overdue alerts generated for 100% of vehicles exceeding threshold

**Customer Experience Metrics:**
- Fulfill 95%+ of reservations without vehicle substitution
- When substitution required, achieve 80%+ same-type match rate (avoiding upgrades/refunds)
- Enable 90%+ auto-approval rate for extension requests (no conflicts)

**Data Quality Metrics:**
- Maintain 100% data completeness for critical vehicle fields (VIN, license plate, insurance)
- Capture damage assessment photos for 100% of incidents
- Record accurate fuel levels at 100% of delivery and return transactions
