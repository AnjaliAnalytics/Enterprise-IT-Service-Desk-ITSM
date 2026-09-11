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



