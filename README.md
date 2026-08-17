# TransitOps — Smart Transport Operations Platform

TransitOps is a centralized transport operations management platform built for the Odoo Hackathon. It digitizes fleet management, driver management, trip dispatching, maintenance, fuel and expense tracking, and operational analytics in a single system.

The platform is designed to reduce scheduling conflicts, improve fleet utilization, enforce driver and vehicle compliance, and provide better visibility into transport operations.

## Problem Statement

Many transport and logistics organizations still depend on spreadsheets and manual logbooks to manage their fleets. This can result in:

* Scheduling conflicts and inefficient dispatching
* Underutilized vehicles
* Missed maintenance activities
* Expired driver licenses
* Inaccurate fuel and expense tracking
* Poor visibility into fleet performance

TransitOps addresses these challenges through a centralized and rule-driven transport management system.

## Key Features

### Dashboard

Provides an operational overview of the fleet through key performance indicators such as:

* Active Vehicles
* Available Vehicles
* Vehicles in Maintenance
* Active Trips
* Pending Trips
* Drivers On Duty
* Fleet Utilization

### Vehicle Management

* Vehicle registration and lifecycle tracking
* Unique vehicle registration numbers
* Vehicle type and load capacity management
* Odometer tracking
* Acquisition cost tracking
* Vehicle status management

Supported statuses:

`Available` → `On Trip` → `In Shop` → `Retired`

### Driver Management

* Driver profiles
* License number and category
* License expiry tracking
* Contact information
* Safety score
* Driver availability status

Supported statuses:

`Available` → `On Trip` → `Off Duty` → `Suspended`

### Trip & Dispatch Management

Trips can be created by selecting:

* Source and destination
* Vehicle
* Driver
* Cargo weight
* Planned distance

Trip lifecycle:

`Draft → Dispatched → Completed / Cancelled`

The system automatically updates vehicle and driver availability during the trip lifecycle.

### Maintenance Management

* Create and track maintenance records
* Automatically move vehicles to `In Shop`
* Prevent vehicles under maintenance from being dispatched
* Restore vehicle availability after maintenance is completed

### Fuel & Expense Management

* Fuel log management
* Fuel quantity and cost tracking
* Maintenance expense tracking
* Other operational expenses such as tolls
* Automatic operational cost calculation

### Reports & Analytics

TransitOps provides operational insights including:

* Fuel Efficiency
* Fleet Utilization
* Operational Cost
* Vehicle ROI
* CSV export

Vehicle ROI is calculated as:

```text
ROI = (Revenue - (Maintenance Cost + Fuel Cost)) / Acquisition Cost
```

## Business Rule Enforcement

A major focus of TransitOps is enforcing operational rules automatically rather than relying on manual checks.

The system ensures that:

* Vehicle registration numbers are unique.
* Retired vehicles cannot be dispatched.
* Vehicles under maintenance cannot be dispatched.
* Drivers with expired licenses cannot be assigned.
* Suspended drivers cannot be assigned.
* Drivers and vehicles already on a trip cannot be assigned again.
* Cargo weight cannot exceed vehicle capacity.
* Dispatching a trip automatically changes the vehicle and driver status to `On Trip`.
* Completing a trip restores both to `Available`.
* Cancelling a dispatched trip restores both to `Available`.
* Starting maintenance changes the vehicle status to `In Shop`.
* Completing maintenance restores the vehicle to `Available`, unless it has been retired.

These rules directly address the mandatory business requirements of the problem statement.

## Example Workflow

```text
Register Vehicle
       ↓
Register Driver
       ↓
Create Trip
       ↓
Validate Driver & Vehicle
       ↓
Validate Cargo Capacity
       ↓
Dispatch Trip
       ↓
Vehicle + Driver → On Trip
       ↓
Complete Trip
       ↓
Vehicle + Driver → Available
       ↓
Create Maintenance Record
       ↓
Vehicle → In Shop
       ↓
Complete Maintenance
       ↓
Vehicle → Available
       ↓
Update Analytics
```

## User Roles

TransitOps supports role-based access for different operational responsibilities:

| Role              | Responsibility                                                      |
| ----------------- | ------------------------------------------------------------------- |
| Fleet Manager     | Fleet assets, maintenance and operational efficiency                |
| Dispatcher        | Trip creation, vehicle/driver assignment and active trip monitoring |
| Safety Officer    | Driver compliance, license validity and safety monitoring           |
| Financial Analyst | Fuel, maintenance, expenses and profitability                       |

The problem statement identifies these operational roles and their responsibilities.

## System Architecture

TransitOps is implemented as a custom Odoo module.

```text
                    TransitOps
                        |
        ┌───────────────┼───────────────┐
        │               │               │
    Vehicles         Drivers          Trips
        │               │               │
        └───────────────┼───────────────┘
                        │
               Maintenance
                        │
                Fuel & Expenses
                        │
                   Analytics
                        │
                  Odoo Backend
```

The module uses its own transport data model and does not depend on Odoo's native `fleet` module, giving the project direct control over its models, workflows and UI.

## Data Model

The core entities include:

* Users
* Vehicles
* Drivers
* Trips
* Maintenance Logs
* Fuel Logs
* Expenses

These correspond to the expected database entities specified in the problem statement.

## Project Structure

```text
ODDOO-HACKATHON/
│
├── transitops/
│   ├── data/
│   ├── models/
│   │   ├── transitops_driver.py
│   │   ├── transitops_expense.py
│   │   ├── transitops_fuel_log.py
│   │   ├── transitops_maintenance.py
│   │   ├── transitops_trip.py
│   │   └── transitops_vehicle.py
│   │
│   ├── security/
│   ├── static/
│   │   └── src/
│   │       └── css/
│   │
│   ├── views/
│   ├── __init__.py
│   └── __manifest__.py
│
└── README.md
```

The repository contains dedicated Odoo models for vehicles, drivers, trips, maintenance, fuel logs and expenses.

## Technology Stack

* **ERP Platform:** Odoo 17
* **Backend:** Python
* **Database:** PostgreSQL
* **Frontend:** Odoo Web Framework
* **UI Styling:** CSS
* **Access Control:** Odoo Security & RBAC
* **Module Architecture:** Custom Odoo Add-on

The module is configured as an Odoo 17 application and uses Odoo's `base`, `mail`, `board`, and `web` modules.

## Installation

### Prerequisites

* Odoo 17
* PostgreSQL
* Python 3
* Git

### Clone the Repository

```bash
git clone https://github.com/Vishwaruban-S/ODDOO-HACKATHON.git
cd ODDOO-HACKATHON
```

### Install the Module

Copy the `transitops` directory into your Odoo custom addons directory.

Then:

1. Start the Odoo server.
2. Enable Developer Mode.
3. Update the Apps List.
4. Search for `TransitOps`.
5. Install the module.
6. Configure users and roles.
7. Start adding vehicles, drivers and trips.

## Expected Workflow

The system follows the operational flow defined in the problem statement:

```text
Vehicle Registration
        ↓
Driver Registration
        ↓
Trip Creation
        ↓
Validation
        ↓
Dispatch
        ↓
Active Trip
        ↓
Trip Completion
        ↓
Fuel & Expense Recording
        ↓
Maintenance
        ↓
Reports & Analytics
```

## Hackathon Requirements Covered

* Responsive transport operations interface
* Authentication and role-based access
* Vehicle and driver CRUD
* Trip management with validations
* Automatic status transitions
* Maintenance workflow
* Fuel and expense tracking
* Dashboard KPIs
* Charts and analytics
* Search, filtering and sorting

The problem statement also lists PDF export, license-expiry email reminders, vehicle document management and dark mode as additional deliverables/bonus functionality.

## Future Enhancements

Potential extensions include:

* Predictive maintenance
* Advanced route optimization
* Real-time vehicle tracking
* Automated driver compliance alerts
* Advanced profitability analytics
* Mobile application
* Integration with GPS and telematics systems
* AI-assisted dispatch planning

## Hackathon

**Problem Statement:** PS-02 — TransitOps: Smart Transport Operations Platform

**Event:** Odoo Hackathon

**Project:** TransitOps

## Repository

[GitHub Repository](https://github.com/Vishwaruban-S/ODDOO-HACKATHON)

## License

This project is licensed under the **LGPL-3** license.
