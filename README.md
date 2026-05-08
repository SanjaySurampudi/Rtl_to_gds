# RTL2GDS · OpenLane Flow

> A modern web-based interface to run full RTL → GDSII physical design flows using OpenLane — from your browser, on any device.

---

## What is this?

**RTL2GDS** is a self-hosted web application that lets you submit any Verilog design and automatically run the complete OpenLane ASIC physical design flow — synthesis, floorplanning, placement, routing, and GDSII generation — all from a clean, terminal-style UI.

No command line needed. Just paste your Verilog and click **Run**.

---

## Features

- **Browser-based UI** — works on desktop and mobile
- **JWT Authentication** — secure login with token-based sessions
- **Live Terminal Output** — real-time log streaming with color-coded messages
- **Progress Tracking** — step-by-step progress bar (42 OpenLane steps)
- **Job History** — view and revisit all past runs
- **Auto file generation** — Verilog `.v` and `config.json` written automatically per job
- **Quick Examples** — XOR, AND, 2:1 MUX, Counter preloaded
- **ngrok ready** — expose locally over the internet instantly

---

## Project Structure

```
rtl2gds/
├── app.py                  # Flask backend
├── frontend/
│   └── index.html          # Single-page frontend UI
└── runs/
    └── <job_id>/
        ├── config.json     # Auto-generated OpenLane config
        └── src/
            └── design.v    # Auto-saved Verilog file
```

---

## Prerequisites

- Python 3.8+
- Flask
- PyJWT
- OpenLane 2
- ngrok (optional, for remote access)

---

## Installation

**1. Clone or set up the project:**
```bash
mkdir ~/rtl2gds && cd ~/rtl2gds
mkdir -p frontend runs
```

**2. Install Python dependencies:**
```bash
pip3 install flask PyJWT --break-system-packages
```

**3. Install OpenLane:**
```bash
pip3 install openlane --break-system-packages
```

**4. Add your files:**
- Place `app.py` in `~/rtl2gds/`
- Place `index.html` in `~/rtl2gds/frontend/`

---

## Running the App

**Start the Flask server:**
```bash
cd ~/rtl2gds
python3 app.py
```

You should see:
```
Users: ['admin', 'sanju']
Starting on http://localhost:5000
```

**Expose with ngrok (optional):**
```bash
ngrok http 5000
```

Open the ngrok URL on any device and log in.

---

## Default Login Credentials

| Username | Password  |
|----------|-----------|
| admin    | admin123  |
| sanju    | sanju123  |

> Change these in the `USERS` dict inside `app.py` before deploying.

---

## How It Works

```
User submits Verilog
        ↓
Flask saves .v file + config.json
        ↓
OpenLane runs in background thread
        ↓
Frontend polls /api/status every 1.5s
        ↓
Live logs streamed to terminal UI
        ↓
GDSII output generated on success
```

---

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/login` | Authenticate, get JWT token |
| POST | `/api/run` | Submit a new design job |
| GET | `/api/status/<job_id>` | Poll job status and logs |
| GET | `/api/jobs` | List all jobs |

All endpoints except `/api/login` require `Authorization: Bearer <token>` header.

---

## Supported Verilog Examples

| Design | Description |
|--------|-------------|
| XOR Gate | Simple combinational 2-input XOR |
| AND Gate | Simple combinational 2-input AND |
| 2:1 MUX | Select between two inputs |
| Counter | 4-bit synchronous up-counter with reset |

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | Vanilla HTML/CSS/JS |
| Backend | Python · Flask |
| Auth | JWT (PyJWT) |
| EDA Flow | OpenLane 2 |
| Fonts | JetBrains Mono · Syne |
| Tunnel | ngrok |

---

## Troubleshooting

**"Server unreachable" on login:**
- Make sure `python3 app.py` is running
- Make sure ngrok is pointing to port 5000
- Check port is free: `sudo fuser -k 5000/tcp`

**"No module named openlane":**
```bash
pip3 install openlane --break-system-packages
```

**Port already in use:**
```bash
sudo fuser -k 5000/tcp
python3 app.py
```

**Login screen not showing (cached token):**
Open browser console and run:
```javascript
localStorage.clear(); location.reload();
```

---

## License

MIT License — free to use, modify, and distribute.

---

<div align="center">
  Built with love for open-source chip design
</div>