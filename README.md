# Case Escalation Flow

**Tech Stack:** Salesforce Record-Triggered Flow, Chatter | 2026

## Overview
A Salesforce automation solution that auto-escalates unresolved high-priority 
Cases after 24 hours of inactivity, ensuring critical customer issues don't 
get missed by support teams.

## Problem Statement
Support teams often lose track of high-priority Cases that sit unresolved. 
Without an automated escalation mechanism, these Cases can go unnoticed for 
extended periods, leading to poor customer experience and SLA violations.

## Solution
Built a **Record-Triggered Flow** on the Case object that:

1. **Monitors** Cases with `Priority = High` and an unresolved `Status`
2. **Waits 24 hours** from Case creation via a Scheduled Path
3. **Re-validates** the entry conditions at the 24-hour mark (so already-resolved 
   Cases are automatically skipped)
4. **Escalates** the Case by:
   - Reassigning ownership to a dedicated `Escalation Queue`
   - Updating `Status` to `Escalated`
5. **Notifies** the assigned team via an automated **Chatter post** on the Case 
   record

## Flow Architecture

```
Record-Triggered Flow (Case: Priority = High, Status ≠ Closed)
        │
        ▼
Scheduled Path — 24 Hours After Case Creation
        │
        ▼
Get Records — Fetch Escalation Queue (Group)
        │
        ▼
Update Records — Assign Case to Queue + Set Status = Escalated
        │
        ▼
Action — Post to Chatter (Notify Team)
```


## Key Salesforce Concepts Used
- Record-Triggered Flows with Scheduled Paths
- Declarative automation (no Apex required)
- Queue-based case assignment
- Get Records with filter conditions on the `Group` object
- Chatter API action within Flow Builder

## Testing
Validated using Flow Builder's **Debug** tool by simulating the 24-hour 
scheduled path against a test Case, confirming:
- Correct queue record retrieval
- Successful Case ownership reassignment
- Chatter notification post creation

## Files
- `force-app/main/default/flows/Case_Escalation_Flow.flow-meta.xml` — Flow metadata
