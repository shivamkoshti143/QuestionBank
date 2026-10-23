# Government Hierarchical Assessment & Monitoring Portal (GHAMP)

A production-oriented, secure web application for government-style administrative assessment, central question bank lifecycle management, transparent scoring, and multi-tier upward rollup reporting.

---

## 🏛️ System Architecture & Technology Stack

- **Frontend**: React 18, TypeScript, Vite, Material UI (MUI), React Router v6, Axios, Recharts, Custom SVG Speedometer/Gauges.
- **Backend**: NestJS, TypeScript, TypeORM, MariaDB, Passport JWT, RBAC & Hierarchy Scope Guards, Swagger / OpenAPI at `/api/docs`.
- **Database**: MariaDB (`questionbank` database on existing XAMPP environment port 3306), 19 normalized 3NF tables with foreign keys and indexes.

---

## 👑 Administrative Hierarchy & Access Scope

```
Super Admin (National Jurisdiction)
  └── State (e.g. Gujarat, Maharashtra)
        └── Division (e.g. Ahmedabad Division, Vadodara Division)
              └── Block / Taluka (e.g. Daskroi Taluka, Sanand Taluka)
                    └── Gram Panchayat (e.g. Kasindra GP, Bareja GP)
```

Each administrative tier has distinct, strictly enforced scope boundaries:
- **Super Admin**: Global access across all tiers, uploads central question banks, publishes assessment cycles, and views national drill-downs.
- **State Admin**: Access restricted to assigned State and descendant units.
- **Division Admin**: Access restricted to assigned Division and descendant units.
- **Block Admin**: Access restricted to assigned Block and descendant Gram Panchayats.
- **Gram Panchayat User**: Access restricted to assigned village; completes questionnaires, saves drafts, and submits final returns.

---

## 🔑 Pre-Seeded Test Credentials

| Role | Username | Password | Assigned Administrative Unit | State of Assessment |
|---|---|---|---|---|
| **Super Admin** | `superadmin` | `SuperAdmin@2026!` | National Oversight (All Units) | National Rollup Dashboard |
| **State Admin** | `state_admin_gj` | `Admin@2026!` | Gujarat State | Gujarat State Rollup |
| **Division Admin** | `div_admin_ahm` | `Admin@2026!` | Ahmedabad Division | Division Rollup |
| **Block Admin** | `block_admin_dsk` | `Admin@2026!` | Daskroi Taluka | Block Rollup |
| **Gram Panchayat User** | `gp_kasindra` | `Admin@2026!` | Kasindra Gram Panchayat | **Pending** (Open to answer questionnaire) |
| **Gram Panchayat User** | `gp_bareja` | `Admin@2026!` | Bareja Gram Panchayat | **Submitted** (Score: 88.5%, Verified return) |

*A Quick Account Selector is available on the Login screen for instant testing of every role.*

---

## 🚀 Running the Application

### 1. Database & Services
The application utilizes the existing XAMPP MariaDB service on port `3306`:
```powershell
# MariaDB is running on localhost:3306 with database: questionbank
```

### 2. Backend Service (Port 4000)
```powershell
cd d:\QuestionBank\backend
npm install
npm run seed      # Initializes MariaDB tables and seeds initial data
npm run start:dev # Runs NestJS backend on http://localhost:4000
```
- Swagger API Docs: `http://localhost:4000/api/docs`

### 3. Frontend Service (Port 5173)
```powershell
cd d:\QuestionBank\frontend
npm install
npm run dev       # Runs Vite development server on http://localhost:5173
```
- Open browser at `http://localhost:5173/login`

---

## 📊 Core Features Implemented

1. **JWT Authentication & RBAC**: Access & Refresh token rotation, session revocation, login rate limiting, and hierarchy scope verification.
2. **Central Question Bank**:
   - Supports Single Choice, Multiple Select, Yes/No, True/False, Percentage, Numeric, Rating Scale, and Qualitative Text.
   - Excel (`.xlsx`) & CSV batch importer with row-by-row validation, duplicate code detection, and error diagnostics report.
   - Versioning and publication freezing.
3. **Assessment Cycle Management**:
   - Scope targeting (National ALL, State, Division, Block, or specific GPs).
   - Question snapshotting at publication (historical returns are immutable).
   - Automatic assignment resolution to eligible Gram Panchayats.
4. **Gram Panchayat Questionnaire**:
   - Question stepper navigation with progress indicator.
   - Draft saving & autosave with status timestamp.
   - Review answers summary modal.
   - Final submission verification producing unique Reference Number.
5. **Transparent Scoring Engine**:
   - Weighted percentage scoring formula: $\text{Score} = (\sum \text{Earned} \times w_i / \sum \text{Available} \times w_i) \times 100$.
   - Category breakdowns (Sanitation, Water Supply, Governance, Finance, Infrastructure).
   - Automated Performance Band classification (Excellent $\ge 80\%$, Good $60-80\%$, Average $40-60\%$, Critical $< 40\%$).
   - Missing answers in drafts do not count as zero performance.
6. **Upward Aggregation & Rollup Dashboards**:
   - Gram Panchayat $\rightarrow$ Block $\rightarrow$ Division $\rightarrow$ State $\rightarrow$ Super Admin.
   - Custom SVG Speedometer/Gauge indicators with threshold bands.
   - Recharts visual charts: Comparison Bar Charts, Status Donut Charts, Category Performance Horizontal Bars, Stacked Progress Bars.
   - Detailed tabular drill-down.
7. **Report Exports**:
   - Official multi-sheet Excel spreadsheet (`.xlsx`) with formula injection protection.
   - Formal e-governance PDF report (`.pdf`) with government header and comparison summaries.
8. **Compliance & Audit**:
   - Immutable audit trail viewer for all sensitive administrative actions.
   - In-app notification center.
   - System settings configuration.

---

## 📚 Technical Documentation

- [System Architecture](docs/architecture.md)
- [Database ER Diagram](docs/database-er-diagram.md)
- [REST API Reference](docs/api-documentation.md)
- [Production Deployment Guide](docs/deployment.md)
