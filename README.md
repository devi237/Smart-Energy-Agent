AI Smart Energy Management System

A utility-based intelligent agent that controls HVAC, lighting, and standby devices in a multi-room building to minimize electricity consumption while maintaining occupant comfort.

Author: Devi Parvathy K, Group 8, S7 CSE-A

Overview

Buildings waste significant electricity by running HVAC and lighting in unoccupied rooms, or keeping artificial lights at full brightness even when natural daylight is sufficient. This project implements an intelligent agent that monitors room occupancy, indoor/outdoor temperature, and light levels, and automatically makes energy-efficient control decisions — without compromising comfort.

A 24-hour simulation compares the agent's energy usage against a baseline "dumb" building (HVAC always on, lights always at 100%) to quantify the savings.

Agent Design (PEAS)
Element	Description
Performance Measure	Minimize electricity consumption, maintain occupant comfort, reduce unnecessary energy usage, maximize energy savings
Environment	Smart building rooms, occupants, HVAC systems, lighting systems
Actuators	HVAC controller, smart lights/dimmer, smart plugs
Sensors	Occupancy sensor, indoor temperature sensor, light sensor, outdoor temperature feed, appliance power meter

Agent type: Utility-based, model-based agent Utility function: U = -energy_cost - comfort_penalty

Environment Properties
Property	Type
Observability	Partially Observable (noisy sensors)
Determinism	Stochastic (random occupancy/weather shocks)
Dynamics	Dynamic (state evolves independently of agent)
Discreteness	Continuous (temperature, light, energy are continuous values)
Agents	Single Agent
Knowledge	Partially Known
How It Works
Room — models a single room's temperature, occupancy, light level, and comfort settings; produces noisy sensor readings.
BuildingEnvironment — holds all rooms and evolves them each hour: updates outdoor temperature along a daily curve, randomizes occupancy, drifts room temperature toward outdoor temperature, applies the previous HVAC action, and recalculates daylight.
SmartEnergyAgent — the decision-maker:
decide_hvac() — turns HVAC off in unoccupied rooms; pre-emptively cools/heats occupied rooms before they drift outside the comfort band.
decide_light() — daylight harvesting: dims/switches off lights based on occupancy and available natural light.
act() — applies decisions, tracks energy used, computes the baseline comparison, and logs comfort violations.
run_simulation() — runs a 24-hour simulation across four rooms (Living Room, Bedroom, Office, Kitchen), printing hourly state and a final summary.
Innovations
Occupancy-based energy control — HVAC and lighting switch off automatically in empty rooms.
Daylight harvesting — artificial lighting is dimmed based on available natural light.
Predictive/pre-emptive HVAC control — HVAC activates just before comfort limits are breached, avoiding constant cycling.
Requirements
Python 3.x (standard library only — no external dependencies)
Usage
bash
python smart_energy_agent.py

This runs a 24-hour simulation and prints:

An hourly table of outdoor temperature, per-room temperature/occupancy/HVAC state, and energy used (agent vs. baseline).
A final summary of total energy consumed, energy saved, and comfort satisfaction rate.
Sample Output
SIMULATION SUMMARY (24-hour cycle)
============================================================
Agent total consumption   :    46.26 kWh
Baseline (no AI) consumption:   153.60 kWh
Energy saved              :   107.34 kWh  ( 69.9% reduction)
Comfort violations        : 15 (out of 96 room-hours)
Comfort satisfaction rate :  84.4%
============================================================

In this run, the intelligent agent achieved a 69.9% reduction in energy consumption compared to the baseline, while maintaining an 84.4% comfort satisfaction rate across 96 room-hours.

Project Structure
.
├── smart_energy_agent.py   # Full implementation (environment, agent, simulation)
└── README.md                # This file
Conclusion

The AI Smart Energy Management System demonstrates that a relatively simple, rule-based utility agent — reasoning over occupancy, temperature, and light — can deliver substantial energy savings in a building while keeping occupants comfortable. The simulation framework makes the tradeoff between energy savings and comfort measurable and tunable (e.g., via the comfort band).