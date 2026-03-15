<h1 align="center">LoRa Mesh Tracking System</h1>

<p align="center">
  A custom low-power long-range mesh communication system for position reporting and resilient data forwarding in infrastructure-limited environments.
</p>

<p align="center">
  <img src="docs/images/system-overview.png" alt="System Overview" width="85%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-ESP32%20%7C%20RP2040%20%7C%20Raspberry%20Pi-blue" />
  <img src="https://img.shields.io/badge/Radio-LoRa%20SX1262-green" />
  <img src="https://img.shields.io/badge/GNSS-Enabled-orange" />
  <img src="https://img.shields.io/badge/Topology-Custom%20Mesh-purple" />
  <img src="https://img.shields.io/badge/Status-Research%20%26%20Prototype-success" />
</p>

---

## Overview

This project investigates the design and implementation of a **custom LoRa-based mesh network** for **tracking and position reporting** in areas where traditional communication infrastructure is unavailable, unreliable, or impractical.

The system is built around the idea that position data should still be able to reach a gateway even when direct communication is weak or blocked. Instead of relying only on point-to-point transmission, intermediate nodes can forward packets through a lightweight mesh strategy, improving delivery reliability and extending effective coverage.

The project combines **embedded hardware integration**, **LoRa physical-layer tuning**, **packet forwarding logic**, and **GNSS-based payload transmission** into a single experimental system. The main goal is to evaluate how well a small custom mesh can improve robustness compared with simple direct transmission or blind flooding approaches.

---

## Project Objectives

- Design a practical **LoRa tracking system** for low-power and long-range deployment
- Build a **custom mesh forwarding mechanism** for multi-node communication
- Integrate **GNSS location data** into node payloads
- Evaluate performance using metrics such as:
  - RSSI versus distance
  - SNR versus distance
  - Packet delivery ratio
  - Packet loss ratio
  - End-to-end latency
  - Network behaviour as node count increases
- Compare the proposed communication logic against simpler alternatives such as:
  - Point-to-point LoRa
  - Relay-based forwarding
  - Blind flooding mesh

---

## Why LoRa Mesh

LoRa is attractive for tracking applications because it offers:

- **Long communication range**
- **Low power consumption**
- **Low hardware complexity**
- **Operation without cellular or Wi-Fi infrastructure**

However, direct LoRa links are not always reliable in obstructed or extended-range environments. A mesh or relay-assisted approach can improve communication resilience by allowing packets to travel through neighbouring nodes instead of requiring a single strong end-to-end path.

This project explores that trade-off between **simplicity**, **coverage**, **latency**, and **reliability**.

---

## System Architecture

The system is organised into three main elements:

### 1. End Nodes
Each end node collects or stores local data and prepares packets containing:
- Node ID
- Timestamp
- GNSS coordinates
- Sequence number
- Forwarding metadata

### 2. Relay / Mesh Nodes
Intermediate nodes listen for packets, validate them, and decide whether to forward them based on custom forwarding logic such as:
- packet integrity
- rank or hop condition
- link quality indicators
- duplicate suppression

### 3. Gateway
The gateway receives the final packet, acknowledges successful reception where required, and passes useful information to a higher-level interface for logging, display, or mapping.

<p align="center">
  <img src="docs/images/block-diagram.png" alt="Block Diagram" width="80%">
</p>

---

## Hardware Platform

The prototype system is based on a combination of low-cost embedded boards and LoRa radio modules.

### Example hardware used
- **ESP32-S3** development board
- **Raspberry Pi Pico 2W**
- **Raspberry Pi 4** as gateway/controller
- **SX1262-based LoRa modules**
- **GNSS receiver module**
- Antennas, power regulation, and interface wiring

The exact node role can vary depending on the experiment:
- end node
- relay node
- gateway node

---

## Communication Method

The network does not rely on full LoRaWAN infrastructure. Instead, it uses a **custom lightweight communication framework** designed for small-scale experimental mesh operation.

A typical packet flow is:

1. A source node generates or updates location data
2. The packet is transmitted over LoRa
3. A receiving node checks CRC and packet validity
4. If forwarding conditions are satisfied, the node retransmits the packet
5. The gateway receives the final version
6. An acknowledgement can be returned to confirm successful delivery

This allows the system to test how forwarding logic interacts with physical-layer settings such as:
- Spreading Factor (SF)
- Bandwidth (BW)
- Coding Rate (CR)
- Transmit power
- Preamble length

---

## Forwarding Logic

The mesh behaviour is designed to be more selective than simple blind flooding.

Typical forwarding decisions can include:
- discard corrupted packets
- reject duplicates
- prefer better-ranked forwarding opportunities
- reduce unnecessary retransmissions
- improve network scalability as node count increases

This helps limit the rapid performance collapse that often appears in naive flooding systems as more nodes begin transmitting.

---

## Evaluation Metrics

The system is evaluated using both radio-level and network-level metrics.

### Radio metrics
- **RSSI**
- **SNR**
- **Link stability over range**

### Network metrics
- **Packet Delivery Ratio (PDR)**
- **Packet Loss Ratio (PLR)**
- **End-to-End Latency**
- **Forwarding efficiency**
- **Scalability with increasing node count**

Example relationships studied in this project include:
- PDR vs distance
- RSSI vs distance
- SNR vs distance
- latency vs node count
- custom mesh vs flooding performance comparison

---

## Example Results

The expected trend from the experiments is that both custom mesh and flooding may perform well at very small node counts, but as the network becomes denser:

- **blind flooding degrades quickly**
- **custom forwarding remains more stable**
- **latency grows more slowly**
- **packet delivery remains higher for longer**

<p align="center">
  <img src="docs/images/pdr-vs-distance.png" alt="PDR vs Distance" width="48%">
  <img src="docs/images/latency-vs-nodes.png" alt="Latency vs Nodes" width="48%">
</p>

---

## Repository Structure

```text
.
├── firmware/              # Node and gateway firmware
├── hardware/              # Schematics, wiring notes, pin maps
├── docs/                  # Images, diagrams, poster assets, logbook material
│   └── images/
├── experiments/           # Test results, measurements, datasets
├── scripts/               # Parsing, logging, plotting, utility scripts
└── README.md
