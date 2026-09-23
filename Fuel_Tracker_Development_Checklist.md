# Fuel Tracker — Development Checklist

A checkable development roadmap for building, testing, deploying, and maintaining the Fuel Tracker application.

> **How to use this file:** Replace `⬜` with `✅` as each item is completed. Add notes beneath items when useful.

---

# 0. Project Setup

## 0.1 Repository

    ⬜ Create GitHub repository
    ⬜ Add README.md
    ⬜ Add Node.js .gitignore
    ⬜ Add open-source license
    ⬜ Clone repository to development Mac
    ⬜ Create initial Git commit
    ⬜ Establish main/development branch strategy

## 0.2 Development Environment

    ⬜ Install/verify Node.js
    ⬜ Install/verify npm
    ⬜ Install/verify Git
    ⬜ Install/verify Docker
    ⬜ Establish local development environment
    ⬜ Establish environment-variable strategy
    ⬜ Create .env.example
    ⬜ Ensure secrets are excluded from Git

## 0.3 Initial Project Structure

    ⬜ Create backend directory
    ⬜ Create frontend directory
    ⬜ Create database/migrations directory
    ⬜ Create deployment directory
    ⬜ Create documentation directory
    ⬜ Create development scripts
    ⬜ Create production configuration

---

# 1. Backend Foundation

## 1.1 Node.js / Express

    ⬜ Initialize Node.js project
    ⬜ Install Express
    ⬜ Create Express application
    ⬜ Create development server
    ⬜ Add environment-based configuration
    ⬜ Add API routing structure
    ⬜ Add request validation
    ⬜ Add centralized error handling
    ⬜ Add logging
    ⬜ Add health-check endpoint

## 1.2 SQLite

    ⬜ Add SQLite database
    ⬜ Create database connection layer
    ⬜ Create migration system
    ⬜ Create initial migration
    ⬜ Add database backup strategy
    ⬜ Add database integrity checks
    ⬜ Test database creation from an empty installation

## 1.3 API Foundation

    ⬜ Establish REST API conventions
    ⬜ Establish API versioning strategy
    ⬜ Establish standard response/error format
    ⬜ Add authentication middleware placeholder
    ⬜ Add user-data authorization middleware structure

---

# 2. Authentication & Users

## 2.1 User Accounts

    ⬜ Create users table
    ⬜ Create user registration flow
    ⬜ Create login flow
    ⬜ Create logout flow
    ⬜ Hash passwords securely
    ⬜ Add session/authentication mechanism
    ⬜ Add authentication middleware
    ⬜ Add authenticated user endpoint
    ⬜ Add account/settings foundation

## 2.2 Data Isolation

    ⬜ Associate user-owned data with users
    ⬜ Enforce user ownership at API level
    ⬜ Prevent cross-user data access
    ⬜ Test unauthorized access attempts
    ⬜ Test multiple users independently

---

# 3. Vehicles

## 3.1 Vehicle Database

    ⬜ Create vehicles table
    ⬜ Add vehicle name
    ⬜ Add year
    ⬜ Add make
    ⬜ Add model
    ⬜ Add default fuel grade
    ⬜ Associate vehicles with users

## 3.2 Vehicle API

    ⬜ Create vehicle
    ⬜ Read vehicles
    ⬜ Read individual vehicle
    ⬜ Update vehicle
    ⬜ Delete vehicle
    ⬜ Validate vehicle ownership

## 3.3 Vehicle UI

    ⬜ Create vehicle list
    ⬜ Create add vehicle screen
    ⬜ Create edit vehicle screen
    ⬜ Create delete confirmation
    ⬜ Select default fuel grade
    ⬜ Test multiple vehicles

---

# 4. Fuel Grades

## 4.1 Fuel Grade System

    ⬜ Create fuel grades table
    ⬜ Add predefined Regular grade
    ⬜ Add predefined Premium grade
    ⬜ Add optional Mid-grade
    ⬜ Add optional E85
    ⬜ Add optional Diesel
    ⬜ Support custom fuel grades
    ⬜ Add optional octane value
    ⬜ Add enabled/disabled state
    ⬜ Associate custom grades with users

## 4.2 Fuel Grade Display

    ⬜ Support display by name
    ⬜ Support display by octane
    ⬜ Fall back to name when octane is unavailable
    ⬜ Add fuel grade settings UI

---

# 5. Fuel Tracking

## 5.1 Fuel Purchase Database

    ⬜ Create fuel purchase table
    ⬜ Store user
    ⬜ Store vehicle
    ⬜ Store date
    ⬜ Store odometer
    ⬜ Store fuel grade
    ⬜ Store price per gallon
    ⬜ Store gallons
    ⬜ Store total cost
    ⬜ Store station
    ⬜ Store location
    ⬜ Store partial-fill status
    ⬜ Add local/client-generated ID for synchronization

## 5.2 Add Fuel

    ⬜ Create Add Fuel screen
    ⬜ Pre-fill current date
    ⬜ Select vehicle
    ⬜ Pre-select vehicle default fuel grade
    ⬜ Enter odometer
    ⬜ Enter $/gallon
    ⬜ Enter total cost
    ⬜ Enter gallons
    ⬜ Calculate gallons from total cost
    ⬜ Calculate total cost from gallons
    ⬜ Show calculated value as secondary information
    ⬜ Add partial-fill toggle
    ⬜ Save fuel purchase

## 5.3 Fuel History

    ⬜ Create fuel history screen
    ⬜ Display fuel purchases
    ⬜ Filter by vehicle
    ⬜ Filter by date
    ⬜ View fuel entry details
    ⬜ Edit fuel entry
    ⬜ Delete fuel entry
    ⬜ Confirm destructive actions

## 5.4 Partial Fill-ups

    ⬜ Include partial fills in fuel history
    ⬜ Include partial fills in spending
    ⬜ Include partial fills in gallons purchased
    ⬜ Include partial fills in fuel-price statistics
    ⬜ Exclude partial fills from standard MPG calculations

---

# 6. Stations & Locations

## 6.1 Station Database

    ⬜ Create stations table
    ⬜ Store station name
    ⬜ Store city
    ⬜ Store state
    ⬜ Store optional latitude
    ⬜ Store optional longitude
    ⬜ Associate stations with users

## 6.2 Station Selection

    ⬜ Manual station entry
    ⬜ Previous station picker
    ⬜ Search/filter previous stations
    ⬜ Store station list for offline use
    ⬜ Select an existing station
    ⬜ Create a new station

## 6.3 Location Input

    ⬜ Support Station + City + State format
    ⬜ Support Station + Location format
    ⬜ Support single combined location format
    ⬜ Add configurable default state
    ⬜ Allow per-entry state override
    ⬜ Ensure per-entry state changes do not alter default state

---

# 7. Frontend & PWA

## 7.1 Responsive UI

    ⬜ Establish frontend framework/structure
    ⬜ Create responsive layout
    ⬜ Design mobile-first navigation
    ⬜ Create desktop-friendly layout
    ⬜ Establish reusable UI components
    ⬜ Establish form components
    ⬜ Establish loading states
    ⬜ Establish error states
    ⬜ Establish empty states

## 7.2 PWA

    ⬜ Create web app manifest
    ⬜ Configure application name/icon
    ⬜ Add service worker
    ⬜ Cache required application resources
    ⬜ Make application installable
    ⬜ Test PWA installation on iPhone
    ⬜ Test PWA installation on desktop

---

# 8. Offline Mode & Synchronization

## 8.1 IndexedDB

    ⬜ Add IndexedDB
    ⬜ Create local data model
    ⬜ Store pending fuel entries
    ⬜ Store previous stations
    ⬜ Store required user configuration
    ⬜ Generate local unique IDs
    ⬜ Track local synchronization state

## 8.2 Offline Fuel Entry

    ⬜ Detect offline state
    ⬜ Allow Add Fuel while offline
    ⬜ Save fuel entry locally
    ⬜ Display Offline status
    ⬜ Display Pending Sync status
    ⬜ Preserve entry after page/application restart

## 8.3 Synchronization

    ⬜ Create synchronization API
    ⬜ Create client sync queue
    ⬜ Upload pending entries
    ⬜ Mark successfully synchronized entries
    ⬜ Display Syncing status
    ⬜ Display Sync Error status
    ⬜ Retry failed synchronization
    ⬜ Synchronize when connectivity returns
    ⬜ Handle interrupted uploads
    ⬜ Prevent duplicate entries
    ⬜ Test repeated upload of the same local ID
    ⬜ Test synchronization after prolonged offline use

## 8.4 Station Synchronization

    ⬜ Synchronize newly created stations
    ⬜ Prevent duplicate stations where appropriate
    ⬜ Preserve local station availability offline

---

# 9. GPS Station Detection

## 9.1 Location Access

    ⬜ Request location only when user selects GPS
    ⬜ Handle location permission
    ⬜ Handle location permission denial
    ⬜ Handle unavailable location
    ⬜ Do not continuously track user location

## 9.2 Station Matching

    ⬜ Compare GPS coordinates against local stations
    ⬜ Implement configurable detection radius
    ⬜ Suggest nearby station
    ⬜ Allow user to accept suggested station
    ⬜ Allow user to choose another station
    ⬜ Allow user to ignore location
    ⬜ Create new station when no match exists
    ⬜ Ensure matching works offline

---

# 10. Additional Fuel Price Observations

## 10.1 Price Observation Database

    ⬜ Support fuel price observation records
    ⬜ Store user
    ⬜ Store date
    ⬜ Store fuel grade
    ⬜ Store price per gallon
    ⬜ Store station
    ⬜ Store city
    ⬜ Store state
    ⬜ Store optional coordinates
    ⬜ Exclude vehicle from observations
    ⬜ Exclude odometer from observations
    ⬜ Exclude gallons from observations
    ⬜ Exclude total cost from observations

## 10.2 Additional Fuel UI

    ⬜ Add + Other Fuel button
    ⬜ Reveal additional fuel-price fields
    ⬜ Allow multiple fuel grades at one station
    ⬜ Save each additional fuel type as its own database entry
    ⬜ Keep normal Add Fuel workflow compact

---

# 11. Statistics

## 11.1 MPG

    ⬜ Calculate average MPG
    ⬜ Calculate MPG over time
    ⬜ Calculate MPG by vehicle
    ⬜ Filter MPG by date range
    ⬜ Exclude partial fills from standard MPG
    ⬜ Handle insufficient data gracefully

## 11.2 Cost

    ⬜ Calculate total fuel spending
    ⬜ Calculate cost per mile
    ⬜ Calculate cost per gallon
    ⬜ Show spending over time
    ⬜ Filter cost statistics

## 11.3 Fuel Prices

    ⬜ Show price history by fuel grade
    ⬜ Show price history by station
    ⬜ Show price history by city/state
    ⬜ Show price trends over time
    ⬜ Include price observations
    ⬜ Clearly distinguish purchases from observations

---

# 12. Configurability

## 12.1 Fuel Entry Layout

    ⬜ Store field ordering per user
    ⬜ Create drag-and-drop field ordering UI
    ⬜ Reorder main fuel-entry fields
    ⬜ Preserve dependencies between related controls
    ⬜ Keep + Other Fuel associated with Grade
    ⬜ Keep GPS/previous-station controls associated with Station
    ⬜ Add Restore Default Layout

## 12.2 User Settings

    ⬜ Configure primary quantity input
    ⬜ Configure location input format
    ⬜ Configure default state
    ⬜ Configure GPS detection radius
    ⬜ Configure fuel grade display mode
    ⬜ Configure enabled fuel grades
    ⬜ Configure custom fuel grades
    ⬜ Configure vehicle defaults

---

# 13. Testing & Reliability

## 13.1 Backend Testing

    ⬜ Add automated backend tests
    ⬜ Test authentication
    ⬜ Test authorization
    ⬜ Test vehicle CRUD
    ⬜ Test fuel CRUD
    ⬜ Test station CRUD
    ⬜ Test fuel grades
    ⬜ Test price observations
    ⬜ Test synchronization
    ⬜ Test duplicate protection
    ⬜ Test database migrations

## 13.2 Frontend Testing

    ⬜ Add frontend tests where appropriate
    ⬜ Test Add Fuel workflow
    ⬜ Test vehicle selection
    ⬜ Test quantity calculations
    ⬜ Test station selection
    ⬜ Test offline entry
    ⬜ Test synchronization states
    ⬜ Test GPS workflow
    ⬜ Test responsive layouts

## 13.3 Real-World Testing

    ⬜ Test on iPhone
    ⬜ Test on desktop browser
    ⬜ Test with no network
    ⬜ Test with intermittent network
    ⬜ Test with multiple vehicles
    ⬜ Test with multiple users
    ⬜ Test database backup/restore
    ⬜ Test application restart
    ⬜ Test server restart
    ⬜ Test upgrade from an older version

---

# 14. Deployment & Infrastructure

## 14.1 Docker

    ⬜ Create production Dockerfile
    ⬜ Create Docker Compose configuration
    ⬜ Configure persistent data storage
    ⬜ Configure environment variables
    ⬜ Configure health checks
    ⬜ Configure automatic restart
    ⬜ Verify containerized application locally

## 14.2 Proxmox

    ⬜ Define recommended Proxmox VM/LXC configuration
    ⬜ Document CPU requirements
    ⬜ Document RAM requirements
    ⬜ Document storage requirements
    ⬜ Document networking requirements
    ⬜ Test installation on Proxmox
    ⬜ Test application restart after host reboot

## 14.3 Installation Script

    ⬜ Create install.sh
    ⬜ Check prerequisites
    ⬜ Install required dependencies
    ⬜ Clone/download application
    ⬜ Generate secure secrets
    ⬜ Create configuration
    ⬜ Initialize database
    ⬜ Run migrations
    ⬜ Start application
    ⬜ Verify health check
    ⬜ Display application address
    ⬜ Document first-run setup

## 14.4 Updates

    ⬜ Create update.sh
    ⬜ Backup database before update
    ⬜ Pull new application version
    ⬜ Run migrations
    ⬜ Restart application
    ⬜ Verify health check
    ⬜ Provide rollback procedure

## 14.5 Backups

    ⬜ Create backup.sh
    ⬜ Create automated database backups
    ⬜ Define backup retention
    ⬜ Support manual backup
    ⬜ Create restore.sh
    ⬜ Test restoration
    ⬜ Document backup location

## 14.6 Reverse Proxy / HTTPS

    ⬜ Document reverse proxy requirements
    ⬜ Configure domain
    ⬜ Configure HTTPS
    ⬜ Verify secure cookies/authentication
    ⬜ Test PWA over HTTPS
    ⬜ Test API over HTTPS

---

# 15. GitHub & Release Process

## 15.1 Repository Management

    ⬜ Establish branch strategy
    ⬜ Establish commit conventions
    ⬜ Document development workflow
    ⬜ Document issue/feature workflow
    ⬜ Create GitHub issue templates if useful

## 15.2 CI/CD

    ⬜ Add GitHub Actions
    ⬜ Run automated tests on push
    ⬜ Run linting/checks
    ⬜ Build production application
    ⬜ Build Docker image
    ⬜ Publish versioned releases
    ⬜ Document release process

## 15.3 Versioning

    ⬜ Establish application version format
    ⬜ Display application version
    ⬜ Tag releases
    ⬜ Maintain changelog
    ⬜ Ensure database migrations are versioned

---

# 16. Documentation

    ⬜ Complete README
    ⬜ Installation documentation
    ⬜ Proxmox deployment documentation
    ⬜ Configuration documentation
    ⬜ Development documentation
    ⬜ API documentation
    ⬜ Database documentation
    ⬜ Backup/restore documentation
    ⬜ Upgrade documentation
    ⬜ Troubleshooting documentation
    ⬜ Contribution documentation

---

# 17. Initial Release

    ⬜ Complete core fuel tracking
    ⬜ Complete authentication
    ⬜ Complete vehicle management
    ⬜ Complete station management
    ⬜ Complete PWA
    ⬜ Complete offline entry
    ⬜ Complete synchronization
    ⬜ Complete GPS station detection
    ⬜ Complete additional fuel observations
    ⬜ Complete core statistics
    ⬜ Complete user configuration
    ⬜ Complete production deployment
    ⬜ Complete backup/restore
    ⬜ Complete documentation
    ⬜ Perform final end-to-end testing
    ⬜ Create v1.0.0 release

---

# 18. Future Vehicle Maintenance

> Maintenance is intentionally outside the initial Fuel Tracker implementation. The architecture should support adding it later without rebuilding the fuel system.

## 18.1 Planning

    ⬜ Define maintenance data model
    ⬜ Define maintenance API
    ⬜ Define maintenance UI
    ⬜ Determine relationship between vehicles, fuel, and maintenance

## 18.2 Maintenance Records

    ⬜ Maintenance records
    ⬜ Service type
    ⬜ Date
    ⬜ Odometer
    ⬜ Cost
    ⬜ Parts
    ⬜ Labor
    ⬜ Notes
    ⬜ Service history

## 18.3 Maintenance Planning

    ⬜ Recurring maintenance
    ⬜ Maintenance intervals
    ⬜ Maintenance reminders
    ⬜ Maintenance schedules

## 18.4 Maintenance Statistics

    ⬜ Maintenance spending
    ⬜ Cost by vehicle
    ⬜ Cost by service type
    ⬜ Maintenance history

---

# 19. Future Expansion

    ⬜ Define module architecture
    ⬜ Determine future vehicle-management modules
    ⬜ Determine module installation/update strategy
    ⬜ Determine API extension strategy
    ⬜ Determine future integrations

---

# Development Notes

Use this section to record important implementation decisions, problems encountered, or changes to the development plan.

## Notes

- 
- 
- 

## Architecture Decisions

- 
- 
- 

## Known Issues

- 
- 
- 

## Future Ideas

- 
- 
- 

---

# Milestone Log

| Date | Milestone | Notes |
|------|-----------|-------|
| | Project started | |
| | Foundation complete | |
| | Authentication complete | |
| | Vehicles complete | |
| | Fuel tracking complete | |
| | Stations complete | |
| | PWA complete | |
| | Offline/sync complete | |
| | GPS complete | |
| | Price observations complete | |
| | Statistics complete | |
| | Configuration complete | |
| | Production deployment complete | |
| | v1.0.0 released | |
