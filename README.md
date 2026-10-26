# ConstructAI — AI-Powered Multimodal Construction Intelligence Platform

ConstructAI is an enterprise-grade full-stack multimodal intelligence platform engineered for civil engineering, construction site supervision, and structural forensics. It combines a high-performance **Python + FastAPI** backend with a modern **React + Vite + Tailwind CSS** frontend, powered by multimodal AI (Google Gemini 2.5 Flash / 1.5 Flash) and domain knowledge heuristics.

---

## Architecture Overview

```
                                      ┌─────────────────────────────────────────────────────────┐
                                      │                     Browser Client                      │
                                      │             React.js + Vite + Tailwind CSS              │
                                      │                   (Port: 5173)                          │
                                      └────────────────────────────▲────────────────────────────┘
                                                                   │
                                                      HTTP REST / Multipart Uploads
                                                      (Images + JSON Payloads)
                                                                   │
                                      ┌────────────────────────────▼────────────────────────────┐
                                      │                   FastAPI Backend Server                │
                                      │                 Python 3.11+ / Uvicorn                  │
                                      │                   (Port: 8000)                          │
                                      ├─────────────────────────────────────────────────────────┤
                                      │  • Pydantic Strict JSON Schema Validation               │
                                      │  • Pillow Image Integrity & Dimension Validation        │
                                      │  • SQLAlchemy ORM (SQLite / PostgreSQL Ready)           │
                                      │  • Dual AI Engine:                                      │
                                      │      - Google Gemini 2.5 Flash Multimodal Vision        │
                                      │      - Zero-Config Intelligent Domain Simulator         │
                                      │  • Open-Meteo Standards-Based Weather Advisory          │
                                      └────────────────────────────▲────────────────────────────┘
                                                                   │
                                      ┌────────────────────────────▼────────────────────────────┐
                                      │             Google Gemini Multimodal API                │
                                      │          (Server-to-Server Only — No Leaks)             │
                                      └─────────────────────────────────────────────────────────┘
```

---

## Features & Navigation Modules

1. **Dashboard (`/`)**: Real-time project overview, KPI metrics, cumulative capital expenditure charts, safety hazard distribution pie chart, and weather summary.
2. **Safety Analyzer (`/safety`)**: Upload site photos or click the built-in sample to triage PPE non-compliance, missing perimeter guardrails, protruding rebar dowels, and OSHA 1926 reference codes.
3. **Crack Detection & Structural Triage (`/cracks`)**: Visual triage for concrete beams, columns, and slabs. Classifies diagonal shear vs. shrinkage cracks, evaluates scale-calibrated width, and identifies hypotheses and maintenance remedies.
4. **Cost & Material Estimator (`/estimation`)**: Transparent quantity surveying formulas (cement, sand, aggregate, steel rebar, masonry, concrete volume). Features editable unit rates and cost breakdown charts.
5. **Planning & Gantt Schedule (`/planning`)**: Project milestone tracker with task dependency configurations, live progress sliders, and an AI-assisted schedule proposal generator.
6. **Real Estate Valuation (`/valuation`)**: Parametric property valuation incorporating micro-market benchmarks, structural quality tiers, straight-line age depreciation, and amenities premiums.
7. **Daily Site Assistant (`/assistant`)**: Interactive field copilot with quick prompt chips and an automated formal Daily Site Report generator that archives records to the database.
8. **Weather Advisory (`/weather`)**: Real-time meteorological intelligence evaluating OSHA crane wind velocity thresholds (> 38 km/h), rain risk on concrete placement, and ACI 305R hot weather curing protocols.
9. **Knowledge Assistant (`/knowledge`)**: Grounded knowledge repository covering concrete technology, structural cover requirements, formwork stripping times, and OSHA safety standards.
10. **Reports & Analytics (`/reports`)**: Consolidated compliance dossier with severity filters and print-to-PDF layout with formal engineering sign-off blocks.
11. **Settings & AI Status (`/settings`)**: System diagnostics, active AI engine telemetry, database connection verification, and Gemini API key configuration guide.
12. **Authentication (`/login`)**: Secure JWT login with 1-click instant demo mode for project evaluation.

---

## Prerequisites

Before running the application on Windows, verify that you have:
- **Python 3.9+** (Tested with Python 3.11.9)
- **Node.js 18+** (Tested with Node.js v24.11.0)
- **npm** (Tested with npm 11.6.1)
- Windows PowerShell

---

## Step-by-Step Installation & Run Guide (Windows PowerShell)

Open **two separate PowerShell terminals** in the project root directory:
`c:\Users\nakul\Downloads\SAC-main\Multimodal Construction Intelligence Platform`

### Terminal 1: Backend (Python + FastAPI)

1. Navigate to the backend directory:
   ```powershell
   cd backend
   ```

2. Create and activate a Python virtual environment:
   ```powershell
   python -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```
   *(If PowerShell gives an execution policy error, run: `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass`)*

3. Install required dependencies:
   ```powershell
   pip install -r requirements.txt
   ```

4. *(Optional)* Configure your Google Gemini API key:
   - Edit `backend/.env`:
     ```env
     GEMINI_API_KEY=your_actual_api_key_here
     GEMINI_MODEL=gemini-2.5-flash
     ```
   - **Note**: If left blank, ConstructAI automatically runs in **Intelligent Domain Simulator Mode** with zero crashes, allowing full offline testing!

5. Run automated tests to verify backend integrity:
   ```powershell
   pytest tests/
   ```

6. Start the FastAPI backend server:
   ```powershell
   python -m uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
   ```
   The backend will be live at:
   - API Root: `http://127.0.0.1:8000`
   - Swagger Documentation: `http://127.0.0.1:8000/api/docs`
   - Health Check: `http://127.0.0.1:8000/api/health`

---

### Terminal 2: Frontend (React.js + Vite)

1. Navigate to the frontend directory:
   ```powershell
   cd frontend
   ```

2. Install npm dependencies:
   ```powershell
   npm install
   ```

3. Start the Vite development server:
   ```powershell
   npm run dev
   ```

4. Open your web browser at:
   `http://localhost:5173`

---

## Automated Verification & Test Commands

You can run the built-in test suite to confirm all schemas, services, and API contracts:

```powershell
cd backend
pytest tests/ -v
```

To test the frontend production build:
```powershell
cd frontend
npm run build
```

---

## Security & Reliability Design

- **Zero Secret Exposure**: The frontend never receives, stores, or handles the `GEMINI_API_KEY`. All multimodal inference calls are executed server-to-server from FastAPI.
- **Strict Envelope Contracts**: Every API endpoint responds with the validated contract `{ "success": true, "data": {...}, "message": "...", "request_id": "..." }`.
- **Image Validation**: Uploaded files are inspected for MIME type and binary integrity via Pillow, rejecting files over 10MB or corrupted inputs.
- **Fail-Safe Fallbacks**: If the AI API experiences quota limits or network downtime, the system automatically transitions to the domain knowledge simulator without interrupting user workflow.
