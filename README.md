# CPE11-AFCS — Automated Fare Collection System

![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi%204-A22846?style=flat&logo=raspberrypi&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![React](https://img.shields.io/badge/React%2018-61DAFB?style=flat&logo=react&logoColor=black)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=flat&logo=firebase&logoColor=black)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat&logo=linux&logoColor=black)

A Raspberry Pi 4–based automated fare collection system for modern jeepneys: NFC tap-in/tap-out, GPS-based fare computation, seat occupancy sensing, and automated gate control on the embedded side, synchronized in real time with **Commuter**, **Driver**, and **Admin** web apps over Firebase.

**Role:** Team Leader — led a 5-person multidisciplinary team through a modified V-Model with iterative prototyping (Dec 2025 – Apr 2026).

**Validation:** Functional, integration, system, expert, and user testing against ISO/IEC 25010:2023 quality characteristics — **4.56/5 expert score, 4.57/5 user score**.

## System Overview

| Layer | What it does |
|---|---|
| **Embedded firmware** (Raspberry Pi 4, Python) | Event-driven state machine governing boarding, destination selection, fare computation, seat reservation, and tap-in/tap-out. Integrates USB NFC (ACR1252U), UART GPS, SPI TFT display (ILI9488), PWM gate servos, and GPIO IR seat sensors. Timeout handling, duplicate-tap prevention, and fault recovery built in. |
| **Commuter app** (`src/`) | Passenger-facing: login + 2FA, card balance, live jeepney availability with seat counts, terminal selection, route extension. |
| **Driver app** (`4CPEA-11-DRIVER/`) | Operator-facing: passenger manifest, route/shift management, daily earnings view. |
| **Admin app** (`4CPEA-11-ADMIN/`) | Office-facing: fleet totals, trends, terminal management, reporting. |
| **Cloud** | Firebase Auth, Firestore, Cloud Functions (`functions/`, `cpe-11/`), Hosting. Firmware syncs via event-driven REST calls (not polling) to cut redundant writes. |

## Getting Started (web apps)

```bash
npm install
npm run dev        # http://localhost:3000
```

Build for production:

```bash
npm run build        # commuter app only → dist/
npm run build:all    # all web apps → dist-combined/ (for Firebase Hosting)
```

Install dependencies in each sub-app first if needed: root, `4CPEA-11-ADMIN`, `4CPEA-11-DRIVER`, `functions`, `cpe-11`.

Deploy: `firebase deploy` (see `HOSTINGER_DEPLOYMENT.md` for static hosting alternative).

Firebase config: copy `src/firebase/config.example.js` → `src/firebase/config.js` and fill in your project credentials.

## Embedded Firmware

The Raspberry Pi firmware source is included as [`firmware_src.txt`](firmware_src.txt) (hardware-dependent — requires the physical NFC reader, GPS receiver, TFT display, and sensor rig to run). It implements the state machine and hardware integration described above.

## Docs

- [`docs/SYSTEM_TUTORIAL.md`](docs/SYSTEM_TUTORIAL.md) — full guided walkthrough of the system (architecture, security, design rationale)
- [`HOSTINGER_DEPLOYMENT.md`](HOSTINGER_DEPLOYMENT.md) — static hosting deployment guide

## Tech Stack

Raspberry Pi 4 · Python · React 18 · React Router · Vite · Firebase (Auth, Firestore, Functions, Hosting) · NFC (ACR1252U/DESFire EV2) · GPS · UART/SPI/GPIO/PWM
