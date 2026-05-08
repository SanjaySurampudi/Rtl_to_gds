<div align="center">

# ⚡ RTL2GDS · OpenLane Flow

### A modern web-based interface for full ASIC physical design flows

[![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Flask](https://img.shields.io/badge/Flask-2.x-000000?style=for-the-badge&logo=flask&logoColor=white)](https://flask.palletsprojects.com)
[![OpenLane](https://img.shields.io/badge/OpenLane-2.x-00D4AA?style=for-the-badge&logo=chip&logoColor=white)](https://openlane2.readthedocs.io)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![WSL2](https://img.shields.io/badge/WSL2-Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)](https://ubuntu.com)

<br/>

> Paste your Verilog. Click **Run**. Get **GDSII**.  
> No terminal. No config headaches. Just chip design.

<br/>

</div>

---

## 📌 What is RTL2GDS?

**RTL2GDS** is a self-hosted web application that lets you submit any Verilog RTL design and automatically run the complete **OpenLane ASIC physical design flow** — synthesis, floorplanning, placement, routing, and GDSII generation — all from a clean, terminal-style browser UI.

```
Verilog Input  →  Synthesis  →  Floorplan  →  Place & Route  →  GDSII Output
   (Paste)         (Yosys)     (OpenROAD)      (OpenROAD)       (Magic/KLayout)
```

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🔐 **JWT Authentication** | Secure token-based login with 8-hour sessions |
| 📡 **Live Log Streaming** | Real-time terminal output polled every 1.5s |
| 📊 **Progress Tracking** | 42-step progress bar matching OpenLane stages |
| 🗂️ **Job History** | View and revisit all past runs in one click |
| 💾 **Auto File Generation** | Verilog `.v` and `config.json` auto-written per job |
| 🧪 **Quick Examples** | XOR, AND, MUX, Counter preloaded with one click |
| 📱 **Mobile Ready** | Fully responsive UI, tested via ngrok on Android |
| 🌐 **ngrok Support** | Expose your local server to any device instantly |

---

## 🗂️ Project Structure

```
rtl2gds/
├── app.py                    # Flask backend + JWT auth
├── frontend/
│   └── index.html            # Single-page browser UI
├── runs/
│   └── <job_id>/
│       ├── config.json       # Auto-generated OpenLane config
│       └── src/
│           └── <design>.v    # Auto-saved Verilog source
└── README.md
```

---

## ⚙️ Prerequisites

- Python 3.8+
- Flask + PyJWT
- OpenLane 2
- ngrok *(optional, for remote access)*

---

## 🚀 Installation

**1. Set up the project:**
```bash
mkdir ~/rtl2gds && cd ~/rtl2gds
mkdir -p frontend runs
```

**2. Install Python dependencies:**
```bash
pip3 install flask PyJWT --break-system-packages
```

**3. Install OpenLane 2:**
```bash
pip3 install openlane --break-system-packages
```

**4. Place your files:**
- `app.py` → `~/rtl2gds/app.py`
- `index.html` → `~/rtl2gds/frontend/index.html`

---

## ▶️ Running the App

**Terminal 1 — Start Flask:**
```bash
cd ~/rtl2gds
python3 app.py
```

Expected output:
```
Users: ['admin', 'sanju']
Starting on http://localhost:5000
 * Serving Flask app 'app'
 * Running on http://127.0.0.1:5000
```

**Terminal 2 — Expose with ngrok (optional):**
```bash
ngrok http 5000
```

Copy the `https://xxxx.ngrok-free.app` URL and open on any device.

---

## 🔑 Default Credentials

> ⚠️ Change these in the `USERS` dict inside `app.py` before any public deployment.

| Username | Password  |
|----------|-----------|
| `admin`  | `admin123` |
| `sanju`  | `sanju123` |

---

## 🔌 API Reference

All endpoints except `/api/login` require:
```
Authorization: Bearer <token>
```

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/login` | Authenticate and receive JWT token |
| `POST` | `/api/run` | Submit Verilog design, start job |
| `GET` | `/api/status/<job_id>` | Poll job status and live logs |
| `GET` | `/api/jobs` | List all jobs |

**Example login request:**
```bash
curl -X POST http://localhost:5000/api/login \
  -H "Content-Type: application/json" \
  -d '{"username": "admin", "password": "admin123"}'
```

**Example run request:**
```bash
curl -X POST http://localhost:5000/api/run \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "design_name": "my_xor",
    "verilog_code": "module my_xor(input a, b, output y); assign y = a ^ b; endmodule",
    "clock_port": null,
    "die_area": "0 0 100 100",
    "fp_core_util": 5,
    "pl_target_density": 0.3
  }'
```

---

## 🧪 Supported Example Designs

| Design | Type | Description |
|--------|------|-------------|
| XOR Gate | Combinational | 2-input XOR |
| AND Gate | Combinational | 2-input AND |
| 2:1 MUX | Combinational | Select between two inputs |
| Counter | Sequential | 4-bit synchronous up-counter with reset |

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | Vanilla HTML · CSS · JavaScript |
| Backend | Python · Flask |
| Auth | JWT (PyJWT) · Bearer tokens |
| EDA Tool | OpenLane 2 · Yosys · OpenROAD |
| Fonts | JetBrains Mono · Syne (Google Fonts) |
| Tunnel | ngrok |
| Platform | WSL2 · Ubuntu |

---

## 🐛 Troubleshooting

<details>
<summary><b>❌ "Server unreachable" on login</b></summary>

- Make sure Flask is running: `python3 app.py`
- Make sure ngrok is running: `ngrok http 5000`
- Both must run at the same time in separate terminals
- Check port: `sudo fuser -k 5000/tcp`

</details>

<details>
<summary><b>❌ "No module named openlane"</b></summary>

```bash
pip3 install openlane --break-system-packages
python3 -m openlane --version
```

</details>

<details>
<summary><b>❌ Port 5000 already in use</b></summary>

```bash
sudo fuser -k 5000/tcp
python3 app.py
```

</details>

<details>
<summary><b>❌ Login screen not showing (cached token)</b></summary>

Open browser console and run:
```javascript
localStorage.clear(); location.reload();
```
Or open in **incognito/private** window (`Ctrl+Shift+N`).

</details>

<details>
<summary><b>❌ SyntaxError / encoding error in app.py</b></summary>

```bash
rm ~/rtl2gds/app.py
nano ~/rtl2gds/app.py
```
Make sure the first line is:
```python
# -*- coding: utf-8 -*-
```

</details>

---

## 📄 License

This project is licensed under the **MIT License** — free to use, modify, and distribute.

---

<div align="center">

Built with ❤️ for open-source chip design

**[OpenLane 2](https://openlane2.readthedocs.io)** · **[Flask](https://flask.palletsprojects.com)** · **[WSL2](https://docs.microsoft.com/en-us/windows/wsl/)**

</div>
