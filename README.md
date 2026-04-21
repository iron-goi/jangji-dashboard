<!DOCTYPE html>
<html lang="ko" data-theme="dark">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>수도권 장지 통합 현황 대시보드</title>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;700;900&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
/* ── 다크 테마 (기본) ── */
:root {
  --bg:#0d1117; --surface:#161b22; --surface2:#1c2128;
  --border:#30363d; --text:#e6edf3; --muted:#7d8590;
  --accent:#58a6ff; --green:#3fb950; --yellow:#d29922;
  --red:#f85149; --purple:#d2a8ff; --orange:#ffa657; --teal:#39d353;
  --row-hover:rgba(88,166,255,0.05);
  --period-bg:#21262d;
  --fs-base: 14px;
}
/* ── 라이트 테마 ── */
[data-theme="light"] {
  --bg:#f6f8fa; --surface:#ffffff; --surface2:#f0f3f6;
  --border:#d0d7de; --text:#1f2328; --muted:#636c76;
  --accent:#0969da; --green:#1a7f37; --yellow:#9a6700;
  --red:#cf222e; --purple:#8250df; --orange:#bc4c00; --teal:#116329;
  --row-hover:rgba(9,105,218,0.04);
  --period-bg:#eaeef2;
}
/* ── 폰트 크기 조절 ── */
[data-size="sm"] { --fs-base:13px; }
[data-size="md"] { --fs-base:14px; }
[data-size="lg"] { --fs-base:15px; }
[data-size="xl"] { --fs-base:16px; }

*{box-sizing:border-box;margin:0;padding:0;}
html{font-size:var(--fs-base);}
body{font-family:'Noto Sans KR',sans-serif;background:var(--bg);color:var(--text);min-height:100vh;transition:background .2s,color .2s;}

/* HEADER */
.header{background:var(--surface);border-bottom:1px solid var(--border);padding:14px 24px;display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;z-index:200;gap:12px;}
.header-left{display:flex;align-items:center;gap:12px;min-width:0;}
.logo{width:36px;height:36px;background:linear-gradient(135deg,#1f6feb,var(--accent));border-radius:8px;display:flex;align-items:center;justify-content:center;font-size:18px;flex-shrink:0;}
.header-title{font-size:1rem;font-weight:700;}
.header-sub{font-size:.75rem;color:var(--muted);margin-top:1px;}
.header-right{display:flex;align-items:center;gap:8px;flex-shrink:0;}
.badge-date{font-size:.7rem;color:var(--muted);background:var(--period-bg);border:1px solid var(--border);padding:4px 10px;border-radius:20px;font-family:'DM Mono',monospace;}

/* 테마/폰트 컨트롤 */
.ctrl-group{display:flex;align-items:center;gap:4px;background:var(--surface2);border:1px solid var(--border);border-radius:8px;padding:3px;}
.ctrl-btn{background:transparent;border:none;border-radius:5px;padding:4px 9px;font-size:.72rem;font-family:'Noto Sans KR',sans-serif;color:var(--muted);cursor:pointer;transition:all .15s;white-space:nowrap;}
.ctrl-btn:hover{color:var(--text);}
.ctrl-btn.active{background:var(--accent);color:#fff;}
[data-theme="light"] .ctrl-btn.active{background:var(--accent);color:#fff;}
.ctrl-sep{width:1px;height:18px;background:var(--border);}

/* CONTAINER */
.container{padding:20px 24px;max-width:1800px;margin:0 auto;}

/* STATS */
.stats-row{display:grid;grid-template-columns:repeat(6,1fr);gap:12px;margin-bottom:20px;}
.stat-card{background:var(--surface);border:1px solid var(--border);border-radius:10px;padding:14px 16px;transition:border-color .2s,transform .15s;cursor:default;}
.stat-card:hover{border-color:var(--accent);transform:translateY(-1px);}
.stat-label{font-size:.68rem;color:var(--muted);text-transform:uppercase;letter-spacing:.7px;margin-bottom:6px;}
.stat-value{font-size:1.7rem;font-weight:900;font-family:'DM Mono',monospace;line-height:1;}
.sv-blue{color:var(--accent);} .sv-green{color:var(--green);} .sv-yellow{color:var(--yellow);}
.sv-red{color:var(--red);} .sv-purple{color:var(--purple);} .sv-gray{color:var(--muted);}
.stat-sub{font-size:.72rem;color:var(--muted);margin-top:4px;}

/* CONTROLS */
.controls{display:flex;gap:8px;margin-bottom:16px;flex-wrap:wrap;align-items:center;}
.search-wrap{flex:1;min-width:220px;position:relative;}
.search-icon{position:absolute;left:12px;top:50%;transform:translateY(-50%);color:var(--muted);pointer-events:none;font-size:.85rem;}
.search-box{width:100%;background:var(--surface);border:1px solid var(--border);border-radius:8px;padding:8px 14px 8px 34px;color:var(--text);font-family:'Noto Sans KR',sans-serif;font-size:.88rem;outline:none;transition:border-color .2s;}
.search-box:focus{border-color:var(--accent);}
.search-box::placeholder{color:var(--muted);}
.sep{width:1px;height:28px;background:var(--border);margin:0 2px;flex-shrink:0;}
.filter-btn{background:var(--surface);border:1px solid var(--border);border-radius:8px;padding:7px 13px;color:var(--muted);font-family:'Noto Sans KR',sans-serif;font-size:.8rem;cursor:pointer;transition:all .15s;white-space:nowrap;}
.filter-btn:hover{color:var(--text);border-color:var(--muted);}
.f-blue{border-color:var(--accent)!important;color:var(--accent)!important;background:rgba(88,166,255,0.08)!important;}
.f-green{border-color:var(--green)!important;color:var(--green)!important;background:rgba(63,185,80,0.08)!important;}
.f-yellow{border-color:var(--yellow)!important;color:var(--yellow)!important;background:rgba(210,153,34,0.08)!important;}
.f-red{border-color:var(--red)!important;color:var(--red)!important;background:rgba(248,81,73,0.08)!important;}
.f-teal{border-color:var(--teal)!important;color:var(--teal)!important;background:rgba(57,211,83,0.08)!important;}
.f-gray{border-color:var(--muted)!important;color:var(--muted)!important;background:rgba(125,133,144,0.08)!important;}

/* TABLE */
.table-wrap{background:var(--surface);border:1px solid var(--border);border-radius:12px;overflow:hidden;}
.table-top{display:flex;align-items:center;justify-content:space-between;padding:12px 16px;border-bottom:1px solid var(--border);}
.table-top-title{font-size:.88rem;font-weight:600;}
.row-count{font-size:.72rem;color:var(--muted);font-family:'DM Mono',monospace;}
.scroll-hint{display:none;font-size:.7rem;color:var(--muted);padding:5px 16px 0;text-align:right;}
.table-scroll-wrap{overflow-x:auto;-webkit-overflow-scrolling:touch;}

.tbl{width:100%;border-collapse:collapse;min-width:1380px;}
.tbl th:nth-child(1){width:70px;} .tbl th:nth-child(2){width:90px;} .tbl th:nth-child(3){width:175px;}
.tbl th:nth-child(4){width:110px;} .tbl th:nth-child(5){width:110px;} .tbl th:nth-child(6){width:150px;}
.tbl th:nth-child(7){width:195px;} .tbl th:nth-child(8){width:195px;} .tbl th:nth-child(9){width:72px;}
.tbl th:nth-child(10){width:115px;} .tbl th:nth-child(11){width:100px;} .tbl th:nth-child(12){width:150px;}

.tbl thead tr{background:var(--surface2);border-bottom:1px solid var(--border);}
.tbl th{padding:10px 12px;font-size:.72rem;font-weight:700;color:var(--muted);text-align:left;text-transform:uppercase;letter-spacing:.5px;cursor:pointer;white-space:nowrap;user-select:none;}
.tbl th:hover{color:var(--text);}
.tbl th.sorted{color:var(--accent);}
.tbl tbody tr{border-bottom:1px solid var(--border);transition:background .1s;}
.tbl tbody tr:last-child{border-bottom:none;}
.tbl tbody tr:hover{background:var(--row-hover);}
.tbl td{padding:10px 12px;font-size:.82rem;vertical-align:middle;}

.cell-name{font-weight:600;color:var(--text);font-size:.85rem;}
.cell-sub{font-size:.72rem;color:var(--muted);margin-top:2px;}
.cell-region{color:var(--muted);white-space:nowrap;font-size:.8rem;}

/* BADGES */
.kind-badge{display:inline-block;padding:2px 8px;border-radius:4px;font-size:.68rem;font-weight:700;white-space:nowrap;}
.kind-공설{background:rgba(88,166,255,0.15);color:#58a6ff;border:1px solid rgba(88,166,255,0.3);}
.kind-사설제휴{background:rgba(57,211,83,0.15);color:#39d353;border:1px solid rgba(57,211,83,0.3);}
.kind-사설비제휴{background:rgba(125,133,144,0.12);color:var(--muted);border:1px solid rgba(125,133,144,0.25);}
[data-theme="light"] .kind-공설{color:#0969da;}
[data-theme="light"] .kind-사설제휴{color:#1a7f37;}

.badge{display:inline-block;padding:2px 8px;border-radius:4px;font-size:.75rem;font-weight:500;white-space:nowrap;}
.t-봉안당{background:rgba(88,166,255,0.15);color:#58a6ff;border:1px solid rgba(88,166,255,0.25);}
.t-봉안담,.t-야외봉안담{background:rgba(121,192,255,0.12);color:#79c0ff;border:1px solid rgba(121,192,255,0.25);}
.t-잔디장{background:rgba(63,185,80,0.15);color:#3fb950;border:1px solid rgba(63,185,80,0.25);}
.t-수목장{background:rgba(86,211,100,0.12);color:#56d364;border:1px solid rgba(86,211,100,0.25);}
.t-봉안묘{background:rgba(210,168,255,0.15);color:#d2a8ff;border:1px solid rgba(210,168,255,0.25);}
.t-매장묘{background:rgba(255,166,87,0.15);color:#ffa657;border:1px solid rgba(255,166,87,0.25);}
.t-평장묘,.t-자연장{background:rgba(255,200,87,0.12);color:#e3b341;border:1px solid rgba(255,200,87,0.25);}
.t-해양장{background:rgba(56,189,248,0.12);color:#38bdf8;border:1px solid rgba(56,189,248,0.25);}
.t-제사상품{background:rgba(248,81,73,0.1);color:#f87171;border:1px solid rgba(248,81,73,0.2);}
.t-기타{background:rgba(125,133,144,0.12);color:var(--muted);border:1px solid rgba(125,133,144,0.2);}

[data-theme="light"] .t-봉안당{color:#0969da;background:rgba(9,105,218,0.08);}
[data-theme="light"] .t-잔디장{color:#1a7f37;background:rgba(26,127,55,0.08);}
[data-theme="light"] .t-수목장{color:#116329;background:rgba(17,99,41,0.08);}
[data-theme="light"] .t-봉안묘{color:#8250df;background:rgba(130,80,223,0.08);}
[data-theme="light"] .t-매장묘{color:#bc4c00;background:rgba(188,76,0,0.08);}

/* STATUS */
.status{display:inline-flex;align-items:center;gap:5px;font-size:.8rem;white-space:nowrap;}
.dot{width:7px;height:7px;border-radius:50%;flex-shrink:0;}
.dot-ok{background:var(--green);box-shadow:0 0 5px rgba(63,185,80,.5);}
.dot-warn{background:var(--yellow);box-shadow:0 0 5px rgba(210,153,34,.5);}
.dot-full{background:var(--red);box-shadow:0 0 5px rgba(248,81,73,.5);}
.dot-na{background:var(--muted);}

/* PRICE */
.price-col{font-family:'DM Mono',monospace;font-size:.8rem;}
.price-in{color:var(--green);}
.price-out{color:var(--orange);}
.price-fee{color:var(--teal);}
.price-na{color:var(--muted);}
.price-unit{font-size:.68rem;color:var(--muted);}

.condition-text{color:var(--muted);font-size:.75rem;line-height:1.5;word-break:keep-all;}
.period-chip{display:inline-block;font-size:.72rem;font-family:'DM Mono',monospace;color:var(--muted);background:var(--period-bg);border:1px solid var(--border);padding:2px 7px;border-radius:4px;white-space:nowrap;}
.note-text{font-size:.72rem;color:var(--purple);line-height:1.5;word-break:keep-all;}
[data-theme="light"] .note-text{color:var(--purple);}
.empty-row td{text-align:center;padding:48px;color:var(--muted);}

::-webkit-scrollbar{width:6px;height:6px;}
::-webkit-scrollbar-track{background:var(--bg);}
::-webkit-scrollbar-thumb{background:var(--border);border-radius:3px;}
::-webkit-scrollbar-thumb:hover{background:var(--muted);}

@media(max-width:1400px){.stats-row{grid-template-columns:repeat(3,1fr);}.scroll-hint{display:block;}}
@media(max-width:900px){.stats-row{grid-template-columns:repeat(2,1fr);}.container{padding:14px 12px;}.controls{gap:6px;}.sep{display:none;}}
</style>
</head>
<body>

<div class="header">
  <div class="header-left">
    <div class="logo">🏛</div>
    <div>
      <div class="header-title">수도권 장지 통합 현황</div>
      <div class="header-sub">공설 · 사설 제휴 · 비제휴 Sales Dashboard</div>
    </div>
  </div>
  <div class="header-right">
    <!-- 테마 -->
    <div class="ctrl-group">
      <button class="ctrl-btn active" id="theme-dark" onclick="setTheme('dark')">🌙 다크</button>
      <div class="ctrl-sep"></div>
      <button class="ctrl-btn" id="theme-light" onclick="setTheme('light')">☀️ 라이트</button>
    </div>
    <!-- 폰트 크기 -->
    <div class="ctrl-group">
      <button class="ctrl-btn" id="size-sm" onclick="setSize('sm')">소</button>
      <div class="ctrl-sep"></div>
      <button class="ctrl-btn active" id="size-md" onclick="setSize('md')">중</button>
      <div class="ctrl-sep"></div>
      <button class="ctrl-btn" id="size-lg" onclick="setSize('lg')">대</button>
      <div class="ctrl-sep"></div>
      <button class="ctrl-btn" id="size-xl" onclick="setSize('xl')">특대</button>
    </div>
    <span class="badge-date">업데이트 2026.04.17</span>
  </div>
</div>

<div class="container">
  <div class="stats-row" id="statsRow"></div>

  <div class="controls">
    <div class="search-wrap">
      <span class="search-icon">🔍</span>
      <input class="search-box" type="text" placeholder="시설명, 지역, 장법, 수수료 검색..." id="searchInput" oninput="applyFilters()">
    </div>
    <div class="sep"></div>
    <button class="filter-btn f-blue"  id="kbtn-"       onclick="setFilter('kind','')">전체</button>
    <button class="filter-btn"         id="kbtn-공설"    onclick="setFilter('kind','공설')">🏛 공설</button>
    <button class="filter-btn"         id="kbtn-사설제휴" onclick="setFilter('kind','사설제휴')">🤝 사설 제휴</button>
    <button class="filter-btn"         id="kbtn-사설비제휴" onclick="setFilter('kind','사설비제휴')">🚫 사설 비제휴</button>
    <div class="sep"></div>
    <button class="filter-btn f-blue"  id="rbtn-"   onclick="setFilter('region','')">전체 지역</button>
    <button class="filter-btn"         id="rbtn-서울" onclick="setFilter('region','서울')">서울</button>
    <button class="filter-btn"         id="rbtn-인천" onclick="setFilter('region','인천')">인천</button>
    <button class="filter-btn"         id="rbtn-경기" onclick="setFilter('region','경기')">경기</button>
    <div class="sep"></div>
    <button class="filter-btn f-blue"  id="sbtn-"     onclick="setFilter('status','')">전체 현황</button>
    <button class="filter-btn"         id="sbtn-ok"   onclick="setFilter('status','ok')">✅ 여유</button>
    <button class="filter-btn"         id="sbtn-warn" onclick="setFilter('status','warn')">⚠️ 임박</button>
    <button class="filter-btn"         id="sbtn-full" onclick="setFilter('status','full')">🔴 만장</button>
    <div class="sep"></div>
    <button class="filter-btn f-blue"  id="tbtn-"     onclick="setFilter('type','')">전체 장법</button>
    <button class="filter-btn"         id="tbtn-봉안당" onclick="setFilter('type','봉안당')">봉안당</button>
    <button class="filter-btn"         id="tbtn-봉안담" onclick="setFilter('type','봉안담')">봉안담</button>
    <button class="filter-btn"         id="tbtn-잔디장" onclick="setFilter('type','잔디장')">잔디장</button>
    <button class="filter-btn"         id="tbtn-수목장" onclick="setFilter('type','수목장')">수목장</button>
    <button class="filter-btn"         id="tbtn-봉안묘" onclick="setFilter('type','봉안묘')">봉안묘</button>
    <button class="filter-btn"         id="tbtn-매장묘" onclick="setFilter('type','매장묘')">매장묘</button>
  </div>

  <div class="table-wrap">
    <div class="table-top">
      <span class="table-top-title">장지 목록</span>
      <span class="row-count" id="rowCountLabel">0개</span>
    </div>
    <div class="scroll-hint">← → 좌우 스크롤하면 모든 정보를 볼 수 있어요</div>
    <div class="table-scroll-wrap">
      <table class="tbl" id="mainTable">
        <thead><tr>
          <th onclick="sortBy('kind')">구분</th>
          <th onclick="sortBy('region')">지역</th>
          <th onclick="sortBy('name')">시설명</th>
          <th onclick="sortBy('type')">장법</th>
          <th>안치 종류</th>
          <th onclick="sortBy('statusLevel')">현황</th>
          <th>관내 / 이용 조건</th>
          <th>관외 / 기타 조건</th>
          <th onclick="sortBy('periodNum')">기간</th>
          <th onclick="sortBy('priceIn')">관내금액 / 수수료</th>
          <th onclick="sortBy('priceOut')">관외금액</th>
          <th>비고</th>
        </tr></thead>
        <tbody id="tableBody"></tbody>
      </table>
    </div>
  </div>
</div>

<script>
// ═══════════════════════════════════════════════════════════════════
// ── 공설 장지 데이터 (CSV 공설장지현황 기준 완전 검증) ──────────────
// ═══════════════════════════════════════════════════════════════════
const PUBLIC = [
  // ── 서울 ──
  {region:"서울",name:"용미리 제1묘지 자연장지",type:"잔디장",unit:"개인단",status:"여유 있음",condIn:"6개월 이상 주민등록 서울·고양·파주",condOut:"",period:"40년",priceIn:500000,priceOut:0,note:""},
  {region:"서울",name:"용미리 납골당",type:"봉안당",unit:"개인단",status:"만장",condIn:"국가유공자·수급자·의사상자만 가능",condOut:"",period:"",priceIn:0,priceOut:0,note:"만장"},
  // ── 경기 하남 ──
  {region:"경기 하남",name:"하남시 마루공원",type:"봉안당",unit:"개인단",status:"5년 정도 여유",condIn:"1년이상 주민등록 / 관내 개장한 유골",condOut:"부모·배우자·자녀 1년이상 거주",period:"30년",priceIn:500000,priceOut:1000000,note:""},
  {region:"경기 하남",name:"하남시 마루공원",type:"봉안당",unit:"부부단",status:"5년 정도 여유",condIn:"1년이상 주민등록 / 관내 개장한 유골",condOut:"부모·배우자·자녀 1년이상 거주",period:"30년",priceIn:700000,priceOut:1400000,note:""},
  // ── 경기 화성 ──
  {region:"경기 화성",name:"화성함백산추모공원(별빛쉼터)",type:"봉안당",unit:"개인단",status:"만장",condIn:"6개월이상 주민등록 7개시",condOut:"7개시 주민등록의 배우자",period:"30년",priceIn:500000,priceOut:1000000,note:""},
  {region:"경기 화성",name:"화성함백산추모공원(별빛쉼터)",type:"봉안당",unit:"부부단",status:"몇개월 안남음",condIn:"6개월이상 주민등록 7개시",condOut:"7개시 주민등록의 배우자",period:"30년",priceIn:750000,priceOut:1500000,note:""},
  {region:"경기 화성",name:"화성함백산추모공원(바람마루)",type:"잔디장",unit:"개인단",status:"여유 있음",condIn:"6개월이상 주민등록 화성시",condOut:"6개월이상 주민등록 화성시 배우자",period:"30년",priceIn:800000,priceOut:1600000,note:""},
  {region:"경기 화성",name:"화성함백산추모공원(바람마루)",type:"수목장",unit:"개인단",status:"여유 있음",condIn:"6개월이상 주민등록 화성시",condOut:"6개월이상 주민등록 화성시 배우자",period:"30년",priceIn:1800000,priceOut:3600000,note:""},
  {region:"경기 화성",name:"화성시추모공원 봉안당",type:"봉안당",unit:"개인단",status:"26년도 내에는 가능",condIn:"1년이상 주민등록 화성시",condOut:"부모·자녀 / 이미 안치된 자의 배우자",period:"30년",priceIn:500000,priceOut:1000000,note:""},
  {region:"경기 화성",name:"화성시추모공원 봉안당",type:"봉안당",unit:"부부단",status:"26년도 내에는 가능",condIn:"1년이상 주민등록 화성시",condOut:"부모·자녀 / 이미 안치된 자의 배우자",period:"30년",priceIn:750000,priceOut:1500000,note:""},
  // ── 경기 여주 ──
  {region:"경기 여주",name:"여주추모공원 봉안담",type:"봉안담",unit:"개인담",status:"5~6개월 이내 만장",condIn:"6개월이상 주민등록 여주시",condOut:"배우자·부모·자녀",period:"45년",priceIn:500000,priceOut:1000000,note:""},
  {region:"경기 여주",name:"여주추모공원 봉안담",type:"봉안담",unit:"부부담",status:"5~6개월 이내 만장",condIn:"6개월이상 주민등록 여주시",condOut:"배우자·부모·자녀",period:"45년",priceIn:750000,priceOut:1500000,note:""},
  {region:"경기 여주",name:"여주추모공원 잔디장",type:"잔디장",unit:"개인장",status:"5~6개월 이내 만장",condIn:"6개월이상 주민등록 여주시",condOut:"배우자·부모·자녀",period:"45년",priceIn:350000,priceOut:700000,note:""},
  {region:"경기 여주",name:"여주추모공원 잔디장",type:"잔디장",unit:"부부장",status:"5~6개월 이내 만장",condIn:"6개월이상 주민등록 여주시",condOut:"배우자·부모·자녀",period:"45년",priceIn:525000,priceOut:1050000,note:""},
  // ── 경기 오산 ──
  {region:"경기 오산",name:"오산시립쉼터공원",type:"봉안당",unit:"개인단",status:"여유 있음",condIn:"6개월이상 주민등록 오산시",condOut:"배우자·직계존비속",period:"45년",priceIn:500000,priceOut:750000,note:""},
  {region:"경기 오산",name:"오산시립쉼터공원",type:"봉안당",unit:"부부단",status:"여유 있음",condIn:"6개월이상 주민등록 오산시",condOut:"배우자·직계존비속",period:"45년",priceIn:700000,priceOut:1050000,note:""},
  // ── 경기 광명 ──
  {region:"경기 광명",name:"광명메모리얼파크",type:"봉안당",unit:"개인단",status:"3년 정도 여유",condIn:"6개월이상 주민등록 광명시",condOut:"배우자·부모·자녀 / 10년이상 거주자 / 초중고 졸업생",period:"45년",priceIn:750000,priceOut:1500000,note:""},
  {region:"경기 광명",name:"광명메모리얼파크",type:"봉안당",unit:"부부단",status:"3년 정도 여유",condIn:"6개월이상 주민등록 광명시",condOut:"",period:"45년",priceIn:1000000,priceOut:2000000,note:""},
  // ── 경기 용인 ──
  {region:"경기 용인",name:"용인평온의숲(봉안당)",type:"봉안당",unit:"개인단",status:"여유 있음",condIn:"6개월이상 용인·안성시 주민등록",condOut:"6개월이상 용인시 주민등록 부모·배우자·자녀",period:"30년",priceIn:450000,priceOut:1300000,note:""},
  {region:"경기 용인",name:"용인평온의숲(봉안당)",type:"봉안당",unit:"부부단",status:"여유 있음",condIn:"6개월이상 용인·안성시 주민등록",condOut:"6개월이상 용인시 주민등록 부모·배우자·자녀",period:"30년",priceIn:700000,priceOut:2400000,note:""},
  {region:"경기 용인",name:"용인평온의숲(예정원)",type:"봉안묘",unit:"개인장",status:"만장",condIn:"6개월이상 용인·안성시 주민등록",condOut:"6개월이상 용인시 주민등록 부모·배우자·자녀",period:"30년",priceIn:1400000,priceOut:2200000,note:""},
  {region:"경기 용인",name:"용인평온의숲(예정원)",type:"봉안묘",unit:"부부장",status:"만장",condIn:"6개월이상 용인·안성시 주민등록",condOut:"6개월이상 용인시 주민등록 부모·배우자·자녀",period:"30년",priceIn:2600000,priceOut:3900000,note:""},
  {region:"경기 용인",name:"용인평온의숲(예정원)",type:"봉안묘",unit:"가족(4위)",status:"만장",condIn:"6개월이상 용인·안성시 주민등록",condOut:"6개월이상 용인시 주민등록 부모·배우자·자녀",period:"30년",priceIn:4200000,priceOut:6500000,note:""},
  {region:"경기 용인",name:"용인평온의숲(예정원)",type:"잔디장",unit:"개인장",status:"여유 있음",condIn:"6개월이상 용인·안성시 주민등록",condOut:"6개월이상 용인시 주민등록 부모·배우자·자녀",period:"30년",priceIn:1100000,priceOut:1650000,note:""},
  {region:"경기 용인",name:"용인평온의숲(예정원)",type:"잔디장",unit:"부부장",status:"만장",condIn:"6개월이상 용인·안성시 주민등록",condOut:"6개월이상 용인시 주민등록 부모·배우자·자녀",period:"30년",priceIn:1600000,priceOut:2400000,note:""},
  {region:"경기 용인",name:"용인평온의숲(예정원)",type:"잔디장",unit:"가족(4위)",status:"여유 있음",condIn:"6개월이상 용인·안성시 주민등록",condOut:"6개월이상 용인시 주민등록 부모·배우자·자녀",period:"30년",priceIn:2600000,priceOut:3900000,note:""},
  {region:"경기 용인",name:"용인평온의숲(예정원)",type:"수목장",unit:"공동(12위)",status:"여유 있음",condIn:"6개월이상 용인·안성시 주민등록",condOut:"6개월이상 용인시 주민등록 부모·배우자·자녀",period:"30년",priceIn:1200000,priceOut:1800000,note:"1위당"},
  // ── 경기 안성 ──
  {region:"경기 안성",name:"안성 추모 공원",type:"봉안담",unit:"개인담",status:"여유 있음",condIn:"6개월이상 주민등록 안성시",condOut:"부모·배우자·자녀",period:"45년",priceIn:450000,priceOut:1000000,note:""},
  {region:"경기 안성",name:"안성 추모 공원",type:"봉안담",unit:"부부담",status:"만장",condIn:"6개월이상 주민등록 안성시",condOut:"부모·배우자·자녀",period:"45년",priceIn:700000,priceOut:1600000,note:""},
  {region:"경기 안성",name:"안성 추모 공원",type:"잔디장",unit:"개인장",status:"만장",condIn:"6개월이상 주민등록 안성시",condOut:"부모·배우자·자녀",period:"45년",priceIn:500000,priceOut:1000000,note:""},
  {region:"경기 안성",name:"안성 추모 공원",type:"잔디장",unit:"부부장",status:"만장",condIn:"6개월이상 주민등록 안성시",condOut:"부모·배우자·자녀",period:"45년",priceIn:800000,priceOut:1600000,note:""},
  {region:"경기 안성",name:"안성 추모 공원",type:"수목장",unit:"가족(4위)",status:"만장",condIn:"6개월이상 주민등록 안성시",condOut:"부모·배우자·자녀",period:"45년",priceIn:5200000,priceOut:10400000,note:""},
  {region:"경기 안성",name:"안성 추모 공원",type:"수목장",unit:"가족(6위)",status:"만장",condIn:"6개월이상 주민등록 안성시",condOut:"부모·배우자·자녀",period:"45년",priceIn:6600000,priceOut:13200000,note:""},
  // ── 경기 평택 ──
  {region:"경기 평택",name:"평택시립추모공원",type:"봉안당",unit:"개인단",status:"여유 있음",condIn:"3개월이상 주민등록 평택시",condOut:"미리 안장된 배우자 / 6개월 주민등록자 가족",period:"45년",priceIn:527000,priceOut:790000,note:""},
  {region:"경기 평택",name:"평택시립추모공원",type:"봉안당",unit:"부부단",status:"여유 있음",condIn:"3개월이상 주민등록 평택시",condOut:"미리 안장된 배우자 / 6개월 주민등록자 가족",period:"45년",priceIn:790000,priceOut:1185000,note:""},
  // ── 경기 수원 ──
  {region:"경기 수원",name:"수원시연화장 추모의집",type:"봉안당",unit:"개인단",status:"여유 있음",condIn:"1년이상 주민등록 수원시",condOut:"배우자·부모·자녀 / 수원시 재직 중 사망 공무원",period:"30년",priceIn:800000,priceOut:1300000,note:""},
  {region:"경기 수원",name:"수원시연화장 추모의집",type:"봉안당",unit:"부부단",status:"여유 있음",condIn:"1년이상 주민등록 수원시",condOut:"기존 부부단에 안치된 자의 배우자",period:"30년",priceIn:1100000,priceOut:1800000,note:""},
  {region:"경기 수원",name:"수원시연화장 추모의집",type:"봉안담",unit:"개인담",status:"반환 몇개만 남음",condIn:"1년이상 주민등록 수원시",condOut:"",period:"30년",priceIn:700000,priceOut:1100000,note:"반환분 소량"},
  {region:"경기 수원",name:"수원시연화장 추모의집",type:"봉안담",unit:"부부담",status:"만장",condIn:"1년이상 주민등록 수원시",condOut:"",period:"30년",priceIn:1000000,priceOut:1500000,note:""},
  {region:"경기 수원",name:"수원시연화장 추모의집",type:"잔디장",unit:"개인장",status:"여유 있음",condIn:"1년이상 주민등록 수원시",condOut:"",period:"30년",priceIn:500000,priceOut:1200000,note:""},
  // ── 경기 김포 ──
  {region:"경기 김포",name:"김포시추모공원",type:"봉안담",unit:"개인담",status:"여유 있음",condIn:"6개월이상 주민등록 김포시",condOut:"배우자·부모·자녀",period:"45년",priceIn:350000,priceOut:750000,note:""},
  {region:"경기 김포",name:"김포시추모공원",type:"봉안담",unit:"부부담",status:"여유 있음",condIn:"6개월이상 주민등록 김포시",condOut:"배우자·부모·자녀",period:"45년",priceIn:600000,priceOut:1200000,note:""},
  {region:"경기 김포",name:"김포시추모공원",type:"잔디장",unit:"개인장",status:"여유 있음",condIn:"6개월이상 주민등록 김포시",condOut:"배우자·부모·자녀",period:"30년",priceIn:800000,priceOut:1600000,note:""},
  {region:"경기 김포",name:"김포시추모공원",type:"잔디장",unit:"부부장",status:"여유 있음",condIn:"6개월이상 주민등록 김포시",condOut:"배우자·부모·자녀",period:"30년",priceIn:1500000,priceOut:3000000,note:""},
  {region:"경기 김포",name:"무지개 뜨는 언덕",type:"봉안당",unit:"개인단",status:"만장",condIn:"6개월이상 주민등록 김포시",condOut:"배우자·부모·자녀",period:"45년",priceIn:500000,priceOut:1000000,note:""},
  {region:"경기 김포",name:"무지개 뜨는 언덕",type:"봉안당",unit:"부부단",status:"만장",condIn:"6개월이상 주민등록 김포시",condOut:"배우자·부모·자녀",period:"45년",priceIn:900000,priceOut:1800000,note:""},
  // ── 경기 안산 ──
  {region:"경기 안산",name:"하늘공원(부곡동공설공원묘지)",type:"봉안담",unit:"개인담",status:"여유 있음",condIn:"6개월이상 주민등록 안산시",condOut:"배우자·부모 / 이미 안치된 자의 배우자",period:"45년",priceIn:367000,priceOut:734000,note:""},
  {region:"경기 안산",name:"하늘공원(부곡동공설공원묘지)",type:"봉안담",unit:"부부담",status:"여유 있음",condIn:"6개월이상 주민등록 안산시",condOut:"배우자·부모 / 이미 안치된 자의 배우자",period:"45년",priceIn:555500,priceOut:1111000,note:"생존 배우자 만 70세 이상 조건"},
  {region:"경기 안산",name:"안산 꽃빛 공원",type:"잔디장",unit:"개인장",status:"여유 있음",condIn:"6개월이상 주민등록 안산시",condOut:"배우자·부모",period:"45년",priceIn:1600000,priceOut:0,note:""},
  {region:"경기 안산",name:"안산 꽃빛 공원",type:"수목장",unit:"개인장",status:"여유 있음",condIn:"6개월이상 주민등록 안산시",condOut:"배우자·부모",period:"45년",priceIn:3600000,priceOut:0,note:""},
  // ── 경기 의왕 ──
  {region:"경기 의왕",name:"의왕하늘쉼터",type:"봉안담",unit:"개인담(의왕)",status:"여유 있음",condIn:"1년이상 주민등록 의왕시",condOut:"배우자·직계가족",period:"45년",priceIn:700000,priceOut:1400000,note:""},
  {region:"경기 의왕",name:"의왕하늘쉼터",type:"봉안담",unit:"부부담(의왕)",status:"여유 있음",condIn:"1년이상 주민등록 의왕시",condOut:"배우자·직계가족",period:"45년",priceIn:1300000,priceOut:2600000,note:""},
  {region:"경기 의왕",name:"의왕하늘쉼터",type:"봉안담",unit:"개인담(안양·군포·과천)",status:"여유 있음",condIn:"",condOut:"1년이상 안양·군포·과천 주민등록자",period:"45년",priceIn:0,priceOut:2500000,note:""},
  {region:"경기 의왕",name:"의왕하늘쉼터",type:"봉안담",unit:"부부담(안양·군포·과천)",status:"여유 있음",condIn:"",condOut:"1년이상 안양·군포·과천 주민등록자",period:"45년",priceIn:0,priceOut:4800000,note:""},
  {region:"경기 의왕",name:"의왕하늘쉼터",type:"잔디장",unit:"개인장",status:"여유 있음",condIn:"1년이상 주민등록 의왕시",condOut:"배우자·직계가족",period:"45년",priceIn:680000,priceOut:1360000,note:"경사 심해 노인 방문 어려움"},
  {region:"경기 의왕",name:"의왕하늘쉼터",type:"잔디장",unit:"부부장",status:"여유 있음",condIn:"1년이상 주민등록 의왕시",condOut:"배우자·직계가족",period:"45년",priceIn:1160000,priceOut:2320000,note:""},
  {region:"경기 의왕",name:"의왕하늘쉼터",type:"수목장",unit:"공동(12위)",status:"여유 있음",condIn:"1년이상 주민등록 의왕시",condOut:"배우자·직계가족",period:"45년",priceIn:1840000,priceOut:3680000,note:"개인8기/부부10기/가족4기"},
  // ── 경기 양주 ──
  {region:"경기 양주",name:"경신하늘뜰공원",type:"봉안당",unit:"개인단",status:"여유 있음",condIn:"1년이상 주민등록한 사실이 있는 경우",condOut:"이미 안치된 자의 배우자·자녀·부모",period:"30년",priceIn:500000,priceOut:1000000,note:""},
  {region:"경기 양주",name:"경신하늘뜰공원",type:"봉안당",unit:"부부단",status:"만장",condIn:"1년이상 주민등록한 사실이 있는 경우",condOut:"이미 안치된 자의 배우자·자녀·부모",period:"30년",priceIn:1000000,priceOut:2000000,note:""},
  {region:"경기 양주",name:"경신하늘뜰공원",type:"잔디장",unit:"개인장",status:"만장",condIn:"1년이상 주민등록한 사실이 있는 경우",condOut:"이미 안치된 자의 배우자·자녀·부모",period:"30년",priceIn:400000,priceOut:600000,note:""},
  // ── 경기 이천 ──
  {region:"경기 이천",name:"이천시립 추모의집",type:"봉안당",unit:"개인단",status:"여유 있음",condIn:"6개월이상 주민등록 이천시",condOut:"배우자·자녀·부모",period:"45년",priceIn:350000,priceOut:525000,note:""},
  {region:"경기 이천",name:"이천시립 추모의집",type:"봉안당",unit:"부부단",status:"여유 있음",condIn:"6개월이상 주민등록 이천시",condOut:"배우자·자녀·부모",period:"45년",priceIn:600000,priceOut:900000,note:""},
  {region:"경기 이천",name:"이천시립 자연장지",type:"잔디장",unit:"개인장",status:"여유 있음",condIn:"6개월이상 주민등록 이천시",condOut:"배우자·자녀·부모",period:"50년",priceIn:300000,priceOut:450000,note:"중도 반출 불가"},
  // ── 경기 성남 ──
  {region:"경기 성남",name:"성남장례문화사업소 하늘누리",type:"봉안당",unit:"개인단",status:"여유 있음 (2추모관 만장)",condIn:"1년이상 주민등록 성남시",condOut:"배우자·부모·자녀",period:"30년",priceIn:100000,priceOut:1000000,note:"2추모관 만장"},
  {region:"경기 성남",name:"성남장례문화사업소 하늘누리",type:"봉안당",unit:"부부단",status:"만장",condIn:"1년이상 주민등록 성남시",condOut:"배우자·부모·자녀",period:"30년",priceIn:200000,priceOut:2000000,note:""},
  // ── 경기 동두천 ──
  {region:"경기 동두천",name:"안흥동공설묘지(봉안시설)",type:"봉안담",unit:"개인담",status:"여유 있음",condIn:"1개월이상 주민등록 동두천시",condOut:"배우자·부모·자녀",period:"60년",priceIn:695000,priceOut:1370000,note:""},
  {region:"경기 동두천",name:"안흥동공설묘지(봉안시설)",type:"봉안담",unit:"부부담",status:"여유 있음",condIn:"1개월이상 주민등록 동두천시",condOut:"배우자·부모·자녀",period:"60년",priceIn:1290000,priceOut:2540000,note:""},
  // ── 경기 가평 ──
  {region:"경기 가평",name:"가평추모공원",type:"봉안담",unit:"개인담",status:"1년 이상 가능",condIn:"6개월이상 주민등록 가평군",condOut:"배우자·부모·자녀",period:"30년",priceIn:500000,priceOut:1000000,note:""},
  {region:"경기 가평",name:"가평추모공원",type:"봉안담",unit:"부부담",status:"1년 이상 가능",condIn:"6개월이상 주민등록 가평군",condOut:"",period:"30년",priceIn:750000,priceOut:1500000,note:""},
  {region:"경기 가평",name:"가평추모공원",type:"잔디장",unit:"개인장",status:"1년 이상 가능",condIn:"6개월이상 주민등록 가평군",condOut:"",period:"30년",priceIn:350000,priceOut:700000,note:""},
  {region:"경기 가평",name:"가평추모공원",type:"잔디장",unit:"부부장",status:"1년 이상 가능",condIn:"6개월이상 주민등록 가평군",condOut:"",period:"30년",priceIn:525000,priceOut:1050000,note:""},
  // ── 경기 광주 ──
  {region:"경기 광주",name:"중대 공원 자연장지",type:"잔디장",unit:"개인장",status:"5~6월 정도 만장",condIn:"사망당시 주민등록",condOut:"직계 자녀 6개월이상 거주",period:"30년",priceIn:300000,priceOut:450000,note:""},
  {region:"경기 광주",name:"신월 공설 자연장지",type:"잔디장",unit:"개인장",status:"여유 있음",condIn:"사망당시 주민등록",condOut:"직계 자녀 6개월이상 거주",period:"30년",priceIn:300000,priceOut:450000,note:""},
  // ── 경기 시흥 ──
  {region:"경기 시흥",name:"정왕 공설 자연장지",type:"잔디장",unit:"개인장",status:"여유 있음",condIn:"3개월이상 주민등록 시흥시",condOut:"부부합장",period:"30년",priceIn:300000,priceOut:0,note:""},
  {region:"경기 시흥",name:"정왕 공설 자연장지",type:"잔디장",unit:"부부장",status:"만장",condIn:"3개월이상 주민등록 시흥시",condOut:"부부합장",period:"",priceIn:600000,priceOut:0,note:""},
  // ── 경기 포천 ──
  {region:"경기 포천",name:"내촌 공설 자연장지",type:"잔디장",unit:"개인장",status:"여유 있음",condIn:"6개월이상 주민등록 포천시",condOut:"배우자·부모·자녀",period:"30년",priceIn:400000,priceOut:600000,note:"개인장에 합장 가능"},
  // ── 경기 양평·구리 (만장/신설예정) ──
  {region:"경기 양평",name:"양평군공설봉안시설",type:"-",unit:"-",status:"만장 신설예정 26~27년",condIn:"",condOut:"",period:"",priceIn:0,priceOut:0,note:"신설 예정"},
  {region:"경기 구리",name:"구리시공설묘지 봉안묘",type:"-",unit:"-",status:"만장 (신규 안치 불가)",condIn:"무연고 봉안당만 있음",condOut:"",period:"",priceIn:0,priceOut:0,note:"일반인 안치 불가"},
  // ── 인천 ──
  {region:"인천",name:"인천가족공원",type:"봉안담",unit:"개인담",status:"여유 있음",condIn:"6개월이상 주민등록 인천시",condOut:"30년이상 인천시 거주 사망자의 부모·자녀·배우자",period:"30년",priceIn:850000,priceOut:0,note:""},
  {region:"인천",name:"인천가족공원",type:"봉안담",unit:"부부담",status:"여유 있음",condIn:"6개월이상 주민등록 인천시",condOut:"30년이상 인천시 거주 사망자의 부모·자녀·배우자",period:"30년",priceIn:1400000,priceOut:0,note:"한번에 같이 모셔야 함"},
  {region:"인천",name:"인천가족공원",type:"봉안담",unit:"가족(8위)",status:"여유 있음",condIn:"6개월이상 주민등록 인천시",condOut:"30년이상 인천시 거주 사망자의 부모·자녀·배우자",period:"90년",priceIn:10170000,priceOut:0,note:""},
  {region:"인천",name:"인천가족공원",type:"봉안담",unit:"가족(12위)",status:"여유 있음",condIn:"6개월이상 주민등록 인천시",condOut:"30년이상 인천시 거주 사망자의 부모·자녀·배우자",period:"90년",priceIn:15260000,priceOut:0,note:""},
  {region:"인천",name:"인천가족공원",type:"수목장",unit:"개인장",status:"만장",condIn:"6개월이상 주민등록 인천시",condOut:"시 소재 병원·요양시설 입원 중 사망자",period:"30년",priceIn:900000,priceOut:0,note:""},
  {region:"인천",name:"인천가족공원",type:"잔디장",unit:"개인장",status:"여유 있음",condIn:"",condOut:"",period:"30년",priceIn:900000,priceOut:0,note:"별빛 정원 잔디장만 운영"},
  {region:"인천",name:"인천가족공원",type:"수목장",unit:"개인장(정원식)",status:"여유 있음",condIn:"",condOut:"",period:"30년",priceIn:1300000,priceOut:0,note:"별빛정원 수목장만 운영"},
  // ── 인천 도서 지역 (해당 주민만 가능) ──
  {region:"인천 옹진",name:"연평리공설묘지 자연장지",type:"자연장지",unit:"-",status:"연평리 주민만 가능",condIn:"연평리 주민만 가능",condOut:"",period:"",priceIn:0,priceOut:0,note:"도서 주민 한정"},
  {region:"인천 옹진",name:"이작리공설묘지 자연장지",type:"자연장지",unit:"-",status:"이작리 주민만 가능",condIn:"이작리 주민만 가능",condOut:"",period:"",priceIn:0,priceOut:0,note:"도서 주민 한정"},
  {region:"인천 옹진",name:"선재리공설묘지 자연장지",type:"자연장지",unit:"-",status:"선재리 주민만 가능",condIn:"선재리 주민만 가능",condOut:"",period:"",priceIn:0,priceOut:0,note:"도서 주민 한정"},
  {region:"인천 옹진",name:"장봉리공설묘지 자연장지",type:"자연장지",unit:"-",status:"장봉리 주민만 가능",condIn:"장봉리 주민만 가능",condOut:"",period:"",priceIn:0,priceOut:0,note:"도서 주민 한정"},
  {region:"인천 강화",name:"월곳리 공설자연장지",type:"자연장지",unit:"-",status:"월곳리 주민만 가능",condIn:"월곳리 주민만 가능",condOut:"",period:"",priceIn:0,priceOut:0,note:"도서 주민 한정"},
  {region:"인천 강화",name:"강화해누리공원",type:"잔디장",unit:"개인단",status:"여유 있음",condIn:"3년 이전부터 강화도 주민등록자",condOut:"",period:"45년",priceIn:3000000,priceOut:0,note:""},
].map(r=>({...r, kind:"공설", fullYn: r.status.includes('만장')?'O':'X'}));

// ═══════════════════════════════════════════════════════════════════
// ── 사설 장지 데이터 (CSV 원본 수도권 3~151행 완전 반영, 검증 완료) ──
// ═══════════════════════════════════════════════════════════════════
const PRIVATE = [
  // ── 서울 ──
  {kind:"사설비제휴",region:"서울",name:"관음사 미타전",type:"봉안당",fee:"X",note:""},
  {kind:"사설비제휴",region:"서울",name:"봉원사",type:"봉안당",fee:"X",note:""},
  {kind:"사설비제휴",region:"서울",name:"정각사",type:"봉안당",fee:"X",note:""},
  // ── 인천 ──
  {kind:"사설제휴",region:"인천 강화",name:"강화파라다이스 추모원",type:"봉안당",fee:"30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"인천 강화",name:"강화파라다이스 추모원",type:"야외 봉안담",fee:"10%",note:"부가세 포함"},
  {kind:"사설비제휴",region:"인천 강화",name:"강화파라다이스 추모원",type:"봉안묘",fee:"X",note:""},
  {kind:"사설제휴",region:"인천",name:"약사사",type:"봉안당",fee:"25%",note:"세금계산서 미발행"},
  {kind:"사설제휴",region:"인천",name:"푸른바다장",type:"해양장",fee:"24/26/29만원",note:""},
  {kind:"사설제휴",region:"인천",name:"푸른바다장",type:"제사상품",fee:"5만/5만/10만",note:""},
  {kind:"사설제휴",region:"인천",name:"현대유람선",type:"해양장",fee:"23/26/29만원",note:"부가세 별도"},
  {kind:"사설제휴",region:"인천",name:"현대유람선",type:"제사상품",fee:"5만/5만/10만",note:"부가세 포함"},
  {kind:"사설제휴",region:"인천",name:"흥륜사 정토원",type:"봉안당",fee:"40%",note:"부가세 별도 / 1층"},
  {kind:"사설제휴",region:"인천",name:"흥륜사 정토원",type:"봉안당",fee:"30%",note:"부가세 별도 / 2층"},
  {kind:"사설비제휴",region:"인천",name:"흥창사 서락원",type:"봉안당",fee:"X",note:""},
  // ── 경기 고양 ──
  {kind:"사설제휴",region:"경기 고양",name:"공감수목장",type:"수목장",fee:"30%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 고양",name:"벽제중앙추모공원",type:"봉안당",fee:"30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 고양",name:"연화추모공원",type:"봉안당",fee:"30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 고양",name:"예원추모관",type:"봉안당",fee:"30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 고양",name:"유일추모공원",type:"봉안당",fee:"30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 고양",name:"일산 푸른솔공원",type:"봉안당",fee:"30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 고양",name:"자연애숲수목장",type:"수목장",fee:"30%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 고양",name:"자하연 일산",type:"봉안묘",fee:"120~190만원",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 고양",name:"자하연 일산",type:"평장묘",fee:"120만원",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 고양",name:"자하연 일산",type:"매장묘",fee:"140만원",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 고양",name:"자하연 일산",type:"야외 봉안담",fee:"30%+10%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 고양",name:"장안추모공원",type:"봉안당",fee:"30%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 고양",name:"추모공원 하늘문",type:"봉안당",fee:"20~30%",note:"부가세 포함"},
  {kind:"사설비제휴",region:"경기 고양",name:"청아공원",type:"봉안당",fee:"X",note:""},
  // ── 경기 파주 ──
  {kind:"사설비제휴",region:"경기 파주",name:"검단사",type:"봉안당",fee:"X",note:""},
  {kind:"사설비제휴",region:"경기 파주",name:"보광사",type:"봉안당",fee:"X",note:""},
  {kind:"사설제휴",region:"경기 파주",name:"서현추모공원",type:"봉안당",fee:"30%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 파주",name:"통일로추모공원",type:"봉안당",fee:"40%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 파주",name:"파주추모공원",type:"봉안당",fee:"30%",note:"부가세 별도"},
  {kind:"사설비제휴",region:"경기 파주",name:"해방공원묘원",type:"매장묘",fee:"X",note:""},
  {kind:"사설비제휴",region:"경기 파주",name:"해방공원묘원",type:"평장묘",fee:"X",note:""},
  {kind:"사설비제휴",region:"경기 파주",name:"해방공원묘원",type:"봉안묘",fee:"X",note:""},
  // ── 경기 양주 ──
  {kind:"사설제휴",region:"경기 양주",name:"대원정사",type:"봉안당",fee:"30%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 양주",name:"삼성엘리시움(양주)",type:"매장묘",fee:"150만원",note:""},
  {kind:"사설제휴",region:"경기 양주",name:"삼성엘리시움(양주)",type:"봉안묘",fee:"7%",note:""},
  {kind:"사설제휴",region:"경기 양주",name:"삼성엘리시움(양주)",type:"평장묘",fee:"100만원",note:"2위 평장묘 수수료"},
  {kind:"사설제휴",region:"경기 양주",name:"신세계공원묘원",type:"평장묘",fee:"10%",note:"부가세 별도"},
  {kind:"사설비제휴",region:"경기 양주",name:"신세계공원묘원",type:"수목장",fee:"X",note:""},
  {kind:"사설제휴",region:"경기 양주",name:"신세계공원묘원",type:"매장묘",fee:"10%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 양주",name:"신세계공원묘원",type:"봉안묘",fee:"10%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 양주",name:"양주추모공원",type:"봉안당",fee:"40%",note:"부가세 포함"},
  {kind:"사설비제휴",region:"경기 양주",name:"장흥수목장(대원정사)",type:"수목장",fee:"X",note:""},
  {kind:"사설비제휴",region:"경기 양주",name:"청련사",type:"봉안당",fee:"X",note:""},
  {kind:"사설제휴",region:"경기 양주",name:"하늘계단수목장",type:"수목장",fee:"10%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 양주",name:"하늘계단수목장",type:"잔디장",fee:"10%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 양주",name:"하늘소풍수목장",type:"수목장",fee:"20%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 양주",name:"하늘소풍수목장",type:"잔디장",fee:"20%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 양주",name:"하늘소풍수목장",type:"봉안당",fee:"20%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 양주",name:"하늘안 추모공원",type:"봉안당",fee:"30%",note:"부가세 포함"},
  // ── 경기 남양주 ──
  {kind:"사설제휴",region:"경기 남양주",name:"무량수목장",type:"수목장/잔디장",fee:"30%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 남양주",name:"에덴추모공원",type:"봉안당",fee:"35%",note:"부가세 별도"},
  // ── 경기 연천 ──
  {kind:"사설제휴",region:"경기 연천",name:"더유일추모공원",type:"봉안당",fee:"35%",note:""},
  {kind:"사설제휴",region:"경기 연천",name:"더유일추모공원",type:"수목장",fee:"35%",note:""},
  {kind:"사설제휴",region:"경기 연천",name:"동막골 추모공원",type:"봉안당",fee:"30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 연천",name:"연천중앙추모공원",type:"봉안당",fee:"30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 연천",name:"연천중앙추모공원",type:"수목장/잔디장",fee:"30%",note:"부가세 포함"},
  // ── 경기 포천 ──
  {kind:"사설비제휴",region:"경기 포천",name:"다보정사",type:"봉안당",fee:"X",note:""},
  {kind:"사설제휴",region:"경기 포천",name:"도성사",type:"봉안당",fee:"30%",note:"부가세 별도 / 포천시민 10% 할인"},
  {kind:"사설제휴",region:"경기 포천",name:"광릉더크레스트",type:"봉안당",fee:"30%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 포천",name:"광릉추모공원",type:"수목장",fee:"15%",note:"부가세 별도"},
  {kind:"사설비제휴",region:"경기 포천",name:"광릉추모공원",type:"봉안묘",fee:"X",note:""},
  {kind:"사설비제휴",region:"경기 포천",name:"광릉추모공원",type:"매장묘",fee:"X",note:""},
  {kind:"사설비제휴",region:"경기 포천",name:"광릉추모공원",type:"평장묘",fee:"X",note:""},
  {kind:"사설제휴",region:"경기 포천",name:"자하연 포천",type:"봉안묘",fee:"140만원",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 포천",name:"자하연 포천",type:"매장묘",fee:"120~190만원",note:"부가세 별도"},
  // ── 경기 동두천 ──
  {kind:"사설비제휴",region:"경기 동두천",name:"탑동추모공원",type:"봉안당",fee:"X",note:""},
  {kind:"사설제휴",region:"경기 동두천",name:"예래원",type:"잔디장",fee:"12%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 동두천",name:"예래원",type:"봉안묘",fee:"12%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 동두천",name:"예래원",type:"매장묘",fee:"12%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 동두천",name:"예래원",type:"봉안담",fee:"20%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 동두천",name:"이담추모관",type:"봉안당",fee:"30%",note:"부가세 포함"},
  // ── 경기 성남 ──
  {kind:"사설제휴",region:"경기 성남",name:"봉안당 홈",type:"봉안당",fee:"25%",note:"부가세 별도"},
  {kind:"사설비제휴",region:"경기 성남",name:"분당메모리얼파크",type:"봉안담",fee:"X",note:""},
  {kind:"사설비제휴",region:"경기 성남",name:"분당메모리얼파크",type:"봉안묘",fee:"X",note:""},
  // ── 경기 광주 ──
  {kind:"사설제휴",region:"경기 광주",name:"광주공원묘원",type:"매장묘",fee:"10%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 광주",name:"광주공원묘원",type:"봉안묘",fee:"10%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 광주",name:"남서울가족공원",type:"봉안당",fee:"40%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 광주",name:"남서울가족공원",type:"수목장/잔디장",fee:"30%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 광주",name:"남한강공원묘원",type:"매장묘",fee:"5%",note:""},
  {kind:"사설비제휴",region:"경기 광주",name:"남한강공원묘원",type:"봉안묘",fee:"X",note:""},
  {kind:"사설제휴",region:"경기 광주",name:"미타정사",type:"봉안당",fee:"30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 광주",name:"분당스카이캐슬 추모공원",type:"봉안당",fee:"25%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 광주",name:"분당추모공원 휴",type:"봉안당",fee:"30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 광주",name:"분당추모공원 휴",type:"수목장",fee:"30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 광주",name:"분당추모공원 휴",type:"봉안담",fee:"40%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 광주",name:"삼성엘리시움(광주)",type:"매장묘",fee:"120~300만원",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 광주",name:"삼성엘리시움(광주)",type:"봉안묘",fee:"120~300만원",note:"부가세 별도"},
  {kind:"사설비제휴",region:"경기 광주",name:"시안공원",type:"봉안담",fee:"X",note:""},
  {kind:"사설비제휴",region:"경기 광주",name:"시안공원",type:"봉안묘",fee:"X",note:""},
  {kind:"사설제휴",region:"경기 광주",name:"자하연 분당",type:"봉안묘",fee:"120~190만원",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 광주",name:"자하연 분당",type:"평장묘",fee:"120만원",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 광주",name:"자하연 분당",type:"야외 봉안담",fee:"30%+10%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 광주",name:"자하연 분당",type:"매장묘",fee:"140만원",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 광주",name:"한남공원묘원",type:"봉안묘",fee:"6~8%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 광주",name:"한남공원묘원",type:"매장묘",fee:"6~8%",note:"부가세 별도"},
  // ── 경기 용인 ──
  {kind:"사설제휴",region:"경기 용인",name:"금오동지",type:"수목장",fee:"30%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 용인",name:"부활의 아침 추모관",type:"봉안당",fee:"30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 용인",name:"서울공원묘원",type:"매장묘",fee:"100~120만원",note:"부가세 별도 / 단장묘100·쌍묘합장120"},
  {kind:"사설제휴",region:"경기 용인",name:"서울공원묘원",type:"평장묘",fee:"7%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 용인",name:"서울공원묘원",type:"봉안묘",fee:"7%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 용인",name:"양지수목장",type:"수목장",fee:"30%",note:"개인(세금계산서 미발행)"},
  {kind:"사설제휴",region:"경기 용인",name:"용인공원 아너스톤",type:"봉안당",fee:"30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 용인",name:"용인공원묘원",type:"수목장",fee:"10%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 용인",name:"용인공원묘원",type:"봉안묘",fee:"150~200만원",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 용인",name:"용인공원묘원",type:"매장묘",fee:"200만원",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 용인",name:"용인공원묘원",type:"봉안담",fee:"30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 용인",name:"용인로뎀파크",type:"수목장/잔디장",fee:"11.7~25%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 용인",name:"용인추모원",type:"봉안당",fee:"30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 용인",name:"용인추모원",type:"봉안당(단기임대)",fee:"20%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 용인",name:"용인추모원",type:"수목장",fee:"20%",note:"부가세 포함"},
  // ── 경기 양평 ──
  {kind:"사설제휴",region:"경기 양평",name:"가람추모관",type:"봉안당",fee:"40%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 양평",name:"갈월공원",type:"수목장",fee:"20~30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 양평",name:"갈월공원",type:"봉안당",fee:"40%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 양평",name:"갈월공원",type:"봉안묘",fee:"10%",note:"부가세 포함"},
  {kind:"사설비제휴",region:"경기 양평",name:"갑산공원묘원",type:"매장묘",fee:"X",note:""},
  {kind:"사설비제휴",region:"경기 양평",name:"갑산공원묘원",type:"봉안묘",fee:"X",note:""},
  {kind:"사설제휴",region:"경기 양평",name:"갑산공원묘원",type:"봉안담",fee:"40만원 정액",note:""},
  {kind:"사설제휴",region:"경기 양평",name:"무궁화공원묘원",type:"수목장",fee:"10~40%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 양평",name:"무궁화공원묘원",type:"봉안묘",fee:"10%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 양평",name:"무궁화공원묘원",type:"매장묘",fee:"10%",note:"부가세 포함"},
  {kind:"사설비제휴",region:"경기 양평",name:"북한강 광명수목장",type:"수목장",fee:"X",note:""},
  {kind:"사설제휴",region:"경기 양평",name:"양평추모공원더포레",type:"봉안당",fee:"30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 양평",name:"예진추모관",type:"봉안당",fee:"35%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 양평",name:"자하연 팔당",type:"매장묘",fee:"140만원",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 양평",name:"자하연 팔당",type:"봉안묘",fee:"120~190만원",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 양평",name:"자하연 팔당",type:"자연장(평장)",fee:"120만원",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 양평",name:"자하연 팔당",type:"야외 봉안담",fee:"30%+10%",note:"부가세 별도"},
  // ── 경기 여주 ──
  {kind:"사설제휴",region:"경기 여주",name:"여주세종추모공원",type:"봉안당",fee:"30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 여주",name:"여주세종추모공원",type:"수목장",fee:"20%",note:"부가세 포함"},
  // ── 경기 안성 ──
  {kind:"사설비제휴",region:"경기 안성",name:"대림동산 대원사",type:"봉안당",fee:"X",note:""},
  {kind:"사설제휴",region:"경기 안성",name:"에버그린 수목장",type:"수목장",fee:"20~30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 안성",name:"유토피아추모관",type:"봉안당",fee:"40%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 안성",name:"유토피아추모관",type:"수목장",fee:"30%",note:"부가세 포함"},
  // ── 경기 가평 ──
  {kind:"사설제휴",region:"경기 가평",name:"성불사 자인당",type:"봉안당",fee:"50%",note:"부가세 포함"},
  // ── 경기 평택 ──
  {kind:"사설제휴",region:"경기 평택",name:"서호추모공원",type:"봉안당",fee:"30%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 평택",name:"서호추모공원",type:"수목장",fee:"20%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 평택",name:"서호추모공원",type:"잔디장",fee:"30%",note:"부가세 별도"},
  // ── 경기 김포 ──
  {kind:"사설비제휴",region:"경기 김포",name:"김포 문수산수목장",type:"수목장",fee:"X",note:""},
  {kind:"사설비제휴",region:"경기 김포",name:"김포 애기봉수목장",type:"수목장",fee:"X",note:"수수료 미확인"},
  {kind:"사설제휴",region:"경기 김포",name:"청솔수목장",type:"수목장",fee:"25~30%",note:"부가세 별도"},
  {kind:"사설제휴",region:"경기 김포",name:"청솔수목장",type:"잔디장",fee:"30%",note:"부가세 별도"},
  // ── 경기 안산 ──
  {kind:"사설비제휴",region:"경기 안산",name:"쌍계사 수목장",type:"수목장",fee:"X",note:""},
  // ── 경기 의왕 ──
  {kind:"사설제휴",region:"경기 의왕",name:"오봉정사(석예추모관)",type:"봉안당",fee:"30%",note:"부가세 포함"},
  // ── 경기 화성 ──
  {kind:"사설제휴",region:"경기 화성",name:"하늘공원 수목장림",type:"수목장",fee:"30%",note:"부가세 포함"},
  {kind:"사설제휴",region:"경기 화성",name:"화성 나래원",type:"수목장",fee:"40%",note:"부가세 포함"},
].map(r=>({...r, unit:"-", fullYn:"X", status:"-", condIn:"", condOut:"", period:"", priceIn:0, priceOut:0}));

// ═══════════════════════════════════════
const ALL = [...PUBLIC,...PRIVATE].map(r=>({
  ...r,
  statusLevel: getStatusLevel(r),
  periodNum: parseInt(r.period)||0
}));

function getStatusLevel(r) {
  if(r.kind==='사설제휴'||r.kind==='사설비제휴') return 'na';
  const s=(r.status||'').toLowerCase();
  if(r.fullYn==='O'||s.includes('만장')||s.includes('불가')) return 'full';
  if(s.includes('몇개월')||s.includes('5~6개월')||s.includes('5개월')||s.includes('26년도')||s.includes('5~6월')||s.includes('임박')) return 'warn';
  return 'ok';
}
function getMainRegion(r){
  if(r.startsWith('서울')) return '서울';
  if(r.startsWith('인천')) return '인천';
  return '경기';
}
function getTypeClass(t){
  if(!t) return 't-기타';
  const tl=t.replace(/[\s\/\(\)]/g,'').toLowerCase();
  if(tl.includes('야외봉안담')) return 't-야외봉안담';
  if(tl.includes('봉안당')) return 't-봉안당';
  if(tl.includes('봉안담')) return 't-봉안담';
  if(tl.includes('봉안묘')) return 't-봉안묘';
  if(tl.includes('잔디장')) return 't-잔디장';
  if(tl.includes('수목장')) return 't-수목장';
  if(tl.includes('매장묘')) return 't-매장묘';
  if(tl.includes('자연장지')) return 't-자연장';
  if(tl.includes('평장묘')||tl.includes('자연장')) return 't-자연장';
  if(tl.includes('해양장')) return 't-해양장';
  if(tl.includes('제사')) return 't-제사상품';
  return 't-기타';
}
function fmtPrice(v){
  if(!v||v===0) return '<span class="price-na">-</span>';
  if(v>=10000000) return (v/10000000).toFixed(1).replace(/\.0$/,'')+'<span class="price-unit">천만원</span>';
  return (v/10000).toLocaleString()+'<span class="price-unit">만원</span>';
}

let filters={kind:'',region:'',status:'',type:''};
let sortF='',sortAsc=true;

function applyFilters(){
  const q=document.getElementById('searchInput').value.trim().toLowerCase();
  let rows=ALL.filter(r=>{
    if(filters.kind&&r.kind!==filters.kind) return false;
    if(filters.region&&getMainRegion(r.region)!==filters.region) return false;
    if(filters.status&&r.statusLevel!==filters.status) return false;
    if(filters.type&&!r.type.includes(filters.type)) return false;
    if(q&&![r.name,r.region,r.type,r.unit||'',r.condIn||'',r.condOut||'',r.fee||'',r.note||''].join(' ').toLowerCase().includes(q)) return false;
    return true;
  });
  if(sortF){
    rows.sort((a,b)=>{
      let av=a[sortF],bv=b[sortF];
      if(typeof av==='string') av=av.toLowerCase();
      if(typeof bv==='string') bv=bv.toLowerCase();
      return av<bv?(sortAsc?-1:1):av>bv?(sortAsc?1:-1):0;
    });
  }
  renderTable(rows);
  document.getElementById('rowCountLabel').textContent=rows.length+'개';
}

function renderTable(rows){
  const tb=document.getElementById('tableBody');
  if(!rows.length){tb.innerHTML='<tr class="empty-row"><td colspan="12">검색 결과가 없습니다.</td></tr>';return;}
  tb.innerHTML=rows.map(r=>{
    const sl=r.statusLevel;
    const dc=sl==='full'?'dot-full':sl==='warn'?'dot-warn':sl==='na'?'dot-na':'dot-ok';
    const st=r.kind==='사설제휴'?'🤝 제휴':r.kind==='사설비제휴'?'🚫 비제휴':(r.status||(r.fullYn==='O'?'만장':'여유 있음'));
    const priceCell=r.kind==='공설'?fmtPrice(r.priceIn):`<span class="price-fee">${r.fee||'-'}</span>`;
    const priceOutCell=r.kind==='공설'?fmtPrice(r.priceOut):'<span class="price-na">-</span>';
    return `<tr>
      <td><span class="kind-badge kind-${r.kind}">${r.kind==='공설'?'공설':r.kind==='사설제휴'?'사설제휴':'비제휴'}</span></td>
      <td class="cell-region">${r.region}</td>
      <td><div class="cell-name">${r.name}</div>${r.unit&&r.unit!=='-'?'<div class="cell-sub">'+r.unit+'</div>':''}</td>
      <td><span class="badge ${getTypeClass(r.type)}">${r.type||'-'}</span></td>
      <td style="font-size:.78rem;color:var(--muted)">${r.unit&&r.unit!=='-'?r.unit:'-'}</td>
      <td><span class="status"><span class="dot ${dc}"></span>${st}</span></td>
      <td class="condition-text">${r.condIn||'-'}</td>
      <td class="condition-text">${r.condOut||'-'}</td>
      <td>${r.period?'<span class="period-chip">'+r.period+'</span>':'<span style="color:var(--muted)">-</span>'}</td>
      <td class="price-col">${priceCell}</td>
      <td class="price-col">${priceOutCell}</td>
      <td class="note-text">${r.note||''}</td>
    </tr>`;
  }).join('');
}

function setFilter(key,val){
  filters[key]=val;
  const maps={
    kind:{pre:'kbtn',vals:['','공설','사설제휴','사설비제휴'],cls:['f-blue','f-blue','f-teal','f-gray']},
    region:{pre:'rbtn',vals:['','서울','인천','경기'],cls:['f-blue','f-blue','f-blue','f-blue']},
    status:{pre:'sbtn',vals:['','ok','warn','full'],cls:['f-blue','f-green','f-yellow','f-red']},
    type:{pre:'tbtn',vals:['','봉안당','봉안담','잔디장','수목장','봉안묘','매장묘'],cls:['f-blue','f-blue','f-blue','f-blue','f-blue','f-blue','f-blue']},
  };
  const m=maps[key];
  m.vals.forEach((v,i)=>{
    const el=document.getElementById(m.pre+'-'+v);
    if(el) el.className='filter-btn'+(v===val?' '+m.cls[i]:'');
  });
  applyFilters();
}

function sortBy(f){
  document.querySelectorAll('.tbl th').forEach(th=>th.classList.remove('sorted'));
  sortAsc=sortF===f?!sortAsc:true;
  sortF=f;
  applyFilters();
}

function setTheme(t){
  document.documentElement.setAttribute('data-theme',t);
  ['dark','light'].forEach(v=>document.getElementById('theme-'+v).classList.toggle('active',v===t));
  localStorage.setItem('theme',t);
}
function setSize(s){
  document.documentElement.setAttribute('data-size',s);
  ['sm','md','lg','xl'].forEach(v=>document.getElementById('size-'+v).classList.toggle('active',v===s));
  localStorage.setItem('size',s);
}

function renderStats(){
  const pub=ALL.filter(r=>r.kind==='공설');
  const prv=ALL.filter(r=>r.kind==='사설제휴');
  const non=ALL.filter(r=>r.kind==='사설비제휴');
  const ok=pub.filter(r=>r.statusLevel==='ok').length;
  const warn=pub.filter(r=>r.statusLevel==='warn').length;
  const full=pub.filter(r=>r.statusLevel==='full').length;
  const fac=[...new Set(ALL.map(r=>r.name))].length;
  document.getElementById('statsRow').innerHTML=`
    <div class="stat-card"><div class="stat-label">전체 시설 옵션</div><div class="stat-value sv-blue">${ALL.length}</div><div class="stat-sub">${fac}개 장지 기준</div></div>
    <div class="stat-card"><div class="stat-label">🏛 공설 장지</div><div class="stat-value sv-purple">${pub.length}</div><div class="stat-sub">공공 운영 장지</div></div>
    <div class="stat-card"><div class="stat-label">🤝 사설 제휴</div><div class="stat-value" style="color:var(--teal)">${prv.length}</div><div class="stat-sub">수수료 발생</div></div>
    <div class="stat-card"><div class="stat-label">🚫 사설 비제휴</div><div class="stat-value sv-gray">${non.length}</div><div class="stat-sub">수수료 없음</div></div>
    <div class="stat-card"><div class="stat-label">✅ 공설 여유</div><div class="stat-value sv-green">${ok}</div><div class="stat-sub">즉시 안치 가능</div></div>
    <div class="stat-card"><div class="stat-label">🔴 공설 만장</div><div class="stat-value sv-red">${full}</div><div class="stat-sub">신규 안치 불가</div></div>
  `;
}

// 저장된 설정 복원
(function init(){
  const t=localStorage.getItem('theme')||'dark';
  const s=localStorage.getItem('size')||'md';
  setTheme(t); setSize(s);
  renderStats(); applyFilters();
})();
</script>
</body>
</html># jangji-dashboard
