<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="default">
<title>Supply Chain Simulator — Sandoz</title>
<link href="https://fonts.googleapis.com/css2?family=Barlow:wght@600;700;800&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
:root {
  --p: #004A85; --pd: #002F5C; --pl: #0072C6;
  --sec: #C8E0F4; --acc: #00AEEF; --teal: #007E9F;
  --bg: #FFFFFF; --surf: #F4F7FA; --brd: #D8E4EE;
  --t1: #1A2B3C; --t2: #5A7A94; --td: #AAC0D0;
  --ok: #1A9E6F; --warn: #F5A623; --err: #D0021B; --purp: #7C3AED;
  --fd: 'Barlow','Arial Black',sans-serif;
  --fb: 'Inter','Arial',sans-serif;
  --r1: 6px; --r2: 12px; --r3: 20px;
  --sh: 0 2px 12px rgba(0,47,92,.10);
  --nav-h: 64px;
  --safe-b: env(safe-area-inset-bottom, 0px);
}
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
html{font-size:16px;-webkit-text-size-adjust:100%}
body{font-family:var(--fb);background:var(--bg);color:var(--t1);min-height:100vh;overflow-x:hidden}
::-webkit-scrollbar{width:3px;height:3px}
::-webkit-scrollbar-thumb{background:var(--brd);border-radius:9999px}

/* ── Loading ── */
#ov{position:fixed;inset:0;background:rgba(0,47,92,.88);display:flex;flex-direction:column;
  align-items:center;justify-content:center;z-index:9999;gap:16px;transition:opacity .3s}
#ov.hidden{opacity:0;pointer-events:none}
.spin{width:44px;height:44px;border:3px solid rgba(255,255,255,.2);
  border-top-color:var(--acc);border-radius:50%;animation:sp .7s linear infinite}
@keyframes sp{to{transform:rotate(360deg)}}
.ov-txt{font-family:var(--fd);font-weight:700;font-size:12px;color:#fff;
  letter-spacing:2px;text-transform:uppercase}

/* ── Header ── */
#hdr{position:sticky;top:0;z-index:200;background:var(--p);
  display:flex;align-items:center;justify-content:space-between;
  padding:0 16px;height:52px;box-shadow:0 2px 10px rgba(0,47,92,.3)}
.hdr-l{display:flex;align-items:center;gap:10px}
.hdr-title{font-family:var(--fd);font-weight:700;font-size:15px;color:#fff;white-space:nowrap}
.sim-badge{background:rgba(255,255,255,.15);border:1px solid rgba(255,255,255,.25);
  color:#fff;font-size:10px;font-weight:600;padding:3px 9px;border-radius:9999px;white-space:nowrap}
.sim-badge .dot{color:var(--acc)}

/* ── App body ── */
#app-body{display:flex;max-width:1400px;margin:0 auto;min-height:calc(100vh - 52px)}

/* ── Sidebar desktop ── */
#sidebar{width:340px;flex-shrink:0;background:var(--surf);border-right:1px solid var(--brd);
  overflow-y:auto;padding:16px 14px 80px}

/* ── Main ── */
#main{flex:1;overflow-y:auto;padding:16px 20px 40px;background:var(--bg)}

/* ── Section title ── */
.stit{font-family:var(--fd);font-weight:700;font-size:12px;text-transform:uppercase;
  letter-spacing:1.2px;color:var(--p);margin:18px 0 10px;padding-bottom:5px;
  border-bottom:2px solid var(--p);display:flex;align-items:center;gap:6px}
.stit:first-child{margin-top:0}

/* ── Param card (desktop sidebar grouping) ── */
.param-card{background:var(--bg);border:1px solid var(--brd);border-radius:var(--r2);
  padding:14px;margin-bottom:12px;box-shadow:var(--sh)}
.param-card-title{font-size:11px;font-weight:700;text-transform:uppercase;
  letter-spacing:.8px;color:var(--t2);margin-bottom:10px;
  display:flex;align-items:center;gap:5px}

/* ── Input ── */
.ig{margin-bottom:12px}
.il{display:block;font-size:11px;font-weight:600;color:var(--t2);margin-bottom:5px;letter-spacing:.3px}
.if{width:100%;height:44px;background:var(--surf);border:1.5px solid var(--brd);
  border-radius:var(--r1);padding:0 12px;font-family:var(--fb);font-size:16px;
  color:var(--t1);transition:border-color .2s;-webkit-appearance:none;appearance:none}
.if:focus{outline:none;border-color:var(--p);box-shadow:0 0 0 3px rgba(0,74,133,.1)}

/* ── Slider ── */
.sr{margin-bottom:12px}
.sl{font-size:11px;font-weight:600;color:var(--t2);margin-bottom:5px;display:block;letter-spacing:.3px}
.sc{display:flex;align-items:center;gap:8px}
input[type=range]{-webkit-appearance:none;flex:1;height:5px;background:var(--brd);
  border-radius:9999px;outline:none;cursor:pointer;min-height:5px}
input[type=range]::-webkit-slider-thumb{-webkit-appearance:none;width:22px;height:22px;
  background:var(--p);border-radius:50%;box-shadow:0 1px 6px rgba(0,74,133,.35);
  transition:transform .15s;border:2px solid #fff}
input[type=range]:active::-webkit-slider-thumb{transform:scale(1.15)}
.sv{background:var(--p);color:#fff;font-size:11px;font-weight:700;padding:3px 9px;
  border-radius:9999px;min-width:46px;text-align:center;white-space:nowrap;flex-shrink:0}

/* ── Buttons ── */
.btn-p{display:flex;align-items:center;justify-content:center;gap:8px;width:100%;
  height:50px;background:var(--p);color:#fff;font-family:var(--fd);font-weight:700;
  font-size:15px;letter-spacing:.5px;border:none;border-radius:var(--r2);cursor:pointer;
  transition:background .2s,transform .1s,box-shadow .2s;
  box-shadow:0 4px 14px rgba(0,74,133,.3);-webkit-tap-highlight-color:transparent;margin-top:16px}
.btn-p:hover{background:var(--pd)}
.btn-p:active{transform:scale(.98)}
.btn-s{display:inline-flex;align-items:center;gap:5px;height:38px;padding:0 14px;
  background:var(--sec);color:var(--p);font-family:var(--fb);font-size:12px;font-weight:600;
  border:none;border-radius:var(--r1);cursor:pointer;transition:background .2s;
  -webkit-tap-highlight-color:transparent}
.btn-s:hover{background:#b0cfe9}
.btns-row{display:flex;gap:8px;flex-wrap:wrap;margin-top:8px}

/* ── Stagionalità grid ── */
.stag-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:6px;margin-bottom:10px}
.mg{display:flex;flex-direction:column;gap:3px;align-items:center}
.ml{font-size:10px;font-weight:700;color:var(--t2);text-align:center;letter-spacing:.3px}
.mi{width:100%;height:42px;background:var(--surf);border:1.5px solid var(--brd);
  border-radius:var(--r1);text-align:center;font-size:16px;font-weight:600;
  color:var(--t1);-webkit-appearance:none;appearance:none;transition:border-color .2s,background .2s}
.mi:focus{outline:none;border-color:var(--p);background:var(--bg)}
.mi.pos{border-color:rgba(26,158,111,.5);background:rgba(26,158,111,.06);color:var(--ok)}
.mi.neg{border-color:rgba(208,2,27,.4);background:rgba(208,2,27,.05);color:var(--err)}

/* ── Preview stagionalità ── */
#stag-preview-wrap{background:var(--surf);border:1px solid var(--brd);border-radius:var(--r2);
  padding:12px;margin-bottom:12px}
#stag-preview-title{font-size:11px;font-weight:700;color:var(--t2);text-transform:uppercase;
  letter-spacing:.8px;margin-bottom:8px}
#stag-preview-canvas-wrap{height:90px;position:relative}

/* ── Demand summary chips ── */
.demand-chips{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:14px}
.chip{background:var(--surf);border:1px solid var(--brd);border-radius:9999px;
  padding:5px 12px;font-size:12px;font-weight:600;color:var(--t2);display:flex;gap:5px;align-items:center}
.chip strong{color:var(--t1);font-size:13px}

/* ── KPI grid ── */
.kpi-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:10px;margin-bottom:16px}
.kpi{background:var(--bg);border:1px solid var(--brd);border-radius:var(--r2);
  padding:14px 14px 12px;box-shadow:var(--sh);border-top:3px solid var(--p);
  transition:transform .2s}
.kpi:hover{transform:translateY(-1px)}
.kpi-l{font-size:10px;font-weight:700;color:var(--t2);text-transform:uppercase;
  letter-spacing:.8px;margin-bottom:5px}
.kpi-v{font-family:var(--fd);font-weight:800;font-size:24px;color:var(--t1);
  line-height:1;animation:fsu .4s ease}
@keyframes fsu{from{opacity:0;transform:translateY(5px)}to{opacity:1;transform:translateY(0)}}
.kpi-s{font-size:10px;color:var(--t2);margin-top:3px}

/* ── Scenario bands ── */
.scen-bands{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:16px}
.scen{background:var(--surf);border:1px solid var(--brd);border-radius:var(--r2);
  padding:12px 8px;text-align:center}
.scen-t{font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:.6px;margin-bottom:6px}
.scen-v{font-family:var(--fd);font-weight:800;font-size:16px}
.scen-s{font-size:10px;color:var(--t2);margin-top:2px}
.p10 .scen-t,.p10 .scen-v{color:var(--err)}
.p50 .scen-t,.p50 .scen-v{color:var(--p)}
.p90 .scen-t,.p90 .scen-v{color:var(--ok)}

/* ── Chart tabs ── */
.chart-tabs{display:flex;gap:0;background:var(--surf);border:1px solid var(--brd);
  border-radius:var(--r1);padding:3px;margin-bottom:12px;overflow-x:auto;-webkit-overflow-scrolling:touch}
.ctab{flex:1;height:36px;min-width:80px;background:transparent;color:var(--t2);
  font-family:var(--fb);font-size:12px;font-weight:600;border:none;border-radius:4px;
  cursor:pointer;transition:all .2s;white-space:nowrap;padding:0 10px;
  -webkit-tap-highlight-color:transparent}
.ctab.active{background:var(--p);color:#fff;box-shadow:0 2px 6px rgba(0,74,133,.25)}

/* ── Chart container ── */
.chart-box{background:var(--surf);border:1px solid var(--brd);border-radius:var(--r2);
  padding:14px;margin-bottom:16px}
.ch-wrap{height:240px;position:relative}
.cpanel{display:none}
.cpanel.active{display:block}

/* ── Table ── */
.tbl-section{margin-bottom:16px}
.tbl-toggle{display:flex;align-items:center;justify-content:space-between;cursor:pointer;
  padding:12px 14px;background:var(--surf);border:1px solid var(--brd);border-radius:var(--r2);
  -webkit-tap-highlight-color:transparent;transition:background .2s}
.tbl-toggle:hover{background:var(--sec)}
.tbl-toggle-t{font-family:var(--fd);font-weight:700;font-size:12px;color:var(--p);
  text-transform:uppercase;letter-spacing:.8px}
.tbl-wrap{overflow-x:auto;border:1px solid var(--brd);border-top:none;
  border-radius:0 0 var(--r2) var(--r2);display:none}
.tbl-wrap.open{display:block}
table{width:100%;border-collapse:collapse;font-size:11px;min-width:640px}
thead th{background:var(--p);color:#fff;font-weight:600;padding:9px 10px;
  text-align:right;white-space:nowrap;font-size:10px;letter-spacing:.3px}
thead th:first-child{text-align:left}
tbody tr:nth-child(even){background:var(--surf)}
tbody td{padding:8px 10px;border-bottom:1px solid var(--brd);text-align:right;color:var(--t1)}
tbody td:first-child{text-align:left;font-weight:600}
.bst{display:inline-block;padding:2px 7px;border-radius:9999px;
  font-size:9px;font-weight:700;text-transform:uppercase;letter-spacing:.4px}
.b-ok{background:rgba(26,158,111,.12);color:var(--ok)}
.b-low{background:rgba(245,166,35,.12);color:var(--warn)}
.b-crit{background:rgba(208,2,27,.12);color:var(--err)}
.b-so{background:rgba(208,2,27,.2);color:var(--err);font-weight:800}

/* ── Resoconto ── */
.resc{background:var(--surf);border:1px solid var(--brd);border-radius:var(--r2);
  padding:16px;margin-bottom:16px}
.resc-t{font-family:var(--fd);font-weight:700;font-size:14px;color:var(--p);
  margin-bottom:14px;display:flex;align-items:center;gap:7px}
.resc-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:8px;margin-bottom:12px}
.ri{background:var(--bg);border:1px solid var(--brd);border-radius:var(--r1);padding:9px 11px}
.ri-l{font-size:10px;color:var(--t2);font-weight:500}
.ri-v{font-size:14px;font-weight:700;color:var(--t1);margin-top:2px}
.al{border-radius:var(--r1);padding:9px 12px;margin-bottom:7px;font-size:12px;
  font-weight:500;display:flex;align-items:flex-start;gap:7px;line-height:1.4}
.al-w{background:rgba(245,166,35,.1);border-left:3px solid var(--warn);color:#7a4d00}
.al-e{background:rgba(208,2,27,.08);border-left:3px solid var(--err);color:#800012}
.al-s{background:rgba(26,158,111,.1);border-left:3px solid var(--ok);color:#0a5535}
.gf{display:flex;align-items:center;gap:9px;padding:12px 14px;border-radius:var(--r2);
  font-family:var(--fd);font-weight:700;font-size:15px;margin-top:12px}
.gf-ok{background:rgba(26,158,111,.1);color:var(--ok);border:1.5px solid var(--ok)}
.gf-ot{background:rgba(245,166,35,.1);color:#7a4d00;border:1.5px solid var(--warn)}
.gf-cr{background:rgba(208,2,27,.09);color:var(--err);border:1.5px solid var(--err)}

/* ── Bottom Nav ── */
#bnav{display:none;position:fixed;bottom:0;left:0;right:0;
  background:var(--bg);border-top:1px solid var(--brd);z-index:300;
  padding-bottom:var(--safe-b)}
.nav-items{display:flex;height:var(--nav-h)}
.ni{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:2px;
  cursor:pointer;border:none;background:transparent;-webkit-tap-highlight-color:transparent;
  padding-bottom:2px;position:relative}
.ni.active::after{content:'';position:absolute;top:0;left:25%;right:25%;
  height:2px;background:var(--p);border-radius:0 0 2px 2px}
.ni-ic{font-size:22px;color:var(--td);transition:color .15s}
.ni-lb{font-size:10px;font-weight:600;color:var(--td);letter-spacing:.3px;transition:color .15s}
.ni.active .ni-ic,.ni.active .ni-lb{color:var(--p)}

/* ── Mobile sections ── */
.msec{display:block}
.pane{padding:14px 14px 16px}

/* ── Responsive ── */
@media(max-width:767px){
  #sidebar{display:none}
  #main{padding:0 0 calc(var(--nav-h) + var(--safe-b) + 4px) 0}
  #bnav{display:block}
  .kpi-grid{grid-template-columns:repeat(2,1fr);gap:8px}
  .scen-bands{grid-template-columns:repeat(3,1fr);gap:6px}
  .scen{padding:10px 6px}
  .scen-v{font-size:14px}
  .ch-wrap{height:220px}
  .resc-grid{grid-template-columns:repeat(2,1fr)}
  .stag-grid{grid-template-columns:repeat(3,1fr)}
  .msec{display:none}
  .msec.show{display:block}
}
@media(min-width:768px){
  #hdr{height:60px}
  .hdr-title{font-size:17px}
  .kpi-grid{grid-template-columns:repeat(4,1fr)}
  .ch-wrap{height:300px}
  #bnav{display:none!important}
  .msec{display:block!important}
  #mobile-params,.pane-demand-mob{display:none!important}
  #risultati-section,.pane-adv-mob{display:block!important}
}

/* ── Utility ── */
.c-ok{color:var(--ok)}.c-w{color:var(--warn)}.c-err{color:var(--err)}
.c-p{color:var(--p)}.c-pu{color:var(--purp)}
</style>
</head>
<body>

<!-- Loading -->
<div id="ov">
  <div class="spin"></div>
  <div class="ov-txt">Simulazione in corso…</div>
</div>

<!-- Header -->
<header id="hdr">
  <div class="hdr-l">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 130 32" width="80" height="25" style="flex-shrink:0">
      <rect width="130" height="32" rx="4" fill="rgba(255,255,255,.15)"/>
      <text x="9" y="22" font-family="'Arial Black',sans-serif" font-weight="900" font-size="17" fill="#FFF" letter-spacing="2">SANDOZ</text>
    </svg>
    <span class="hdr-title">Supply Chain Simulator</span>
  </div>
  <div class="sim-badge"><span class="dot">●</span> <span id="sim-count">0</span> sim</div>
</header>

<div id="app-body">

<!-- ══ SIDEBAR DESKTOP ══ -->
<aside id="sidebar">

  <!-- Gruppo: Domanda -->
  <div class="param-card">
    <div class="param-card-title">📦 Domanda</div>
    <div class="ig">
      <label class="il">Media mensile (pz)</label>
      <input type="number" id="p-demand" class="if" value="15000" min="100" step="500">
    </div>
    <div class="sr">
      <span class="sl">Variabilità (σ %)</span>
      <div class="sc"><input type="range" id="p-variability" min="0" max="50" value="15" step="1">
        <span class="sv" id="v-variability">15%</span></div>
    </div>
    <div class="stit" style="margin-top:10px">🌿 Stagionalità mensile</div>
    <div class="stag-grid" id="stag-grid-desktop"></div>
    <div class="btns-row">
      <button class="btn-s" onclick="applyExample()">📈 Esempio</button>
      <button class="btn-s" onclick="resetStag()">↺ Reset</button>
    </div>
    <!-- Preview mini -->
    <div id="stag-preview-wrap" style="margin-top:10px">
      <div id="stag-preview-title">Profilo domanda annuale</div>
      <div id="stag-preview-canvas-wrap"><canvas id="stag-preview-chart"></canvas></div>
    </div>
  </div>

  <!-- Gruppo: Stock & Tempi -->
  <div class="param-card">
    <div class="param-card-title">🏭 Stock & Tempi</div>
    <div class="ig">
      <label class="il">Stock attuale (pz)</label>
      <input type="number" id="p-stock" class="if" value="40000" min="0" step="1000">
    </div>
    <div class="sr">
      <span class="sl">Safety stock (giorni)</span>
      <div class="sc"><input type="range" id="p-safety" min="5" max="60" value="20" step="1">
        <span class="sv" id="v-safety">20 gg</span></div>
    </div>
    <div class="sr">
      <span class="sl">Lead time (mesi)</span>
      <div class="sc"><input type="range" id="p-leadtime" min="1" max="18" value="8" step="1">
        <span class="sv" id="v-leadtime">8 m</span></div>
    </div>
  </div>

  <!-- Gruppo: Prodotto & Ordini -->
  <div class="param-card">
    <div class="param-card-title">💊 Prodotto & Ordini</div>
    <div class="ig">
      <label class="il">Shelf life (mesi)</label>
      <input type="number" id="p-shelflife" class="if" value="24" min="1" step="1">
    </div>
    <div class="ig">
      <label class="il">MOQ — Minimum Order Qty (pz)</label>
      <input type="number" id="p-moq" class="if" value="20000" min="100" step="1000">
    </div>
  </div>

  <!-- Gruppo: Simulazione -->
  <div class="param-card">
    <div class="param-card-title">⚙ Simulazione</div>
    <div class="sr">
      <span class="sl">Orizzonte (mesi)</span>
      <div class="sc"><input type="range" id="p-horizon" min="12" max="36" value="24" step="1">
        <span class="sv" id="v-horizon">24 m</span></div>
    </div>
    <div class="sr">
      <span class="sl">N° simulazioni Monte Carlo</span>
      <div class="sc"><input type="range" id="p-nsim" min="200" max="1000" value="500" step="100">
        <span class="sv" id="v-nsim">500</span></div>
    </div>
  </div>

  <button class="btn-p" onclick="run()">▶ Esegui Simulazione</button>
</aside>

<!-- ══ MAIN CONTENT ══ -->
<main id="main">

  <!-- ── MOBILE: Parametri (tab 0) ── -->
  <div id="mobile-params" class="msec pane" style="display:none">
    <div class="param-card">
      <div class="param-card-title">🏭 Stock attuale</div>
      <div class="ig">
        <label class="il">Stock attuale (pz)</label>
        <input type="number" id="m-stock" class="if" value="40000" min="0" step="1000" oninput="sp('stock',this.value)">
      </div>
      <div class="sr">
        <span class="sl">Safety stock (giorni)</span>
        <div class="sc"><input type="range" id="m-safety" min="5" max="60" value="20" step="1" oninput="ss('safety',this.value,' gg')">
          <span class="sv" id="mv-safety">20 gg</span></div>
      </div>
      <div class="sr">
        <span class="sl">Lead time (mesi)</span>
        <div class="sc"><input type="range" id="m-leadtime" min="1" max="18" value="8" step="1" oninput="ss('leadtime',this.value,' m')">
          <span class="sv" id="mv-leadtime">8 m</span></div>
      </div>
    </div>
    <div class="param-card">
      <div class="param-card-title">💊 Prodotto & Ordini</div>
      <div class="ig">
        <label class="il">Shelf life (mesi)</label>
        <input type="number" id="m-shelflife" class="if" value="24" min="1" step="1" oninput="sp('shelflife',this.value)">
      </div>
      <div class="ig">
        <label class="il">MOQ — Minimum Order Qty (pz)</label>
        <input type="number" id="m-moq" class="if" value="20000" min="100" step="1000" oninput="sp('moq',this.value)">
      </div>
    </div>
    <div class="param-card">
      <div class="param-card-title">⚙ Simulazione</div>
      <div class="sr">
        <span class="sl">Orizzonte (mesi)</span>
        <div class="sc"><input type="range" id="m-horizon" min="12" max="36" value="24" step="1" oninput="ss('horizon',this.value,' m')">
          <span class="sv" id="mv-horizon">24 m</span></div>
      </div>
      <div class="sr">
        <span class="sl">N° simulazioni</span>
        <div class="sc"><input type="range" id="m-nsim" min="200" max="1000" value="500" step="100" oninput="ss('nsim',this.value,'')">
          <span class="sv" id="mv-nsim">500</span></div>
      </div>
    </div>
    <button class="btn-p" onclick="run(true)">▶ Esegui Simulazione</button>
  </div>

  <!-- ── MOBILE: Domanda (tab 1) ── -->
  <div id="pane-demand-mob" class="msec pane show" style="display:none">
    <!-- Summary chips -->
    <div class="demand-chips" id="demand-chips">
      <div class="chip">Media <strong id="chip-media">15.000</strong> pz/m</div>
      <div class="chip">Variabilità <strong id="chip-var">15%</strong></div>
      <div class="chip">Picco annuale <strong id="chip-peak" class="c-ok">+0%</strong></div>
      <div class="chip">Minimo annuale <strong id="chip-min" class="c-err">+0%</strong></div>
    </div>

    <div class="param-card">
      <div class="param-card-title">📦 Domanda base</div>
      <div class="ig">
        <label class="il">Media mensile (pz)</label>
        <input type="number" id="md-demand" class="if" value="15000" min="100" step="500"
          oninput="sp('demand',this.value);updateDemandChips()">
      </div>
      <div class="sr">
        <span class="sl">Variabilità stocastica (σ %)</span>
        <div class="sc"><input type="range" id="md-variability" min="0" max="50" value="15" step="1"
          oninput="ss('variability',this.value,'%');updateDemandChips()">
          <span class="sv" id="mdv-variability">15%</span></div>
      </div>
      <p style="font-size:11px;color:var(--t2);line-height:1.5;margin-top:4px">
        La variabilità simula la fluttuazione casuale attorno alla media (distribuzione normale). Valori alti = domanda imprevedibile.
      </p>
    </div>

    <div class="param-card">
      <div class="param-card-title">🌿 Stagionalità mensile (%)</div>
      <p style="font-size:11px;color:var(--t2);line-height:1.5;margin-bottom:10px">
        Inserisci la variazione rispetto alla media per ogni mese. Positivo = domanda più alta, negativo = più bassa.
      </p>
      <div class="stag-grid" id="stag-grid-mobile" style="grid-template-columns:repeat(3,1fr)"></div>
      <div class="btns-row">
        <button class="btn-s" onclick="applyExample()">📈 Esempio farmaceutico</button>
        <button class="btn-s" onclick="resetStag()">↺ Reset</button>
      </div>
    </div>

    <!-- Preview grafico stagionalità -->
    <div class="param-card">
      <div class="param-card-title">📊 Profilo domanda annuale (preview)</div>
      <div style="height:130px;position:relative"><canvas id="stag-preview-mob"></canvas></div>
      <p style="font-size:10px;color:var(--t2);margin-top:8px;text-align:center">
        Domanda mensile stimata con stagionalità applicata
      </p>
    </div>

    <button class="btn-p" onclick="run(true)">▶ Esegui con questi parametri</button>
  </div>

  <!-- ── Risultati (tab 2, sempre visibile desktop) ── -->
  <div id="risultati-section" class="msec pane show">

    <div class="stit" style="margin-top:0">📊 KPI Principali (P50)</div>
    <div class="kpi-grid">
      <div class="kpi" id="kpi-fr" style="border-top-color:var(--ok)">
        <div class="kpi-l">Fill Rate</div>
        <div class="kpi-v" id="kv-fr">—</div>
        <div class="kpi-s">% domanda soddisfatta</div>
      </div>
      <div class="kpi" style="border-top-color:var(--warn)">
        <div class="kpi-l">Mesi Stockout</div>
        <div class="kpi-v" id="kv-so">—</div>
        <div class="kpi-s">mesi con stock = 0</div>
      </div>
      <div class="kpi" style="border-top-color:var(--purp)">
        <div class="kpi-l">Pz Scaduti</div>
        <div class="kpi-v c-pu" id="kv-exp">—</div>
        <div class="kpi-s">totale periodo</div>
      </div>
      <div class="kpi" style="border-top-color:var(--ok)">
        <div class="kpi-l">Copertura Media</div>
        <div class="kpi-v" id="kv-cov">—</div>
        <div class="kpi-s">mesi di stock medio</div>
      </div>
    </div>

    <div class="stit">📉 Scenario Bands — Fill Rate</div>
    <div class="scen-bands">
      <div class="scen p10">
        <div class="scen-t">P10 Pessim.</div>
        <div class="scen-v" id="sc-p10">—</div>
        <div class="scen-s">Fill Rate</div>
      </div>
      <div class="scen p50">
        <div class="scen-t">P50 Base</div>
        <div class="scen-v" id="sc-p50">—</div>
        <div class="scen-s">Fill Rate</div>
      </div>
      <div class="scen p90">
        <div class="scen-t">P90 Ottim.</div>
        <div class="scen-v" id="sc-p90">—</div>
        <div class="scen-s">Fill Rate</div>
      </div>
    </div>

    <div class="stit">📈 Grafici</div>
    <div class="chart-tabs">
      <button class="ctab active" onclick="switchChart(0,this)">Panoramica</button>
      <button class="ctab" onclick="switchChart(1,this)">Stock</button>
      <button class="ctab" onclick="switchChart(2,this)">Dom. vs Vendite</button>
      <button class="ctab" onclick="switchChart(3,this)">Ordini</button>
    </div>
    <div class="chart-box">
      <div class="cpanel active" id="cp-0"><div class="ch-wrap"><canvas id="ch-overview"></canvas></div></div>
      <div class="cpanel" id="cp-1"><div class="ch-wrap"><canvas id="ch-stock"></canvas></div></div>
      <div class="cpanel" id="cp-2"><div class="ch-wrap"><canvas id="ch-demand"></canvas></div></div>
      <div class="cpanel" id="cp-3"><div class="ch-wrap"><canvas id="ch-orders"></canvas></div></div>
    </div>

    <!-- Tabella collassabile -->
    <div class="tbl-section">
      <div class="tbl-toggle" onclick="toggleTbl(this)">
        <span class="tbl-toggle-t">📋 Dettaglio mensile</span>
        <span id="tbl-ico">▼</span>
      </div>
      <div class="tbl-wrap" id="tbl-wrap">
        <table>
          <thead><tr>
            <th>Mese</th><th>Domanda</th><th>Vendite</th><th>Ordini</th>
            <th>Arrivi</th><th>Stock fine</th><th>Cop.</th><th>Scaduto</th><th>Stato</th>
          </tr></thead>
          <tbody id="tbl-body"></tbody>
        </table>
      </div>
    </div>

    <!-- Resoconto -->
    <div class="resc" id="resc" style="display:none">
      <div class="resc-t">🧭 Resoconto Strategico</div>
      <div class="resc-grid" id="resc-grid"></div>
      <div id="alert-box"></div>
      <div id="giudizio"></div>
    </div>
  </div>

</main>
</div>

<!-- Bottom Nav Mobile -->
<nav id="bnav">
  <div class="nav-items">
    <button class="ni" id="ni-0" onclick="mTab(0,this)">
      <span class="ni-ic">⚙</span><span class="ni-lb">Parametri</span>
    </button>
    <button class="ni" id="ni-1" onclick="mTab(1,this)">
      <span class="ni-ic">📦</span><span class="ni-lb">Domanda</span>
    </button>
    <button class="ni active" id="ni-2" onclick="mTab(2,this)">
      <span class="ni-ic">📊</span><span class="ni-lb">Risultati</span>
    </button>
  </div>
</nav>

<script>
/* ══════════════════════════════════════════════
   SUPPLY CHAIN MONTE CARLO — Sandoz Italia v2
   ══════════════════════════════════════════════ */

let simCount = 0;
let charts = {};
let stagPreviewChart = null, stagPreviewMobChart = null;

const MESI = ['Gen','Feb','Mar','Apr','Mag','Giu','Lug','Ago','Set','Ott','Nov','Dic'];

/* ── Init griglia stagionalità ── */
function initGrid(id, cols) {
  const c = document.getElementById(id);
  if (!c) return;
  c.innerHTML = '';
  if (cols) c.style.gridTemplateColumns = `repeat(${cols},1fr)`;
  MESI.forEach((m,i) => {
    const w = document.createElement('div');
    w.className = 'mg';
    w.innerHTML = `<label class="ml">${m}</label>
      <input type="number" class="mi stag-input" data-m="${i}" value="0"
             min="-50" max="100" step="1" id="${id}-s${i}"
             oninput="onStagChange(this)">`;
    c.appendChild(w);
  });
}

function onStagChange(el) {
  const v = parseFloat(el.value) || 0;
  // Colora input
  el.classList.toggle('pos', v > 0);
  el.classList.toggle('neg', v < 0);
  // Sync tra desktop e mobile
  document.querySelectorAll(`.stag-input[data-m="${el.dataset.m}"]`).forEach(o => {
    if (o !== el) { o.value = el.value; o.classList.toggle('pos',v>0); o.classList.toggle('neg',v<0); }
  });
  updateStagPreview();
  updateDemandChips();
}

/* ── Leggi stagionalità ── */
function getStag() {
  const map = {};
  document.querySelectorAll('.stag-input').forEach(inp => {
    const m = inp.dataset.m;
    if (!(m in map)) map[m] = parseFloat(inp.value) || 0;
  });
  return Array.from({length:12}, (_,i) => (map[i]||0)/100);
}

/* ── Preview stagionalità ── */
function updateStagPreview() {
  const demand = parseFloat(document.getElementById('p-demand')?.value || 15000);
  const stag = getStag();
  const vals = stag.map(s => Math.round(demand * (1 + s)));
  const avg = demand;

  const cfg = {
    type: 'bar',
    data: {
      labels: MESI,
      datasets: [{
        data: vals,
        backgroundColor: vals.map(v => v >= avg ? 'rgba(0,74,133,0.75)' : 'rgba(0,174,239,0.55)'),
        borderRadius: 4
      }]
    },
    options: {
      responsive: true, maintainAspectRatio: false, animation: false,
      plugins: {
        legend: { display: false },
        tooltip: { callbacks: { label: ctx => ' ' + numIt(ctx.raw) + ' pz' },
          backgroundColor:'#1A2B3C', bodyColor:'#fff', padding:7, cornerRadius:5 }
      },
      scales: {
        x: { ticks: { color:'#5A7A94', font:{size:9} }, grid: { display:false } },
        y: { ticks: { color:'#5A7A94', font:{size:9}, callback: v => numIt(Math.round(v/1000))+'k' },
             grid: { color:'#D8E4EE' } }
      }
    }
  };

  // Desktop
  const elD = document.getElementById('stag-preview-chart');
  if (elD) {
    if (stagPreviewChart) stagPreviewChart.destroy();
    stagPreviewChart = new Chart(elD, JSON.parse(JSON.stringify(cfg)));
  }
  // Mobile
  const elM = document.getElementById('stag-preview-mob');
  if (elM) {
    if (stagPreviewMobChart) stagPreviewMobChart.destroy();
    stagPreviewMobChart = new Chart(elM, JSON.parse(JSON.stringify(cfg)));
  }
}

/* ── Demand chips ── */
function updateDemandChips() {
  const demand = parseFloat(document.getElementById('p-demand')?.value ||
                            document.getElementById('md-demand')?.value || 15000);
  const varEl = document.getElementById('p-variability') || document.getElementById('md-variability');
  const varPct = varEl ? varEl.value : 15;
  const stag = getStag();
  const peak = Math.max(...stag)*100;
  const min = Math.min(...stag)*100;

  const cm = document.getElementById('chip-media');
  const cv = document.getElementById('chip-var');
  const cp = document.getElementById('chip-peak');
  const cmi = document.getElementById('chip-min');
  if (cm) cm.textContent = numIt(demand);
  if (cv) cv.textContent = varPct + '%';
  if (cp) { cp.textContent = (peak >= 0 ? '+' : '') + peak.toFixed(0) + '%'; cp.className = peak >= 0 ? 'c-ok' : 'c-err'; }
  if (cmi) { cmi.textContent = (min >= 0 ? '+' : '') + min.toFixed(0) + '%'; cmi.className = min < 0 ? 'c-err' : 'c-ok'; }
}

/* ── Parametri ── */
function getParams() {
  const g = id => {
    const el = document.getElementById(id);
    return el ? el.value : null;
  };
  // Leggi da desktop, fallback mobile
  const demand = parseFloat(g('p-demand') || g('md-demand') || 15000);
  const variability = parseFloat(g('p-variability') || g('md-variability') || 15) / 100;
  const stock = parseFloat(g('p-stock') || g('m-stock') || 40000);
  const safetyDays = parseFloat(g('p-safety') || g('m-safety') || 20);
  const leadtime = parseInt(g('p-leadtime') || g('m-leadtime') || 8);
  const shelflife = parseInt(g('p-shelflife') || g('m-shelflife') || 24);
  const moq = parseInt(g('p-moq') || g('m-moq') || 20000);
  const horizon = parseInt(g('p-horizon') || g('m-horizon') || 24);
  const nsim = parseInt(g('p-nsim') || g('m-nsim') || 500);
  const stagionalita = getStag();
  return { demand, variability, stock, safetyDays, leadtime, shelflife, moq, horizon, nsim, stagionalita };
}

/* ── Box-Muller ── */
function randn() {
  let u,v;
  do{u=Math.random()}while(u===0);
  do{v=Math.random()}while(v===0);
  return Math.sqrt(-2*Math.log(u))*Math.cos(2*Math.PI*v);
}

/* ── Simulazione singola ── */
function simSingle(p) {
  const { demand, variability, stock, safetyDays, leadtime, shelflife, moq, horizon, stagionalita } = p;
  const safetyMesi = safetyDays / 30;
  let lotti = [{ qty: stock, expiry: horizon + shelflife }];
  let inTransito = [];
  const res = [];
  let totD=0, totV=0, totExp=0, totOrd=0, totPzOrd=0, mSO=0;

  for (let m=0; m<horizon; m++) {
    const stagMod = stagionalita[m%12];
    const domMese = Math.max(0, demand*(1+stagMod)*(1+variability*randn()));

    // Arrivi
    const arr = inTransito.filter(o=>o.arrivo===m);
    arr.forEach(a=>lotti.push({qty:a.qty,expiry:a.expiry}));
    inTransito = inTransito.filter(o=>o.arrivo!==m);

    // Scaduti
    let expMese=0;
    lotti = lotti.filter(l=>{ if(l.expiry<=m){expMese+=l.qty;return false}return true });

    // FIFO: vendi dal più vicino a scadere
    lotti.sort((a,b)=>a.expiry-b.expiry);
    let rim=domMese, vend=0;
    for(let i=0;i<lotti.length&&rim>0;i++){
      const s=Math.min(lotti[i].qty,rim);
      lotti[i].qty-=s; vend+=s; rim-=s;
    }
    lotti=lotti.filter(l=>l.qty>0);

    const stockFine=lotti.reduce((s,l)=>s+l.qty,0);
    const cov=demand>0?stockFine/demand:0;

    // ROP
    const qIT=inTransito.reduce((s,o)=>s+o.qty,0);
    const rop=demand*(leadtime+safetyMesi);
    let ordMese=0;
    if((stockFine+qIT)<rop){
      const fab=demand*(leadtime+safetyMesi+1);
      const qNec=Math.max(0,fab-stockFine-qIT);
      const mult=Math.ceil(qNec/moq);
      const qOrd=Math.max(moq,mult*moq);
      const slRes=shelflife-leadtime;
      if(slRes>0){
        inTransito.push({qty:qOrd,arrivo:m+leadtime,expiry:m+leadtime+slRes});
        ordMese=qOrd; totOrd++; totPzOrd+=qOrd;
      }
    }

    if(stockFine===0||vend<domMese*0.99)mSO++;
    totD+=domMese; totV+=vend; totExp+=expMese;

    res.push({m,dom:domMese,vend,ord:ordMese,
              arr:arr.reduce((s,a)=>s+a.qty,0),
              stock:stockFine,cov,exp:expMese});
  }

  return {
    res, fillRate:totD>0?(totV/totD)*100:100,
    mSO, totExp, totOrd, totPzOrd,
    covMed:res.reduce((s,r)=>s+r.cov,0)/res.length
  };
}

/* ── Monte Carlo ── */
function monteCarlo(p) {
  const runs=[];
  for(let s=0;s<p.nsim;s++) runs.push(simSingle(p));

  function pct(arr,q){
    const s=[...arr].sort((a,b)=>a-b);
    return s[Math.max(0,Math.floor(q*s.length)-1)];
  }

  const monthly=[];
  for(let m=0;m<p.horizon;m++){
    const mk=k=>runs.map(r=>r.res[m][k]);
    monthly.push({
      m,
      sP10:pct(mk('stock'),.1), sP50:pct(mk('stock'),.5), sP90:pct(mk('stock'),.9),
      dP50:pct(mk('dom'),.5), vP50:pct(mk('vend'),.5),
      oP50:pct(mk('ord'),.5), aP50:pct(mk('arr'),.5),
      eP50:pct(mk('exp'),.5), covP50:pct(mk('cov'),.5)
    });
  }

  const fr=runs.map(r=>r.fillRate);
  return {
    monthly,
    frP10:pct(fr,.1), frP50:pct(fr,.5), frP90:pct(fr,.9),
    soP50:pct(runs.map(r=>r.mSO),.5),
    expP50:pct(runs.map(r=>r.totExp),.5),
    covP50:pct(runs.map(r=>r.covMed),.5),
    ordP50:pct(runs.map(r=>r.totOrd),.5),
    pzOrdP50:pct(runs.map(r=>r.totPzOrd),.5)
  };
}

/* ── Label mesi ── */
function labelM(h) {
  const now=new Date(), sm=now.getMonth(), sy=now.getFullYear();
  return Array.from({length:h},(_,i)=>{
    const idx=(sm+i)%12;
    const yr=sy+Math.floor((sm+i)/12);
    return MESI[idx]+' '+String(yr).slice(-2);
  });
}

/* ── Render KPI ── */
function renderKPI(r,p) {
  setV('kv-fr', r.frP50.toFixed(1)+'%');
  setV('kv-so', r.soP50.toFixed(1));
  setV('kv-exp', numIt(Math.round(r.expP50)));
  setV('kv-cov', r.covP50.toFixed(1)+' m');
  const kfr=document.getElementById('kpi-fr');
  kfr.style.borderTopColor = r.frP50>=97?'var(--ok)':r.frP50>=90?'var(--warn)':'var(--err)';
  document.getElementById('sc-p10').textContent = r.frP10.toFixed(1)+'%';
  document.getElementById('sc-p50').textContent = r.frP50.toFixed(1)+'%';
  document.getElementById('sc-p90').textContent = r.frP90.toFixed(1)+'%';
}

function setV(id,val){
  const el=document.getElementById(id);
  if(!el)return;
  el.textContent=val;
  el.style.animation='none'; void el.offsetWidth; el.style.animation='';
}

/* ── Render Charts ── */
function renderCharts(r,p) {
  const labels=labelM(p.horizon);
  const safetyLine=labels.map(()=>p.demand*(p.safetyDays/30));

  Object.values(charts).forEach(c=>{if(c)c.destroy()});
  charts={};

  const base={
    responsive:true, maintainAspectRatio:false,
    plugins:{
      legend:{labels:{color:'#5A7A94',font:{family:'Inter',size:10},boxWidth:10,padding:8}},
      tooltip:{backgroundColor:'#1A2B3C',titleColor:'#C8E0F4',bodyColor:'#fff',
               padding:8,cornerRadius:5,
               callbacks:{label:ctx=>' '+numIt(Math.round(ctx.raw))}}
    },
    scales:{
      x:{ticks:{color:'#5A7A94',maxRotation:45,font:{size:9}},grid:{color:'#D8E4EE'}},
      y:{ticks:{color:'#5A7A94',font:{size:9},callback:v=>numIt(Math.round(v/1000))+'k'},
         grid:{color:'#D8E4EE'}}
    }
  };

  /* ── Grafico 0: PANORAMICA ── */
  /* Domanda attesa (stag * media) + stock P50 + arrivi + scaduti */
  const stagMedDom = labels.map((_,i)=>{
    const stagMod = p.stagionalita[i%12];
    return Math.round(p.demand*(1+stagMod));
  });

  charts.overview = new Chart(document.getElementById('ch-overview'), {
    data: {
      labels,
      datasets: [
        {
          type:'bar', label:'Domanda prevista',
          data: r.monthly.map(m=>m.dP50),
          backgroundColor:'rgba(124,58,237,0.5)', borderRadius:3, yAxisID:'y'
        },
        {
          type:'bar', label:'Arrivi previsti',
          data: r.monthly.map(m=>m.aP50),
          backgroundColor:'rgba(0,126,159,0.6)', borderRadius:3, yAxisID:'y'
        },
        {
          type:'bar', label:'Pz scaduti',
          data: r.monthly.map(m=>m.eP50),
          backgroundColor:'rgba(208,2,27,0.45)', borderRadius:3, yAxisID:'y'
        },
        {
          type:'line', label:'Stock fine mese (P50)',
          data: r.monthly.map(m=>m.sP50),
          borderColor:'#004A85', backgroundColor:'rgba(0,74,133,0.08)',
          fill:true, tension:0.35, borderWidth:2.5, pointRadius:2.5,
          pointBackgroundColor:'#004A85', yAxisID:'y2'
        },
        {
          type:'line', label:'Safety stock',
          data: safetyLine,
          borderColor:'#F5A623', borderDash:[5,3],
          fill:false, tension:0, borderWidth:1.5, pointRadius:0, yAxisID:'y2'
        }
      ]
    },
    options:{
      ...base,
      interaction:{mode:'index',intersect:false},
      plugins:{
        ...base.plugins,
        legend:{...base.plugins.legend,position:'bottom'},
        tooltip:{
          ...base.plugins.tooltip,
          callbacks:{
            label:ctx=>{
              const lbl=ctx.dataset.label||'';
              return ' '+lbl+': '+numIt(Math.round(ctx.raw));
            }
          }
        }
      },
      scales:{
        x:base.scales.x,
        y:{...base.scales.y, position:'left',
           title:{display:true,text:'Quantità (pz)',color:'#5A7A94',font:{size:9}}},
        y2:{...base.scales.y, position:'right',
            grid:{drawOnChartArea:false},
            title:{display:true,text:'Stock (pz)',color:'#004A85',font:{size:9}}}
      }
    }
  });

  /* ── Grafico 1: STOCK ── */
  charts.stock = new Chart(document.getElementById('ch-stock'), {
    type:'line',
    data:{labels, datasets:[
      {label:'P90',data:r.monthly.map(m=>m.sP90),
       borderColor:'rgba(0,174,239,.45)',backgroundColor:'rgba(0,174,239,.1)',
       fill:'+1',tension:.3,borderWidth:1,pointRadius:0},
      {label:'P50 Stock',data:r.monthly.map(m=>m.sP50),
       borderColor:'#004A85',backgroundColor:'rgba(0,74,133,.08)',
       fill:'+1',tension:.3,borderWidth:2.5,pointRadius:2.5,pointBackgroundColor:'#004A85'},
      {label:'P10',data:r.monthly.map(m=>m.sP10),
       borderColor:'rgba(208,2,27,.4)',backgroundColor:'rgba(208,2,27,.05)',
       fill:false,tension:.3,borderWidth:1,pointRadius:0},
      {label:'Safety Stock',data:safetyLine,
       borderColor:'#F5A623',borderDash:[6,3],fill:false,tension:0,borderWidth:1.5,pointRadius:0}
    ]},
    options:{...base, interaction:{mode:'index',intersect:false}}
  });

  /* ── Grafico 2: DOM vs VENDITE ── */
  charts.demand = new Chart(document.getElementById('ch-demand'), {
    type:'bar',
    data:{labels, datasets:[
      {label:'Domanda P50',data:r.monthly.map(m=>m.dP50),
       backgroundColor:'rgba(124,58,237,0.7)',borderRadius:4},
      {label:'Vendite P50',data:r.monthly.map(m=>m.vP50),
       backgroundColor:'rgba(0,74,133,0.8)',borderRadius:4}
    ]},
    options:{...base, interaction:{mode:'index',intersect:false}}
  });

  /* ── Grafico 3: ORDINI ── */
  charts.orders = new Chart(document.getElementById('ch-orders'), {
    data:{labels, datasets:[
      {type:'bar',label:'Ordini inviati',data:r.monthly.map(m=>m.oP50),
       backgroundColor:'rgba(0,74,133,0.7)',borderRadius:4,yAxisID:'y'},
      {type:'bar',label:'Arrivi',data:r.monthly.map(m=>m.aP50),
       backgroundColor:'rgba(0,126,159,0.6)',borderRadius:4,yAxisID:'y'},
      {type:'line',label:'Stock P50',data:r.monthly.map(m=>m.sP50),
       borderColor:'#00AEEF',fill:false,tension:.3,borderWidth:2,pointRadius:2,yAxisID:'y2'}
    ]},
    options:{...base,
      scales:{x:base.scales.x,
              y:{...base.scales.y,position:'left'},
              y2:{...base.scales.y,position:'right',grid:{drawOnChartArea:false}}}}
  });
}

/* ── Render Tabella ── */
function renderTable(r,p) {
  const tbody=document.getElementById('tbl-body');
  tbody.innerHTML='';
  const labels=labelM(p.horizon);
  r.monthly.forEach((m,i)=>{
    const cov=m.covP50;
    let stato,bc;
    if(m.sP50===0||m.vP50<m.dP50*.98){stato='STOCKOUT';bc='b-so'}
    else if(cov<1.5){stato='CRITICO';bc='b-crit'}
    else if(cov<3){stato='BASSO';bc='b-low'}
    else{stato='OK';bc='b-ok'}
    const tr=document.createElement('tr');
    tr.innerHTML=`<td>${labels[i]}</td>
      <td>${numIt(Math.round(m.dP50))}</td><td>${numIt(Math.round(m.vP50))}</td>
      <td>${numIt(Math.round(m.oP50))}</td><td>${numIt(Math.round(m.aP50))}</td>
      <td>${numIt(Math.round(m.sP50))}</td><td>${cov.toFixed(1)} m</td>
      <td>${numIt(Math.round(m.eP50))}</td>
      <td><span class="bst ${bc}">${stato}</span></td>`;
    tbody.appendChild(tr);
  });
}

/* ── Render Resoconto ── */
function renderResoconto(r,p) {
  const resc=document.getElementById('resc');
  resc.style.display='block';
  document.getElementById('resc-grid').innerHTML=`
    <div class="ri"><div class="ri-l">Domanda media</div><div class="ri-v">${numIt(p.demand)} pz/m</div></div>
    <div class="ri"><div class="ri-l">Lead time</div><div class="ri-v">${p.leadtime} mesi</div></div>
    <div class="ri"><div class="ri-l">MOQ</div><div class="ri-v">${numIt(p.moq)} pz</div></div>
    <div class="ri"><div class="ri-l">Shelf life</div><div class="ri-v">${p.shelflife} mesi</div></div>
    <div class="ri"><div class="ri-l">N° ordini (P50)</div><div class="ri-v">${Math.round(r.ordP50)}</div></div>
    <div class="ri"><div class="ri-l">Pz ordinati totali</div><div class="ri-v">${numIt(Math.round(r.pzOrdP50))}</div></div>
  `;

  let alerts='';
  if(r.frP50<95) alerts+=alBox('w','⚠ Fill Rate < 95%: Aumenta ROP o safety stock.');
  if(r.expP50>p.demand) alerts+=alBox('e','⚠ Scaduti > 1 mese domanda: Riduci multipli MOQ.');
  if(r.soP50>0) alerts+=alBox('e','🚨 Stockout previsti: Aumenta stock iniziale o riduci lead time.');
  if(r.frP50>=97&&r.expP50<p.demand*.1&&r.soP50===0)
    alerts+=alBox('s','✅ Performance ottimale: supply chain ben bilanciata.');
  document.getElementById('alert-box').innerHTML=alerts;

  let gc,gt;
  if(r.frP50>=97&&r.soP50===0){gc='gf-ok';gt='✅ Efficiente'}
  else if(r.frP50>=90){gc='gf-ot';gt='⚠ Ottimizzabile'}
  else{gc='gf-cr';gt='🚨 Critico'}
  document.getElementById('giudizio').innerHTML=`<div class="gf ${gc}">${gt}</div>`;
}

function alBox(t,msg){
  const cl={w:'al-w',e:'al-e',s:'al-s'}[t];
  return `<div class="al ${cl}">${msg}</div>`;
}

/* ── Esegui simulazione ── */
function run(goToResults=false) {
  document.getElementById('ov').classList.remove('hidden');
  setTimeout(()=>{
    const p=getParams();
    const res=monteCarlo(p);
    simCount+=p.nsim;
    document.getElementById('sim-count').textContent=numIt(simCount);
    renderKPI(res,p);
    renderCharts(res,p);
    renderTable(res,p);
    renderResoconto(res,p);
    document.getElementById('ov').classList.add('hidden');
    if(goToResults&&window.innerWidth<768) mTab(2,document.getElementById('ni-2'));
  },30);
}

/* ── Esempio farmaceutico ── */
function applyExample(){
  const ex=[-10,-5,5,8,10,5,-15,-20,5,10,15,20];
  document.querySelectorAll('.stag-input').forEach(inp=>{
    inp.value=ex[inp.dataset.m];
    const v=ex[inp.dataset.m];
    inp.classList.toggle('pos',v>0);
    inp.classList.toggle('neg',v<0);
  });
  // Dedup: applica ai desktop input che condividono data-m
  updateStagPreview(); updateDemandChips();
}

function resetStag(){
  document.querySelectorAll('.stag-input').forEach(inp=>{
    inp.value=0; inp.classList.remove('pos','neg');
  });
  updateStagPreview(); updateDemandChips();
}

/* ── Chart switcher ── */
function switchChart(idx,btn){
  document.querySelectorAll('.ctab').forEach(t=>t.classList.remove('active'));
  document.querySelectorAll('.cpanel').forEach(p=>p.classList.remove('active'));
  btn.classList.add('active');
  document.getElementById('cp-'+idx).classList.add('active');
}

/* ── Tabella toggle ── */
function toggleTbl(el){
  const w=document.getElementById('tbl-wrap');
  const ico=document.getElementById('tbl-ico');
  const open=w.classList.contains('open');
  w.classList.toggle('open',!open);
  ico.textContent=open?'▼':'▲';
}

/* ── Mobile tabs ── */
function mTab(idx,btn){
  const secs=['mobile-params','pane-demand-mob','risultati-section'];
  const navIds=['ni-0','ni-1','ni-2'];
  secs.forEach(s=>{const e=document.getElementById(s);if(e)e.style.display='none'});
  navIds.forEach(n=>{const e=document.getElementById(n);if(e)e.classList.remove('active')});
  const sec=document.getElementById(secs[idx]);
  if(sec)sec.style.display='block';
  if(btn)btn.classList.add('active');
}

/* ── Sync mobile → desktop ── */
function sp(f,v){const e=document.getElementById('p-'+f);if(e)e.value=v}
function ss(f,v,suf){
  sp(f,v);
  const mv=document.getElementById('mv-'+f);if(mv)mv.textContent=v+suf;
  if(f==='variability')updateDemandChips();
}

/* ── Bind sliders desktop ── */
function bindSliders(){
  [['p-variability','v-variability','%'],
   ['p-safety','v-safety',' gg'],
   ['p-leadtime','v-leadtime',' m'],
   ['p-horizon','v-horizon',' m'],
   ['p-nsim','v-nsim','']].forEach(([id,vid,suf])=>{
    const el=document.getElementById(id);
    const vel=document.getElementById(vid);
    if(el&&vel) el.addEventListener('input',()=>{ vel.textContent=el.value+suf; });
  });
  // p-demand → aggiorna preview
  const pd=document.getElementById('p-demand');
  if(pd) pd.addEventListener('input',()=>{updateStagPreview();updateDemandChips();});
}

/* ── numIt ── */
function numIt(n){if(isNaN(n))return'—';return n.toLocaleString('it-IT')}

/* ── INIT ── */
document.addEventListener('DOMContentLoaded',()=>{
  initGrid('stag-grid-desktop',4);
  initGrid('stag-grid-mobile',3);
  bindSliders();
  // Mostra risultati su mobile per default
  document.getElementById('risultati-section').style.display='block';
  updateStagPreview();
  updateDemandChips();
  run();
});
</script>
</body>
</html>