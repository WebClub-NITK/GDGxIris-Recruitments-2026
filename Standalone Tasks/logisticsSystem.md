# Task ID: Logistics System

`Agentic-AI`

**Mentor:** Aditi Sinha ([9328392381](tel:9328392381))

**Difficulty:** Medium

---

## Overview
 
Build an autonomous AI logistics system with a **multi-agent architecture** (Root/Orchestrator Agent + specialized agents) that can monitor a simulated delivery network — orders, drivers, warehouses, routes, deadlines, priorities, real-time events — and observe, reason, act, and monitor. The focus is the agentic decision loop, not just tracking or route optimization.

## Required Features

### 1. Multi-Agent Logistics System

Build a multi-agent system consisting of a **Root/Orchestrator Agent** and specialized agents.

The Root Agent should:

* Coordinate the overall workflow
* Understand incoming events
* Coordinate the actions of multiple agents

Specialized agents can be responsible for tasks such as:

* Order and driver assignment
* Route planning
* Monitoring and event handling

### 2. Order, Driver Assignment & Dashboard

The system should be able to:

* Assign orders to suitable drivers
* Consider order priority and delivery deadlines
* Reassign orders when a driver becomes unavailable
* Handle new or urgent orders

The system should also provide a **Driver Dashboard** showing relevant information such as:

* Assigned orders
* Delivery status
* Driver availability/status

### 3. Dynamic Route Planning

The system should be able to find best route for delivery and consider delivery deadlines

*Routing and optimization tools such as **Google Maps or Google OR-Tools** may be used where appropriate.*

### 4. Real-Time Incoming Events

The system must be able to receive and process **real-time events** from the simulated logistics environment.

Events may include changes to:

* Driver status
* New Orders

*Events should be handled based on their priority.*

The agent should determine the impact of incoming events and take appropriate actions.

The expected workflow is:

**Observe → Reason → Delegate → Act → Monitor → Replan (if necessary)**

## Bonus Features

* **Road and traffic condition handling** — Detect changes in road or traffic conditions and adapt delivery routes accordingly.
* **Failure recovery** — Handle failed tool calls or unexpected situations and recover automatically.

## Deliverables

### Source Code

* Complete source code
* Public GitHub repository
* Proper `.gitignore` file
* Clear project structure
* Code comments for complex agentic logic

### Documentation

README.md including:

* Problem overview
* System architecture
* Agent workflow
* Setup instructions
* Dependencies
* Environment variables/configuration
* How to run the project
* Screenshots of the application

### Demo Video (Recommended)

3–5 minute screen recording demonstrating:

* Initial logistics environment
* Agent assigning orders
* A real-time disruption
* Agent responding to the disruption
* Route/order changes

### Deployment / Demo Link

* Deployed application or
* Local runnable application with clear setup instructions

All the best! 
