# Workflow Explanation – OPD Appointment Queue Automation

This document explains the purpose, trigger, and logic behind each automation workflow defined in the `/workflows` folder.  
Each workflow corresponds to a **real operational event** observed in OPD appointment handling and queue coordination.

The workflows are designed using **event-driven automation principles** and orchestrated conceptually using n8n.

---

## Design Principle

The OPD environment is inherently unpredictable due to:
- Variable consultation durations
- Walk-ins and no-shows
- Emergency interruptions

Therefore, the system avoids fixed appointment times and instead relies on:
- A position-based OPD queue
- A rolling buffer of active patients
- Queue movement driven only by real-world events

Each workflow below plays a specific role in maintaining this flow.

---

## 1. Appointment Request → OPD Queue Creation  
**Workflow file:** `appointment_request_to_queue.json`

### Trigger
This workflow is triggered when:
- A patient submits an OPD appointment request (online or via front desk), or
- A scheduling coordinator enters the request on behalf of the patient.

### What the Workflow Does
- Identifies the doctor and visit type
- Adds the patient to a **doctor-day–specific OPD queue**
- Assigns a **queue position**, not a consultation time

### Why This Workflow Exists
Fixed appointment slots do not work reliably in OPD settings.  
By converting appointment requests into **queue entries**, the system aligns with how OPDs function in reality and avoids false time commitments.

---

## 2. Active Buffer Assignment & Queue Stabilization  
**Workflow file:** `buffer_assignment_and_shift.json`

### Trigger
This workflow runs whenever:
- The OPD queue changes, or
- A patient is removed from the queue (consultation start or no-show)

### What the Workflow Does
- Selects the **next 3 patients** from the queue
- Marks them as **Active Buffer**
- Ensures only buffer patients are expected to arrive soon

### Why This Workflow Exists
Calling all patients to wait physically leads to overcrowding and frustration.  
The rolling buffer absorbs uncertainty by keeping only a small, controlled set of patients active at any time while others wait virtually.

This buffer is the **core stabilizing mechanism** of the entire system.

---

## 3. Consultation Start → Queue Advancement  
**Workflow file:** `consultation_event_listener.json`

### Trigger
This workflow is triggered when:
- The doctor starts a consultation for the current patient.

### What the Workflow Does
- Removes the current patient from the OPD queue
- Advances the queue by one position
- Initiates buffer recalculation

### Why This Workflow Exists
Queue movement is driven by **actual doctor activity**, not assumptions about time.  
This ensures the system remains accurate even when consultation durations vary significantly.

---

## 4. No-Show Detection & Handling  
**Workflow file:** `no_show_handling.json`

### Trigger
This workflow is triggered when:
- A patient enters the active buffer.

### What the Workflow Does
- Waits for a predefined buffer window
- Checks whether the patient has arrived
- If not, marks the patient as a no-show
- Removes them from the queue and pulls the next patient into the buffer

### Why This Workflow Exists
No-shows are common in OPDs and can block queues if handled manually.  
Automated no-show handling ensures the queue continues to move smoothly without coordinator intervention.

---

## 5. Emergency Override Handling  
**Workflow file:** `emergency_override.json`

### Trigger
This workflow is triggered when:
- A scheduling coordinator flags an emergency case.

### What the Workflow Does
- Temporarily freezes buffer movement
- Allows emergency patients to be prioritized
- Notifies affected buffer patients of delays
- Resumes normal flow once the emergency is resolved

### Why This Workflow Exists
Healthcare operations require **human-in-the-loop overrides**.  
This workflow ensures automation does not interfere with critical medical priorities.

---

## Summary

Each workflow addresses a specific operational problem:
- Appointment requests → queue creation
- Queue instability → buffer stabilization
- Consultation variability → event-driven advancement
- No-shows → automatic recovery
- Emergencies → controlled human override

Together, these workflows form a realistic, automation-assisted OPD appointment coordination system that reduces manual effort, crowding, and uncertainty without disrupting clinical processes.

---

