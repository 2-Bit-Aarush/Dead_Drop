# DEAD DROP

### Physical storage. Online, without the friction.

<p align="center">
  <img src="assets/dead-drop-3d.gif" width="800">
</p>

<p align="center">
  <strong>A compact ESP32-S3 based storage device designed to make physical storage feel like an online service.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Status-Prototype-orange?style=for-the-badge">
  <img src="https://img.shields.io/badge/PCB-2--Layer-black?style=for-the-badge">
  <img src="https://img.shields.io/badge/MCU-ESP32--S3-blue?style=for-the-badge">
  <img src="https://img.shields.io/badge/Storage-microSD-green?style=for-the-badge">
</p>

---

## What is Dead Drop?

**Dead Drop** is a custom-designed portable storage device built around an **ESP32-S3** and a removable microSD card.

The idea is simple:

> **Take something as physical and inconvenient as removable storage and give it the convenience of an online-first workflow.**

Instead of treating a storage device as nothing more than a USB drive, Dead Drop is designed as a small connected hardware platform that can interact with stored data and provide information through its own interface.

The project combines:

* removable storage
* Wi-Fi connectivity
* a small OLED interface
* USB connectivity
* dedicated power-management circuitry
* a custom compact PCB

The entire hardware platform was designed from the ground up rather than assembled from development boards.

---

# The Hardware

<p align="center">
  <img src="assets/dead-drop-render.png" width="850">
</p>

Dead Drop uses a custom **2-layer FR-4 PCB** measuring approximately:

**92.33 × 41.15 mm**

The board was intentionally kept compact so the finished device can approach the physical form factor expected from a portable storage device rather than looking like a conventional development board.

### Core hardware

| Component                  | Purpose                                 |
| -------------------------- | --------------------------------------- |
| **ESP32-S3-WROOM-1-N16R8** | Main processing + wireless connectivity |
| **MicroSD card**           | Removable local storage                 |
| **128×32 OLED**            | Local status/interface display          |
| **USB-A**                  | Physical host/device connectivity       |
| **USB-C**                  | Power / connectivity                    |
| **TPS2116DRLR**            | Power-path management                   |
| **AP63203QWU-7**           | DC-DC power conversion                  |
| **Tactile switch**         | User input                              |
| **3.3 µH inductor**        | Power regulation                        |
| **Decoupling capacitors**  | Power integrity                         |
| **Custom PCB**             | Integrates the entire system            |

---

# Why Build a Custom PCB?

The first instinct for a project like this would be to combine:

```text
ESP32 Dev Board
       +
SD Card Module
       +
OLED Module
       +
USB Module
       +
Power Module
```

That works for a prototype.

It doesn't make a good product.

Dead Drop instead puts the important circuitry onto a **single compact board**.

### The result

```text
┌─────────────────────────────────────────────┐
│                                             │
│   STORAGE        POWER       USER INTERFACE │
│      │              │               │       │
│      ▼              ▼               ▼       │
│   microSD ───── ESP32-S3 ─────── OLED       │
│                   │                         │
│                   ▼                         │
│              Wi-Fi / USB                    │
│                                             │
└─────────────────────────────────────────────┘
```

This reduces:

* wiring
* board count
* physical size
* unnecessary connectors
* assembly complexity

and makes the device much closer to an actual piece of hardware.

---

# PCB Architecture

The board was designed around several functional zones.

### 01 — Storage

A dedicated microSD interface provides removable local storage.

The storage is intentionally kept physically accessible so the device can use standard removable media rather than locking the project to a proprietary memory solution.

### 02 — Processing

At the center of the system is the **ESP32-S3-WROOM-1-N16R8**.

It provides:

* processing
* Wi-Fi
* Bluetooth capabilities
* USB functionality
* communication with the storage subsystem
* communication with the OLED

### 03 — Power

The board includes dedicated power-management circuitry rather than relying on an external regulator module.

The power subsystem uses:

* **TPS2116DRLR**
* **AP63203QWU-7**
* dedicated inductive/capacitive filtering

This keeps power conversion and distribution directly on the PCB.

### 04 — User Interface

A small **128×32 OLED** provides local feedback without requiring the device to be connected to another screen.

A physical tactile switch provides direct user input.

### 05 — Connectivity

Dead Drop exposes both:

* **USB-A**
* **USB-C**

This gives the hardware multiple physical interaction points while retaining wireless connectivity through the ESP32-S3.

---

# PCB Design

The PCB was designed in **EasyEDA**.

The design process included:

```text
Schematic
   ↓
Component Selection
   ↓
PCB Placement
   ↓
Power Routing
   ↓
Signal Routing
   ↓
Ground / Return Paths
   ↓
Silkscreen + Branding
   ↓
DRC
   ↓
Gerber / BOM / CPL
   ↓
Manufacturing
```

The board uses a **2-layer stackup** with:

* FR-4
* 1.6 mm thickness
* 1 oz copper
* black solder mask
* white silkscreen

The final design was checked with EasyEDA's DRC and reached **0 reported DRC errors** before manufacturing preparation.

---

# Design Philosophy

Dead Drop isn't meant to be another oversized development-board project.

The physical design follows three principles:

### Small

Every millimeter matters.

The board was kept around **92 × 41 mm**, with the layout arranged around the actual physical constraints of the connectors, display, storage socket and ESP32 module.

### Integrated

Modules were replaced with circuitry integrated directly onto the PCB wherever practical.

### Deliberate

The board isn't just a collection of components that happen to work.

Component placement, routing, connector positioning and the external board shape are all part of the product design.

---

# Manufacturing Files

The repository contains the files required to reproduce the PCB.

```text
manufacturing/
├── gerbers/
├── bom/
└── cpl/
```

### Gerbers

Contains the fabrication layers required by a PCB manufacturer.

### BOM

The Bill of Materials contains the components used on the final design, including manufacturer and supplier information where available.

### CPL

The Component Placement List / Pick-and-Place file provides placement information required for automated assembly.

---

# Bill of Materials

The final board contains **19 detected component groups** in the assembly BOM.

Some of the notable parts include:

| Part                   | Manufacturer / Source |
| ---------------------- | --------------------- |
| ESP32-S3-WROOM-1-N16R8 | Espressif             |
| TPS2116DRLR            | Texas Instruments     |
| AP63203QWU-7           | Diodes Inc.           |
| HS91L02W2C01           | HS                    |
| 472192001              | Molex                 |
| 917-181A102ED60200     | USB-A connector       |
| TYPE-C-31-M-12         | USB-C connector       |

The passive components use compact **0402 / 0603 / 0805** footprints where appropriate to keep the board small.

---

# The Manufacturing Challenge

One of the less visible parts of building custom hardware is getting from:

> **"It works on my desk."**

to:

> **"Someone can actually manufacture this."**

Dead Drop therefore went through the full manufacturing-data workflow:

```text
EasyEDA PCB
     │
     ├── Gerber
     │
     ├── BOM
     │
     └── CPL
          │
          ▼
      PCBA Quote
          │
          ▼
    Component Matching
          │
          ▼
      Assembly
```

The BOM was prepared with supplier/manufacturer information to make automated component sourcing and assembly possible.

---

# Design Highlights

### Compact form factor

Approximately:

**92.33 × 41.15 mm**

### Wireless

ESP32-S3 provides the wireless foundation.

### Removable storage

Standard microSD storage keeps the storage medium replaceable.

### Local display

A dedicated OLED allows the device to communicate status without another computer.

### Dual USB interfaces

USB-A and USB-C provide physical connectivity options.

### Custom power management

Power conversion and power-path management are integrated into the PCB.

### Manufacturing-ready output

Gerber + BOM + CPL files are generated for PCB assembly.

---

# Project Status

### Hardware

* [x] Schematic
* [x] Component selection
* [x] PCB layout
* [x] Routing
* [x] Board outline
* [x] Branding / silkscreen
* [x] DRC — 0 errors
* [x] Gerber generation
* [x] BOM generation
* [x] CPL generation
* [ ] PCB fabrication
* [ ] PCBA
* [ ] Hardware bring-up
* [ ] Firmware integration
* [ ] Final enclosure

---

# Repository Structure

```text
dead-drop/
│
├── README.md
│
├── hardware/
│   ├── schematic/
│   ├── pcb/
│   └── libraries/
│
├── manufacturing/
│   ├── gerbers/
│   ├── bom/
│   └── cpl/
│
├── assets/
│   ├── dead-drop-3d.gif
│   ├── dead-drop-render.png
│   └── logo/
│
├── firmware/
│
└── docs/
```

---

# What's Next?

The PCB is only the first half of Dead Drop.

The next stage is turning the manufactured board into a functional device.

Planned work includes:

* ESP32-S3 firmware
* storage management
* wireless file access
* OLED interface
* device configuration
* USB functionality
* data transfer workflows
* enclosure design
* hardware testing
* power-consumption optimization

The goal is to eventually move from:

**custom PCB → functional prototype → finished portable device**

---

# Dead Drop

> **Not another development board.**
>
> **A storage device built like a product.**

---

<p align="center">
  <strong>DEAD DROP</strong><br>
  <sub>Physical in your hand. Online when you need it.</sub>
</p>

<p align="center">
  Built by <strong>Aarush Sharma</strong>
</p>
