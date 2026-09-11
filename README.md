# Enterprise IT Service Desk & Incident Management System

## Project Overview
**ApexGlobal Technologies Enterprise IT Service Desk & Incident Management System** is an enterprise-grade ITSM project built using ITIL v4 best practices on the ServiceNow platform (Personal Developer Instance). 

This project simulates how an enterprise Service Desk ingests, categorizes, prioritizes, escalates, resolves, and reports IT incidents and service requests while maintaining strict SLA compliance.

---

## Target Role Alignment
This project directly demonstrates hands-on competencies required for **Engineer - Cloud & Infra Management / IT Service Desk / IT Operations** roles.

### Job Description (JD) to Project Implementation Mapping

| Target JD Requirement | Project Implementation | Evidence / Deliverables |
| :--- | :--- | :--- |
| **24x7 IT Service Desk Support & L1 Support** | Service Portal configured with self-service catalog, automated routing, and dynamic multi-channel intake mechanisms. | Configured Service Portal landing page, catalog items, and intake workflow diagrams. |
| **Incident Management** | End-to-end incident lifecycle from creation, initial assessment, workarounds, root-cause tagging, to closure. | Incident form customization, record producers, and functional state transition tests. |
| **Service Request Management** | Service Catalog implementation for standard IT requests (hardware, software access, password resets). | Automated catalog items with multi-stage approval workflows (Flow Designer). |
| **Knowledge Base (KCS)** | Knowledge Centric Service (KCS) workflow where resolved incidents trigger Knowledge Article (KB) generation. | Published Knowledge Base articles linked directly to resolved incident records. |
| **Performance Monitoring & Reporting** | Custom ITSM Dashboards tracking MTTR, FCR, SLA breaches, and ticket distribution by category. | Real-time executive dashboard screenshots, exportable reports, and SLA breakdown metrics. |
| **VIP Support** | Custom VIP user flagging with automated priority escalation and dedicated high-priority assignment rules. | Client script auto-detecting VIP status + automated visual priority overrides. |
| **Major Incident Management (MIM)** | Major Incident trigger conditions, automated bridge creation alerts, and executive communication templates. | Major Incident workbench workflow execution logs and post-incident review (PIR) reports. |
| **Multichannel Support** | Support for email-to-incident ingestion, web portal submissions, and simulated agent phone intake. | Inbound Email Action rules and multi-channel ticket source tracking configurations. |
| **Access & Configuration Management** | Role-Based Access Control (RBAC) setup for end users, L1, L2, L3 agents, and System Administrators. | User groups, roles, and Group Membership matrices with ACL rule implementations. |
| **SLA Compliance** | Response and Resolution SLA definitions with 50%, 75%, and 100% breach notifications. | Configured SLA definitions, running SLA timers, and automated breach alert logs. |
| **Joiner-Mover-Leaver (JML)** | Standardized automated onboarding and offboarding catalog workflows for IT provisioning and deprovisioning. | Flow Designer workflows for automated user creation, software access, and task generation. |
| **Continuous Improvement (CSI)** | Incident pattern analysis identifying recurring root causes to drive Knowledge creation and workflow optimization. | CSI register entries based on incident trend reports and updated automation flows. |

---

## Enterprise Architecture Overview

```
 [ USER INGESTION CHANNELS ]
   Service Portal         Inbound Email         Agent Intake
          │                     │                    │
          └─────────────────────┼────────────────────┘
                                ▼
                  [ SERVICENOW ITSM ENGINE ]
┌───────────────────────────────────────────────────────────────┐
│  1. Ingestion & Routing                                        │
│     - Role-Based Access Control (End User / L1 / L2 / VIP)    │
│     - Data Ingestion & Field Mapping                          │
│                                                               │
│  2. Automation & Workflow Engine (Flow Designer)               │
│     - Priority Matrix (Impact x Urgency)                      │
│     - SLA Engine (Response & Resolution Timers)               │
│     - VIP Override & Major Incident Escalation Trigger        │
│                                                               │
│  3. Operations & Resolution                                   │
│     - Assignment Rules (L1 Helpdesk -> L2 Infra -> L3 Cloud)  │
│     - Knowledge Base Integration (KCS)                        │
│     - Joiner-Mover-Leaver (JML) Workflows                      │
└───────────────────────────────────────────────────────────────┘
                                │
                                ▼
                  [ OUTPUTS & REPORTING ]
   SLA Breach Alerts     Executive Dashboard    Auto-KB Output
```

```
---

## System Specifications & Specifications
- **Fictional Company:** ApexGlobal Technologies
- **Business Problem:** Enterprise growth caused fragmented IT requests, high MTTR (Mean Time to Resolve), unmonitored SLA breaches, and poor visibility into infrastructure outages.
- **Business Objective:** Centralize all IT Service Operations into a single ITSM platform to lower MTTR by 35%, enforce 99% SLA compliance for VIPs, automate JML user provisioning, and enable self-service troubleshooting via Knowledge Base management.

Markdown
---

## Phase 2: System Design & Organizational Structure

### Enterprise Structure (ApexGlobal Technologies)
- **Departments Configured:** `Information Technology`, `Human Resources`, `Finance`, `Sales`, `Operations`.

### Service Desk Roles & Group Matrix
- **ApexGlobal End Users:** Submits incidents and catalog requests via self-service portal.
- **L1 Service Desk Agents:** First-line triage, password resets, basic troubleshooting, FCR tracking.
- **IT Service Desk Management:** SLA breach tracking, escalation management, executive reporting.
- **L2 Network Support:** VPN, switches, routers, firewall, and network topology troubleshooting.
- **L2 Infrastructure Support:** On-premises servers, cloud virtual machines, Active Directory, storage.
- **L2 Application Support:** SaaS platform support, enterprise application issues, bug triage.
- **L3 Information Security:** Security incidents, access policy enforcement, threat response.

### Organizational Hierarchy Diagram
                      ┌──────────────────────────┐
                      │    VP of IT & Cloud      │
                      │      Operations          │
                      └────────────┬─────────────┘
                                   │
            ┌──────────────────────┴──────────────────────┐
            ▼                                             ▼
┌───────────────────────────┐                 ┌───────────────────────────┐
│   IT Service Desk Manager │                 │   Infra & Security Lead   │
└─────────────┬─────────────┘                 └─────────────┬─────────────┘
│                                             │
┌─────────┴─────────┐                       ┌───────────┼───────────┐
▼                   ▼                       ▼           ▼           ▼
┌───────────┐       ┌───────────┐           ┌───────────┐ ┌───────┐ ┌───────────┐
│ L1 Service│       │ L2 App    │           │ Network   │ │ Infra │ │ Security  │
│ Desk Team │       │ Support   │           │ Team      │ │ Team  │ │ Team      │
└───────────┘       └───────────┘           └───────────┘ └───────┘ └───────────┘


Markdown
---

## Phase 3: ITSM Data Model & Entity Schema

### Core Entities Architecture

| Entity / Table Name | Table Identifier | Primary Purpose | Key Relationships | Business Justification |
| :--- | :--- | :--- | :--- | :--- |
| **Users** | `sys_user` | Profile & identity records | `caller_id` on Incident, `requested_for` on Requests | Enables RBAC, authentication, and personalized service delivery. |
| **Assignment Groups**| `sys_user_group` | Team queues for work allocation | Assigned to `incident`, `sc_req_item`, `change_request` | Ensures work routes to operational units rather than single points of failure. |
| **Incidents** | `incident` | Unplanned service interruptions | Belongs to `sys_user`, links to `cmdb_ci`, `problem` | Restores normal service operations quickly to minimize business impact. |
| **Service Requests**| `sc_req_item` | Service Catalog order fulfillments | Child of `sc_request`, parent to `sc_task` | Fulfills planned standard end-user requests (hardware, software, access). |
| **Problems** | `problem` | Root-cause analysis tracking | 1-to-Many relationship with `incident` records | Prevents recurring incidents by identifying permanent fixes and workarounds. |
| **Changes** | `change_request` | System modifications & updates | Linked to `problem` and `cmdb_ci` | Controls risk during patches, upgrades, and structural modifications. |
| **Knowledge Articles**| `kb_knowledge` | SOPs, guides, and workarounds | Linked to `incident` via KCS resolution flow | Enhances First Contact Resolution (FCR) and enables self-service support. |
| **SLA Records** | `task_sla` | Response and resolution timing | Attached to `task` child tables | Enforces compliance with operational level agreements and contracts. |
| **Configuration Items**| `cmdb_ci` | IT infrastructure hardware/software | Linked to `incident`, `change_request`, `problem` | Maps technical dependencies to assess risk and pinpoint outage causes. |
| **Approvals** | `sys_approval` | Governance and sign-off tracking | Linked to `sc_req_item` and `change_request` | Enforces policy and financial oversight before fulfilling requests. |

### Data Model Relationship Schema
┌──────────────┐          1:N          ┌──────────────────┐
│   sys_user   │ ────────────────────► │     incident     │
└──────┬───────┘                       └────────┬─────────┘
│                                        │
│ 1:N                                    │ N:1
▼                                        ▼
┌──────────────┐          N:1          ┌──────────────────┐
│sys_user_group│ ◄──────────────────── │     cmdb_ci      │
└──────────────┘                       └────────┬─────────┘
│
│ N:1
▼
┌──────────────────┐
│  change_request  │
└──────────────────┘

---

## Phase 4: Incident Management Lifecycle & Sample Datasets

### Incident Lifecycle Flow
`New` ➔ `Assigned` ➔ `In Progress` ➔ `Pending` ➔ `Resolved` ➔ `Closed`

### Sample Incident Dataset (15 Production Scenarios)

| Incident # | Category | Short Description | Impact / Urgency | Priority | Assignment Group | Resolution Code |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `INC0010001` | VPN | Executive VPN authentication timeout | Medium / High | **P2 - High** | `L2 Network Support` | Solved (Permanently) |
| `INC0010002` | Email | Outlook failing to send outbound emails | Low / Medium | **P4 - Low** | `L1 Service Desk Agents` | Solved (Workaround) |
| `INC0010003` | Access | Active Directory domain login account locked | Low / High | **P3 - Medium** | `L1 Service Desk Agents` | Solved (Permanently) |
| `INC0010004` | Access | Self-service MFA reset failure | Low / Medium | **P4 - Low** | `L1 Service Desk Agents` | Solved (Permanently) |
| `INC0010005` | Hardware | Floor 3 Shared Network Printer Offline | Medium / Medium | **P3 - Medium** | `L2 Infrastructure Support` | Solved (Permanently) |
| `INC0010006` | Application | Enterprise ERP Application crash on startup | Low / Medium | **P4 - Low** | `L2 Application Support` | Solved (Permanently) |
| `INC0010007` | Network | Building B Core Switch Outage | High / High | **P1 - Critical** | `L2 Network Support` | Solved (Permanently) |
| `INC0010008` | Software | Microsoft Teams audio dropouts on Wi-Fi | Low / Low | **P4 - Low** | `L1 Service Desk Agents` | Solved (Workaround) |
| `INC0010009` | Software | Power BI Desktop installation deployment | Low / Low | **P4 - Low** | `L1 Service Desk Agents` | Solved (Permanently) |
| `INC00100010`| Security | Suspicious phishing link clicked | Medium / High | **P2 - High** | `L3 Information Security` | Solved (Permanently) |
| `INC00100011`| Hardware | Laptop battery not charging | Low / Low | **P4 - Low** | `L1 Service Desk Agents` | Solved (Permanently) |
| `INC00100012`| VPN | Split tunnel routing failing for AWS Console | Medium / Medium | **P3 - Medium** | `L2 Network Support` | Solved (Permanently) |
| `INC00100013`| Email | Shared Mailbox missing from Outlook | Medium / Medium | **P3 - Medium** | `L1 Service Desk Agents` | Solved (Permanently) |
| `INC00100014`| Access | Developer missing access to AWS S3 Bucket | Medium / Medium | **P3 - Medium** | `L3 Information Security` | Solved (Permanently) |
| `INC00100015`| Application | Salesforce CRM sync error 401 Unauthorized | Medium / High | **P2 - High** | `L2 Application Support` | Solved (Permanently) |

Markdown
---

## Phase 5: Service Request Management & Service Catalog

### Incident vs. Service Request

| Feature | Incident Management | Service Request Management |
| :--- | :--- | :--- |
| **Primary Goal** | Restore normal service after an unplanned outage or error. | Fulfill pre-approved, planned user requests for goods/services. |
| **Trigger** | Unexpected failure (e.g., VPN down, system crash). | User demand (e.g., new employee setup, software installation). |
| **Workflow** | Triage ➔ Investigation ➔ Fix ➔ Resolution. | Approval ➔ Provisioning ➔ Verification ➔ Fulfillment. |
| **SLA Impact** | High urgency (varies by business disruption). | Standard fulfillment lead time (e.g., 2–5 business days). |

### Service Catalog Architecture

#### 1. Hardware Category
- **Standard Laptop Request:** Provisioning developer or executive hardware models.
- **External Monitor Request:** Single or dual-monitor setup options.
- **Peripherals Replacement:** Ergonomic keyboard, mouse, and headset dispatch.

#### 2. Software Category
- **Microsoft Office / 365 License:** Automated license provisioning via Azure AD groups.
- **VPN Client Provisioning:** Configuration and access setup for Cisco AnyConnect / GlobalProtect.
- **Application Installation Request:** Silent deployment via SCCM/Intune for approved software.

#### 3. Access & Identity Category
- **New Employee Account Onboarding:** Provisioning AD, Email, and Slack accounts upon HR trigger.
- **Shared Network Folder Access:** Folder permissions managed via Security Group assignment.
- **Role-Based Application Access:** Granting elevated entitlements with manager sign-off.

### Request Fulfillment Data Architecture
┌─────────────────────────────────────────┐
│              Request (REQ)              │  <-- Master Order Bucket
└────────────────────┬────────────────────┘
│
▼
┌─────────────────────────────────────────┐
│          Requested Item (RITM)          │  <-- Individual Catalog Item
└────────────────────┬────────────────────┘
│
▼
┌─────────────────────────────────────────┐
│           Catalog Task (SCTASK)         │  <-- Fulfillment Task for Support Team
└─────────────────────────────────────────┘

Markdown
---

## Phase 6: Service Level Agreement (SLA) Management

> **Note:** The SLA targets and thresholds detailed below are project-defined example SLA targets created for this ITSM implementation model, not actual company SLAs.

### SLA Priority Targets Matrix

| Priority Level | Response Time SLA Target | Resolution Time SLA Target | Schedule / Operating Hours |
| :--- | :--- | :--- | :--- |
| **P1 — Critical** | `15 minutes` | `1 hour` | 24x7 Continuous |
| **P2 — High** | `30 minutes` | `4 hours` | 24x7 Continuous |
| **P3 — Medium** | `2 hours` | `8 business hours` | 8x5 Business Hours |
| **P4 — Low** | `4 hours` | `24 business hours` | 8x5 Business Hours |

---

### SLA State Lifecycle & Trigger Rules

- **SLA Start:** Triggered automatically upon Incident creation when `Active = true` and `Priority` is assigned.
- **SLA Pause:** Triggered when Incident `State` changes to `On Hold` / `Pending` with reason `Awaiting Caller` or `Awaiting Vendor`. (Prevents unfair SLA elapsed time during external delays).
- **SLA Resume:** Triggered when Incident state transitions back to `In Progress` after receiving caller/vendor response.
- **SLA Completion:** Triggered when Incident state changes to `Resolved` or `Closed` prior to target duration breach.
- **SLA Breach:** Occurs if elapsed business time exceeds target duration while ticket remains un-resolved.

---

### SLA Performance & Breach Reporting Architecture

                   ┌──────────────────────────┐
                   │   Incident Created       │
                   └────────────┬─────────────┘
                                │
                                ▼
                   ┌──────────────────────────┐
                   │   Task SLA Attached      │
                   └────────────┬─────────────┘
                                │
        ┌───────────────────────┴───────────────────────┐
        ▼                                               ▼
┌───────────────────────┐                       ┌───────────────────────┐
│ Ticket Resolved within│                       │  Time Target Exceeded │
│     Target Time       │                       │     Without Fix       │
└───────────┬───────────┘                       └───────────┬───────────┘
│                                               │
▼                                               ▼
┌───────────────────────┐                       ┌───────────────────────┐
│ Stage: COMPLETED      │                       │ Stage: BREACHED       │
│ (SLA Met)             │                       │ (Escalated to Manager)│
└───────────────────────┘                       └───────────────────────┘


#### Key SLA KPI Reports
1. **First Contact Resolution (FCR) Rate:** Percentage of tickets resolved on initial L1 contact without transfer.
2. **SLA Compliance %:** `(Total Met SLAs / Total Incidents) * 100` (Target: > 95%).
3. **Mean Time to Respond (MTTRsp):** Average time taken from ticket creation to agent assignment (`In Progress`).
4. **Mean Time to Resolve (MTTR):** Average elapsed time from ticket creation to state set to `Resolved`.

Markdown
---

## Phase 7: Major Incident Management (MIM) & Post-Incident Review (PIR)

### Core ITSM Concept Distinctions

| Metric / Aspect | Incident | Major Incident (MIM) | Problem Management |
| :--- | :--- | :--- | :--- |
| **Definition** | Unplanned service interruption or degradation. | Critical P1 outage causing widespread business impact. | Root cause investigation underlying recurring issues. |
| **Primary Goal** | Fast service restoration for affected user(s). | Emergency service restoration and business continuity. | Permanent elimination of underlying infrastructure defects. |
| **Target Audience** | Individual user or small department. | Entire enterprise, critical business lines, or customers. | Internal IT infrastructure and development teams. |
| **Management Lead** | L1 / L2 Support Agent. | Dedicated Major Incident Manager (MIM). | Problem Manager / L3 Systems Engineering Lead. |

---

### Enterprise Major Incident Scenario: P1 Company-Wide VPN Outage

- **Incident Reference:** `INC0010007-MIM`
- **Affected Service:** Global Remote Access Service (Cisco AnyConnect VPN Gateway)
- **Business Impact:** 4,500+ remote workers unable to authenticate or access internal corporate resources.
- **Priority:** `P1 - Critical` (Impact: 1 - High | Urgency: 1 - High)

#### End-to-End Major Incident Lifecycle & Timeline

Detection ──► P1 Declared ──► MIM Bridge ──► Workaround ──► PIR


- **08:00 AM — Detection:** Automated Monitoring (SAML SSO Gateway alert) triggers multiple L1 calls regarding VPN authentication failures.
- **08:05 AM — P1 Declaration:** L1 Service Desk escalates to IT Operations Manager; ticket flagged and approved as **Major Incident**.
- **08:10 AM — Technical Escalation & MIM Assignment:** MIM Lead assumes command, launches Emergency Bridge, and pages L2 Network & Security teams.
- **08:15 AM — Stakeholder Communication #1:** Initial broadcast sent to leadership & end users: *"Investigating enterprise VPN access degradation."*
- **08:25 AM — Investigation:** Technical team identifies expired SAML IDP Signing Certificate on the VPN Gateway cluster.
- **08:35 AM — Workaround / Resolution Applied:** Emergency secondary certificate bound to the gateway; SAML metadata re-synced.
- **08:45 AM — Service Restoration:** Authentication tests confirmed across 50 test users; VPN traffic stabilizes across all regions.
- **08:50 AM — User Communication #2:** Final notification sent: *"Enterprise VPN service fully restored."*
- **09:00 AM — Post-Incident Review (PIR):** Major Incident closed; Problem record created for Root Cause Analysis (RCA).

---

### Major Incident Escalation Matrix

┌─────────────────────────────────────────────────────────────────────────┐
│                        Major Incident Manager                           │
│                 (Command, Control & Stakeholder Comms)                  │
└────────────────────────────────────┬────────────────────────────────────┘
│
┌───────────────────┴───────────────────┐
▼                                       ▼
┌──────────────────────────────────┐   ┌──────────────────────────────────┐
│     L2/L3 Network & Security     │   │     Executive Leadership /       │
│  (Technical Root Cause Triage)   │   │     Business Stakeholders        │
└──────────────────────────────────┘   └──────────────────────────────────┘


---

### Stakeholder Communication Templates

#### Initial Broadcast Notification (T+15 mins)
> **SUBJECT:** [INCIDENT ALERT] Enterprise VPN Service Interruption — Priority 1
> 
> **Summary:** ApexGlobal IT is responding to a high-priority service interruption affecting the Cisco AnyConnect VPN Gateway.
> 
> **Impact:** Remote users are currently unable to establish VPN connections.
> 
> **Next Update:** 30 minutes or upon significant progress.

---

### Post-Incident Review (PIR) & RCA Report

- **Root Cause:** The primary SAML Signing Certificate utilized for VPN SSO single-sign-on authentication expired unexpectedly due to an unmonitored automated renewal cron task.
- **Resolution Applied:** Bound active secondary wildcard SSL/SAML certificate to the gateway cluster and force-refreshed identity provider metadata.
- **Preventive Actions (Action Items):**
  1. **PRB0010045:** Configure automated 30-day/15-day SSL/TLS certificate expiry alerts in Azure Key Vault / ServiceNow CMDB.
  2. **PRB0010046:** Add VPN SAML endpoints to synthetic monitoring suite for proactive ping checks.

  Markdown
---

## Phase 8: Executive VIP Support & Governance Framework

### VIP Process Governance & Compliance Rules
- **No Unsafe Shortcuts:** VIP status grants white-glove, prioritized handling, but **never** bypasses identity verification, security approvals, or audit logging.
- **Urgency Elevation:** Issues reported by VIPs default to **High Urgency** due to executive business exposure, elevating overall priority (e.g., Medium Impact + High Urgency = **P2 High**).
- **Executive Concierge Queue:** Incidents involving VIP callers auto-notify the **IT Service Desk Manager** and route to dedicated Executive Support Leads.

---

### Executive VIP Scenario: C-Suite Email Sync Failure Before Board Meeting

- **Caller:** `Chief Financial Officer (CFO)` [VIP Flag Active]
- **Issue:** Unable to access email via Outlook Desktop or Mobile device 30 minutes prior to quarterly earnings call.
- **Incident Number:** `INC0010020-VIP`
- **Impact / Urgency:** `2 - Medium` / `1 - High` ➔ **Priority:** `P2 - High`

#### Step-by-Step VIP Incident Handling Workflow

1. **VIP Identification & Visual Alert:**
   - Ticket ingested via phone call to Executive Desk. ServiceNow automatically flags caller with VIP visual badge, triggering SMS alert to the Service Desk Duty Manager.
2. **Prioritized Assignment & SLA Tracking:**
   - Ticket routed immediately to `L1 Service Desk Agents` (Executive Concierge Lead). Response SLA target set to **< 15 minutes**.
3. **Identity Verification & Security Controls:**
   - Agent performs out-of-band identity check via corporate MFA push verification before touching executive credentials or mailbox settings, preserving zero-trust security policies.
4. **Troubleshooting & Investigation:**
   - Agent discovers Exchange Online conditional access policy failure caused by an outdated device compliance token on the CFO's primary laptop.
5. **Compliant Resolution & Workaround:**
   - Agent generates a temporary, time-bound compliance bypass via Intune, re-authenticates the Outlook client, and verifies email send/receive functionality within 12 minutes.
6. **Executive Communication & Follow-up:**
   - Agent stays on the line until CFO confirms email sync on both laptop and mobile device, then sends a concise wrap-up summary to the Executive Assistant.
7. **Audit & Documentation:**
   - All steps, identity verification logs, and Intune temporary policy exceptions documented in the ticket for SOC2 compliance auditing.

   ---

## Phase 9: Knowledge Base & Knowledge-Centered Service (KCS)

### How Knowledge Management Drives Operational Efficiency
- **Ticket Volume Reduction (Deflection):** Empowering end-users with self-service KB articles via the Service Portal reduces repetitive L1 password reset and basic access tickets by up to 30%.
- **First Contact Resolution (FCR) Improvement:** Standardized SOPs allow L1 Service Desk agents to troubleshoot complex issues during initial contact without escalating to L2/L3 teams.
- **Mean Time to Resolve (MTTR) Optimization:** Eliminates reinventing the wheel by linking verified resolution steps directly to Incident records.

---

### Enterprise Knowledge Base Repository (10 Production SOP Articles)

#### 1. KB0010001 — How to Reset Windows Domain Password
- **Problem:** User forgotten or expired Active Directory / Windows login password.
- **Symptoms:** `The password you entered is incorrect` prompt at Windows lock screen.
- **Cause:** Domain password expired after 90-day cycle or user entered incorrect password multiple times.
- **Resolution:**
  1. Navigate to `https://sso.apexglobal.com/password-reset`.
  2. Authenticate using secondary MFA (Okta / Authenticator App).
  3. Enter new password meeting complexity requirements (12+ characters, mixed case, number, symbol).
- **Verification:** Lock Windows screen (`Win + L`) and log in with new password.
- **Escalation Criteria:** If MFA token is out of sync, escalate to L1 Service Desk for identity verification and bypass code.

#### 2. KB0010002 — How to Troubleshoot VPN Connectivity Issues
- **Problem:** Cisco AnyConnect VPN fails to establish connection from remote network.
- **Symptoms:** Error message `Connection attempt failed due to network latency or timeout`.
- **Cause:** Corrupted local SSL certificate cache or DNS resolution failure.
- **Resolution:**
  1. Open Command Prompt as Administrator and run `ipconfig /flushdns`.
  2. Open Cisco AnyConnect client, click **Preferences** ➔ check **Unblock untrusted certificates**.
  3. Restart `Cisco AnyConnect Secure Mobility Agent` in `services.msc`.
- **Verification:** Reconnect to `vpn.apexglobal.com`; verify green checkmark and internal intranet access.
- **Escalation Criteria:** If authentication fails with 401, escalate to L2 Network Support.

#### 3. KB0010003 — Outlook Not Sending or Receiving Emails
- **Problem:** Outbound emails stuck in Outbox folder.
- **Symptoms:** Error code `0x800CCC0F` in Outlook status bar.
- **Cause:** Corrupted Offline Outlook Data File (.OST).
- **Resolution:**
  1. Close Microsoft Outlook.
  2. Navigate to `%localappdata%\Microsoft\Outlook`.
  3. Rename `outlook.ost` to `outlook.ost.old`.
  4. Relaunch Outlook to force automatic rebuild of offline cache.
- **Verification:** Send a test email to `test-mail@apexglobal.com` and confirm receipt.
- **Escalation Criteria:** If issue persists on Outlook Web App (OWA), escalate to L2 Application Support (Exchange Online).

#### 4. KB0010004 — Microsoft Teams Audio and Video Troubleshooting
- **Problem:** Audio dropping or webcam failing during Teams calls.
- **Symptoms:** `Your microphone is not working` warning during call initialization.
- **Cause:** Windows privacy settings blocking app permissions or cached device driver conflicts.
- **Resolution:**
  1. Navigate to **Windows Settings ➔ Privacy & Security ➔ Microphone**.
  2. Ensure **Let desktop apps access your microphone** is toggled **ON**.
  3. In Teams, go to **Settings ➔ Devices** and confirm correct headset/mic is selected under primary audio.
- **Verification:** Perform a test call via **Teams ➔ Settings ➔ Devices ➔ Make a test call**.
- **Escalation Criteria:** If hardware driver failure persists, escalate to L1 Service Desk for hardware diagnostic.

#### 5. KB0010005 — Windows Network Connectivity & IP Release/Renew Procedure
- **Problem:** Workstation loses local LAN/Wi-Fi internet access.
- **Symptoms:** Yellow warning triangle on network icon; `No Internet, Secured` message.
- **Cause:** Invalid IP address assignment or stale DHCP lease.
- **Resolution:**
  1. Open CMD as Administrator.
  2. Run `ipconfig /release` followed by `ipconfig /renew`.
  3. Run `netsh winsock reset` and restart the computer.
- **Verification:** Run `ping 8.8.8.8` and verify successful ICMP echo replies.
- **Escalation Criteria:** If no IP assigned (`169.254.x.x` APIPA), escalate to L2 Infrastructure / Network team.

#### 6. KB0010006 — Active Directory Account Lockout Troubleshooting
- **Problem:** User account repeatedly locked out automatically.
- **Symptoms:** `Your account has been locked. Contact your administrator` message.
- **Cause:** Stale cached credentials on secondary devices (mobile email client, mapped network drives).
- **Resolution:**
  1. Disconnect Wi-Fi on user's mobile phone/tablet.
  2. Clear stored credentials in Windows **Credential Manager**.
  3. Unlock account via Active Directory Administrative Center or Self-Service Portal.
- **Verification:** Confirm zero failed audit login attempts in Active Directory event log for 15 minutes.
- **Escalation Criteria:** If continuous lockouts occur immediately after unlocking, escalate to L3 Security for brute-force investigation.

#### 7. KB0010007 — Shared Network Printer Mapping & Troubleshooting
- **Problem:** Unable to print to department network printer.
- **Symptoms:** Printer state shows `Offline` or print job remains in queue indefinitely.
- **Cause:** Print spooler service hang on local client machine.
- **Resolution:**
  1. Open `services.msc`, locate **Print Spooler**, and click **Stop**.
  2. Navigate to `C:\Windows\System32\spool\PRINTERS` and delete all temporary spool files.
  3. Start the **Print Spooler** service again.
- **Verification:** Print a test page from Printer Properties.
- **Escalation Criteria:** If print server port unreachable, escalate to L2 Infrastructure.

#### 8. KB0010008 — Software Installation Standard Procedure via Intune / Software Center
- **Problem:** User requires pre-approved software installation (e.g., VS Code, Chrome, Adobe Reader).
- **Symptoms:** Standard user account lacks Administrator privileges for manual `.exe` installation.
- **Cause:** Security policy restricting non-admin installer execution.
- **Resolution:**
  1. Open **Company Portal** (Microsoft Intune app) from Start Menu.
  2. Search for desired software in search catalog.
  3. Click **Install** to initiate elevated background deployment.
- **Verification:** Verify application shortcut appears in Start Menu and launches cleanly.
- **Escalation Criteria:** If software installation fails with exit code `1603`, escalate to L1 Service Desk.

#### 9. KB0010009 — New Employee IT Onboarding Checklist (Internal SOP)
- **Problem:** Provisioning equipment and access for new hires prior to Day 1.
- **Symptoms:** Unprovisioned accounts or missing hardware causing onboarding delays.
- **Cause:** Mandatory onboarding tasks not executed in sequence.
- **Resolution:**
  1. Verify active `HR-RITM` onboarding ticket in ServiceNow.
  2. Generate Active Directory account & Office 365 license assignment.
  3. Image standard laptop model using Windows Autopilot deployment profile.
  4. Dispatch welcome email containing temporary credentials to hiring manager.
- **Verification:** Perform test login with temporary credentials on imaged device.
- **Escalation Criteria:** If HR data mismatch occurs, escalate to HR Systems Admin.

#### 10. KB0010010 — Employee Offboarding IT Checklist (Internal SOP)
- **Problem:** Revoking system access and securing company assets upon employee departure.
- **Symptoms:** Active corporate credentials remaining accessible post-termination date.
- **Cause:** Incomplete offboarding task execution.
- **Resolution:**
  1. Disable Active Directory account and force-signout all active Office 365 sessions.
  2. Convert user mailbox to Shared Mailbox and assign access to departing employee's manager.
  3. Wipe mobile device corporate container via Intune MDM.
  4. Issue asset return shipping label for hardware retrieval.
- **Verification:** Confirm zero active OAuth tokens and account state set to `Disabled` in AD.
- **Escalation Criteria:** For immediate involuntary terminations, escalate directly to L3 Security Operations.

Markdown
---

## Phase 10: Joiner-Mover-Leaver (JML) Identity & Access Management

### Overview & Security Governance
The Joiner-Mover-Leaver (JML) workflow automates and secures the IT user lifecycle. It ensures zero-trust security compliance by enforcing **Least Privilege Access**, preventing **Permission Creep**, and guaranteeing immediate **Offboarding Deprovisioning**.

---

### 1. JOINER (Employee Onboarding Workflow)

- **Trigger:** HR System (Workday/BambooHR) generates an automated Onboarding Request (`REQ0020001`).

┌─────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│   HR Trigger    │ ──► │ Active Directory │ ──► │  Hardware & AD   │
│ (Workday Entry) │     │ User Created     │     │ Group Assignment │
└─────────────────┘     └──────────────────┘     └──────────────────┘


#### Step-by-Step Joiner Execution Steps:
1. **User & Identity Creation:** Automated creation of Active Directory (AD) / Azure AD account and corporate email address (`john.doe@apexglobal.com`).
2. **Role-Based Group Assignment:** Auto-assignment to core Security Groups:
   - `SG-All-Employees`
   - `SG-Finance-Department`
3. **Application & Access Provisioning:**
   - Microsoft 365 E5 License assignment via Azure AD Group Rule.
   - Cisco AnyConnect VPN profile provisioned with MFA token binding.
4. **Hardware Asset Provisioning:**
   - Catalog Task generated for Desktop Support to image standard Developer Laptop via Windows Autopilot.
5. **Day 1 Delivery:** Laptop and secure temporary credentials delivered to hiring manager.

---

### 2. MOVER (Internal Department Transfer Workflow)

- **Trigger:** HR transfer notification (`REQ0020050`) submitted 10 business days prior to effective transfer date.
- **Scenario:** Employee moves from **Finance Department** to **Data Analytics Team**.

#### Step-by-Step Mover Execution Steps:
1. **Access Audit & Revocation (Old Access):**
   - Remove membership from `SG-Finance-Department`.
   - Revoke read/write permissions to `\\fileserver\finance_shared`.
   - De-provision access to SAP Financials portal.
2. **New Entitlement Provisioning:**
   - Add membership to `SG-Data-Analytics`.
   - Provision access to Power BI Premium workspace and Snowflake Data Warehouse.
   - Grant access to `\\fileserver\analytics_shared`.
3. **Manager & Metadata Update:** Update manager, department code, and cost center attributes in Active Directory / ServiceNow User record.

---

### 3. LEAVER (Employee Offboarding Workflow)

- **Trigger:** HR Termination Notice (`REQ0030001`).
- **SLA Target:** Immediate account termination upon offboarding effective timestamp.

┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│ Active Directory │ ──► │ Session Revoke & │ ──► │   Asset Return   │
│ Account Disabled │     │ Shared Mailbox   │     │   SCTASK Created │
└──────────────────┘     └──────────────────┘     └──────────────────┘


#### Step-by-Step Leaver Execution Steps:
1. **Immediate Identity Disablement:**
   - Set Active Directory account attribute `Active = False`.
   - Revoke all active Azure AD OAuth/SAML single sign-on refresh tokens (Force Sign-Out).
2. **Access & Security Group Revocation:**
   - Strip all Active Directory security group memberships and elevated application roles.
3. **Data Protection & Mailbox Delegation:**
   - Convert primary user mailbox to Shared Mailbox and delegate access to line manager.
   - Apply 30-day Litigation Hold to user OneDrive/Email archive.
4. **Asset Recovery & Request Closure:**
   - Generate Catalog Task (`SCTASK`) for IT Logistics to dispatch prepaid shipping container for laptop, badge, and peripherals return.
   - Upon asset receipt confirmation, close all open access-related service requests (`REQ'/`RITM`).

---

## Phase 11: Access Management & Security Governance Framework

### Enterprise Access Management Principles

1. **Principle of Least Privilege:**
   - Access is restricted to the minimum permissions required for job execution.
   - Default state for all internal systems is `Deny All` until authorized.
2. **Mandatory Multi-Tier Approvals:**
   - No access provisioned without auditable manager and application owner sign-off.
3. **Separation of Duties (SoD):**
   - Requester, Approver, and Provisioner must be three distinct individuals.
   - Prevents privileged access abuse and internal financial/security conflicts.
4. **Periodic Access Review (Access Certification):**
   - Quarterly automated access reviews sent to department managers to confirm active employee entitlements.
   - Unverified access auto-revoked after 14 calendar days.

---

### End-to-End Access Request & Provisioning Lifecycle

[User Request] ──► [Manager Approval] ──► [IT Owner Approval] ──► [Provisioning] ──► [Verification]

#### Step-by-Step Access Execution Workflow (Example: Production Database Access)

- **Request Reference:** `RITM0010099`
- **Target Entitlement:** Read-Only Access to Production Finance Database (`DB-FIN-PROD-01`)

1. **User Access Request:**
   - End-user submits request via Service Catalog with business justification.
   - System records requester ID, timestamp, and target resource details.
2. **Manager Approval (Tier 1):**
   - Automated approval task assigned to requester's direct manager.
   - Manager reviews business need and approves via email / portal action.
3. **IT / Resource Owner Approval (Tier 2):**
   - Secondary approval task routed to **Database Security Administrator**.
   - Security team verifies Separation of Duties compliance before sign-off.
4. **Provisioning (Automated / Manual Task):**
   - Catalog Task (`SCTASK`) assigned to `L3 Information Security`.
   - Security engineer provisions user into Active Directory group `SG-DB-FIN-READ`.
5. **Verification & Audit Logging:**
   - Automated script tests database authentication using provisioned group.
   - Confirmation logged in `RITM0010099`; ticket closed with complete audit trail.
6. **Deprovisioning (Lifecycle Trigger):**
   - Access set to expire automatically after 90 days unless re-certified during quarterly access review.