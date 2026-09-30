# Implement Client Script & UI Policy (Incident)

## ServiceNow Skill Wallet Micro Project

This project demonstrates how **UI Policies, UI Policy Actions, and Client Scripts** can work together on the ServiceNow **Incident** table to enforce dynamic field behavior, automate updates, validate records before save, and restrict direct list edits.

## Problem Statement

Incident records require consistent and accurate data entry for effective triage, routing, and resolution. Manual checks can lead to incomplete or inconsistent submissions. This project addresses that problem by applying conditional field behavior and client-side validation directly to the Incident user interface.

## Objective

The objective is to demonstrate ServiceNow client-side controls that:

- Apply conditional UI behavior based on Incident Impact.
- Make selected fields mandatory.
- Make the Urgency field read-only under a defined condition.
- Automatically set Urgency for High Impact incidents.
- Prevent saving a High Impact Incident when Assigned To is empty.
- Prevent State changes through inline list editing.
- Allow State changes through the normal Incident form.

## Skills Demonstrated

- Incident Management
- UI Policy
- UI Policy Actions
- Client Scripts
- Form Validation
- onChange Client Script
- onSubmit Client Script
- onCellEdit Client Script

---

## Project Configuration

### 1. UI Policy — High Impact Control

**Table:** Incident  
**Active:** true  
**Condition:** Impact is 1 - High  
**Reverse if false:** true

The project source specifies an action for **Assignment group** to be mandatory.

See: [`ui-policy/high-impact-control.md`](ui-policy/high-impact-control.md)

### 2. UI Policy Action — Urgency

**Field:** Urgency  
**Read-only:** true  
**Visible:** Leave as is

When Impact is High, Urgency becomes read-only.

See: [`ui-policy/urgency-action.md`](ui-policy/urgency-action.md)

### 3. onChange Client Script — Auto set urgency

**Table:** Incident  
**Type:** onChange  
**Field:** Impact  
**Active:** true

When Impact changes to High, Urgency is automatically set to High.

Source code: [`client-scripts/onChange-auto-set-urgency.js`](client-scripts/onChange-auto-set-urgency.js)

### 4. onSubmit Client Script — Save validation

**Table:** Incident  
**Type:** onSubmit  
**Active:** true

When Impact is High and Assigned To is empty, the Incident cannot be saved.

Source code: [`client-scripts/onSubmit-prevent-save.js`](client-scripts/onSubmit-prevent-save.js)

### 5. onCellEdit Client Script — List edit blocking

**Table:** Incident  
**Type:** onCellEdit  
**Field:** State  
**Active:** true

Direct State changes through Incident list editing are blocked. Users are instructed to open the Incident form instead.

Source code: [`client-scripts/onCellEdit-block-state.js`](client-scripts/onCellEdit-block-state.js)

---

## Testing Scenarios

### Test 1 — Mandatory Enforcement

1. Open **Incident → Create New**.
2. Set Impact to **High**.
3. Leave Assigned To empty.
4. Click Submit.
5. Expected result: the Incident is not saved and an error is shown for Assigned To.

### Test 2 — Successful Save

1. Open the Incident form.
2. Set Impact to High.
3. Select a user in Assigned To.
4. Submit the Incident.
5. Expected result: the record saves successfully.

### Test 3 — Reverse Condition

1. Open an Incident where Impact is High.
2. Change Impact to Medium.
3. Expected result: the UI Policy condition becomes false and its controlled field behavior is reversed.
4. Save the record.

### Test 4 — List Edit Blocking

1. Open **Incident → All**.
2. Double-click the State field of an Incident.
3. Expected result: an alert states that State cannot be updated using list editing.
4. The State value remains unchanged.

### Test 5 — Form-Based State Update

1. Open the Incident form.
2. Change State using the form.
3. Click Update.
4. Expected result: the State change is saved successfully.

---

## ServiceNow Navigation

### UI Policy

`System UI → UI Policies`

### Client Scripts

`System UI → Client Scripts`

### Incident Testing

`Incident → Create New`

### Incident List Testing

`Incident → All`

---

## Repository Structure

```text
Implement-Client-Script-UI-Policy-Incident/
├── README.md
├── client-scripts/
│   ├── onChange-auto-set-urgency.js
│   ├── onSubmit-prevent-save.js
│   └── onCellEdit-block-state.js
├── ui-policy/
│   ├── high-impact-control.md
│   └── urgency-action.md
├── testing/
│   └── test-cases.md
└── screenshots/
    └── README.md
```

## Important Note

This repository contains the **project source/documentation**, not a native ServiceNow update-set export. ServiceNow records such as UI Policies and Client Scripts are configured inside the ServiceNow instance. The JavaScript files here document the scripts used by the project.

The project source also describes both an **Assignment group UI Policy action** and an **Assigned To onSubmit validation**. They are separate controls and should not be treated as the same field.

## Result

The completed implementation demonstrates dynamic Incident form behavior, automatic Urgency updates, save-time validation, UI Policy controls, and protection against direct State list edits.
