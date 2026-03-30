# retirement-planner
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Retirement Planner</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=Spectral:ital,wght@0,300;0,400;0,600;1,300&display=swap" rel="stylesheet" />
<style>
/* ── Reset ── */
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
:root{
--ink:#0b1524;
--ink2:#0e1c30;
--ink3:#152a47;
--ink4:#1d3a60;
--rule:#1e3a5f;
--rule2:#2a4a70;
--fog:#94a3b8;
--mist:#475569;
--paper:#e2e8f0;
--sky:#60a5fa;
--sky2:#3b82f6;
--mint:#34d399;
--mint2:#10b981;
--lavender:#a78bfa;
--amber:#fb923c;
--rose:#f472b6;
--red:#f87171;
--gold:#fbbf24;
--r:10px;
--r-sm:6px;
--sh:0 8px 32px rgba(0,0,0,.45);
--sh-sm:0 2px 12px rgba(0,0,0,.3);
}
html{scroll-behavior:smooth}
body{
font-family:'Spectral',Georgia,serif;
background:var(--ink);
color:var(--paper);
min-height:100vh;
font-size:15px;
line-height:1.55;
}
input[type=number]{-moz-appearance:textfield}
input[type=number]::-webkit-outer-spin-button,
input[type=number]::-webkit-inner-spin-button{-webkit-appearance:none;margin:0}
button{font-family:'Syne',sans-serif;cursor:pointer}

/* ── Header ── */
.site-header{
background:linear-gradient(160deg,#0a1828 0%,#06111f 100%);
border-bottom:1px solid var(--rule);
position:sticky;top:0;z-index:200;
}
.header-inner{
max-width:1280px;margin:0 auto;
padding:.85rem 1.5rem;
display:flex;align-items:center;justify-content:space-between;gap:1rem;flex-wrap:wrap;
}
.brand{display:flex;align-items:center;gap:.75rem}
.brand-icon{
width:40px;height:40px;border-radius:10px;
background:linear-gradient(135deg,var(--sky2),var(--lavender));
display:flex;align-items:center;justify-content:center;
font-size:1.1rem;flex-shrink:0;
}
.brand-text h1{
font-family:'Syne',sans-serif;font-size:1.15rem;font-weight:800;
color:var(--paper);letter-spacing:-.01em;line-height:1;
}
.brand-text p{font-size:.68rem;color:var(--fog);font-style:italic;margin-top:2px}
.header-kpis{display:flex;gap:1.5rem}
.hkpi{text-align:right}
.hkpi-label{font-size:.62rem;color:var(--fog);text-transform:uppercase;letter-spacing:.1em;font-family:'Syne',sans-serif}
.hkpi-val{font-family:'Syne',sans-serif;font-size:1.2rem;font-weight:800;color:var(--sky);line-height:1}
.hkpi-val.green{color:var(--mint)}

/* ── Nav ── */
.site-nav{background:var(--ink2);border-bottom:1px solid var(--rule);position:sticky;top:63px;z-index:190}
.nav-inner{
max-width:1280px;margin:0 auto;
padding:0 1rem;display:flex;overflow-x:auto;gap:.1rem;
scrollbar-width:none;
}
.nav-inner::-webkit-scrollbar{display:none}
.nav-btn{
background:none;border:none;color:var(--fog);
font-size:.78rem;font-weight:600;letter-spacing:.04em;text-transform:uppercase;
padding:.7rem 1.1rem;white-space:nowrap;
border-bottom:2px solid transparent;
transition:color .2s,border-color .2s;
}
.nav-btn:hover{color:var(--paper)}
.nav-btn.active{color:var(--sky);border-bottom-color:var(--sky)}

/* ── Layout ── */
.page{display:none;max-width:1280px;margin:0 auto;padding:1.5rem;animation:fadeIn .25s ease}
.page.active{display:block}
@keyframes fadeIn{from{opacity:0;transform:translateY(6px)}to{opacity:1;transform:none}}

/* ── Cards ── */
.card{
background:var(--ink2);border:1px solid var(--rule);
border-radius:var(--r);padding:1.35rem 1.5rem;
box-shadow:var(--sh-sm);
}
.card+.card,.card+.grid,.grid+.card,.grid+.grid{margin-top:1.25rem}
.card-title{
font-family:'Syne',sans-serif;font-size:.72rem;font-weight:700;
text-transform:uppercase;letter-spacing:.1em;color:var(--sky);
margin-bottom:1.1rem;
}

/* ── Grid layouts ── */
.grid-2{display:grid;grid-template-columns:1fr 1fr;gap:1.25rem}
.grid-3{display:grid;grid-template-columns:repeat(3,1fr);gap:1.25rem}
.grid-4{display:grid;grid-template-columns:repeat(4,1fr);gap:1rem}
.grid-5{display:grid;grid-template-columns:repeat(5,1fr);gap:1rem}
@media(max-width:900px){
.grid-2,.grid-3,.grid-4,.grid-5{grid-template-columns:1fr 1fr}
}
@media(max-width:550px){
.grid-2,.grid-3,.grid-4,.grid-5{grid-template-columns:1fr}
}

/* ── KPI Cards ── */
.kpi-strip{display:grid;grid-template-columns:repeat(4,1fr);gap:1rem;margin-bottom:1.25rem}
@media(max-width:800px){.kpi-strip{grid-template-columns:1fr 1fr}}
@media(max-width:420px){.kpi-strip{grid-template-columns:1fr}}
.kpi{
background:var(--ink2);border:1px solid var(--rule);
border-radius:var(--r);padding:1.1rem 1.2rem;
border-top:2px solid var(--accent,var(--sky));
position:relative;overflow:hidden;
}
.kpi::after{
content:'';position:absolute;inset:0;
background:linear-gradient(135deg,rgba(255,255,255,.03) 0%,transparent 60%);
pointer-events:none;
}
.kpi-label{font-family:'Syne',sans-serif;font-size:.62rem;font-weight:700;text-transform:uppercase;letter-spacing:.1em;color:var(--fog);margin-bottom:.35rem}
.kpi-val{font-family:'Syne',sans-serif;font-size:1.6rem;font-weight:800;line-height:1;color:var(--accent,var(--sky));font-variant-numeric:tabular-nums}
.kpi-sub{font-size:.68rem;color:var(--mist);margin-top:.25rem;font-style:italic}

/* ── Input rows ── */
.input-stack{display:flex;flex-direction:column;gap:.55rem}
.irow{display:grid;grid-template-columns:1fr auto;align-items:center;gap:.5rem 1rem}
.irow-label{font-size:.8rem;color:var(--fog)}
.irow-note{font-size:.65rem;color:var(--mist);grid-column:1/-1;font-style:italic}
.ifield-wrap{display:flex;align-items:center}
.ifix{
background:var(--ink3);border:1px solid var(--rule2);
color:var(--mist);font-size:.8rem;height:32px;
padding:0 .45rem;display:flex;align-items:center;font-family:'Syne',sans-serif;
}
.ifix.pre{border-right:none;border-radius:var(--r-sm) 0 0 var(--r-sm)}
.ifix.suf{border-left:none;border-radius:0 var(--r-sm) var(--r-sm) 0}
.ifield{
background:var(--ink);border:1px solid var(--rule2);
color:var(--sky);font-weight:700;font-size:.85rem;
padding:0 .6rem;height:32px;width:115px;
font-family:'Syne',sans-serif;transition:border-color .2s;
}
.ifield:focus{outline:none;border-color:var(--sky)}
.ifield.pre{border-radius:0 var(--r-sm) var(--r-sm) 0;border-left:none}
.ifield.suf{border-radius:var(--r-sm) 0 0 var(--r-sm);border-right:none}
.ifield.bare{border-radius:var(--r-sm)}

/* ── Account cards ── */
.acct-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(170px,1fr));gap:1rem}
.acct{
background:var(--ink3);border:1px solid var(--rule);
border-top:2px solid var(--ac,var(--sky));
border-radius:var(--r-sm);padding:.9rem 1rem;
}
.acct-dot{width:8px;height:8px;border-radius:50%;background:var(--ac,var(--sky));margin-bottom:.4rem}
.acct-name{font-family:'Syne',sans-serif;font-size:.7rem;font-weight:700;color:var(--fog);text-transform:uppercase;letter-spacing:.06em;margin-bottom:.5rem}
.acct-ifield{
background:var(--ink);border:1px solid var(--ac,var(--sky));
color:var(--sky);font-weight:700;font-size:.95rem;
padding:.3rem .5rem;width:100%;border-radius:var(--r-sm);
font-family:'Syne',sans-serif;
}
.acct-ifield:focus{outline:none;box-shadow:0 0 0 2px var(--ac,var(--sky))44}
.acct-note{font-size:.65rem;color:var(--mist);margin-top:.35rem;font-style:italic}
.acct-total{
background:var(--ink3);border:1px solid var(--mint);
border-top:2px solid var(--mint);border-radius:var(--r-sm);
padding:.9rem 1rem;display:flex;flex-direction:column;align-items:center;justify-content:center;text-align:center;
}
.acct-total-label{font-family:'Syne',sans-serif;font-size:.65rem;font-weight:700;color:var(--fog);text-transform:uppercase;letter-spacing:.08em;margin-bottom:.35rem}
.acct-total-val{font-family:'Syne',sans-serif;font-size:1.3rem;font-weight:800;color:var(--mint)}

/* ── Income table ── */
.income-list{display:flex;flex-direction:column}
.income-row{
display:flex;align-items:center;justify-content:space-between;
padding:.55rem 0;border-bottom:1px solid var(--rule);gap:1rem;
}
.income-row:last-child{border-bottom:none}
.income-row.hl{background:#0c2518;margin:0 -1.5rem;padding:.55rem 1.5rem;border-left:3px solid var(--mint)}
.income-lbl{font-size:.8rem;color:var(--fog)}
.income-val{font-family:'Syne',sans-serif;font-size:.95rem;font-weight:800;font-variant-numeric:tabular-nums}
.income-note{font-size:.65rem;color:var(--rose);width:100%;font-style:italic}

/* ── Tab bar ── */
.tab-bar{display:flex;gap:.35rem;flex-wrap:wrap;margin-bottom:1.25rem}
.tab{
background:transparent;border:1px solid var(--rule2);
color:var(--fog);font-size:.72rem;font-weight:700;letter-spacing:.04em;
padding:.35rem .85rem;border-radius:var(--r-sm);
text-transform:uppercase;transition:all .2s;
}
.tab:hover{border-color:var(--sky);color:var(--paper)}
.tab.active{background:var(--sky2);border-color:var(--sky2);color:#fff}

/* ── Chart ── */
.chart-wrap{position:relative;width:100%}
.chart-note{font-size:.75rem;color:var(--mist);margin-bottom:1rem;font-style:italic;line-height:1.5}
.legend{display:flex;flex-wrap:wrap;gap:.85rem;margin-top:1rem;justify-content:center}
.leg-item{display:flex;align-items:center;gap:.4rem;font-size:.72rem;color:var(--fog)}
.leg-dot{width:10px;height:10px;border-radius:2px;flex-shrink:0}

/* ── Data table ── */
.tbl-wrap{overflow-x:auto;-webkit-overflow-scrolling:touch;margin-top:1rem}
.dtbl{width:100%;border-collapse:collapse;font-size:.76rem}
.dtbl th{
padding:.5rem .7rem;text-align:right;
font-family:'Syne',sans-serif;font-size:.65rem;font-weight:700;
text-transform:uppercase;letter-spacing:.06em;color:var(--sky);
border-bottom:1px solid var(--rule);background:var(--ink3);white-space:nowrap;
}
.dtbl th:first-child{text-align:left}
.dtbl td{
padding:.42rem .7rem;text-align:right;
border-bottom:1px solid #0f2040;
font-variant-numeric:tabular-nums;white-space:nowrap;
font-size:.75rem;
}
.dtbl td:first-child{text-align:left}
.dtbl tr:hover td{background:#0e1f3a}
.dtbl .row-alt td{background:#0a1422}
.dtbl .retire-row td{background:#0c2518!important;font-weight:700}
.retire-star{color:var(--mint);margin-right:.3rem}
.cur-row td{background:#0d1f3e!important}
.cur-badge{color:var(--mint);font-weight:800;margin-left:.3rem}
.fw{font-weight:800}
.dim{color:var(--mist)}
.total-col{color:var(--paper)!important;font-weight:800!important}
.real-col{color:var(--amber)!important}
.good{color:var(--mint);font-weight:700}
.warn{color:var(--amber);font-weight:700}
.dpos{color:var(--mint);font-weight:700}
.dneg{color:var(--red);font-weight:700}

/* ── Bucket bar ── */
.bucket-bar{display:flex;height:18px;border-radius:9px;overflow:hidden;gap:2px;margin-bottom:.4rem}
.bseg{transition:width .4s ease}
.bpct-row{display:flex;justify-content:space-between;margin-bottom:1.1rem}
.bucket-cards{display:grid;grid-template-columns:repeat(3,1fr);gap:1rem}
@media(max-width:650px){.bucket-cards{grid-template-columns:1fr}}
.bcard{
background:var(--ink3);border:1px solid var(--rule);
border-left:3px solid var(--bc,var(--sky));border-radius:var(--r-sm);padding:1rem;
}
.bcard-icon{font-size:1.2rem;margin-bottom:.4rem}
.bcard-name{font-family:'Syne',sans-serif;font-size:.8rem;font-weight:800;color:var(--paper);margin-bottom:.15rem}
.bcard-sub{font-size:.68rem;color:var(--mist);margin-bottom:.6rem;font-style:italic}
.bcard-val{font-family:'Syne',sans-serif;font-size:1.15rem;font-weight:800;margin-bottom:.15rem}
.bcard-pct{font-size:.68rem;color:var(--mist);margin-bottom:.6rem}
.bcard-note{font-size:.7rem;color:var(--fog);line-height:1.5}

/* ── RMD warning ── */
.rmd-box{
background:#1a0812;border:1px solid var(--rose);
border-left:3px solid var(--rose);border-radius:var(--r-sm);
padding:1rem 1.2rem;margin-top:1.25rem;
}
.rmd-title{font-family:'Syne',sans-serif;font-size:.82rem;font-weight:800;color:var(--rose);margin-bottom:.45rem}
.rmd-body{font-size:.8rem;color:#fca5a5;line-height:1.7}

/* ── Conversion planner ── */
.conv-results{display:flex;flex-direction:column}
.conv-row{
display:flex;align-items:flex-start;justify-content:space-between;
padding:.55rem 0;border-bottom:1px solid var(--rule);gap:1rem;
}
.conv-row:last-child{border-bottom:none}
.conv-lbl{font-size:.8rem;color:var(--fog)}
.conv-note{font-size:.65rem;color:var(--mist);font-style:italic}
.conv-val{font-family:'Syne',sans-serif;font-size:.95rem;font-weight:800;font-variant-numeric:tabular-nums;white-space:nowrap}
.tip-box{
display:flex;gap:.75rem;background:#0b1540;
border:1px solid #1e3a8a;border-radius:var(--r-sm);
padding:1rem;margin-top:1.25rem;font-size:.78rem;color:#93c5fd;line-height:1.6;
}
.tip-icon{font-size:1rem;flex-shrink:0;margin-top:.1rem}

/* ── Scenario highlight ── */
.scen-highlight{background:var(--ink3);border-left:3px solid var(--mint)}

/* ── Scrollbar ── */
::-webkit-scrollbar{width:5px;height:5px}
::-webkit-scrollbar-track{background:var(--ink)}
::-webkit-scrollbar-thumb{background:var(--rule);border-radius:3px}

/* ── Footer ── */
.site-footer{
text-align:center;padding:1.25rem;font-size:.67rem;
color:var(--mist);border-top:1px solid var(--rule);
background:var(--ink2);margin-top:2rem;font-style:italic;
}
</style>
</head>
<body>

<!-- ── Header ── -->
<header class="site-header">
<div class="header-inner">
<div class="brand">
<div class="brand-icon">💰</div>
<div class="brand-text">
<h1>Retirement Planner</h1>
<p>Live projections · Change any input and everything updates instantly</p>
</div>
</div>
<div class="header-kpis">
<div class="hkpi">
<div class="hkpi-label">Nest Egg at 65</div>
<div class="hkpi-val" id="h-nestegg">—</div>
</div>
<div class="hkpi">
<div class="hkpi-label">Monthly Income</div>
<div class="hkpi-val green" id="h-monthly">—</div>
</div>
</div>
</div>
</header>

<!-- ── Nav ── -->
<nav class="site-nav">
<div class="nav-inner">
<button class="nav-btn active" data-page="dashboard">📊 Dashboard</button>
<button class="nav-btn" data-page="chart">📈 Growth Chart</button>
<button class="nav-btn" data-page="projections">📋 Projections</button>
<button class="nav-btn" data-page="scenarios">🔀 Scenarios</button>
<button class="nav-btn" data-page="tax">🏛 Tax Strategy</button>
</div>
</nav>

<!-- ══════════════════════════════════════════════════ -->
<!-- PAGE: DASHBOARD -->
<!-- ══════════════════════════════════════════════════ -->
<div class="page active" id="page-dashboard">

<!-- KPI Strip -->
<div class="kpi-strip">
<div class="kpi" style="--accent:var(--mint)">
<div class="kpi-label">Projected Nest Egg</div>
<div class="kpi-val" id="kpi-nestegg">—</div>
<div class="kpi-sub">At retirement</div>
</div>
<div class="kpi" style="--accent:var(--sky)">
<div class="kpi-label">Monthly Income</div>
<div class="kpi-val" id="kpi-monthly">—</div>
<div class="kpi-sub">4% withdrawal rule</div>
</div>
<div class="kpi" style="--accent:var(--lavender)">
<div class="kpi-label">Years to Retire</div>
<div class="kpi-val" id="kpi-years">—</div>
<div class="kpi-sub" id="kpi-years-sub">Until age 65</div>
</div>
<div class="kpi" style="--accent:var(--amber)">
<div class="kpi-label">Total Today</div>
<div class="kpi-val" id="kpi-total">—</div>
<div class="kpi-sub">All accounts</div>
</div>
</div>

<div class="grid-2">
<!-- Personal Info -->
<div class="card">
<div class="card-title">Personal Information</div>
<div class="input-stack">
<div class="irow">
<label class="irow-label">Current Age</label>
<div class="ifield-wrap"><input class="ifield bare" type="number" id="inp-age" value="29" step="0.25" min="18" max="80" /></div>
</div>
<div class="irow">
<label class="irow-label">Retirement Age</label>
<div class="ifield-wrap"><input class="ifield bare" type="number" id="inp-retage" value="65" step="1" min="50" max="80" /></div>
</div>
<div class="irow">
<label class="irow-label">Current Annual Salary</label>
<div class="ifield-wrap"><span class="ifix pre">$</span><input class="ifield pre" type="number" id="inp-salary" value="98000" step="1000" /></div>
</div>
<div class="irow">
<label class="irow-label">Annual Salary Growth</label>
<div class="ifield-wrap"><input class="ifield suf" type="number" id="inp-salgrowth" value="1.5" step="0.1" min="0" max="20" /><span class="ifix suf">%</span></div>
</div>
<div class="irow">
<label class="irow-label">Annual Market Return</label>
<div class="ifield-wrap"><input class="ifield suf" type="number" id="inp-mktreturn" value="6.0" step="0.1" min="0" max="20" /><span class="ifix suf">%</span></div>
<div class="irow-note">7% is the historical average</div>
</div>
<div class="irow">
<label class="irow-label">Annual Inflation Rate</label>
<div class="ifield-wrap"><input class="ifield suf" type="number" id="inp-inflation" value="3.0" step="0.1" min="0" max="15" /><span class="ifix suf">%</span></div>
</div>
</div>
</div>

<!-- Contributions -->
<div class="card">
<div class="card-title">Annual Contributions</div>
<div class="input-stack">
<div class="irow">
<label class="irow-label">Your 401(k) Contribution</label>
<div class="ifield-wrap"><input class="ifield suf" type="number" id="inp-emp401k" value="8.0" step="0.5" min="0" max="100" /><span class="ifix suf">%</span></div>
</div>
<div class="irow">
<label class="irow-label">Employer 401(k) Match</label>
<div class="ifield-wrap"><input class="ifield suf" type="number" id="inp-emr401k" value="6.0" step="0.5" min="0" max="100" /><span class="ifix suf">%</span></div>
</div>
<div class="irow">
<label class="irow-label">Roth IRA (annual)</label>
<div class="ifield-wrap"><span class="ifix pre">$</span><input class="ifield pre" type="number" id="inp-rothira-c" value="5000" step="500" /></div>
</div>
<div class="irow">
<label class="irow-label">HSA (annual)</label>
<div class="ifield-wrap"><span class="ifix pre">$</span><input class="ifield pre" type="number" id="inp-hsa-c" value="4400" step="100" /></div>
</div>
<div class="irow">
<label class="irow-label">Brokerage (annual)</label>
<div class="ifield-wrap"><span class="ifix pre">$</span><input class="ifield pre" type="number" id="inp-brok-c" value="0" step="500" /></div>
</div>
<div class="irow">
<label class="irow-label">401(k) IRS Limit</label>
<div class="ifield-wrap"><span class="ifix pre">$</span><input class="ifield pre" type="number" id="inp-lim401k" value="23500" step="500" /></div>
<div class="irow-note">Update each year</div>
</div>
<div class="irow">
<label class="irow-label">Roth IRA IRS Limit</label>
<div class="ifield-wrap"><span class="ifix pre">$</span><input class="ifield pre" type="number" id="inp-limroth" value="7000" step="500" /></div>
<div class="irow-note">2026: $7,000</div>
</div>
</div>
</div>
</div>

<!-- Account Balances -->
<div class="card" style="margin-top:1.25rem">
<div class="card-title">Current Account Balances — Update these whenever you check your accounts</div>
<div class="acct-grid">
<div class="acct" style="--ac:var(--sky)">
<div class="acct-dot"></div>
<div class="acct-name">Traditional 401(k)</div>
<input class="acct-ifield" style="border-color:var(--sky)" type="number" id="inp-trad401k" value="40406.92" step="100" />
<div class="acct-note">8% you + 6% employer match</div>
</div>
<div class="acct" style="--ac:var(--lavender)">
<div class="acct-dot"></div>
<div class="acct-name">Roth 401(k)</div>
<input class="acct-ifield" style="border-color:var(--lavender)" type="number" id="inp-roth401k" value="7980.64" step="100" />
<div class="acct-note">No new contributions</div>
</div>
<div class="acct" style="--ac:var(--mint)">
<div class="acct-dot"></div>
<div class="acct-name">Roth IRA</div>
<input class="acct-ifield" style="border-color:var(--mint)" type="number" id="inp-rothira" value="26291.91" step="100" />
<div class="acct-note" id="acct-rira-note">$5,000/yr</div>
</div>
<div class="acct" style="--ac:var(--amber)">
<div class="acct-dot"></div>
<div class="acct-name">Brokerage</div>
<input class="acct-ifield" style="border-color:var(--amber)" type="number" id="inp-brokerage" value="7038.57" step="100" />
<div class="acct-note" id="acct-brok-note">$0/yr</div>
</div>
<div class="acct" style="--ac:var(--rose)">
<div class="acct-dot"></div>
<div class="acct-name">HSA</div>
<input class="acct-ifield" style="border-color:var(--rose)" type="number" id="inp-hsa" value="3741.51" step="100" />
<div class="acct-note" id="acct-hsa-note">$4,400/yr</div>
</div>
<div class="acct-total">
<div class="acct-total-label">Total Today</div>
<div class="acct-total-val" id="acct-total-val">—</div>
</div>
</div>
</div>

<!-- Retirement Income -->
<div class="card" style="margin-top:1.25rem">
<div class="card-title">Retirement Income Analysis</div>
<div class="income-list" id="income-list"></div>
</div>

</div><!-- /dashboard -->

<!-- ══════════════════════════════════════════════════ -->
<!-- PAGE: CHART -->
<!-- ══════════════════════════════════════════════════ -->
<div class="page" id="page-chart">
<div class="card">
<div style="display:flex;justify-content:space-between;align-items:flex-start;flex-wrap:wrap;gap:.75rem;margin-bottom:1rem">
<div class="card-title" style="margin-bottom:0">Portfolio Growth</div>
<div class="tab-bar" style="margin-bottom:0">
<button class="tab active" data-chart="stacked">Stacked Growth</button>
<button class="tab" data-chart="milestones">Milestones</button>
<button class="tab" data-chart="benchmarks">Fidelity Benchmarks</button>
</div>
</div>
<div class="chart-wrap" style="height:360px">
<canvas id="main-chart"></canvas>
</div>
<div class="legend" id="chart-legend"></div>
</div>
<div class="card" id="chart-table-card" style="margin-top:1.25rem;display:none">
<div class="card-title" id="chart-table-title">Milestone Data</div>
<div class="tbl-wrap"><table class="dtbl" id="chart-table"></table></div>
</div>
</div>

<!-- ══════════════════════════════════════════════════ -->
<!-- PAGE: PROJECTIONS -->
<!-- ══════════════════════════════════════════════════ -->
<div class="page" id="page-projections">
<div class="card">
<div style="display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:.75rem;margin-bottom:.75rem">
<div class="card-title" style="margin-bottom:0">Year-by-Year Projections</div>
<button class="tab active" id="toggle-post">Show Post-Retirement</button>
</div>
<p class="chart-note">All values recalculate live when you change inputs on the Dashboard.</p>
<div class="tbl-wrap"><table class="dtbl" id="proj-table"></table></div>
</div>
</div>

<!-- ══════════════════════════════════════════════════ -->
<!-- PAGE: SCENARIOS -->
<!-- ══════════════════════════════════════════════════ -->
<div class="page" id="page-scenarios">
<div class="card">
<div class="card-title">Market Return Scenarios</div>
<p class="chart-note">How different average annual returns affect your nest egg. All other inputs stay the same.</p>
<div class="chart-wrap" style="height:260px"><canvas id="scen-chart"></canvas></div>
<div class="tbl-wrap" style="margin-top:1.25rem"><table class="dtbl" id="scen-table"></table></div>
</div>
<div class="card" style="margin-top:1.25rem">
<div class="card-title">Savings Rate Scenarios</div>
<p class="chart-note">What if you saved more — or less — in your Roth IRA, HSA, or brokerage?</p>
<div class="chart-wrap" style="height:240px"><canvas id="sav-chart"></canvas></div>
<div class="tbl-wrap" style="margin-top:1.25rem"><table class="dtbl" id="sav-table"></table></div>
</div>
</div>

<!-- ══════════════════════════════════════════════════ -->
<!-- PAGE: TAX STRATEGY -->
<!-- ══════════════════════════════════════════════════ -->
<div class="page" id="page-tax">
<div class="card">
<div class="card-title">Tax Bucket Breakdown at Retirement</div>
<p class="chart-note">Having money in all three buckets gives you flexibility to manage your tax bracket in retirement.</p>
<div class="bucket-bar" id="bucket-bar"></div>
<div class="bpct-row" id="bucket-pcts"></div>
<div class="bucket-cards" id="bucket-cards"></div>
<div class="rmd-box">
<div class="rmd-title">⚠️ RMD Risk — Plan Ahead</div>
<div class="rmd-body" id="rmd-body"></div>
</div>
</div>

<div class="card" style="margin-top:1.25rem">
<div class="card-title">Roth Conversion Planner — Ages 65–73 Window</div>
<p class="chart-note">Between retirement and age 73 (when RMDs begin), you can convert pre-tax 401(k) money to Roth at a potentially lower rate — shrinking your future tax bill.</p>
<div class="grid-2" style="gap:2rem">
<div>
<div style="font-family:'Syne',sans-serif;font-size:.7rem;font-weight:700;text-transform:uppercase;letter-spacing:.08em;color:var(--fog);margin-bottom:.75rem">Conversion Inputs</div>
<div class="input-stack">
<div class="irow">
<label class="irow-label">Convert Per Year</label>
<div class="ifield-wrap"><span class="ifix pre">$</span><input class="ifield pre" type="number" id="inp-conv-amt" value="50000" step="5000" /></div>
</div>
<div class="irow">
<label class="irow-label">Tax Rate on Conversion</label>
<div class="ifield-wrap"><input class="ifield suf" type="number" id="inp-conv-rate" value="22" step="1" min="0" max="40" /><span class="ifix suf">%</span></div>
<div class="irow-note">Your expected bracket ages 65–73</div>
</div>
<div class="irow">
<label class="irow-label">Years of Conversions</label>
<div class="ifield-wrap"><input class="ifield bare" type="number" id="inp-conv-yrs" value="8" step="1" min="1" max="8" /></div>
<div class="irow-note">Max 8 (ages 65–73)</div>
</div>
<div class="irow">
<label class="irow-label">Growth Rate During Window</label>
<div class="ifield-wrap"><input class="ifield suf" type="number" id="inp-conv-grow" value="6.0" step="0.1" /><span class="ifix suf">%</span></div>
</div>
</div>
</div>
<div>
<div style="font-family:'Syne',sans-serif;font-size:.7rem;font-weight:700;text-transform:uppercase;letter-spacing:.08em;color:var(--fog);margin-bottom:.75rem">Results</div>
<div class="conv-results" id="conv-results"></div>
</div>
</div>
<div class="tip-box">
<div class="tip-icon">💡</div>
<div>At $98K salary you're currently in the 22% bracket. Converting at 12–22% during early retirement (when income is lower) beats paying 22%+ via forced RMDs at 73. Target filling the 12% bracket first, then 22% with any remaining room.</div>
</div>
</div>
</div>

<footer class="site-footer">
This tool is for personal planning purposes only and is not financial advice.
Projections assume consistent contributions and average market returns.
Consult a fiduciary financial advisor for personalized guidance.
</footer>

<script>
// ══════════════════════════════════════════════════════
// CONSTANTS
// ══════════════════════════════════════════════════════
const COLORS = {
trad401k: '#4f9cf9',
roth401k: '#a78bfa',
rothIRA: '#34d399',
brokerage: '#fb923c',
hsa: '#f472b6',
};
const ACCT_LABELS = {
trad401k:'Traditional 401(k)', roth401k:'Roth 401(k)',
rothIRA:'Roth IRA', brokerage:'Brokerage', hsa:'HSA'
};
const MILESTONE_AGES = [30,35,40,45,50,55,60,65];
const BENCH_MULT = {30:1,35:2,40:3,45:4,50:6,55:7,60:8,65:10};

// ══════════════════════════════════════════════════════
// FORMAT HELPERS
// ══════════════════════════════════════════════════════
const fmt = n => {
if(n==null||isNaN(n)) return '—';
if(Math.abs(n)>=1e6) return '$'+(n/1e6).toFixed(2)+'M';
if(Math.abs(n)>=1e3) return '$'+(n/1e3).toFixed(0)+'K';
return '$'+Math.round(n).toLocaleString();
};
const fmtF = n => n==null||isNaN(n)?'—':'$'+Math.round(n).toLocaleString();
const fmtP = n => n==null||isNaN(n)?'—':(n*100).toFixed(1)+'%';
const fmtD = n => {
if(n==null||isNaN(n)) return '—';
return (n>=0?'+':'')+fmtF(n);
};

// ══════════════════════════════════════════════════════
// CALCULATIONS
// ══════════════════════════════════════════════════════
function getInputs(){
const g = id => parseFloat(document.getElementById(id).value)||0;
return {
currentAge: g('inp-age'),
retAge: g('inp-retage'),
salary: g('inp-salary'),
salGrowth: g('inp-salgrowth')/100,
mktReturn: g('inp-mktreturn')/100,
inflation: g('inp-inflation')/100,
trad401k: g('inp-trad401k'),
roth401k: g('inp-roth401k'),
rothIRA: g('inp-rothira'),
brokerage: g('inp-brokerage'),
hsa: g('inp-hsa'),
emp401kPct: g('inp-emp401k')/100,
emr401kPct: g('inp-emr401k')/100,
rothIRAann: g('inp-rothira-c'),
hsaAnn: g('inp-hsa-c'),
brokAnn: g('inp-brok-c'),
lim401k: g('inp-lim401k'),
limRoth: g('inp-limroth'),
convAmt: g('inp-conv-amt'),
convRate: g('inp-conv-rate')/100,
convYrs: g('inp-conv-yrs'),
convGrow: g('inp-conv-grow')/100,
};
}

function buildProjections(inp){
let sal=inp.salary, t=inp.trad401k, r4=inp.roth401k,
ri=inp.rothIRA, br=inp.brokerage, hs=inp.hsa;
const rows=[], totalYrs=Math.ceil(inp.retAge - inp.currentAge)+10;
for(let i=1;i<=totalYrs;i++){
const age=Math.floor(inp.currentAge)+i;
if(age>inp.retAge+10) break;
sal*=(1+inp.salGrowth);
const isRetired=age>inp.retAge;
const empC=isRetired?0:Math.min(sal*inp.emp401kPct,inp.lim401k);
const emrC=isRetired?0:Math.min(sal*inp.emr401kPct,inp.lim401k-empC);
const riraC=isRetired?0:Math.min(inp.rothIRAann,inp.limRoth);
const brkC=isRetired?0:inp.brokAnn;
const hsaC=isRetired?0:inp.hsaAnn;
t=t*(1+inp.mktReturn)+empC+emrC;
r4=r4*(1+inp.mktReturn);
ri=ri*(1+inp.mktReturn)+riraC;
br=br*(1+inp.mktReturn)+brkC;
hs=hs*(1+inp.mktReturn)+hsaC;
const total=t+r4+ri+br+hs;
const yrs=age-inp.currentAge;
rows.push({age,salary:sal,empC,emrC,trad401k:t,roth401k:r4,rothIRA:ri,brokerage:br,hsa:hs,total,realValue:total/Math.pow(1+inp.inflation,yrs)});
}
return rows;
}

function retRow(proj,age){
return proj.find(r=>r.age===Math.round(age))||proj[proj.length-1];
}

function buildMarketScens(inp){
const rates=[
{label:'Bear Market',rate:.04},
{label:'Conservative',rate:.05},
{label:'Moderate',rate:.06},
{label:'Your Assumption',rate:inp.mktReturn},
{label:'Historical Avg',rate:.10},
{label:'Optimistic',rate:.12},
];
return rates.map(s=>{
const proj=buildProjections({...inp,mktReturn:s.rate});
const r=retRow(proj,inp.retAge);
const n=inp.retAge-inp.currentAge;
return {...s,total:r.total,monthly:(r.total*.04)/12,realValue:r.total/Math.pow(1+inp.inflation,n),isCurrent:s.label==='Your Assumption'};
});
}

function buildSavScens(inp){
const scens=[
{label:'Cut Back',rothIRAann:2500,hsaAnn:2200,brokAnn:0},
{label:'Current Plan',rothIRAann:inp.rothIRAann,hsaAnn:inp.hsaAnn,brokAnn:inp.brokAnn},
{label:'Max Roth IRA',rothIRAann:inp.limRoth,hsaAnn:inp.hsaAnn,brokAnn:0},
{label:'Max Roth + HSA Bump',rothIRAann:inp.limRoth,hsaAnn:8300,brokAnn:0},
{label:'Max Everything',rothIRAann:inp.limRoth,hsaAnn:8300,brokAnn:12000},
];
const baseProj=buildProjections(inp);
const baseRet=retRow(baseProj,inp.retAge);
return scens.map(s=>{
const proj=buildProjections({...inp,...s});
const r=retRow(proj,inp.retAge);
return {...s,total:r.total,monthly:(r.total*.04)/12,delta:r.total-baseRet.total,isCurrent:s.label==='Current Plan'};
});
}

// ══════════════════════════════════════════════════════
// CHARTS
// ══════════════════════════════════════════════════════
let mainChart=null, scenChart=null, savChart=null;
let currentChartTab='stacked';
let showPost=false;

function destroyChart(c){ if(c) c.destroy(); return null; }

function buildStackedChart(proj){
const data=proj.filter(r=>r.age<=proj.find(r2=>r2.age===Math.round(getInputs().retAge))?.age||r.age<=65);
const labels=data.map(r=>r.age);
const keys=['trad401k','roth401k','rothIRA','brokerage','hsa'];
const datasets=keys.map(k=>({
label:ACCT_LABELS[k], data:data.map(r=>r[k]),
backgroundColor:COLORS[k]+'88', borderColor:COLORS[k],
borderWidth:1.5, fill:true, tension:.35,
}));
return {type:'line',data:{labels,datasets},options:{
responsive:true,maintainAspectRatio:false,
interaction:{mode:'index',intersect:false},
plugins:{legend:{display:false},tooltip:{callbacks:{label:c=>`${c.dataset.label}: ${fmt(c.raw)}`}}},
scales:{
x:{stacked:true,grid:{color:'#1e3a5f'},ticks:{color:'#64748b',font:{size:11}}},
y:{stacked:true,grid:{color:'#1e3a5f'},ticks:{color:'#64748b',font:{size:11},callback:v=>fmt(v)}},
},
}};
}

function buildMilestoneChart(proj){
const data=proj.filter(r=>MILESTONE_AGES.includes(r.age));
const keys=['trad401k','roth401k','rothIRA','brokerage','hsa'];
const datasets=keys.map(k=>({
label:ACCT_LABELS[k], data:data.map(r=>r[k]),
backgroundColor:COLORS[k]+'bb', borderColor:COLORS[k], borderWidth:1.5,
}));
return {type:'bar',data:{labels:data.map(r=>r.age),datasets},options:{
responsive:true,maintainAspectRatio:false,
interaction:{mode:'index',intersect:false},
plugins:{legend:{display:false},tooltip:{callbacks:{label:c=>`${c.dataset.label}: ${fmt(c.raw)}`}}},
scales:{
x:{stacked:true,grid:{color:'#1e3a5f'},ticks:{color:'#64748b'}},
y:{stacked:true,grid:{color:'#1e3a5f'},ticks:{color:'#64748b',callback:v=>fmt(v)}},
},
}};
}

function buildBenchmarkChart(proj,inp){
const pts=MILESTONE_AGES.map(age=>{
const r=proj.find(x=>x.age===age);
const yrs=age-inp.currentAge;
const salAtAge=inp.salary*Math.pow(1+inp.salGrowth,yrs);
return {age,target:salAtAge*(BENCH_MULT[age]||1),projected:r?r.total:null};
}).filter(p=>p.projected!==null);
return {type:'line',data:{
labels:pts.map(p=>p.age),
datasets:[
{label:'Fidelity Target',data:pts.map(p=>p.target),borderColor:'#fb923c',borderDash:[6,3],borderWidth:2,pointRadius:4,fill:false,tension:.35},
{label:'Your Projection',data:pts.map(p=>p.projected),borderColor:'#34d399',borderWidth:2.5,pointRadius:5,fill:false,tension:.35},
]
},options:{
responsive:true,maintainAspectRatio:false,
interaction:{mode:'index',intersect:false},
plugins:{legend:{labels:{color:'#94a3b8',font:{size:11}}},tooltip:{callbacks:{label:c=>`${c.dataset.label}: ${fmt(c.raw)}`}}},
scales:{
x:{grid:{color:'#1e3a5f'},ticks:{color:'#64748b'}},
y:{grid:{color:'#1e3a5f'},ticks:{color:'#64748b',callback:v=>fmt(v)}},
},
}};
}

function renderLegend(keys){
const el=document.getElementById('chart-legend');
el.innerHTML=keys.map(k=>`<div class="leg-item"><div class="leg-dot" style="background:${COLORS[k]}"></div>${ACCT_LABELS[k]}</div>`).join('');
}

function renderMilestoneTable(proj){
const rows=proj.filter(r=>MILESTONE_AGES.includes(r.age));
const card=document.getElementById('chart-table-card');
document.getElementById('chart-table-title').textContent='Milestone Balances';
card.style.display='block';
const keys=['trad401k','roth401k','rothIRA','brokerage','hsa'];
document.getElementById('chart-table').innerHTML=`
<thead><tr><th>Age</th><th>Salary</th>${keys.map(k=>`<th style="color:${COLORS[k]}">${ACCT_LABELS[k]}</th>`).join('')}<th>Total</th></tr></thead>
<tbody>${rows.map((r,i)=>`
<tr class="${i%2?'row-alt':''}">
<td class="fw" style="color:var(--sky)">${r.age}</td>
<td>${fmt(r.salary)}</td>
${keys.map(k=>`<td style="color:${COLORS[k]}">${fmt(r[k])}</td>`).join('')}
<td class="total-col">${fmt(r.total)}</td>
</tr>`).join('')}
</tbody>`;
}

function renderBenchmarkTable(proj,inp){
const card=document.getElementById('chart-table-card');
document.getElementById('chart-table-title').textContent='Fidelity Benchmark Comparison';
card.style.display='block';
const rows=MILESTONE_AGES.map(age=>{
const r=proj.find(x=>x.age===age);
const yrs=age-inp.currentAge;
const salAtAge=inp.salary*Math.pow(1+inp.salGrowth,yrs);
const target=salAtAge*(BENCH_MULT[age]||1);
const projected=r?r.total:null;
return {age,mult:BENCH_MULT[age],target,projected,on:projected!=null&&projected>=target};
});
document.getElementById('chart-table').innerHTML=`
<thead><tr><th>Age</th><th>Target Multiple</th><th>Target $</th><th>Your Projection</th><th>Status</th></tr></thead>
<tbody>${rows.map((r,i)=>`
<tr class="${i%2?'row-alt':''}">
<td class="fw" style="color:var(--sky)">${r.age}</td>
<td class="dim">${r.mult}× salary</td>
<td>${fmtF(r.target)}</td>
<td class="fw">${r.projected!=null?fmtF(r.projected):'—'}</td>
<td>${r.projected!=null?(r.on?'<span class="good">✅ On Track</span>':'<span class="warn">⚠️ Behind</span>'):'—'}</td>
</tr>`).join('')}
</tbody>`;
}

// ══════════════════════════════════════════════════════
// RENDER FUNCTIONS
// ══════════════════════════════════════════════════════
function renderDashboard(proj,inp){
const ret=retRow(proj,inp.retAge);
if(!ret) return;
const totalNow=inp.trad401k+inp.roth401k+inp.rothIRA+inp.brokerage+inp.hsa;
const nest=ret.total, monthly=(nest*.04)/12;
const yrs=inp.retAge-inp.currentAge;

// Header
document.getElementById('h-nestegg').textContent=fmt(nest);
document.getElementById('h-monthly').textContent=fmtF(monthly);
// KPIs
document.getElementById('kpi-nestegg').textContent=fmt(nest);
document.getElementById('kpi-monthly').textContent=fmtF(monthly);
document.getElementById('kpi-years').textContent=yrs.toFixed(0);
document.getElementById('kpi-years-sub').textContent=`Until age ${inp.retAge}`;
document.getElementById('kpi-total').textContent=fmtF(totalNow);
document.getElementById('acct-total-val').textContent=fmtF(totalNow);

// Account notes
document.getElementById('acct-rira-note').textContent=fmtF(inp.rothIRAann)+'/yr';
document.getElementById('acct-brok-note').textContent=fmtF(inp.brokAnn)+'/yr';
document.getElementById('acct-hsa-note').textContent=fmtF(inp.hsaAnn)+'/yr';

// Income list
const estRMD=ret.trad401k*Math.pow(1+inp.mktReturn,8)/26.5;
const incomeData=[
{lbl:'Nest Egg at Retirement',val:fmtF(nest),color:'var(--paper)',hl:false},
{lbl:'Annual Income (4% Rule)',val:fmtF(nest*.04),color:'var(--mint)',hl:false},
{lbl:'Monthly Income (4% Rule)',val:fmtF(monthly),color:'var(--mint)',hl:true},
{lbl:'Annual Income (3.5% Rule)',val:fmtF(nest*.035),color:'var(--sky)',hl:false},
{lbl:'Monthly Income (3.5% Rule)',val:fmtF((nest*.035)/12),color:'var(--sky)',hl:false},
{lbl:'Real Value (Inflation-Adjusted)',val:fmtF(ret.realValue),color:'var(--amber)',hl:false},
{lbl:'Est. RMD at Age 73',val:fmtF(estRMD),color:'var(--rose)',note:'⚠️ Taxable income — plan ahead',hl:false},
];
document.getElementById('income-list').innerHTML=incomeData.map(d=>`
<div class="income-row${d.hl?' hl':''}">
<span class="income-lbl">${d.lbl}</span>
<span class="income-val" style="color:${d.color}">${d.val}</span>
${d.note?`<span class="income-note">${d.note}</span>`:''}
</div>`).join('');
}

function renderChart(proj,inp){
const ctx=document.getElementById('main-chart').getContext('2d');
Chart.defaults.color='#94a3b8';
mainChart=destroyChart(mainChart);
document.getElementById('chart-table-card').style.display='none';
document.getElementById('chart-legend').innerHTML='';

if(currentChartTab==='stacked'){
mainChart=new Chart(ctx,buildStackedChart(proj));
renderLegend(['trad401k','roth401k','rothIRA','brokerage','hsa']);
} else if(currentChartTab==='milestones'){
mainChart=new Chart(ctx,buildMilestoneChart(proj));
renderLegend(['trad401k','roth401k','rothIRA','brokerage','hsa']);
renderMilestoneTable(proj);
} else {
mainChart=new Chart(ctx,buildBenchmarkChart(proj,inp));
renderBenchmarkTable(proj,inp);
}
}

function renderProjections(proj,inp){
const data=showPost?proj:proj.filter(r=>r.age<=inp.retAge);
const keys=['trad401k','roth401k','rothIRA','brokerage','hsa'];
document.getElementById('proj-table').innerHTML=`
<thead><tr>
<th>Age</th><th>Salary</th><th>Emp $</th><th>Match $</th>
${keys.map(k=>`<th style="color:${COLORS[k]}">${ACCT_LABELS[k]}</th>`).join('')}
<th>Total</th><th>Real Value</th>
</tr></thead>
<tbody>${data.map((r,i)=>{
const isRet=r.age===Math.round(inp.retAge);
return `<tr class="${isRet?'retire-row':i%2?'row-alt':''}">
<td class="fw" style="color:var(--sky)">${isRet?'<span class="retire-star">★</span>':''}${r.age}</td>
<td>${fmt(r.salary)}</td>
<td class="dim">${fmt(r.empC)}</td>
<td style="color:var(--mint)">${fmt(r.emrC)}</td>
${keys.map(k=>`<td style="color:${COLORS[k]}">${fmt(r[k])}</td>`).join('')}
<td class="total-col">${fmt(r.total)}</td>
<td class="real-col">${fmt(r.realValue)}</td>
</tr>`;
}).join('')}</tbody>`;
}

function renderScenarios(inp){
Chart.defaults.color='#94a3b8';
const mkt=buildMarketScens(inp);
const sav=buildSavScens(inp);

// Market chart
scenChart=destroyChart(scenChart);
const ctx1=document.getElementById('scen-chart').getContext('2d');
scenChart=new Chart(ctx1,{type:'bar',data:{
labels:mkt.map(s=>s.label),
datasets:[{
label:'Nest Egg at Retirement',
data:mkt.map(s=>s.total),
backgroundColor:mkt.map(s=>s.isCurrent?'#34d399bb':'#4f9cf9bb'),
borderColor:mkt.map(s=>s.isCurrent?'#34d399':'#4f9cf9'),
borderWidth:1.5,borderRadius:5,
}]
},options:{
responsive:true,maintainAspectRatio:false,
plugins:{legend:{display:false},tooltip:{callbacks:{label:c=>`${fmt(c.raw)}`}}},
scales:{x:{grid:{color:'#1e3a5f'},ticks:{color:'#64748b'}},y:{grid:{color:'#1e3a5f'},ticks:{color:'#64748b',callback:v=>fmt(v)}}},
}});

document.getElementById('scen-table').innerHTML=`
<thead><tr><th>Scenario</th><th>Return</th><th>Nest Egg at 65</th><th>Monthly Income</th><th>Real Value</th></tr></thead>
<tbody>${mkt.map((s,i)=>`
<tr class="${s.isCurrent?'cur-row':i%2?'row-alt':''}">
<td class="fw">${s.label}${s.isCurrent?'<span class="cur-badge">←</span>':''}</td>
<td class="dim">${fmtP(s.rate)}</td>
<td class="fw" style="color:${s.isCurrent?'var(--mint)':'inherit'}">${fmtF(s.total)}</td>
<td>${fmtF(s.monthly)}</td>
<td class="dim">${fmtF(s.realValue)}</td>
</tr>`).join('')}
</tbody>`;

// Savings chart
savChart=destroyChart(savChart);
const ctx2=document.getElementById('sav-chart').getContext('2d');
savChart=new Chart(ctx2,{type:'bar',data:{
labels:sav.map(s=>s.label),
datasets:[{
label:'Nest Egg at Retirement',
data:sav.map(s=>s.total),
backgroundColor:sav.map(s=>s.isCurrent?'#34d399bb':'#a78bfabb'),
borderColor:sav.map(s=>s.isCurrent?'#34d399':'#a78bfa'),
borderWidth:1.5,borderRadius:5,
}]
},options:{
responsive:true,maintainAspectRatio:false,
plugins:{legend:{display:false},tooltip:{callbacks:{label:c=>`${fmt(c.raw)}`}}},
scales:{x:{grid:{color:'#1e3a5f'},ticks:{color:'#64748b',font:{size:10}}},y:{grid:{color:'#1e3a5f'},ticks:{color:'#64748b',callback:v=>fmt(v)}}},
}});

document.getElementById('sav-table').innerHTML=`
<thead><tr><th>Scenario</th><th>Roth IRA/yr</th><th>HSA/yr</th><th>Brokerage/yr</th><th>Nest Egg</th><th>Monthly</th><th>vs. Current</th></tr></thead>
<tbody>${sav.map((s,i)=>`
<tr class="${s.isCurrent?'cur-row':i%2?'row-alt':''}">
<td class="fw">${s.label}${s.isCurrent?'<span class="cur-badge">←</span>':''}</td>
<td>${fmtF(s.rothIRAann)}</td>
<td>${fmtF(s.hsaAnn)}</td>
<td>${fmtF(s.brokAnn)}</td>
<td class="fw" style="color:${s.isCurrent?'var(--mint)':'inherit'}">${fmtF(s.total)}</td>
<td>${fmtF(s.monthly)}</td>
<td class="${s.isCurrent?'':s.delta>=0?'dpos':'dneg'}">${s.isCurrent?'—':fmtD(s.delta)}</td>
</tr>`).join('')}
</tbody>`;
}

function renderTax(proj,inp){
const ret=retRow(proj,inp.retAge);
if(!ret) return;
const preTax=ret.trad401k;
const taxFree=ret.rothIRA+ret.roth401k;
const flex=ret.hsa+ret.brokerage;
const total=ret.total;

const buckets=[
{label:'Pre-Tax',sub:'Traditional 401(k)',val:preTax,color:'#4f9cf9',icon:'🏦',note:'Taxed as ordinary income on withdrawal. RMDs required at age 73.'},
{label:'Tax-Free (Roth)',sub:'Roth IRA + Roth 401(k)',val:taxFree,color:'#34d399',icon:'✨',note:'Zero tax on qualified withdrawals. No RMDs on Roth IRA. Best for heirs.'},
{label:'Flexible',sub:'HSA + Brokerage',val:flex,color:'#fb923c',icon:'🔀',note:'HSA is triple tax-free for medical. Brokerage taxed at capital gains rates.'},
];

// Bar
document.getElementById('bucket-bar').innerHTML=buckets.map(b=>`<div class="bseg" style="width:${(b.val/total*100).toFixed(1)}%;background:${b.color}"></div>`).join('');
document.getElementById('bucket-pcts').innerHTML=buckets.map(b=>`<div style="color:${b.color};font-size:.72rem;font-weight:700;font-family:'Syne',sans-serif">${fmtP(b.val/total)}</div>`).join('');
document.getElementById('bucket-cards').innerHTML=buckets.map(b=>`
<div class="bcard" style="--bc:${b.color}">
<div class="bcard-icon">${b.icon}</div>
<div class="bcard-name">${b.label}</div>
<div class="bcard-sub">${b.sub}</div>
<div class="bcard-val" style="color:${b.color}">${fmtF(b.val)}</div>
<div class="bcard-pct">${fmtP(b.val/total)} of total</div>
<div class="bcard-note">${b.note}</div>
</div>`).join('');

// RMD
const estRMD=preTax*Math.pow(1+inp.mktReturn,8)/26.5;
document.getElementById('rmd-body').innerHTML=`Your Traditional 401(k) is projected to reach <strong>${fmtF(preTax)}</strong> at retirement. The IRS requires Required Minimum Distributions (RMDs) starting at age 73 — whether you need the money or not. At this balance, your first RMD alone could be approximately <strong>${fmtF(estRMD)}</strong> of taxable income, which could push you into a higher bracket. The Roth Conversion Planner below can help reduce this.`;

// Conversion
renderConversion(preTax,inp);
}

function renderConversion(preTax,inp){
const amt=inp.convAmt, rate=inp.convRate, yrs=inp.convYrs, grow=inp.convGrow;
const taxCost=amt*rate*yrs;
const rothGrowth=amt*(grow===0?yrs:((Math.pow(1+grow,yrs)-1)/grow))*(1+grow);
const remainPre=preTax*Math.pow(1+grow,yrs)-amt*yrs;
const reducedRMD=Math.max(0,remainPre*Math.pow(1+grow,8)/26.5);
const origRMD=preTax*Math.pow(1+grow,8)/26.5;
const rmdSavings=Math.max(0,(origRMD-reducedRMD)*rate);

document.getElementById('conv-results').innerHTML=[
{lbl:'Total Tax Cost',val:fmtF(taxCost),color:'var(--rose)',note:'Paid during conversion years'},
{lbl:'Roth Balance Added',val:fmtF(rothGrowth),color:'var(--mint)',note:'Tax-free growth'},
{lbl:'Remaining Pre-Tax',val:fmtF(remainPre),color:'var(--sky)',note:'Trad 401(k) after window'},
{lbl:'Reduced RMD at 73',val:fmtF(reducedRMD),color:'var(--lavender)',note:'After conversions'},
{lbl:'Original RMD at 73',val:fmtF(origRMD),color:'var(--fog)',note:'Without any conversions'},
{lbl:'Annual RMD Tax Savings',val:fmtF(rmdSavings),color:'var(--mint)',note:'Est. annual savings'},
].map(r=>`
<div class="conv-row">
<div><div class="conv-lbl">${r.lbl}</div><div class="conv-note">${r.note}</div></div>
<div class="conv-val" style="color:${r.color}">${r.val}</div>
</div>`).join('');
}

// ══════════════════════════════════════════════════════
// MASTER UPDATE
// ══════════════════════════════════════════════════════
function update(){
const inp=getInputs();
const proj=buildProjections(inp);
renderDashboard(proj,inp);
if(document.getElementById('page-chart').classList.contains('active')) renderChart(proj,inp);
if(document.getElementById('page-projections').classList.contains('active')) renderProjections(proj,inp);
if(document.getElementById('page-scenarios').classList.contains('active')) renderScenarios(inp);
if(document.getElementById('page-tax').classList.contains('active')) renderTax(proj,inp);
}

// ══════════════════════════════════════════════════════
// NAVIGATION
// ══════════════════════════════════════════════════════
function showPage(id){
document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
document.querySelectorAll('.nav-btn').forEach(b=>b.classList.remove('active'));
document.getElementById('page-'+id).classList.add('active');
document.querySelector(`.nav-btn[data-page="${id}"]`).classList.add('active');
const inp=getInputs();
const proj=buildProjections(inp);
if(id==='chart') renderChart(proj,inp);
if(id==='projections') renderProjections(proj,inp);
if(id==='scenarios') renderScenarios(inp);
if(id==='tax') renderTax(proj,inp);
}

// ══════════════════════════════════════════════════════
// EVENT LISTENERS
// ══════════════════════════════════════════════════════
document.querySelectorAll('.nav-btn').forEach(b=>b.addEventListener('click',()=>showPage(b.dataset.page)));

// All inputs trigger update
document.querySelectorAll('input[type=number]').forEach(inp=>inp.addEventListener('input',update));

// Chart tabs
document.querySelectorAll('[data-chart]').forEach(btn=>btn.addEventListener('click',function(){
document.querySelectorAll('[data-chart]').forEach(b=>b.classList.remove('active'));
this.classList.add('active');
currentChartTab=this.dataset.chart;
const inp=getInputs();
const proj=buildProjections(inp);
renderChart(proj,inp);
}));

// Post-retirement toggle
document.getElementById('toggle-post').addEventListener('click',function(){
showPost=!showPost;
this.textContent=showPost?'Hide Post-Retirement':'Show Post-Retirement';
const inp=getInputs();
const proj=buildProjections(inp);
renderProjections(proj,inp);
});

// ── Initial render ──
update();
</script>
</body>
</html>
