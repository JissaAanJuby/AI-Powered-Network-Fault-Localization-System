# Cross-Layer Fault Localisation System

An AI-driven full-stack application for detecting and localising network faults across multiple network layers. The system combines a React dashboard with a FastAPI backend that analyses telemetry such as latency, packet loss, jitter, optical power, CRC errors, CPU usage, SNMP status, and hop position to identify likely fault categories and affected network segments.

## Features

- Cross-layer telemetry input for network fault diagnosis.
- FastAPI backend with `/api/diagnose` prediction endpoint.
- React + Vite frontend for interactive fault analysis.
- Sector-aware thresholds for Industries, Public, and Household networks.
- Fault classification for healthy links, fiber cuts, OLT failures, congestion, and MPLS transport failures.
- Local and LAN-ready development setup.

## Tech Stack

- Frontend: React, TypeScript, Vite, Tailwind CSS
- Backend: Python, FastAPI, Uvicorn, Pydantic
- Data/ML assets: CSV datasets and notebook-based experimentation in `backend`

## Project Structure

```text
.
├── backend/
│   ├── api.py              # FastAPI application
│   ├── backend.py          # ML/backend experimentation code
│   ├── backend.ipynb       # Notebook workflow
│   └── *.csv               # Sample datasets
├── frontend/
│   ├── src/                # React application source
│   ├── package.json        # Frontend scripts and dependencies
│   └── vite.config.ts      # Vite config and API proxy
├── start.ps1               # Windows helper script
└── README.md
```

## Prerequisites

- Node.js 16 or newer
- Python 3.9 or newer
- pip

## Setup

Install backend dependencies:

```bash
cd backend
python3 -m pip install fastapi uvicorn pydantic pandas numpy torch lightning pytorch_forecasting scikit-learn
```

Install frontend dependencies:

```bash
cd frontend
npm install
```

## Run Locally

Start the backend:

```bash
cd backend
python3 -m uvicorn api:app --reload --host 0.0.0.0 --port 8000
```

Start the frontend in another terminal:

```bash
cd frontend
npm run dev
```

Open the app:

```text
http://localhost:3000
```

The frontend proxies `/api` requests to the backend at `http://127.0.0.1:8000`.

## LAN Access

The frontend dev script binds to `0.0.0.0`, so devices on the same LAN can open:

```text
http://YOUR_LAN_IP:3000
```

The backend API docs are available at:

```text
http://YOUR_LAN_IP:8000/docs
```

Replace `YOUR_LAN_IP` with the host machine's local network IP address.

## API Endpoint

`POST /api/diagnose`

Example payload:

```json
{
  "sector": "Industries",
  "lat": 120,
  "loss": 4,
  "jitter": 12,
  "opt": -24,
  "crc": 2,
  "status": 1,
  "cpu": 78,
  "snmp": 1,
  "hop": 4
}
```

The response includes the predicted fault, fault category, confidence percentage, and isolated node or segment.
