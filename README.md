# AI SmartOps – AI-Powered IoT Institute & Office Automation Platform

Observe → Analyze → Recommend → Approve → Act → Verify

## Phase 1 status
| Service | Tech | URL |
|---|---|---|
| client | React + Vite + Bootstrap + Recharts + Socket.io-client | http://localhost:5173 |
| server | Node + Express + Mongoose + Socket.io + MQTT | http://localhost:5000 |
| ai-service | Python 3.11 + FastAPI | http://localhost:8000 |

## Prerequisites
Node 18+, Python 3.11+, MongoDB (local or Atlas). Optional: Mosquitto for MQTT.

## Setup
```bash
# server
cd server && cp .env.example .env && npm install
# client
cd ../client && cp .env.example .env && npm install
# ai-service
cd ../ai-service && python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt && cp .env.example .env
```

## Run separately (3 terminals)
```bash
cd server && npm run dev                          # :5000
cd client && npm run dev                          # :5173
cd ai-service && uvicorn main:app --reload --port 8000
```

## Run all at once
```bash
npm install          # in project root (installs concurrently)
npm run dev          # activate the Python venv first
```

## Test
```bash
curl http://localhost:5000/            # {"success":true,"message":"AI SmartOps Server Running"}
curl http://localhost:5000/api/health  # db / mqtt / aiService status
curl http://localhost:8000/            # {"success":true,"message":"AI SmartOps AI Service Running"}
```
Open http://localhost:5173 – four status cards should be green (MQTT is "disabled" until Phase 4).
FastAPI docs: http://localhost:8000/docs

## Common errors
- `MONGO_URI is not defined` → copy `.env.example` to `.env`.
- `ECONNREFUSED 127.0.0.1:27017` → start MongoDB (`mongod` / service) or use Atlas URI.
- CORS error in browser → `CLIENT_URL` in server/.env must be `http://localhost:5173`.
- `EADDRINUSE :5000` → another process uses the port; change `PORT` or kill it.
- `uvicorn: command not found` → activate the venv.
- AI Service shows offline → start ai-service; check `AI_SERVICE_URL`.

## Phase 2 – Auth & RBAC
See docs/PHASE-2.md. After `npm install` in server, add `JWT_SECRET` to `.env` and run `npm run seed:admin`, then sign in at http://localhost:5173/login.

## Phase 3 – Academics
See docs/PHASE-3.md. Run `npm run seed:demo` in server for demo data.

## Next: Phase 4
IoT – ESP32, MQTT, devices, telemetry, RFID attendance.
