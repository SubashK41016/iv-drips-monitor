# SMART IV DRIP MONITORING SYSTEM USING IoT

**Project Report**

---

## 1. ABSTRACT

The **Smart IV Drip Monitoring System** is an IoT-based healthcare solution designed to monitor intravenous (IV) fluid levels and drip rates in real time. In hospitals, nurses manually check IV bottles, which can lead to delays, human errors, and patient risk if the fluid finishes unexpectedly.

This system uses sensors connected to a microcontroller (ESP8266) to continuously monitor fluid levels and drip rates. The data is displayed on a **real-time web dashboard** accessible from any browser or Android device. The dashboard supports **Firebase Authentication** for secure access, **multi-patient monitoring** with the ability to add and edit patient records, **automatic alerts** when fluid levels or drip rates breach safe thresholds, and **live trend charts** for clinical analysis.

The system improves patient safety, reduces nurse workload, and enables centralized monitoring in hospital wards.

---

## 2. INTRODUCTION

Intravenous (IV) therapy is commonly used in hospitals for delivering fluids, medications, and nutrients to patients. Traditional IV monitoring requires manual supervision, which is inefficient and error-prone.

With advancements in IoT and real-time web technologies, it is possible to automate IV monitoring using sensors and cloud-based dashboards.

This project integrates:

- **IoT Hardware** — ESP8266 microcontroller with sensors
- **Frontend Dashboard** — React + Vite (responsive web app)
- **Authentication** — Firebase Auth (Email/Password)
- **Real-Time Simulation** — Live data updates every 2 seconds
- **Android App** — Capacitor-based hybrid mobile application
- **Cloud Deployment** — Vercel (free hosting)

---

## 3. PROBLEM STATEMENT

In hospitals:

- Nurses must manually monitor IV bottles across multiple patients.
- If IV fluid finishes unnoticed, air can enter the bloodstream (air embolism).
- Drip rate inconsistencies may go undetected, affecting medication dosage.
- Manual tracking increases workload, stress, and risk of human error.
- There is no centralized view of all patients' IV status.

**There is a need for an automated, real-time IV monitoring system that reduces risks, improves efficiency, and provides centralized monitoring.**

---

## 4. OBJECTIVES

1. To monitor IV fluid levels in real time.
2. To track drip rate continuously for each patient.
3. To generate automatic alerts when fluid level is critically low or drip rate is abnormal.
4. To provide a centralized monitoring dashboard for multiple patients.
5. To allow adding new patients and editing existing patient records.
6. To display the saline/fluid type used for each patient.
7. To implement secure authentication for hospital staff.
8. To support mobile access via Android application.
9. To reduce manual supervision workload.
10. To improve overall patient safety.

---

## 5. SYSTEM ARCHITECTURE

### Architecture Overview

```
┌──────────────┐     ┌──────────────┐     ┌──────────────────────┐
│  IV Sensor   │────▶│   ESP8266    │────▶│   Firebase / Cloud  │
│  (Load Cell) │     │  (WiFi MCU)  │     │   (Authentication)   │
└──────────────┘     └──────────────┘     └──────────┬───────────┘
                                                      │
                                          ┌───────────▼──────────┐
                                          │   React Dashboard    │
                                          │  (Web + Android App) │
                                          │                      │
                                          │  ┌─ Patient Cards    │
                                          │  ├─ Live Charts      │
                                          │  ├─ Alert Panel      │
                                          │  ├─ Add/Edit Patient │
                                          │  └─ Saline Display   │
                                          └──────────────────────┘
```

### Hardware Layer
- ESP8266 Microcontroller (WiFi-enabled)
- Load Cell / Flow Sensor
- HX711 Amplifier Module

### Frontend Layer (Dashboard)
- React 19 with Vite 7
- React Router v7 (SPA routing)
- Recharts (data visualization)
- Lucide React (icon library)
- Vanilla CSS (premium dark theme)

### Authentication Layer
- Firebase Authentication (Email/Password)
- Demo mode fallback for presentations

### Mobile Layer
- Capacitor (hybrid Android app)
- WebView with native shell
- Package: `com.sk.ivdripmonitor`

### Deployment
- Vercel (production web hosting)
- Firebase Hosting (optional)

---

## 6. WORKING PRINCIPLE

1. The sensor measures IV fluid level (weight) and calculates drip rate.
2. ESP8266 reads sensor data and sends it via WiFi to the cloud/backend.
3. The dashboard receives data and renders Patient Cards with live values.
4. **Data updates every 2 seconds** in the simulation mode.
5. Color-coded status indicators change dynamically:
   - 🟢 **NORMAL** — Fluid > 80ml, Drip Rate > 20 drops/min
   - 🟡 **WARNING** — Fluid < 80ml or Drip Rate < 20 drops/min
   - 🔴 **CRITICAL** — Fluid < 30ml or Drip Rate < 10 drops/min
6. **Alerts are auto-generated** when status transitions to WARNING or CRITICAL.
7. Staff can **add new patients** or **edit existing records** via modal forms.

---

## 7. FEATURES IMPLEMENTED

### 7.1 Authentication System
| Feature | Description |
|---------|-------------|
| **Sign In** | Email/password login with Firebase Auth |
| **Register** | New account creation with display name |
| **Demo Mode** | One-click access without Firebase for presentations |
| **Protected Routes** | Dashboard only accessible to authenticated users |
| **Session Management** | Auto-login via Firebase `onAuthStateChanged` |
| **Logout** | Secure sign-out with session cleanup |

### 7.2 Dashboard — Summary Cards
| Card | Data Shown |
|------|------------|
| **Total Patients** | Count of all monitored patients |
| **Active IVs** | Number of patients with active IV drips |
| **Critical Alerts** | Count of patients in critical status |
| **Avg Drip Rate** | Average drip rate across all patients |

### 7.3 Patient Monitoring Cards
Each patient card displays:
- **Patient Name** and **ID**
- **Bed Number** and **Ward**
- **Age**
- **Saline/Fluid Type** (prominent blue badge)
- **Fluid Level** — animated progress bar with color coding
- **Drip Rate** — numerical display with trend arrow (↑ or ↓)
- **Status Badge** — NORMAL / WARNING / CRITICAL
- **IV Start Time**
- **Edit Button** — pencil icon (appears on hover)

### 7.4 Add Patient
- Modal form with fields: Name, Age, Bed Number, Ward, Saline Type, Fluid Volume, Drip Rate
- **8 Saline Types**: Normal Saline 0.9%, Dextrose 5%, Ringer Lactate, Dextrose 10%, Half Normal Saline 0.45%, DNS, Mannitol 20%, Hypertonic Saline 3%
- **6 Ward Options**: General, ICU, Surgical, Emergency, Pediatric, Maternity
- New patient card appears instantly with live monitoring

### 7.5 Edit Patient
- Hover over any patient card → click pencil icon
- Edit: Name, Age, Bed Number, Ward, Saline Type, Max Fluid Volume
- Changes save immediately without page refresh

### 7.6 Live Trend Charts (Recharts)
- Click any patient card to view detailed history
- **Fluid Level Chart** — Area chart showing fluid drainage over time
- **Drip Rate Chart** — Line chart showing rate fluctuations
- Dark-themed tooltips with real-time data points

### 7.7 Alert System
- **Auto-generated** when thresholds are crossed
- **Color-coded**: Red for Critical, Yellow for Warning
- Shows: Patient Name, Bed Number, Alert Message, Timestamp
- Scrollable list with latest alerts at top (max 50)
- Pulsing animation on critical patient cards

### 7.8 Real-Time Simulation (Demo Mode)
- **6 pre-loaded sample patients** with realistic Indian names
- Fluid decreases every 2 seconds (simulating drainage)
- Drip rate varies slightly (simulating real fluctuations)
- Auto-refill when fluid reaches 0 (simulating bottle replacement)
- Alerts trigger automatically on status transitions

### 7.9 Android Mobile App
- Built using **Capacitor** (hybrid framework)
- Package: `com.sk.ivdripmonitor`
- Same dashboard experience on mobile
- Installable APK for Android devices

---

## 8. TECHNOLOGIES USED

| Layer | Technology | Version |
|-------|-----------|---------|
| Frontend Framework | React | 19 |
| Build Tool | Vite | 7 |
| Routing | React Router | 7 |
| Authentication | Firebase Auth | 11 |
| Charts | Recharts | 2.x |
| Icons | Lucide React | Latest |
| Styling | Vanilla CSS | — |
| Mobile | Capacitor | 7 |
| Deployment | Vercel | — |
| Hardware | ESP8266 | — |
| Sensor | HX711 + Load Cell | — |

---

## 9. SOFTWARE REQUIREMENTS

| Software | Purpose |
|----------|---------|
| Node.js (v18+) | JavaScript runtime |
| npm | Package manager |
| VS Code | Code editor |
| Vite | Development server & bundler |
| Firebase Console | Authentication setup |
| Android Studio | APK building |
| Git | Version control |
| Browser (Chrome) | Testing |

---

## 10. HARDWARE REQUIREMENTS

| Hardware | Purpose |
|----------|---------|
| ESP8266 (NodeMCU) | WiFi-enabled microcontroller |
| Load Cell (5kg) | Measures IV bottle weight |
| HX711 Module | Amplifier for load cell signals |
| Flow Sensor | Measures drip rate |
| IV Drip Setup | Physical IV bottle and tube |
| Jumper Wires | Circuit connections |
| Power Supply (5V/3.3V) | Powers ESP8266 |
| WiFi Router | Internet connectivity |

---

## 11. MODULE DESCRIPTION

### Module 1: Authentication Module
- Firebase Email/Password sign-in and registration
- Demo mode bypass for presentations
- Protected route guard for dashboard access
- Session persistence via `onAuthStateChanged`

### Module 2: Patient Monitoring Module
- Displays 6+ patient cards in responsive grid
- Each card shows: fluid level, drip rate, status, saline type
- Color-coded progress bars with animated shimmer effect
- Trend arrows indicate drip rate direction

### Module 3: Patient Management Module
- **Add Patient**: Modal form with validation (name, age, bed, ward, saline, volume, rate)
- **Edit Patient**: Click pencil icon → edit any field → save instantly
- Supports 8 saline types and 6 ward categories

### Module 4: Alert Module
- Automatic threshold-based alert generation
- Critical alerts: fluid < 30ml OR drip rate < 10 drops/min
- Warning alerts: fluid < 80ml OR drip rate < 20 drops/min
- Color-coded alert list with timestamps
- Pulsing glow animation on critical cards

### Module 5: Data Visualization Module
- Interactive Recharts-based charts
- Fluid Level: Area chart with gradient fill
- Drip Rate: Line chart with smooth interpolation
- Dark-themed, responsive charts
- Click patient card to select and view charts

### Module 6: Simulation Module
- Demo data engine with 6 sample patients
- 2-second data refresh interval
- Realistic fluid drainage simulation
- Automatic alert generation on threshold crossing
- Auto-refill when fluid reaches zero

### Module 7: Mobile App Module
- Capacitor hybrid framework
- Android WebView wrapper with native shell
- Dark themed splash/background
- HTTPS scheme for secure content loading

---

## 12. UI/UX DESIGN

### Design Principles
- **Dark Mode** — Reduces eye strain in hospital environments
- **Glassmorphism** — Modern frosted-glass card effects
- **Color-Coded Status** — Instant visual triage (Green/Yellow/Red)
- **Micro-Animations** — Shimmer on fluid bars, pulse on critical alerts, slide-in cards
- **Responsive Layout** — Works on desktop, tablet, and mobile
- **Inter Font** — Clean, modern medical typography

### Color Palette
| Element | Color |
|---------|-------|
| Background | `#0a0e1a` (Deep Navy) |
| Card Glass | `rgba(255,255,255,0.04)` |
| Accent Blue | `#00d4ff` |
| Accent Teal | `#2dd4bf` |
| Normal (Green) | `#22c55e` |
| Warning (Amber) | `#f59e0b` |
| Critical (Red) | `#ef4444` |

---

## 13. SCREENSHOTS

### Login Page
- Glassmorphism card with animated gradient background orbs
- Sign In / Register tabs
- Email & Password fields with icons
- "Try Demo Mode" button for instant access

### Dashboard
- Header with logo, LIVE indicator, Add Patient button, Logout
- 4 Summary cards (Total Patients, Active IVs, Critical Alerts, Avg Drip Rate)
- Patient grid with 6+ cards showing live data
- Side panel with Charts and Alerts

---

## 14. DEPLOYMENT

### Web Deployment (Vercel)
- **Live URL**: https://iv-drips-monitor.vercel.app
- Free hosting with automatic SSL
- Zero-config deployment via Vercel CLI

### Android App (Capacitor)
- Package: `com.sk.ivdripmonitor`
- Built via Android Studio
- APK output: `android/app/build/outputs/apk/debug/app-debug.apk`

---

## 15. ADVANTAGES

1. ✅ Real-time monitoring reduces nurse workload
2. ✅ Prevents IV empty risk (air embolism danger)
3. ✅ Color-coded alerts enable instant triage
4. ✅ Centralized dashboard for all patients
5. ✅ Scalable — supports unlimited patients
6. ✅ Cost-effective IoT solution
7. ✅ Secure login with Firebase Authentication
8. ✅ Add/Edit patient records dynamically
9. ✅ Mobile app for on-the-go monitoring
10. ✅ Works on any device with a browser

---

## 16. LIMITATIONS

1. Requires stable internet/WiFi connection
2. Sensor calibration required for accurate readings
3. Initial hardware setup cost
4. Demo mode uses simulated data (not real sensors)
5. WebView-based Android app (not fully native)

---

## 17. FUTURE ENHANCEMENTS

1. SMS / Email alerts for critical events
2. Push notifications on Android app
3. AI-based anomaly detection for drip rate
4. Cloud deployment on AWS / Firebase Hosting
5. Hospital database integration (MySQL / MongoDB)
6. RFID patient identification
7. Multiple hospital/ward support
8. Nurse assignment and shift management
9. Historical data analytics and reports
10. Bluetooth Low Energy (BLE) connection to ESP8266

---

## 18. APPLICATIONS

1. Government Hospitals
2. Private Clinics
3. ICU Monitoring Units
4. Home Healthcare Systems
5. Emergency Care Units
6. Pediatric Wards
7. Post-Surgical Recovery Rooms
8. Rural Health Centers

---

## 19. RESULT

The system successfully demonstrates:

- ✅ Real-time IV fluid monitoring with live-updating patient cards
- ✅ Dynamic dashboard with summary statistics
- ✅ Automatic alert generation for critical and warning conditions
- ✅ Multi-patient support with add/edit functionality
- ✅ Saline/fluid type display for each patient
- ✅ Interactive trend charts for clinical analysis
- ✅ Secure Firebase authentication with demo mode
- ✅ Responsive web dashboard deployed to Vercel
- ✅ Android APK-ready via Capacitor

The implementation proves that IoT combined with modern web technologies can significantly improve hospital patient monitoring systems.

---

## 20. CONCLUSION

The Smart IV Drip Monitoring System effectively addresses the problem of manual IV supervision in hospitals. By integrating IoT hardware (ESP8266 + sensors) with real-time web technologies (React + Firebase), the system ensures patient safety and reduces workload on medical staff.

The dashboard provides a **centralized, real-time view** of all patients' IV statuses with **automatic alert generation**, **patient management** (add/edit), and **mobile access** via Android app. The project demonstrates how technology can enhance healthcare infrastructure and pave the way for smarter hospital systems.

---

## 21. REFERENCES

1. Firebase Documentation — https://firebase.google.com/docs
2. React Official Documentation — https://react.dev
3. Capacitor Documentation — https://capacitorjs.com/docs
4. Vite Build Tool — https://vite.dev
5. Recharts Library — https://recharts.org
6. ESP8266 Technical Reference — https://docs.espressif.com
7. IoT in Healthcare — IEEE Research Papers
8. Node.js Documentation — https://nodejs.org/docs

---

## PROJECT DETAILS

| Field | Value |
|-------|-------|
| **Project Title** | Smart IV Drip Monitoring System Using IoT |
| **Technology Stack** | React + Vite + Firebase + Capacitor + ESP8266 |
| **Category** | IoT + Healthcare |
| **Type** | Mini / Major Project |
| **Live URL** | https://iv-drips-monitor.vercel.app |
| **Android Package** | com.sk.ivdripmonitor |
