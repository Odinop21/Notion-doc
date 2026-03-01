# Notion-doc
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Phase 2 — AI Recommendations</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:ital,wght@0,300;0,400;0,500;0,700;1,400&family=Syne:wght@400;600;700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #080c10;
    --surface: #0d1117;
    --surface2: #161b22;
    --border: #21262d;
    --border2: #30363d;
    --text: #e6edf3;
    --muted: #7d8590;
    --dim: #484f58;
    --green: #39d353;
    --green-dim: #1a4a2e;
    --green-glow: rgba(57,211,83,0.15);
    --cyan: #58a6ff;
    --cyan-dim: #0d2b4e;
    --orange: #f78166;
    --orange-dim: #4a1a10;
    --yellow: #e3b341;
    --yellow-dim: #3d2e0a;
    --purple: #d2a8ff;
    --purple-dim: #2d1a4e;
    --red: #ff7b72;
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    line-height: 1.7;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* GRID BG */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(57,211,83,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(57,211,83,0.03) 1px, transparent 1px);
    background-size: 40px 40px;
    pointer-events: none;
    z-index: 0;
  }

  /* SCANLINES */
  body::after {
    content: '';
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,0,0,0.08) 2px,
      rgba(0,0,0,0.08) 4px
    );
    pointer-events: none;
    z-index: 0;
  }

  .container {
    max-width: 1100px;
    margin: 0 auto;
    padding: 0 24px 80px;
    position: relative;
    z-index: 1;
  }

  /* ── HEADER ── */
  .header {
    padding: 60px 0 48px;
    border-bottom: 1px solid var(--border);
    margin-bottom: 48px;
    position: relative;
  }

  .header::after {
    content: '';
    position: absolute;
    bottom: -1px;
    left: 0;
    width: 200px;
    height: 1px;
    background: var(--green);
    box-shadow: 0 0 12px var(--green);
  }

  .phase-label {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    font-weight: 500;
    color: var(--green);
    letter-spacing: 0.15em;
    text-transform: uppercase;
    margin-bottom: 16px;
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .phase-label::before {
    content: '//';
    color: var(--dim);
  }

  .header h1 {
    font-family: 'Syne', sans-serif;
    font-size: clamp(32px, 5vw, 52px);
    font-weight: 800;
    color: var(--text);
    line-height: 1.1;
    letter-spacing: -0.02em;
    margin-bottom: 8px;
  }

  .header h1 span {
    color: var(--green);
    text-shadow: 0 0 30px rgba(57,211,83,0.4);
  }

  .header-sub {
    color: var(--muted);
    font-size: 12px;
    margin-top: 12px;
  }

  /* ── META ROW ── */
  .meta-row {
    display: flex;
    gap: 12px;
    margin-bottom: 48px;
    flex-wrap: wrap;
  }

  .meta-chip {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 8px 14px;
    border-radius: 6px;
    border: 1px solid var(--border2);
    background: var(--surface);
    font-size: 11px;
    font-weight: 500;
    letter-spacing: 0.05em;
  }

  .meta-chip .dot {
    width: 6px; height: 6px;
    border-radius: 50%;
  }

  .chip-time { color: var(--cyan); }
  .chip-time .dot { background: var(--cyan); box-shadow: 0 0 6px var(--cyan); }
  .chip-progress { color: var(--green); }
  .chip-progress .dot { background: var(--green); box-shadow: 0 0 6px var(--green); animation: pulse 2s infinite; }
  .chip-diff { color: var(--yellow); }
  .chip-diff .dot { background: var(--yellow); box-shadow: 0 0 6px var(--yellow); }

  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.3; }
  }

  /* ── TODAY FOCUS ── */
  .today-block {
    background: var(--surface);
    border: 1px solid var(--border2);
    border-left: 3px solid var(--green);
    border-radius: 8px;
    padding: 20px 24px;
    margin-bottom: 48px;
    display: flex;
    align-items: flex-start;
    gap: 16px;
  }

  .today-icon {
    font-size: 20px;
    flex-shrink: 0;
    margin-top: 2px;
  }

  .today-label {
    font-size: 10px;
    color: var(--green);
    letter-spacing: 0.15em;
    text-transform: uppercase;
    font-weight: 700;
    margin-bottom: 6px;
  }

  .today-input {
    background: transparent;
    border: none;
    outline: none;
    color: var(--muted);
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    width: 100%;
    cursor: text;
  }

  .today-input::placeholder { color: var(--dim); }

  /* ── SECTION TITLES ── */
  .section-header {
    display: flex;
    align-items: center;
    gap: 14px;
    margin-bottom: 28px;
    margin-top: 56px;
  }

  .section-num {
    font-family: 'Syne', sans-serif;
    font-size: 11px;
    font-weight: 700;
    color: var(--bg);
    background: var(--green);
    width: 24px; height: 24px;
    border-radius: 4px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
  }

  .section-title {
    font-family: 'Syne', sans-serif;
    font-size: 20px;
    font-weight: 700;
    color: var(--text);
    letter-spacing: -0.01em;
  }

  .section-days {
    font-size: 11px;
    color: var(--cyan);
    background: var(--cyan-dim);
    border: 1px solid rgba(88,166,255,0.2);
    padding: 3px 10px;
    border-radius: 4px;
    margin-left: auto;
    white-space: nowrap;
  }

  /* ── LEARN GRID ── */
  .learn-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 12px;
    margin-bottom: 28px;
  }

  .learn-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 18px;
    transition: border-color 0.2s, transform 0.2s;
    position: relative;
    overflow: hidden;
  }

  .learn-card:hover {
    border-color: var(--border2);
    transform: translateY(-2px);
  }

  .learn-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0;
    width: 3px;
    height: 100%;
    background: var(--cyan);
    opacity: 0.6;
  }

  .learn-card.orange::before { background: var(--orange); }
  .learn-card.purple::before { background: var(--purple); }
  .learn-card.yellow::before { background: var(--yellow); }
  .learn-card.green::before { background: var(--green); }

  .learn-card h4 {
    font-family: 'Syne', sans-serif;
    font-size: 13px;
    font-weight: 700;
    color: var(--text);
    margin-bottom: 6px;
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .tag {
    font-family: 'JetBrains Mono', monospace;
    font-size: 9px;
    color: var(--cyan);
    background: var(--cyan-dim);
    border: 1px solid rgba(88,166,255,0.2);
    padding: 1px 6px;
    border-radius: 3px;
    font-weight: 400;
  }

  .tag.orange { color: var(--orange); background: var(--orange-dim); border-color: rgba(247,129,102,0.2); }
  .tag.green { color: var(--green); background: var(--green-dim); border-color: rgba(57,211,83,0.2); }
  .tag.yellow { color: var(--yellow); background: var(--yellow-dim); border-color: rgba(227,179,65,0.2); }
  .tag.purple { color: var(--purple); background: var(--purple-dim); border-color: rgba(210,168,255,0.2); }

  .learn-card p {
    color: var(--muted);
    font-size: 11.5px;
    line-height: 1.6;
  }

  /* ── MILESTONE ── */
  .milestone {
    background: var(--green-dim);
    border: 1px solid rgba(57,211,83,0.25);
    border-radius: 6px;
    padding: 12px 16px;
    margin-top: 16px;
    font-size: 11.5px;
    color: var(--green);
    display: flex;
    gap: 10px;
    align-items: flex-start;
  }

  .milestone::before {
    content: '▶';
    font-size: 9px;
    margin-top: 3px;
    flex-shrink: 0;
  }

  /* ── CONCEPT BLOCK ── */
  .concept-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    margin-bottom: 16px;
  }

  @media (max-width: 640px) { .concept-row { grid-template-columns: 1fr; } }

  .concept-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 20px;
  }

  .concept-label {
    font-size: 9px;
    color: var(--purple);
    letter-spacing: 0.2em;
    text-transform: uppercase;
    font-weight: 700;
    margin-bottom: 10px;
  }

  .concept-card h4 {
    font-family: 'Syne', sans-serif;
    font-size: 15px;
    font-weight: 700;
    color: var(--text);
    margin-bottom: 8px;
  }

  .concept-card p { color: var(--muted); font-size: 11.5px; line-height: 1.6; }

  .code-inline {
    font-family: 'JetBrains Mono', monospace;
    background: var(--surface2);
    border: 1px solid var(--border);
    padding: 1px 6px;
    border-radius: 3px;
    color: var(--orange);
    font-size: 11px;
  }

  /* ── CODE BLOCK ── */
  .code-block {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    margin: 16px 0;
    overflow: hidden;
  }

  .code-header {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 10px 16px;
    background: var(--surface2);
    border-bottom: 1px solid var(--border);
  }

  .code-dots { display: flex; gap: 6px; }
  .code-dots span {
    width: 10px; height: 10px;
    border-radius: 50%;
  }
  .d1 { background: #ff5f57; }
  .d2 { background: #febc2e; }
  .d3 { background: #28c840; }

  .code-filename {
    font-size: 10px;
    color: var(--muted);
    margin-left: 4px;
  }

  .code-lang {
    margin-left: auto;
    font-size: 9px;
    color: var(--dim);
    letter-spacing: 0.1em;
    text-transform: uppercase;
  }

  pre {
    padding: 20px;
    overflow-x: auto;
    font-size: 11.5px;
    line-height: 1.7;
    color: var(--text);
  }

  /* token colors */
  .kw { color: #ff79c6; }
  .fn { color: #50fa7b; }
  .str { color: #f1fa8c; }
  .cm { color: var(--dim); font-style: italic; }
  .num { color: #bd93f9; }
  .cls { color: #8be9fd; }
  .op { color: var(--muted); }

  /* ── DSA BLOCK ── */
  .dsa-box {
    background: linear-gradient(135deg, rgba(210,168,255,0.05), rgba(88,166,255,0.03));
    border: 1px solid rgba(210,168,255,0.2);
    border-radius: 10px;
    padding: 28px;
    margin: 32px 0;
    position: relative;
    overflow: hidden;
  }

  .dsa-box::before {
    content: 'DSA';
    position: absolute;
    top: -12px;
    right: 20px;
    font-family: 'Syne', sans-serif;
    font-size: 72px;
    font-weight: 800;
    color: rgba(210,168,255,0.04);
    letter-spacing: -4px;
    pointer-events: none;
  }

  .dsa-label {
    font-size: 9px;
    color: var(--purple);
    letter-spacing: 0.2em;
    text-transform: uppercase;
    font-weight: 700;
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .dsa-label::after {
    content: '';
    flex: 1;
    height: 1px;
    background: rgba(210,168,255,0.2);
  }

  .dsa-title {
    font-family: 'Syne', sans-serif;
    font-size: 22px;
    font-weight: 800;
    color: var(--text);
    margin-bottom: 8px;
  }

  .dsa-title span { color: var(--purple); }

  .dsa-box p { color: var(--muted); font-size: 12px; line-height: 1.6; margin-bottom: 12px; }

  /* ── CHECKLIST ── */
  .checklist-section {
    margin-top: 60px;
    padding-top: 40px;
    border-top: 1px solid var(--border);
  }

  .checklist-title {
    font-family: 'Syne', sans-serif;
    font-size: 28px;
    font-weight: 800;
    color: var(--text);
    margin-bottom: 8px;
    letter-spacing: -0.02em;
  }

  .checklist-title span { color: var(--green); }

  .checklist-sub {
    color: var(--muted);
    font-size: 11px;
    margin-bottom: 32px;
  }

  .checklist-groups {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
  }

  @media (max-width: 700px) { .checklist-groups { grid-template-columns: 1fr; } }

  .cl-group {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    overflow: hidden;
  }

  .cl-group-header {
    padding: 14px 20px;
    background: var(--surface2);
    border-bottom: 1px solid var(--border);
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .cl-group-header h3 {
    font-family: 'Syne', sans-serif;
    font-size: 13px;
    font-weight: 700;
    color: var(--text);
  }

  .cl-count {
    margin-left: auto;
    font-size: 10px;
    color: var(--dim);
  }

  .cl-items { padding: 8px 0; }

  .cl-item {
    display: flex;
    align-items: flex-start;
    gap: 12px;
    padding: 8px 20px;
    border-radius: 4px;
    transition: background 0.15s;
    cursor: pointer;
  }

  .cl-item:hover { background: rgba(255,255,255,0.02); }

  .cl-item input[type="checkbox"] {
    appearance: none;
    width: 14px; height: 14px;
    border: 1.5px solid var(--border2);
    border-radius: 3px;
    background: transparent;
    cursor: pointer;
    flex-shrink: 0;
    margin-top: 2px;
    transition: all 0.15s;
    position: relative;
  }

  .cl-item input[type="checkbox"]:checked {
    background: var(--green);
    border-color: var(--green);
  }

  .cl-item input[type="checkbox"]:checked::after {
    content: '✓';
    position: absolute;
    top: -2px; left: 1px;
    font-size: 10px;
    color: var(--bg);
    font-weight: 700;
  }

  .cl-item label {
    font-size: 11.5px;
    color: var(--muted);
    cursor: pointer;
    line-height: 1.5;
    transition: color 0.15s;
  }

  .cl-item:has(input:checked) label {
    color: var(--dim);
    text-decoration: line-through;
  }

  /* ── SERVICE DIAGRAM ── */
  .arch-diagram {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 28px;
    margin: 20px 0;
    text-align: center;
  }

  .arch-row {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    flex-wrap: wrap;
    margin: 8px 0;
  }

  .arch-box {
    padding: 10px 18px;
    border-radius: 6px;
    font-size: 11px;
    font-weight: 600;
    border: 1px solid;
  }

  .arch-react { background: rgba(88,166,255,0.08); border-color: rgba(88,166,255,0.3); color: var(--cyan); }
  .arch-node { background: rgba(57,211,83,0.08); border-color: rgba(57,211,83,0.3); color: var(--green); }
  .arch-python { background: rgba(210,168,255,0.08); border-color: rgba(210,168,255,0.3); color: var(--purple); }
  .arch-db { background: rgba(227,179,65,0.08); border-color: rgba(227,179,65,0.3); color: var(--yellow); }

  .arch-arrow {
    color: var(--dim);
    font-size: 14px;
    flex-shrink: 0;
  }

  .arch-label {
    font-size: 9px;
    color: var(--dim);
    margin: 4px 0 12px;
    letter-spacing: 0.1em;
  }

  /* ── COMPLEXITY TABLE ── */
  .complexity-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 11.5px;
    margin-top: 16px;
  }

  .complexity-table th {
    text-align: left;
    padding: 8px 14px;
    background: var(--surface2);
    color: var(--muted);
    font-size: 10px;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    border-bottom: 1px solid var(--border);
  }

  .complexity-table td {
    padding: 10px 14px;
    border-bottom: 1px solid var(--border);
    color: var(--muted);
  }

  .complexity-table td:first-child { color: var(--text); font-weight: 500; }
  .complexity-table td .big-o { color: var(--green); font-weight: 600; }
  .complexity-table tr:last-child td { border-bottom: none; }
  .complexity-table tr:hover td { background: rgba(255,255,255,0.015); }

  /* ── PROGRESS BAR ── */
  .prog-wrap { margin: 16px 0; }
  .prog-label { display: flex; justify-content: space-between; font-size: 10px; color: var(--muted); margin-bottom: 6px; }
  .prog-track {
    height: 4px;
    background: var(--border);
    border-radius: 2px;
    overflow: hidden;
  }
  .prog-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--green), rgba(57,211,83,0.5));
    border-radius: 2px;
    box-shadow: 0 0 8px rgba(57,211,83,0.4);
    transition: width 1s ease;
  }

  /* ── SECTION DIVIDER ── */
  .divider {
    height: 1px;
    background: var(--border);
    margin: 56px 0;
    position: relative;
  }

  .divider::after {
    content: attr(data-label);
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: var(--bg);
    padding: 0 16px;
    color: var(--dim);
    font-size: 10px;
    letter-spacing: 0.2em;
    text-transform: uppercase;
  }

  /* animations */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(16px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .animate {
    animation: fadeUp 0.5s ease both;
  }

  .a1 { animation-delay: 0.05s; }
  .a2 { animation-delay: 0.1s; }
  .a3 { animation-delay: 0.15s; }
  .a4 { animation-delay: 0.2s; }
  .a5 { animation-delay: 0.25s; }
</style>
</head>
<body>
<div class="container">

  <!-- HEADER -->
  <header class="header animate">
    <div class="phase-label">PHASE 2 OF 6 &nbsp;·&nbsp; AI RECOMMENDATIONS</div>
    <h1>The App Learns<br>What You <span>Like</span></h1>
    <p class="header-sub"># personalization &nbsp;·&nbsp; machine-learning &nbsp;·&nbsp; auth &nbsp;·&nbsp; python &nbsp;·&nbsp; heap</p>
  </header>

  <!-- META ROW -->
  <div class="meta-row animate a1">
    <div class="meta-chip chip-time">
      <div class="dot"></div>14–19 days total
    </div>
    <div class="meta-chip chip-progress">
      <div class="dot"></div>0% complete &nbsp;·&nbsp; 0/4 sections
    </div>
    <div class="meta-chip chip-diff">
      <div class="dot"></div>Beginner → Intermediate
    </div>
  </div>

  <!-- PROGRESS BARS -->
  <div class="animate a2">
    <div class="prog-wrap">
      <div class="prog-label"><span>Auth</span><span>0%</span></div>
      <div class="prog-track"><div class="prog-fill" style="width:0%"></div></div>
    </div>
    <div class="prog-wrap">
      <div class="prog-label"><span>Watch History</span><span>0%</span></div>
      <div class="prog-track"><div class="prog-fill" style="width:0%"></div></div>
    </div>
    <div class="prog-wrap">
      <div class="prog-label"><span>ML Service</span><span>0%</span></div>
      <div class="prog-track"><div class="prog-fill" style="width:0%"></div></div>
    </div>
    <div class="prog-wrap">
      <div class="prog-label"><span>DSA — Heap</span><span>0%</span></div>
      <div class="prog-track"><div class="prog-fill" style="width:0%"></div></div>
    </div>
  </div>

  <!-- TODAY FOCUS -->
  <div class="today-block animate a3">
    <div class="today-icon">📌</div>
    <div style="flex:1">
      <div class="today-label">Today's Focus</div>
      <input class="today-input" type="text" placeholder="What are you working on right now? Write your task here...">
    </div>
  </div>

  <div class="divider" data-label="LEARNING ROADMAP"></div>

  <!-- SECTION 1: AUTH -->
  <div class="section-header animate">
    <div class="section-num">1</div>
    <div class="section-title">User Authentication</div>
    <div class="section-days">4–5 days</div>
  </div>

  <p style="color:var(--muted);font-size:12px;margin-bottom:20px;">Before personalizing anything, you need to know <em style="color:var(--text)">WHO</em> is using the app.</p>

  <div class="learn-grid animate a1">
    <div class="learn-card">
      <h4>Hashing <span class="tag">security</span></h4>
      <p>Why you never store a plain password. Use <span class="code-inline">bcrypt</span> to scramble it before saving. One-way operation — can't reverse it.</p>
    </div>
    <div class="learn-card orange">
      <h4>JWT <span class="tag orange">auth</span></h4>
      <p>A digital badge. User logs in → gets a token → sends it with every request to prove who they are. Stateless auth.</p>
    </div>
    <div class="learn-card purple">
      <h4>Protected Routes <span class="tag purple">routes</span></h4>
      <p>How to block pages that require login. Guard components check for a valid token before rendering.</p>
    </div>
    <div class="learn-card yellow">
      <h4>Middleware <span class="tag yellow">node</span></h4>
      <p>Code that runs between receiving a request and responding. Used to verify the JWT on every protected endpoint.</p>
    </div>
  </div>

  <div class="milestone">
    Users can register and log in. Visiting a protected page without a token returns a <span class="code-inline">401</span> error. With a valid token → data is returned.
  </div>

  <!-- SECTION 2: WATCH HISTORY -->
  <div class="section-header" style="margin-top:56px;">
    <div class="section-num">2</div>
    <div class="section-title">Watch History Tracking</div>
    <div class="section-days">2 days</div>
  </div>

  <p style="color:var(--muted);font-size:12px;margin-bottom:20px;">You need to store what the user watches and likes before you can recommend anything.</p>

  <div class="learn-grid">
    <div class="learn-card green">
      <h4>Buttons <span class="tag green">UI</span></h4>
      <p>"Mark as Watched" and "Like ❤️" on the movie page. Clicking sends a <span class="code-inline">POST</span> to your backend.</p>
    </div>
    <div class="learn-card">
      <h4>Database Writes <span class="tag">prisma</span></h4>
      <p>Backend saves the action to the <span class="code-inline">WatchHistory</span> table with <span class="code-inline">userId</span>, <span class="code-inline">movieId</span>, and timestamp.</p>
    </div>
    <div class="learn-card orange">
      <h4>Liked Column <span class="tag orange">schema</span></h4>
      <p>"Like" button sets <span class="code-inline">liked = true</span> in the database. Used to seed the recommendation engine.</p>
    </div>
    <div class="learn-card purple">
      <h4>Query History <span class="tag purple">read</span></h4>
      <p>Fetch a user's last 10 liked movies from the database. This list powers the ML input.</p>
    </div>
  </div>

  <div class="milestone">
    After liking 3 movies, query the database and see those 3 entries tied to your user ID with <span class="code-inline">liked = true</span>.
  </div>

  <!-- SECTION 3: ML SERVICE -->
  <div class="section-header" style="margin-top:56px;">
    <div class="section-num">3</div>
    <div class="section-title">Python ML Recommendation Service</div>
    <div class="section-days">7–10 days</div>
  </div>

  <p style="color:var(--muted);font-size:12px;margin-bottom:20px;">This sounds scary. It's not. You'll write <strong style="color:var(--green)">~50 lines of Python</strong> to make it work.</p>

  <!-- Python setup -->
  <div style="background:var(--surface);border:1px solid var(--border);border-radius:8px;padding:18px;margin-bottom:16px;">
    <div style="font-size:10px;color:var(--yellow);letter-spacing:0.15em;text-transform:uppercase;margin-bottom:12px;font-weight:700;">⚙ Setup First</div>
    <div class="learn-grid" style="margin-bottom:0;">
      <div class="learn-card yellow" style="background:transparent;border:1px solid var(--border2);">
        <h4>Python + pip</h4>
        <p>Install Python and the package manager. Verify with <span class="code-inline">python --version</span>.</p>
      </div>
      <div class="learn-card yellow" style="background:transparent;border:1px solid var(--border2);">
        <h4>Virtual Env</h4>
        <p><span class="code-inline">python -m venv venv</span> — isolated project dependencies. Always activate before installing.</p>
      </div>
      <div class="learn-card yellow" style="background:transparent;border:1px solid var(--border2);">
        <h4>Dependencies</h4>
        <p>Install <span class="code-inline">flask</span>, <span class="code-inline">scikit-learn</span>, <span class="code-inline">pandas</span> into the venv.</p>
      </div>
    </div>
  </div>

  <div class="concept-row">
    <div class="concept-card">
      <div class="concept-label">Concept 1</div>
      <h4>TF-IDF</h4>
      <p>Converts a movie's text description into a list of numbers. Movies with similar number vectors are similar movies.</p>
      <br>
      <p style="color:var(--dim);font-size:11px;">"A thief who steals by entering dreams"<br>→ <span style="color:var(--purple)">[0, 0.8, 0.1, 0.7, ...]</span></p>
    </div>
    <div class="concept-card">
      <div class="concept-label">Concept 2</div>
      <h4>Cosine Similarity</h4>
      <p>Once movies are numbers, cosine similarity measures how close those number vectors are.</p>
      <br>
      <p>Result is <span style="color:var(--green)">0 → 1</span>. Closer to 1 = more similar. That's your recommendation score.</p>
    </div>
  </div>

  <!-- Architecture diagram -->
  <div class="arch-diagram">
    <div style="font-size:10px;color:var(--dim);letter-spacing:0.15em;text-transform:uppercase;margin-bottom:20px;">Service Architecture</div>
    <div class="arch-row">
      <div class="arch-box arch-react">React<br><span style="font-size:9px;opacity:0.7">user clicks</span></div>
      <div class="arch-arrow">→</div>
      <div class="arch-box arch-node">Node.js<br><span style="font-size:9px;opacity:0.7">the waiter</span></div>
      <div class="arch-arrow">→</div>
      <div class="arch-box arch-python">Python Flask<br><span style="font-size:9px;opacity:0.7">the chef</span></div>
    </div>
    <div class="arch-label">Node handles web stuff · Python handles the math</div>
    <div class="arch-row">
      <div class="arch-box arch-db" style="font-size:10px;">PostgreSQL · WatchHistory · liked movie IDs</div>
    </div>
    <div class="arch-label">Database feeds liked IDs → Node → Python → 10 recommendations back</div>
  </div>

  <div class="code-block">
    <div class="code-header">
      <div class="code-dots"><span class="d1"></span><span class="d2"></span><span class="d3"></span></div>
      <span class="code-filename">recommend.py</span>
      <span class="code-lang">python</span>
    </div>
    <pre><span class="cm"># POST /recommend — receives liked IDs, returns top 10</span>
<span class="kw">from</span> flask <span class="kw">import</span> Flask, request, jsonify
<span class="kw">from</span> sklearn.feature_extraction.text <span class="kw">import</span> TfidfVectorizer
<span class="kw">from</span> sklearn.metrics.pairwise <span class="kw">import</span> cosine_similarity
<span class="kw">import</span> pandas <span class="kw">as</span> pd

app = <span class="cls">Flask</span>(<span class="str">__name__</span>)

@app.route(<span class="str">'/recommend'</span>, methods=[<span class="str">'POST'</span>])
<span class="kw">def</span> <span class="fn">recommend</span>():
    liked_ids = request.json.get(<span class="str">'liked_movie_ids'</span>, [])
    <span class="cm"># TF-IDF on overviews + cosine similarity → top K</span>
    scores = <span class="fn">get_recommendations</span>(liked_ids)
    <span class="kw">return</span> <span class="fn">jsonify</span>({ <span class="str">'recommended_ids'</span>: scores[:<span class="num">10</span>] })</pre>
  </div>

  <div class="code-block">
    <div class="code-header">
      <div class="code-dots"><span class="d1"></span><span class="d2"></span><span class="d3"></span></div>
      <span class="code-filename">server.js — calling Python from Node</span>
      <span class="code-lang">javascript</span>
    </div>
    <pre><span class="cm">// Node.js calls Python service for recommendations</span>
app.get(<span class="str">'/api/recommendations'</span>, verifyToken, <span class="kw">async</span> (req, res) => {
  <span class="kw">const</span> liked = <span class="kw">await</span> prisma.watchHistory.<span class="fn">findMany</span>({
    where: { userId: req.user.id, liked: <span class="kw">true</span> },
    select: { movieId: <span class="kw">true</span> }
  });

  <span class="kw">const</span> { data } = <span class="kw">await</span> axios.<span class="fn">post</span>(<span class="str">'http://localhost:5001/recommend'</span>, {
    liked_movie_ids: liked.<span class="fn">map</span>(h => h.movieId)
  });

  <span class="cm">// Enrich IDs with full movie data from TMDB</span>
  <span class="kw">const</span> enriched = <span class="kw">await</span> <span class="fn">enrichMovies</span>(data.recommended_ids);
  res.<span class="fn">json</span>(enriched);
});</pre>
  </div>

  <div class="milestone">
    <span><span class="code-inline">POST /recommend</span> with <span class="code-inline">{"liked_movie_ids": [550, 27205]}</span> returns 10 sensible recommendations matching the same genre/vibe.</span>
  </div>

  <!-- DSA: HEAP -->
  <div class="dsa-box">
    <div class="dsa-label">DSA to Learn Alongside This Phase</div>
    <div class="dsa-title">Heap <span>(Priority Queue)</span></div>
    <p>When the ML model returns 100 candidates with scores, you need the top 10 — fast. A sort is wasteful. A heap is not.</p>

    <div class="concept-row" style="margin-bottom:0;">
      <div class="concept-card" style="background:rgba(210,168,255,0.04);border-color:rgba(210,168,255,0.15);">
        <div class="concept-label">Min-Heap</div>
        <h4>Smallest at top</h4>
        <p>Insert all candidates. Once size > K, pop the minimum. What remains = top K.</p>
      </div>
      <div class="concept-card" style="background:rgba(210,168,255,0.04);border-color:rgba(210,168,255,0.15);">
        <div class="concept-label">Max-Heap</div>
        <h4>Largest at top</h4>
        <p>Extract max K times to get top-K in descending order. Depends on the use case.</p>
      </div>
    </div>

    <div class="code-block" style="margin-top:16px;">
      <div class="code-header">
        <div class="code-dots"><span class="d1"></span><span class="d2"></span><span class="d3"></span></div>
        <span class="code-filename">heap.js</span>
        <span class="code-lang">javascript</span>
      </div>
      <pre><span class="kw">class</span> <span class="cls">MinHeap</span> {
  constructor() { <span class="kw">this</span>.heap = []; }

  <span class="fn">push</span>(val) {
    <span class="kw">this</span>.heap.<span class="fn">push</span>(val);
    <span class="kw">this</span>.<span class="fn">_bubbleUp</span>(<span class="kw">this</span>.heap.length - <span class="num">1</span>);
  }

  <span class="fn">pop</span>() {
    <span class="kw">const</span> top = <span class="kw">this</span>.heap[<span class="num">0</span>];
    <span class="kw">const</span> last = <span class="kw">this</span>.heap.<span class="fn">pop</span>();
    <span class="kw">if</span> (<span class="kw">this</span>.heap.length) {
      <span class="kw">this</span>.heap[<span class="num">0</span>] = last;
      <span class="kw">this</span>.<span class="fn">_sinkDown</span>(<span class="num">0</span>);
    }
    <span class="kw">return</span> top;
  }

  <span class="fn">size</span>() { <span class="kw">return</span> <span class="kw">this</span>.heap.length; }
  <span class="fn">peek</span>() { <span class="kw">return</span> <span class="kw">this</span>.heap[<span class="num">0</span>]; }
}

<span class="cm">// Get top K recommendations by score</span>
<span class="kw">function</span> <span class="fn">getTopK</span>(candidates, k) {
  <span class="kw">const</span> heap = <span class="kw">new</span> <span class="cls">MinHeap</span>();
  <span class="kw">for</span> (<span class="kw">const</span> c <span class="kw">of</span> candidates) {
    heap.<span class="fn">push</span>(c);
    <span class="kw">if</span> (heap.<span class="fn">size</span>() > k) heap.<span class="fn">pop</span>(); <span class="cm">// evict lowest</span>
  }
  <span class="kw">return</span> heap.heap; <span class="cm">// O(N log K) vs O(N log N) for sort</span>
}</pre>
    </div>

    <table class="complexity-table" style="margin-top:20px;">
      <thead>
        <tr>
          <th>Operation</th>
          <th>Time Complexity</th>
          <th>Why</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Heap insert</td>
          <td><span class="big-o">O(log K)</span></td>
          <td>Tree height is log(K), bubble up at most that many steps</td>
        </tr>
        <tr>
          <td>Top-K from N</td>
          <td><span class="big-o">O(N log K)</span></td>
          <td>N inserts × log K each — K stays fixed and small</td>
        </tr>
        <tr>
          <td>Sort (comparison)</td>
          <td><span style="color:var(--orange)">O(N log N)</span></td>
          <td>Sorts the whole list even though you only need K items</td>
        </tr>
      </tbody>
    </table>
  </div>

  <!-- CHECKLIST -->
  <div class="checklist-section">
    <div class="checklist-title">Phase 2 <span>Checklist</span></div>
    <p class="checklist-sub">Track your progress — check off each item as you complete it.</p>

    <div class="checklist-groups">

      <!-- Auth -->
      <div class="cl-group">
        <div class="cl-group-header">
          <span style="color:var(--cyan)">⬡</span>
          <h3>Authentication</h3>
          <span class="cl-count">5 items</span>
        </div>
        <div class="cl-items">
          <div class="cl-item"><input type="checkbox" id="a1"><label for="a1">User can register with email + password</label></div>
          <div class="cl-item"><input type="checkbox" id="a2"><label for="a2">Passwords are hashed — DB shows bcrypt hash, not plaintext</label></div>
          <div class="cl-item"><input type="checkbox" id="a3"><label for="a3">User can log in and receives a JWT token</label></div>
          <div class="cl-item"><input type="checkbox" id="a4"><label for="a4"><code style="font-size:10px;color:var(--orange)">/api/user/history</code> returns 401 if no token</label></div>
          <div class="cl-item"><input type="checkbox" id="a5"><label for="a5">Protected route returns data with valid token</label></div>
        </div>
      </div>

      <!-- Watch History -->
      <div class="cl-group">
        <div class="cl-group-header">
          <span style="color:var(--orange)">⬡</span>
          <h3>Watch History</h3>
          <span class="cl-count">3 items</span>
        </div>
        <div class="cl-items">
          <div class="cl-item"><input type="checkbox" id="w1"><label for="w1">"Mark as Watched" saves record to WatchHistory table</label></div>
          <div class="cl-item"><input type="checkbox" id="w2"><label for="w2">"Like ❤️" sets the <code style="font-size:10px;color:var(--orange)">liked</code> column to <code style="font-size:10px;color:var(--green)">true</code></label></div>
          <div class="cl-item"><input type="checkbox" id="w3"><label for="w3">Can query and display user's last 10 watched movies</label></div>
        </div>
      </div>

      <!-- ML Service -->
      <div class="cl-group">
        <div class="cl-group-header">
          <span style="color:var(--purple)">⬡</span>
          <h3>ML Service</h3>
          <span class="cl-count">5 items</span>
        </div>
        <div class="cl-items">
          <div class="cl-item"><input type="checkbox" id="m1"><label for="m1">Python Flask server starts on port 5001 with no errors</label></div>
          <div class="cl-item"><input type="checkbox" id="m2"><label for="m2"><code style="font-size:10px;color:var(--orange)">POST /recommend</code> with 2 liked IDs returns 10 movie IDs</label></div>
          <div class="cl-item"><input type="checkbox" id="m3"><label for="m3">Recommendations make sense thematically (test manually)</label></div>
          <div class="cl-item"><input type="checkbox" id="m4"><label for="m4">No history → shows popular movies instead of crashing</label></div>
          <div class="cl-item"><input type="checkbox" id="m5"><label for="m5">Node calls Python service → returns enriched results to React</label></div>
        </div>
      </div>

      <!-- DSA Heap -->
      <div class="cl-group">
        <div class="cl-group-header">
          <span style="color:var(--green)">⬡</span>
          <h3>DSA — Heap</h3>
          <span class="cl-count">4 items</span>
        </div>
        <div class="cl-items">
          <div class="cl-item"><input type="checkbox" id="d1"><label for="d1">MinHeap class with <code style="font-size:10px;color:var(--orange)">push()</code> and <code style="font-size:10px;color:var(--orange)">pop()</code> methods</label></div>
          <div class="cl-item"><input type="checkbox" id="d2"><label for="d2"><code style="font-size:10px;color:var(--orange)">getTopK([...100 items], 10)</code> returns correct top 10</label></div>
          <div class="cl-item"><input type="checkbox" id="d3"><label for="d3">Can state time complexity of heap insert & top-K from N</label></div>
          <div class="cl-item"><input type="checkbox" id="d4"><label for="d4">Explain in plain English: heap vs <code style="font-size:10px;color:var(--orange)">.sort()</code> for top-K</label></div>
        </div>
      </div>

    </div>
  </div>

  <!-- FOOTER -->
  <div style="margin-top:60px;padding-top:32px;border-top:1px solid var(--border);display:flex;justify-content:space-between;align-items:center;flex-wrap:gap;gap:12px;">
    <div>
      <div style="font-size:10px;color:var(--dim);letter-spacing:0.1em;text-transform:uppercase;margin-bottom:4px;">Phase 2 complete when...</div>
      <div style="font-size:12px;color:var(--muted)">
        Auth works · Watch history saves · Python returns real recommendations · Heap is implemented
      </div>
    </div>
    <div style="text-align:right;flex-shrink:0;">
      <div style="font-size:10px;color:var(--dim);letter-spacing:0.1em;margin-bottom:4px;">NEXT</div>
      <div style="font-family:'Syne',sans-serif;font-size:14px;font-weight:700;color:var(--green)">Phase 3 →</div>
    </div>
  </div>

</div>

<script>
  // Animate progress bars on load
  window.addEventListener('load', () => {
    document.querySelectorAll('.prog-fill').forEach(bar => {
      const w = bar.style.width;
      bar.style.width = '0%';
      setTimeout(() => { bar.style.width = w; }, 200);
    });
  });

  // Checkbox persistence via localStorage
  document.querySelectorAll('input[type="checkbox"]').forEach(cb => {
    const key = 'phase2_' + cb.id;
    if (localStorage.getItem(key) === 'true') cb.checked = true;
    cb.addEventListener('change', () => {
      localStorage.setItem(key, cb.checked);
    });
  });

  // Today focus persistence
  const tf = document.querySelector('.today-input');
  tf.value = localStorage.getItem('phase2_today') || '';
  tf.addEventListener('input', () => localStorage.setItem('phase2_today', tf.value));
</script>
</body>
</html>
