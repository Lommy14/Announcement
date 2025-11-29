
const burned = Math.round(kcalPerMin * mins);
STATE.burned += burned;
addPoints(Math.round(burned/5));
updateSummary(); saveState(); alert(`เพิ่มการออกกำลังกาย: ${ex.part} — ${mins} นาที, เบิร์นประมาณ ${burned} kcal`);
}


function scaledBurnRate(basePerMin, weight){
// basePerMin is for ~60kg reference; scale linearly by weight/60
return basePerMin * (STATE.weight/60);
}


// ---------------- Summary & Challenge ----------------
function updateSummary(){
const plateTotal = STATE.plate.reduce((s,p)=>s+p.kcal,0);
const burned = STATE.burned;
const net = plateTotal - burned;
netCalEl.textContent = net + ' kcal';
netNote.textContent = `อาหาร ${plateTotal} kcal — เบิร์น ${burned} kcal`;
const g = AGE_GROUPS.find(a=>a.id===STATE.ageGroup);
const target = Math.round(g.target * g.multiplier);
const pct = Math.max(0, Math.min(100, Math.round((net/target)*100)));
meterFill.style.width = pct + '%';
// color meter if over
meterFill.style.background = net > target ? 'linear-gradient(90deg,#ef4444,#f97316)' : 'linear-gradient(90deg,var(--accent),#06b6d4)';
pointsEl.textContent = STATE.points;
}


function startChallenge(){
const mode = confirm('เลือก OK: พยายามเข้าในช่วงเป้าหมาย (เข้าหรือใกล้)\nยกเลิก: พยายาม "ลด" ไม่ให้เกินเป้าหมาย');
const g = AGE_GROUPS.find(a=>a.id===STATE.ageGroup);
const target = Math.round(g.target * g.multiplier);
const plateTotal = STATE.plate.reduce((s,p)=>s+p.kcal,0);
const net = plateTotal - STATE.burned;
let message='';
if(mode){
const low = Math.round(target*0.85), high = Math.round(target*1.05);
if(net >= low && net <= high){ message='เก่ง! คุณอยู่ในช่วงเป้าหมาย'; addPoints(20); }
else if(net < low){ message=`พลังงานน้อยกว่าช่วงเป้าหมาย — พิจารณาเพิ่มอาหารที่มีคุณค่า (โปรตีน/ผัก/ไขมันดี)`; addPoints(5); }
else { message='พลังงานเกินช่วง — ลองชดเชยด้วยการเคลื่อนไหวหรือเลือกอาหารที่แคลอรี่น้อยลง'; addPoints(0); }
} else {
if(net <= target){ message='ดีแล้ว! คุณไม่เกินเป้าหมายวันนี้'; addPoints(15); }
else { message='เกินเป้าหมาย — ลองลดเครื่องดื่มหวานหรือเพิ่มการเดินเร็ว'; addPoints(3); }
}
alert(message + '\n\nสรุป: อินพุต ' + plateTotal + ' kcal, เบิร์น ' + STATE.burned + ' kcal, สุทธิ ' + net + ' kcal');
updateSummary(); saveState();
}


function addPoints(n){ STATE.points = Math.max(0, STATE.points + n); }


// ---------------- Persistence ----------------
function saveState(){ try{ localStorage.setItem('calorie_game_state', JSON.stringify(STATE)); }catch(e){ console.warn('ไม่สามารถบันทึกได้', e); } }
function loadState(){ try{ const raw = localStorage.getItem('calorie_game_state'); if(raw){ const s = JSON.parse(raw); Object.assign(STATE, s); } }catch(e){ console.warn(e); } }


function saveSession(){ const raw = { date: new Date().toISOString(), plate: STATE.plate, burned: STATE.burned, points: STATE.points, ageGroup: STATE.ageGroup, weight: STATE.weight }; const arr = JSON.parse(localStorage.getItem('calorie_game_sessions')||'[]'); arr.push(raw); localStorage.setItem('calorie_game_sessions', JSON.stringify(arr)); alert('บันทึกเซสชันแล้ว'); }


function clearAll(){ if(confirm('ยืนยันรีเซ็ตข้อมูลทั้งหมด?')){ localStorage.removeItem('calorie_game_state'); localStorage.removeItem('calorie_game_sessions'); location.reload(); } }


// ---------------- Utilities ----------------
function showTips(){ alert('เคล็ดลับ: เลือกอาหารครบหมู่ — โปรตีนเพื่อความอิ่ม ผักผลไม้ให้วิตามิน และลดเครื่องดื่มที่มีน้ำตาลสูง'); }


// helper to select elements by class (for ex highlight)
function $all(sel){ return Array.from(document.querySelectorAll(sel)); }


// ---------------- Startup ----------------
init();


// small UI: click on exercise cards to add minutes via keyboard if focused
exerciseGrid.addEventListener('keydown', (e)=>{ if(e.key==='Enter'){ const focused = document.activeElement; const idx = Array.from(exerciseGrid.children).indexOf(focused); if(idx>=0){ selectExercise(EXERCISE_DATA[idx].id); addExerciseFromSelected(); } } });


</script>
</body>
</html>
