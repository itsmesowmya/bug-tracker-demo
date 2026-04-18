# 🐛 Bug Tracker Demo — COCOS UI & Service Activation Issues

This repository tracks known bugs and incidents related to **COCOS UI**, **CAMS**, **SOST**, and **SPT** service activation systems. It is used as a sample issue database for the **Bug Command Center** — a multi-agent bug triage tool powered by Azure OpenAI.

---

## 📋 Table of Contents

- [About This Repo](#about-this-repo)
- [Systems Covered](#systems-covered)
- [Issue Categories](#issue-categories)
- [Issue List](#issue-list)
- [Labels Guide](#labels-guide)
- [How to Report a Bug](#how-to-report-a-bug)
- [Workarounds](#workarounds)
- [Escalation Guide](#escalation-guide)
- [Related Tools](#related-tools)

---

## About This Repo

This repo stores sample bug issues for the **Bug Command Center** multi-agent system. When a user reports a bug in the chat interface, the **Detective Agent** searches these issues for matches, the **Brain Agent** assesses severity using Azure OpenAI, and the **Fixer Agent** creates a ServiceNow ticket automatically.

> **Note:** Issues here are based on real KB articles from COCOS UI and Service Activation systems.

---

## Systems Covered

| System | Description |
|--------|-------------|
| **COCOS UI** | Vehicle conversion and validation interface |
| **CAMS** | Conversion kit application and management system |
| **SOST** | Service order and status tracking tool |
| **SPT** | Service provisioning tool for enabler management |
| **NAMS** | Network activation and management system |
| **VGCS** | Vehicle gateway and connected services backend |
| **VDAH** | Vehicle data and activation handler |
| **TISP** | Telecom infrastructure and service provisioning |
| **Service Configurator** | CSID management for connected services |

---

## Issue Categories

### 🔴 COCOS UI — Conversion Kit Issues
Problems that occur when applying conversion kits via CAMS through the COCOS UI. Typically results in `Invalid` or `Validation Required` vehicle status.

### 🟠 Service Activation — Stuck in Activating State
Problems where a service remains in `Activating` state in SOST or SPT and does not transition to `Active` after provisioning.

### 🟡 Backend Sync Issues
Problems caused by delays or failures in connected backend systems (NAMS, VGCS, VDAH, TISP) failing to respond or sync with SPT.

---

## Issue List

| # | Title | Category | Priority | Status |
|---|-------|----------|----------|--------|
| [#1](../../issues/1) | COCOS UI shows red cross after applying conversion kit 85136161 | Conversion Kit | P1 | Open |
| [#2](../../issues/2) | Vehicle stuck in Validation Required state after CAMS conversion | Conversion Kit | P2 | Open |
| [#3](../../issues/3) | Service stuck in Activating state in SOST — no transition to Active | Service Activation | P1 | Open |
| [#4](../../issues/4) | NAMS/VDAH Active response not received in SPT — enabler stuck | Backend Sync | P2 | Open |
| [#5](../../issues/5) | CSID missing in Service Configurator causing activation failure | Service Activation | P1 | Open |
| [#6](../../issues/6) | Conversion kit returns "no kit operations" for specific vehicle configs | Conversion Kit | P2 | Open |
| [#7](../../issues/7) | SPT not receiving final Active response from VGCS after provisioning | Backend Sync | P1 | Open |
| [#8](../../issues/8) | Expired conversion kit causes silent failure in NAMS | Conversion Kit | P3 | Open |
| [#9](../../issues/9) | Revalidation button in COCOS UI does not clear Invalid status | Conversion Kit | P2 | Open |
| [#10](../../issues/10) | Variant driven service shows data validation error in SPT dashboard | Backend Sync | P2 | Open |

---

## Labels Guide

| Label | Color | Description |
|-------|-------|-------------|
| `bug` | 🔴 Red `#d73a4a` | Confirmed bug or unexpected behavior |
| `critical` | 🟠 Orange `#e4e669` | High impact — affects all users or blocks core functionality |
| `validation` | 🟣 Purple `#7057ff` | Related to COCOS UI validation or Invalid vehicle status |
| `activating` | 🟢 Teal `#0075ca` | Service stuck in Activating state in SOST or SPT |
| `backend` | 🔵 Blue `#1d76db` | Root cause is in a backend system (NAMS, VGCS, VDAH, TISP) |
| `duplicate` | ⚫ Gray `#cfd3d7` | Same issue reported before |

---

## How to Report a Bug

### Option 1 — Use the Bug Command Center (recommended)
Type your bug description in the [Bug Command Center](https://your-app-url) chat. The agents will automatically search this repo, assess severity, and create a ServiceNow ticket.

### Option 2 — Create a GitHub Issue manually
1. Click **Issues** → **New Issue**
2. Use this template:

```
**System affected:** COCOS UI / CAMS / SOST / SPT / Other

**Steps to reproduce:**
1. 
2. 
3. 

**Expected behavior:**

**Actual behavior:**

**Error message (if any):**

**Vehicle ID / Chassis ID / Kit ID:**

**Screenshot:**
```

3. Add appropriate labels: `bug` + one of `critical`, `validation`, `activating`, `backend`

---

## Workarounds

### COCOS UI — Vehicle showing Invalid / Red Cross ❌

1. Open the vehicle in COCOS UI
2. Click **Revalidation**
3. Click the **Red Cross (❌)** icon on the vehicle
4. Review the detailed reason for the Invalid status
5. Correct the data if needed
6. Re-run validation

If the Invalid reason is still unclear, raise a ServiceNow case with:
- Vehicle ID
- Conversion Kit ID (e.g., `85136161`)
- Screenshot of the CAMS error

---

### Service stuck in Activating state (SOST / SPT)

**Step 1 — Check SOST**
Log in to SOST → Search by VIN/Chassis ID → Confirm service state is `Activating`

**Step 2 — Check SPT**
Open SPT → Search V2 → Search by Chassis ID or Service Order Number → Check Enabler states:
- `Active` → provisioning complete ✅
- `Activating` → still in progress ⚠️
- `Pending Activation` → not yet started or delayed ⚠️

**Step 3 — Check connected systems**

For **Connected Services (CSID-based)**:
- Go to [Service Configurator](https://serviceconfigurator.volvo.com/ValidateServices)
- Search by Chassis Number → Validate CSID

| CSID Status | Action |
|-------------|--------|
| Missing | Contact Service Configurator Team |
| Active | Contact SOST/COCOS Team |
| Activating / Pending | Contact VGCS Team (possible TISP/VDAH delay) |

For **Variant Driven Services**:
- Go to SPT → Support V2 → Technical Dashboard
- Search using Chassis Number in Business ID field
- Review `NAMSMQGatewayClientResponse` for errors

| Error | Action |
|-------|--------|
| `CAMS: Failed to apply conversion kit` | Contact NAMS Team |
| `Data validation errors` | Contact NAMS Team |
| `Expired conversion kit` | Contact NAMS Team |

---

## Escalation Guide

Escalate to **L3 teams** only if:

- ✅ Service is `Active` in backend but still `Activating` in SPT
- ✅ CSID is added and active but SPT is still showing `Activating`
- ✅ System sync failures are suspected across multiple systems

For **P1 issues** (3+ similar reports in same week), the Bug Command Center will auto-escalate to P1 and flag the pattern.

---

## Related Tools

| Tool | Purpose |
|------|---------|
| [Bug Command Center](https://your-app-url) | Multi-agent bug triage — Azure OpenAI + GitHub + ServiceNow |
| [Service Configurator](https://serviceconfigurator.volvo.com/ValidateServices) | Validate and manage CSID for connected services |
| [SPT Technical Dashboard](https://serviceprovweb-prod-master-cocos-prod.apps.aro-echo.euw-hub02.azure.volvo.net/TechnicalDash) | Review NAMSMQGatewayClientResponse errors |
| ServiceNow (SNOW) | Raise support cases for unresolved issues |

---

## 📬 Contact & Support

For issues that cannot be resolved through the UI or this repo:

- **COCOS Support Team** — raise a SNOW case with Vehicle ID + Kit ID + screenshot
- **NAMS Team** — for backend activation errors and conversion kit failures
- **VGCS Team** — for TISP/VDAH delays affecting service activation
- **Service Configurator Team** — for missing or incorrect CSID

---

*This repo is maintained as a sample dataset for the Bug Command Center demo. Issues are based on KB articles covering COCOS UI and service activation systems.*
