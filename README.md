# Groflex Synthetic Data Engine

Generates realistic synthetic datasets for FinTech, Medical, Retail, and SaaS domains using three generation modes.

# Groflex Synthetic Data Engine

## Overview
Groflex Synthetic Data Engine is a full-stack platform for generating realistic synthetic datasets across multiple domains (FinTech, Medical, Retail, SaaS). It features a modern web frontend and a robust FastAPI backend, enabling users to configure, generate, analyze, and download synthetic data for simulation, testing, and analytics.

---

## Architecture

- **Frontend**: HTML/CSS/JS (in `frontend/`)
  - User interface for configuring data generation, uploading samples, and downloading results.
- **Backend**: FastAPI (in `app/`)
  - REST API for data generation, schema management, validation, and quality reporting.
- **Schemas**: JSON Schema files (in `schemas/`)
  - Define the structure of entities for each domain.

---

## End-to-End Workflow

### 1. User Interaction (Frontend)
- User opens `frontend/index.html` in a browser.
- Selects a **domain** (e.g., FinTech, Medical).
- Chooses an **entity schema** (e.g., Customer, Account).
- Sets **generation parameters**:
  - Generation mode (Neural Matrix, Mimic Buffer, Augmented Set)
  - Volume (number of records)
  - Optional: Seed, Business Profile, or uploads a sample file
- Clicks **"Execute Neural Synthesis"** to start data generation.

### 2. API Request (Frontend → Backend)
- The frontend sends a POST request to the backend endpoint:
  - `POST /api/synthetic/generate`
- Payload includes all user-selected parameters.

### 3. Data Generation (Backend)
- **Request Validation**: Backend validates the request using Pydantic models.
- **Schema Loading**: Loads the appropriate JSON schema for the selected domain/entity.
- **Generation Modes**:
  - **Neural Matrix**: Generates new data from scratch using business logic and optional profile hints.
  - **Mimic Buffer**: Learns statistical distribution from an uploaded sample (CSV/JSON) and mimics it.
  - **Augmented Set**: Extends the uploaded sample with additional rows and logic.
- **Data Generation**: The selected generator class creates the requested number of records, ensuring schema compliance.
- **Quality Reporting**: The `QualityReporter` analyzes the generated data for schema compliance, null rates, uniqueness, and (if applicable) distribution similarity.
- **Response**: Backend returns the generated data, schema, and quality report as JSON.

### 4. Results & Download (Frontend)
- The frontend receives the response and displays:
  - Quality metrics (record count, compliance, uniqueness, etc.)
  - Preview table of generated data
- User can download the data as JSON or CSV.

---

## Key Components

### Backend (app/)
- `main.py`: FastAPI app, API endpoints, static file serving
- `models.py`: Pydantic models for requests/responses
- `generators/`: Data generation logic for each mode
- `quality/`: Quality reporting logic
- `validators/`: Schema validation logic
- `schema_loader.py`: Loads and lists schemas

### Frontend (frontend/)
- `index.html`: Main UI, includes all logic and styles

### Schemas (schemas/)
- Domain/entity JSON schemas (e.g., `schemas/fintech/customer.json`)

---

## Running the Project

### 1. Install Dependencies
```bash
# In project root
<your_venv_path>/Scripts/activate  # Activate your Python virtual environment
pip install -r requirements.txt
```

### 2. Start the Backend
```bash
# From project root
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

### 3. Open the Frontend
- Open `frontend/index.html` in your browser.
- (If running on a remote server, ensure CORS and port 8000 are accessible.)

---

## API Endpoints
- `POST /api/synthetic/generate` — Generate synthetic data
- `GET /api/synthetic/schemas/{domain}` — List available schemas
- `POST /api/synthetic/validate` — Validate records
- `POST /api/synthetic/preview` — Preview data
- `GET /api/health` — Health check

---

## How Data Generation Works
1. **User configures parameters in the UI**
2. **Frontend sends a request to the backend**
3. **Backend loads schema and generates data using the selected mode**
4. **Quality report is generated**
5. **Frontend displays results and allows download**

---

## Extending the Project
- **Add new schemas**: Place new JSON schema files in the appropriate `schemas/{domain}/` folder.
- **Add new generation logic**: Implement a new generator class in `app/generators/` and update the backend logic.
- **Customize frontend**: Edit `frontend/index.html` for UI/UX changes.

---

## Troubleshooting
- Ensure the backend is running on port 8000 before using the frontend.
- Check browser console and backend logs for errors.
- For CORS issues, verify FastAPI CORS middleware settings.

---

## Credits
- Built with FastAPI, Pydantic, and Uvicorn (backend)
- HTML/CSS/JS (frontend)
- Designed for extensibility and realistic synthetic data generation

---

## Contact
For questions or contributions, contact the project maintainer or open an issue in your team repository.
├── tests/
│   └── test_engine.py             # Unit tests
└── requirements.txt               # Python dependencies
```

## Performance

- **Volume**: 5-250 records per request
- **Speed**: ~50ms for NEURAL_MATRIX, ~100ms for AUGMENTED_SET
- **Memory**: Efficient pandas operations with streaming validation
- **Determinism**: Seed-based reproduction for all modes


