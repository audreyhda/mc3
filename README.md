# 🚂 Tracks

> **See the next 3 trains from your Apple Watch or iPhone widget — because in Naples, the station board is never on time.**

[![Swift](https://img.shields.io/badge/Swift-F54A2A?style=flat&logo=swift&logoColor=white)](https://swift.org/)
[![SwiftUI](https://img.shields.io/badge/SwiftUI-0D96F6?style=flat&logo=swift&logoColor=white)](https://developer.apple.com/xcode/swiftui/)
[![watchOS](https://img.shields.io/badge/watchOS-000000?style=flat&logo=apple&logoColor=white)](https://developer.apple.com/watchos/)
[![WidgetKit](https://img.shields.io/badge/WidgetKit-5856D6?style=flat&logo=apple&logoColor=white)](https://developer.apple.com/documentation/widgetkit)

---

## 📌 Problem Statement

If you've ever commuted by train in Naples, you know the problem: trains are frequently late, and the station departure boards often don't update in time — or at all. You're standing on the platform with no idea when the next train is actually coming. Checking the Trenitalia app means unlocking your phone, opening the app, navigating to your route... all while hoping you haven't missed it.

Tracks solves this with a **glanceable interface**: a complication on your Apple Watch or a widget on your home screen that shows the next 3 departures for your saved route — with real-time delay information from the Trenitalia API — without unlocking your phone.

---

## 🧩 Project Overview

Tracks was built as a **second group project** during the 2022/2023 programme at the [Apple Developer Academy](https://www.developeracademy.unina.it/en/) at the University of Naples Federico II, Italy, using the **Challenge Based Learning (CBL)** methodology.

The Academy brought together students from across Europe and beyond — which is reflected in the app's **trilingual support** (English, French, Italian), built by a genuinely international team.

The app fetches live train timetable data from the **Trenitalia API**, displays the next 3 departures for a saved route, and surfaces them on the Apple Watch and as an iOS home screen widget — so your timetable is always one glance away.

---

## 🧩 Project Context

| | |
|---|---|
| **Programme** | Apple Developer Academy — University of Naples Federico II |
| **Location** | Naples, Italy |
| **Year** | 2022 / 2023 |
| **Type** | Second group project |
| **Methodology** | Challenge Based Learning (CBL) |
| **Data source** | Trenitalia API |

---

## 🛠️ Tech Stack

### 🌐 Languages
- Swift 5

### 📦 Frameworks & Libraries
- SwiftUI — Declarative UI for iPhone and Watch
- watchOS — Native Apple Watch app target
- WidgetKit — iOS home screen widget
- WatchConnectivity — Data sync between iPhone app and Watch app
- URLSession + `async`/`await` — Trenitalia API calls

### 🌍 Localisation
- English (`en.lproj`)
- French (`fr.lproj`)
- Italian (`it.lproj`)

### 🔧 Tools
- Xcode — IDE (multi-target project: iOS + watchOS + Widget)
- Git / GitHub — Version control and group collaboration (54 commits)
- Figma — UI/UX design

---

## ✨ Features

- **Next 3 trains at a glance** — The app shows the next departure and the 2 following ones for your saved route, with scheduled time and real-time delay
- **Apple Watch app** — A native watchOS companion app displays the timetable directly on your wrist — no need to take your phone out
- **Home screen widget** — An iOS WidgetKit widget shows the next departures on your home screen, updating automatically
- **iPhone ↔ Watch sync** — `WatchConnectivity` keeps the saved route and timetable data in sync between the iPhone and Watch apps
- **Trip cards** — Each departure is presented as a `tripCard` — a clean, scannable card showing train number, departure time, and delay status
- **Trenitalia live data** — Real-time departure times and delays fetched directly from the Trenitalia API
- **Trilingual** — Full localisation in English, French, and Italian

---

## 🏗️ Architecture & Technical Details

### System Design

The app is a **multi-target Xcode project** with three separate targets sharing the same data model:

```
Trenitalia API
      │
      │  HTTP / JSON
      ▼
iPhone App (tracks/)
  ├── Fetches departures for saved route
  ├── Displays trip cards list
  └── Sends data to Watch via WatchConnectivity
        │
        ├──▶ Watch App (Tracks Watch App/)
        │       Displays next 3 departures on wrist
        │
        └──▶ Widget (MyWidget/)
                Displays next 3 departures on home screen
```

### Key Files

| File | Purpose |
|------|---------|
| `tripCard.swift` | Reusable card component showing departure time, train number, and delay |
| `watchConnectivityManager.swift` | Manages iPhone ↔ Watch data sync via `WCSession` |
| `model/` | Data models for train departures, routes, and API responses |
| `tracks/` | Main iPhone app — views, view models, API service |
| `Tracks Watch App/` | watchOS app target |
| `MyWidget/` | WidgetKit extension — timeline provider + widget view |

### Key Technical Decisions

- **WatchConnectivity for sync**: Rather than having the Watch make its own API calls, the iPhone fetches data and pushes it to the Watch via `WCSession`. This saves battery on the Watch and avoids duplicate network calls.
- **WidgetKit timeline provider**: The widget uses a `TimelineProvider` to schedule refresh points around the next departure times, so the widget always shows fresh data without draining battery.
- **Trenitalia API**: Trenitalia exposes an unofficial but stable real-time departure API used by third-party apps across Italy. The app queries it by station and direction to get live timetable data.
- **Shared model layer**: The `model/` folder is shared across all three targets (iPhone, Watch, Widget) so `tripCard` and departure models are defined once.
- **Trilingual from day one**: With a team from France, Italy, and elsewhere, localisation in EN/FR/IT was built in from the start rather than retrofitted.

---

## 📁 Project Structure

```
mc3/
├── tracks/                     # iPhone app — views, ViewModels, API service
├── Tracks Watch App/           # watchOS app target
├── MyWidget/                   # WidgetKit extension
├── model/                      # Shared data models (Train, Departure, Route)
├── en.lproj/                   # English localisation strings
├── fr.lproj/                   # French localisation strings
├── it.lproj/                   # Italian localisation strings
├── tripCard.swift              # Reusable trip card UI component
├── watchConnectivityManager.swift  # WCSession manager (iPhone ↔ Watch)
└── tracks.xcodeproj            # Xcode multi-target project
```

---

## ⚙️ Getting Started

### Prerequisites
- Xcode 14+
- iOS 16+ device or simulator (for iPhone app and widget)
- watchOS 9+ device or simulator (for Watch app)
- An active internet connection (Trenitalia API)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/audreyhda/mc3.git
cd mc3

# 2. Open in Xcode
open tracks.xcodeproj

# 3. Select the tracks scheme + a simulator and press Run (⌘R)
#    For the Watch app, select the Tracks Watch App scheme
#    For the widget, it appears automatically on a simulator with the iPhone app installed
```

No API keys required — the Trenitalia API is public.

---

## ✅ Skills Demonstrated

- Multi-target Xcode project (iOS + watchOS + WidgetKit)
- WatchConnectivity for real-time iPhone ↔ Apple Watch data sync
- WidgetKit timeline provider with scheduled refresh
- REST API integration with URLSession and `async`/`await`
- Shared data model layer across multiple targets
- Full app localisation (EN / FR / IT)
- Collaborative development with Git (54 commits across the team)
- Challenge Based Learning methodology — problem identified, designed, and shipped as a team

---

## 👥 Team

Built as a group project at the **Apple Developer Academy**, Naples — 2022/2023.

- **Audrey** — [@audreyhda](https://github.com/audreyhda)
- [@alevinaccia](https://github.com/alevinaccia)
- *Add other team members here*

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

*Apple Developer Academy — University of Naples Federico II · Second Group Project · 2022/2023*
