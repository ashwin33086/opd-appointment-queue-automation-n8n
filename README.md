# OPD Appointment Queue & Buffer-Based Automation (n8n)

## Overview

This repository demonstrates how a **broken OPD appointment and queue coordination workflow** can be redesigned using **event-driven automation** and a **buffer-based queue model**, orchestrated using **n8n**.

The solution focuses on **Scheduling & Appointment Coordinators** and addresses a common real-world hospital problem:  
patients waiting for hours due to unpredictable consultation durations, walk-ins, emergencies, and manual queue handling.

This is **not a production hospital system**.  
It is a **conceptual yet executable automation blueprint** that shows how operational workflows can be translated into automation logic.

---

## Problem Statement

OPD appointment workflows in mid-sized hospitals are inefficient because they rely on:
- Physical presence and manual tokens
- Guesswork for waiting times
- Fixed time-slot scheduling that breaks in real conditions
- Heavy manual coordination by front-desk staff

As a result:
- Patients crowd waiting areas unnecessarily
- Scheduling coordinators face constant stress
- Waiting times become unpredictable
- Trust in the system erodes

---

## Persona in Scope

**Scheduling & Appointment Coordinators**

These are non-clinical staff responsible for:
- Managing OPD appointment requests
- Handling walk-ins and follow-ups
- Coordinating patient queues
- Communicating waiting status to patients

They do **not** control doctors or medical priority — they coordinate **flow under uncertainty**.

---

## Scope Clarification

This solution covers **only OPD consultation appointments**.

### Explicitly OUT of Scope
- Inpatient admission
- Bed allocation
- Ward management
- Clinical decision-making

The workflow starts when a patient requests to see a doctor and ends when the patient enters the consultation room.

---

## Core Idea of the Solution

Instead of enforcing rigid appointment times, the system introduces:

1. A **digital OPD queue** (position-based, not time-based)
2. A **rolling buffer of 3–4 active patients**
3. **Event-driven automation** using n8n
4. Predictive analytics to reduce uncertainty (advisory only)

The buffer absorbs real-world chaos such as:
- Long or short consultations
- No-shows
- Emergency interruptions

---

## How the Redesigned Workflow Works

### 1. Appointment Request
- Patient (new or existing) requests an OPD visit
- Request is added to a doctor-day–specific digital queue
- No fixed consultation time is promised

### 2. Digital Queue
- Queue is position-based
- Patients can see how many people are ahead
- Coordinators no longer manage physical tokens

### 3. Buffer-Based Stabilization
- Only the next **3–4 patients** are placed in an **active buffer**
- Buffer patients receive alerts to be ready
- Patients outside the buffer wait virtually

### 4. Event-Driven Queue Movement
- Queue advances when the doctor starts a consultation
- No-show patients are automatically skipped
- Emergency cases temporarily freeze or reorder the buffer

This ensures queue movement is driven by **real events**, not assumptions.

---

## Role of n8n in This Solution

**n8n is an open-source workflow automation platform** used here as an **orchestration layer**.

n8n:
- Listens to operational events (appointment request, consultation start, no-show, emergency)
- Applies queue and buffer logic
- Triggers notifications and updates
- Logs events for analytics

n8n does **not**:
- Store medical records
- Decide medical priority
- Replace hospital systems

It strictly coordinates **operational flow**.

---

## n8n Workflows Included

The repository includes modular n8n workflows representing key events:

- Appointment request → Queue creation
- Buffer assignment & shifting
- Consultation start → Queue advancement
- No-show handling
- Emergency override handling
- Analytics & event logging

These workflows are **event-driven and loosely coupled**, reflecting real OPD behavior.

---

