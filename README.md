# ISP Customer Tracking System

Menu‑driven Python application that manages ISP customers, their service locations, subscribed services, installed equipment, and billing status—all backed by a normalized PostgreSQL database.

## 📒 Table of Contents
1. [Project Overview](#project-overview)  
2. [Architecture](#architecture)  
3. [Database Design](#database-design)  
4. [Repository Layout](#repository-layout)  
5. [Prerequisites](#prerequisites)  
6. [Installation & Setup](#installation--setup)  

---

### Project Overview
ISPs require granular visibility into multi‑site customers, active services, hardware, and overdue balances.  
This project delivers a lightweight CLI that lets administrators:

* Add / update / delete **customers, locations, services, equipment, billing records**  
* Query customers with overdue payments  
* View all account details in a structured report  

The system separates **user interface** (`main.py`) from **data access** (`database.py`) for maintainability.

---

### Architecture
| Layer | Responsibility | Key Files |
|-------|----------------|-----------|
| **Database** | SQL tables & relationships | Postgres schema auto‑created by `database.py` |
| **Data Access** | CRUD helpers, connection pooling | `database.py` |
| **Controller / UI** | Menu prompts, input validation | `main.py` |

---

### Database Design
| Table | Core Fields | Relationships |
|-------|-------------|---------------|
| **Customers** | `customer_id` PK · `name` · `phone_number` | 1 —┐ |
| **Locations** | `location_id` PK · FK `customer_id` · `address` | ├ many‑to‑1 |
| **Services** | `service_id` PK · FK `location_id` · `service_name` · `cost` | ├ many‑to‑1 |
| **Equipment** | `equipment_id` PK · FK `location_id` · `equipment_name` · `cost` | └ many‑to‑1 |
| **Billing** | `billing_id` PK · FK `customer_id` · `amount_due` · `last_payment_date` · `is_late` | many‑to‑1 → Customers |

The schema is **fully normalized** to avoid redundancy and ensure referential integrity.

---


---

### Prerequisites
* **Python 3.9+**  
* **PostgreSQL 14+**  
* Database user with `CREATE` privileges  

---

### Installation & Setup

# Clone repository
git clone https://github.com/<your‑org>/isp-customer-tracker.git
cd isp-customer-tracker

# Create virtual environment
python -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -r requirements.txt   # psycopg2-binary, tabulate (optional)

# Configure DB connection
export DATABASE_URI="postgresql://user:password@localhost:5432/ispdb"
# (Alternatively, store this in a .env file and modify database.py to read it.)

# Launch application (tables auto‑create on first run)
python main.py

## 🖥️ CLI Usage
> Navigate menus with digits + Enter.

| Main Menu Option | Sub‑actions |
|------------------|-------------|
| **Manage Customers** | add · view · delete |
| **Manage Locations** | add location to customer |
| **Manage Services** | add service to location · update cost |
| **Manage Equipment** | add equipment to location |
| **Billing** | view customers late on payments |
| **Reporting** | view all customers with full details |

All operations print success/failure messages; input validation guards against bad IDs.

---

## 🌟 Features & Highlights
* **Connection Pooling** for low‑latency database access  
* **Atomic Queries** with rollback on errors  
* **Hierarchical Data Model** (Customer > Location > Service / Equipment)  
* **Late‑Payment Report** surfaces delinquent accounts instantly  
* **Clear Separation of Concerns** between UI and data layer—easy to extend  

---

## 🛣️ Roadmap
1. **Unit Tests** (pytest) for CRUD logic  
2. **CSV Import** to bulk‑load customers and locations  
3. **Role‑Based Permissions** (read‑only vs. admin)  
4. **Web Dashboard** using FastAPI + React  
5. **Docker Compose** for one‑command Postgres + app deployment  

---

## 🤝 Contributing
Issues and pull requests are welcome! For major changes (schema updates, new modules), please open an issue first to discuss scope.

---

## 📜 License
Released under the MIT License – see `LICENSE` for details.
