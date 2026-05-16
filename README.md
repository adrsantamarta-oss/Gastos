<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0"/>
<meta name="apple-mobile-web-app-capable" content="yes"/>
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent"/>
<meta name="apple-mobile-web-app-title" content="Gastos"/>
<meta name="theme-color" content="#0e0f14"/>
<title>Gastos del Mes</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;600;700&family=Bebas+Neue&display=swap" rel="stylesheet"/>
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0;}
:root{
  --bg:#0e0f14;
  --card:#16181f;
  --card2:#1c1f28;
  --border:#252833;
  --text:#f0f2f8;
  --muted:#6b7280;
  --dim:#3a3f50;
  --income:#34d399;
  --expense:#f87171;
  --accent:#818cf8;
  --warn:#fbbf24;
}
html,body{height:100%;background:var(--bg);color:var(--text);font-family:'Space Grotesk',sans-serif;}
body{display:flex;flex-direction:column;align-items:center;padding:24px 14px 100px;min-height:100vh;}
.wrap{width:100%;max-width:440px;}

/* Header */
.header{margin-bottom:22px;}
.header-top{display:flex;justify-content:space-between;align-items:flex-start;}
.app-title{font-family:'Bebas Neue',sans-serif;font-size:36px;letter-spacing:2px;color:#fff;line-height:1;}
.app-sub{font-size:10px;color:var(--muted);letter-spacing:3px;margin-top:3px;}
.month-badge{background:var(--card2);border:1px solid var(--border);border-radius:20px;padding:6px 14px;font-size:11px;color:var(--accent);letter-spacing:1px;cursor:pointer;transition:background 0.2s;}
.month-badge:hover{background:var(--border);}

/* Month selector */
#month-selector{display:none;background:var(--card);border:1px solid var(--border);border-radius:14px;padding:14px;margin-bottom:14px;display:none;}
#month-selector.open{display:block;}
.month-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:6px;}
.month-btn{background:var(--card2);border:1px solid var(--border);border-radius:8px;padding:7px 4px;font-size:11px;color:var(--muted);cursor:pointer;text-align:center;font-family:inherit;transition:all 0.2s;}
.month-btn.active{background:var(--accent);color:#fff;border-color:var(--accent);}
.month-btn:hover:not(.active){background:var(--border);color:var(--text);}
.year-row{display:flex;justify-content:space-between;align-items:center;margin-bottom:10px;}
.year-btn{background:none;border:none;color:var(--muted);font-size:18px;cursor:pointer;padding:4px 10px;border-radius:6px;}
.year-btn:hover{color:var(--text);}
.year-label{font-size:13px;color:var(--text);font-weight:600;}

/* Summary card */
.summary{background:var(--card);border:1px solid var(--border);border-radius:18px;padding:20px;margin-bottom:14px;position:relative;overflow:hidden;}
.summary::before{content:'';position:absolute;top:-40px;right:-40px;width:120px;height:120px;border-radius:50%;opacity:0.06;background:var(--accent);}
.summary-label{font-size:10px;letter-spacing:3px;color:var(--muted);margin-bottom:6px;}
.balance-amount{font-family:'Bebas Neue',sans-serif;font-size:52px;letter-spacing:1px;line-height:1;transition:color 0.4s;}
.balance-amount.pos{color:var(--income);}
.balance-amount.neg{color:var(--expense);}
.balance-amount.zero{color:var(--muted);}
.summary-row{display:flex;gap:12px;margin-top:16px;}
.summary-pill{flex:1;background:var(--card2);border-radius:12px;padding:12px;border:1px solid var(--border);}
.pill-label{font-size:9px;letter-spacing:2px;color:var(--muted);margin-bottom:4px;}
.pill-val{font-size:16px;font-weight:700;}
.pill-val.inc{color:var(--income);}
.pill-val.exp{color:var(--expense);}

/* Progress bar */
.progress-wrap{margin-top:14px;}
.progress-label{display:flex;justify-content:space-between;font-size:10px;color:var(--muted);margin-bottom:5px;letter-spacing:1px;}
.progress-bg{height:6px;background:var(--dim);border-radius:3px;overflow:hidden;}
.progress-fill{height:100%;border-radius:3px;transition:width 0.5s cubic-bezier(.4,0,.2,1),background 0.4s;}

/* Sueldo */
.sueldo-card{background:var(--card);border:1px solid var(--border);border-radius:18px;padding:16px 18px;margin-bottom:14px;}
.sueldo-header{display:flex;justify-content:space-between;align-items:center;margin-bottom:10px;}
.sueldo-label{font-size:10px;letter-spacing:2px;color:var(--muted);}
.sueldo-edit{font-size:11px;color:var(--accent);cursor:pointer;text-decoration:none;background:none;border:none;font-family:inherit;}
.sueldo-display{display:flex;align-items:baseline;gap:6px;}
.sueldo-amount{font-family:'Bebas Neue',sans-serif;font-size:32px;color:var(--income);letter-spacing:1px;}
.sueldo-currency{font-size:14px;color:var(--muted);}
.sueldo-form{display:flex;gap:8px;}
.sueldo-input{flex:1;background:#0a0b10;border:1px solid var(--accent);border-radius:10px;color:var(--text);padding:10px 13px;font-size:15px;font-family:inherit;outline:none;}

/* Add form */
.add-card{background:var(--card);border:1px solid var(--border);border-radius:18px;padding:16px 18px;margin-bottom:14px;}
.add-label{font-size:10px;letter-spacing:2px;color:var(--muted);margin-bottom:12px;}
.type-toggle{display:flex;gap:6px;margin-bottom:12px;}
.type-btn{flex:1;padding:9px;border:1px solid var(--border);border-radius:10px;font-size:12px;font-weight:600;cursor:pointer;font-family:inherit;transition:all 0.2s;background:var(--card2);color:var(--muted);}
.type-btn.active-inc{background:#065f46;border-color:var(--income);color:var(--income);}
.type-btn.active-exp{background:#7f1d1d;border-color:var(--expense);color:var(--expense);}
.add-row{display:flex;gap:8px;margin-bottom:8px;}
.add-input{flex:2 1 130px;background:#0a0b10;border:1px solid var(--border);border-radius:10px;color:var(--text);padding:11px 13px;font-size:14px;font-family:inherit;outline:none;transition:border 0.2s;}
.add-input:focus{border-color:#818cf855;}
.add-input.num{flex:1 1 90px;}
.btn-add{flex:0 0 auto;border:none;border-radius:10px;padding:11px 18px;font-size:13px;font-weight:700;font-family:inherit;cursor:pointer;transition:all 0.2s;letter-spacing:0.5px;}
.btn-add.inc{background:var(--income);color:#022c22;}
.btn-add.exp{background:var(--expense);color:#450a0a;}
.btn-add:hover{opacity:0.85;transform:scale(0.97);}

/* Transaction list */
.list-card{background:var(--card);border:1px solid var(--border);border-radius:18px;overflow:hidden;margin-bottom:14px;}
.list-header{display:flex;justify-content:space-between;align-items:center;padding:13px 18px 11px;border-bottom:1px solid var(--border);}
.list-title{font-size:10px;letter-spacing:2px;color:var(--muted);}
.btn-clear{background:none;border:none;font-size:10px;color:var(--dim);cursor:pointer;font-family:inherit;letter-spacing:1px;}
.btn-clear:hover{color:var(--expense);}
.tx-row{display:flex;align-items:center;gap:10px;padding:12px 16px;border-bottom:1px solid #1a1d25;transition:background 0.15s;}
.tx-row:last-child{border-bottom:none;}
.tx-row:hover{background:#1a1d25;}
.tx-icon{width:36px;height:36px;border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:16px;flex-shrink:0;}
.tx-icon.inc{background:#065f4622;}
.tx-icon.exp{background:#7f1d1d22;}
.tx-info{flex:1;min-width:0;}
.tx-desc{font-size:13px;font-weight:500;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
.tx-date{font-size:10px;color:var(--muted);margin-top:1px;}
.tx-amount{font-size:14px;font-weight:700;white-space:nowrap;}
.tx-amount.inc{color:var(--income);}
.tx-amount.exp{color:var(--expense);}
.btn-del-tx{background:none;border:none;color:var(--dim);cursor:pointer;font-size:14px;padding:4px;border-radius:4px;transition:color 0.15s;line-height:1;}
.btn-del-tx:hover{color:var(--expense);}
.empty-tx{padding:28px;text-align:center;color:var(--dim);font-size:12px;letter-spacing:1px;}

/* Notification */
#notif{position:fixed;top:16px;right:16px;z-index:999;background:#1c1f28;border:1px solid var(--border);color:var(--income);padding:10px 16px;border-radius:8px;font-size:12px;letter-spacing:1px;opacity:0;transform:translateY(-10px);transition:all 0.25s;pointer-events:none;}
#notif.show{opacity:1;transform:translateY(0);}

@media(max-width:400px){
  .add-input,.sueldo-input{font-size:16px;}
  .balance-amount{font-size:44px;}
}
</style>
</head>
<body>
<div class="wrap">
<div id="notif"></div>

<!-- Header -->
<div class="header">
  <div class="header-top">
    <div>
      <div class="app-title">GASTOS</div>
      <div class="app-sub">CONTROL MENSUAL</div>
    </div>
    <button class="month-badge" id="month-toggle" onclick="toggleMonthPicker()">
      <span id="month-badge-label">MES</span>
    </button>
  </div>
</div>

<!-- Month picker -->
<div id="month-selector">
  <div class="year-row">
    <button class="year-btn" onclick="changeYear(-1)">‹</button>
    <span class="year-label" id="year-label">2026</span>
    <button class="year-btn" onclick="changeYear(1)">›</button>
  </div>
  <div class="month-grid" id="month-grid"></div>
</div>

<!-- Balance summary -->
<div class="summary">
  <div class="summary-label">BALANCE DEL MES</div>
  <div class="balance-amount zero" id="balance-display">€0</div>
  <div class="summary-row">
    <div class="summary-pill">
      <div class="pill-label">INGRESOS</div>
      <div class="pill-val inc" id="total-inc">€0</div>
    </div>
    <div class="summary-pill">
      <div class="pill-label">GASTOS</div>
      <div class="pill-val exp" id="total-exp">€0</div>
    </div>
    <div class="summary-pill">
      <div class="pill-label">TRANSACC.</div>
      <div class="pill-val" id="total-count" style="color:var(--accent)">0</div>
    </div>
  </div>
  <div class="progress-wrap">
    <div class="progress-label">
      <span>GASTOS vs INGRESOS</span>
      <span id="pct-label">0%</span>
    </div>
    <div class="progress-bg">
      <div class="progress-fill" id="progress-fill" style="width:0%"></div>
    </div>
  </div>
</div>

<!-- Sueldo -->
<div class="sueldo-card">
  <div class="sueldo-header">
    <span class="sueldo-label">SUELDO DEL MES</span>
    <button class="sueldo-edit" onclick="toggleSueldoEdit()">✏️ editar</button>
  </div>
  <div id="sueldo-display" class="sueldo-display">
    <span class="sueldo-amount" id="sueldo-amount-label">€0</span>
    <span class="sueldo-currency">este mes</span>
  </div>
  <div id="sueldo-form" style="display:none" class="sueldo-form">
    <input class="sueldo-input" id="sueldo-inp" type="number" min="0" step="any" placeholder="Tu sueldo este mes €"/>
    <button class="btn-add inc" onclick="saveSueldo()">✓</button>
  </div>
</div>

<!-- Add transaction -->
<div class="add-card">
  <div class="add-label">AÑADIR MOVIMIENTO</div>
  <div class="type-toggle">
    <button class="type-btn active-exp" id="btn-type-exp" onclick="setType('expense')">↓ Gasto</button>
    <button class="type-btn" id="btn-type-inc" onclick="setType('income')">↑ Ingreso extra</button>
  </div>
  <div class="add-row">
    <input class="add-input" id="inp-desc" type="text" placeholder="Descripción (ej: Supermercado)" autocomplete="off"/>
    <input class="add-input num" id="inp-amount" type="number" min="0" step="any" placeholder="€"/>
  </div>
  <button class="btn-add exp" id="btn-add-tx" onclick="addTransaction()" style="width:100%;margin-top:0;">+ Añadir gasto</button>
</div>

<!-- Transaction list -->
<div class="list-card">
  <div class="list-header">
    <span class="list-title">MOVIMIENTOS</span>
    <button class="btn-clear" onclick="clearAll()">Borrar todo</button>
  </div>
  <div id="tx-list"></div>
</div>

</div>
<script>
const STORAGE_KEY = "gastos_v2";
const MONTHS = ["Enero","Febrero","Marzo","Abril","Mayo","Junio","Julio","Agosto","Septiembre","Octubre","Noviembre","Diciembre"];
const MONTH_SHORT = ["ENE","FEB","MAR","ABR","MAY","JUN","JUL","AGO","SEP","OCT","NOV","DIC"];

let currentType = "expense";
let pickerYear = new Date().getFullYear();
let currentMonth = new Date().getMonth();
let currentYear = new Date().getFullYear();
let data = {};
let nextId = Date.now();

function storageKey(y, m) { return `${STORAGE_KEY}_${y}_${m}`; }

function loadMonth(y, m) {
  try {
    const raw = localStorage.getItem(storageKey(y, m));
    return raw ? JSON.parse(raw) : { sueldo: 0, transactions: [] };
  } catch(e) { return { sueldo: 0, transactions: [] }; }
}

function saveMonth(y, m) {
  localStorage.setItem(storageKey(y, m), JSON.stringify(data));
}

function fmt(v) {
  return new Intl.NumberFormat("es-ES", { style:"currency", currency:"EUR", minimumFractionDigits:2 }).format(v);
}

// Notification
let notifTimer;
function notify(msg, color) {
  const el = document.getElementById("notif");
  el.textContent = msg;
  el.style.color = color || "var(--income)";
  el.classList.add("show");
  clearTimeout(notifTimer);
  notifTimer = setTimeout(() => el.classList.remove("show"), 2100);
}

// Month picker
function toggleMonthPicker() {
  const el = document.getElementById("month-selector");
  el.classList.toggle("open");
  pickerYear = currentYear;
  renderMonthGrid();
}

function changeYear(d) {
  pickerYear += d;
  document.getElementById("year-label").textContent = pickerYear;
  renderMonthGrid();
}

function renderMonthGrid() {
  document.getElementById("year-label").textContent = pickerYear;
  const grid = document.getElementById("month-grid");
  grid.innerHTML = "";
  MONTH_SHORT.forEach((m, i) => {
    const btn = document.createElement("button");
    btn.className = "month-btn" + (i === currentMonth && pickerYear === currentYear ? " active" : "");
    btn.textContent = m;
    btn.onclick = () => selectMonth(pickerYear, i);
    grid.appendChild(btn);
  });
}

function selectMonth(y, m) {
  currentYear = y; currentMonth = m;
  data = loadMonth(y, m);
  document.getElementById("month-selector").classList.remove("open");
  updateBadge();
  render();
}

function updateBadge() {
  document.getElementById("month-badge-label").textContent = `${MONTH_SHORT[currentMonth]} ${currentYear}`;
}

// Sueldo
function toggleSueldoEdit() {
  const form = document.getElementById("sueldo-form");
  const display = document.getElementById("sueldo-display");
  const isOpen = form.style.display !== "none";
  form.style.display = isOpen ? "none" : "flex";
  display.style.display = isOpen ? "flex" : "none";
  if (!isOpen) {
    const inp = document.getElementById("sueldo-inp");
    inp.value = data.sueldo || "";
    inp.focus();
  }
}

function saveSueldo() {
  const val = parseFloat(document.getElementById("sueldo-inp").value);
  if (!isNaN(val) && val >= 0) {
    data.sueldo = val;
    saveMonth(currentYear, currentMonth);
    notify("Sueldo guardado ✓");
    toggleSueldoEdit();
    render();
  }
}

document.getElementById("sueldo-inp").addEventListener("keydown", e => { if(e.key === "Enter") saveSueldo(); });

// Type toggle
function setType(type) {
  currentType = type;
  const expBtn = document.getElementById("btn-type-exp");
  const incBtn = document.getElementById("btn-type-inc");
  const addBtn = document.getElementById("btn-add-tx");
  const descInp = document.getElementById("inp-desc");
  if (type === "expense") {
    expBtn.className = "type-btn active-exp";
    incBtn.className = "type-btn";
    addBtn.className = "btn-add exp";
    addBtn.textContent = "+ Añadir gasto";
    descInp.placeholder = "Descripción (ej: Supermercado)";
  } else {
    incBtn.className = "type-btn active-inc";
    expBtn.className = "type-btn";
    addBtn.className = "btn-add inc";
    addBtn.textContent = "+ Añadir ingreso";
    descInp.placeholder = "Descripción (ej: Freelance, Bonus...)";
  }
}

// Add transaction
document.getElementById("inp-amount").addEventListener("keydown", e => { if(e.key === "Enter") addTransaction(); });
document.getElementById("inp-desc").addEventListener("keydown", e => { if(e.key === "Enter") document.getElementById("inp-amount").focus(); });

function addTransaction() {
  const desc = document.getElementById("inp-desc").value.trim();
  const amount = parseFloat(document.getElementById("inp-amount").value);
  if (!desc || isNaN(amount) || amount <= 0) {
    notify("Rellena descripción y cantidad", "var(--expense)"); return;
  }
  const now = new Date();
  data.transactions.unshift({
    id: nextId++,
    type: currentType,
    desc,
    amount,
    date: now.toLocaleDateString("es-ES", { day:"2-digit", month:"short" })
  });
  saveMonth(currentYear, currentMonth);
  document.getElementById("inp-desc").value = "";
  document.getElementById("inp-amount").value = "";
  document.getElementById("inp-desc").focus();
  notify(currentType === "expense" ? "Gasto añadido" : "Ingreso añadido ✓", currentType === "expense" ? "var(--expense)" : "var(--income)");
  render();
}

function deleteTransaction(id) {
  data.transactions = data.transactions.filter(t => t.id !== id);
  saveMonth(currentYear, currentMonth);
  render();
}

function clearAll() {
  if (!data.transactions.length) return;
  if (!confirm("¿Borrar todos los movimientos de este mes?")) return;
  data.transactions = [];
  saveMonth(currentYear, currentMonth);
  notify("Movimientos borrados", "var(--muted)");
  render();
}

// Render
function render() {
  const sueldo = data.sueldo || 0;
  const txs = data.transactions || [];

  const totalInc = sueldo + txs.filter(t => t.type === "income").reduce((a, t) => a + t.amount, 0);
  const totalExp = txs.filter(t => t.type === "expense").reduce((a, t) => a + t.amount, 0);
  const balance = totalInc - totalExp;

  // Sueldo display
  document.getElementById("sueldo-amount-label").textContent = fmt(sueldo);

  // Balance
  const balEl = document.getElementById("balance-display");
  balEl.textContent = (balance >= 0 ? "+" : "") + fmt(balance);
  balEl.className = "balance-amount " + (balance > 0 ? "pos" : balance < 0 ? "neg" : "zero");

  // Pills
  document.getElementById("total-inc").textContent = fmt(totalInc);
  document.getElementById("total-exp").textContent = fmt(totalExp);
  document.getElementById("total-count").textContent = txs.length;

  // Progress
  const pct = totalInc > 0 ? Math.min((totalExp / totalInc) * 100, 100) : (totalExp > 0 ? 100 : 0);
  const fill = document.getElementById("progress-fill");
  fill.style.width = pct + "%";
  fill.style.background = pct >= 100 ? "var(--expense)" : pct >= 80 ? "var(--warn)" : "var(--income)";
  document.getElementById("pct-label").textContent = Math.round(pct) + "%";

  // Transaction list
  const list = document.getElementById("tx-list");
  if (!txs.length) {
    list.innerHTML = '<div class="empty-tx">Sin movimientos este mes</div>';
    return;
  }
  list.innerHTML = "";
  txs.forEach(t => {
    const row = document.createElement("div");
    row.className = "tx-row";
    const emoji = t.type === "income" ? "💰" : guessEmoji(t.desc);
    row.innerHTML = `
      <div class="tx-icon ${t.type === "income" ? "inc" : "exp"}">${emoji}</div>
      <div class="tx-info">
        <div class="tx-desc">${t.desc}</div>
        <div class="tx-date">${t.date}</div>
      </div>
      <div class="tx-amount ${t.type === "income" ? "inc" : "exp"}">${t.type === "income" ? "+" : "-"}${fmt(t.amount)}</div>
      <button class="btn-del-tx" onclick="deleteTransaction(${t.id})">✕</button>
    `;
    list.appendChild(row);
  });
}

function guessEmoji(desc) {
  const d = desc.toLowerCase();
  if (/super|mercado|fruta|carnicer|panad|alimenta|comida|cena|restaur|bar|cafe|café/.test(d)) return "🛒";
  if (/luz|agua|gas|electric|suministr/.test(d)) return "⚡";
  if (/alquiler|hipotec|piso|casa|rent/.test(d)) return "🏠";
  if (/gasolina|coche|parking|transport|metro|bus|taxi|uber/.test(d)) return "🚗";
  if (/ropa|zara|h&m|mango|nike|adidas|zapato/.test(d)) return "👕";
  if (/gym|deporte|fitness|sport/.test(d)) return "💪";
  if (/netflix|spotify|amazon|prime|suscri/.test(d)) return "📱";
  if (/médic|farmaci|salud|doctor|clínic/.test(d)) return "🏥";
  if (/viaje|hotel|vuelo|avión|vacacion/.test(d)) return "✈️";
  if (/seguro/.test(d)) return "🛡️";
  if (/teléfon|móvil|internet/.test(d)) return "📡";
  return "💸";
}

// Init
data = loadMonth(currentYear, currentMonth);
updateBadge();
render();
</script>
</body>
</html>
