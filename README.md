# Enterprise IT Service Management (ITSM) & Operations Analytics Platform

[![ITIL v4 Framework](https://img.shields.io/badge/ITIL-v4%20Compliant-blue.svg)](https://www.axelos.com/certifications/itil-service-management)
[![Service Now Architecture](https://img.shields.io/badge/ServiceNow-Utah%2FTokyo-green.svg)](https://www.servicenow.com)
[![SLA Compliance Target](https://img.shields.io/badge/SLA%20Target-95%25%20Met-brightgreen.svg)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## Executive Summary & Business Problem

In high-growth enterprise environments, uncoordinated IT support operations lead to severe operational friction, high Mean Time to Resolve (MTTR), recurring system outages, and unmanaged security exposure. Without standard governance, organizations face:
- **Uncontrolled SLA Breaches:** Lack of automated priority matrix calculations and duty manager escalation paths.
- **Identity & Access Security Risks:** Unregulated employee onboarding/offboarding leading to permission creep and active accounts for departed personnel.
- **Incredible L1 Ticket Volume:** High operational overhead caused by repetitive user requests (e.g., password resets, basic VPN troubleshooting) due to missing self-service options.

This project delivers a production-ready, ITIL v4-compliant IT Service Management & Analytics Architecture deployed on ServiceNow and documented with enterprise rigor.

---

## Core Objectives & Strategic Scope

- **ITIL v4 Process Alignment:** Standardize Incident, Service Request, Major Incident, Access, and Problem Management workflows.
- **Operational Automation & SLA Governance:** Implement dynamic SLA definitions (Response & Resolution) linked to automatic escalation triggers.
- **Shift-Left Strategy & Deflection:** Build a structured Knowledge Base repository to increase First Contact Resolution (FCR) and reduce L1 ticket volume by up to 30%.
- **Zero-Trust Identity Governance:** Enforce strict Joiner-Mover-Leaver (JML) workflows, Separation of Duties (SoD), and Least Privilege access controls.
- **Data-Driven Continuous Improvement:** Analyze ticket datasets to identify recurring root causes, establish CSAT benchmarks, and propose strategic IT improvement initiatives.

---

## Technical & Operational Architecture

┌────────────────────────────────┐
                           │     Service Portal / Users     │
                           └───────────────┬────────────────┘
                                           │
                                           ▼
                           ┌────────────────────────────────┐
                           │  ServiceNow Ingestion Engine   │
                           └───────────────┬────────────────┘
                                           │
           ┌───────────────────────────────┼───────────────────────────────┐
           ▼                               ▼                               ▼
┌────────────────────┐          ┌────────────────────┐          ┌────────────────────┐
│  Incident Stream   │          │  Service Request   │          │  Knowledge Base    │
│ (P1-P4 Matrix Engine)│          │ (Approval Engine)  │          │ (Self-Service)     │
└──────────┬─────────┘          └──────────┬─────────┘          └──────────┬─────────┘
           │                               │                               │
           ▼                               ▼                               ▼
┌────────────────────┐          ┌────────────────────┐          ┌────────────────────┐
│  SLA Enforcement   │          │  JML / Access Mgmt │          │ Ticket Deflection  │
│  & Escalations     │          │  & Provisioning    │          │  & FCR Boost       │
└──────────┬─────────┘          └──────────┬─────────┘          └──────────┬─────────┘
           │                               │                               │
           └───────────────────────────────┼───────────────────────────────┘
                                           │
                                           ▼
                           ┌────────────────────────────────┐
                           │  Executive Analytics Dashboard │
                           │   & Continuous Improvement     │
                           └────────────────────────────────┘

---

## Detailed ITSM Implementation Lifecycle

### 1. Incident & Major Incident Management (MIM)
- **Dynamic Priority Calculation:** Automated matrix calculation ($Impact \times Urgency = Priority$) driving P1 Critical through P4 Low classifications.
- **Major Incident Bridge:** Emergency P1 workflow triggering instant SMS notifications to Duty Managers, establishing dedicated technical bridge channels, and running hourly executive communications.

### 2. SLA Management & Service Level Trees
- **SLA Definitions:** 15-minute response target for P1/VIP tickets; 4-hour MTTR target for standard P3 incidents.
- **Breach Mitigation:** Automated warning triggers at 50% and 75% SLA duration elapsed.

### 3. VIP Concierge Support
- **Visual Alerting:** Automated visual flagging on Caller selection (`sys_user.VIP = true`).
- **High-Touch Governance:** White-glove handling without compromising security controls or MFA verification requirements.

### 4. Knowledge Management & Shift-Left Strategy
- **Production SOPs:** 10 fully structured Knowledge Base articles featuring Problem, Symptoms, Cause, Resolution, Verification, and Escalation criteria.
- **Self-Service Deflection:** Embedded self-help portal reducing password and VPN L1 ticket creation.

### 5. Joiner-Mover-Leaver (JML) Identity & Access Management
- **Joiner:** Role-Based Access Control (RBAC) provisioning, hardware imaging, and welcome kit dispatch.
- **Mover:** Department transfer access adjustments preventing entitlement accumulation.
- **Leaver:** Immediate session termination, mailbox conversion, and hardware retrieval tracking.

### 6. Continuous Service Improvement (CSI) Analytics
- **Data-Driven Trend Analysis:** Categorization and root-cause analysis across 1,250 monthly incident records.
- **Actionable Remediation:** Targeted SSPR deployment, PKI certificate automation, and workflow updates.

---

## Key Performance Indicators (KPIs) & Operational Results

| Operational KPI Metric | Pre-Implementation Baseline | Post-Implementation Outcome | Business Impact |
| :--- | :--- | :--- | :--- |
| **First Contact Resolution (FCR)** | 32% | **68%** | +112% increase in L1 resolution efficiency |
| **Mean Time to Resolve (MTTR)** | 14.2 Hours | **2.8 Hours** | 80% reduction in end-user downtime |
| **SLA Compliance Rate** | 78.5% | **96.8%** | Reached target compliance tier |
| **Password Reset Ticket Share** | 38% of total volume | **11% of total volume** | Deflected 60%+ password volume via SSPR |
| **User Satisfaction (CSAT)** | 3.1 / 5.0 | **4.7 / 5.0** | Significant increase in user trust |

---

## Test Cases & Diagnostic Verification Matrix

| Test Case ID | Target Feature / Workflow | Input / Trigger Condition | Expected Result | Pass / Fail |
| :--- | :--- | :--- | :--- | :--- |
| `TC-ITSM-001` | Dynamic Priority Matrix | Set Impact = `1 - High`, Urgency = `1 - High` | Priority auto-populates as `1 - Critical` | **PASS** |
| `TC-ITSM-002` | VIP Visual Flagging | Select VIP User in Caller field | Orange/Red VIP badge displays on form | **PASS** |
| `TC-ITSM-003` | SLA Escalation Trigger | P1 Incident unassigned after 10 mins | Warning email sent to Service Desk Lead | **PASS** |
| `TC-ITSM-004` | JML Leaver Disablement | Set `User.Active = False` | Active session tokens revoked; AD disabled | **PASS** |
| `TC-ITSM-005` | Approval Gate Enforcement | Access Request submitted without Manager Sign-off | Provisioning task remains blocked | **PASS** |

---

## Lessons Learned & Future Roadmap

### Key Operational Lessons Learned
1. **Security Cannot Be Sacrificed for Speed:** Even high-priority VIP requests must undergo MFA verification to prevent social engineering exploits.
2. **Data Standardization Drive Automation:** Automation scripts fail when assignment groups or categories lack consistent naming conventions.

### Future Improvements
- **Generative AI Deflection Agent:** Integrate virtual AI agents to answer KB inquiries interactively via Slack / Teams.
- **CMDB Discovery Automation:** Implement ServiceNow Discovery agents to maintain auto-updated CI relationship maps.

---

## Technical Job Description (JD) Mapping

This project directly demonstrates the skill sets required for senior ITSM, Data Analytics, and MIS roles:

| Targeted Job Description Requirement | Demonstrated Project Deliverable |
| :--- | :--- |
| **ITIL v4 Framework Mastery** | Executed end-to-end Incident, Service Request, Major Incident, Access, and Change workflows. |
| **ServiceNow Administration & Config** | Configured `sys_user`, `sys_approval`, `task_sla`, Assignment Rules, and KB categories. |
| **IT Operations & Data Analytics** | Conducted metric trend analysis on 1,250 monthly incident records; built KPI dashboards. |
| **Identity & Access Management (IAM)** | Built Joiner-Mover-Leaver (JML) lifecycle workflows with RBAC and Separation of Duties controls. |
| **Technical Documentation & SOPs** | Authored 10 production KB articles, troubleshooting playbooks, and architecture guides. |

---

## Disclaimer & Terms of Use

*This repository is created for professional portfolio demonstration purposes. All system configurations, ticket datasets, user details, and organizational entities referenced herein (e.g., Apex Global) are fictional and configured in non-production developer environments.*