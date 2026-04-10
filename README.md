<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=1024">
<title>PilotReflect EFB</title>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500;600&family=IBM+Plex+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
:root {
  --bg:      #F5F7FA;
  --s1:      #FFFFFF;
  --s2:      #EEF1F6;
  --s3:      #E3E8F0;
  --border:  #C8D0DC;
  --border2: #A8B4C4;
  --text:    #111827;
  --muted:   #4B5563;
  --dim:     #9CA3AF;
  /* Avionics accent — muted but print-safe */
  --navy:    #1D3557;
  --blue:    #1A6FB5;
  --blue-bg: #EAF2FB;
  --blue-bd: #A8CCEA;
  --green:   #1A7A3F;
  --green-bg:#E6F5EC;
  --green-bd:#8FD0AA;
  --amber:   #92600A;
  --amber-bg:#FDF3DC;
  --amber-bd:#E0B96A;
  --red:     #B91C1C;
  --red-bg:  #FEE8E8;
  --red-bd:  #F4A0A0;
  --teal:    #0D7077;
  --teal-bg: #E0F4F5;
  --teal-bd: #7FD0D6;
  --mono:    'IBM Plex Mono', monospace;
  --sans:    'IBM Plex Sans', sans-serif;
}
*{box-sizing:border-box;margin:0;padding:0;}
body{font-family:var(--sans);background:var(--bg);color:var(--text);width:1024px;height:768px;overflow:hidden;}

/* STATUS BAR */
.statusbar{
  height:26px;background:var(--navy);
  display:flex;align-items:center;justify-content:space-between;
  padding:0 16px;font-family:var(--mono);font-size:10px;color:#C8D8EC;
  letter-spacing:0.3px;
}
.sb-left{display:flex;gap:16px;align-items:center;}
.sb-right{display:flex;gap:16px;align-items:center;}
.sb-sep{width:1px;height:12px;background:#3D5A7A;}
.sb-green{color:#6EE7A0;}
.sb-amber{color:#FCD34D;}

/* HEADER */
.header{
  height:46px;background:var(--s1);
  border-bottom:3px solid var(--navy);
  display:flex;align-items:center;padding:0 18px;gap:14px;
}
.efb-logo{display:flex;align-items:center;gap:8px;padding-right:14px;border-right:1px solid var(--border);}
.efb-badge{
  background:var(--navy);color:#fff;font-family:var(--mono);
  font-size:10px;font-weight:600;padding:2px 7px;border-radius:2px;letter-spacing:1.5px;
}
.efb-name{font-family:var(--mono);font-size:13px;font-weight:600;color:var(--navy);letter-spacing:0.3px;}
.efb-name span{color:var(--blue);}
.htabs{display:flex;gap:1px;flex:1;}
.htab{
  font-size:11px;font-family:var(--mono);font-weight:500;letter-spacing:0.4px;
  padding:6px 14px;border-radius:3px 3px 0 0;cursor:pointer;
  color:var(--muted);border:1px solid transparent;border-bottom:none;
  transition:all .12s;
}
.htab:hover{color:var(--navy);background:var(--s2);}
.htab.active{
  background:var(--bg);color:var(--navy);font-weight:600;
  border-color:var(--border);border-bottom-color:var(--bg);
  margin-bottom:-3px;z-index:2;position:relative;
}
.header-right{display:flex;align-items:center;gap:10px;margin-left:auto;}
.priv-badge{
  font-family:var(--mono);font-size:9px;letter-spacing:0.6px;
  background:var(--green-bg);border:1px solid var(--green-bd);
  color:var(--green);padding:3px 8px;border-radius:2px;font-weight:500;
}
.utc{font-family:var(--mono);font-size:11px;color:var(--navy);font-weight:600;}

/* LAYOUT */
.layout{display:flex;height:calc(768px - 26px - 46px);}

/* SIDEBAR */
.sidebar{
  width:196px;background:var(--s1);border-right:1px solid var(--border);
  display:flex;flex-direction:column;flex-shrink:0;
}
.sid-label{
  padding:10px 10px 4px;
  font-family:var(--mono);font-size:9px;color:var(--dim);
  letter-spacing:1.2px;font-weight:600;
}
.sitem{
  display:flex;align-items:center;gap:8px;
  padding:7px 10px;margin:1px 5px;border-radius:3px;
  font-size:11px;font-family:var(--mono);color:var(--muted);cursor:pointer;
  border:1px solid transparent;transition:all .1s;
}
.sitem:hover{background:var(--s2);color:var(--navy);}
.sitem.active{
  background:var(--blue-bg);color:var(--blue);
  border-color:var(--blue-bd);font-weight:600;
}
.sitem svg{width:13px;height:13px;flex-shrink:0;stroke-width:2;}
.sbadge{
  margin-left:auto;font-family:var(--mono);font-size:9px;font-weight:600;
  background:var(--amber-bg);color:var(--amber);
  padding:1px 5px;border-radius:2px;border:1px solid var(--amber-bd);
}
.sid-foot{
  margin-top:auto;padding:10px;border-top:1px solid var(--border);
  font-family:var(--mono);font-size:9px;color:var(--dim);line-height:1.8;
}
.sid-foot strong{color:var(--muted);}

/* MAIN */
.main{flex:1;overflow-y:auto;overflow-x:hidden;padding:16px;background:var(--bg);}

/* SECTION HEADER */
.shead{display:flex;align-items:flex-start;justify-content:space-between;margin-bottom:14px;}
.stitle{font-family:var(--mono);font-size:14px;font-weight:600;color:var(--navy);}
.ssub{font-size:11px;color:var(--muted);margin-top:3px;font-family:var(--mono);}
.btnsm{
  font-family:var(--mono);font-size:10px;letter-spacing:0.4px;
  background:var(--s1);border:1px solid var(--border2);color:var(--muted);
  padding:5px 12px;border-radius:3px;cursor:pointer;
  display:flex;align-items:center;gap:6px;font-weight:500;
}
.btnsm:hover{color:var(--navy);border-color:var(--navy);}

/* GRIDS */
.g3{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:12px;}
.g2{display:grid;grid-template-columns:repeat(2,1fr);gap:10px;margin-bottom:12px;}

/* PANEL */
.panel{background:var(--s1);border:1px solid var(--border);border-radius:4px;padding:14px;}
.panel-t{
  font-family:var(--mono);font-size:9px;letter-spacing:1px;color:var(--dim);
  margin-bottom:10px;padding-bottom:8px;border-bottom:1px solid var(--border);
  display:flex;align-items:center;justify-content:space-between;font-weight:600;
}

/* STAT */
.stat{background:var(--s1);border:1px solid var(--border);border-radius:3px;padding:12px;}
.slbl{font-family:var(--mono);font-size:9px;color:var(--dim);letter-spacing:0.6px;margin-bottom:6px;font-weight:600;}
.sval{font-family:var(--mono);font-size:24px;font-weight:300;letter-spacing:-0.5px;}
.ssub2{font-family:var(--mono);font-size:9px;color:var(--muted);margin-top:4px;}

/* SCORE RING */
.ring-wrap{display:flex;align-items:center;gap:18px;margin-bottom:12px;}
.ring-svg-w{position:relative;width:88px;height:88px;flex-shrink:0;}
.ring-svg-w svg{width:88px;height:88px;transform:rotate(-90deg);}
.ring-ctr{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);text-align:center;}
.ring-num{font-family:var(--mono);font-size:26px;font-weight:600;color:var(--navy);}
.ring-den{font-family:var(--mono);font-size:9px;color:var(--dim);}
.score-rows{flex:1;}
.srow{display:flex;align-items:center;gap:8px;margin-bottom:7px;}
.srow-lbl{font-family:var(--mono);font-size:10px;color:var(--muted);width:108px;flex-shrink:0;}
.srow-bg{flex:1;height:5px;background:var(--s3);border-radius:3px;overflow:hidden;}
.srow-fill{height:100%;border-radius:3px;}
.srow-val{font-family:var(--mono);font-size:10px;font-weight:600;min-width:24px;text-align:right;}

/* ALERTS */
.alert{
  display:flex;gap:10px;align-items:flex-start;
  padding:8px 10px;border-radius:3px;margin-bottom:6px;
  border-left:3px solid;
}
.adot{width:6px;height:6px;border-radius:50%;flex-shrink:0;margin-top:4px;}
.atext{font-size:11px;line-height:1.5;font-family:var(--mono);font-weight:500;}
.ameta{font-size:9px;color:var(--muted);margin-top:2px;font-family:var(--mono);}

/* BIO ROWS */
.brow{display:flex;align-items:center;gap:12px;padding:9px 0;border-bottom:1px solid var(--border);}
.brow:last-child{border-bottom:none;}
.bicon{width:30px;height:30px;border-radius:3px;display:flex;align-items:center;justify-content:center;flex-shrink:0;}
.bicon svg{width:14px;height:14px;}
.bname{font-family:var(--mono);font-size:11px;font-weight:600;color:var(--text);}
.bdet{font-family:var(--mono);font-size:9px;color:var(--muted);margin-top:2px;}
.bbadge{font-family:var(--mono);font-size:9px;font-weight:700;padding:2px 7px;border-radius:2px;border:1px solid;white-space:nowrap;}

/* BAR CHART */
.bbar-row{display:flex;align-items:center;gap:8px;margin-bottom:8px;}
.bbar-lbl{font-family:var(--mono);font-size:10px;color:var(--muted);width:72px;flex-shrink:0;font-weight:500;}
.bbar-bg{flex:1;height:7px;background:var(--s3);border-radius:4px;overflow:hidden;border:1px solid var(--border);}
.bbar-fill{height:100%;border-radius:3px;}
.bbar-val{font-family:var(--mono);font-size:10px;font-weight:700;min-width:24px;text-align:right;}

/* SPARK */
.sparkwrap{display:flex;align-items:flex-end;gap:2px;height:50px;}
.spk{border-radius:1px 1px 0 0;flex:1;min-width:5px;}
.spk-labels{display:flex;gap:2px;margin-top:4px;}
.spk-lbl{flex:1;font-family:var(--mono);font-size:8px;color:var(--dim);text-align:center;}

/* PHASE TIMELINE */
.ptl{padding-left:14px;}
.pitem{position:relative;padding:7px 0 7px 14px;border-left:2px solid var(--border);}
.pitem.hi{border-left-color:var(--navy);}
.pdot{position:absolute;left:-5px;top:13px;width:8px;height:8px;border-radius:50%;background:var(--border);border:2px solid var(--border);}
.pitem.hi .pdot{background:var(--navy);border-color:var(--navy);}
.pname{font-family:var(--mono);font-size:11px;font-weight:600;color:var(--text);}
.ptime{font-family:var(--mono);font-size:9px;color:var(--muted);margin-top:2px;}
.pbar-bg{margin-top:5px;height:5px;background:var(--s3);border-radius:3px;overflow:hidden;border:1px solid var(--border);}
.pbar-fill{height:100%;border-radius:2px;}

/* INSIGHT */
.insight{
  background:var(--blue-bg);border:1px solid var(--blue-bd);
  border-left:3px solid var(--blue);border-radius:3px;padding:10px;margin-bottom:8px;
}
.insight-tag{font-family:var(--mono);font-size:9px;color:var(--blue);letter-spacing:0.8px;margin-bottom:5px;font-weight:700;}
.insight-txt{font-family:var(--mono);font-size:10px;color:var(--muted);line-height:1.8;}
.insight-txt span{color:var(--amber);font-weight:700;}

/* REFLECT */
.rq{margin-bottom:12px;}
.rq label{font-family:var(--mono);font-size:10px;font-weight:600;color:var(--navy);display:block;margin-bottom:5px;letter-spacing:0.3px;}
.rq textarea{
  width:100%;background:var(--s2);border:1px solid var(--border);border-radius:3px;
  padding:8px 10px;font-size:11px;font-family:var(--mono);color:var(--text);resize:none;
}
.rq textarea:focus{outline:none;border-color:var(--navy);}

/* HISTORY */
.hrow{display:flex;align-items:center;gap:12px;padding:8px 0;border-bottom:1px solid var(--border);}
.hrow:last-child{border-bottom:none;}
.hdate{font-family:var(--mono);font-size:10px;color:var(--dim);width:52px;font-weight:600;}
.hroute{font-family:var(--mono);font-size:11px;font-weight:600;color:var(--text);}
.hinfo{font-family:var(--mono);font-size:9px;color:var(--dim);margin-top:2px;}
.hscore{font-family:var(--mono);font-size:11px;font-weight:700;margin-left:auto;}

/* SECTION SWITCH */
.section{display:none;}
.section.active{display:block;}
</style>
</head>
<body>

<!-- STATUS BAR -->
<div class="statusbar">
  <div class="sb-left">
    <span class="sb-green">● EFB TYPE B — CLASS 1 PORTABLE</span>
    <div class="sb-sep"></div>
    <span>ICAO DOC 10020 COMPLIANT</span>
    <div class="sb-sep"></div>
    <span>FAA AC 120-76E / EASA AMC 20-25</span>
  </div>
  <div class="sb-right">
    <span>PILOT: CAP. M. CANTEKİN</span>
    <div class="sb-sep"></div>
    <span>LTBA/IST</span>
    <div class="sb-sep"></div>
    <span class="sb-amber">◆ POST-FLIGHT MODE</span>
  </div>
</div>

<!-- HEADER -->
<div class="header">
  <div class="efb-logo">
    <div class="efb-badge">EFB</div>
    <div class="efb-name">Pilot<span>Reflect</span></div>
  </div>
  <div class="htabs">
    <div class="htab active" onclick="htab('dashboard',this)">FLIGHT LOG</div>
    <div class="htab" onclick="htab('biometrics',this)">BIOMETRICS</div>
    <div class="htab" onclick="htab('phases',this)">PHASE ANALYSIS</div>
    <div class="htab" onclick="htab('reflect',this)">DEBRIEFING</div>
    <div class="htab" onclick="htab('history',this)">HISTORY</div>
  </div>
  <div class="header-right">
    <div class="priv-badge">● PRIVATE · LOCAL ONLY</div>
    <div class="utc" id="clk">02 APR 2026 · 13:24:00Z</div>
  </div>
</div>

<!-- LAYOUT -->
<div class="layout">

  <!-- SIDEBAR -->
  <div class="sidebar">
    <div class="sid-label">NAVIGATION</div>
    <div class="sitem active" onclick="nav('dashboard',this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><rect x="3" y="3" width="7" height="7"/><rect x="14" y="3" width="7" height="7"/><rect x="14" y="14" width="7" height="7"/><rect x="3" y="14" width="7" height="7"/></svg>
      FLIGHT SUMMARY
    </div>
    <div class="sid-label">ANALYTICS</div>
    <div class="sitem" onclick="nav('biometrics',this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><polyline points="22 12 18 12 15 21 9 3 6 12 2 12"/></svg>
      BIOMETRICS
      <span class="sbadge">WARN</span>
    </div>
    <div class="sitem" onclick="nav('phases',this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><line x1="3" y1="6" x2="21" y2="6"/><line x1="3" y1="12" x2="21" y2="12"/><line x1="3" y1="18" x2="21" y2="18"/></svg>
      PHASE ANALYSIS
    </div>
    <div class="sitem" onclick="nav('reflect',this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><path d="M12 20h9"/><path d="M16.5 3.5a2.121 2.121 0 013 3L7 19l-4 1 1-4z"/></svg>
      DEBRIEFING
    </div>
    <div class="sid-label">RECORDS</div>
    <div class="sitem" onclick="nav('history',this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>
      FLIGHT HISTORY
    </div>
    <div class="sid-foot">
      <strong>DATA POLICY</strong><br>
      All biometric &amp; reflective<br>
      data stored locally only.<br>
      Zero external transmission.<br>
      AES-256 encrypted.
    </div>
  </div>

  <!-- MAIN -->
  <div class="main">

    <!-- FLIGHT LOG -->
    <div id="dashboard" class="section active">
      <div class="shead">
        <div>
          <div class="stitle">POST-FLIGHT SUMMARY</div>
          <div class="ssub">02 APR 2026 · LTBA/IST → LTBJ/ESB · B737-800 · TC-JFD</div>
        </div>
        <button class="btnsm">
          <svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 15v4a2 2 0 01-2 2H5a2 2 0 01-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" y1="15" x2="12" y2="3"/></svg>
          EXPORT REPORT
        </button>
      </div>

      <div class="g3">
        <div class="stat" style="border-top:3px solid var(--blue)">
          <div class="slbl">BLOCK TIME</div>
          <div class="sval" style="color:var(--navy)">01:47</div>
          <div class="ssub2">OFF: 12:44Z · ON: 14:31Z</div>
        </div>
        <div class="stat" style="border-top:3px solid var(--green)">
          <div class="slbl">MAX ALTITUDE</div>
          <div class="sval" style="color:var(--green)">FL220</div>
          <div class="ssub2">22,000 FT MSL</div>
        </div>
        <div class="stat" style="border-top:3px solid var(--green)">
          <div class="slbl">AVG SpO₂</div>
          <div class="sval" style="color:var(--green)">95%</div>
          <div class="ssub2">MIN: 91% · MAX: 98%</div>
        </div>
      </div>

      <div class="g2">
        <div class="panel">
          <div class="panel-t">
            <span>PERSONAL SAFETY SCORE</span>
            <span style="font-size:9px;color:var(--dim);">NOT SHARED</span>
          </div>
          <div class="ring-wrap">
            <div class="ring-svg-w">
              <svg viewBox="0 0 88 88">
                <circle cx="44" cy="44" r="36" fill="none" stroke="#E3E8F0" stroke-width="7"/>
                <circle cx="44" cy="44" r="36" fill="none" stroke="#1A6FB5" stroke-width="7"
                  stroke-dasharray="226.2" stroke-dashoffset="40.7" stroke-linecap="butt"/>
              </svg>
              <div class="ring-ctr">
                <div class="ring-num">82</div>
                <div class="ring-den">/100</div>
              </div>
            </div>
            <div class="score-rows">
              <div class="srow"><span class="srow-lbl">SpO₂ MGMT</span><div class="srow-bg"><div class="srow-fill" style="width:91%;background:#1A7A3F"></div></div><span class="srow-val" style="color:#1A7A3F">91</span></div>
              <div class="srow"><span class="srow-lbl">STRESS INDEX</span><div class="srow-bg"><div class="srow-fill" style="width:74%;background:#1A6FB5"></div></div><span class="srow-val" style="color:#1A6FB5">74</span></div>
              <div class="srow"><span class="srow-lbl">FATIGUE LVL</span><div class="srow-bg"><div class="srow-fill" style="width:80%;background:#1A7A3F"></div></div><span class="srow-val" style="color:#1A7A3F">80</span></div>
              <div class="srow"><span class="srow-lbl">DECISION QTY</span><div class="srow-bg"><div class="srow-fill" style="width:68%;background:#92600A"></div></div><span class="srow-val" style="color:#92600A">68</span></div>
              <div class="srow"><span class="srow-lbl">SOP ADHERENCE</span><div class="srow-bg"><div class="srow-fill" style="width:88%;background:#1A7A3F"></div></div><span class="srow-val" style="color:#1A7A3F">88</span></div>
            </div>
          </div>
        </div>

        <div class="panel">
          <div class="panel-t"><span>BIOMETRIC EVENTS LOG</span></div>
          <div class="alert" style="background:var(--amber-bg);border-left-color:var(--amber)">
            <div class="adot" style="background:var(--amber)"></div>
            <div>
              <div class="atext" style="color:var(--amber)">SpO₂ DROP TO 91% — PRE-LANDING</div>
              <div class="ameta">14:32Z · DESCENT · BREATHING EXERCISE SUGGESTED</div>
            </div>
          </div>
          <div class="alert" style="background:var(--amber-bg);border-left-color:var(--amber)">
            <div class="adot" style="background:var(--amber)"></div>
            <div>
              <div class="atext" style="color:var(--amber)">HR SPIKE 108+ BPM FOR 3+ MINUTES</div>
              <div class="ameta">14:18Z · APPROACH · HIGH WORKLOAD DETECTED</div>
            </div>
          </div>
          <div class="alert" style="background:var(--green-bg);border-left-color:var(--green)">
            <div class="adot" style="background:var(--green)"></div>
            <div>
              <div class="atext" style="color:var(--green)">SpO₂ STABLE 95–98% THROUGHOUT CRUISE</div>
              <div class="ameta">12:45–14:10Z · CRUISE PHASE · NOMINAL</div>
            </div>
          </div>
          <div class="alert" style="background:var(--blue-bg);border-left-color:var(--blue)">
            <div class="adot" style="background:var(--blue)"></div>
            <div>
              <div class="atext" style="color:var(--blue)">HRV 42ms — 8% BELOW PERSONAL BASELINE</div>
              <div class="ameta">PRE-FLIGHT CHECK · MONITOR TREND</div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- BIOMETRICS -->
    <div id="biometrics" class="section">
      <div class="shead">
        <div>
          <div class="stitle">BIOMETRIC VISUALIZATION</div>
          <div class="ssub">FLIGHT PHYSIOLOGICAL PARAMETER ANALYSIS · 02 APR 2026</div>
        </div>
      </div>

      <div class="g3" style="margin-bottom:10px;">
        <div class="stat" style="border-top:3px solid var(--green)">
          <div class="slbl">AVG HEART RATE</div>
          <div class="sval" style="color:var(--green)">78 <span style="font-size:13px;color:var(--dim)">BPM</span></div>
          <div class="ssub2">MAX 112 BPM (APPROACH)</div>
        </div>
        <div class="stat" style="border-top:3px solid var(--blue)">
          <div class="slbl">AVG SpO₂</div>
          <div class="sval" style="color:var(--blue)">95 <span style="font-size:13px;color:var(--dim)">%</span></div>
          <div class="ssub2">MIN 91% (DESCENT)</div>
        </div>
        <div class="stat" style="border-top:3px solid var(--amber)">
          <div class="slbl">MEAN HRV</div>
          <div class="sval" style="color:var(--amber)">42 <span style="font-size:13px;color:var(--dim)">MS</span></div>
          <div class="ssub2">−8% BELOW BASELINE</div>
        </div>
      </div>

      <div class="g2">
        <div class="panel">
          <div class="panel-t"><span>SENSOR PARAMETERS</span></div>
          <div class="brow">
            <div class="bicon" style="background:var(--green-bg)">
              <svg viewBox="0 0 24 24" fill="none" stroke="#1A7A3F" stroke-width="2"><polyline points="22 12 18 12 15 21 9 3 6 12 2 12"/></svg>
            </div>
            <div style="flex:1">
              <div class="bname">HEART RATE (HR)</div>
              <div class="bdet">AVG 78 BPM · MAX 112 BPM (APPROACH)</div>
            </div>
            <div class="bbadge" style="background:var(--green-bg);color:var(--green);border-color:var(--green-bd)">NORMAL</div>
          </div>
          <div class="brow">
            <div class="bicon" style="background:var(--blue-bg)">
              <svg viewBox="0 0 24 24" fill="none" stroke="#1A6FB5" stroke-width="2"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg>
            </div>
            <div style="flex:1">
              <div class="bname">SpO₂ — OXYGEN SATURATION</div>
              <div class="bdet">AVG 95% · MIN 91% (DESCENT PHASE)</div>
            </div>
            <div class="bbadge" style="background:var(--blue-bg);color:var(--blue);border-color:var(--blue-bd)">GOOD</div>
          </div>
          <div class="brow">
            <div class="bicon" style="background:var(--amber-bg)">
              <svg viewBox="0 0 24 24" fill="none" stroke="#92600A" stroke-width="2"><path d="M22 12h-4l-3 9L9 3l-3 9H2"/></svg>
            </div>
            <div style="flex:1">
              <div class="bname">HRV — HEART RATE VARIABILITY</div>
              <div class="bdet">MEAN 42MS · 8% BELOW PERSONAL NORM</div>
            </div>
            <div class="bbadge" style="background:var(--amber-bg);color:var(--amber);border-color:var(--amber-bd)">CAUTION</div>
          </div>
          <div class="brow">
            <div class="bicon" style="background:var(--teal-bg)">
              <svg viewBox="0 0 24 24" fill="none" stroke="#0D7077" stroke-width="2"><path d="M18 8h1a4 4 0 010 8h-1"/><path d="M2 8h16v9a4 4 0 01-4 4H6a4 4 0 01-4-4V8z"/><line x1="6" y1="1" x2="6" y2="4"/><line x1="10" y1="1" x2="10" y2="4"/><line x1="14" y1="1" x2="14" y2="4"/></svg>
            </div>
            <div style="flex:1">
              <div class="bname">PRE-FLIGHT SLEEP QUALITY</div>
              <div class="bdet">6.8 HRS · REM RATIO 18%</div>
            </div>
            <div class="bbadge" style="background:var(--amber-bg);color:var(--amber);border-color:var(--amber-bd)">MODERATE</div>
          </div>
          <div class="brow">
            <div class="bicon" style="background:var(--red-bg)">
              <svg viewBox="0 0 24 24" fill="none" stroke="#B91C1C" stroke-width="2"><polygon points="7.86 2 16.14 2 22 7.86 22 16.14 16.14 22 7.86 22 2 16.14 2 7.86 7.86 2"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg>
            </div>
            <div style="flex:1">
              <div class="bname">STRESS INDEX</div>
              <div class="bdet">LOW CRUISE · HIGH APPROACH (81/100)</div>
            </div>
            <div class="bbadge" style="background:var(--blue-bg);color:var(--blue);border-color:var(--blue-bd)">VARIABLE</div>
          </div>
        </div>

        <div class="panel">
          <div class="panel-t"><span>PHASE-BASED STRESS PROFILE</span></div>
          <div style="margin-bottom:14px;">
            <div class="bbar-row"><span class="bbar-lbl">TAKEOFF</span><div class="bbar-bg"><div class="bbar-fill" style="width:55%;background:#1A6FB5"></div></div><span class="bbar-val" style="color:#1A6FB5">55</span></div>
            <div class="bbar-row"><span class="bbar-lbl">CLIMB</span><div class="bbar-bg"><div class="bbar-fill" style="width:42%;background:#1A7A3F"></div></div><span class="bbar-val" style="color:#1A7A3F">42</span></div>
            <div class="bbar-row"><span class="bbar-lbl">CRUISE</span><div class="bbar-bg"><div class="bbar-fill" style="width:28%;background:#1A7A3F"></div></div><span class="bbar-val" style="color:#1A7A3F">28</span></div>
            <div class="bbar-row"><span class="bbar-lbl">DESCENT</span><div class="bbar-bg"><div class="bbar-fill" style="width:62%;background:#92600A"></div></div><span class="bbar-val" style="color:#92600A">62</span></div>
            <div class="bbar-row"><span class="bbar-lbl">APPROACH</span><div class="bbar-bg"><div class="bbar-fill" style="width:81%;background:#B91C1C"></div></div><span class="bbar-val" style="color:#B91C1C">81</span></div>
            <div class="bbar-row"><span class="bbar-lbl">LANDING</span><div class="bbar-bg"><div class="bbar-fill" style="width:49%;background:#1A6FB5"></div></div><span class="bbar-val" style="color:#1A6FB5">49</span></div>
          </div>
          <div style="border-top:1px solid var(--border);padding-top:10px;">
            <div class="panel-t" style="border:none;padding:0;margin:0 0 8px;">HR CHRONOLOGY — FLIGHT PHASES</div>
            <div class="sparkwrap">
              <div class="spk" style="height:55%;background:#1A6FB566;border:1px solid #1A6FB5aa"></div>
              <div class="spk" style="height:62%;background:#1A6FB566;border:1px solid #1A6FB5aa"></div>
              <div class="spk" style="height:57%;background:#1A6FB566;border:1px solid #1A6FB5aa"></div>
              <div class="spk" style="height:46%;background:#1A7A3F66;border:1px solid #1A7A3Faa"></div>
              <div class="spk" style="height:43%;background:#1A7A3F66;border:1px solid #1A7A3Faa"></div>
              <div class="spk" style="height:44%;background:#1A7A3F66;border:1px solid #1A7A3Faa"></div>
              <div class="spk" style="height:35%;background:#1A7A3F66;border:1px solid #1A7A3Faa"></div>
              <div class="spk" style="height:32%;background:#1A7A3F66;border:1px solid #1A7A3Faa"></div>
              <div class="spk" style="height:30%;background:#1A7A3F66;border:1px solid #1A7A3Faa"></div>
              <div class="spk" style="height:33%;background:#1A7A3F66;border:1px solid #1A7A3Faa"></div>
              <div class="spk" style="height:31%;background:#1A7A3F66;border:1px solid #1A7A3Faa"></div>
              <div class="spk" style="height:34%;background:#1A7A3F66;border:1px solid #1A7A3Faa"></div>
              <div class="spk" style="height:52%;background:#92600A66;border:1px solid #92600Aaa"></div>
              <div class="spk" style="height:58%;background:#92600A66;border:1px solid #92600Aaa"></div>
              <div class="spk" style="height:63%;background:#92600A66;border:1px solid #92600Aaa"></div>
              <div class="spk" style="height:61%;background:#92600A66;border:1px solid #92600Aaa"></div>
              <div class="spk" style="height:78%;background:#B91C1C88;border:1px solid #B91C1Ccc"></div>
              <div class="spk" style="height:92%;background:#B91C1CCC;border:1px solid #B91C1C"></div>
              <div class="spk" style="height:96%;background:#B91C1CCC;border:1px solid #B91C1C"></div>
              <div class="spk" style="height:88%;background:#B91C1C88;border:1px solid #B91C1Ccc"></div>
              <div class="spk" style="height:60%;background:#1A6FB566;border:1px solid #1A6FB5aa"></div>
              <div class="spk" style="height:52%;background:#1A6FB566;border:1px solid #1A6FB5aa"></div>
              <div class="spk" style="height:47%;background:#1A6FB566;border:1px solid #1A6FB5aa"></div>
            </div>
            <div class="spk-labels">
              <div class="spk-lbl" style="flex:3">T/O</div>
              <div class="spk-lbl" style="flex:3">CLB</div>
              <div class="spk-lbl" style="flex:6">CRZ</div>
              <div class="spk-lbl" style="flex:4">DES</div>
              <div class="spk-lbl" style="flex:4">APP</div>
              <div class="spk-lbl" style="flex:3">LDG</div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- PHASE ANALYSIS -->
    <div id="phases" class="section">
      <div class="shead">
        <div>
          <div class="stitle">PHASE ANALYSIS</div>
          <div class="ssub">FLIGHT SEGMENT PERFORMANCE · LTBA → LTBJ · 02 APR 2026</div>
        </div>
      </div>
      <div class="g2">
        <div class="panel">
          <div class="panel-t"><span>FLIGHT PHASE TIMELINE</span></div>
          <div class="ptl">
            <div class="pitem hi">
              <div class="pdot"></div>
              <div class="pname">TAKEOFF</div>
              <div class="ptime">12:44Z · 8 MIN · RWY 17R LTBA · STRESS INDEX: 55</div>
              <div class="pbar-bg"><div class="pbar-fill" style="width:55%;background:#1A6FB5"></div></div>
            </div>
            <div class="pitem hi">
              <div class="pdot"></div>
              <div class="pname">CLIMB</div>
              <div class="ptime">12:52Z · 18 MIN · FL000→FL220 · STRESS INDEX: 42</div>
              <div class="pbar-bg"><div class="pbar-fill" style="width:42%;background:#1A7A3F"></div></div>
            </div>
            <div class="pitem hi">
              <div class="pdot"></div>
              <div class="pname">CRUISE</div>
              <div class="ptime">13:10Z · 62 MIN · FL220 · STRESS INDEX: 28</div>
              <div class="pbar-bg"><div class="pbar-fill" style="width:28%;background:#1A7A3F"></div></div>
            </div>
            <div class="pitem hi">
              <div class="pdot"></div>
              <div class="pname">DESCENT</div>
              <div class="ptime">14:12Z · 14 MIN · FL220→FL000 · STRESS INDEX: 62</div>
              <div class="pbar-bg"><div class="pbar-fill" style="width:62%;background:#92600A"></div></div>
            </div>
            <div class="pitem hi">
              <div class="pdot"></div>
              <div class="pname">APPROACH &amp; LANDING</div>
              <div class="ptime">14:26Z · 13 MIN · ILS RWY 35 LTBJ · STRESS INDEX: 81</div>
              <div class="pbar-bg"><div class="pbar-fill" style="width:81%;background:#B91C1C"></div></div>
            </div>
          </div>
        </div>
        <div class="panel">
          <div class="panel-t"><span>AI INSIGHT MODULE</span></div>
          <div class="insight">
            <div class="insight-tag">▸ PATTERN DETECTED</div>
            <div class="insight-txt">APPROACH PHASE HR EXCEEDED PERSONAL AVERAGE BY <span>39%</span>. SAME PROFILE OBSERVED ON 28 MAR LTBA–LTAI SECTOR. CONSIDER 2-MIN PREPARATION PROTOCOL BEFORE TOP-OF-DESCENT.</div>
          </div>
          <div class="insight">
            <div class="insight-tag">▸ TREND ALERT</div>
            <div class="insight-txt">HRV THIS MONTH <span>12% BELOW</span> PERSONAL NORM. PRE-FLIGHT SLEEP QUALITY SUBOPTIMAL (6.8H, REM 18%). RECOMMEND MONITORING SLEEP CYCLE FOR NEXT 7 DAYS.</div>
          </div>
          <div class="insight">
            <div class="insight-tag">▸ TRAINING RECOMMENDATION</div>
            <div class="insight-txt">SIMULATOR SESSIONS SUGGESTED: HIGH-DENSITY TRAFFIC APPROACH SCENARIOS. TARGET: LTBJ ILS CAT I UNDER CROSSWIND CONDITIONS.</div>
          </div>
        </div>
      </div>
    </div>

    <!-- DEBRIEFING -->
    <div id="reflect" class="section">
      <div class="shead">
        <div>
          <div class="stitle">POST-FLIGHT DEBRIEFING</div>
          <div class="ssub">PERSONAL REFLECTIVE NOTES · ENCRYPTED · LOCAL STORAGE ONLY</div>
        </div>
      </div>
      <div class="g2">
        <div class="panel">
          <div class="panel-t"><span>STRUCTURED SELF-ASSESSMENT</span></div>
          <div class="rq">
            <label>Q1 — BEST MANAGED MOMENT THIS FLIGHT?</label>
            <textarea rows="3" placeholder="Enter your notes..."></textarea>
          </div>
          <div class="rq">
            <label>Q2 — MOMENT OF PRESSURE OR UNCERTAINTY?</label>
            <textarea rows="3" placeholder="Enter your notes..."></textarea>
          </div>
          <div class="rq">
            <label>Q3 — WHAT WOULD YOU DO DIFFERENTLY?</label>
            <textarea rows="3" placeholder="Enter your notes..."></textarea>
          </div>
          <button onclick="saveNote(this)" style="width:100%;background:var(--navy);border:none;color:#fff;font-family:var(--mono);font-size:10px;letter-spacing:1px;padding:9px;border-radius:3px;cursor:pointer;font-weight:600;margin-top:4px;">SAVE TO LOCAL STORAGE</button>
          <div style="font-family:var(--mono);font-size:9px;color:var(--dim);text-align:center;margin-top:6px;">NOTES REMAIN ON THIS DEVICE ONLY — NEVER TRANSMITTED</div>
        </div>
        <div class="panel">
          <div class="panel-t"><span>SKILL DEVELOPMENT &amp; TREND</span></div>
          <div class="insight" style="background:var(--blue-bg);border-color:var(--blue-bd);border-left-color:var(--navy)">
            <div class="insight-tag" style="color:var(--navy)">▸ RECOMMENDED FOCUS AREA</div>
            <div class="insight-txt">Based on biometric data, <span>APPROACH PHASE MANAGEMENT</span> is the priority for this month. Simulator sessions with dense traffic and ILS procedures are recommended.</div>
          </div>
          <div style="margin-top:14px;">
            <div class="panel-t" style="border:none;padding:0;margin:0 0 10px;">FLIGHT SATISFACTION RATING</div>
            <div style="display:flex;gap:6px;" id="stars">
              <span onclick="rate(1)" style="font-size:22px;cursor:pointer;color:#C8D0DC;font-family:var(--mono)">★</span>
              <span onclick="rate(2)" style="font-size:22px;cursor:pointer;color:#C8D0DC;font-family:var(--mono)">★</span>
              <span onclick="rate(3)" style="font-size:22px;cursor:pointer;color:#C8D0DC;font-family:var(--mono)">★</span>
              <span onclick="rate(4)" style="font-size:22px;cursor:pointer;color:#C8D0DC;font-family:var(--mono)">★</span>
              <span onclick="rate(5)" style="font-size:22px;cursor:pointer;color:#C8D0DC;font-family:var(--mono)">★</span>
            </div>
          </div>
          <div style="margin-top:14px;">
            <div class="panel-t" style="border:none;padding:0;margin:0 0 10px;">MONTHLY SCORE TREND</div>
            <div class="bbar-row"><span class="bbar-lbl" style="width:40px;font-size:9px;">NOV</span><div class="bbar-bg"><div class="bbar-fill" style="width:68%;background:#1A6FB5"></div></div><span class="bbar-val" style="color:#1A6FB5">68</span></div>
            <div class="bbar-row"><span class="bbar-lbl" style="width:40px;font-size:9px;">DEC</span><div class="bbar-bg"><div class="bbar-fill" style="width:72%;background:#1A6FB5"></div></div><span class="bbar-val" style="color:#1A6FB5">72</span></div>
            <div class="bbar-row"><span class="bbar-lbl" style="width:40px;font-size:9px;">JAN</span><div class="bbar-bg"><div class="bbar-fill" style="width:75%;background:#1A7A3F"></div></div><span class="bbar-val" style="color:#1A7A3F">75</span></div>
            <div class="bbar-row"><span class="bbar-lbl" style="width:40px;font-size:9px;">FEB</span><div class="bbar-bg"><div class="bbar-fill" style="width:79%;background:#1A7A3F"></div></div><span class="bbar-val" style="color:#1A7A3F">79</span></div>
            <div class="bbar-row"><span class="bbar-lbl" style="width:40px;font-size:9px;">MAR</span><div class="bbar-bg"><div class="bbar-fill" style="width:80%;background:#1A7A3F"></div></div><span class="bbar-val" style="color:#1A7A3F">80</span></div>
            <div class="bbar-row"><span class="bbar-lbl" style="width:40px;font-size:9px;">APR</span><div class="bbar-bg"><div class="bbar-fill" style="width:82%;background:#1D3557"></div></div><span class="bbar-val" style="color:#1D3557">82</span></div>
          </div>
        </div>
      </div>
    </div>

    <!-- HISTORY -->
    <div id="history" class="section">
      <div class="shead">
        <div>
          <div class="stitle">FLIGHT HISTORY</div>
          <div class="ssub">PERSONAL RECORDS · 5 ENTRIES · LOCAL DATABASE</div>
        </div>
      </div>
      <div class="panel">
        <div class="panel-t"><span>RECENT FLIGHTS</span></div>
        <div class="hrow">
          <div class="hdate">02 APR</div>
          <div><div class="hroute">LTBA/IST → LTBJ/ESB</div><div class="hinfo">01:47 · FL220 · B737 · SCORE 82</div></div>
          <div class="hscore" style="color:var(--green)">82 ▲</div>
        </div>
        <div class="hrow">
          <div class="hdate">28 MAR</div>
          <div><div class="hroute">LTBA/IST → LTAI/AYT</div><div class="hinfo">01:12 · FL260 · B737 · SCORE 77</div></div>
          <div class="hscore" style="color:var(--amber)">77</div>
        </div>
        <div class="hrow">
          <div class="hdate">21 MAR</div>
          <div><div class="hroute">LTBJ/ESB → LTBA/IST</div><div class="hinfo">01:50 · FL220 · B737 · SCORE 89</div></div>
          <div class="hscore" style="color:var(--green)">89 ▲</div>
        </div>
        <div class="hrow">
          <div class="hdate">14 MAR</div>
          <div><div class="hroute">LTBA/IST → LTFE/ERC</div><div class="hinfo">02:08 · FL280 · B737 · SCORE 71</div></div>
          <div class="hscore" style="color:var(--amber)">71</div>
        </div>
        <div class="hrow">
          <div class="hdate">07 MAR</div>
          <div><div class="hroute">LTBA/IST → LTCC/DYB</div><div class="hinfo">01:34 · FL240 · B737 · SCORE 85</div></div>
          <div class="hscore" style="color:var(--green)">85 ▲</div>
        </div>
      </div>
    </div>

  </div>
</div>

<script>
function htab(id,el){
  document.querySelectorAll('.htab').forEach(t=>t.classList.remove('active'));
  el.classList.add('active');
  nav(id,null);
}
function nav(id,el){
  document.querySelectorAll('.section').forEach(s=>s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  if(el){
    document.querySelectorAll('.sitem').forEach(s=>s.classList.remove('active'));
    el.classList.add('active');
  }
  document.querySelectorAll('.htab').forEach(t=>{
    if(t.getAttribute('onclick')&&t.getAttribute('onclick').includes("'"+id+"'"))
      t.classList.add('active');
    else t.classList.remove('active');
  });
}
function rate(n){
  document.querySelectorAll('#stars span').forEach((s,i)=>{
    s.style.color=i<n?'#92600A':'#C8D0DC';
  });
}
function saveNote(btn){
  const orig=btn.textContent;
  btn.textContent='✓ SAVED TO LOCAL STORAGE';
  btn.style.background='#1A7A3F';
  setTimeout(()=>{btn.textContent=orig;btn.style.background='var(--navy)';},2000);
}
function tick(){
  const now=new Date();
  const pad=n=>String(n).padStart(2,'0');
  const months=['JAN','FEB','MAR','APR','MAY','JUN','JUL','AUG','SEP','OCT','NOV','DEC'];
  document.getElementById('clk').textContent=
    `${pad(now.getUTCDate())} ${months[now.getUTCMonth()]} ${now.getUTCFullYear()} · ${pad(now.getUTCHours())}:${pad(now.getUTCMinutes())}:${pad(now.getUTCSeconds())}Z`;
}
tick();setInterval(tick,1000);
</script>
</body>
</html>
