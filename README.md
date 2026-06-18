<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Padres Jersey Manager</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/umd/react.production.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/umd/react-dom.production.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.23.2/babel.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&display=swap" rel="stylesheet" />
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    html, body { height: 100%; }
    body { font-family: 'Inter', sans-serif; background: #0a0e1a; color: #fff; }
    #root { min-height: 100vh; }

    .app { min-height: 100vh; background: #0a0e1a; }
    .header { background: linear-gradient(135deg,#1a1f35 0%,#0d1124 100%); border-bottom: 1px solid rgba(255,198,47,.15); padding: 0 24px; position: sticky; top: 0; z-index: 100; }
    .header-inner { max-width: 1100px; margin: 0 auto; display: flex; align-items: center; justify-content: space-between; padding: 16px 0; gap: 16px; flex-wrap: wrap; }
    .logo-area { display: flex; align-items: center; gap: 12px; }
    .logo-icon { width: 44px; height: 44px; border-radius: 12px; background: linear-gradient(135deg,#FFC62F,#c9a000); display: flex; align-items: center; justify-content: center; font-size: 22px; box-shadow: 0 4px 16px rgba(255,198,47,.3); }
    .logo-title { font-size: 18px; font-weight: 800; color: #fff; letter-spacing: -.3px; }
    .logo-sub { font-size: 12px; color: rgba(255,255,255,.45); font-weight: 500; margin-top: 1px; }
    .threshold-control { display: flex; align-items: center; gap: 8px; }
    .threshold-label { font-size: 12px; color: rgba(255,255,255,.45); font-weight: 500; }
    .threshold-input { width: 52px; text-align: center; border-radius: 8px; padding: 6px 8px; background: rgba(255,255,255,.07); color: #fff; border: 1px solid rgba(255,255,255,.12); font-size: 14px; font-weight: 700; outline: none; font-family: inherit; transition: border-color .2s; }
    .threshold-input:focus { border-color: #FFC62F; }

    .nav { background: rgba(255,255,255,.02); border-bottom: 1px solid rgba(255,255,255,.06); }
    .nav-inner { max-width: 1100px; margin: 0 auto; display: flex; gap: 4px; overflow-x: auto; }
    .nav-btn { padding: 14px 20px; font-size: 13px; font-weight: 600; cursor: pointer; border: none; background: none; color: rgba(255,255,255,.4); border-bottom: 2px solid transparent; transition: all .2s; white-space: nowrap; font-family: inherit; display: flex; align-items: center; gap: 7px; }
    .nav-btn:hover { color: rgba(255,255,255,.75); }
    .nav-btn.active { color: #FFC62F; border-bottom-color: #FFC62F; }
    .nav-badge { background: #e05a2a; color: #fff; border-radius: 999px; padding: 2px 7px; font-size: 11px; font-weight: 700; }

    .content { max-width: 1100px; margin: 0 auto; padding: 28px 24px; }

    .style-tabs { display: flex; gap: 10px; margin-bottom: 24px; }
    .style-tab { flex: 1; padding: 14px 20px; border-radius: 14px; cursor: pointer; border: 1.5px solid transparent; font-family: inherit; font-weight: 700; font-size: 14px; transition: all .2s; display: flex; align-items: center; gap: 10px; }
    .style-tab.home { background: rgba(75,0,0,.3); color: rgba(255,255,255,.5); border-color: rgba(75,0,0,.4); }
    .style-tab.home.active { background: linear-gradient(135deg,#4B0000,#2d0000); color: #FFC62F; border-color: #8B1A1A; box-shadow: 0 4px 20px rgba(139,26,26,.4); }
    .style-tab.city { background: rgba(255,198,47,.05); color: rgba(255,255,255,.5); border-color: rgba(255,198,47,.1); }
    .style-tab.city.active { background: linear-gradient(135deg,#3d3000,#1a1500); color: #FFC62F; border-color: #FFC62F; box-shadow: 0 4px 20px rgba(255,198,47,.2); }
    .style-tab-dot { width: 10px; height: 10px; border-radius: 50%; flex-shrink: 0; }
    .style-tab-dot.home { background: #8B1A1A; }
    .style-tab-dot.home.active { background: #FFC62F; }
    .style-tab-dot.city { background: #555; }
    .style-tab-dot.city.active { background: #FFC62F; }

    .card { background: linear-gradient(135deg,#141929 0%,#0f1420 100%); border: 1px solid rgba(255,255,255,.07); border-radius: 18px; padding: 24px; margin-bottom: 20px; }
    .card-title { font-size: 11px; font-weight: 700; color: rgba(255,255,255,.35); text-transform: uppercase; letter-spacing: 1.2px; margin-bottom: 14px; }

    .cell-grid { display: flex; flex-wrap: wrap; gap: 6px; }
    .inv-cell { border-radius: 10px; padding: 6px 4px; text-align: center; cursor: pointer; min-width: 42px; transition: all .15s; user-select: none; }
    .inv-cell:hover { transform: translateY(-1px); }
    .inv-cell .cell-key { font-size: 9px; font-weight: 600; opacity: .6; letter-spacing: .5px; margin-bottom: 2px; }
    .inv-cell .cell-val { font-size: 14px; font-weight: 800; }
    .inv-cell input { width: 38px; background: transparent; border: none; outline: none; font-size: 14px; font-weight: 800; text-align: center; font-family: inherit; color: inherit; }
    .cell-ok { background: rgba(255,255,255,.05); color: rgba(255,255,255,.8); }
    .cell-ok:hover { background: rgba(255,255,255,.09); }
    .cell-low { background: rgba(255,198,47,.15); color: #FFC62F; border: 1px solid rgba(255,198,47,.3); }
    .cell-low:hover { background: rgba(255,198,47,.22); }
    .cell-out { background: rgba(220,38,38,.2); color: #f87171; border: 1px solid rgba(220,38,38,.3); }
    .cell-out:hover { background: rgba(220,38,38,.28); }

    .legend { display: flex; gap: 18px; margin-top: 14px; flex-wrap: wrap; }
    .legend-item { display: flex; align-items: center; gap: 6px; font-size: 11px; color: rgba(255,255,255,.4); font-weight: 500; }
    .legend-dot { width: 8px; height: 8px; border-radius: 50%; flex-shrink: 0; }

    .btn { border: none; cursor: pointer; font-family: inherit; font-weight: 600; border-radius: 10px; padding: 10px 18px; font-size: 13px; transition: all .18s; display: inline-flex; align-items: center; gap: 7px; }
    .btn-primary { background: linear-gradient(135deg,#FFC62F,#e6a800); color: #0a0e1a; box-shadow: 0 4px 14px rgba(255,198,47,.3); }
    .btn-primary:hover { box-shadow: 0 6px 20px rgba(255,198,47,.45); transform: translateY(-1px); }
    .btn-success { background: linear-gradient(135deg,#16a34a,#15803d); color: #fff; box-shadow: 0 4px 14px rgba(22,163,74,.3); }
    .btn-success:hover { box-shadow: 0 6px 20px rgba(22,163,74,.4); transform: translateY(-1px); }
    .btn-ghost { background: rgba(255,255,255,.06); color: rgba(255,255,255,.6); border: 1px solid rgba(255,255,255,.1); }
    .btn-ghost:hover { background: rgba(255,255,255,.1); color: #fff; }
    .btn-row { display: flex; gap: 10px; margin-top: 14px; flex-wrap: wrap; }

    .code-hint { background: rgba(255,255,255,.03); border: 1px solid rgba(255,255,255,.07); border-radius: 10px; padding: 12px 14px; margin-bottom: 14px; font-family: 'Courier New',monospace; font-size: 12px; color: rgba(255,255,255,.35); line-height: 1.7; }
    .code-hint .hint-label { font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; color: rgba(255,198,47,.5); margin-bottom: 6px; }
    .styled-textarea { width: 100%; border-radius: 12px; padding: 14px 16px; background: rgba(255,255,255,.04); color: #e5e7eb; border: 1px solid rgba(255,255,255,.1); font-size: 13px; font-family: 'Courier New',monospace; resize: vertical; min-height: 160px; outline: none; transition: border-color .2s; line-height: 1.7; }
    .styled-textarea:focus { border-color: rgba(255,198,47,.4); background: rgba(255,255,255,.05); }
    .styled-textarea::placeholder { color: rgba(255,255,255,.2); }

    .section-label { font-size: 22px; font-weight: 800; color: #fff; margin-bottom: 4px; letter-spacing: -.4px; }
    .section-sub { font-size: 13px; color: rgba(255,255,255,.4); margin-bottom: 22px; font-weight: 500; }
    .format-tag { color: #FFC62F; font-weight: 700; background: rgba(255,198,47,.1); border-radius: 5px; padding: 1px 6px; font-family: monospace; }

    .table-wrap { border-radius: 12px; overflow: hidden; border: 1px solid rgba(255,255,255,.07); }
    .data-table { width: 100%; border-collapse: collapse; font-size: 13px; }
    .data-table th { text-align: left; padding: 10px 14px; font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; color: rgba(255,255,255,.3); border-bottom: 1px solid rgba(255,255,255,.07); }
    .data-table td { padding: 11px 14px; border-bottom: 1px solid rgba(255,255,255,.04); }
    .data-table tr:last-child td { border-bottom: none; }
    .data-table tr:hover td { background: rgba(255,255,255,.02); }
    .td-name { font-family: monospace; color: #FFC62F; font-weight: 700; font-size: 14px; }
    .td-num { font-family: monospace; color: #60a5fa; font-weight: 600; }
    .td-size { color: #34d399; font-weight: 600; }
    .td-style { color: rgba(255,255,255,.4); font-size: 12px; }
    .td-item { font-family: monospace; color: #FFC62F; font-weight: 800; font-size: 15px; }
    .td-current { color: rgba(255,255,255,.5); font-weight: 600; }
    .td-add { color: #34d399; font-weight: 700; }
    .td-total { color: #fff; font-weight: 800; }
    .table-bg-even { background: rgba(255,255,255,.02); }

    .deduct-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-top: 16px; }
    .deduct-card { background: rgba(255,255,255,.03); border: 1px solid rgba(255,255,255,.07); border-radius: 12px; padding: 14px; }
    .deduct-card-title { font-size: 12px; font-weight: 700; color: rgba(255,255,255,.6); margin-bottom: 8px; display: flex; align-items: center; gap: 6px; }
    .deduct-line { font-size: 11px; color: rgba(255,255,255,.35); margin-bottom: 4px; font-family: monospace; }

    .error-box { margin-top: 14px; background: rgba(220,38,38,.1); border: 1px solid rgba(220,38,38,.25); border-radius: 12px; padding: 14px 16px; }
    .error-title { color: #f87171; font-weight: 700; font-size: 13px; margin-bottom: 6px; }
    .error-line { color: #fca5a5; font-size: 12px; margin-bottom: 3px; }

    .preview-header { font-size: 15px; font-weight: 700; color: #fff; margin: 20px 0 12px; display: flex; align-items: center; gap: 8px; }
    .preview-count { background: rgba(255,198,47,.15); color: #FFC62F; border-radius: 999px; padding: 2px 10px; font-size: 12px; font-weight: 700; }

    .reorder-empty { text-align: center; padding: 48px 24px; }
    .reorder-empty-icon { font-size: 48px; margin-bottom: 12px; }
    .reorder-empty-text { font-size: 16px; font-weight: 700; color: #34d399; }
    .reorder-section { margin-bottom: 24px; }
    .reorder-section-title { font-size: 14px; font-weight: 700; color: rgba(255,255,255,.6); margin-bottom: 12px; display: flex; align-items: center; gap: 8px; }
    .reorder-chips { display: flex; flex-wrap: wrap; gap: 8px; }
    .reorder-chip { border-radius: 10px; padding: 8px 14px; font-size: 13px; font-weight: 700; }
    .chip-low { background: rgba(255,198,47,.15); color: #FFC62F; border: 1px solid rgba(255,198,47,.3); }
    .chip-out { background: rgba(220,38,38,.2); color: #f87171; border: 1px solid rgba(220,38,38,.3); }

    .toast { position: fixed; top: 20px; right: 20px; z-index: 999; padding: 14px 20px; border-radius: 14px; font-size: 14px; font-weight: 600; box-shadow: 0 8px 32px rgba(0,0,0,.4); display: flex; align-items: center; gap: 10px; animation: slideIn .25s ease; }
    .toast-success { background: linear-gradient(135deg,#16a34a,#15803d); color: #fff; }
    .toast-error { background: linear-gradient(135deg,#dc2626,#b91c1c); color: #fff; }
    @keyframes slideIn { from { opacity:0; transform:translateX(24px); } to { opacity:1; transform:translateX(0); } }

    ::-webkit-scrollbar { width: 6px; height: 6px; }
    ::-webkit-scrollbar-track { background: transparent; }
    ::-webkit-scrollbar-thumb { background: rgba(255,255,255,.1); border-radius: 999px; }
  </style>
</head>
<body>
<div id="root"></div>
<script type="text/babel">
const { useState, useEffect } = React;

const LETTERS = "ABCDEFGHIJKLMNOPQRSTUVWXYZ".split("");
const DIGITS  = "0123456789".split("");
const SIZES   = ["S","M","L","XL","XXL","XXXL"];
const STYLES  = ["Home Padres","City Connect"];
const STORAGE_KEY   = "jersey_inventory_v2";
const THRESHOLD_KEY = "jersey_threshold_v1";

const defaultInventory = () => {
  const inv = {};
  STYLES.forEach(style => {
    inv[style] = { letters:{}, digits:{}, sizes:{} };
    LETTERS.forEach(l => inv[style].letters[l] = 50);
    DIGITS.forEach(d  => inv[style].digits[d]  = 50);
    SIZES.forEach(s   => inv[style].sizes[s]   = 20);
  });
  return inv;
};

const loadInventory  = () => { try { const r = localStorage.getItem(STORAGE_KEY);   if (r) return JSON.parse(r); } catch{} return defaultInventory(); };
const saveInventory  = inv => { try { localStorage.setItem(STORAGE_KEY, JSON.stringify(inv)); } catch{} };
const loadThreshold  = ()  => { try { const r = localStorage.getItem(THRESHOLD_KEY); if (r) return parseInt(r); } catch{} return 5; };
const saveThreshold  = t   => { try { localStorage.setItem(THRESHOLD_KEY, String(t)); } catch{} };

const parseOrders = text => {
  const lines = text.trim().split("\n").filter(l=>l.trim());
  const orders=[], errors=[];
  lines.forEach((line,i) => {
    const parts = line.split(",").map(p=>p.trim());
    if (parts.length < 4) { errors.push(`Line ${i+1}: needs Name, Number, Size, Style`); return; }
    const [name,number,size,...sp] = parts;
    const style = sp.join(",").trim();
    const styleName = STYLES.find(s=>s.toLowerCase()===style.toLowerCase());
    if (!styleName) { errors.push(`Line ${i+1}: Unknown style "${style}"`); return; }
    if (!SIZES.includes(size.toUpperCase())) { errors.push(`Line ${i+1}: Unknown size "${size}"`); return; }
    orders.push({ name:name.toUpperCase(), number, size:size.toUpperCase(), style:styleName });
  });
  return { orders, errors };
};

const parseRestock = text => {
  const lines = text.trim().split("\n").filter(l=>l.trim());
  const entries=[], errors=[];
  lines.forEach((line,i) => {
    const parts = line.split(",").map(p=>p.trim());
    if (parts.length < 3) { errors.push(`Line ${i+1}: needs Item, Quantity, Style`); return; }
    const [item,qty,...sp] = parts;
    const style = sp.join(",").trim();
    const styleName = STYLES.find(s=>s.toLowerCase()===style.toLowerCase());
    if (!styleName) { errors.push(`Line ${i+1}: Unknown style "${style}"`); return; }
    const quantity = parseInt(qty);
    if (isNaN(quantity)||quantity<=0) { errors.push(`Line ${i+1}: Invalid quantity "${qty}"`); return; }
    const upper = item.toUpperCase();
    if (LETTERS.includes(upper)) { entries.push({type:"letters",key:upper,quantity,style:styleName}); return; }
    if (DIGITS.includes(upper))  { entries.push({type:"digits", key:upper,quantity,style:styleName}); return; }
    if (SIZES.includes(upper))   { entries.push({type:"sizes",  key:upper,quantity,style:styleName}); return; }
    errors.push(`Line ${i+1}: Unknown item "${item}"`);
  });
  return { entries, errors };
};

const computeNeeded = orders => {
  const needed = {};
  STYLES.forEach(s => { needed[s]={letters:{},digits:{},sizes:{}}; });
  orders.forEach(({name,number,size,style}) => {
    name.split("").forEach(ch => { if(LETTERS.includes(ch)) needed[style].letters[ch]=(needed[style].letters[ch]||0)+1; });
    String(number).split("").forEach(ch => { if(DIGITS.includes(ch)) needed[style].digits[ch]=(needed[style].digits[ch]||0)+1; });
    needed[style].sizes[size]=(needed[style].sizes[size]||0)+1;
  });
  return needed;
};

function App() {
  const [inventory,     setInventory]     = useState(loadInventory);
  const [threshold,     setThreshold]     = useState(loadThreshold);
  const [activeTab,     setActiveTab]     = useState("Home Padres");
  const [orderText,     setOrderText]     = useState("");
  const [preview,       setPreview]       = useState(null);
  const [orderErrors,   setOrderErrors]   = useState([]);
  const [restockText,   setRestockText]   = useState("");
  const [restockPreview,setRestockPreview]= useState(null);
  const [restockErrors, setRestockErrors] = useState([]);
  const [editCell,      setEditCell]      = useState(null);
  const [editVal,       setEditVal]       = useState("");
  const [toast,         setToast]         = useState(null);
  const [activeSection, setActiveSection] = useState("inventory");

  useEffect(()=>{ saveInventory(inventory); },[inventory]);
  useEffect(()=>{ saveThreshold(threshold); },[threshold]);

  const showToast = (msg,type="success") => { setToast({msg,type}); setTimeout(()=>setToast(null),3200); };

  const handleParseOrders = () => { const {orders,errors}=parseOrders(orderText); setOrderErrors(errors); setPreview(orders.length>0?orders:null); };
  const handleApplyOrders = () => {
    if (!preview) return;
    const needed = computeNeeded(preview);
    const newInv = JSON.parse(JSON.stringify(inventory));
    STYLES.forEach(style => {
      Object.entries(needed[style].letters).forEach(([k,v])=>newInv[style].letters[k]-=v);
      Object.entries(needed[style].digits).forEach(([k,v])=>newInv[style].digits[k]-=v);
      Object.entries(needed[style].sizes).forEach(([k,v])=>newInv[style].sizes[k]-=v);
    });
    setInventory(newInv); setPreview(null); setOrderText(""); setOrderErrors([]);
    showToast(`Applied ${preview.length} orders to inventory`);
  };

  const handleParseRestock = () => { const {entries,errors}=parseRestock(restockText); setRestockErrors(errors); setRestockPreview(entries.length>0?entries:null); };
  const handleApplyRestock = () => {
    if (!restockPreview) return;
    const newInv = JSON.parse(JSON.stringify(inventory));
    restockPreview.forEach(({type,key,quantity,style})=>{ newInv[style][type][key]=(newInv[style][type][key]||0)+quantity; });
    setInventory(newInv); setRestockPreview(null); setRestockText(""); setRestockErrors([]);
    showToast(`Restocked ${restockPreview.length} items successfully`);
  };

  const startEdit  = (style,type,key) => { setEditCell({style,type,key}); setEditVal(String(inventory[style][type][key])); };
  const commitEdit = () => {
    if (!editCell) return;
    const {style,type,key} = editCell;
    const val = parseInt(editVal);
    if (!isNaN(val)) { const n=JSON.parse(JSON.stringify(inventory)); n[style][type][key]=val; setInventory(n); }
    setEditCell(null);
  };

  const getCellClass = val => val<=0 ? "inv-cell cell-out" : val<=threshold ? "inv-cell cell-low" : "inv-cell cell-ok";

  const getLowItems = style => {
    const low=[];
    Object.entries(inventory[style].letters).forEach(([k,v])=>{ if(v<=threshold) low.push({label:`Letter ${k}`,val:v}); });
    Object.entries(inventory[style].digits).forEach(([k,v])=>{ if(v<=threshold) low.push({label:`# ${k}`,val:v}); });
    Object.entries(inventory[style].sizes).forEach(([k,v])=>{ if(v<=threshold) low.push({label:`Size ${k}`,val:v}); });
    return low;
  };
  const allLowItems = STYLES.flatMap(style=>getLowItems(style).map(i=>({...i,style})));
  const previewNeeded = preview ? computeNeeded(preview) : null;

  const renderCell = (style,type,key) => {
    const val = inventory[style][type][key];
    const isEditing = editCell?.style===style && editCell?.type===type && editCell?.key===key;
    return (
      <div key={key} className={getCellClass(val)} onClick={()=>startEdit(style,type,key)}>
        <div className="cell-key">{key}</div>
        {isEditing
          ? <input autoFocus value={editVal} onChange={e=>setEditVal(e.target.value)} onBlur={commitEdit} onKeyDown={e=>{ if(e.key==="Enter") commitEdit(); }} />
          : <div className="cell-val">{val}</div>}
      </div>
    );
  };

  return (
    <div className="app">
      <div className="header">
        <div className="header-inner">
          <div className="logo-area">
            <div className="logo-icon">⚾</div>
            <div>
              <div className="logo-title">Padres Jersey Manager</div>
              <div className="logo-sub">Inventory &amp; Order System</div>
            </div>
          </div>
          <div className="threshold-control">
            <span className="threshold-label">Low stock alert at</span>
            <input type="number" min={0} className="threshold-input" value={threshold} onChange={e=>setThreshold(parseInt(e.target.value)||0)} />
          </div>
        </div>
        <div className="nav">
          <div className="nav-inner">
            {[
              {id:"inventory",label:"Inventory"},
              {id:"orders",   label:"Orders"},
              {id:"restock",  label:"Restock"},
              {id:"reorder",  label:"Reorder", badge: allLowItems.length>0 ? allLowItems.length : null},
            ].map(({id,label,badge})=>(
              <button key={id} className={`nav-btn ${activeSection===id?"active":""}`} onClick={()=>setActiveSection(id)}>
                {label}{badge && <span className="nav-badge">{badge}</span>}
              </button>
            ))}
          </div>
        </div>
      </div>

      {toast && <div className={`toast ${toast.type==="success"?"toast-success":"toast-error"}`}>✓ {toast.msg}</div>}

      <div className="content">

        {activeSection==="inventory" && (
          <>
            <div className="style-tabs">
              {STYLES.map(style=>{
                const isHome=style==="Home Padres", cls=isHome?"home":"city", isActive=activeTab===style;
                return (
                  <button key={style} className={`style-tab ${cls} ${isActive?"active":""}`} onClick={()=>setActiveTab(style)}>
                    <span className={`style-tab-dot ${cls} ${isActive?"active":""}`}></span>{style}
                  </button>
                );
              })}
            </div>
            <div className="card"><div className="card-title">Letters A – Z</div><div className="cell-grid">{LETTERS.map(l=>renderCell(activeTab,"letters",l))}</div></div>
            <div className="card"><div className="card-title">Numbers 0 – 9</div><div className="cell-grid">{DIGITS.map(d=>renderCell(activeTab,"digits",d))}</div></div>
            <div className="card"><div className="card-title">Jersey Sizes</div><div className="cell-grid">{SIZES.map(s=>renderCell(activeTab,"sizes",s))}</div></div>
            <div className="legend">
              <div className="legend-item"><span className="legend-dot" style={{background:"rgba(255,255,255,0.2)"}}></span>In stock</div>
              <div className="legend-item"><span className="legend-dot" style={{background:"#FFC62F"}}></span>Low (≤ {threshold})</div>
              <div className="legend-item"><span className="legend-dot" style={{background:"#f87171"}}></span>Out of stock</div>
              <div className="legend-item" style={{marginLeft:"auto",fontSize:11,color:"rgba(255,255,255,0.25)"}}>Click any cell to edit</div>
            </div>
          </>
        )}

        {activeSection==="orders" && (
          <>
            <div className="section-label">Bulk Order Entry</div>
            <div className="section-sub">One order per line — <span className="format-tag">NAME, NUMBER, SIZE, STYLE</span></div>
            <div className="code-hint"><div className="hint-label">Example</div>SMITH, 23, XL, Home Padres<br/>JONES, 7, M, City Connect<br/>RODRIGUEZ, 44, XXL, Home Padres</div>
            <textarea className="styled-textarea" placeholder="Paste your order list here..." value={orderText} onChange={e=>setOrderText(e.target.value)} />
            <div className="btn-row">
              <button className="btn btn-primary" onClick={handleParseOrders}>Preview Orders</button>
              {preview && <button className="btn btn-success" onClick={handleApplyOrders}>Apply {preview.length} Orders</button>}
              {(preview||orderText) && <button className="btn btn-ghost" onClick={()=>{setPreview(null);setOrderText("");setOrderErrors([]);}}>Clear</button>}
            </div>
            {orderErrors.length>0 && <div className="error-box"><div className="error-title">⚠ Errors found</div>{orderErrors.map((e,i)=><div key={i} className="error-line">{e}</div>)}</div>}
            {preview && previewNeeded && (
              <>
                <div className="preview-header">Preview <span className="preview-count">{preview.length} jerseys</span></div>
                <div className="table-wrap">
                  <table className="data-table">
                    <thead><tr><th>Name</th><th>Number</th><th>Size</th><th>Style</th></tr></thead>
                    <tbody>{preview.map((o,i)=>(
                      <tr key={i} className={i%2===0?"table-bg-even":""}>
                        <td className="td-name">{o.name}</td><td className="td-num">{o.number}</td>
                        <td className="td-size">{o.size}</td><td className="td-style">{o.style==="Home Padres"?"🟤":"🟡"} {o.style}</td>
                      </tr>
                    ))}</tbody>
                  </table>
                </div>
                <div className="deduct-grid">
                  {STYLES.map(style=>{
                    const nd=previewNeeded[style];
                    const hasAny=Object.values(nd.letters).some(v=>v>0)||Object.values(nd.digits).some(v=>v>0)||Object.values(nd.sizes).some(v=>v>0);
                    if (!hasAny) return null;
                    return (
                      <div key={style} className="deduct-card">
                        <div className="deduct-card-title">{style==="Home Padres"?"🟤":"🟡"} {style}</div>
                        {Object.entries(nd.letters).filter(([,v])=>v>0).length>0 && <div className="deduct-line">Letters: {Object.entries(nd.letters).filter(([,v])=>v>0).map(([k,v])=>`${k}×${v}`).join(" · ")}</div>}
                        {Object.entries(nd.digits).filter(([,v])=>v>0).length>0  && <div className="deduct-line">Numbers: {Object.entries(nd.digits).filter(([,v])=>v>0).map(([k,v])=>`${k}×${v}`).join(" · ")}</div>}
                        {Object.entries(nd.sizes).filter(([,v])=>v>0).length>0   && <div className="deduct-line">Sizes: {Object.entries(nd.sizes).filter(([,v])=>v>0).map(([k,v])=>`${k}×${v}`).join(" · ")}</div>}
                      </div>
                    );
                  })}
                </div>
              </>
            )}
          </>
        )}

        {activeSection==="restock" && (
          <>
            <div className="section-label">Restock Inventory</div>
            <div className="section-sub">One item per line — <span className="format-tag">ITEM, QUANTITY, STYLE</span> — added on top of existing stock</div>
            <div className="code-hint"><div className="hint-label">Example</div>A, 100, Home Padres<br/>3, 50, City Connect<br/>XL, 30, Home Padres<br/>XXXL, 10, City Connect</div>
            <textarea className="styled-textarea" placeholder="Paste your restock list here..." value={restockText} onChange={e=>setRestockText(e.target.value)} />
            <div className="btn-row">
              <button className="btn btn-primary" onClick={handleParseRestock}>Preview Restock</button>
              {restockPreview && <button className="btn btn-success" onClick={handleApplyRestock}>Apply Restock ({restockPreview.length})</button>}
              {(restockPreview||restockText) && <button className="btn btn-ghost" onClick={()=>{setRestockPreview(null);setRestockText("");setRestockErrors([]);}}>Clear</button>}
            </div>
            {restockErrors.length>0 && <div className="error-box"><div className="error-title">⚠ Errors found</div>{restockErrors.map((e,i)=><div key={i} className="error-line">{e}</div>)}</div>}
            {restockPreview && (
              <>
                <div className="preview-header">Preview <span className="preview-count">{restockPreview.length} items</span></div>
                <div className="table-wrap">
                  <table className="data-table">
                    <thead><tr><th>Item</th><th>Current</th><th>Adding</th><th>New Total</th><th>Style</th></tr></thead>
                    <tbody>{restockPreview.map((e,i)=>{
                      const current=inventory[e.style][e.type][e.key]||0;
                      return (
                        <tr key={i} className={i%2===0?"table-bg-even":""}>
                          <td className="td-item">{e.key}</td><td className="td-current">{current}</td>
                          <td className="td-add">+{e.quantity}</td><td className="td-total">{current+e.quantity}</td>
                          <td className="td-style">{e.style==="Home Padres"?"🟤":"🟡"} {e.style}</td>
                        </tr>
                      );
                    })}</tbody>
                  </table>
                </div>
              </>
            )}
          </>
        )}

        {activeSection==="reorder" && (
          <>
            <div className="section-label">Needs Reorder</div>
            <div className="section-sub">Items at or below your threshold of <span className="format-tag">{threshold}</span></div>
            {allLowItems.length===0
              ? <div className="card reorder-empty"><div className="reorder-empty-icon">✓</div><div className="reorder-empty-text">All items are well stocked!</div></div>
              : STYLES.map(style=>{
                  const low=getLowItems(style);
                  if (low.length===0) return null;
                  return (
                    <div key={style} className="reorder-section">
                      <div className="reorder-section-title">{style==="Home Padres"?"🟤":"🟡"} {style}</div>
                      <div className="reorder-chips">
                        {low.map((item,i)=>(
                          <div key={i} className={`reorder-chip ${item.val<=0?"chip-out":"chip-low"}`}>
                            {item.label} — <strong>{item.val<=0?"OUT":item.val}</strong>
                          </div>
                        ))}
                      </div>
                    </div>
                  );
                })
            }
          </>
        )}
      </div>
    </div>
  );
}

ReactDOM.createRoot(document.getElementById("root")).render(<App />);
</script>
</body>
</html>
