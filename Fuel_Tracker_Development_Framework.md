# Fuel Tracker — Development Framework

## 1. Project Overview

A self-hosted, multi-user vehicle fuel tracking web application designed primarily for mobile use.

The application will:

- Run on a self-hosted Proxmox server
- Be accessible through a personal domain
- Use Node.js and Express for the backend
- Use SQLite for the database initially
- Use a responsive web interface
- Be installable as a Progressive Web App (PWA)
- Support offline fuel entry
- Synchronize locally stored data with the server when connectivity returns
- Keep user data isolated between accounts
- Support multiple vehicles
- Track fuel purchases and fuel-price observations
- Provide fuel economy and cost statistics
- Support location identification without requiring an external gas-station database
- Be designed to accommodate future vehicle maintenance functionality

---

# 2. Core Architecture

```text
                         Internet
                            │
                            ▼
                  ┌──────────────────┐
                  │ Reverse Proxy    │
                  │ HTTPS / Domain   │
                  └────────┬─────────┘
                           │
                           ▼
                  ┌──────────────────┐
                  │ Node.js          │
                  │ Express          │
                  │                  │
                  │ REST API         │
                  │ Authentication   │
                  │ Synchronization  │
                  └────────┬─────────┘
                           │
                           ▼
                  ┌──────────────────┐
                  │ SQLite           │
                  │ Server Database  │
                  └──────────────────┘
                           ▲
                           │
                     Synchronization
                           │
             ┌─────────────┴─────────────┐
             │                           │
        ┌────▼─────┐               ┌────▼─────┐
        │ iPhone   │               │ Other    │
        │ PWA      │               │ Device   │
        │          │               │ PWA      │
        │ IndexedDB│               │ IndexedDB│
        └──────────┘               └──────────┘
```

### Backend

- Node.js
- Express
- SQLite
- REST API
- Authentication
- Synchronization system

### Frontend

- Responsive web application
- PWA
- Local IndexedDB database
- Service worker
- Offline support

---

# 3. Authentication & Users

The application will support multiple users.

Each user should have their own:

- Vehicles
- Fuel entries
- Stations
- Settings
- Fuel grades/customizations
- Statistics

Users should not be able to access another user's data.

Authentication should be handled by the Express backend.

---

# 4. Vehicles

Users can create multiple vehicles.

A vehicle should contain at minimum:

```text
Vehicle
├── Name
├── Year
├── Make
├── Model
└── Default fuel grade
```

The vehicle's default fuel grade is used to pre-populate the Add Fuel form.

Example:

```text
2001 Toyota 4Runner
Default fuel: Regular

2020 Kawasaki Ninja 400
Default fuel: Premium
```

The default can always be overridden for an individual fuel entry.

---

# 5. Fuel Grades

Fuel grades are configurable.

### Default grades

- Regular
- Premium

### Predefined optional grades

- Mid-grade
- E85
- Diesel

### Custom grades

Users can create their own fuel types, such as:

- Race Fuel
- 100 Octane
- Other specialty fuels

Fuel grades should contain:

```text
Fuel Grade
├── Name
├── Octane (optional)
└── Enabled
```

The user can choose whether fuel grades are displayed by:

**Name**

```text
Regular
Mid-grade
Premium
```

or by:

**Octane**

```text
87 Octane
89 Octane
91 Octane
```

Fuel grades without an applicable octane value can continue to display their name.

---

# 6. Fuel Entries

There are two types of fuel entries.

## Purchase

Represents fuel actually purchased.

Contains:

- User
- Vehicle
- Date
- Odometer
- Fuel grade
- Price per gallon
- Gallons
- Total cost
- Station
- Location
- Partial-fill status

## Price Observation

Represents a fuel price observed at a station without purchasing that fuel.

Contains:

- User
- Date
- Fuel grade
- Price per gallon
- Station
- Location

It does **not** contain:

- Vehicle
- Odometer
- Gallons
- Total cost

This allows multiple fuel prices to be recorded at one station.

Example:

```text
Purchase
Regular — $3.89/gal — 12.4 gal — $48.24

Price Observation
Premium — $4.39/gal
```

Both reference the same station and date.

---

# 7. Add Fuel Screen

Default layout:

```text
Add Fuel

Date
Vehicle
Odometer
$/Gallon
Total $
Grade
    + Other Fuel Type(s)
Station
    📍 Current Location
    ⋮ Previous Stations
Location
Partial Fill-up
Save Fuel Stop
```

Date is pre-filled with today's date.

Vehicle is selected from the user's vehicles.

The fuel grade defaults to the selected vehicle's default fuel grade.

---

# 8. Fuel Quantity Input

Users can configure whether they primarily enter:

### Total cost

```text
$/Gallon
$3.89

Total $
$48.24
12.40 gallons
```

or:

### Gallons

```text
$/Gallon
$3.89

Gallons
12.40

Total $
$48.24
```

The application calculates the other value and displays it as secondary/subtext.

---

# 9. Additional Fuel Types

The normal Add Fuel screen should remain compact.

Initially:

```text
Grade
Regular

[ + Other Fuel ]
```

Pressing the button reveals additional fuel-price fields.

Example:

```text
Grade
Regular

[ + Other Fuel ]

Other Fuel
Premium

Price
$4.39
```

Additional fuel types are price observations.

Saving the fuel stop creates separate database entries for them.

Each additional fuel type recorded at a station is its own database entry containing:

- User
- Date
- Fuel grade
- Price per gallon
- Station
- City
- State
- Optional coordinates

It does not contain:

- Vehicle
- Odometer
- Gallons
- Total cost

---

# 10. Partial Fill-ups

Fuel purchases have a:

```text
Partial Fill-up
[ OFF ]
```

toggle.

Partial fill-ups remain part of:

- Fuel history
- Fuel spending
- Gallons purchased
- Fuel price statistics

But they are excluded from standard MPG calculations.

---

# 11. Location & Stations

Stations are user-created rather than dependent on an external gas-station database.

A station contains:

```text
Station
├── Name
├── City
├── State
├── Latitude (optional)
└── Longitude (optional)
```

The application should support:

### Manual entry

Depending on user settings:

```text
Station
City
State
```

or:

```text
Station
Location
```

or:

```text
Station, City, State
```

---

# 12. Location Input Configuration

Users can choose between:

### Single field

```text
Costco, Orem, UT
```

### Station + Location

```text
Station
Costco

Location
Orem, UT
```

### Station + City + State

```text
Station
Costco

City
Orem

State
UT
```

The selected format controls the Add Fuel interface.

---

# 13. Default State

Users can configure a default state.

Example:

```text
Default state: Utah
```

The state is automatically selected for new entries.

The user can still change it for individual entries.

Changing the state for an individual fuel stop does **not** change the default state.

---

# 14. Previously Used Stations

A small button beside the station input opens previously used stations.

Example:

```text
Station
[ Costco              ] [⋮]
```

The station picker can show:

```text
Costco
Orem, UT

Maverik
Provo, UT

Chevron
Salt Lake City, UT
```

The station list should be available offline.

The station picker should support searching/filtering when the user has many previously used stations.

---

# 15. GPS Station Detection

The GPS button obtains the phone's current location.

The application compares the coordinates against locally stored stations.

A configurable radius accounts for GPS drift.

Example:

```text
GPS location
     │
     ▼
Local stations
     │
     ▼
Within radius?
   /       \
 Yes       No
  │         │
  ▼         ▼
Suggest   Create new
station   station
```

If a station is found:

> Is this Costco — Orem, UT?

Options:

- Use station
- Choose another station
- Ignore location

If no station is found:

- Create new station
- Choose previous station
- Enter manually

GPS matching must work offline.

The application should not continuously track the user's location. Location is requested only when the user explicitly chooses the GPS/location button.

---

# 16. Offline Functionality

The PWA should be designed as **offline-first**.

IndexedDB stores locally:

- Pending fuel entries
- Previously used stations
- User configuration needed by the interface
- Other data necessary to record a fuel stop

A user should be able to record a fuel stop without Internet access.

Entries receive a locally generated unique ID.

Example:

```text
localId:
8f3c7e21-...
```

The entry is marked:

```text
Pending Sync
```

When connectivity returns:

```text
Local entry
    ↓
Express API
    ↓
SQLite
    ↓
Confirmation
    ↓
Synced
```

The UI should clearly communicate sync status, for example:

- Online / Synced
- Offline
- Pending Sync
- Syncing
- Sync Error

---

# 17. Synchronization

Synchronization must prevent duplicate entries.

The server should recognize the locally generated unique ID.

If an upload is repeated because of a network interruption, the server should recognize that the entry already exists rather than creating another record.

Synchronization should be designed into the API from the beginning.

The server should be authoritative for synchronized/shared data while allowing locally-created fuel entries and stations to be queued while offline.

---

# 18. Fuel Statistics

The application should eventually provide statistics for:

### MPG

- Average MPG
- MPG over time
- MPG by vehicle
- MPG by date range
- MPG excluding partial fill-ups

### Cost

- Total fuel spending
- Cost per mile
- Cost per gallon
- Fuel spending over time

### Fuel prices

- Regular price history
- Premium price history
- Other fuel-grade price history
- Price by station
- Price by city/state
- Price trends over time

Price observations should participate in fuel-price statistics even though they aren't purchases.

Statistics should clearly distinguish purchase data from price-only observations.

---

# 19. Configurable Fuel Entry Layout

The Add Fuel screen should not have a permanently fixed field order.

Users can reorder fields.

Default:

```text
Date
Vehicle
Odometer
$/Gallon
Total $
Grade
Station
Location
Partial Fill-up
```

Settings can provide a drag-and-drop ordering interface.

Example:

```text
☰ Date
☰ Vehicle
☰ Odometer
☰ $/Gallon
☰ Total $
☰ Grade
☰ Station
☰ Location
☰ Partial Fill-up
```

There should be a:

**Restore Default Layout**

button.

The layout configuration should be stored per user.

The configurable layout should affect the main fuel-entry fields while preserving dependencies between related controls. For example, the additional fuel button remains associated with Grade, and the GPS/previous-station buttons remain associated with Station.

---

# 20. Settings

Settings should include at minimum:

## Fuel Entry

- Input field order
- Primary quantity input: Total $ or Gallons
- Location input format
- Default state
- GPS station detection radius
- Restore default layout

## Fuel Grades

- Enable/disable predefined grades
- Add custom fuel grades
- Edit custom fuel grades
- Optional octane value
- Fuel grade display mode: Name or Octane

## Vehicles

- Vehicle management
- Default fuel grade per vehicle

## Statistics

- Fuel grade display preference
- MPG calculation behavior/settings as additional options are developed

---

# 21. Future Vehicle Maintenance

Vehicle maintenance should be planned as a **future feature/module**, without attempting to fully specify it yet.

The architecture should allow:

```text
Vehicle
   │
   ├── Fuel
   │
   └── Maintenance
```

Future maintenance functionality could eventually include:

- Maintenance records
- Service type
- Date
- Odometer
- Cost
- Parts
- Labor
- Notes
- Recurring maintenance
- Maintenance intervals
- Service history
- Cost statistics

The maintenance system should remain outside the initial Fuel Tracker development scope.

The initial architecture should avoid making fuel-specific assumptions that would make future vehicle maintenance difficult to add.

---

# 22. Development Phases

## Phase 1 — Foundation

- Node.js
- Express
- SQLite
- Project structure
- Database migrations
- Authentication
- Basic API
- Basic frontend

## Phase 2 — Vehicles

- Vehicle management
- Default fuel grades
- Vehicle selection

## Phase 3 — Fuel Tracking

- Add fuel
- Edit fuel
- Delete fuel
- Fuel history
- Partial fill-ups
- Quantity calculations
- Fuel grades

## Phase 4 — Stations & Locations

- Station database
- Manual locations
- Previous station selection
- Default state
- Configurable location formats

## Phase 5 — PWA

- Web app manifest
- Installability
- Service worker
- Mobile UI

## Phase 6 — Offline Mode

- IndexedDB
- Offline fuel entry
- Offline station data
- Sync queue
- Duplicate protection
- Sync status

## Phase 7 — GPS

- Location permissions
- GPS station matching
- Configurable detection radius
- New station creation
- Offline GPS matching
- Ignore-location option

## Phase 8 — Additional Fuel Prices

- Other fuel button
- Price observations
- Multiple grades per station
- Price history

## Phase 9 — Statistics

- MPG
- Fuel costs
- Cost/mile
- Fuel price trends
- Filtering

## Phase 10 — Configurability

- Fuel entry field ordering
- Fuel grade configuration
- Name/octane display preference
- Location input preferences
- Other user settings
- Restore defaults

## Future Phase — Maintenance

Design and implement the vehicle maintenance system once the fuel system is stable.

---

# 23. Guiding Principles

1. **Self-hosted first** — personal data should remain on the user's server.
2. **Offline-first** — lack of cellular/Wi-Fi should not prevent recording a fuel stop.
3. **No unnecessary external dependencies** — especially for identifying previously recorded stations.
4. **Mobile-first** — the primary interface is the phone.
5. **Configurable** — users should be able to adapt the application to their workflow.
6. **Data-driven** — fuel grades, stations, vehicles, and settings should not be hard-coded.
7. **Separate observations from purchases** — recording a price does not imply purchasing that fuel.
8. **GPS is optional** — users can always enter or select a station manually.
9. **Privacy-conscious location handling** — do not continuously track location; only request it when the user explicitly asks to use the current location.
10. **Reliable synchronization** — offline data must not be silently lost or duplicated.
11. **Designed for expansion** — maintenance and other vehicle-related functionality can be added later without rebuilding the core.
12. **Mobile entry should be fast** — the common fuel-entry workflow should require as little scrolling and interaction as practical.

---

# 24. Initial Default Fuel Entry

The default Add Fuel screen should use this order:

```text
Title: Add Fuel

Date
    Pre-filled with today

Vehicle

Odometer

$/Gallon

Total $

Grade
    + Other Fuel Type(s)

Station
    📍 Current Location
    ⋮ Previously Used Stations

Location

Partial Fill-up

Save Fuel Stop
```

The form should be optimized for quickly recording a normal fuel stop while keeping less-common functionality accessible without cluttering the primary workflow.

---

# 25. Example Fuel-Entry Workflow

## Normal online fuel stop

```text
Open PWA
    ↓
Add Fuel
    ↓
Date already filled
    ↓
Select vehicle
    ↓
Enter odometer
    ↓
Enter $/gallon
    ↓
Enter total $ or gallons
    ↓
Vehicle's default fuel grade is selected
    ↓
Use GPS
    ↓
Previously recorded station is detected
    ↓
Accept station
    ↓
Save Fuel Stop
    ↓
Entry synchronized with server
```

## Normal offline fuel stop

```text
Open PWA
    ↓
Add Fuel
    ↓
Enter fuel information
    ↓
Use GPS
    ↓
Local station database finds a match
    ↓
Accept station
    ↓
Save Fuel Stop
    ↓
Entry stored in IndexedDB
    ↓
Marked Pending Sync
    ↓
Internet returns
    ↓
Entry synchronized
```

## New station while offline

```text
Open PWA
    ↓
Add Fuel
    ↓
Use GPS
    ↓
No nearby recorded station
    ↓
Create New Station
    ↓
Enter station information
    ↓
Station saved locally
    ↓
Record fuel purchase
    ↓
Fuel entry saved locally
    ↓
Both synchronize later
```

## Forgot to record the fuel stop

```text
Open PWA later
    ↓
Add Fuel
    ↓
Enter historical date
    ↓
Enter vehicle / odometer / fuel information
    ↓
Select previous station or enter manually
    ↓
Do not use GPS
    ↓
Save Fuel Stop
```

---

# 26. Long-Term Project Direction

The Fuel Tracker should begin as a focused fuel-management application but be structured as the foundation of a broader vehicle-management platform.

Potential future areas include:

```text
Vehicle Management
│
├── Fuel
│   ├── Fuel Entries
│   ├── Fuel Prices
│   ├── MPG
│   └── Cost Statistics
│
├── Maintenance
│   ├── Service Records
│   ├── Maintenance Schedules
│   └── Maintenance Costs
│
└── Future Modules
    └── To be determined
```

The initial implementation should prioritize a reliable, fast, offline-capable fuel tracker rather than attempting to implement every future feature at once.
