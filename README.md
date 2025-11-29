<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>เกมพัฒนาสุขภาพนักเรียน</title>
<link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#f3f7ff; --card:#ffffff; --accent:#0f4bd8; --accent2:#06b6d4;
    --muted:#6b7280; --success:#10b981; --danger:#ef4444;
  }
  *{box-sizing:border-box;font-family:"Sarabun",system-ui,-apple-system,"Segoe UI",sans-serif}
  body{margin:0;background:linear-gradient(180deg,#eef6ff,#f8fbff);color:#0f172a}
  .wrap{max-width:1100px;margin:28px auto;padding:20px}
  header{display:flex;align-items:center;gap:16px}
  .logo{width:64px;height:64px;border-radius:12px;background:linear-gradient(135deg,var(--accent),var(--accent2));color:#fff;display:flex;align-items:center;justify-content:center;font-weight:800;font-size:20px;box-shadow:0 8px 30px rgba(15,75,216,0.12)}
  h1{margin:0;font-size:20px}
  p.lead{margin:4px 0 14px;color:var(--muted)}
  .grid{display:grid;grid-template-columns:320px 1fr;gap:18px}
  @media(max-width:980px){.grid{grid-template-columns:1fr}}
  .card{background:var(--card);border-radius:12px;padding:14px;border:1px solid rgba(15,23,42,0.04);box-shadow:0 8px 30px rgba(15,23,42,0.04)}
  label{display:block;font-weight:600;margin-bottom:6px}
  select,input{width:100%;padding:8px;border-radius:8px;border:1px solid #e6eefb}
  .tabs{display:flex;gap:8px;margin-top:10px}
  .tab{flex:1;text-align:center;padding:8px;border-radius:999px;background:#eef5ff;color:var(--accent);cursor:pointer;font-weight:700}
  .tab.active{background:linear-gradient(90deg,var(--accent),var(--accent2));color:#fff}
  .section{margin-top:12px}
  .badge{display:inline-block;padding:6px 10px;border-radius:999px;background:#eef2ff;color:var(--accent);font-weight:700}
  .pill{display:inline-block;padding:6px 10px;border-radius:999px;background:#f1f5f9;color:#0f172a}
  .muted{color:var(--muted);font-size:13px}
  button{background:linear-gradient(90deg,var(--accent),var(--accent2));color:#fff;border:0;padding:8px 12px;border-radius:999px;cursor:pointer;font-weight:700}
  button.ghost{background:transparent;color:var(--accent);border:1px solid rgba(15,75,216,0.12)}
  .list{list-style:none;padding:0;margin:0}
  .list li{padding:10px;border-radius:8px;border:1px solid #eef6ff;margin-bottom:8px;background:#fff;display:flex;justify-content:space-between;align-items:center}
  .center{display:flex;align-items:center;justify-content:center}
  .score{font-size:20px;font-weight:800;color:var(--accent)}
  .result{margin-top:10px;padding:10px;border-radius:8px;background:#f1f8ff;border:1px solid #dbeefe}
  .error{background:#fff1f2;border:1px solid #f8d7da;color:#b91c1c}
  .success{background:#ecfdf5;border:1px solid #bbf7d0;color:#065f46}
  .game-area{min-height:160px;display:flex;flex-direction:column;gap:8px}
  .meter{height:10px;background:#eef2ff;border-radius:999px;overflow:hidden}
  .meter > div{height:100%;background:linear-gradient(90deg,#06b6d4,#0f4bd8)}
  .small{padding:6px 10px;font-size:13px;border-radius:8px}
  .footer{margin-top:18px;color:var(--muted);font-size:13px;text-align:center}
</style>
</head>
<body>
  <div class="wrap">
    <header>
      <div class="logo">GH</div>
      <div>
        <h1>เกมพัฒนาสุขภาพนักเรียน</h1>
        <p class="lead">สนุก เรียนรู้ และสร้างนิสัยสุขภาพดี — เล่นเป็นทีม เก็บแต้ม รับเหรียญ!</p>
      </div>
      <div style="margin-left:auto" class="center">
        <div style="text-align:right;margin-right:12px">
          <div class="muted">คะแนนรวม</div>
          <div class="score" id="totalScore">0</div>
        </div>
      </div>
    </header>

    <div class="grid" style="margin-top:18px">
      <!-- LEFT: Menu / Profile -->
      <aside>
        <div class="card">
          <div style="display:flex;align-items:center;gap:12px">
            <div style="width:56px;height:56px;border-radius:10px;background:#eef5ff;display:flex;align-items:center;justify-content:center;font-weight:700;color:var(--accent)">ST</div>
            <div>
              <div style="font-weight:800" id="playerName">นักเรียน</div>
              <div class="muted" id="playerAge">อายุ: -</div>
            </div>
          </div>

          <div class="section">
            <label>เลือกช่วงอายุ</label>
            <select id="ageGroup">
              <option value="child">7–12 ปี</option>
              <option value="teen" selected>13–18 ปี</option>
              <option value="adult">19–59 ปี</option>
            </select>
          </div>

          <div class="section">
            <label>ม็อดของเกม</label>
            <div class="tabs">
              <div class="tab active" data-tab="quiz">ทายแคลอรี่</div>
              <div class="tab" data-tab="exercise">มินิ: ออกกำลังกาย</div>
              <div class="tab" data-tab="habit">ภารกิจนิสัย</div>
            </div>
          </div>

          <div class="section">
            <label>สถานะ/เหรียญ</label>
            <div style="display:flex;gap:8px;flex-wrap:wrap">
              <div class="pill">Level <span id="playerLevel">1</span></div>
              <div class="badge" id="coinCount">0 ✦</div>
            </div>
          </div>

          <div class="section">
            <button id="btnViewSummary" class="small ghost">สรุปผล / พิมพ์</button>
          </div>
        </div>
      </aside>

      <!-- RIGHT: Game content -->
      <main>
        <div class="card">
          <!-- dynamic content area -->
          <div id="gameContainer" class="game-area">
            <!-- initial blank -->
            <div style="text-align:center;color:var(--muted)">เลือกม็อดเกมด้านซ้ายเพื่อเริ่มเล่น</div>
          </div>

          <div id="gameFooter" style="margin-top:12px;display:flex;justify-content:space-between;align-items:center">
            <div class="muted">เวลาที่เล่น: <span id="playTime">0</span> นาที</div>
            <div>
              <button id="btnReset" class="small ghost">รีเซ็ตคะแนนในเครื่อง</button>
              <button id="btnSave" class="small">บันทึกผล</button>
            </div>
          </div>
        </div>

        <div class="card" style="margin-top:12px">
          <div style="display:flex;justify-content:space-between;align-items:center">
            <div>
              <div class="muted">Leaderboard (เครื่องนี้)</div>
              <div style="font-weight:800" id="leaderSummary">ไม่มีข้อมูล</div>
            </div>
            <div style="text-align:right">
              <div class="muted">ยอดแต้มสูงสุด</div>
              <div style="font-weight:800" id="bestScore">0</div>
            </div>
          </div>
        </div>
      </main>
    </div>

    <div class="footer">เกมนี้เป็นเครื่องมือการศึกษา — ไม่ใช่คำแนะนำทางการแพทย์</div>
  </div>

<script>
/* ---------------------------
   Data
----------------------------*/
const foods = [
  { name:"ข้าวมันไก่", calories:600 },
  { name:"ข้าวผัดกะเพรา+ไข่ดาว", calories:650 },
  { name:"ส้มตำไทย", calories:120 },
  { name:"ชานมไข่มุก 500ml", calories:300 },
  { name:"โค้ก 330ml", calories:140 },
  { name:"ขนมถุง มันฝรั่งทอด", calories:450 },
  { name:"นมจืด 250ml", calories:130 },
  { name:"ผลไม้ (กล้วย)", calories:90 },
  { name:"สลัดผัก", calories:90 },
  { name:"ก๋วยเตี๋ยว 1 ชาม", calories:320 }
];

const exercises = [
  { name:"เดินเร็ว", burnPerMin:4 },
  { name:"วิ่งเหยาะ", burnPerMin:8 },
  { name:"กระโดดเชือก", burnPerMin:10 },
  { name:"สควอท", burnPerMin:6 },
  { name:"วิดพื้น", burnPerMin:7 },
];

/* ---------------------------
   Game state (in-memory, persist to localStorage)
----------------------------*/
let state = {
  name: 'นักเรียน',
  ageGroup: 'teen',
  score: 0,
  coins: 0,
  level: 1,
  playMinutes: 0,
  history: [] // {mode, details, score, time}
};

const STORAGE_KEY = 'school_health_game_v1';

/* ---------------------------
   DOM refs
----------------------------*/
const tabs = document.querySelectorAll('.tab');
const gameContainer = document.getElementById('gameContainer');
const ageGroupEl = document.getElementById('ageGroup');
const totalScoreEl = document.getElementById('totalScore');
const coinCountEl = document.getElementById('coinCount');
const playerLevelEl = document.getElementById('playerLevel');
const playerNameEl = document.getElementById('playerName');
const playerAgeEl = document.getElementById('playerAge');
const playTimeEl = document.getElementById('playTime');
const leaderSummary = document.getElementById('leaderSummary');
const bestScoreEl = document.getElementById('bestScore');

const btnReset = document.getElementById('btnReset');
const btnSave = document.getElementById('btnSave');
const btnViewSummary = document.getElementById('btnViewSummary');

/* ---------------------------
   Init
----------------------------*/
function loadState(){
  const raw = localStorage.getItem(STORAGE_KEY);
  if(raw){
    try{ state = JSON.parse(raw); } catch(e){ console.warn('parse err',e); }
  }
  updateUI();
}
function saveState(){
  localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
  showToast('บันทึกผลเรียบร้อย');
  updateLeader();
}
function resetState(){
  if(!confirm('ต้องการรีเซ็ตคะแนนและข้อมูลบนเครื่องนี้หรือไม่?')) return;
  state = { name:'นักเรียน', ageGroup:'teen', score:0, coins:0, level:1, playMinutes:0, history:[] };
  saveState();
  updateUI();
}

/* ---------------------------
   UI helpers
----------------------------*/
function updateUI(){
  totalScoreEl.textContent = state.score;
  coinCountEl.textContent = `${state.coins} ✦`;
  playerLevelEl.textContent = state.level;
  playerNameEl.textContent = state.name;
  playerAgeEl.textContent = `ช่วงอายุ: ${state.ageGroup}`;
  playTimeEl.textContent = state.playMinutes;
  leaderSummary.textContent = state.history.length ? `${state.history.length} รอบเล่น` : 'ไม่มีข้อมูล';
  bestScoreEl.textContent = localStorage.getItem('bestScore') || 0;
}

function showToast(msg){
  const div = document.createElement('div');
  div.textContent = msg;
  div.style.position='fixed';
  div.style.right='16px';
  div.style.bottom='16px';
  div.style.background='#0f172a';
  div.style.color='#fff';
  div.style.padding='10px 14px';
  div.style.borderRadius='8px';
  div.style.boxShadow='0 8px 30px rgba(15,23,42,0.2)';
  document.body.appendChild(div);
  setTimeout(()=>{ div.style.opacity='0'; setTimeout(()=>div.remove(),400); },1800);
}

/* ---------------------------
   Tab handling
----------------------------*/
tabs.forEach(t=>{
  t.addEventListener('click', ()=> {
    tabs.forEach(x=>x.classList.remove('active'));
    t.classList.add('active');
    const tab = t.dataset.tab;
    openTab(tab);
  });
});

function openTab(tab){
  gameContainer.innerHTML = '';
  if(tab === 'quiz') renderQuiz();
  else if(tab === 'exercise') renderExerciseMini();
  else if(tab === 'habit') renderHabit();
}

/* ---------------------------
   QUIZ: ทายแคลอรี่
----------------------------*/
function renderQuiz(){
  const box = document.createElement('div');
  box.innerHTML = `
    <div style="display:flex;justify-content:space-between;align-items:center">
      <div><strong>เกมทายแคลอรี่</strong><div class="muted">เดาแคลอรี่ให้ใกล้เคียงเพื่อรับคะแนน</div></div>
      <div class="pill">คำถามที่ <span id="qIndex">0</span>/5</div>
    </div>
  `;
  const content = document.createElement('div'); content.className='section';
  const foodName = document.createElement('div'); foodName.style.fontWeight='800'; foodName.style.fontSize='18px';
  const hint = document.createElement('div'); hint.className='muted'; hint.style.marginTop='6px';
  const input = document.createElement('input'); input.type='number'; input.placeholder='กรอกแคลอรี่ (kcal)';
  input.style.marginTop='8px';
  const btn = document.createElement('button'); btn.textContent='ตรวจคำตอบ';
  const res = document.createElement('div'); res.className='result'; res.style.display='none';

  content.appendChild(foodName); content.appendChild(hint); content.appendChild(input); content.appendChild(btn); content.appendChild(res);
  box.appendChild(content);
  gameContainer.appendChild(box);

  // quiz logic
  let index = 0; const maxQ=5; let roundScore=0; let accumulateCalories=0;
  function nextQuestion(){
    if(index>=maxQ){
      // finish
      res.style.display='block';
      res.className='result success';
      res.innerHTML = `จบเกม! คะแนนจากรอบนี้: <strong>${roundScore}</strong><br>รวมแคลอรี่ที่ปรากฏ: ${accumulateCalories} kcal`;
      // update state
      state.score += roundScore;
      state.coins += Math.floor(roundScore/2);
      state.history.push({mode:'quiz', score:roundScore, time: new Date().toISOString()});
      state.playMinutes += 2;
      checkBest();
      updateUI(); saveState();
      return;
    }
    index++;
    document.getElementById('qIndex').textContent = index;
    const f = foods[Math.floor(Math.random()*foods.length)];
    currentFood = f;
    foodName.textContent = f.name;
    hint.textContent = `ประมาณการจริงจะถือว่า "ใกล้เคียง" ถ้า ±50 kcal`;
    input.value = '';
    res.style.display='none';
  }

  let currentFood = null;
  nextQuestion();

  btn.addEventListener('click', ()=>{
    const val = parseFloat(input.value);
    if(isNaN(val)){ res.style.display='block'; res.className='result error'; res.textContent='กรุณากรอกตัวเลข'; return; }
    const diff = Math.abs(val - currentFood.calories);
    let gain=0;
    if(diff<=10){ gain=10; res.className='result success'; res.innerHTML = `เยี่ยม! ตรงมาก ได้ ${gain} คะแนน (จริง ${currentFood.calories} kcal)`; }
    else if(diff<=50){ gain=6; res.className='result success'; res.innerHTML = `ดีมาก ได้ ${gain} คะแนน (จริง ${currentFood.calories} kcal)`; }
    else if(diff<=100){ gain=3; res.className='result'; res.innerHTML = `พอได้ ได้ ${gain} คะแนน (จริง ${currentFood.calories} kcal)`; }
    else { gain=0; res.className='result error'; res.innerHTML = `พลาดไป ได้ ${gain} คะแนน (จริง ${currentFood.calories} kcal)`; }
    res.style.display='block';
    roundScore += gain;
    accumulateCalories += currentFood.calories;
    // small delay then next
    setTimeout(nextQuestion,900);
  });
}

/* ---------------------------
   EXERCISE MINI: Tap challenge
   - ผู้เล่นต้องกดปุ่มเร็วที่สุดใน 20 วินาที
----------------------------*/
function renderExerciseMini(){
  const box = document.createElement('div');
  box.innerHTML = `<div style="display:flex;justify-content:space-between;align-items:center">
    <div><strong>มินิเกม: ออกกำลังกาย (Tap Challenge)</strong><div class="muted">กดปุ่มให้ได้มากที่สุดภายในเวลา</div></div>
    <div class="pill">เวลา: 20 วินาที</div>
  </div>`;
  const area = document.createElement('div'); area.style.marginTop='12px';
  const counter = document.createElement('div'); counter.style.fontSize='28px'; counter.style.fontWeight='800'; counter.textContent='0';
  const timerBar = document.createElement('div'); timerBar.className='meter'; timerBar.style.marginTop='8px'; const meterInner = document.createElement('div'); meterInner.style.width='100%'; timerBar.appendChild(meterInner);
  const btn = document.createElement('button'); btn.textContent='เริ่ม/กดที่นี่!'; btn.style.marginTop='12px'; btn.style.width='100%'; btn.className='';
  area.appendChild(counter); area.appendChild(timerBar); area.appendChild(btn);
  box.appendChild(area); gameContainer.appendChild(box);

  let running=false; let count=0; let timeLeft=20; let interval=null;
  btn.addEventListener('click', ()=>{
    if(!running){
      // start
      running=true; count=0; timeLeft=20; counter.textContent='0'; meterInner.style.width='100%';
      btn.textContent='กด!';
      interval = setInterval(()=>{
        timeLeft -= 0.1;
        const pct = Math.max(0, (timeLeft/20))*100;
        meterInner.style.width = pct + '%';
        if(timeLeft<=0){
          clearInterval(interval); running=false; btn.textContent='เริ่มใหม่';
          // evaluate
          const earned = Math.min(30, Math.floor(count/3)); // scale
          state.score += earned;
          state.coins += Math.floor(earned/2);
          state.history.push({mode:'exercise', score:earned, details:{taps:count}, time:new Date().toISOString()});
          state.playMinutes += 1;
          checkBest();
          updateUI(); saveState();
          showToast(`คุณกด ${count} ครั้ง — ได้ ${earned} คะแนน`);
        }
      },100);
    } else {
      // count tap
      count++;
      counter.textContent = count;
      // small animation
      btn.style.transform = 'scale(0.98)';
      setTimeout(()=>btn.style.transform='scale(1)',60);
    }
  });
}

/* ---------------------------
   HABIT CHALLENGE:
   - เลือก 1 ภารกิจ/นิสัย และบันทึกวันที่ทำ
----------------------------*/
function renderHabit(){
  const box = document.createElement('div');
  box.innerHTML = `<div style="display:flex;justify-content:space-between;align-items:center">
    <div><strong>ภารกิจนิสัย (Habit Challenge)</strong><div class="muted">เลือกภารกิจ แล้วกด 'เช็กอิน' เมื่อทำสำเร็จ</div></div>
    <div class="badge">สะสมแต้มรายสัปดาห์</div>
  </div>`;
  const area = document.createElement('div'); area.style.marginTop='12px';
  const select = document.createElement('select');
  ['เดิน 20 นาที','ดื่มน้ำ 6 แก้ว','งดของหวาน 1 วัน','นอนก่อน 22:00'].forEach(t=> {
    const opt = document.createElement('option'); opt.value=t; opt.textContent=t; select.appendChild(opt);
  });
  const btnCheck = document.createElement('button'); btnCheck.textContent='เช็กอินวันนี้'; btnCheck.style.marginTop='8px';
  const histDiv = document.createElement('div'); histDiv.style.marginTop='10px';
  area.appendChild(select); area.appendChild(btnCheck); area.appendChild(histDiv);
  box.appendChild(area); gameContainer.appendChild(box);

  // habit storage in state.habits
  if(!state.habits) state.habits = {}; // {habitName: [dates]}
  function renderHist(){
    histDiv.innerHTML = '';
    const h = select.value;
    const dates = state.habits[h] || [];
    if(dates.length===0) histDiv.innerHTML = '<div class="muted">ยังไม่มีการเช็กอิน</div>';
    else{
      const ul = document.createElement('ul'); ul.className='list';
      dates.slice(-7).reverse().forEach(d=> {
        const li = document.createElement('li'); li.innerHTML = `<div>${d}</div><div class="muted">✓</div>`; ul.appendChild(li);
      });
      histDiv.appendChild(ul);
    }
  }
  renderHist();

  btnCheck.addEventListener('click', ()=>{
    const h = select.value;
    const today = new Date().toISOString().slice(0,10);
    state.habits[h] = state.habits[h] || [];
    if(state.habits[h].includes(today)){ showToast('คุณเช็กอินวันนี้ไปแล้ว'); return; }
    state.habits[h].push(today);
    // reward: 5 coins per check-in, 10 points
    state.coins += 5;
    state.score += 10;
    state.history.push({mode:'habit', score:10, details:{habit:h, date:today}, time:new Date().toISOString()});
    state.playMinutes += 1;
    checkBest();
    saveState(); updateUI(); renderHist();
    showToast(`เช็กอิน "${h}" เรียบร้อย ได้ 10 คะแนน + 5 ✦`);
  });

}

/* ---------------------------
   Leaderboard local
----------------------------*/
function updateLeader(){
  const best = Number(localStorage.getItem('bestScore')||0);
  if(state.score > best) localStorage.setItem('bestScore', state.score);
  bestScoreEl.textContent = localStorage.getItem('bestScore')||0;
  // simple top: store last results array
  let top = JSON.parse(localStorage.getItem('topHist')||'[]');
  top.push({score: state.score, time: new Date().toISOString()});
  top = top.slice(-10);
  localStorage.setItem('topHist', JSON.stringify(top));
  leaderSummary.textContent = `รอบเล่น ${state.history.length} รอบ`;
}

/* ---------------------------
   Misc: Save, Reset, View Summary
----------------------------*/
btnSave.addEventListener('click', ()=>{ saveState(); });
btnReset.addEventListener('click', ()=>{ resetState(); });

btnViewSummary.addEventListener('click', ()=>{
  const w = window.open('','_blank','width=900,height=700');
  w.document.write(`<html><head><meta charset="utf-8"><title>สรุปผลการเล่น</title></head><body><h2>สรุปผล — ${state.name}</h2>`);
  w.document.write(`<p>คะแนนรวม: ${state.score} | เหรียญ: ${state.coins} | Level: ${state.level} | เวลาที่เล่นโดยประมาณ: ${state.playMinutes} นาที</p>`);
  if(state.history.length===0) w.document.write('<p>ยังไม่มีประวัติการเล่น</p>');
  else{
    w.document.write('<ol>');
    state.history.slice().reverse().forEach(h=> w.document.write(`<li>${h.mode} — คะแนน ${h.score} — ${h.time}</li>`));
    w.document.write('</ol>');
  }
  w.document.write('</body></html>'); w.document.close(); w.print();
});

/* ---------------------------
   Utility: update best check
----------------------------*/
function checkBest(){
  const best = Number(localStorage.getItem('bestScore')||0);
  if(state.score > best){
    localStorage.setItem('bestScore', state.score);
    showToast('ใหม่! สถิติดีที่สุดบนเครื่องนี้ 🎉');
  }
}

/* ---------------------------
   Start
----------------------------*/
loadState();
openTab('quiz'); // default
updateLeader();

/* optional: autosave every 20s */
setInterval(()=>{ saveState(); },20000);

</script>
</body>
</html>
