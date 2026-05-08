<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>RTL2GDS · OpenLane Flow — README</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;500;700&family=Syne:wght@400;600;700;800&display=swap" rel="stylesheet"/>
<style>
:root {
  --bg: #060809;
  --surface: #0d1117;
  --panel: #131920;
  --border: #1e2530;
  --border2: #2a333f;
  --text: #cdd9e5;
  --muted: #636e7b;
  --accent: #00d4aa;
  --accent2: #58a6ff;
  --accent3: #f0883e;
  --danger: #f85149;
  --success: #3fb950;
  --mono: 'JetBrains Mono', monospace;
  --sans: 'Syne', sans-serif;
}
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  background: var(--bg);
  color: var(--text);
  font-family: var(--mono);
  font-size: 13px;
  line-height: 1.7;
  overflow-x: hidden;
}

/* GRID BACKGROUND */
body::before {
  content: '';
  position: fixed;
  inset: 0;
  background-image:
    linear-gradient(rgba(0,212,170,0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0,212,170,0.03) 1px, transparent 1px);
  background-size: 40px 40px;
  pointer-events: none;
  z-index: 0;
}

/* SCANLINES */
body::after {
  content: '';
  position: fixed;
  inset: 0;
  background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,0,0,0.08) 2px, rgba(0,0,0,0.08) 4px);
  pointer-events: none;
  z-index: 1;
}

/* NOISE OVERLAY */
.noise {
  position: fixed;
  inset: 0;
  opacity: 0.025;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)'/%3E%3C/svg%3E");
  pointer-events: none;
  z-index: 2;
}

/* GLOW ORBS */
.orb {
  position: fixed;
  border-radius: 50%;
  filter: blur(120px);
  pointer-events: none;
  z-index: 0;
  animation: orbFloat 8s ease-in-out infinite;
}
.orb-1 { width: 600px; height: 600px; background: rgba(0,212,170,0.06); top: -200px; left: -200px; animation-delay: 0s; }
.orb-2 { width: 400px; height: 400px; background: rgba(88,166,255,0.05); bottom: -100px; right: -100px; animation-delay: -4s; }
@keyframes orbFloat {
  0%, 100% { transform: translate(0, 0); }
  50% { transform: translate(30px, 20px); }
}

/* WRAPPER */
.wrapper {
  position: relative;
  z-index: 10;
  max-width: 900px;
  margin: 0 auto;
  padding: 0 24px 80px;
}

/* ── HERO ── */
.hero {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 60px 0;
  position: relative;
}

.hero-chip {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: rgba(0,212,170,0.08);
  border: 1px solid rgba(0,212,170,0.2);
  border-radius: 20px;
  padding: 6px 16px;
  font-size: 11px;
  color: var(--accent);
  letter-spacing: 1.5px;
  text-transform: uppercase;
  margin-bottom: 32px;
  animation: fadeDown 0.8s ease both;
}
.hero-chip .dot { width: 6px; height: 6px; border-radius: 50%; background: var(--accent); box-shadow: 0 0 8px var(--accent); animation: pulse 2s infinite; }
@keyframes pulse { 0%,100%{opacity:1} 50%{opacity:0.3} }

.hero-title {
  font-family: var(--sans);
  font-weight: 800;
  font-size: clamp(52px, 8vw, 88px);
  letter-spacing: -3px;
  line-height: 1;
  margin-bottom: 8px;
  animation: fadeDown 0.8s 0.1s ease both;
}
.hero-title .t1 { color: var(--text); }
.hero-title .t2 {
  color: transparent;
  -webkit-text-stroke: 1px rgba(0,212,170,0.5);
}
.hero-title .arrow {
  color: var(--accent);
  display: inline-block;
  animation: arrowPulse 2s ease-in-out infinite;
  font-style: normal;
}
@keyframes arrowPulse {
  0%,100% { transform: translateX(0); opacity: 1; }
  50% { transform: translateX(6px); opacity: 0.7; }
}

.hero-sub {
  font-family: var(--sans);
  font-size: clamp(13px, 2vw, 16px);
  color: var(--muted);
  max-width: 520px;
  margin: 24px auto 0;
  line-height: 1.8;
  animation: fadeDown 0.8s 0.2s ease both;
}

.hero-badges {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
  margin-top: 36px;
  animation: fadeDown 0.8s 0.3s ease both;
}
.badge {
  padding: 5px 14px;
  border-radius: 4px;
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 0.8px;
  text-transform: uppercase;
}
.badge-green { background: rgba(63,185,80,0.12); color: var(--success); border: 1px solid rgba(63,185,80,0.25); }
.badge-blue { background: rgba(88,166,255,0.12); color: var(--accent2); border: 1px solid rgba(88,166,255,0.25); }
.badge-orange { background: rgba(240,136,62,0.12); color: var(--accent3); border: 1px solid rgba(240,136,62,0.25); }
.badge-teal { background: rgba(0,212,170,0.12); color: var(--accent); border: 1px solid rgba(0,212,170,0.25); }

/* CIRCUIT DECORATION */
.circuit-line {
  width: 1px;
  height: 80px;
  background: linear-gradient(to bottom, transparent, var(--accent), transparent);
  margin: 40px auto;
  animation: fadeDown 0.8s 0.4s ease both;
  position: relative;
}
.circuit-line::before, .circuit-line::after {
  content: '';
  position: absolute;
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background: var(--accent);
  left: 50%;
  transform: translateX(-50%);
  box-shadow: 0 0 10px var(--accent);
}
.circuit-line::before { top: 0; }
.circuit-line::after { bottom: 0; }

/* ── SECTIONS ── */
section {
  margin-bottom: 64px;
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}
section.visible { opacity: 1; transform: translateY(0); }

.section-label {
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 10px;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 20px;
}
.section-label::after {
  content: '';
  flex: 1;
  height: 1px;
  background: linear-gradient(to right, var(--border2), transparent);
}

h2 {
  font-family: var(--sans);
  font-size: 26px;
  font-weight: 800;
  color: var(--text);
  margin-bottom: 16px;
  letter-spacing: -0.5px;
}

p { color: var(--muted); margin-bottom: 12px; line-height: 1.8; }

/* ── FEATURE GRID ── */
.feature-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
  gap: 12px;
}
.feature-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 20px;
  transition: border-color 0.2s, transform 0.2s, box-shadow 0.2s;
  cursor: default;
}
.feature-card:hover {
  border-color: var(--accent);
  transform: translateY(-3px);
  box-shadow: 0 8px 30px rgba(0,212,170,0.08);
}
.feature-icon {
  font-size: 20px;
  margin-bottom: 10px;
  display: block;
}
.feature-title {
  font-family: var(--sans);
  font-size: 13px;
  font-weight: 700;
  color: var(--text);
  margin-bottom: 6px;
}
.feature-desc { font-size: 11px; color: var(--muted); line-height: 1.6; }

/* ── CODE BLOCK ── */
.code-block {
  background: #0d1117;
  border: 1px solid var(--border);
  border-radius: 8px;
  overflow: hidden;
  margin: 16px 0;
}
.code-header {
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  padding: 8px 16px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 10px;
  color: var(--muted);
}
.code-dots { display: flex; gap: 6px; }
.code-dots span { width: 10px; height: 10px; border-radius: 50%; }
.dot-red { background: #f85149; }
.dot-yellow { background: #f0883e; }
.dot-green { background: #3fb950; }
.code-body {
  padding: 16px 20px;
  font-size: 12px;
  line-height: 1.8;
  white-space: pre;
  overflow-x: auto;
  color: #8b949e;
}
.code-body .kw { color: #ff7b72; }
.code-body .str { color: #a5d6ff; }
.code-body .cm { color: #3fb950; }
.code-body .fn { color: #d2a8ff; }
.code-body .num { color: #f0883e; }
.code-body .acc { color: #00d4aa; }

/* ── FLOW DIAGRAM ── */
.flow {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  gap: 0;
  padding: 32px 0;
}
.flow-step {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  opacity: 0;
  transform: scale(0.85);
  transition: opacity 0.4s ease, transform 0.4s ease;
}
.flow-step.visible { opacity: 1; transform: scale(1); }
.flow-box {
  background: var(--surface);
  border: 1px solid var(--border2);
  border-radius: 8px;
  padding: 12px 16px;
  font-size: 11px;
  color: var(--text);
  font-weight: 500;
  text-align: center;
  min-width: 100px;
  transition: border-color 0.2s, box-shadow 0.2s;
  white-space: nowrap;
}
.flow-box:hover { border-color: var(--accent); box-shadow: 0 0 20px rgba(0,212,170,0.1); }
.flow-box.active { border-color: var(--accent); background: rgba(0,212,170,0.06); color: var(--accent); }
.flow-label { font-size: 9px; color: var(--muted); letter-spacing: 0.5px; }
.flow-arrow {
  font-size: 16px;
  color: var(--accent);
  margin: 0 4px;
  align-self: center;
  padding-bottom: 20px;
  animation: arrowPulse 2s ease-in-out infinite;
}

/* ── TABLE ── */
table {
  width: 100%;
  border-collapse: collapse;
  margin: 16px 0;
}
th {
  background: var(--surface);
  padding: 10px 16px;
  text-align: left;
  font-size: 10px;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: var(--accent);
  border-bottom: 1px solid var(--border2);
  font-weight: 600;
}
td {
  padding: 10px 16px;
  font-size: 12px;
  border-bottom: 1px solid var(--border);
  color: var(--text);
  transition: background 0.15s;
}
tr:hover td { background: rgba(0,212,170,0.03); }
code {
  background: rgba(0,212,170,0.1);
  color: var(--accent);
  padding: 2px 6px;
  border-radius: 3px;
  font-family: var(--mono);
  font-size: 11px;
}

/* ── TREE ── */
.tree {
  background: #0d1117;
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 20px 24px;
  font-size: 12px;
  line-height: 2;
  color: #8b949e;
}
.tree .dir { color: var(--accent2); }
.tree .file { color: var(--text); }
.tree .comment { color: var(--muted); font-size: 11px; }

/* ── CREDS CARD ── */
.creds-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
.cred-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 20px;
  text-align: center;
  transition: all 0.2s;
}
.cred-card:hover { border-color: var(--accent); box-shadow: 0 0 20px rgba(0,212,170,0.06); }
.cred-user { font-family: var(--sans); font-size: 18px; font-weight: 800; color: var(--text); margin-bottom: 4px; }
.cred-pass { font-size: 11px; color: var(--accent); letter-spacing: 1px; }
.cred-label { font-size: 9px; color: var(--muted); text-transform: uppercase; letter-spacing: 1px; margin-bottom: 8px; }

/* ── TROUBLESHOOT ── */
.trouble-list { display: flex; flex-direction: column; gap: 12px; }
.trouble-item {
  background: var(--surface);
  border: 1px solid var(--border);
  border-left: 3px solid var(--accent3);
  border-radius: 0 8px 8px 0;
  padding: 14px 16px;
  transition: border-color 0.2s;
}
.trouble-item:hover { border-left-color: var(--accent); }
.trouble-q { font-size: 12px; color: var(--text); font-weight: 500; margin-bottom: 8px; }
.trouble-a { font-size: 11px; color: var(--muted); line-height: 1.7; }

/* ── FOOTER ── */
footer {
  text-align: center;
  padding: 60px 0 40px;
  border-top: 1px solid var(--border);
  margin-top: 40px;
}
.footer-logo {
  font-family: var(--sans);
  font-size: 32px;
  font-weight: 800;
  letter-spacing: -1px;
  margin-bottom: 16px;
}
.footer-logo span { color: var(--accent); }
.footer-sub { color: var(--muted); font-size: 12px; }
.footer-heart { color: var(--danger); animation: heartbeat 1.5s ease-in-out infinite; display: inline-block; }
@keyframes heartbeat {
  0%,100%{transform:scale(1)} 50%{transform:scale(1.3)}
}

/* SCROLL PROGRESS */
.scroll-progress {
  position: fixed;
  top: 0;
  left: 0;
  height: 2px;
  background: linear-gradient(90deg, var(--accent), var(--accent2));
  z-index: 1000;
  transition: width 0.1s;
  box-shadow: 0 0 8px var(--accent);
}

/* ANIMATIONS */
@keyframes fadeDown {
  from { opacity: 0; transform: translateY(-20px); }
  to { opacity: 1; transform: translateY(0); }
}
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

/* TYPING CURSOR */
.typing::after {
  content: '|';
  color: var(--accent);
  animation: blink 1s step-end infinite;
}
@keyframes blink { 0%,100%{opacity:1} 50%{opacity:0} }

/* TERMINAL ANIMATION */
.terminal {
  background: #0d1117;
  border: 1px solid var(--border);
  border-radius: 10px;
  overflow: hidden;
}
.terminal-header {
  background: var(--surface);
  padding: 10px 16px;
  display: flex;
  align-items: center;
  gap: 12px;
  border-bottom: 1px solid var(--border);
}
.terminal-body {
  padding: 16px 20px;
  font-size: 12px;
  line-height: 2;
  min-height: 160px;
}
.t-line { display: flex; gap: 8px; opacity: 0; animation: lineIn 0.3s ease forwards; }
.t-line .prompt { color: var(--accent); user-select: none; }
.t-line .cmd { color: var(--text); }
.t-line .out { color: #8b949e; }
.t-line .ok { color: var(--success); }
.t-line .err { color: var(--danger); }

/* STEP counter animation */
@keyframes lineIn { from{opacity:0;transform:translateX(-8px)} to{opacity:1;transform:none} }

/* NAV */
nav {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100;
  background: rgba(6,8,9,0.85);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--border);
  padding: 0 32px;
  height: 52px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.nav-logo { font-family: var(--sans); font-weight: 800; font-size: 15px; color: var(--text); }
.nav-logo span { color: var(--accent); }
.nav-links { display: flex; gap: 24px; }
.nav-links a { font-size: 11px; color: var(--muted); text-decoration: none; letter-spacing: 0.5px; transition: color 0.15s; }
.nav-links a:hover { color: var(--accent); }

.wrapper { padding-top: 80px; }
</style>
</head>
<body>

<div class="noise"></div>
<div class="orb orb-1"></div>
<div class="orb orb-2"></div>
<div class="scroll-progress" id="scrollProgress"></div>

<nav>
  <div class="nav-logo">RTL<span>2GDS</span></div>
  <div class="nav-links">
    <a href="#features">Features</a>
    <a href="#flow">Flow</a>
    <a href="#install">Install</a>
    <a href="#api">API</a>
    <a href="#troubleshoot">Help</a>
  </div>
</nav>

<div class="wrapper">

  <!-- HERO -->
  <div class="hero">
    <div class="hero-chip"><span class="dot"></span>OpenLane 2 · ASIC Flow</div>
    <div class="hero-title">
      <span class="t1">RTL</span><i class="arrow">→</i><span class="t2">GDS</span>
    </div>
    <div class="hero-sub">
      A self-hosted web interface for full ASIC physical design flows.
      Paste Verilog. Click Run. Get GDSII.
    </div>
    <div class="hero-badges">
      <span class="badge badge-green">✓ Open Source</span>
      <span class="badge badge-blue">✓ Mobile Ready</span>
      <span class="badge badge-teal">✓ JWT Auth</span>
      <span class="badge badge-orange">✓ Live Logs</span>
      <span class="badge badge-green">✓ ngrok Support</span>
    </div>
    <div class="circuit-line"></div>
  </div>

  <!-- FEATURES -->
  <section id="features">
    <div class="section-label">01 · Features</div>
    <h2>Everything you need</h2>
    <p>A complete browser-based EDA environment — no terminal required.</p>
    <div class="feature-grid">
      <div class="feature-card"><span class="feature-icon">⚡</span><div class="feature-title">One-Click Flow</div><div class="feature-desc">Submit any Verilog design and run the full OpenLane pipeline instantly.</div></div>
      <div class="feature-card"><span class="feature-icon">🔐</span><div class="feature-title">JWT Auth</div><div class="feature-desc">Secure token-based sessions. Login required for all API endpoints.</div></div>
      <div class="feature-card"><span class="feature-icon">📡</span><div class="feature-title">Live Streaming</div><div class="feature-desc">Real-time log output polled every 1.5s with color-coded messages.</div></div>
      <div class="feature-card"><span class="feature-icon">📊</span><div class="feature-title">Progress Bar</div><div class="feature-desc">42-step progress tracking matching OpenLane's actual flow stages.</div></div>
      <div class="feature-card"><span class="feature-icon">🗂️</span><div class="feature-title">Job History</div><div class="feature-desc">All runs saved per session. Click any job to view its logs again.</div></div>
      <div class="feature-card"><span class="feature-icon">📱</span><div class="feature-title">Mobile Ready</div><div class="feature-desc">Responsive UI tested on Android via ngrok tunnel. Works anywhere.</div></div>
      <div class="feature-card"><span class="feature-icon">💾</span><div class="feature-title">Auto File Gen</div><div class="feature-desc">Verilog .v and config.json written automatically for every job.</div></div>
      <div class="feature-card"><span class="feature-icon">🧪</span><div class="feature-title">Quick Examples</div><div class="feature-desc">XOR, AND, MUX, Counter preloaded — one click to populate the editor.</div></div>
    </div>
  </section>

  <!-- FLOW -->
  <section id="flow">
    <div class="section-label">02 · Pipeline</div>
    <h2>How it works</h2>
    <p>From Verilog source to physical layout in one automated pipeline.</p>
    <div class="flow" id="flowDiagram">
      <div class="flow-step">
        <div class="flow-box active">Verilog Input</div>
        <div class="flow-label">RTL Source</div>
      </div>
      <div class="flow-arrow">→</div>
      <div class="flow-step">
        <div class="flow-box">Synthesis</div>
        <div class="flow-label">Yosys</div>
      </div>
      <div class="flow-arrow">→</div>
      <div class="flow-step">
        <div class="flow-box">Floorplan</div>
        <div class="flow-label">OpenROAD</div>
      </div>
      <div class="flow-arrow">→</div>
      <div class="flow-step">
        <div class="flow-box">P&R</div>
        <div class="flow-label">Place & Route</div>
      </div>
      <div class="flow-arrow">→</div>
      <div class="flow-step">
        <div class="flow-box">GDSII</div>
        <div class="flow-label">Magic / KLayout</div>
      </div>
    </div>
  </section>

  <!-- STRUCTURE -->
  <section>
    <div class="section-label">03 · Structure</div>
    <h2>Project layout</h2>
    <div class="tree">
<span class="dir">rtl2gds/</span>
├── <span class="file">app.py</span>               <span class="comment"># Flask backend + JWT auth</span>
├── <span class="dir">frontend/</span>
│   └── <span class="file">index.html</span>       <span class="comment"># Single-page UI</span>
└── <span class="dir">runs/</span>
    └── <span class="dir">&lt;job_id&gt;/</span>
        ├── <span class="file">config.json</span>  <span class="comment"># Auto-generated OpenLane config</span>
        └── <span class="dir">src/</span>
            └── <span class="file">design.v</span> <span class="comment"># Saved Verilog source</span>
    </div>
  </section>

  <!-- INSTALL -->
  <section id="install">
    <div class="section-label">04 · Installation</div>
    <h2>Get running in 3 steps</h2>

    <p style="margin-bottom:16px">Step 1 — Install dependencies</p>
    <div class="code-block">
      <div class="code-header">
        <div class="code-dots"><span class="dot-red"></span><span class="dot-yellow"></span><span class="dot-green"></span></div>
        <span>bash</span>
      </div>
      <div class="code-body"><span class="acc">pip3</span> install flask PyJWT --break-system-packages
<span class="acc">pip3</span> install openlane --break-system-packages</div>
    </div>

    <p style="margin-bottom:16px;margin-top:24px">Step 2 — Start Flask</p>
    <div class="code-block">
      <div class="code-header">
        <div class="code-dots"><span class="dot-red"></span><span class="dot-yellow"></span><span class="dot-green"></span></div>
        <span>bash</span>
      </div>
      <div class="code-body"><span class="cm"># Terminal 1</span>
<span class="kw">cd</span> ~/rtl2gds && <span class="acc">python3</span> app.py

<span class="cm"># Terminal 2 — expose over internet</span>
<span class="acc">ngrok</span> http <span class="num">5000</span></div>
    </div>

    <p style="margin-bottom:16px;margin-top:24px">Step 3 — Open in browser</p>
    <div class="terminal">
      <div class="terminal-header">
        <div class="code-dots"><span class="dot-red"></span><span class="dot-yellow"></span><span class="dot-green"></span></div>
        <span style="font-size:10px;color:var(--muted)">terminal output</span>
      </div>
      <div class="terminal-body" id="terminalDemo">
        <div class="t-line" style="animation-delay:0.2s"><span class="prompt">$</span><span class="cmd">python3 app.py</span></div>
        <div class="t-line" style="animation-delay:0.8s"><span class="out">Users: ['admin', 'sanju']</span></div>
        <div class="t-line" style="animation-delay:1.2s"><span class="out">Starting on</span><span class="ok"> http://localhost:5000</span></div>
        <div class="t-line" style="animation-delay:1.6s"><span class="out"> * Serving Flask app 'app'</span></div>
        <div class="t-line" style="animation-delay:2.0s"><span class="out"> * Running on</span><span class="ok"> http://127.0.0.1:5000</span></div>
        <div class="t-line" style="animation-delay:2.6s"><span class="prompt">$</span><span class="cmd">ngrok http 5000</span></div>
        <div class="t-line" style="animation-delay:3.2s"><span class="ok">Forwarding  https://xxxx.ngrok-free.app → localhost:5000</span></div>
      </div>
    </div>
  </section>

  <!-- CREDENTIALS -->
  <section>
    <div class="section-label">05 · Credentials</div>
    <h2>Default login</h2>
    <p>Change these in the <code>USERS</code> dict in <code>app.py</code> before deploying publicly.</p>
    <div class="creds-grid">
      <div class="cred-card">
        <div class="cred-label">Username</div>
        <div class="cred-user">admin</div>
        <div class="cred-pass">admin123</div>
      </div>
      <div class="cred-card">
        <div class="cred-label">Username</div>
        <div class="cred-user">sanju</div>
        <div class="cred-pass">sanju123</div>
      </div>
    </div>
  </section>

  <!-- API -->
  <section id="api">
    <div class="section-label">06 · API Reference</div>
    <h2>Endpoints</h2>
    <p>All endpoints except <code>/api/login</code> require <code>Authorization: Bearer &lt;token&gt;</code></p>
    <table>
      <thead><tr><th>Method</th><th>Endpoint</th><th>Description</th></tr></thead>
      <tbody>
        <tr><td><span class="badge badge-blue">POST</span></td><td><code>/api/login</code></td><td>Authenticate, receive JWT token</td></tr>
        <tr><td><span class="badge badge-orange">POST</span></td><td><code>/api/run</code></td><td>Submit Verilog design, start job</td></tr>
        <tr><td><span class="badge badge-green">GET</span></td><td><code>/api/status/&lt;id&gt;</code></td><td>Poll job status and live logs</td></tr>
        <tr><td><span class="badge badge-green">GET</span></td><td><code>/api/jobs</code></td><td>List all jobs for session</td></tr>
      </tbody>
    </table>
  </section>

  <!-- TECH STACK -->
  <section>
    <div class="section-label">07 · Tech Stack</div>
    <h2>Built with</h2>
    <table>
      <thead><tr><th>Layer</th><th>Technology</th></tr></thead>
      <tbody>
        <tr><td>Frontend</td><td>Vanilla HTML · CSS · JavaScript</td></tr>
        <tr><td>Backend</td><td>Python · Flask · Werkzeug</td></tr>
        <tr><td>Auth</td><td>JWT (PyJWT) · Bearer tokens</td></tr>
        <tr><td>EDA Flow</td><td>OpenLane 2 · Yosys · OpenROAD</td></tr>
        <tr><td>Fonts</td><td>JetBrains Mono · Syne</td></tr>
        <tr><td>Tunnel</td><td>ngrok</td></tr>
        <tr><td>Platform</td><td>WSL2 · Ubuntu</td></tr>
      </tbody>
    </table>
  </section>

  <!-- TROUBLESHOOT -->
  <section id="troubleshoot">
    <div class="section-label">08 · Troubleshooting</div>
    <h2>Common issues</h2>
    <div class="trouble-list">
      <div class="trouble-item">
        <div class="trouble-q">⚠ "Server unreachable" on login</div>
        <div class="trouble-a">Flask is not running. Run <code>python3 app.py</code> in WSL. Also make sure ngrok is pointing to port 5000 and both are running simultaneously.</div>
      </div>
      <div class="trouble-item">
        <div class="trouble-q">⚠ "No module named openlane"</div>
        <div class="trouble-a">Run: <code>pip3 install openlane --break-system-packages</code> then restart Flask.</div>
      </div>
      <div class="trouble-item">
        <div class="trouble-q">⚠ Port 5000 already in use</div>
        <div class="trouble-a">Run: <code>sudo fuser -k 5000/tcp</code> then restart with <code>python3 app.py</code></div>
      </div>
      <div class="trouble-item">
        <div class="trouble-q">⚠ Login screen not showing (cached token)</div>
        <div class="trouble-a">Open browser console and run: <code>localStorage.clear(); location.reload()</code> — or open in incognito mode.</div>
      </div>
      <div class="trouble-item">
        <div class="trouble-q">⚠ SyntaxError / encoding error in app.py</div>
        <div class="trouble-a">Delete and recreate: <code>rm ~/rtl2gds/app.py && nano ~/rtl2gds/app.py</code> — paste fresh code with <code># -*- coding: utf-8 -*-</code> at top.</div>
      </div>
    </div>
  </section>

  <!-- FOOTER -->
  <footer>
    <div class="footer-logo">RTL<span>2GDS</span></div>
    <div class="footer-sub">
      Built with <span class="footer-heart">♥</span> for open-source chip design<br/>
      <span style="color:var(--border2);margin-top:8px;display:block">MIT License · OpenLane 2 · WSL2 · Flask</span>
    </div>
  </footer>

</div>

<script>
// Scroll progress bar
const prog = document.getElementById('scrollProgress');
window.addEventListener('scroll', () => {
  const pct = (window.scrollY / (document.body.scrollHeight - window.innerHeight)) * 100;
  prog.style.width = pct + '%';
});

// Intersection observer for section reveals
const observer = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      e.target.classList.add('visible');
    }
  });
}, { threshold: 0.1 });

document.querySelectorAll('section').forEach(s => observer.observe(s));

// Flow diagram stagger
const flowSteps = document.querySelectorAll('.flow-step');
const flowObs = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      flowSteps.forEach((s, i) => {
        setTimeout(() => s.classList.add('visible'), i * 150);
      });
    }
  });
}, { threshold: 0.3 });
if (document.getElementById('flowDiagram')) flowObs.observe(document.getElementById('flowDiagram'));

// Animate flow boxes cycling highlight
let currentFlow = 0;
const flowBoxes = document.querySelectorAll('.flow-box');
setInterval(() => {
  flowBoxes.forEach(b => b.classList.remove('active'));
  flowBoxes[currentFlow].classList.add('active');
  currentFlow = (currentFlow + 1) % flowBoxes.length;
}, 1200);
</script>
</body>
</html>