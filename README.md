<html lang="th">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>ระบบประกาศข้อมูลข่าวสารโรงเรียน</title>
  <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;600;700&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg:#f7fbff;
      --card:#ffffff;
      --primary:#1e3a8a;
      --accent:#2563eb;
      --muted:#6b7280;
      --line:#e6eefc;
      --radius:14px;
    }
    *{box-sizing:border-box;font-family:"Sarabun", system-ui, -apple-system, "Segoe UI", sans-serif}
    body{
      margin:0;
      background:var(--bg);
      padding:20px;
      color:#10243a;
      display:flex;
      justify-content:center;
    }

    .container{
      width:100%;
      max-width:1200px;
      background:var(--card);
      border-radius:18px;
      padding:22px;
      border:1px solid var(--line);
      box-shadow:0 10px 30px rgba(16,36,58,0.06);
    }

    header{
      text-align:center;
      margin-bottom:16px;
    }
    header h1{
      margin:0;
      color:var(--primary);
      font-size:28px;
      font-weight:700;
    }
    header p{
      margin:6px 0 0 0;
      color:var(--muted);
      font-size:13px;
    }

    .layout{
      display:grid;
      grid-template-columns: 420px 1fr;
      gap:18px;
    }
    @media(max-width:980px){
      .layout{grid-template-columns:1fr}
    }

    .card{
      background:linear-gradient(180deg,#fbfeff,#f6fbff);
      border-radius:12px;
      padding:16px;
      border:1px solid var(--line);
    }

    .card h2{
      margin:0 0 10px 0;
      font-size:18px;
      color:#10243a;
    }

    label{
      display:block;
      font-size:13px;
      font-weight:600;
      color:#1f2937;
      margin-top:10px;
    }
    input[type="text"], input[type="date"], select, textarea {
      width:100%;
      padding:10px 12px;
      border-radius:10px;
      border:1px solid #dbe9ff;
      background:#fff;
      font-size:14px;
      margin-top:6px;
      outline:none;
    }
    textarea{min-height:100px;resize:vertical}

    .checkbox-row{display:flex;flex-wrap:wrap;gap:8px;margin-top:8px}
    .checkbox-row label{font-weight:500;font-size:14px;display:flex;gap:8px;align-items:center}

    .muted{color:var(--muted);font-size:13px;margin-top:6px}

    .btn-row{display:flex;gap:10px;margin-top:14px;align-items:center}
    .btn{
      padding:10px 14px;border-radius:999px;border:0;cursor:pointer;font-weight:700;
      display:inline-flex;align-items:center;gap:8px;
    }
    .btn-primary{background:linear-gradient(90deg,var(--accent),#16a34a);color:#fff}
    .btn-ghost{background:#eef6ff;color:#10243a;border:1px solid #e6f0ff}

    /* right column filters */
    .filters{
      display:flex;
      gap:8px;
      flex-wrap:wrap;
      align-items:center;
      margin-bottom:10px;
    }
    .filters .small-input{max-width:260px;padding:8px;border-radius:10px;border:1px solid #dbe9ff;background:#fff}
    .filters .date-input{max-width:150px;padding:8px;border-radius:10px;border:1px solid #dbe9ff;background:#fff}

    .news-list{display:flex;flex-direction:column;gap:12px;max-height:560px;overflow:auto;padding-right:6px}

    .news-item{
      background:#fff;border-radius:10px;padding:12px;border:1px solid #eef6ff;
      box-shadow:0 6px 18px rgba(16,36,58,0.03);
    }
    .news-top{display:flex;justify-content:space-between;gap:8px;align-items:flex-start}
    .news-title{font-weight:800;color:#0b2b48;font-size:16px;margin-bottom:6px}
    .meta{font-size:13px;color:var(--muted)}
    .targets{margin-top:6px}
    .targets .tag{display:inline-block;padding:4px 8px;border-radius:999px;background:#f1fbff;color:#0b4db1;margin-right:6px;font-weight:700;font-size:12px}

    .news-body{margin-top:8px;color:#203845;white-space:pre-line;font-size:14px}

    .actions{display:flex;gap:8px;justify-content:flex-end;margin-top:10px}
    .action-btn{padding:6px 10px;border-radius:8px;background:#f3f7ff;border:1px solid #e6f0ff;cursor:pointer;font-weight:700}

    .empty{text-align:center;color:var(--muted);padding:24px;border-radius:8px;border:1px dashed #e6eefc}

    .note-removed{display:none} /* we removed the LocalStorage note as requested */
  </style>
</head>
<body>
  <div class="container">
    <header>
      <h1>ระบบประกาศข้อมูลข่าวสารโรงเรียน</h1>
      <p>บันทึกข่าวใหม่และแสดงข่าวทั้งหมดในที่เดียว</p>
    </header>

    <div class="layout">
      <!-- left: form -->
      <div class="card">
        <h2>บันทึกข่าวสารใหม่</h2>

        <label for="title">หัวข้อข่าว</label>
        <input id="title" type="text" placeholder="เช่น แจ้งกำหนดการประชุมผู้ปกครอง">

        <label for="publisher">ชื่อผู้ลงประกาศ</label>
        <input id="publisher" type="text" placeholder="ชื่อ-นามสกุล">

        <label for="date">วันที่ประกาศ</label>
        <input id="date" type="date">

        <label for="category">หมวดหมู่ข่าว</label>
        <select id="category">
          <option value="ทั่วไป">ทั่วไป</option>
          <option value="การเรียนการสอน">การเรียนการสอน</option>
          <option value="กิจกรรม">กิจกรรม</option>
          <option value="ประชุม/อบรม">ประชุม/อบรม</option>
          <option value="งานวิชาการ">งานวิชาการ</option>
          <option value="อื่น ๆ">อื่น ๆ</option>
        </select>

        <label>กลุ่มเป้าหมาย</label>
        <div id="targets" class="checkbox-row">
          <label><input type="checkbox" value="นักเรียน"> นักเรียน</label>
          <label><input type="checkbox" value="ครู"> ครู</label>
          <label><input type="checkbox" value="ผู้ปกครอง"> ผู้ปกครอง</label>
          <label><input type="checkbox" value="บุคลากร"> บุคลากร</label>
          <label><input type="checkbox" value="บุคคลทั่วไป"> บุคคลทั่วไป</label>
        </div>

        <label for="content">รายละเอียดข่าว</label>
        <textarea id="content" placeholder="รายละเอียด เช่น วัน เวลา สถานที่ เงื่อนไข"></textarea>

        <div class="btn-row">
          <button id="saveBtn" class="btn btn-primary">บันทึกข่าว</button>
          <button id="clearBtn" class="btn btn-ghost">ล้างแบบฟอร์ม</button>
        </div>

        <!-- Note removed per request -->
        <div class="note-removed">
          <p class="muted">หมายเหตุ: ข่าวที่บันทึกจะเก็บไว้ในเบราว์เซอร์เครื่องนี้ (LocalStorage) หากเปิดจากเครื่องอื่น จะไม่เห็นข่าวชุดนี้</p>
        </div>
      </div>

      <!-- right: list + filters -->
      <div class="card">
        <h2>รายการข่าวสาร</h2>

        <div class="filters">
          <input id="search" class="small-input" type="text" placeholder="พิมพ์คำค้นหัวข้อ / รายละเอียด">
          <select id="filterCategory" class="small-input">
            <option value="">ทุกหมวดหมู่</option>
            <option>ทั่วไป</option>
            <option>การเรียนการสอน</option>
            <option>กิจกรรม</option>
            <option>ประชุม/อบรม</option>
            <option>งานวิชาการ</option>
            <option>อื่น ๆ</option>
          </select>

          <div style="display:flex;gap:8px;align-items:center">
            <label style="font-weight:700;margin-right:6px">กลุ่ม:</label>
            <label><input type="checkbox" class="filter-target" value="นักเรียน">นักเรียน</label>
            <label><input type="checkbox" class="filter-target" value="ครู">ครู</label>
            <label><input type="checkbox" class="filter-target" value="ผู้ปกครอง">ผู้ปกครอง</label>
            <label><input type="checkbox" class="filter-target" value="บุคลากร">บุคลากร</label>
            <label><input type="checkbox" class="filter-target" value="บุคคลทั่วไป">บุคคลทั่วไป</label>
          </div>

          <input id="fromDate" class="date-input" type="date" title="วันที่จาก">
          <input id="toDate" class="date-input" type="date" title="ถึง">

          <label style="margin-left:auto; display:flex;align-items:center;gap:8px">
            <input id="pinnedOnly" type="checkbox"> แสดงเฉพาะปักหมุด
          </label>

          <button id="clearAllBtn" class="btn btn-ghost small">ลบข่าวทั้งหมด</button>
        </div>

        <div id="newsList" class="news-list">
          <!-- news items injected here -->
        </div>
      </div>
    </div>
  </div>

  <script>
    // Key for localStorage
    const STORAGE_KEY = 'school_news_v2';

    // DOM Elements
    const titleEl = document.getElementById('title');
    const publisherEl = document.getElementById('publisher');
    const dateEl = document.getElementById('date');
    const categoryEl = document.getElementById('category');
    const contentEl = document.getElementById('content');
    const targetsEl = document.getElementById('targets');
    const saveBtn = document.getElementById('saveBtn');
    const clearBtn = document.getElementById('clearBtn');

    const searchEl = document.getElementById('search');
    const filterCategoryEl = document.getElementById('filterCategory');
    const filterTargetCheckboxes = document.querySelectorAll('.filter-target');
    const fromDateEl = document.getElementById('fromDate');
    const toDateEl = document.getElementById('toDate');
    const pinnedOnlyEl = document.getElementById('pinnedOnly');
    const newsListDiv = document.getElementById('newsList');
    const clearAllBtn = document.getElementById('clearAllBtn');

    let items = [];

    // Initialize default date to today
    function setToday() {
      const t = new Date();
      const yyyy = t.getFullYear();
      const mm = String(t.getMonth()+1).padStart(2,'0');
      const dd = String(t.getDate()).padStart(2,'0');
      dateEl.value = `${yyyy}-${mm}-${dd}`;
    }

    // Load from localStorage
    function load() {
      try {
        const raw = localStorage.getItem(STORAGE_KEY);
        items = raw ? JSON.parse(raw) : [];
      } catch (e) {
        items = [];
      }
    }

    // Save to localStorage
    function save() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(items));
    }

    // Clear form
    function clearForm() {
      titleEl.value = '';
      publisherEl.value = '';
      contentEl.value = '';
      categoryEl.value = 'ทั่วไป';
      targetsEl.querySelectorAll('input[type="checkbox"]').forEach(cb => cb.checked = false);
      setToday();
    }

    // Helper: get selected targets from form
    function getFormTargets() {
      const res = [];
      targetsEl.querySelectorAll('input[type="checkbox"]').forEach(cb => { if(cb.checked) res.push(cb.value); });
      return res;
    }

    // Helper: get selected filter targets (array)
    function getFilterTargets() {
      const res = [];
      document.querySelectorAll('.filter-target').forEach(cb => { if(cb.checked) res.push(cb.value); });
      return res;
    }

    // Date in range helper (strings YYYY-MM-DD)
    function dateInRange(d, from, to) {
      if (!d) return false;
      if (from && d < from) return false;
      if (to && d > to) return false;
      return true;
    }

    // Render items with filters
    function render() {
      newsListDiv.innerHTML = '';
      const keyword = (searchEl.value || '').trim().toLowerCase();
      const catFilter = filterCategoryEl.value;
      const selectedTargets = getFilterTargets();
      const from = fromDateEl.value;
      const to = toDateEl.value;
      const pinnedOnly = pinnedOnlyEl.checked;

      // sort: pinned first, then newest createdAt
      const sorted = items.slice().sort((a,b) => {
        if (a.pinned && !b.pinned) return -1;
        if (!a.pinned && b.pinned) return 1;
        return (b.createdAt || 0) - (a.createdAt || 0);
      });

      const filtered = sorted.filter(it => {
        if (pinnedOnly && !it.pinned) return false;

        const text = ((it.title||'') + ' ' + (it.content||'') + ' ' + (it.publisher||'') + ' ' + (it.targets||[]).join(' ')).toLowerCase();
        if (keyword && !text.includes(keyword)) return false;

        if (catFilter && it.category !== catFilter) return false;

        // targets: OR logic — if user selected some filter targets, must match at least one
        if (selectedTargets.length > 0) {
          if (!Array.isArray(it.targets) || it.targets.length === 0) return false;
          const any = selectedTargets.some(t => it.targets.includes(t));
          if (!any) return false;
        }

        if ((from || to) && !dateInRange(it.date || '', from, to)) return false;

        return true;
      });

      if (filtered.length === 0) {
        const empty = document.createElement('div');
        empty.className = 'empty';
        empty.textContent = 'ยังไม่มีข่าว หรือไม่พบข่าวตามเงื่อนไข';
        newsListDiv.appendChild(empty);
        return;
      }

      filtered.forEach(it => {
        const card = document.createElement('div');
        card.className = 'news-item';

        const top = document.createElement('div');
        top.className = 'news-top';

        const left = document.createElement('div');
        const title = document.createElement('div');
        title.className = 'news-title';
        title.textContent = it.title || '(ไม่มีหัวข้อ)';

        const meta = document.createElement('div');
        meta.className = 'meta';
        meta.innerHTML = `<strong>${it.publisher? escapeHtml(it.publisher) : '—'}</strong> &nbsp; • &nbsp; ${it.date || '-'}`;

        left.appendChild(title);
        left.appendChild(meta);

        const right = document.createElement('div');
        if (it.pinned) {
          const p = document.createElement('div');
          p.className = 'tag';
          p.style.padding = '6px 10px';
          p.style.borderRadius = '999px';
          p.style.background = '#fff7ed';
          p.style.color = '#92400e';
          p.style.fontWeight = '800';
          p.textContent = 'ปักหมุด';
          right.appendChild(p);
        }

        top.appendChild(left);
        top.appendChild(right);

        const targetsLine = document.createElement('div');
        targetsLine.className = 'targets';
        if (it.targets && it.targets.length) {
          it.targets.forEach(t => {
            const tag = document.createElement('span');
            tag.className = 'tag';
            tag.style.marginRight = '6px';
            tag.style.background = '#eef9ff';
            tag.style.color = '#0b58a5';
            tag.style.padding = '4px 8px';
            tag.style.borderRadius = '999px';
            tag.style.fontWeight = '700';
            tag.textContent = t;
            targetsLine.appendChild(tag);
          });
        } else {
          const tag = document.createElement('span');
          tag.className = 'tag';
          tag.style.background = '#f3f7ff';
          tag.style.color = '#6b7280';
          tag.style.padding = '4px 8px';
          tag.style.borderRadius = '999px';
          tag.textContent = 'กลุ่มเป้าหมาย: ไม่ระบุ';
          targetsLine.appendChild(tag);
        }

        const body = document.createElement('div');
        body.className = 'news-body';
        body.textContent = it.content || '';

        const actions = document.createElement('div');
        actions.className = 'actions';
        const pinBtn = document.createElement('button');
        pinBtn.className = 'action-btn';
        pinBtn.textContent = it.pinned ? 'ยกเลิกปักหมุด' : 'ปักหมุด';
        pinBtn.addEventListener('click', () => togglePin(it.id));

        const delBtn = document.createElement('button');
        delBtn.className = 'action-btn';
        delBtn.textContent = 'ลบ';
        delBtn.addEventListener('click', () => deleteItem(it.id));

        actions.appendChild(pinBtn);
        actions.appendChild(delBtn);

        card.appendChild(top);
        card.appendChild(targetsLine);
        card.appendChild(body);
        card.appendChild(actions);

        newsListDiv.appendChild(card);
      });
    }

    // escape for display in meta
    function escapeHtml(s){
      if(!s) return '';
      return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
    }

    // Save new item
    function addItem(){
      const title = titleEl.value.trim();
      const publisher = publisherEl.value.trim();
      const date = dateEl.value;
      const category = categoryEl.value;
      const content = contentEl.value.trim();
      const targets = getFormTargets();

      if(!title){
        alert('กรุณากรอกหัวข้อข่าว');
        return;
      }

      const obj = {
        id: Date.now().toString(),
        title, publisher, date, category, content, targets,
        pinned: false,
        createdAt: Date.now()
      };

      items.push(obj);
      save();
      render();
      clearForm();
    }

    function togglePin(id){
      const idx = items.findIndex(i => i.id === id);
      if(idx !== -1){
        items[idx].pinned = !items[idx].pinned;
        save();
        render();
      }
    }

    function deleteItem(id){
      if(!confirm('ต้องการลบข่าวนี้หรือไม่?')) return;
      items = items.filter(i => i.id !== id);
      save();
      render();
    }

    function clearAll(){
      if(!confirm('ต้องการลบข่าวทั้งหมดหรือไม่?')) return;
      items = [];
      save();
      render();
    }

    // Attach events
    saveBtn.addEventListener('click', addItem);
    clearBtn.addEventListener('click', clearForm);
    clearAllBtn.addEventListener('click', clearAll);
    searchEl.addEventListener('input', render);
    filterCategoryEl.addEventListener('change', render);
    fromDateEl.addEventListener('change', render);
    toDateEl.addEventListener('change', render);
    pinnedOnlyEl.addEventListener('change', render);
    document.querySelectorAll('.filter-target').forEach(cb => cb.addEventListener('change', render));

    // init
    setToday();
    load();
    render();
  </script>
</body>
</html>
