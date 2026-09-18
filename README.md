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

# Section 5: Work Breakdown Structure (WBS)

Before constructing the project schedule, the smart parking platform was decomposed into smaller, manageable components following a 3-level hierarchical structure (**Subsystem → Module → Work Package/Feature**). The breakdown directly reflects the operational boundaries and user personas established in the Software Requirements Specification (SRS).

* **1.0 Project Management**
  * **1.1 Kickoff & Planning**
    * 1.1.1 Vision & Scope Document
    * 1.1.2 Software Requirements Specification (SRS)
  * **1.2 Governance & Tracking**
    * 1.2.1 Weekly GitHub Sprint Updates
    * 1.2.2 Stakeholder Progress Briefings & Demos

* **2.0 Authentication & Access Control**
  * **2.1 Driver Identity Services**
    * 2.1.1 Email/Password Registration & Credential Store
    * 2.1.2 OAuth 2.0 Social Federation
    * 2.1.3 SMS/MFA Verification Flow
  * **2.2 Operator Identity Services**
    * 2.2.1 Enterprise Directory & Staff Provisioning
    * 2.2.2 Two-Factor Authentication (TOTP / Hardware Token)
  * **2.3 Role-Based Access Control (RBAC)**
    * 2.3.1 Driver Profile & Vehicle Asset Permissions
    * 2.3.2 Garage Operator Attendant Permissions
    * 2.3.3 System Administrator Policy Engine

* **3.0 Driver Mobile Application (iOS / Android)**
  * **3.1 Discovery & Search Module**
    * 3.1.1 Interactive Geospatial Map Engine
    * 3.1.2 Real-Time Bay Availability Feed
    * 3.1.3 Multi-Criteria Filtering & Fee Calculator
  * **3.2 Reservation & Checkout**
    * 3.2.1 15-Minute Dynamic Slot Reservation Lock
    * 3.2.2 Payment Method Selector
    * 3.2.3 Digital Booking Confirmation & Pass Issuance
  * **3.3 Digital Access Passes**
    * 3.3.1 Dynamic Secure QR Code Generator
    * 3.3.2 Automated License Plate Recognition (ALPR) Linking
  * **3.4 In-Facility Guidance**
    * 3.4.1 BLE Beacon Triangulation & Handshake
    * 3.4.2 Turn-by-Turn Indoor Bay Routing
  * **3.5 Active Session Lifecycle**
    * 3.5.1 Expiration Warning Push Notification Service
    * 3.5.2 Overstay Grace Period & Self-Extension Module
    * 3.5.3 Early Check-Out & Prorated Refund Pipeline
  * **3.6 Driver History & Expense Export**
    * 3.6.1 Historical Session Invoicing & PDF/CSV Export Engine

* **4.0 Operator Web Portal**
  * **4.1 Facility Operations Dashboard**
    * 4.1.1 Multi-Facility Real-Time Status View
    * 4.1.2 Bay Maintenance Locking & Manual Override
  * **4.2 Dynamic Pricing & Tariff Engine**
    * 4.2.1 Peak / Event-Based Surge Pricing Algorithms
    * 4.2.2 Tiered Rate Scheduling by Time & Vehicle Class
  * **4.3 Telemetry & Analytics**
    * 4.3.1 Financial Ledger & Revenue Export Engine
    * 4.3.2 Occupancy Heatmaps & Historical Trend Analysis
  * **4.4 Administration & Audit**
    * 4.4.1 Centralized Access Audit Logging
    * 4.4.2 Ingestion Gateway & Health Monitoring Dashboards

* **5.0 Cloud Backend & Data Pipelines**
  * **5.1 Transactional Core Services**
    * 5.1.1 Distributed Reservation State Machine
    * 5.1.2 Profile & Account Microservice
  * **5.2 Telemetry Ingestion Layer**
    * 5.2.1 IoT Sensor & Barrier Gate Event Stream Processing
    * 5.2.2 WebSocket Broadcaster for Live Occupancy Status
  * **5.3 Data Persistence & Infrastructure**
    * 5.3.1 Relational Schema Design & Migration Scripts
    * 5.3.2 Containerized Orchestration & Auto-Scaling Rules
  * **5.4 Enforcement & Violation Detection**
    * 5.4.1 Scheduled Overstay & Unpaid Bay Auditing Jobs
    * 5.4.2 Enforcement Dispatch & Tow Notification Alerts

* **6.0 Payments & Regulatory Compliance**
  * **6.1 Payment Gateway Processing**
    * 6.1.1 Credit / Debit Processing Engine
    * 6.1.2 Mobile Wallet Handshake (Apple Pay, Google Pay)
  * **6.2 Security & Financial Governance**
    * 6.2.1 PCI-DSS Vault Tokenization
    * 6.2.2 At-Rest & In-Flight Cryptographic Enforcement (TLS 1.3 / AES-256)

* **7.0 Quality Assurance & Reliability Verification**
  * **7.1 Automated Testing**
    * 7.1.1 Unit Test Suites (Services, Controllers, Utilities)
    * 7.1.2 End-to-End API Integration Pipelines
  * **7.2 Scalability & Performance Benchmarks**
    * 7.2.1 Peak Load Simulation (20,000 Concurrent Users)
    * 7.2.2 Database Query Optimization & Latency Benchmarks
  * **7.3 User Acceptance Testing (UAT)**
    * 7.3.1 Driver Mobile Flow Validation
    * 7.3.2 Garage Operator Operational Drill Testing

---

# Section 6: Project Schedule & Timeline

Effort estimations were derived through **analogy-based sizing** from benchmark microservices and comparable transit systems, balancing implementation complexity against delivery targets. Scheduling dependencies predominantly follow a **Finish-to-Start (FS)** sequence: the client interface layers depend on the foundational core schema, the booking engine requires payment gateway integration, and verification testing proceeds only once functional features reach deployment readiness.

### Milestone Definitions

Milestones serve as zero-duration control gates measuring critical delivery thresholds across the project lifecycle.

| Milestone | Description | Target Date | Criteria for Completion |
| :--- | :--- | :--- | :--- |
| **M1** | Requirements Finalized | Week 2 | Vision & Scope Document and SRS approved |
| **M2** | Schedule & WBS Baseline | Week 3 | WBS, activity estimates, and dependency plan completed |
| **M3** | Core Services & Auth Engine | Week 6 | Schemas deployed, RBAC active, Auth endpoints passing tests |
| **M4** | Driver Mobile Alpha | Week 9 | Live map discovery, spot hold, and local checkout functioning |
| **M5** | Operator Web Portal Alpha | Week 10 | Facility dashboard, space overriding, and surge toggles active |
| **M6** | Integrated Payments & Navigation | Week 11 | Gateway processing live; BLE positioning active |
| **M7** | System Beta Release | Week 13 | Feature-complete across driver, operator, and analytics layers |
| **M8** | Verification Sign-Off | Week 15 | Stress testing, security audit, and UAT validated |
| **M9** | Production Release (1.0) | Week 16 | Deployment to cloud production environment |

### Sprint Work Schedule

| Sprint Window | Primary Activity | WBS Focus Areas | Major Predecessor (Dependency) |
| :--- | :--- | :--- | :--- |
| **Weeks 1–2** | Project Initiation & Requirements | 1.1, 1.2 | Project Kickoff (FS) |
| **Week 3** | WBS, Architecture & Estimation Baseline | 1.1, 1.2 | M1 (FS) |
| **Weeks 4–6** | Core Backend, DB Schemas, & Identity Services | 2.1, 2.2, 2.3, 5.1, 5.3 | M2 (FS) |
| **Weeks 6–8** | Live Telemetry & Ingestion Pipelines | 5.2, 5.4 | 5.3 Database Provisioning (FS) |
| **Weeks 7–9** | Driver Mobile: Map Discovery & Reservation | 3.1, 3.2 | 5.1 Core Services (FS) |
| **Weeks 8–10** | Operator Portal: Bay Admin & Dynamic Pricing | 4.1, 4.2 | 2.3 RBAC, 5.1 Services (FS) |
| **Weeks 9–11** | Payment Gateway Integration & Hardening | 6.1, 6.2 | 3.2 Reservation Checkout (FS) |
| **Weeks 10–12** | Mobile Features: BLE Navigation & Pass Passes | 3.3, 3.4, 3.5 | M4 Driver Mobile Alpha (FS) |
| **Weeks 11–13** | Reporting, Analytics & Security Audit Console | 4.3, 4.4, 3.6 | 5.2 Ingestion, 6.1 Payments (FS) |
| **Weeks 13–15** | Comprehensive QA: Load, Security, and UAT | 7.1, 7.2, 7.3 | M7 Beta Feature Freeze (FS) |
| **Week 16** | Production Deployment & Release 1.0 | Release Delivery | M8 Verification Approval (FS) |

*** THIS IS THE END OF MY HOMEWORK 2 POTION ***
