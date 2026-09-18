# smart-parking-platform
CIS 4374 - Information Systems Project Management Semester Project- Fatima Chaudhry


================================================================================
SMART PARKING PLATFORM
Project Management Specification & Requirements Document
================================================================================

DOCUMENT CONTROL
--------------------------------------------------------------------------------
Project Name:        Smart Parking Platform
Document Version:    1.0
Author:              Coogs Systems Group
Project Lead:        Fatima Zahra Chaudhry
Course:              CIS 4374 - Information Systems Project Management
Date:                September 10, 2026


1. EXECUTIVE RESEARCH & COMPETITIVE LANDSCAPE ANALYSIS
--------------------------------------------------------------------------------

1.1 Current Market Offerings

    Urban drivers routinely lose productive time circling for off-street and 
    curbside parking spaces, which intensifies roadway congestion and fuel 
    consumption. While consumer-facing parking software exists today, existing 
    market solutions suffer from fundamental design limitations:

    A. SpotHero and ParkWhiz
       These aggregation platforms focus on advance, prepaid parking reservations 
       within third-party facilities. While effective for basic ticketing, their 
       systems depend on asynchronous batch updates with local garage operators. 
       This latency regularly causes phantom availability, where drivers book a 
       spot only to arrive at a garage that is already full. Furthermore, these 
       platforms provide zero guidance once a driver enters the garage, leaving 
       them to wander between levels looking for empty stalls.

    B. ParkMobile and PayByPhone
       These solutions cater primarily to on-street municipal metered parking. 
       Their architecture acts merely as a digital replacement for coin meters 
       tied to physical zone numbers. They lack direct integration with physical 
       barrier gates, offer no advance reservation guarantees for enclosed garages, 
       and give parking operators no automated controls for real-time yield 
       management or occupancy forecasting.

1.2 Strategic Market Differentiation

    Coogs Systems Group addresses these shortcomings by integrating software 
    services directly with hardware garage infrastructure:

    * Sub-Second Gate and Sensor Telemetry: Direct communication links to barrier 
      arms and individual stall sensors eliminate data latency, ensuring 100% 
      accurate spot availability with zero overbooking risk.

    * Automated Gate Clearance: Dynamic, offline-verifiable QR codes and 
      automated license plate recognition remove the need for physical ticket 
      dispensers and eliminate entry backups.

    * In-Garage Bay Navigation: Built-in positioning systems guide drivers 
      directly through interior ramps to their assigned parking bay.

    * Operator Yield Management Engine: Web tools give facility operators 
      automated occupancy forecasting, dynamic rate adjustment, and instant 
      alerts for vehicles exceeding their reserved reservation window.


2. VISION AND SCOPE (VnS)
--------------------------------------------------------------------------------

2.1 Studio Overview and Governance

    Coogs Systems Group is structured as a dedicated software development 
    studio operating with an enterprise budget and cross-functional teams:

    * Mobile Application Team: Native iOS (Swift) and Android (Kotlin) clients.
    * Web Application Team: Responsive operator administrative dashboards.
    * Backend and Cloud API Team: Scalable microservices and database clusters.
    * Mapping and Location Services Team: Spatial indexing and indoor navigation.
    * Payment Integration Team: PCI-DSS Level 1 compliant financial workflows.
    * Quality Assurance Team: Continuous automated test suites and load testing.

2.2 Project Acquisition

    Coogs Systems Group won this contract through a competitive Request for 
    Proposal issued jointly by regional transit authorities and a coalition of 
    private garage operators. The project objective is to deploy a single unified 
    platform that cuts downtown street traffic while maximizing revenue for 
    commercial and municipal parking facilities.

2.3 Scope Boundaries

    In-Scope (Release 1.0):
    * Driver mobile applications for iOS and Android devices.
    * Central administrative web portal for garage operators and administrators.
    * Dynamic map display showing facilities, fee rates, and live open bays.
    * Reservation workflow with a 15-minute checkout hold on selected bays.
    * Secure payment gateway processing credit cards, Apple Pay, and Google Pay.
    * Encrypted QR access passes and automated license plate entry tracking.
    * Reporting modules for garage revenue and utilization tracking.

    Out-of-Scope (Release 1.0):
    * Manufacturing or field assembly of ultrasonic stall sensors.
    * Cash or coin handling systems for on-street parking meters.
    * Valet personnel dispatching and physical key management.


3. SOFTWARE REQUIREMENTS SPECIFICATION (SRS)
--------------------------------------------------------------------------------

3.1 Overall System Description

    Target Users:
    * Drivers and vehicle owners.
    * Garage managers and field operators.
    * System administrators.
    * Payment clearinghouses and third-party mapping providers.

    Operating Environment:
    * Mobile clients running modern iOS and Android operating systems.
    * Central backend deployed across cloud microservices with relational storage.
    * Administrative portal accessible via standard desktop web browsers.

    Operating Constraints:
    * Continuous 24/7 uptime with high transaction throughput.
    * Strict adherence to PCI-DSS payment data protection and AES-256 encryption.

    General Assumptions:
    * User devices maintain active cellular internet and location permissions.
    * Partner garages possess networked barrier controllers and digital scanners.

3.2 Functional Requirements (FRs)

    FR1:  The system shall allow users to register and sign in securely using 
          email credentials or OAuth 2.0 federated providers.

    FR2:  The system shall render an interactive map showing nearby garages with 
          real-time open stall counts and hourly rate tables.

    FR3:  The system shall update space availability across all client screens 
          within 1.0 second of an entry or exit gate event.

    FR4:  The system shall hold an identified parking space for 15 minutes while 
          the user completes the checkout process.

    FR5:  The system shall process debit/credit card and digital wallet payments 
          through an encrypted, tokenized payment provider.

    FR6:  The system shall issue an encrypted digital QR pass and associate the 
          driver's license plate upon reservation completion.

    FR7:  The system shall launch external navigation tools to guide vehicles 
          directly to the selected garage entrance lane.

    FR8:  The system shall send push notifications 15 minutes prior to session 
          expiration and support remote parking extensions.

    FR9:  The administrative portal shall allow operators to set automated 
          pricing rules based on real-time facility occupancy levels.

    FR10: The system shall compile and export daily, weekly, and monthly revenue 
          and utilization records in PDF and CSV formats.

3.3 Non-Functional Requirements (NFRs)

    NFR1: Search queries and map loading times shall resolve within 500 
          milliseconds under standard operational loads.

    NFR2: The system shall support a minimum of 20,000 active concurrent users 
          without API response degradation.

    NFR3: The reservation and payment backend shall maintain 99.99% service 
          availability outside of pre-scheduled maintenance windows.

    NFR4: All data transmissions shall enforce TLS 1.3 encryption. No unencrypted 
          payment card data shall reside on internal application databases.

    NFR5: Gate controllers shall retain a locally cached offline pass database to 
          permit vehicle entry during short-term network disruptions.


## 4. System Use Cases

### Use Case 1: Register New Driver Account
> **Actor:** Driver  
> **Precondition:** Application is installed on mobile device; cellular data or Wi-Fi is active.  
> **Postcondition:** Driver account is authenticated and ready for booking spaces.

**Steps:**
1. User selects "Create Account" on the mobile splash screen.
2. User enters full name, email address, phone number, and password.
3. User inputs the 6-digit SMS verification code sent by the system.
4. User enters primary vehicle details (license plate, state, make, model).
5. User confirms terms of service and saves account profile.

---

### Use Case 2: Search Available Parking Locations
> **Actor:** Driver  
> **Precondition:** User is logged in; location permissions are active on device.  
> **Postcondition:** Facility rate cards, occupancy counts, and details are displayed on screen.

**Steps:**
1. User opens the interactive map search interface.
2. User inputs destination address and expected parking time window.
3. User reviews filtered map markers showing facilities and rates.
4. User selects a specific facility to inspect real-time bay availability.

---

### Use Case 3: Reserve Parking Bay and Checkout
> **Actor:** Driver  
> **Precondition:** Facility has been selected; payment method is ready for processing.  
> **Postcondition:** Parking bay is reserved, available inventory decreases by one, and entry QR code is saved to profile.

**Steps:**
1. User clicks "Reserve Space" on the facility details page.
2. User confirms stay duration and reviews the billing calculation.
3. User selects a saved payment method (credit card, Apple Pay, Google Pay).
4. User clicks "Authorize Payment" to confirm transaction.
5. User verifies booking confirmation details and digital entry pass.

---

### Use Case 4: Check In at Garage Barrier Gate
> **Actor:** Driver / Facility Barrier Scanner  
> **Precondition:** Driver arrives at entry lane; digital QR pass is displayed on device.  
> **Postcondition:** Gate arm closes, parking session switches to "Active", and session timer starts.

**Steps:**
1. User positions phone screen under the entry lane optical scanner.
2. Scanner reads encrypted pass token and verifies data with gate controller.
3. System confirms reservation status, arrival time, and vehicle plate.
4. Controller triggers barrier gate arm to open.
5. User drives vehicle past the gate threshold.

---

### Use Case 5: Navigate to Assigned Indoor Parking Space
> **Actor:** Driver  
> **Precondition:** Vehicle is inside garage; device is receiving local Bluetooth beacon signals.  
> **Postcondition:** Vehicle is parked and interior navigation session closes.

**Steps:**
1. User taps "Start In-Garage Guidance" on the active session view.
2. System determines vehicle location via indoor positioning beacons.
3. System displays floor-by-floor route map through ramps and driving lanes.
4. User follows screen prompts to the assigned parking level and row.
5. User pulls vehicle into the designated parking bay.

---

### Use Case 6: Extend Active Parking Duration
> **Actor:** Driver  
> **Precondition:** Parking session is currently active; facility permits duration extensions.  
> **Postcondition:** Expiration timestamp is updated across database and gate exit systems.

**Steps:**
1. User receives alert that 15 minutes remain on active session.
2. User clicks "Extend Session" from the active booking view.
3. User selects additional time block (e.g., 1 hour, 2 hours).
4. User confirms additional fee and authorizes payment.
5. User verifies updated session departure time on screen.

---

### Use Case 7: Cancel Upcoming Reservation
> **Actor:** Driver  
> **Precondition:** Future reservation exists and current time is outside the penalty window.  
> **Postcondition:** Reserved bay is returned to open pool and refund is processed.

**Steps:**
1. User navigates to "My Bookings" and chooses scheduled reservation.
2. User clicks "Cancel Reservation".
3. System displays cancellation terms and applicable refund total.
4. User confirms cancellation request.
5. User receives cancellation confirmation and refund notification.

---

### Use Case 8: Export Receipt and Tax Invoice
> **Actor:** Driver  
> **Precondition:** User is logged in; at least one finished session exists in account history.  
> **Postcondition:** Itemized payment receipt is stored locally on user device.

**Steps:**
1. User opens the "Booking History" tab within profile settings.
2. User taps on the targeted completed session record.
3. User selects "Download Tax Receipt (PDF)".
4. System creates formatted document showing rates, timestamps, and taxes.
5. User downloads or shares the generated document.

---

### Use Case 9: Operator Sign-In and Role Verification
> **Actor:** Parking Facility Operator  
> **Precondition:** Operator has an authorized staff account with facility assignments.  
> **Postcondition:** Operator session is validated with assigned administrative rights.

**Steps:**
1. Operator loads the web-based administrative management portal.
2. Operator enters enterprise email and password credentials.
3. System prompts for two-factor authentication security code.
4. Operator enters temporary verification code from authenticator app.
5. Operator is directed to the central facility dashboard.

---

### Use Case 10: Configure Dynamic Pricing Schedules
> **Actor:** Parking Facility Operator  
> **Precondition:** Operator is logged into portal and viewing facility rate controls.  
> **Postcondition:** New pricing table updates live search pricing across mobile apps.

**Steps:**
1. Operator selects "Rate Management" for the targeted parking garage.
2. Operator sets high-occupancy surge trigger (e.g., 85% capacity).
3. Operator enters the adjusted hourly surge rate amount.
4. Operator sets effective schedule duration or leaves rule on automatic.
5. Operator saves settings and verifies published changes.

---

### Use Case 11: Manually Block Space for Facility Maintenance
> **Actor:** Parking Facility Operator  
> **Precondition:** Operator dashboard is open; facility space layout diagram is displayed.  
> **Postcondition:** Selected parking bays are removed from public reservation inventory.

**Steps:**
1. Operator opens the "Bay Management" layout view.
2. Operator clicks on specific stall IDs (e.g., Level 2, Bays 14-16).
3. Operator sets stall status to "Out of Service".
4. Operator logs maintenance reason (e.g., sweeping, painting, repaving).
5. Operator confirms and saves updated stall status.

---

### Use Case 12: Ingest Gate Sensor Telemetry
> **Actor:** Facility Barrier Sensor / System Controller  
> **Precondition:** Vehicle triggers optical sensor or inductive vehicle loop at barrier.  
> **Postcondition:** Live availability database and user interfaces reflect updated stall counts.

**Steps:**
1. Physical lane sensor records vehicle entry or exit motion.
2. Gate hardware packages sensor ID, lane number, and timestamp payload.
3. Hardware transmits telemetry message to the cloud ingestion endpoint.
4. System adjusts facility occupancy counters in real-time memory cache.
5. System broadcasts adjusted space count to active mobile and web clients.

---

### Use Case 13: Generate Revenue and Occupancy Analytics Report
> **Actor:** Parking Facility Operator  
> **Precondition:** Transaction and gate sensor logs exist in the system database.  
> **Postcondition:** Structured performance document is generated and downloaded.

**Steps:**
1. Operator clicks "Reports and Analytics" in the administrative portal.
2. Operator picks reporting date interval and target parking facility.
3. Operator selects target metrics: total volume, peak hours, and net revenue.
4. Operator clicks "Generate Report".
5. Operator reviews generated graphs and exports file as CSV or PDF.

---

### Use Case 14: Dispatch Overstay Violation Alert
> **Actor:** Automated System Notification Service  
> **Precondition:** Vehicle is inside garage past paid reservation departure time.  
> **Postcondition:** Overstay event is recorded and flagged on operator control screen.

**Steps:**
1. System monitoring task identifies active parking pass past expiration cutoff.
2. System queries exit gate events to verify vehicle has not scanned out.
3. System marks reservation record with an overstay violation flag.
4. System dispatches push alert to driver detailing overtime fee structure.
5. System logs vehicle plate and bay identifier to operator enforcement queue.

---

### Use Case 15: Audit API Security and Access Logs
> **Actor:** System Administrator  
> **Precondition:** Administrator possesses elevated administrative access credentials.  
> **Postcondition:** Security audit log is reviewed, verified, and saved to long-term storage.

**Steps:**
1. Administrator signs into cloud monitoring console.
2. Administrator opens the "Security and Access Logging" interface.
3. Administrator filters traffic by authorization failures and API errors.
4. Administrator inspects flagged IP addresses and invalid token requests.
5. Administrator archives audit records to meet compliance storage guidelines.

*** THIS IS THE END OF MY HOMEWORK 1 PORTION ***

## 5. Work Breakdown Structure (WBS)

Before I could put together a schedule, I needed to break the project down 
into smaller, more manageable pieces. Below is my Work Breakdown Structure, 
broken down three levels deep (major component -> sub-component -> 
individual task), based on the scope already defined in the SRS above.

**1. Project Management**
   1.1 Kickoff & Planning
       1.1.1 Vision & Scope Document
       1.1.2 Software Requirements Specification
   1.2 Documentation & Reporting
       1.2.1 Weekly GitHub Update
       1.2.2 Weekly Stakeholder Video

**2. Authentication & Access Control**
   2.1 Driver Login
       2.1.1 Email/Password Registration
       2.1.2 OAuth 2.0 Login
       2.1.3 SMS Verification
   2.2 Operator Login
       2.2.1 Employee Login
       2.2.2 Two-Factor Authentication
   2.3 Role-Based Permissions
       2.3.1 Driver Role
       2.3.2 Operator Role
       2.3.3 Admin Role

**3. Driver Mobile App (iOS/Android)**
   3.1 Map & Search
       3.1.1 Interactive Map
       3.1.2 Real-Time Spot Availability
       3.1.3 Filters/Rate Display
   3.2 Reservation & Checkout
       3.2.1 15-Minute Hold Logic
       3.2.2 Payment Method Picker
       3.2.3 Booking Confirmation Screen
   3.3 Digital Access Pass
       3.3.1 QR Code Generation
       3.3.2 License Plate Linking
   3.4 In-Garage Navigation
       3.4.1 Bluetooth Beacon Positioning
       3.4.2 Turn-by-Turn Routing
   3.5 Session Management
       3.5.1 Expiration Push Notifications
       3.5.2 Extend Session Flow
       3.5.3 Cancel/Refund Flow
   3.6 Booking History
       3.6.1 Receipt/Tax Export

**4. Operator Web Portal**
   4.1 Dashboard
       4.1.1 Facility Overview
       4.1.2 Bay Management (block bays for maintenance)
   4.2 Dynamic Pricing
       4.2.1 Surge Pricing Rules
       4.2.2 Rate Scheduling
   4.3 Reports & Analytics
       4.3.1 Revenue Export (CSV/PDF)
       4.3.2 Occupancy Dashboard
   4.4 Security
       4.4.1 Access Logs
       4.4.2 API Monitoring

**5. Backend & Cloud API**
   5.1 Core Services
       5.1.1 Reservation Service
       5.1.2 User/Account Service
   5.2 Live Telemetry
       5.2.1 Sensor/Gate Event Ingestion
       5.2.2 Live Occupancy Updates
   5.3 Database/Infrastructure
       5.3.1 Schema Design
       5.3.2 Scaling & Deployment
   5.4 Overstay Detection
       5.4.1 Monitoring Job
       5.4.2 Alert Dispatch

**6. Payments**
   6.1 Payment Gateway
       6.1.1 Credit/Debit Cards
       6.1.2 Apple Pay / Google Pay
   6.2 Compliance
       6.2.1 PCI-DSS Tokenization
       6.2.2 Encryption (TLS 1.3 / AES-256)

**7. QA & Testing**
   7.1 Automated Testing
       7.1.1 Unit Tests
       7.1.2 Integration Tests
   7.2 Load Testing
       7.2.1 Concurrency Test (20k users)
       7.2.2 Latency Benchmarks
   7.3 User Acceptance Testing
       7.3.1 Driver Flow Testing
       7.3.2 Operator Flow Testing


## 6. Project Schedule & Timeline

Once the WBS was complete, I used it along with rough effort estimates to 
draft a schedule. Most estimates are based on comparable features already 
implemented in similar applications, since building precise estimates for 
every task wasn't practical at this stage. Task dependencies are mostly 
finish-to-start: the mobile app's booking flow won't start until the backend 
reservation service exists, and testing on a feature doesn't begin until 
that feature is built.

**Draft Timeline**
Week 1-2:   Project kickoff, requirements, Vision & Scope (done)
Week 3:     WBS & schedule baseline (this deliverable)
Week 4-6:   Core backend, database schema, and authentication
Week 6-8:   Real-time telemetry and live occupancy tracking
Week 7-9:   Mobile app - map, search, reservations, checkout
Week 8-10:  Web admin portal - dashboard, bay management, dynamic pricing
Week 9-11:  Payment gateway integration and PCI-DSS compliance
Week 10-12: Mobile app - QR passes, in-garage navigation, session management
Week 11-13: Reporting, analytics, and security/audit console
Week 13-15: QA - automated testing, load testing, user acceptance testing
Week 16:    Final delivery / Release 1.0

**Milestones**
M1 - Requirements Signed Off ........... Week 2
M2 - WBS + Schedule Baseline (this HW) .. Week 3
M3 - Core Backend/Auth Complete ......... Week 6
M4 - Mobile App Alpha ................... Week 9
M5 - Web Portal Alpha ................... Week 10
M6 - Payments + Navigation Complete ..... Week 11
M7 - Feature-Complete Beta .............. Week 13
M8 - QA/Load/UAT Complete ............... Week 15
M9 - Final Delivery (Release 1.0) ....... Week 16

--- Gantt chart will be on separate document----

* THIS IS THE END OF MY HOMEWORK 2 *
