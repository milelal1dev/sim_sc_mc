<!DOCTYPE html>

<html lang="it">
<head>
  <meta charset="UTF-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover, user-scalable=no"/>
  <title>SimChain MC</title>

  <!-- PWA Safari -->

  <meta name="apple-mobile-web-app-capable" content="yes"/>
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent"/>
  <meta name="apple-mobile-web-app-title" content="SimChain"/>
  <meta name="theme-color" content="#004A85"/>

  <!-- PWA Icons inline SVG → data URI (nessun file esterno) -->

  <link rel="apple-touch-icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 180 180'%3E%3Crect width='180' height='180' rx='40' fill='%23004A85'/%3E%3Ctext x='20' y='110' font-family='Arial Black' font-weight='900' font-size='52' fill='white'%3ESC%3C/text%3E%3Ctext x='18' y='145' font-family='Arial' font-size='22' fill='%23C8E0F4'%3EMonte Carlo%3C/text%3E%3C/svg%3E"/>

  <!-- Google Fonts -->

  <link rel="preconnect" href="https://fonts.googleapis.com"/>
  <link href="https://fonts.googleapis.com/css2?family=Barlow:wght@600;700;800&family=Inter:wght@400;500;600&display=swap" rel="stylesheet"/>

  <!-- React + Babel -->

  <script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>

  <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>

  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

  <!-- Chart.js -->

  <script src="https://cdn.jsdelivr.net/npm/chart.js@4/dist/chart.umd.min.js"></script>

  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    html, body { height: 100%; background: #0d1117; }
    body {
      display: flex; align-items: center; justify-content: center;
      font-family: 'Inter', Arial, sans-serif;
      -webkit-tap-highlight-color: transparent;
      overflow: hidden;
    }
    input[type=range] { -webkit-appearance: none; appearance: none; width: 100%; height: 4px; border-radius: 2px; background: #D8E4EE; outline: none; }
    input[type=range]::-webkit-slider-thumb { -webkit-appearance: none; width: 20px; height: 20px; border-radius: 50%; background: #004A85; cursor: pointer; box-shadow: 0 1px 4px rgba(0,74,133,0.3); }
    input[type=number] { -moz-appearance: textfield; }
    input[type=number]::-webkit-outer-spin-button, input[type=number]::-webkit-inner-spin-button { -webkit-appearance: none; }
    ::-webkit-scrollbar { width: 0; background: transparent; }
    @keyframes spin { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
    @keyframes fadeUp { from { opacity:0; transform:translateY(14px); } to { opacity:1; transform:translateY(0); } }
    .kpi-card { animation: fadeUp 0.4s ease both; }
    /* Adatta a schermo reale su iPhone */
    @media (max-width: 430px) {
      #app-shell {
        width: 100vw !important;
        height: 100vh !important;
        height: 100dvh !important;
        border-radius: 0 !important;
        box-shadow: none !important;
      }
      #notch { display: none !important; }
      #status-bar { padding-top: env(safe-area-inset-top, 44px) !important; height: auto !important; }
      #home-ind { bottom: env(safe-area-inset-bottom, 8px) !important; }
      #tab-bar { padding-bottom: env(safe-area-inset-bottom, 0px) !important; height: auto !important; min-height: 60px !important; }
      #run-btn { bottom: calc(70px + env(safe-area-inset-bottom, 0px)) !important; width: calc(100vw - 32px) !important; }
    }
  </style>

</head>
<body>
<div id="root"></div>

<script type="text/babel">
const { useState, useCallback, useRef, useEffect } = React;

// ─── THEME ────────────────────────────────────────────────────────────────────
const T = {
  blue:"#004A85", navy:"#002F5C", sky:"#0072C6", lightBg:"#C8E0F4",
  cyan:"#00AEEF", teal:"#007E9F", white:"#FFFFFF", off:"#F4F7FA",
  border:"#D8E4EE", text:"#1A2B3C", mid:"#5A7A94", pale:"#AAC0D0",
  ok:"#1A9E6F", warn:"#F5A623", err:"#D0021B",
};

const STAGIONALITA_ESEMPIO = [-5,-8,0,5,8,15,20,18,10,-2,-5,-10];
const MESI = ["Gen","Feb","Mar","Apr","Mag","Giu","Lug","Ago","Set","Ott","Nov","Dic"];

// ─── BOX-MULLER ───────────────────────────────────────────────────────────────
function randn() {
  let u=0,v=0;
  while(u===0)u=Math.random();
  while(v===0)v=Math.random();
  return Math.sqrt(-2*Math.log(u))*Math.cos(2*Math.PI*v);
}

// ─── MONTE CARLO ENGINE ───────────────────────────────────────────────────────
function runMonteCarlo(p) {
  const { baseDemand,cv,initialStock,safetyDays,leadTime,shelfLife,moq,horizon,nSim,seasonality } = p;
  const safetyMonths = safetyDays/30;
  const rop = baseDemand*(leadTime+safetyMonths);
  const orderQty = Math.ceil(rop/moq)*moq;

  const stockByM=[],demByM=[],salesByM=[],ordByM=[],expByM=[],lostByM=[];
  for(let m=0;m<horizon;m++){stockByM.push([]);demByM.push([]);salesByM.push([]);ordByM.push([]);expByM.push([]);lostByM.push([]);}

  for(let sim=0;sim<nSim;sim++){
    let lots=[{qty:initialStock,exp:shelfLife}];
    let transit=[];
    for(let m=0;m<horizon;m++){
      const smod=seasonality[m%12]/100, sigma=cv/100;
      const demand=Math.max(0,Math.round(baseDemand*(1+smod)*(1+sigma*randn())));
      // arrivi
      transit.filter(o=>o.arr===m).forEach(o=>lots.push({qty:o.qty,exp:o.exp}));
      transit=transit.filter(o=>o.arr!==m);
      // scaduti
      let expired=0;
      lots=lots.filter(l=>{ if(l.exp<=m){expired+=l.qty;return false;}return true; });
      expByM[m].push(expired);
      // vendite FIFO
      lots.sort((a,b)=>a.exp-b.exp);
      let rem=demand,sold=0;
      for(const lot of lots){ const t=Math.min(lot.qty,rem);lot.qty-=t;sold+=t;rem-=t;if(!rem)break; }
      lots=lots.filter(l=>l.qty>0);
      lostByM[m].push(rem);
      // stock + ROP
      const stockNow=lots.reduce((s,l)=>s+l.qty,0);
      const inTrQty=transit.reduce((s,o)=>s+o.qty,0);
      let orderSent=0;
      if(stockNow+inTrQty<rop){
        const qty=Math.max(orderQty,moq);
        transit.push({qty,arr:m+leadTime,exp:m+leadTime+(shelfLife-leadTime)});
        orderSent=qty;
      }
      stockByM[m].push(stockNow); demByM[m].push(demand);
      salesByM[m].push(sold); ordByM[m].push(orderSent);
    }
  }

  const pct=(arr,p)=>{const s=[...arr].sort((a,b)=>a-b);return s[Math.floor(p/100*(s.length-1))];};

  const chartData=Array.from({length:horizon},(_,m)=>({
    month:`M${m+1}`,
    stockP10:pct(stockByM[m],10), stockP50:pct(stockByM[m],50), stockP90:pct(stockByM[m],90),
    demandP50:pct(demByM[m],50), salesP50:pct(salesByM[m],50),
    ordersP50:pct(ordByM[m],50), expiredP50:pct(expByM[m],50),
    safetyLine:Math.round(baseDemand*safetyMonths),
  }));

  const totDem=demByM.reduce((s,a)=>s+pct(a,50),0);
  const totSales=salesByM.reduce((s,a)=>s+pct(a,50),0);
  const totExp=expByM.reduce((s,a)=>s+pct(a,50),0);
  const fillRate=totDem>0?(totSales/totDem)*100:100;
  const stockoutMonths=stockByM.filter(a=>pct(a,50)===0).length;
  const avgCoverage=chartData.reduce((s,d)=>s+d.stockP50,0)/horizon/baseDemand;
  const totalOrdersN=chartData.filter(d=>d.ordersP50>0).length;
  const totalOrdersQty=chartData.reduce((s,d)=>s+d.ordersP50,0);

  return {
    chartData,
    kpi:{fillRate,stockoutMonths,expiredP50:totExp,avgCoverage},
    scenario:{
      stockFinalP10:pct(stockByM[horizon-1],10),
      stockFinalP50:pct(stockByM[horizon-1],50),
      stockFinalP90:pct(stockByM[horizon-1],90),
    },
    meta:{totalOrdersN,totalOrdersQty,rop,orderQty,safetyMonths},
  };
}

// ─── CHART: Canvas-based con Chart.js ────────────────────────────────────────
function StockChart({data}){
  const ref=useRef();
  useEffect(()=>{
    const ctx=ref.current.getContext('2d');
    const labels=data.map(d=>d.month);
    const ch=new Chart(ctx,{
      type:'line',
      data:{
        labels,
        datasets:[
          {label:'P90',data:data.map(d=>d.stockP90),fill:'+1',backgroundColor:'rgba(200,224,244,0.4)',borderColor:'transparent',pointRadius:0,tension:0.3},
          {label:'P50',data:data.map(d=>d.stockP50),fill:false,borderColor:T.blue,borderWidth:2,pointRadius:0,tension:0.3},
          {label:'P10',data:data.map(d=>d.stockP10),fill:false,borderColor:'rgba(200,224,244,0.6)',borderWidth:1,pointRadius:0,tension:0.3},
          {label:'Safety',data:data.map(d=>d.safetyLine),fill:false,borderColor:T.warn,borderWidth:1.5,borderDash:[4,3],pointRadius:0},
        ]
      },
      options:{
        responsive:true,maintainAspectRatio:false,
        plugins:{legend:{display:false},tooltip:{mode:'index',intersect:false,callbacks:{label:ctx=>`${ctx.dataset.label}: ${Number(ctx.raw).toLocaleString('it-IT')}`}}},
        scales:{
          x:{ticks:{font:{size:9},color:T.mid,maxTicksLimit:8},grid:{color:T.border}},
          y:{ticks:{font:{size:9},color:T.mid,callback:v=>(v/1000).toFixed(0)+'K'},grid:{color:T.border}},
        }
      }
    });
    return()=>ch.destroy();
  },[]);
  return <canvas ref={ref} style={{width:'100%',height:'190px'}}/>;
}

function DemandChart({data}){
  const ref=useRef();
  useEffect(()=>{
    const ctx=ref.current.getContext('2d');
    const ch=new Chart(ctx,{
      type:'bar',
      data:{
        labels:data.map(d=>d.month),
        datasets:[
          {label:'Domanda P50',data:data.map(d=>d.demandP50),backgroundColor:T.lightBg,borderRadius:2},
          {label:'Vendite P50',data:data.map(d=>d.salesP50),backgroundColor:T.blue,borderRadius:2},
        ]
      },
      options:{
        responsive:true,maintainAspectRatio:false,
        plugins:{legend:{display:false},tooltip:{mode:'index',intersect:false,callbacks:{label:ctx=>`${ctx.dataset.label}: ${Number(ctx.raw).toLocaleString('it-IT')}`}}},
        scales:{
          x:{ticks:{font:{size:9},color:T.mid,maxTicksLimit:8},grid:{color:T.border}},
          y:{ticks:{font:{size:9},color:T.mid,callback:v=>(v/1000).toFixed(0)+'K'},grid:{color:T.border}},
        }
      }
    });
    return()=>ch.destroy();
  },[]);
  return <canvas ref={ref} style={{width:'100%',height:'190px'}}/>;
}

function OrdersChart({data}){
  const ref=useRef();
  useEffect(()=>{
    const ctx=ref.current.getContext('2d');
    const ch=new Chart(ctx,{
      data:{
        labels:data.map(d=>d.month),
        datasets:[
          {type:'bar',label:'Ordini P50',data:data.map(d=>d.ordersP50),backgroundColor:T.cyan,borderRadius:2,yAxisID:'y'},
          {type:'bar',label:'Scaduti P50',data:data.map(d=>d.expiredP50),backgroundColor:T.err,borderRadius:2,yAxisID:'y'},
          {type:'line',label:'Stock P50',data:data.map(d=>d.stockP50),borderColor:T.blue,borderWidth:2,fill:false,pointRadius:0,tension:0.3,yAxisID:'y2'},
        ]
      },
      options:{
        responsive:true,maintainAspectRatio:false,
        plugins:{legend:{display:false},tooltip:{mode:'index',intersect:false,callbacks:{label:ctx=>`${ctx.dataset.label}: ${Number(ctx.raw).toLocaleString('it-IT')}`}}},
        scales:{
          x:{ticks:{font:{size:9},color:T.mid,maxTicksLimit:8},grid:{color:T.border}},
          y:{ticks:{font:{size:9},color:T.mid,callback:v=>(v/1000).toFixed(0)+'K'},grid:{color:T.border}},
          y2:{position:'right',ticks:{font:{size:9},color:T.mid,callback:v=>(v/1000).toFixed(0)+'K'},grid:{display:false}},
        }
      }
    });
    return()=>ch.destroy();
  },[]);
  return <canvas ref={ref} style={{width:'100%',height:'190px'}}/>;
}

// ─── UI ATOMS ─────────────────────────────────────────────────────────────────
const fmt = v => Number(v).toLocaleString('it-IT');

function SliderField({label,value,min,max,step=1,onChange,unit='',helper}){
  return(
    <div style={{marginBottom:16}}>
      <div style={{display:'flex',justifyContent:'space-between',marginBottom:6}}>
        <span style={{fontSize:13,color:T.text,fontWeight:500}}>{label}</span>
        <span style={{fontSize:13,color:T.blue,fontWeight:600}}>{value}{unit}</span>
      </div>
      <input type="range" min={min} max={max} step={step} value={value} onChange={e=>onChange(Number(e.target.value))}/>
      {helper&&<div style={{fontSize:11,color:T.mid,marginTop:4}}>{helper}</div>}
    </div>
  );
}

function NumberField({label,value,onChange,unit=''}){
  return(
    <div style={{marginBottom:14}}>
      <div style={{fontSize:13,color:T.text,fontWeight:500,marginBottom:6}}>{label}</div>
      <div style={{display:'flex',alignItems:'center',background:T.off,border:`1px solid ${T.border}`,borderRadius:10,padding:'10px 14px'}}>
        <input type="number" value={value} onChange={e=>onChange(Number(e.target.value))}
          style={{flex:1,background:'transparent',border:'none',outline:'none',fontSize:15,color:T.text,fontWeight:500,width:0}}/>
        {unit&&<span style={{fontSize:13,color:T.mid,marginLeft:4}}>{unit}</span>}
      </div>
    </div>
  );
}

function SectionHdr({label}){
  return <div style={{fontSize:11,fontWeight:700,color:T.blue,textTransform:'uppercase',letterSpacing:1,marginBottom:10,marginTop:4,paddingBottom:6,borderBottom:`2px solid ${T.blue}`}}>{label}</div>;
}

// ─── TAB PARAMETRI ────────────────────────────────────────────────────────────
function TabParametri({params,setParams,onRun,loading}){
  const set=k=>v=>setParams(p=>({...p,[k]:v}));
  return(
    <div style={{flex:1,overflowY:'auto',padding:'16px 16px 130px',WebkitOverflowScrolling:'touch'}}>
      <div style={{background:T.lightBg,borderRadius:12,padding:'12px 14px',marginBottom:20,display:'flex',gap:10,alignItems:'flex-start'}}>
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke={T.blue} strokeWidth="2" style={{marginTop:2,flexShrink:0}}><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg>
        <span style={{fontSize:12,color:T.blue,lineHeight:1.5}}>Configura i parametri. Simulazione con distribuzione normale Box-Muller e gestione lotti FIFO.</span>
      </div>

      <SectionHdr label="Domanda"/>
      <NumberField label="Domanda media mensile" value={params.baseDemand} onChange={set('baseDemand')} unit="pz"/>
      <SliderField label="Variabilità domanda (CV)" value={params.cv} min={0} max={50} onChange={set('cv')} unit="%" helper="Coefficiente di variazione della domanda mensile"/>

      <SectionHdr label="Inventario"/>
      <NumberField label="Stock attuale" value={params.initialStock} onChange={set('initialStock')} unit="pz"/>
      <SliderField label="Safety stock" value={params.safetyDays} min={5} max={60} onChange={set('safetyDays')} unit=" gg" helper={`= ${(params.safetyDays/30).toFixed(1)} mesi di copertura`}/>
      <NumberField label="Shelf life" value={params.shelfLife} onChange={set('shelfLife')} unit="mesi"/>

      <SectionHdr label="Parametri Ordine"/>
      <SliderField label="Lead time" value={params.leadTime} min={1} max={18} onChange={set('leadTime')} unit=" mesi" helper="Tempo fisso conferma → ricezione"/>
      <NumberField label="MOQ (Minimum Order Quantity)" value={params.moq} onChange={set('moq')} unit="pz"/>

      <SectionHdr label="Simulazione"/>
      <SliderField label="Orizzonte" value={params.horizon} min={12} max={36} onChange={set('horizon')} unit=" mesi"/>
      <SliderField label="Numero simulazioni" value={params.nSim} min={200} max={1000} step={100} onChange={set('nSim')} helper="Più run = maggiore precisione"/>
    </div>
  );
}

// ─── TAB STAGIONALITÀ ─────────────────────────────────────────────────────────
function TabStagionalita({seasonality,setSeasonality}){
  return(
    <div style={{flex:1,overflowY:'auto',padding:'16px 16px 100px',WebkitOverflowScrolling:'touch'}}>
      <div style={{background:T.lightBg,borderRadius:12,padding:'12px 14px',marginBottom:16}}>
        <p style={{fontSize:12,color:T.blue,margin:0,lineHeight:1.5}}>Variazione % domanda mensile rispetto alla media. Range: −50% → +100%</p>
      </div>
      <button onClick={()=>setSeasonality([...STAGIONALITA_ESEMPIO])}
        style={{width:'100%',background:T.off,border:`1px solid ${T.border}`,borderRadius:10,padding:'12px 16px',fontSize:14,color:T.blue,fontWeight:600,cursor:'pointer',marginBottom:20,display:'flex',alignItems:'center',justifyContent:'center',gap:8}}>
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke={T.blue} strokeWidth="2"><rect x="3" y="4" width="18" height="18" rx="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/></svg>
        Applica stagionalità farmaceutica esempio
      </button>
      {MESI.map((m,i)=>(
        <div key={i} style={{marginBottom:14}}>
          <div style={{display:'flex',justifyContent:'space-between',marginBottom:6}}>
            <span style={{fontSize:13,color:T.text,fontWeight:500}}>{m}</span>
            <span style={{fontSize:13,fontWeight:600,color:seasonality[i]>0?T.ok:seasonality[i]<0?T.err:T.mid}}>
              {seasonality[i]>0?'+':''}{seasonality[i]}%
            </span>
          </div>
          <input type="range" min={-50} max={100} step={1} value={seasonality[i]}
            onChange={e=>{const n=[...seasonality];n[i]=Number(e.target.value);setSeasonality(n);}}/>
        </div>
      ))}
    </div>
  );
}

// ─── TAB RISULTATI ────────────────────────────────────────────────────────────
function TabRisultati({result,params}){
  const [chartTab,setChartTab]=useState(0);
  if(!result) return(
    <div style={{flex:1,display:'flex',flexDirection:'column',alignItems:'center',justifyContent:'center',padding:32,gap:16}}>
      <div style={{width:64,height:64,borderRadius:'50%',background:T.lightBg,display:'flex',alignItems:'center',justifyContent:'center'}}>
        <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke={T.blue} strokeWidth="1.5"><line x1="18" y1="20" x2="18" y2="10"/><line x1="12" y1="20" x2="12" y2="4"/><line x1="6" y1="20" x2="6" y2="14"/></svg>
      </div>
      <div style={{textAlign:'center'}}>
        <div style={{fontSize:16,fontWeight:700,color:T.text}}>Nessuna simulazione</div>
        <div style={{fontSize:13,color:T.mid,marginTop:4}}>Vai in "Parametri" ed esegui la simulazione</div>
      </div>
    </div>
  );

  const {kpi,chartData,scenario,meta}=result;
  const fillColor=kpi.fillRate>=95?T.ok:kpi.fillRate>=85?T.warn:T.err;
  const status=kpi.fillRate>=95&&kpi.stockoutMonths===0
    ?{emoji:'✅',label:'Strategia efficiente',color:T.ok}
    :kpi.fillRate>=85
    ?{emoji:'⚠️',label:'Richiede ottimizzazione',color:T.warn}
    :{emoji:'🚨',label:'Scenario critico',color:T.err};

  return(
    <div style={{flex:1,overflowY:'auto',padding:'16px 16px 100px',WebkitOverflowScrolling:'touch'}}>

      {/* KPI 2×2 */}
      <div style={{display:'grid',gridTemplateColumns:'1fr 1fr',gap:10,marginBottom:20}}>
        {[
          {label:'Fill Rate',value:`${kpi.fillRate.toFixed(1)}%`,sub:'domanda soddisfatta',color:fillColor,delay:'0ms',
           icon:<svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke={fillColor} strokeWidth="2"><polyline points="20 6 9 17 4 12"/></svg>},
          {label:'Mesi Stockout',value:kpi.stockoutMonths,sub:'mesi con stock = 0',color:kpi.stockoutMonths===0?T.ok:T.err,delay:'120ms',
           icon:<svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke={kpi.stockoutMonths===0?T.ok:T.err} strokeWidth="2"><path d="M10.29 3.86L1.82 18a2 2 0 001.71 3h16.94a2 2 0 001.71-3L13.71 3.86a2 2 0 00-3.42 0z"/><line x1="12" y1="9" x2="12" y2="13"/><line x1="12" y1="17" x2="12.01" y2="17"/></svg>},
          {label:'Pz Scaduti',value:`${(kpi.expiredP50/1000).toFixed(1)}K`,sub:'stima P50 periodo',color:T.warn,delay:'240ms',
           icon:<svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke={T.warn} strokeWidth="2"><path d="M21 16V8a2 2 0 00-1-1.73l-7-4a2 2 0 00-2 0l-7 4A2 2 0 003 8v8a2 2 0 001 1.73l7 4a2 2 0 002 0l7-4A2 2 0 0021 16z"/></svg>},
          {label:'Copertura Media',value:`${kpi.avgCoverage.toFixed(1)}m`,sub:'mesi di stock medio',color:T.blue,delay:'360ms',
           icon:<svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke={T.blue} strokeWidth="2"><polyline points="22 7 13.5 15.5 8.5 10.5 2 17"/><polyline points="16 7 22 7 22 13"/></svg>},
        ].map((k,i)=>(
          <div key={i} className="kpi-card" style={{
            background:T.white,borderRadius:12,padding:'14px',
            border:`1px solid ${T.border}`,boxShadow:'0 2px 12px rgba(0,47,92,0.10)',
            animationDelay:k.delay
          }}>
            <div style={{display:'flex',alignItems:'center',gap:8,marginBottom:8}}>
              <div style={{width:28,height:28,borderRadius:8,background:k.color+'18',display:'flex',alignItems:'center',justifyContent:'center'}}>{k.icon}</div>
              <span style={{fontSize:10,color:T.mid,fontWeight:500,textTransform:'uppercase',letterSpacing:0.5}}>{k.label}</span>
            </div>
            <div style={{fontSize:22,fontWeight:700,color:T.text,fontFamily:"'Barlow',sans-serif"}}>{k.value}</div>
            <div style={{fontSize:11,color:T.mid,marginTop:2}}>{k.sub}</div>
          </div>
        ))}
      </div>

      {/* Scenario bands */}
      <div style={{background:T.white,borderRadius:12,padding:'14px 16px',border:`1px solid ${T.border}`,boxShadow:'0 2px 12px rgba(0,47,92,0.10)',marginBottom:20}}>
        <div style={{fontSize:13,fontWeight:700,color:T.text,marginBottom:8}}>Stock Finale — Scenari</div>
        {[
          {label:'Pessimistico',pct:'P10',val:scenario.stockFinalP10,color:T.err},
          {label:'Base',pct:'P50',val:scenario.stockFinalP50,color:T.blue},
          {label:'Ottimistico',pct:'P90',val:scenario.stockFinalP90,color:T.ok},
        ].map((s,i)=>(
          <div key={i} style={{display:'flex',alignItems:'center',gap:12,padding:'10px 0',borderBottom:i<2?`1px solid ${T.border}`:'none'}}>
            <div style={{width:8,height:8,borderRadius:'50%',background:s.color,flexShrink:0}}/>
            <div style={{flex:1}}>
              <div style={{fontSize:12,color:T.mid}}>{s.pct}</div>
              <div style={{fontSize:13,fontWeight:600,color:T.text}}>{s.label}</div>
            </div>
            <div style={{fontSize:16,fontWeight:700,color:T.blue,fontFamily:"'Barlow',sans-serif"}}>{fmt(s.val)} pz</div>
          </div>
        ))}
      </div>

      {/* Grafici */}
      <div style={{background:T.white,borderRadius:12,border:`1px solid ${T.border}`,boxShadow:'0 2px 12px rgba(0,47,92,0.10)',marginBottom:20,overflow:'hidden'}}>
        <div style={{display:'flex',borderBottom:`1px solid ${T.border}`}}>
          {['Stock','Domanda','Ordini'].map((t,i)=>(
            <button key={i} onClick={()=>setChartTab(i)} style={{
              flex:1,padding:'10px 0',fontSize:12,fontWeight:600,
              background:chartTab===i?T.blue:'transparent',
              color:chartTab===i?T.white:T.mid,
              border:'none',cursor:'pointer',transition:'all 0.2s',
            }}>{t}</button>
          ))}
        </div>
        <div style={{padding:'12px 8px 4px'}}>
          {chartTab===0&&<StockChart data={chartData}/>}
          {chartTab===1&&<DemandChart data={chartData}/>}
          {chartTab===2&&<OrdersChart data={chartData}/>}
        </div>
        <div style={{padding:'4px 12px 10px',fontSize:10,color:T.mid}}>
          {chartTab===0&&'Banda = P10–P90 · Blu = P50 · Tratteggio = Safety Stock'}
          {chartTab===1&&'Grigio = Domanda P50 · Blu = Vendite P50'}
          {chartTab===2&&'Cyan = Ordini · Rosso = Scaduti · Linea = Stock P50'}
        </div>
      </div>

      {/* Resoconto strategico */}
      <div style={{background:T.white,borderRadius:12,border:`1px solid ${T.border}`,boxShadow:'0 2px 12px rgba(0,47,92,0.10)',overflow:'hidden'}}>
        <div style={{background:T.navy,padding:'12px 16px',display:'flex',alignItems:'center',gap:10}}>
          <span style={{fontSize:18}}>{status.emoji}</span>
          <span style={{fontSize:14,fontWeight:700,color:T.white}}>{status.label}</span>
        </div>
        <div style={{padding:16}}>
          {[
            {label:'Strategia ROP',value:`${fmt(meta.rop)} pz trigger`},
            {label:"Lotto d'ordine",value:`${fmt(meta.orderQty)} pz (${Math.round(meta.orderQty/params.moq)}× MOQ)`},
            {label:'Ordini nel periodo',value:`${meta.totalOrdersN} ordini · ${(meta.totalOrdersQty/1000).toFixed(0)}K pz totali`},
            {label:'Orizzonte simulato',value:`${params.horizon} mesi · ${params.nSim} run`},
          ].map((r,i,arr)=>(
            <div key={i} style={{display:'flex',justifyContent:'space-between',alignItems:'flex-start',paddingBottom:10,marginBottom:10,borderBottom:i<arr.length-1?`1px solid ${T.border}`:'none'}}>
              <span style={{fontSize:12,color:T.mid,flex:1}}>{r.label}</span>
              <span style={{fontSize:12,fontWeight:600,color:T.text,textAlign:'right',flex:1}}>{r.value}</span>
            </div>
          ))}
          {kpi.fillRate<95&&(
            <div style={{background:T.err+'12',border:`1px solid ${T.err}30`,borderRadius:10,padding:'10px 12px',marginTop:12,display:'flex',gap:8,alignItems:'flex-start'}}>
              <span style={{fontSize:14}}>🚨</span>
              <p style={{fontSize:12,color:T.text,margin:0,lineHeight:1.5}}>Fill rate {kpi.fillRate.toFixed(1)}% &lt; 95%. Valuta di ridurre il lead time, aumentare il safety stock a ≥30 gg, o pre-posizionare stock in periodi ad alta stagionalità.</p>
            </div>
          )}
          {kpi.expiredP50>params.baseDemand&&(
            <div style={{background:T.warn+'12',border:`1px solid ${T.warn}30`,borderRadius:10,padding:'10px 12px',marginTop:12,display:'flex',gap:8,alignItems:'flex-start'}}>
              <span style={{fontSize:14}}>⚠️</span>
              <p style={{fontSize:12,color:T.text,margin:0,lineHeight:1.5}}>Scaduti stimati ({(kpi.expiredP50/1000).toFixed(1)}K pz) superiori a 1 mese di domanda. Valuta riduzione MOQ o frequenza ordini più alta.</p>
            </div>
          )}
          {kpi.fillRate>=95&&kpi.stockoutMonths===0&&kpi.expiredP50<=params.baseDemand&&(
            <div style={{background:T.ok+'12',border:`1px solid ${T.ok}30`,borderRadius:10,padding:'10px 12px',marginTop:12,display:'flex',gap:8,alignItems:'flex-start'}}>
              <span style={{fontSize:14}}>✅</span>
              <p style={{fontSize:12,color:T.text,margin:0,lineHeight:1.5}}>La strategia ROP con i parametri attuali garantisce copertura adeguata senza eccesso di scorte in scadenza.</p>
            </div>
          )}
        </div>
      </div>
    </div>
  );
}

// ─── APP ROOT ─────────────────────────────────────────────────────────────────
function App(){
  const [tab,setTab]=useState('params');
  const [loading,setLoading]=useState(false);
  const [result,setResult]=useState(null);
  const [params,setParams]=useState({
    baseDemand:15000,cv:15,initialStock:40000,safetyDays:20,
    leadTime:8,shelfLife:24,moq:20000,horizon:24,nSim:500,
  });
  const [seasonality,setSeasonality]=useState(Array(12).fill(0));

  const handleRun=useCallback(()=>{
    setLoading(true);
    setTimeout(()=>{
      const res=runMonteCarlo({...params,seasonality});
      setResult(res);setLoading(false);setTab('results');
    },60);
  },[params,seasonality]);

  const navTitle=tab==='params'?'Parametri':tab==='results'?'Risultati':'Stagionalità';
  const navSub=tab==='params'?'Supply Chain Monte Carlo':tab==='results'?`${params.nSim} simulazioni · ${params.horizon} mesi`:'Modulazione mensile domanda';

  const tabs=[
    {id:'params',label:'Parametri',
     icon:<svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="1.8"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 00.33 1.82l.06.06a2 2 0 010 2.83 2 2 0 01-2.83 0l-.06-.06a1.65 1.65 0 00-1.82-.33 1.65 1.65 0 00-1 1.51V21a2 2 0 01-4 0v-.09A1.65 1.65 0 009 19.4a1.65 1.65 0 00-1.82.33l-.06.06a2 2 0 01-2.83-2.83l.06-.06A1.65 1.65 0 004.68 15a1.65 1.65 0 00-1.51-1H3a2 2 0 010-4h.09A1.65 1.65 0 004.6 9a1.65 1.65 0 00-.33-1.82l-.06-.06a2 2 0 012.83-2.83l.06.06A1.65 1.65 0 009 4.68a1.65 1.65 0 001-1.51V3a2 2 0 014 0v.09a1.65 1.65 0 001 1.51 1.65 1.65 0 001.82-.33l.06-.06a2 2 0 012.83 2.83l-.06.06A1.65 1.65 0 0019.4 9a1.65 1.65 0 001.51 1H21a2 2 0 010 4h-.09a1.65 1.65 0 00-1.51 1z"/></svg>},
    {id:'results',label:'Risultati',
     icon:<svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="1.8"><line x1="18" y1="20" x2="18" y2="10"/><line x1="12" y1="20" x2="12" y2="4"/><line x1="6" y1="20" x2="6" y2="14"/></svg>},
    {id:'stagionalita',label:'Stagionalità',
     icon:<svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="1.8"><rect x="3" y="4" width="18" height="18" rx="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/></svg>},
  ];

  return(
    <div id="app-shell" style={{
      width:390,height:844,background:T.off,borderRadius:44,overflow:'hidden',
      boxShadow:'0 30px 80px rgba(0,20,60,0.55),0 0 0 10px #1A2B3C',
      position:'relative',display:'flex',flexDirection:'column',
      fontFamily:"'Inter','Arial',sans-serif",
    }}>
      {/* Notch */}
      <div id="notch" style={{position:'absolute',top:0,left:'50%',transform:'translateX(-50%)',width:126,height:34,background:'#1A2B3C',borderBottomLeftRadius:18,borderBottomRightRadius:18,zIndex:20}}/>

      {/* Status bar */}
      <div id="status-bar" style={{height:44,background:T.blue,display:'flex',alignItems:'flex-end',paddingBottom:8,paddingLeft:16,paddingRight:16,justifyContent:'space-between',flexShrink:0}}>
        <span style={{color:T.white,fontSize:12,fontWeight:600}}>9:41</span>
        <div style={{width:16,height:10,border:`1.5px solid ${T.white}`,borderRadius:2,position:'relative'}}>
          <div style={{position:'absolute',left:1,top:1,bottom:1,right:4,background:T.white,borderRadius:1}}/>
          <div style={{position:'absolute',right:-4,top:3,width:2,height:4,background:T.white,borderRadius:1}}/>
        </div>
      </div>

      {/* Nav bar */}
      <div style={{background:T.blue,paddingTop:8,paddingBottom:12,paddingLeft:16,paddingRight:80,flexShrink:0}}>
        <div style={{color:T.white,fontSize:20,fontWeight:700,fontFamily:"'Barlow','Arial Black',sans-serif",letterSpacing:0.3}}>{navTitle}</div>
        <div style={{color:T.lightBg,fontSize:12,marginTop:2}}>{navSub}</div>
      </div>

      {/* Logo */}
      <div style={{position:'absolute',top:52,right:16,zIndex:10}}>
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 24" width="72">
          <text x="0" y="19" fontFamily="'Arial Black',sans-serif" fontWeight="900" fontSize="16" fill="#FFFFFF" letterSpacing="1.5">SANDOZ</text>
        </svg>
      </div>

      {/* Content */}
      <div style={{flex:1,display:'flex',flexDirection:'column',overflow:'hidden',position:'relative'}}>
        {tab==='params'&&<TabParametri params={params} setParams={setParams} onRun={handleRun} loading={loading}/>}
        {tab==='results'&&<TabRisultati result={result} params={params}/>}
        {tab==='stagionalita'&&<TabStagionalita seasonality={seasonality} setSeasonality={setSeasonality}/>}

        {/* Tab bar */}
        <div id="tab-bar" style={{
          position:'absolute',bottom:0,left:0,right:0,
          height:80,background:T.white,borderTop:`1px solid ${T.border}`,
          display:'flex',alignItems:'flex-start',paddingTop:8,flexShrink:0,
        }}>
          {tabs.map(({id,label,icon})=>(
            <button key={id} onClick={()=>setTab(id)} style={{
              flex:1,display:'flex',flexDirection:'column',alignItems:'center',gap:4,
              background:'none',border:'none',cursor:'pointer',
              color:tab===id?T.blue:T.pale,transition:'color 0.2s',position:'relative',
              minHeight:44,
            }}>
              {icon}
              <span style={{fontSize:10,fontWeight:tab===id?600:400}}>{label}</span>
              {id==='results'&&!!result&&tab!=='results'&&(
                <div style={{position:'absolute',top:2,right:'28%',width:6,height:6,borderRadius:'50%',background:T.ok}}/>
              )}
            </button>
          ))}
        </div>
      </div>

      {/* CTA Run button */}
      <button id="run-btn" onClick={handleRun} disabled={loading} style={{
        position:'fixed',bottom:88,left:'50%',transform:'translateX(-50%)',
        width:358,background:loading?T.pale:T.blue,color:T.white,
        border:'none',borderRadius:12,height:52,fontSize:16,fontWeight:700,
        display:'flex',alignItems:'center',justifyContent:'center',gap:10,
        cursor:loading?'not-allowed':'pointer',
        boxShadow:'0 4px 16px rgba(0,74,133,0.25)',
        transition:'background 0.2s',fontFamily:"'Barlow',sans-serif",
        zIndex:tab==='params'?100:-1,opacity:tab==='params'?1:0,
        pointerEvents:tab==='params'?'auto':'none',
      }}>
        {loading
          ?<><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="white" strokeWidth="2" style={{animation:'spin 1s linear infinite'}}><path d="M21 12a9 9 0 11-6.219-8.56"/></svg> Simulazione in corso…</>
          :<><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="white" strokeWidth="2" strokeLinejoin="round"><polygon points="5 3 19 12 5 21 5 3"/></svg> Esegui Simulazione</>
        }
      </button>

      {/* Home indicator */}
      <div id="home-ind" style={{position:'absolute',bottom:8,left:'50%',transform:'translateX(-50%)',width:120,height:5,background:T.text,borderRadius:3,opacity:0.3}}/>
    </div>
  );
}

ReactDOM.createRoot(document.getElementById('root')).render(<App/>);
</script>

<!-- Service Worker per offline/PWA -->

<script>
if('serviceWorker' in navigator){
  window.addEventListener('load',()=>{
    // SW inline via Blob — nessun file sw.js separato necessario
    const swCode=`
      const CACHE='simchain-v1';
      const URLS=['/'];
      self.addEventListener('install',e=>e.waitUntil(caches.open(CACHE).then(c=>c.addAll(URLS))));
      self.addEventListener('fetch',e=>e.respondWith(
        caches.match(e.request).then(r=>r||fetch(e.request)).catch(()=>caches.match('/'))
      ));
    `;
    const blob=new Blob([swCode],{type:'application/javascript'});
    const url=URL.createObjectURL(blob);
    navigator.serviceWorker.register(url).catch(()=>{});
  });
}
</script>

</body>
</html>************