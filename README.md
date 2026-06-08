<div align="center">
  <h1>🏔️ MineSafe</h1>
  <p><b>AI-Powered Geotechnical Stability & Rockfall Intelligence</b></p>
  <p><i>"Bridging Physics and Machine Learning to Protect Lives Under the Earth"</i></p>

  <p>
    <img src="https://img.shields.io/badge/React-19.0-61DAFB?logo=react&logoColor=black&style=for-the-badge" alt="React" />
    <img src="https://img.shields.io/badge/Vite-7.0-646CFF?logo=vite&logoColor=white&style=for-the-badge" alt="Vite" />
    <img src="https://img.shields.io/badge/Python-3.9+-3776AB?logo=python&logoColor=white&style=for-the-badge" alt="Python" />
    <img src="https://img.shields.io/badge/Flask-3.0-000000?logo=flask&logoColor=white&style=for-the-badge" alt="Flask" />
    <img src="https://img.shields.io/badge/Express-5.0-000000?logo=express&logoColor=white&style=for-the-badge" alt="Express" />
    <img src="https://img.shields.io/badge/Pandas-2.0-150458?logo=pandas&logoColor=white&style=for-the-badge" alt="Pandas" />
    <img src="https://img.shields.io/badge/Scikit--Learn-1.3-F7931E?logo=scikit-learn&logoColor=white&style=for-the-badge" alt="Scikit-Learn" />
  </p>
</div>

<br />

**MineSafe** is an enterprise-grade Geotechnical Slope Stability Monitoring & Early Warning System designed for open-cast mines. In remote mines, internet connectivity is scarce and lives depend on zero-latency local calculations. MineSafe integrates real-time IoT telemetry, machine learning predictions, and physics-based engineering calculations into a high-performance offline-first dashboard.

---

## ⚡ Core Engineering Philosophy: Hybrid AI + Physics

Typical machine learning models operate as "black boxes" which safety managers can be hesitant to trust. MineSafe solves this by implementing a **Hybrid AI + Physics Architecture**:

```
 ┌────────────────────────────────────────────────────────┐
 │                      INPUT DATA                        │
 │     (Rock Type, Rainfall, Slope Angle, Moisture...)    │
 └───────────┬────────────────────────────────┬───────────┘
             │                                │
             ▼                                ▼
┌─────────────────────────┐      ┌────────────────────────┐
│  Scikit-Learn Pipeline  │      │  Physics-Based Engine  │
│  (Random Forest Reg)    │      │  (Limit Equilibrium)   │
└────────────┬────────────┘      └───────────┬────────────┘
             │                                │
      Predicts Probability                    Calculates Factor
        & Time-to-Impact                       of Safety (FS)
             │                                │
             └───────────────┬────────────────┘
                             │
                             ▼
             ┌────────────────────────────────┐
             │   AI vs. LEM Trust Evaluator   │
             │   (Dynamic Trust Score %)      │
             └────────────────────────────────┘
```

1. **Machine Learning Pipeline**: Random Forest Regressors predict the **Rockfall Probability (%)** and **Estimated Time-to-Impact (hours)** based on geological features.
2. **Physics-Based Engine**: Calculates the **Factor of Safety (FS)** using the Limit Equilibrium Method (LEM). An $FS < 1.0$ indicates physical instability, while $FS \ge 1.5$ indicates a fully stable slope.
3. **AI vs. LEM Trust Score**: A dynamic heuristic compares the AI's predictions with the physics calculations. If the AI predicts high risk while the physics engine indicates stable geology, the system flags a discrepancy and adjusts the **Trust Score (%)**, ensuring safety managers have reliable warnings.

---

## 🚀 Interactive UI Modules Walkthrough

The frontend is divided into specialized workspaces for different mining safety duties:

### 1. 📊 Telemetry Dashboard (HUD)
- **Mine Risk Visualization**: Dynamic SVG map of India highlighting major mining hubs (Jharkhand, West Bengal, Odisha) colored by real-time risk severity.
- **PPV Ground Vibration Charts**: Visualizes Peak Particle Velocity (PPV) blasting logs in real time.
- **KPI Summary HUD**: Live counters of active sensors, active alerts, and site status indicators.

### 2. 🎛️ Live Sensors Page
- **Multi-Sensor Telemetry**: Visual tracking of key variables: Strain, Temperature, Rainfall, Pore Pressure, Slope Angle, and Vibrations.
- **Real-Time Safety Thresholding**: Color-coded warnings (Success, Warning, Error) flag variables exceeding physical limits:
  - **Strain limit**: $75\mu\epsilon$ (micro-strain)
  - **Rainfall limit**: $100\text{mm}$ (accumulated 24h)
  - **Slope Angle limit**: $60^\circ$ (steepness)
  - **Pore Pressure limit**: $50\text{kPa}$
  - **Vibration limit**: $1.0\text{mm/s}$ (blasting tremor)

### 3. 🚨 Alerts Control Center
- **Alert Logs**: Filterable log of safety incidents (High, Medium, and Low severity) with sorting options by Timestamp, Severity, and Location.
- **Interactive Details Page**: In-depth view of why an alert was triggered, listing variables that exceeded thresholds.
- **Operational Triggers**: Direct controls to "Acknowledge Warning", "Export Incident logs", or "Trigger On-Site Evacuation Sirens" in emergencies.

### 4. 🔮 Inference Center (Prediction & Report Engine)
- **Manual Input Geotechnical Interface**: Paste raw comma-separated values to request instant predictions from the Flask ML server.
- **Automated PDF Auditing**: A built-in reporting layout compiles current weather data, live telemetry metrics, coordinates, and ML results, exporting a formatted PDF audit report in a single click (powered by `html2canvas` and `jspdf`).

### 5. 🔬 What-If Scenario Simulator
- Allows safety experts to virtually test extreme conditions and triggers without real-world risks:
  - **Rainfall Scenario**: Simulates precipitation amount (mm) and duration (hours), modeling impacts on soil moisture and slope stability.
  - **Blasting Scenario**: Simulates proximity (meters) and charge size (kg TNT equivalent) of mine blasting operations.
  - **Temperature Scenario**: Simulates extreme cold and rapid thaw cycles (°C) to evaluate freeze-thaw expansions on rock joints.

---

## 📊 Geotechnical Data Dictionary

Here is the data structure utilized by our inference models and physics calculators:

| Feature Name | Telemetry Unit | Sensor Source | Geotechnical Significance |
| :--- | :--- | :--- | :--- |
| **Rock_Type** | Categorical | Geology Database | Determines baseline cohesion and internal friction angle (Igneous, Metamorphic, Sedimentary). |
| **Rainfall** | mm / 24 hours | Live Piezometer / Weather | Controls pore-water pressure, reducing effective stress in joint planes. |
| **Slope_Angle** | Degrees (°) | Digital Inclinometers | Higher angle values increase shear stress on the failure plane. |
| **NDVI** | -1.0 to 1.0 | Sentinel-2 Satellite | Normalized Difference Vegetation Index: measures slope vegetation cover. |
| **Change_in_NDVI** | Delta ($\Delta$) | Satellite Epoch Comparison | Negative delta flags active surface erosion or rock movement. |
| **Soil_Moisture** | Percentage (%) | TDR Moisture Probes | Correlates with soil shear strength degradation. |
| **Blast_Vibration** | mm/s (PPV) | Seismic Geophones | Peak Particle Velocity: measures structural stress from mine blasting. |
| **Seismic_Vibration** | g (acceleration) | Accelerometer Arrays | Tracks ambient seismic events or tectonic shifts. |

---

## 🧠 Machine Learning & Data Pipeline Details

### 1. Model Selection & Pipelines
- The model uses two **Random Forest Regressors** (`sklearn.ensemble.RandomForestRegressor`):
  - **Probability Pipeline**: Forecasts slope failure probability (0.0 to 1.0).
  - **Time-to-Impact Pipeline**: Forecasts remaining time before a rockfall occurs (in hours).
- **ColumnTransformer Preprocessor**:
  - **Numerical columns** are scaled using `StandardScaler` to handle features with differing units (e.g., Rainfall vs. Slope Angle).
  - **Categorical columns** (`Rock_Type`) are encoded using `OneHotEncoder` to convert geological labels into numeric features.

### 2. Local Excel Database Persistence
- In remote mine environments, external database servers can go offline. MineSafe uses an **Excel-based Local database** (`data/users.xlsx`):
  - **Password Security**: Passwords are encrypted on the backend using the secure `scrypt` hashing algorithm with custom salt protection before storage.
  - **Robust Serializer Handlers**: Implements custom pandas DataFrame-to-Dictionary translators to convert `NaN` float values to JSON-compatible `None`/`null` objects, preventing client-side parsing failures.

---

## 🛠️ Software Design Showcase (For Recruiters)

This repository demonstrates several clean coding patterns and engineering best practices:

- **Isolated API State Handlers**: State changes, API fetch states, and UI rendering logic are fully isolated to prevent React component rendering crashes from masking network exceptions.
- **Stable Loopback Configuration**: Configured to use explicit IPv4 loopback (`127.0.0.1`) addressing rather than `localhost` to bypass IPv6 resolution discrepancies common on Windows workstations.
- **Offline-First Security**: Hybrid auth that secures accounts with standard backend crypt-hashing and falls back to a highly portable spreadsheet database.
- **Custom PDF Engine**: Combines `html2canvas` and `jspdf` to convert DOM states into high-resolution documents, bypassing heavy server-side print servers.

---

## 🚀 Setup & Execution Guide

Ensure you have **Python 3.9+** and **Node.js 18+** installed.

### 1. Clone & Install Dependencies
```bash
# Install root (Frontend) packages
npm install

# Install Express backend gateway packages
cd backend
npm install
cd ..
```

### 2. Install Python ML service dependencies
```bash
pip install pandas scikit-learn Flask openpyxl
```

### 3. Run System Services (Run in separate terminal windows)

#### Step A: Start Flask ML service (Port 5000)
```bash
python ml_service/app.py
```

#### Step B: Start Express Server (Port 5001)
```bash
cd backend
npm start
cd ..
```

#### Step C: Start Vite Frontend Server (Port 5173)
```bash
npm run dev
```

Navigate to: **[http://localhost:5173/SIH/](http://localhost:5173/SIH/)**
