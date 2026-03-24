# workout-log<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<title>83돼지 운동일지</title>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;700;900&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #f2f2f7;
    --surface: #ffffff;
    --surface2: #f2f2f7;
    --surface3: #e5e5ea;
    --border: rgba(0,0,0,0.08);
    --accent: #ff3b30;
    --accent-blue: #007aff;
    --accent-green: #34c759;
    --accent-orange: #ff9500;
    --text: #1c1c1e;
    --text-muted: #8e8e93;
    --text-dim: #c7c7cc;
    --good: #34c759;
    --bad: #ff3b30;
    --mid: #ff9500;
    --shadow: 0 2px 20px rgba(0,0,0,0.08);
    --shadow-sm: 0 1px 8px rgba(0,0,0,0.06);
    --radius: 16px;
    --radius-sm: 12px;
  }

  * { margin:0; padding:0; box-sizing:border-box; -webkit-tap-highlight-color:transparent; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Noto Sans KR', -apple-system, sans-serif;
    min-height: 100vh;
    padding: 0 0 40px 0;
    -webkit-font-smoothing: antialiased;
  }

  .app { max-width: 480px; margin: 0 auto; }

  /* 헤더 */
  .header {
    background: rgba(255,255,255,0.85);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    padding: 52px 20px 16px;
    position: sticky;
    top: 0;
    z-index: 50;
    border-bottom: 1px solid var(--border);
  }

  .header-inner { display:flex; align-items:center; justify-content:space-between; }

  .header-left h1 {
    font-size: 28px;
    font-weight: 800;
    color: var(--text);
    letter-spacing: -0.5px;
    line-height: 1;
  }

  .header-left h1 span { color: var(--accent); }

  .header-right {
    text-align: right;
  }

  .header-date {
    font-size: 15px;
    font-weight: 600;
    color: var(--text);
  }

  .header-day {
    font-size: 12px;
    color: var(--text-muted);
    margin-top: 1px;
  }

  /* 탭 */
  .tabs {
    display: flex;
    gap: 0;
    margin: 16px 16px 0;
    background: var(--surface3);
    padding: 3px;
    border-radius: 12px;
  }

  .tab {
    flex: 1;
    padding: 7px 4px;
    border: none;
    border-radius: 9px;
    background: transparent;
    color: var(--text-muted);
    font-size: 12px;
    font-family: 'Noto Sans KR', sans-serif;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.2s;
  }

  .tab.active {
    background: white;
    color: var(--text);
    font-weight: 700;
    box-shadow: var(--shadow-sm);
  }

  .content { padding: 12px 16px 0; }
  .section { display:none; }
  .section.active { display:block; }

  /* 카드 */
  .card {
    background: var(--surface);
    border-radius: var(--radius);
    padding: 16px;
    margin-bottom: 12px;
    box-shadow: var(--shadow-sm);
  }

  .card-title {
    font-size: 13px;
    font-weight: 700;
    color: var(--text-muted);
    letter-spacing: 0.3px;
    text-transform: uppercase;
    margin-bottom: 12px;
  }

  /* 인풋 */
  .input-group { margin-bottom: 12px; }
  .input-group:last-child { margin-bottom: 0; }
  .input-group label { display:block; font-size:13px; font-weight:500; color:var(--text-muted); margin-bottom:6px; }

  input[type="date"], input[type="text"], input[type="number"], textarea, select {
    width: 100%;
    background: var(--surface2);
    border: none;
    border-radius: 10px;
    padding: 11px 13px;
    color: var(--text);
    font-size: 15px;
    font-family: 'Noto Sans KR', sans-serif;
    outline: none;
    transition: background 0.15s;
    -webkit-appearance: none;
    appearance: none;
    box-sizing: border-box;
    display: block;
    min-width: 0;
  }

  input:focus, textarea:focus { background: #ebebf0; }
  textarea { resize:none; height:60px; line-height:1.5; }
  input[type="date"]::-webkit-calendar-picker-indicator { opacity: 0.4; }

  /* 컨디션 */
  .condition-btns { display:flex; gap:8px; }
  .cond-btn {
    flex: 1;
    padding: 10px 4px;
    border: 2px solid transparent;
    border-radius: 12px;
    background: var(--surface2);
    font-size: 22px;
    cursor: pointer;
    transition: all 0.15s;
    text-align: center;
  }
  .cond-btn.selected-good { border-color: var(--good); background: rgba(52,199,89,0.1); }
  .cond-btn.selected-mid  { border-color: var(--mid);  background: rgba(255,149,0,0.1); }
  .cond-btn.selected-bad  { border-color: var(--bad);  background: rgba(255,59,48,0.1); }

  /* 즐겨찾기 칩 */
  .fav-chips { display:flex; flex-wrap:wrap; gap:6px; margin-bottom:6px; }

  .fav-chip {
    background: var(--surface2);
    border: 1.5px solid transparent;
    border-radius: 20px;
    padding: 6px 12px;
    font-size: 13px;
    font-weight: 500;
    color: var(--text-muted);
    cursor: pointer;
    transition: all 0.15s;
    user-select: none;
    display: flex;
    align-items: center;
    gap: 4px;
  }

  .fav-chip:active { transform: scale(0.95); }
  .fav-chip:hover  { border-color: var(--accent-blue); color: var(--accent-blue); }
  .fav-chip.added  { background: rgba(0,122,255,0.1); border-color: var(--accent-blue); color: var(--accent-blue); font-weight:600; }
  .chip-x { color: var(--text-dim); font-size:14px; }
  .chip-x:hover { color: var(--bad); }

  .fav-add-row { display:flex; gap:8px; margin-top:10px; }
  .fav-add-row input { flex:1; }
  .fav-add-btn {
    background: var(--accent-blue);
    border: none;
    border-radius: 10px;
    color: white;
    font-size: 14px;
    font-weight: 600;
    padding: 0 16px;
    cursor: pointer;
    font-family: 'Noto Sans KR', sans-serif;
    white-space: nowrap;
    transition: opacity 0.15s;
  }
  .fav-add-btn:active { opacity: 0.8; }

  /* 운동 종목 */
  .exercise-list { display:flex; flex-direction:column; gap:10px; }

  .exercise-item {
    background: var(--surface2);
    border-radius: var(--radius-sm);
    padding: 13px;
    animation: slideIn 0.2s ease;
  }

  @keyframes slideIn {
    from { opacity:0; transform:translateY(-8px); }
    to   { opacity:1; transform:translateY(0); }
  }

  .exercise-header { display:flex; align-items:center; gap:8px; margin-bottom:11px; }

  .exercise-name {
    flex: 1;
    background: transparent;
    border: none;
    border-bottom: 1.5px solid var(--surface3);
    border-radius: 0;
    padding: 4px 0;
    font-size: 15px;
    font-weight: 700;
    color: var(--text);
    outline: none;
  }

  .exercise-name:focus { border-bottom-color: var(--accent-blue); }

  .remove-btn {
    background: rgba(255,59,48,0.1);
    border: none;
    border-radius: 50%;
    color: var(--bad);
    font-size: 14px;
    font-weight: 700;
    cursor: pointer;
    width: 26px;
    height: 26px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background 0.15s;
  }

  .remove-btn:active { background: rgba(255,59,48,0.2); }

  .sets-header { display:grid; grid-template-columns:26px 1fr 1fr 26px; gap:5px; margin-bottom:5px; }
  .sets-header span { font-size:11px; color:var(--text-dim); text-align:center; font-weight:500; }

  .set-row { display:grid; grid-template-columns:26px 1fr 1fr 26px; gap:5px; align-items:center; margin-bottom:5px; }

  .set-num {
    width: 26px; height: 26px;
    background: white;
    border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    font-size: 11px; color: var(--text-muted); font-weight: 700;
    flex-shrink: 0;
    box-shadow: var(--shadow-sm);
  }

  .set-input {
    padding: 7px 6px !important;
    font-size: 14px !important;
    text-align: center;
    background: white !important;
    border-radius: 8px !important;
  }

  .remove-set {
    background: none; border: none;
    color: var(--text-dim);
    cursor: pointer; font-size: 14px;
    padding: 0; transition: color 0.15s;
    text-align: center;
  }
  .remove-set:hover { color: var(--bad); }

  .add-set-btn {
    display: flex; align-items: center; justify-content: center; gap: 4px;
    background: white;
    border: 1.5px dashed var(--text-dim);
    border-radius: 8px;
    color: var(--text-muted);
    font-size: 12px; font-weight: 500;
    font-family: 'Noto Sans KR', sans-serif;
    padding: 6px; cursor: pointer;
    margin-top: 6px; transition: all 0.15s; width: 100%;
  }

  .add-set-btn:hover { border-color: var(--accent-blue); color: var(--accent-blue); }

  .add-exercise-btn {
    width: 100%; padding: 13px;
    background: rgba(0,122,255,0.08);
    border: 1.5px dashed rgba(0,122,255,0.4);
    border-radius: var(--radius-sm);
    color: var(--accent-blue);
    font-size: 14px; font-weight: 600;
    font-family: 'Noto Sans KR', sans-serif;
    cursor: pointer; transition: all 0.15s; margin-top: 4px;
  }
  .add-exercise-btn:active { background: rgba(0,122,255,0.15); }

  .save-btn {
    width: 100%; padding: 15px;
    background: var(--accent-blue);
    border: none; border-radius: var(--radius);
    color: white; font-size: 16px; font-weight: 700;
    font-family: 'Noto Sans KR', sans-serif;
    cursor: pointer; transition: all 0.15s;
    margin-top: 6px;
    box-shadow: 0 4px 16px rgba(0,122,255,0.3);
  }
  .save-btn:active { opacity: 0.85; transform: scale(0.99); }

  /* 기록 목록 */
  .log-empty {
    text-align: center; padding: 60px 0;
    color: var(--text-dim); font-size: 14px; line-height: 2.5;
  }
  .log-empty .icon { font-size: 40px; margin-bottom: 10px; display:block; }

  .log-item {
    background: var(--surface);
    border-radius: var(--radius);
    padding: 15px;
    margin-bottom: 10px;
    box-shadow: var(--shadow-sm);
    cursor: pointer;
    transition: transform 0.15s;
  }
  .log-item:active { transform: scale(0.99); }

  .log-item-header { display:flex; align-items:center; justify-content:space-between; margin-bottom:9px; }
  .log-date { font-size:15px; font-weight:700; }
  .log-exercises { display:flex; flex-wrap:wrap; gap:5px; margin-bottom:7px; }
  .log-tag {
    background: var(--surface2);
    border-radius: 6px; padding: 3px 9px;
    font-size: 12px; font-weight: 500; color: var(--text-muted);
  }
  .log-memo {
    font-size: 13px; color: var(--text-muted);
    border-top: 1px solid var(--border); padding-top: 8px;
    margin-top: 5px; line-height: 1.5;
  }
  .log-delete {
    background: rgba(255,59,48,0.1); border: none;
    color: var(--bad); font-size: 12px; font-weight: 600;
    cursor: pointer; padding: 4px 9px; border-radius: 7px;
    transition: background 0.15s;
  }
  .log-delete:active { background: rgba(255,59,48,0.2); }

  /* 모달 */
  .modal-overlay {
    display: none; position: fixed; inset: 0;
    background: rgba(0,0,0,0.4);
    backdrop-filter: blur(8px);
    -webkit-backdrop-filter: blur(8px);
    z-index: 100; padding: 20px; overflow-y: auto;
  }
  .modal-overlay.show { display:flex; align-items:flex-end; justify-content:center; padding-bottom:0; }
  .modal {
    background: var(--surface);
    border-radius: 24px 24px 0 0;
    padding: 24px 20px 40px;
    width: 100%; max-width: 480px;
    animation: modalUp 0.3s cubic-bezier(0.34,1.56,0.64,1);
  }
  @keyframes modalUp { from{transform:translateY(100%)} to{transform:translateY(0)} }

  .modal-handle { width:36px; height:4px; background:var(--surface3); border-radius:2px; margin:0 auto 16px; }
  .modal-header { display:flex; justify-content:space-between; align-items:center; margin-bottom:18px; }
  .modal-date { font-size:18px; font-weight:800; }
  .modal-close {
    background: var(--surface2); border: none; border-radius: 50%;
    color: var(--text-muted); font-size:16px; font-weight:700;
    cursor: pointer; width:32px; height:32px;
    display:flex; align-items:center; justify-content:center;
  }
  .modal-exercise { margin-bottom:16px; }
  .modal-ex-name { font-size:14px; font-weight:700; color:var(--accent-blue); margin-bottom:7px; }
  .modal-sets { display:flex; flex-wrap:wrap; gap:6px; }
  .modal-set-badge {
    background: var(--surface2);
    border-radius: 8px; padding:5px 11px;
    font-size:13px; color:var(--text-muted);
  }
  .modal-set-badge span { color:var(--text); font-weight:600; }
  .modal-memo-box {
    background: var(--surface2); border-radius:12px;
    padding:12px 14px; font-size:13px; color:var(--text-muted);
    line-height:1.6; margin-top:14px;
  }

  /* 통계 */
  .stats-grid { display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-bottom:12px; }
  .stat-card {
    background: var(--surface);
    border-radius: var(--radius);
    padding: 16px; box-shadow: var(--shadow-sm); text-align:center;
  }
  .stat-num { font-size:36px; font-weight:900; color:var(--accent-blue); line-height:1; margin-bottom:5px; }
  .stat-label { font-size:12px; color:var(--text-muted); font-weight:500; }

  /* 즐겨찾기 관리 */
  .section-sub { font-size:12px; color:var(--text-muted); margin-bottom:10px; }
  .divider { height:1px; background:var(--border); margin:12px 0; }

  /* 토스트 */
  .toast {
    position: fixed; bottom: 32px; left:50%;
    transform: translateX(-50%) translateY(20px);
    background: rgba(28,28,30,0.92);
    backdrop-filter: blur(10px);
    color: white; padding: 11px 22px;
    border-radius: 22px; font-size:14px; font-weight:500;
    opacity:0; transition:all 0.3s; z-index:200; white-space:nowrap;
  }
  .toast.show { opacity:1; transform:translateX(-50%) translateY(0); }
</style>
</head>
<body>
<div class="app">

  <!-- 헤더 -->
  <div class="header">
    <div class="header-inner">
      <div class="header-left">
        <h1>🐷 <span>운동</span>일지</h1>
      </div>
      <div class="header-right">
        <div class="header-date" id="todayDate"></div>
        <div class="header-day" id="todayDay"></div>
      </div>
    </div>
    <div class="tabs">
      <button class="tab active" onclick="switchTab('write',this)">✏️ 기록</button>
      <button class="tab" onclick="switchTab('logs',this)">📋 보기</button>
      <button class="tab" onclick="switchTab('stats',this)">📊 통계</button>
      <button class="tab" onclick="switchTab('fav',this)">⭐ 즐겨찾기</button>
    </div>
  </div>

  <div class="content">

    <!-- 기록 -->
    <div class="section active" id="write">

      <div class="card">
        <div class="card-title">날짜 & 컨디션</div>
        <div class="input-group">
          <label>날짜</label>
          <input type="date" id="workoutDate">
        </div>
        <div class="input-group" style="margin-bottom:0">
          <label>오늘 컨디션</label>
          <div class="condition-btns">
            <button class="cond-btn" onclick="selectCond('good',this)" title="좋음">😤</button>
            <button class="cond-btn" onclick="selectCond('mid',this)"  title="보통">😐</button>
            <button class="cond-btn" onclick="selectCond('bad',this)"  title="피곤">😴</button>
          </div>
        </div>
      </div>

      <div class="card">
        <div class="card-title">즐겨찾기에서 추가</div>
        <div class="section-sub">탭하면 바로 운동 목록에 추가돼요</div>
        <div class="fav-chips" id="favChipsWrite"></div>
      </div>

      <div class="card">
        <div class="card-title">오늘의 운동</div>
        <div class="exercise-list" id="exerciseList"></div>
        <button class="add-exercise-btn" onclick="addExercise('')">+ 직접 종목 추가</button>
      </div>

      <div class="card">
        <div class="card-title">한줄 메모</div>
        <textarea id="workoutMemo" placeholder="오늘 운동 느낀 점, 특이사항 등..."></textarea>
      </div>

      <button class="save-btn" onclick="saveWorkout()">운동 기록 저장</button>

    </div>

    <!-- 보기 -->
    <div class="section" id="logs">
      <div id="logList"></div>
    </div>

    <!-- 통계 -->
    <div class="section" id="stats">
      <div class="stats-grid">
        <div class="stat-card"><div class="stat-num" id="statTotal">0</div><div class="stat-label">총 운동 횟수</div></div>
        <div class="stat-card"><div class="stat-num" id="statMonth">0</div><div class="stat-label">이번 달 횟수</div></div>
        <div class="stat-card"><div class="stat-num" id="statSets">0</div><div class="stat-label">총 세트 수</div></div>
        <div class="stat-card"><div class="stat-num" id="statStreak">0</div><div class="stat-label">연속 운동일</div></div>
      </div>
      <div class="card">
        <div class="card-title">자주 한 운동 TOP 5</div>
        <div id="mostEx" style="font-size:14px;color:var(--text-muted);line-height:2.2;"></div>
      </div>
    </div>

    <!-- 즐겨찾기 -->
    <div class="section" id="fav">
      <div class="card">
        <div class="card-title">즐겨찾기 종목 관리</div>
        <div class="section-sub">✕ 누르면 삭제 / 아래서 새 종목 추가</div>
        <div class="fav-chips" id="favChipsManage"></div>
        <div class="divider"></div>
        <div class="fav-add-row">
          <input type="text" id="favNewName" placeholder="새 종목 이름 입력" onkeydown="if(event.key==='Enter')addFav()">
          <button class="fav-add-btn" onclick="addFav()">추가</button>
        </div>
      </div>
    </div>

  </div>
</div>

<!-- 모달 -->
<div class="modal-overlay" id="modalOverlay" onclick="closeModalBg(event)">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-header">
      <span class="modal-date" id="modalDate"></span>
      <button class="modal-close" onclick="closeModal()">✕</button>
    </div>
    <div id="modalContent"></div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
const DEFAULT_FAVS = [
  '런닝머신','레그컬','레그익스텐션','레그프레스',
  '벤치프레스','불가리안스쿼트','시티드로우',
  '아웃타이','인클라인덤벨','이너타이',
  '케이블푸시다운','펙덱플라이','풀업'
];

let favList = JSON.parse(localStorage.getItem('pig_favs') || 'null') || [...DEFAULT_FAVS];
let workouts = JSON.parse(localStorage.getItem('pig_workouts') || '[]');
let selectedCond = '';
let exCount = 0, setCount = 0;

const today = new Date();
const yyyy = today.getFullYear();
const mm = String(today.getMonth()+1).padStart(2,'0');
const dd = String(today.getDate()).padStart(2,'0');
const days = ['일','월','화','수','목','금','토'];
document.getElementById('workoutDate').value = `${yyyy}-${mm}-${dd}`;
document.getElementById('todayDate').textContent = `${yyyy}.${mm}.${dd}`;
document.getElementById('todayDay').textContent = `${days[today.getDay()]}요일`;

function switchTab(tab, el) {
  document.querySelectorAll('.section').forEach(s=>s.classList.remove('active'));
  document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
  document.getElementById(tab).classList.add('active');
  el.classList.add('active');
  if(tab==='logs') renderLogs();
  if(tab==='stats') renderStats();
  if(tab==='fav') renderFavManage();
  if(tab==='write') renderFavChipsWrite();
}

function selectCond(val, btn) {
  selectedCond = val;
  document.querySelectorAll('.cond-btn').forEach(b=>b.className='cond-btn');
  btn.classList.add(`selected-${val}`);
}

function saveFavs() {
  favList.sort((a,b)=>a.localeCompare(b,'ko'));
  localStorage.setItem('pig_favs', JSON.stringify(favList));
}

function renderFavChipsWrite() {
  const el = document.getElementById('favChipsWrite');
  if(!el) return;
  el.innerHTML = favList.map(name=>`
    <div class="fav-chip" onclick="quickAdd('${name}',this)">${name}</div>
  `).join('');
}

function quickAdd(name, chip) {
  addExercise(name);
  chip.classList.add('added');
  const orig = name;
  chip.textContent = orig + ' ✓';
  setTimeout(()=>{ chip.classList.remove('added'); chip.textContent = orig; }, 1200);
}

function renderFavManage() {
  document.getElementById('favChipsManage').innerHTML = favList.map((name,i)=>`
    <div class="fav-chip" style="cursor:default">
      ${name}<span class="chip-x" onclick="removeFav(${i})"> ✕</span>
    </div>
  `).join('');
}

function addFav() {
  const input = document.getElementById('favNewName');
  const name = input.value.trim();
  if(!name){ showToast('종목 이름을 입력해주세요'); return; }
  if(favList.includes(name)){ showToast('이미 있는 종목이에요'); return; }
  favList.push(name);
  saveFavs();
  renderFavManage();
  renderFavChipsWrite();
  input.value = '';
  showToast(`✅ "${name}" 추가됐어요`);
}

function removeFav(i) {
  const name = favList[i];
  favList.splice(i,1);
  saveFavs();
  renderFavManage();
  renderFavChipsWrite();
  showToast(`"${name}" 삭제됐어요`);
}

function addExercise(name='') {
  exCount++;
  const id = exCount;
  const div = document.createElement('div');
  div.className = 'exercise-item';
  div.id = `ex-${id}`;
  div.innerHTML = `
    <div class="exercise-header">
      <input class="exercise-name" type="text" placeholder="운동 이름" value="${name}">
      <button class="remove-btn" onclick="removeExercise(${id})">✕</button>
    </div>
    <div class="sets-header">
      <span>세트</span><span>무게(kg)</span><span>횟수(회)</span><span></span>
    </div>
    <div class="set-list" id="sets-${id}"></div>
    <button class="add-set-btn" onclick="addSet(${id})">+ 세트 추가</button>
  `;
  document.getElementById('exerciseList').appendChild(div);
  addSet(id);
  div.scrollIntoView({behavior:'smooth',block:'nearest'});
}

function removeExercise(id) {
  document.getElementById(`ex-${id}`)?.remove();
}

function addSet(exId) {
  setCount++;
  const sid = setCount;
  const setList = document.getElementById(`sets-${exId}`);
  const num = setList.children.length + 1;
  const row = document.createElement('div');
  row.className = 'set-row';
  row.id = `set-${sid}`;
  row.innerHTML = `
    <div class="set-num">${num}</div>
    <input class="set-input" type="text" placeholder="0" inputmode="decimal">
    <input class="set-input" type="text" placeholder="0" inputmode="numeric">
    <button class="remove-set" onclick="removeSet(${exId},${sid})">✕</button>
  `;
  setList.appendChild(row);
}

function removeSet(exId, sid) {
  document.getElementById(`set-${sid}`)?.remove();
  document.querySelectorAll(`#sets-${exId} .set-row`).forEach((r,i)=>{
    r.querySelector('.set-num').textContent = i+1;
  });
}

function saveWorkout() {
  const date = document.getElementById('workoutDate').value;
  const memo = document.getElementById('workoutMemo').value.trim();
  const exercises = [];

  document.querySelectorAll('.exercise-item').forEach(ex=>{
    const name = ex.querySelector('.exercise-name').value.trim();
    if(!name) return;
    const sets = [];
    ex.querySelectorAll('.set-row').forEach(row=>{
      const ins = row.querySelectorAll('input');
      const w = ins[0].value.trim(), r = ins[1].value.trim();
      if(w||r) sets.push({weight:w||'—',reps:r||'—'});
    });
    if(sets.length>0) exercises.push({name,sets});
  });

  if(!date){ showToast('날짜를 선택해주세요!'); return; }
  if(exercises.length===0){ showToast('운동 종목을 1개 이상 입력해주세요!'); return; }

  workouts.unshift({id:Date.now(),date,condition:selectedCond||'mid',exercises,memo});
  localStorage.setItem('pig_workouts',JSON.stringify(workouts));

  document.getElementById('exerciseList').innerHTML='';
  document.getElementById('workoutMemo').value='';
  selectedCond='';
  document.querySelectorAll('.cond-btn').forEach(b=>b.className='cond-btn');
  exCount=0; setCount=0;
  renderFavChipsWrite();
  showToast('✅ 운동 기록 저장 완료!');
}

const condIcon = {good:'😤',mid:'😐',bad:'😴'};

function renderLogs() {
  const el = document.getElementById('logList');
  if(workouts.length===0){
    el.innerHTML=`<div class="log-empty"><span class="icon">🏋️</span>아직 기록이 없어요<br>첫 번째 운동을 기록해보세요!</div>`;
    return;
  }
  el.innerHTML = workouts.map(w=>`
    <div class="log-item" onclick="showDetail(${w.id})">
      <div class="log-item-header">
        <span class="log-date">${w.date.replace(/-/g,'.')}</span>
        <div style="display:flex;align-items:center;gap:8px">
          <span style="font-size:18px">${condIcon[w.condition]||'😐'}</span>
          <button class="log-delete" onclick="deleteLog(event,${w.id})">삭제</button>
        </div>
      </div>
      <div class="log-exercises">
        ${w.exercises.map(e=>`<span class="log-tag">${e.name}</span>`).join('')}
      </div>
      ${w.memo?`<div class="log-memo">"${w.memo}"</div>`:''}
    </div>
  `).join('');
}

function deleteLog(e,id) {
  e.stopPropagation();
  workouts = workouts.filter(w=>w.id!==id);
  localStorage.setItem('pig_workouts',JSON.stringify(workouts));
  renderLogs();
  showToast('삭제됐어요');
}

function showDetail(id) {
  const w = workouts.find(x=>x.id===id);
  if(!w) return;
  document.getElementById('modalDate').textContent=`${w.date.replace(/-/g,'.')} ${condIcon[w.condition]||''}`;
  let html = w.exercises.map(ex=>`
    <div class="modal-exercise">
      <div class="modal-ex-name">${ex.name}</div>
      <div class="modal-sets">
        ${ex.sets.map((s,i)=>`<div class="modal-set-badge">${i+1}세트 <span>${s.weight}kg × ${s.reps}회</span></div>`).join('')}
      </div>
    </div>
  `).join('');
  if(w.memo) html+=`<div class="modal-memo-box">💬 ${w.memo}</div>`;
  document.getElementById('modalContent').innerHTML=html;
  document.getElementById('modalOverlay').classList.add('show');
}

function closeModalBg(e){ if(e.target===document.getElementById('modalOverlay')) closeModal(); }
function closeModal(){ document.getElementById('modalOverlay').classList.remove('show'); }

function renderStats() {
  const total=workouts.length;
  const thisMonth=workouts.filter(w=>w.date.startsWith(`${yyyy}-${mm}`)).length;
  let totalSets=0;
  const exFreq={};
  workouts.forEach(w=>{
    w.exercises.forEach(ex=>{
      totalSets+=ex.sets.length;
      exFreq[ex.name]=(exFreq[ex.name]||0)+1;
    });
  });
  document.getElementById('statTotal').textContent=total;
  document.getElementById('statMonth').textContent=thisMonth;
  document.getElementById('statSets').textContent=totalSets;

  const dates=[...new Set(workouts.map(w=>w.date))].sort().reverse();
  let streak=0, cur=new Date(today);
  for(let d of dates){
    const ds=`${cur.getFullYear()}-${String(cur.getMonth()+1).padStart(2,'0')}-${String(cur.getDate()).padStart(2,'0')}`;
    if(d===ds){streak++;cur.setDate(cur.getDate()-1);}else break;
  }
  document.getElementById('statStreak').textContent=streak;

  const sorted=Object.entries(exFreq).sort((a,b)=>b[1]-a[1]).slice(0,5);
  document.getElementById('mostEx').innerHTML=sorted.length
    ? sorted.map(([n,c],i)=>`<span style="color:var(--accent-blue);font-weight:700">${i+1}.</span> <span style="color:var(--text);font-weight:500">${n}</span> <span style="color:var(--text-muted)">— ${c}회</span><br>`).join('')
    : '<span style="color:var(--text-dim)">아직 기록이 없어요</span>';
}

function showToast(msg){
  const t=document.getElementById('toast');
  t.textContent=msg; t.classList.add('show');
  setTimeout(()=>t.classList.remove('show'),2200);
}

renderFavChipsWrite();
</script>
</body>
</html>
