# overeasy-odoo

> **Self-Hosted Odoo v19 — Custom Module Development & Deployment**
> Over Easy Hospitality | Odoo.sh | Python | QWeb | GitHub CI/CD

---

## Overview

This repository contains the custom Python modules built and deployed for **Over Easy Hospitality (OEH)**, a temporary housing and insurance relocation company. This is **Phase 2** of a two-phase Odoo build.

👉 [Phase 1 — SaaS CRM Architecture](https://github.com/SerraThorne/odoo-crm-architecture)

---

## Phase 1 vs Phase 2

| | Phase 1 (SaaS) | Phase 2 (Self-Hosted) |
|---|---|---|
| **Repo** | `odoo-crm-architecture` | `overeasy-odoo` |
| **Environment** | Odoo SaaS (cloud-managed) | Odoo.sh (self-hosted) |
| **Customization** | Studio fields, automations, pipelines | Custom Python modules, cron engines, Slack webhooks |
| **Deployment** | No-code / low-code | GitHub CI/CD (main + production branches) |
| **Focus** | CRM architecture & documentation | Module development & DevOps |

The migration from SaaS to self-hosted was driven by the need for deeper customization — specifically custom Python logic, cron-based automation, and Slack integration — which are not available in the managed SaaS environment.

---

## Tech Stack

- **Platform:** Odoo v19 on Odoo.sh
- **Language:** Python 3 (Odoo ORM / model layer)
- **Templating:** QWeb / XML
- **Version Control:** GitHub (main + production branches, 360+ commits)
- **Integrations:** Slack Webhooks, Azure OAuth2, IMAP/SMTP
- **Security:** `ir.model.access.csv` per module with role-based CRUD permissions

---

## Repository Structure

```
overeasy-odoo/
├── oeh_cleaning_trackers/       # Monthly cleaning schedule tracking
│   ├── models/
│   ├── views/
│   ├── security/
│   ├── __init__.py
│   └── __manifest__.py
│
├── oeh_guest_issues/            # Guest issue logging and escalation
│   ├── models/
│   ├── views/
│   ├── security/
│   ├── __init__.py
│   └── __manifest__.py
│
├── oeh_moveout_extensions/      # Move-out workflow and extension tracking
│   ├── models/
│   ├── views/
│   ├── security/
│   ├── __init__.py
│   └── __manifest__.py
│
├── oeh_pds_report/              # Property Data Sheet — QWeb PDF report
│   ├── models/
│   ├── views/
│   ├── security/
│   ├── __init__.py
│   └── __manifest__.py
│
├── oeh_property_outreach/       # Homeowner outreach and marketing campaigns
│   ├── models/
│   ├── views/
│   ├── security/
│   ├── __init__.py
│   └── __manifest__.py
│
├── oeh_update_engine/           # CRM lead update automation + cron jobs
│   ├── models/
│   ├── views/
│   ├── security/
│   ├── __init__.py
│   └── __manifest__.py
│
├── oeh_utilities/               # Shared utilities + QWeb PDF template list
│   ├── report/
│   ├── __init__.py
│   └── __manifest__.py
│
└── README.md
```

---

## Modules

### `oeh_cleaning_trackers`
Tracks monthly cleaning schedules tied to active property placements. Includes a custom model with security access controls and a dedicated menu view within the OEH app.

### `oeh_guest_issues`
Logs and manages guest-reported issues during insurance housing placements. Supports categorization, status tracking, and escalation workflows across the operations team.

### `oeh_moveout_extensions`
Handles move-out coordination, including extension requests, Notice to Vacate tracking, and move-out status progression. Integrates with the CRM pipeline stage logic.

### `oeh_pds_report`
Generates a **Property Data Sheet (PDS)** as a dynamic QWeb PDF report. The PDS compiles property details, utility information, compliance fields, and homeowner data into a formatted document for insurance claim submissions.

### `oeh_property_outreach`
Manages homeowner outreach campaigns and property submissions. Tracks outreach status, contact history, and submission outcomes to support inventory expansion efforts.

### `oeh_update_engine`
The core automation module. Contains:
- **Update Follow-Up cron** — scheduled reminders for stale CRM records
- **Call Follow-Up cron** — triggers follow-up tasks based on call outcome fields
- **Approved Checklist Reminder cron** — notifies team when approved properties have pending checklist items
- **Slack webhook notifications** — posts stage-change events to 4 operational Slack channels with emoji-tagged alerts

### `oeh_utilities`
Shared utility module providing the QWeb PDF template list and common helpers used across other OEH modules.

---

## CRM Pipeline

The self-hosted deployment runs a **14-stage automated CRM pipeline** built on top of these modules:

```
New Lead → Outreach Sent → Property Submitted → Under Review →
Approved → Lease Preparation → Lease Sent → Lease Signed →
Move-In Scheduled → Active Placement → Extension Review →
Move-Out Scheduled → Move-Out Complete → Closed
```

Each stage transition triggers automation rules, field updates, and where applicable, Slack channel notifications.

---

## Deployment

This repo is deployed via **Odoo.sh** with a two-branch CI/CD workflow:

- `main` — staging / development branch
- `production` — live deployment branch

All module updates are committed to `main`, reviewed, and promoted to `production` via Odoo.sh's built-in pipeline. The repo has **360+ commits** across the development lifecycle.

---

## Screenshots

> Screenshots show the live self-hosted Odoo.sh deployment. The platform, custom app icons, module dashboards, and pipeline views were all built and configured as part of this project.

### App Dashboard
![OEH App Dashboard](https://github.com/SerraThorne/overeasy-odoo-sh/blob/main/Dashboard.jpg)
*Custom Odoo.sh deployment showing all installed OEH modules with custom app icons*

### CRM Pipeline
![CRM Pipeline View](https://github.com/SerraThorne/overeasy-odoo-sh/blob/main/Stage%201.jpg) 
(https://github.com/SerraThorne/overeasy-odoo-sh/blob/main/Stage%202.jpg) 
(https://github.com/SerraThorne/overeasy-odoo-sh/blob/main/Stage%203.jpg)
*14-stage automated pipeline with conditional field logic and stage-based automation rules*

### Cleaning Trackers Module
![Cleaning Trackers](https://github.com/SerraThorne/overeasy-odoo-sh/commit/2cc34d46af759c3162f3d9b8521dea3c715f4a46#r193004007)
*Custom module for monthly cleaning schedule management*

### Property Outreach Module
![Property Outreach](https://github.com/SerraThorne/overeasy-odoo-sh/commit/2cc34d46af759c3162f3d9b8521dea3c715f4a46#r193004007)
*Homeowner outreach campaign tracking and property submission management*

### Guest Issues Module
![Guest Issues](https://github.com/SerraThorne/overeasy-odoo-sh/commit/2cc34d46af759c3162f3d9b8521dea3c715f4a46#r193004007)
*Guest issue logging with escalation workflow*

### Move-Outs & Extensions Module
![Move-Outs](https://github.com/SerraThorne/overeasy-odoo-sh/commit/2cc34d46af759c3162f3d9b8521dea3c715f4a46#r193004007)
*Move-out coordination and extension request tracking*

### Property Data Sheet (PDF Output)
![PDS Report](screenshots/pds-report.png)
*QWeb-generated Property Data Sheet PDF for insurance claim submissions*

### Module Structure (GitHub)
![Module Structure](screenshots/module-structure.png)
*Repository structure showing custom Python modules with models, views, and security layers*

---

## Related

- 📁 [Phase 1 — odoo-crm-architecture](https://github.com/SerraThorne/odoo-crm-architecture) — SaaS CRM build: pipeline design, Studio fields, QWeb PDF templates, Azure OAuth2 email integration, and full technical documentation suite
- 👤 Author: [SerraThorne](https://github.com/SerraThorne)
