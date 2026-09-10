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


4. SYSTEM USE CASES


Use Case 1
    Title: Register New Driver Account
    Actor: Driver
    Precondition: Application is installed on mobile device; cellular data or Wi-Fi is active.
    Steps:
        1. User selects "Create Account" on the mobile splash screen.
        2. User enters full name, email address, phone number, and password.
        3. User inputs the 6-digit SMS verification code sent by the system.
        4. User enters primary vehicle details (license plate, state, make, model).
        5. User confirms terms of service and saves account profile.
    Postcondition: Driver account is authenticated and ready for booking spaces.

Use Case 2
    Title: Search Available Parking Locations
    Actor: Driver
    Precondition: User is logged in; location permissions are active on device.
    Steps:
        1. User opens the interactive map search interface.
        2. User inputs destination address and expected parking time window.
        3. User reviews filtered map markers showing facilities and rates.
        4. User selects a specific facility to inspect real-time bay availability.
    Postcondition: Facility rate cards, occupancy counts, and details are shown.

Use Case 3
    Title: Reserve Parking Bay and Checkout
    Actor: Driver
    Precondition: Facility has been selected; payment method is ready for processing.
    Steps:
        1. User clicks "Reserve Space" on the facility details page.
        2. User confirms stay duration and reviews the billing calculation.
        3. User selects a saved payment method (credit card, Apple Pay, Google Pay).
        4. User clicks "Authorize Payment" to confirm transaction.
        5. User verifies booking confirmation details and digital entry pass.
    Postcondition: Parking bay is reserved, available inventory decreases by one, and entry QR code is saved to profile.

Use Case 4
    Title: Check In at Garage Barrier Gate
    Actor: Driver / Facility Barrier Scanner
    Precondition: Driver arrives at entry lane; digital QR pass is displayed on device.
    Steps:
        1. User positions phone screen under the entry lane optical scanner.
        2. Scanner reads encrypted pass token and verifies data with gate controller.
        3. System confirms reservation status, arrival time, and vehicle plate.
        4. Controller triggers barrier gate arm to open.
        5. User drives vehicle past the gate threshold.
    Postcondition: Gate arm closes, parking session switches to "Active", and session timer starts.

Use Case 5
    Title: Navigate to Assigned Indoor Parking Space
    Actor: Driver
    Precondition: Vehicle is inside garage; device is receiving local Bluetooth beacon signals.
    Steps:
        1. User taps "Start In-Garage Guidance" on the active session view.
        2. System determines vehicle location via indoor positioning beacons.
        3. System displays floor-by-floor route map through ramps and driving lanes.
        4. User follows screen prompts to the assigned parking level and row.
        5. User pulls vehicle into the designated parking bay.
    Postcondition: Vehicle is parked and interior navigation session closes.

Use Case 6
    Title: Extend Active Parking Duration
    Actor: Driver
    Precondition: Parking session is currently active; facility permits duration extensions.
    Steps:
        1. User receives alert that 15 minutes remain on active session.
        2. User clicks "Extend Session" from the active booking view.
        3. User selects additional time block (e.g., 1 hour, 2 hours).
        4. User confirms additional fee and authorizes payment.
        5. User verifies updated session departure time on screen.
    Postcondition: Expiration timestamp is updated across database and gate exit systems.

Use Case 7
    Title: Cancel Upcoming Reservation
    Actor: Driver
    Precondition: Future reservation exists and current time is outside the penalty window.
    Steps:
        1. User navigates to "My Bookings" and chooses scheduled reservation.
        2. User clicks "Cancel Reservation".
        3. System displays cancellation terms and applicable refund total.
        4. User confirms cancellation request.
        5. User receives cancellation confirmation and refund notification.
    Postcondition: Reserved bay is returned to open pool and refund is processed.

Use Case 8
    Title: Export Receipt and Tax Invoice
    Actor: Driver
    Precondition: User is logged in; at least one finished session exists in account history.
    Steps:
        1. User opens the "Booking History" tab within profile settings.
        2. User taps on the targeted completed session record.
        3. User selects "Download Tax Receipt (PDF)".
        4. System creates formatted document showing rates, timestamps, and taxes.
        5. User downloads or shares the generated document.
    Postcondition: Itemized payment receipt is stored locally on user device.

Use Case 9
    Title: Operator Sign-In and Role Verification
    Actor: Parking Facility Operator
    Precondition: Operator has an authorized staff account with facility assignments.
    Steps:
        1. Operator loads the web-based administrative management portal.
        2. Operator enters enterprise email and password credentials.
        3. System prompts for two-factor authentication security code.
        4. Operator enters temporary verification code from authenticator app.
        5. Operator is directed to the central facility dashboard.
    Postcondition: Operator session is validated with assigned administrative rights.

Use Case 10
    Title: Configure Dynamic Pricing Schedules
    Actor: Parking Facility Operator
    Precondition: Operator is logged into portal and viewing facility rate controls.
    Steps:
        1. Operator selects "Rate Management" for the targeted parking garage.
        2. Operator sets high-occupancy surge trigger (e.g., 85% capacity).
        3. Operator enters the adjusted hourly surge rate amount.
        4. Operator sets effective schedule duration or leaves rule on automatic.
        5. Operator saves settings and verifies published changes.
    Postcondition: New pricing table updates live search pricing across mobile apps.

Use Case 11
    Title: Manually Block Space for Facility Maintenance
    Actor: Parking Facility Operator
    Precondition: Operator dashboard is open; facility space layout diagram is displayed.
    Steps:
        1. Operator opens the "Bay Management" layout view.
        2. Operator clicks on specific stall IDs (e.g., Level 2, Bays 14-16).
        3. Operator sets stall status to "Out of Service".
        4. Operator logs maintenance reason (e.g., sweeping, painting, repaving).
        5. Operator confirms and saves updated stall status.
    Postcondition: Selected parking bays are removed from public reservation inventory.

Use Case 12
    Title: Ingest Gate Sensor Telemetry
    Actor: Facility Barrier Sensor / System Controller
    Precondition: Vehicle triggers optical sensor or inductive vehicle loop at barrier.
    Steps:
        1. Physical lane sensor records vehicle entry or exit motion.
        2. Gate hardware packages sensor ID, lane number, and timestamp payload.
        3. Hardware transmits telemetry message to the cloud ingestion endpoint.
        4. System adjusts facility occupancy counters in real-time memory cache.
        5. System broadcasts adjusted space count to active mobile and web clients.
    Postcondition: Live availability database and user interfaces reflect updated stall counts.

Use Case 13
    Title: Generate Revenue and Occupancy Analytics Report
    Actor: Parking Facility Operator
    Precondition: Transaction and gate sensor logs exist in the system database.
    Steps:
        1. Operator clicks "Reports and Analytics" in the administrative portal.
        2. Operator picks reporting date interval and target parking facility.
        3. Operator selects target metrics: total volume, peak hours, and net revenue.
        4. Operator clicks "Generate Report".
        5. Operator reviews generated graphs and exports file as CSV or PDF.
    Postcondition: Structured performance document is generated and downloaded.

Use Case 14
    Title: Dispatch Overstay Violation Alert
    Actor: Automated System Notification Service
    Precondition: Vehicle is inside garage past paid reservation departure time.
    Steps:
        1. System monitoring task identifies active parking pass past expiration cutoff.
        2. System queries exit gate events to verify vehicle has not scanned out.
        3. System marks reservation record with an overstay violation flag.
        4. System dispatches push alert to driver detailing overtime fee structure.
        5. System logs vehicle plate and bay identifier to operator enforcement queue.
    Postcondition: Overstay event is recorded and flagged on operator control screen.

Use Case 15
    Title: Audit API Security and Access Logs
    Actor: System Administrator
    Precondition: Administrator possesses elevated administrative access credentials.
    Steps:
        1. Administrator signs into cloud monitoring console.
        2. Administrator opens the "Security and Access Logging" interface.
        3. Administrator filters traffic by authorization failures and API errors.
        4. Administrator inspects flagged IP addresses and invalid token requests.
        5. Administrator archives audit records to meet compliance storage guidelines.
    Postcondition: Security audit log is reviewed, verified, and saved to long-term storage.

*** THIS MARKS THE END OF MY HOMEWORK 1 PORTION ***
