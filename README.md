<html lang="th">
items = items.filter(a => a.id !== it.id);
}
}
});
}


function startGame(){ if(gameRunning) return; gameRunning=true; gameTimer = setInterval(stepGame, 300); }
function pauseGame(){ gameRunning=false; clearInterval(gameTimer); }
function resetGame(){ pauseGame(); items.forEach(it=> area.contains(it.el) && area.removeChild(it.el)); items = []; playerX = 50; updatePlayer(); }


function updatePlayer(){ player.style.left = playerX + '%'; }


$('#startGame').addEventListener('click', ()=>startGame());
$('#pauseGame').addEventListener('click', ()=>pauseGame());
$('#resetGame').addEventListener('click', ()=>resetGame());


// keyboard controls
window.addEventListener('keydown', (e)=>{
if(e.key === 'ArrowLeft'){ playerX = Math.max(6, playerX - 8); updatePlayer(); }
if(e.key === 'ArrowRight'){ playerX = Math.min(94, playerX + 8); updatePlayer(); }
});


/********************* HABIT TRACKER *********************/
const habitGrid = $('#habitGrid');
const todayKey = () => new Date().toISOString().slice(0,10);
function loadLogs(){ try{ return JSON.parse(localStorage.getItem('hg_logs')||'{}') }catch(e){ return {} } }
function saveLogs(l){ localStorage.setItem('hg_logs', JSON.stringify(l)) }
function getTodayRow(){ const logs = loadLogs(); return logs[todayKey()] || HABITS.map(()=>false) }


function renderHabits(){ habitGrid.innerHTML = '';
const logs = loadLogs(); const today = getTodayRow();
HABITS.forEach((h,i)=>{
const b = document.createElement('button'); b.className='habit-btn'; b.textContent = h;
const streak = getStreak(i);
const s = document.createElement('div'); s.className='small'; s.style.marginTop='6px'; s.textContent = 'Streak: ' + streak;
const wrapper = document.createElement('div'); wrapper.appendChild(b); wrapper.appendChild(s);
if(today[i]) b.classList.add('active');
b.addEventListener('click', ()=>{
toggleToday(i); renderHabits();
});
habitGrid.appendChild(wrapper);
}) }


function toggleToday(i){ const logs = loadLogs(); const key = todayKey(); const row = logs[key] ? [...logs[key]] : HABITS.map(()=>false); row[i] = !row[i]; logs[key] = row; saveLogs(logs); }


function getStreak(i){ const logs = loadLogs(); const dates = Object.keys(logs).sort().reverse(); let streak=0; for(const d of dates){ if(logs[d][i]) streak++; else break; if(streak>=365) break; } return streak }


renderHabits();


/********************* INITIAL UX TWEAKS *********************/
// make tabs interactive by id selection on load
function init(){
// expose ids for tab switching
document.getElementById('quiz').style.display='block';
document.getElementById('catch').style.display='none';
document.getElementById('habit').style.display='none';
// small helper: clicking nav buttons when dataset exists
$all('nav .tab').forEach(b=> b.addEventListener('click', ()=>{
const t = b.dataset.tab; switchTab(t);
}));
}
init();


// helper: ensure names don't collide with elements with ids
// (previously used in switchTab)
function switchTab(name){
$all('nav .tab').forEach(b=>b.classList.toggle('active', b.dataset.tab===name));
document.getElementById('quiz').style.display = name==='quiz' ? 'block' : 'none';
document.getElementById('catch').style.display = name==='catch' ? 'block' : 'none';
document.getElementById('habit').style.display = name==='habit' ? 'block' : 'none';
}


// small accessibility: focus game area on clicking it
area.addEventListener('click', ()=> area.focus());


// update scoreEl live region
const scoreObserver = new MutationObserver(()=>{});
scoreObserver.observe(scoreEl, {childList:true});


</script>
</body>
</html>
