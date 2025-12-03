<html lang="th">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>ระบบประกาศข้อมูลข่าวสารโรงเรียน (แชร์ผ่าน Firestore)</title>
  <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;600&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg:#f3f6ff;--card:#ffffff;--accent:#2563eb;--muted:#6b7280;
    }
    *{box-sizing:border-box;font-family:"Sarabun",system-ui,-apple-system,"Segoe UI",sans-serif}
    body{margin:0;background:var(--bg);padding:20px;display:flex;justify-content:center}
    .wrap{width:100%;max-width:1100px;background:var(--card);border-radius:14px;padding:18px;box-shadow:0 10px 30px rgba(15,23,42,0.08);border:1px solid #e6eefc}
    header{display:flex;gap:12px;align-items:center}
    .logo{width:52px;height:52px;border-radius:10px;background:linear-gradient(90deg,var(--accent),#12b981);color:#fff;display:flex;align-items:center;justify-content:center;font-weight:700}
    h1{margin:0;font-size:20px;color:#0b3a8a}
    .subtitle{margin:6px 0 12px;color:var(--muted);font-size:13px}
    .grid{display:grid;grid-template-columns:minmax(0,360px) 1fr;gap:16px}
    @media(max-width:920px){.grid{grid-template-columns:1fr}}
    .card{background:#fbfeff;border-radius:10px;padding:12px;border:1px solid #eef6ff}
    label{display:block;font-weight:600;margin-top:8px;color:#16324a}
    input[type="text"],input[type="date"],select,textarea{width:100%;padding:8px;border-radius:8px;border:1px solid #d6e6ff;background:#fff;margin-top:6px}
    textarea{min-height:90px;resize:vertical}
    .checkbox-row{display:flex;flex-wrap:wrap;gap:8px;margin-top:6px}
    .checkbox-row label{display:flex;align-items:center;gap:6px;font-weight:500;color:#233444}
    .btn-row{display:flex;gap:8px;margin-top:12px}
    button{border-radius:999px;border:0;padding:8px 12px;font-weight:700;cursor:pointer}
    .btn-primary{background:linear-gradient(90deg,var(--accent),#22c55e);color:#fff}
    .btn-ghost{background:#eef5ff;color:#0b1220;border:1px solid #e6eefc}
    .hint{color:var(--muted);font-size:13px;margin-top:8px}
    .filters{display:flex;flex-wrap:wrap;gap:8px;align-items:center;margin-bottom:8px}
    .filters input[type="text"],.filters select{max-width:220px;padding:8px}
    .news-list{display:flex;flex-direction:column;gap:10px;max-height:520px;overflow:auto;padding-right:6px}
    .news-card{padding:10px;border-radius:10px;background:#fff;border:1px solid #eef6ff}
    .news-title{font-weight:800;color:#0b1220}
    .meta{font-size:13px;color:var(--muted);margin-top:6px}
    .news-body{margin-top:8px;color:#233444;white-space:pre-line}
    .actions{display:flex;gap:8px;justify-content:flex-end;margin-top:8px}
    .small{padding:6px 8px;border-radius:8px;font-weight:700}
    .badge{display:inline-block;padding:3px 8px;border-radius:999px;font-weight:700;font-size:12px}
    .badge-cat{background:#e6f0ff;color:#1d4ed8}
    .badge-target{background:#ecfdf5;color:#15803d}
    .badge-pin{background:#fff7ed;color:#92400e}
    .note{font-size:13px;color:#7b8794;margin-top:8px}
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <div class="logo">สข</div>
      <div>
        <h1>ระบบประกาศข้อมูลข่าวสารโรงเรียน</h1>
        <div class="subtitle">ข้อมูลแชร์ร่วมกันผ่าน Firebase Firestore — ทุกคนที่เปิดลิงก์จะเห็นข่าวเดียวกัน</div>
      </div>
    </header>

    <div class="grid" style="margin-top:12px">
      <!-- left: form -->
      <div class="card">
        <h3>บันทึกข่าวสารใหม่</h3>

        <label>หัวข้อข่าว</label>
        <input id="newsTitle" type="text" placeholder="เช่น แจ้งกำหนดการประชุมผู้ปกครอง">

        <label>วันที่ประกาศ</label>
        <input id="newsDate" type="date">

        <label>หมวดหมู่</label>
        <select id="newsCategory">
          <option value="ทั่วไป">ทั่วไป</option>
          <option value="การเรียนการสอน">การเรียนการสอน</option>
          <option value="กิจกรรม">กิจกรรม</option>
          <option value="ประชุม/อบรม">ประชุม/อบรม</option>
          <option value="งานวิชาการ">งานวิชาการ</option>
          <option value="อื่น ๆ">อื่น ๆ</option>
        </select>

        <label>กลุ่มเป้าหมาย</label>
        <div id="targetGroup" class="checkbox-row">
          <label><input type="checkbox" value="นักเรียน"> นักเรียน</label>
          <label><input type="checkbox" value="ครู"> ครู</label>
          <label><input type="checkbox" value="ผู้ปกครอง"> ผู้ปกครอง</label>
          <label><input type="checkbox" value="บุคลากร"> บุคลากร</label>
          <label><input type="checkbox" value="บุคคลทั่วไป"> บุคคลทั่วไป</label>
        </div>

        <label>รายละเอียด</label>
        <textarea id="newsContent" placeholder="ระบุเวลา/สถานที่/คำแนะนำเพิ่มเติม"></textarea>

        <div class="btn-row">
          <button id="btnSave" class="btn-primary">บันทึกข่าว</button>
          <button id="btnClearForm" class="btn-ghost">ล้างแบบฟอร์ม</button>
        </div>

        <div class="note">
          ก่อนใช้งาน: แทนที่ค่า <code>firebaseConfig</code> ในสคริปต์ด้วยค่าจาก Firebase Console ของคุณ
        </div>
      </div>

      <!-- right: list + filters -->
      <div class="card">
        <h3>รายการข่าวสาร</h3>

        <div class="filters" style="margin-top:6px">
          <input id="searchInput" type="text" placeholder="ค้นหา หัวข้อหรือเนื้อหา" style="flex:1">
          <select id="filterCategory"><option value="">ทุกหมวดหมู่</option><option>ทั่วไป</option><option>การเรียนการสอน</option><option>กิจกรรม</option><option>ประชุม/อบรม</option><option>งานวิชาการ</option><option>อื่น ๆ</option></select>

          <!-- filter targets checkboxes -->
          <div style="display:flex;gap:6px;align-items:center">
            <label style="font-weight:700">กลุ่ม:</label>
            <label><input type="checkbox" class="filter-target" value="นักเรียน">นักเรียน</label>
            <label><input type="checkbox" class="filter-target" value="ครู">ครู</label>
            <label><input type="checkbox" class="filter-target" value="ผู้ปกครอง">ผู้ปกครอง</label>
            <label><input type="checkbox" class="filter-target" value="บุคลากร">บุคลากร</label>
            <label><input type="checkbox" class="filter-target" value="บุคคลทั่วไป">บุคคลทั่วไป</label>
          </div>

          <label style="margin-left:auto;display:flex;align-items:center;gap:8px">
            <input id="filterPinnedOnly" type="checkbox"> แสดงเฉพาะปักหมุด
          </label>
          <button id="btnClearAll" class="btn-ghost small">ลบข่าวทั้งหมด</button>
        </div>

        <div id="newsList" class="news-list"></div>
      </div>
    </div>
  </div>

  <!-- Firebase (compat) -->
  <script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-firestore-compat.js"></script>

  <script>
    // ===========================
    // TODO: REPLACE firebaseConfig WITH YOUR PROJECT'S CONFIG
    // Go to Firebase Console -> Project settings -> Add web app -> copy config
    // ===========================
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_AUTH_DOMAIN",
      projectId: "YOUR_PROJECT_ID",
      storageBucket: "YOUR_STORAGE_BUCKET",
      messagingSenderId: "YOUR_MSG_SENDER_ID",
      appId: "YOUR_APP_ID"
    };

    // Initialize Firebase
    firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();

    // DOM elements
    const newsTitle = document.getElementById("newsTitle");
    const newsDate = document.getElementById("newsDate");
    const newsCategory = document.getElementById("newsCategory");
    const newsContent = document.getElementById("newsContent");
    const targetGroup = document.getElementById("targetGroup");
    const btnSave = document.getElementById("btnSave");
    const btnClearForm = document.getElementById("btnClearForm");
    const btnClearAll = document.getElementById("btnClearAll");
    const newsListDiv = document.getElementById("newsList");
    const searchInput = document.getElementById("searchInput");
    const filterCategory = document.getElementById("filterCategory");
    const filterPinnedOnly = document.getElementById("filterPinnedOnly");
    const filterTargetCheckboxes = document.querySelectorAll(".filter-target");

    let newsItems = []; // local cache of docs

    // set today's date
    function setToday() {
      const t = new Date();
      const yyyy = t.getFullYear();
      const mm = String(t.getMonth() + 1).padStart(2,'0');
      const dd = String(t.getDate()).padStart(2,'0');
      newsDate.value = `${yyyy}-${mm}-${dd}`;
    }

    // helper: get selected targets from form
    function getFormTargets() {
      const arr = [];
      targetGroup.querySelectorAll("input[type='checkbox']").forEach(cb => {
        if(cb.checked) arr.push(cb.value);
      });
      return arr;
    }

    // helper: get selected filter targets (array) — OR logic (show if any match)
    function getFilterTargets() {
      const arr = [];
      filterTargetCheckboxes.forEach(cb => { if(cb.checked) arr.push(cb.value); });
      return arr;
    }

    // real-time listener: fetch collection ordered pinned desc then createdAt desc
    function startRealtime() {
      db.collection("schoolNews")
        .orderBy("pinned", "desc")
        .orderBy("createdAt", "desc")
        .onSnapshot(snapshot => {
          newsItems = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
          renderNews();
        }, err => {
          console.error("Firestore snapshot error:", err);
        });
    }

    // render with local filters
    function renderNews() {
      newsListDiv.innerHTML = "";
      const keyword = (searchInput.value || "").trim().toLowerCase();
      const catFilter = filterCategory.value;
      const pinnedOnly = filterPinnedOnly.checked;
      const selectedTargets = getFilterTargets();

      let items = [...newsItems];

      // filtering
      items = items.filter(item => {
        // pinned filter
        if(pinnedOnly && !item.pinned) return false;

        // keyword (title + content + targets)
        const text = ((item.title||"") + " " + (item.content||"") + " " + ((item.targets||[]).join(" "))).toLowerCase();
        if(keyword && !text.includes(keyword)) return false;

        // category
        if(catFilter && item.category !== catFilter) return false;

        // targets: if filter has selections, require OR match
        if(selectedTargets.length > 0) {
          if(!Array.isArray(item.targets) || item.targets.length === 0) return false;
          const any = selectedTargets.some(t => item.targets.includes(t));
          if(!any) return false;
        }

        return true;
      });

      if(items.length === 0) {
        const e = document.createElement("div");
        e.className = "empty-state";
        e.textContent = "ยังไม่มีข่าวหรือไม่พบข่าวตามเงื่อนไข";
        newsListDiv.appendChild(e);
        return;
      }

      items.forEach(item => {
        const card = document.createElement("div");
        card.className = "news-card";

        const title = document.createElement("div");
        title.className = "news-title";
        title.textContent = item.title || "(ไม่มีหัวข้อ)";

        const meta = document.createElement("div");
        meta.className = "meta";
        meta.innerHTML =
          `<span class="badge badge-cat">${item.category || 'ทั่วไป'}</span> ` +
          (item.pinned ? `<span class="badge badge-pin">ปักหมุด</span> ` : '') +
          `<span style="margin-left:8px;color:#577093">${item.date || '-'}</span>`;

        const targetLine = document.createElement("div");
        targetLine.className = "meta";
        targetLine.innerHTML = `<span class="badge badge-target">กลุ่ม: ${ (item.targets && item.targets.length) ? item.targets.join(", ") : 'ไม่ระบุ' }</span>`;

        const body = document.createElement("div");
        body.className = "news-body";
        body.textContent = item.content || "";

        const actions = document.createElement("div");
        actions.className = "actions";

        const pinBtn = document.createElement("button");
        pinBtn.className = "btn-ghost small";
        pinBtn.textContent = item.pinned ? "ยกเลิกปักหมุด" : "ปักหมุด";
        pinBtn.addEventListener("click", () => togglePin(item.id, !!item.pinned));

        const delBtn = document.createElement("button");
        delBtn.className = "btn-ghost small";
        delBtn.textContent = "ลบ";
        delBtn.addEventListener("click", () => deleteNews(item.id));

        actions.appendChild(pinBtn);
        actions.appendChild(delBtn);

        card.appendChild(title);
        card.appendChild(meta);
        card.appendChild(targetLine);
        card.appendChild(body);
        card.appendChild(actions);

        newsListDiv.appendChild(card);
      });
    }

    // add news -> Firestore
    async function addNews() {
      const title = (newsTitle.value || "").trim();
      const date = newsDate.value || "";
      const category = newsCategory.value || "";
      const content = (newsContent.value || "").trim();
      const targets = getFormTargets();

      if(!title) { alert("กรุณากรอกหัวข้อข่าว"); return; }

      try {
        await db.collection("schoolNews").add({
          title, date, category, content, targets,
          pinned: false,
          createdAt: Date.now()
        });
        clearForm();
        // no need to call renderNews explicitly — onSnapshot updates automatically
      } catch (err) {
        console.error("Add news error:", err);
        alert("เกิดข้อผิดพลาดขณะบันทึกข่าว ดู console สำหรับรายละเอียด");
      }
    }

    // toggle pinned
    async function togglePin(id, pinned) {
      try {
        await db.collection("schoolNews").doc(id).update({ pinned: !pinned });
      } catch (err) {
        console.error("Toggle pin error:", err);
        alert("เกิดข้อผิดพลาดขณะอัปเดตสถานะ");
      }
    }

    // delete single
    async function deleteNews(id) {
      if(!confirm("ต้องการลบข่าวนี้หรือไม่?")) return;
      try {
        await db.collection("schoolNews").doc(id).delete();
      } catch(err) {
        console.error("Delete error:", err);
        alert("เกิดข้อผิดพลาดขณะลบข่าว");
      }
    }

    // clear all -> batch delete (ใช้ระมัดระวัง)
    async function clearAllNews() {
      if(!confirm("ต้องการลบข่าวทั้งหมดหรือไม่?")) return;
      try {
        const snapshot = await db.collection("schoolNews").get();
        const batch = db.batch();
        snapshot.docs.forEach(doc => batch.delete(doc.ref));
        await batch.commit();
      } catch(err) {
        console.error("Clear all error:", err);
        alert("เกิดข้อผิดพลาดขณะลบทั้งหมด");
      }
    }

    // clear form
    function clearForm() {
      newsTitle.value = "";
      newsContent.value = "";
      newsCategory.value = "ทั่วไป";
      setToday();
      targetGroup.querySelectorAll("input[type=checkbox]").forEach(cb => cb.checked = false);
    }

    // listeners
    btnSave.addEventListener("click", addNews);
    btnClearForm.addEventListener("click", clearForm);
    btnClearAll.addEventListener("click", clearAllNews);
    searchInput.addEventListener("input", renderNews);
    filterCategory.addEventListener("change", renderNews);
    filterPinnedOnly.addEventListener("change", renderNews);
    filterTargetCheckboxes.forEach(cb => cb.addEventListener("change", renderNews));

    // start
    setToday();
    startRealtime();
  </script>
</body>
</html>
