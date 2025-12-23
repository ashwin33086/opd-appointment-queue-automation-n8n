# Assumptions and Constraints – OPD Appointment Queue Automation

This document outlines the key assumptions made while designing the OPD appointment and queue automation system, as well as the constraints under which the solution operates.

The goal is to clearly define **what the system is designed to handle** and **what is intentionally kept out of scope**.

---

## Key Assumptions

### 1. Patient Identification Exists
The solution assumes that the hospital maintains a basic patient identification system where:
- Each patient has a unique patient ID, or
- New patients can be assigned a temporary ID at registration

This enables differentiation between new and returning patients.

---

### 2. Digital Consultation Start Event Is Available
It is assumed that doctors use a minimal digital system to:
- Open a patient record, or
- Mark the start of a consultation

This action serves as a reliable **event trigger** for advancing the OPD queue.

---

### 3. OPD Flow Is Queue-Based
The system assumes that:
- OPD consultations follow a sequential flow
- Patients are generally seen in order, except for emergencies

The automation is designed to assist this existing behavior, not replace it.

---

### 4. Emergency Cases Override Automation
Medical emergencies always take precedence over automation.

The solution assumes:
- Coordinators or staff can manually flag emergencies
- Automation pauses or adjusts flow when emergencies occur

Human judgment is always respected over system logic.

---

### 5. Patients Can Receive Notifications
It is assumed that patients can receive basic notifications via:
- SMS
- WhatsApp
- App notification

This is required to inform patients when they enter the active buffer or when delays occur.

---

### 6. Partial Adoption Is Acceptable
The system does not require full hospital-wide adoption on day one.

Even partial usage (specific departments or doctors) is assumed to deliver measurable benefits such as:
- Reduced crowding
- Better queue visibility
- Lower staff stress

---

## Constraints and Limitations

### 1. Not a Clinical Decision System
The solution does **not**:
- Decide medical priority
- Diagnose conditions
- Recommend treatments

All clinical decisions remain fully with doctors and medical staff.

---

### 2. No Inpatient or Bed Management
This design explicitly excludes:
- Inpatient admissions
- Bed allocation
- Ward or discharge management

It focuses only on **OPD appointment coordination**.

---

### 3. Not Production-Ready Infrastructure
The n8n workflows provided are:
- Conceptual and illustrative
- Designed to show orchestration logic

They are **not production-deployed** systems and do not include:
- Security hardening
- Compliance layers
- Performance optimizations

---

### 4. AI Predictions Are Advisory
Any predictive analytics used (e.g., consultation duration or no-show likelihood) are:
- Probabilistic in nature
- Used only to assist buffer efficiency

They do not enforce scheduling decisions or override human judgment.

---

### 5. Dependence on Human Inputs
Some steps still require human actions, such as:
- Doctor starting a consultation
- Coordinator flagging emergencies

The system assumes these actions occur reasonably on time.

---

## Summary

These assumptions and constraints are intentionally defined to keep the solution:
- Realistic
- Ethical
- Aligned with real OPD operations

By clearly stating what the system can and cannot do, the design avoids overclaiming and ensures that automation supports — rather than replaces — human decision-making in healthcare settings.

---
