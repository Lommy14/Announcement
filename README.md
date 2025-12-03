<html lang="th">
<head>
  <meta charset="UTF-8">
  <title>ระบบประกาศข้อมูลข่าวสารโรงเรียน</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;600&display=swap" rel="stylesheet">
  <style>
    * { box-sizing: border-box; font-family: "Sarabun", system-ui, sans-serif; }

    body {
      margin: 0;
      padding: 16px;
      background: #f3f6ff;
      display: flex;
      justify-content: center;
    }

    .container {
      width: 100%;
      max-width: 1100px;
      background: #ffffff;
      border-radius: 16px;
      padding: 20px 22px 26px;
      box-shadow: 0 8px 26px rgba(15,23,42,0.12);
      border: 1px solid #e0e7ff;
    }

    h1 {
      margin: 0;
      font-size: 24px;
      text-align: center;
      color: #1e3a8a;
    }
    .subtitle {
      text-align: center;
      font-size: 13px;
      color: #6b7280;
      margin-bottom: 14px;
    }

    .grid {
      display: grid;
      grid-template-columns: minmax(0, 360px) minmax(0, 1fr);
      gap: 18px;
    }
    @media (max-width: 900px) {
      .grid { grid-template-columns: 1fr; }
    }

    .card {
      border-radius: 14px;
      border: 1px solid #e5e7eb;
      background: #f9fbff;
      padding: 14px 16px;
    }

    h2 {
      margin: 0 0 6px;
      font-size: 18px;
      color: #1f2937;
    }

    label {
      display: block;
      font-size: 13px;
      font-weight: 600;
      color: #374151;
      margin-top: 8px;
    }

    input[type="text"],
    input[type="date"],
    select,
    textarea {
      width: 100%;
      padding: 8px 10px;
      margin-top: 4px;
      border-radius: 10px;
      border: 1px solid #cbd5e1;
      font-size: 14px;
      outline: none;
      background: #ffffff;
    }

    textarea {
      min-height: 80px;
      resize: vertical;
    }

    input:focus,
    select:focus,
    textarea:focus {
      border-color: #2563eb;
      box-shadow: 0 0 0 2px rgba(37,99,235,0.18);
    }

    .checkbox-row {
      display: flex;
      flex-wrap: wrap;
      gap: 6px 12px;
      margin-top: 4px;
      font-size: 13px;
    }
    .checkbox-row label {
      margin-top: 0;
      font-weight: 500;
      display: flex;
      align-items: center;
      gap: 4px;
    }

    .btn-row {
      display: flex;
      gap: 8px;
      margin-top: 12px;
    }

    button {
      border-radius: 999px;
      border: none;
      padding: 8px 14px;
      font-size: 14px;
      cursor: pointer;
      font-weight: 600;
      display: inline-flex;
      align-items: center;
      gap: 4px;
    }

    .btn-primary {
      background: linear-gradient(90deg, #2563eb, #22c55e);
      color: #ffffff;
    }
    .btn-ghost {
      background: #e5e7eb;
      color: #111827;
    }
    .btn-small {
      padding: 4px 10px;
      font-size: 12px;
    }

    .hint {
      font-size: 12px;
      color: #6b7280;
      margin-top: 4px;
    }

    .filter-row {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      align-items: center;
      margin-bottom: 10px;
      font-size: 13px;
    }

    .filter-row input[type="text"],
    .filter-row select {
      max-width: 220px;
    }

    .badge {
      display: inline-block;
      padding: 2px 8px;
      border-radius: 999px;
      font-size: 11px;
      font-weight: 600;
    }
    .badge-category { background: #dbeafe; color: #1d4ed8; }
    .badge-target { background: #dcfce7; color: #15803d; }
    .badge-pinned { background: #fef3c7; color: #92400e; }

    .news-list {
      display: flex;
      flex-direction: column;
      gap: 10px;
      max-height: 520px;
      overflow-y: auto;
      padding-right: 4px;
    }

    .news-card {
      border-radius: 12px;
      padding: 10px 12px;
      background: #ffffff;
      border: 1px solid #e5e7eb;
    }

    .news-header {
      display: flex;
      justify-content: space-between;
      gap: 8px;
      align-items: flex-start;
    }

    .news-title {
      font-weight: 700;
      font-size: 15px;
      color: #111827;
      margin-bottom: 2px;
    }

    .news-meta {
      font-size: 12px;
      color: #6b7280;
      margin-bottom: 4px;
    }

    .news-body {
      font-size: 13px;
      color: #374151;
      white-space: pre-line;
      margin-top: 4px;
    }

    .news-actions {
      display: flex;
      gap: 6px;
      margin-top: 6px;
      justify-content: flex-end;
    }

    .empty-state {
      font-size: 13px;
      color: #9ca3af;
      text-align: center;
      padding: 14px 4px;
    }
  </style>
</head>
<body>
<div class="container">
  <h1>ระบบประกาศข้อมูลข่าวสารโรงเรียน</h1>
  <p class="subtitle">บันทึกข่าวใหม่และแสดงข่าวทั้งหมดในที่เดียว (ข้อมูลเก็บไว้ที่เครื่องด้วย LocalStorage)</p>

  <div class="grid">
    <!-- ฟอร์มเพิ่มข่าว -->
    <div class="card">
      <h2>บันทึกข่าวสารใหม่</h2>

      <label for="newsTitle">หัวข้อข่าว</label>
      <input type="text" id="newsTitle" placeholder="เช่น แจ้งกำหนดการประชุมผู้ปกครอง">

      <label for="newsDate">วันที่ประกาศ</label>
      <input type="date" id="newsDate">

      <label for="newsCategory">หมวดหมู่ข่าว</label>
      <select id="newsCategory">
        <option value="ทั่วไป">ทั่วไป</option>
        <option value="การเรียนการสอน">การเรียนการสอน</option>
        <option value="กิจกรรม">กิจกรรม</option>
        <option value="ประชุม/อบรม">ประชุม/อบรม</option>
        <option value="งานวิชาการ">งานวิชาการ</option>
        <option value="อื่น ๆ">อื่น ๆ</option>
      </select>

      <label>กลุ่มเป้าหมาย</label>
      <div class="checkbox-row" id="targetGroup">
        <label><input type="checkbox" value="นักเรียน"> นักเรียน</label>
        <label><input type="checkbox" value="ครู"> ครู</label>
        <label><input type="checkbox" value="ผู้ปกครอง"> ผู้ปกครอง</label>
        <label><input type="checkbox" value="บุคลากร"> บุคลากร</label>
        <label><input type="checkbox" value="บุคคลทั่วไป"> บุคคลทั่วไป</label>
      </div>
      <div class="hint">เลือกได้มากกว่าหนึ่งข้อ</div>

      <label for="newsContent">รายละเอียดข่าว</label>
      <textarea id="newsContent" placeholder="รายละเอียดโดยย่อ เช่น วันเวลา สถานที่ เงื่อนไข..."></textarea>

      <div class="btn-row">
        <button class="btn-primary" id="btnSave">บันทึกข่าว</button>
        <button class="btn-ghost" id="btnClearForm" type="button">ล้างแบบฟอร์ม</button>
      </div>

      <p class="hint">
        หมายเหตุ: ข่าวที่บันทึกจะเก็บไว้ในเบราว์เซอร์เครื่องนี้ (LocalStorage)<br>
        หากเปิดจากเครื่องอื่น จะไม่เห็นข่าวชุดนี้
      </p>
    </div>

    <!-- รายการข่าว -->
    <div class="card">
      <h2>รายการข่าวสาร</h2>

      <div class="filter-row">
        <span>ค้นหา:</span>
        <input type="text" id="searchInput" placeholder="พิมพ์คำค้นหัวข้อ / รายละเอียด">
        <select id="filterCategory">
          <option value="">ทุกหมวดหมู่</option>
          <option value="ทั่วไป">ทั่วไป</option>
          <option value="การเรียนการสอน">การเรียนการสอน</option>
          <option value="กิจกรรม">กิจกรรม</option>
          <option value="ประชุม/อบรม">ประชุม/อบรม</option>
          <option value="งานวิชาการ">งานวิชาการ</option>
          <option value="อื่น ๆ">อื่น ๆ</option>
        </select>
        <button class="btn-ghost btn-small" id="btnClearAll">ลบข่าวทั้งหมด</button>
      </div>

      <div id="newsList" class="news-list"></div>
    </div>
  </div>
</div>

<script>
  // คีย์สำหรับ localStorage
  const STORAGE_KEY = "schoolNewsList";

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

  let newsItems = [];

  // ตั้งค่าวันที่วันนี้เป็นค่าเริ่มต้น
  function setTodayDefault() {
    const today = new Date();
    const yyyy = today.getFullYear();
    const mm = String(today.getMonth() + 1).padStart(2, "0");
    const dd = String(today.getDate()).padStart(2, "0");
    newsDate.value = `${yyyy}-${mm}-${dd}`;
  }

  // โหลดข่าวจาก localStorage
  function loadNews() {
    const saved = localStorage.getItem(STORAGE_KEY);
    if (saved) {
      try {
        newsItems = JSON.parse(saved);
      } catch (e) {
        newsItems = [];
      }
    }
  }

  // บันทึกลง localStorage
  function saveNews() {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(newsItems));
  }

  // ล้างฟอร์ม
  function clearForm() {
    newsTitle.value = "";
    newsContent.value = "";
    newsCategory.value = "ทั่วไป";
    setTodayDefault();
    const checkboxes = targetGroup.querySelectorAll("input[type='checkbox']");
    checkboxes.forEach(cb => cb.checked = false);
  }

  // เรนเดอร์รายการข่าว
  function renderNews() {
    newsListDiv.innerHTML = "";

    // กรองตามค้นหา และหมวดหมู่
    const keyword = searchInput.value.trim().toLowerCase();
    const catFilter = filterCategory.value;

    let items = [...newsItems];

    // ปักหมุดขึ้นก่อน
    items.sort((a, b) => {
      if (a.pinned && !b.pinned) return -1;
      if (!a.pinned && b.pinned) return 1;
      // ถ้า pin เท่ากัน เรียงตามวันที่ (ใหม่อยู่บน)
      return (b.createdAt || 0) - (a.createdAt || 0);
    });

    items = items.filter(item => {
      const text = (item.title + " " + item.content).toLowerCase();
      const matchKeyword = keyword === "" || text.includes(keyword);
      const matchCat = !catFilter || item.category === catFilter;
      return matchKeyword && matchCat;
    });

    if (items.length === 0) {
      const empty = document.createElement("div");
      empty.className = "empty-state";
      empty.textContent = "ยังไม่มีข่าว หรือไม่พบข่าวตามเงื่อนไข";
      newsListDiv.appendChild(empty);
      return;
    }

    items.forEach(item => {
      const card = document.createElement("div");
      card.className = "news-card";

      const header = document.createElement("div");
      header.className = "news-header";

      const left = document.createElement("div");

      const title = document.createElement("div");
      title.className = "news-title";
      title.textContent = item.title;

      const meta = document.createElement("div");
      meta.className = "news-meta";

      const dateStr = item.date || "-";
      meta.innerHTML =
        `<span class="badge badge-category">${item.category}</span> ` +
        (item.pinned ? `<span class="badge badge-pinned">ปักหมุด</span> ` : "") +
        `<span>ประกาศเมื่อ: ${dateStr}</span>`;

      const targetBadge = document.createElement("div");
      targetBadge.className = "news-meta";

      if (item.targets && item.targets.length > 0) {
        targetBadge.innerHTML =
          `<span class="badge badge-target">กลุ่มเป้าหมาย: ${item.targets.join(", ")}</span>`;
      } else {
        targetBadge.innerHTML =
          `<span class="badge badge-target">กลุ่มเป้าหมาย: ไม่ระบุ</span>`;
      }

      left.appendChild(title);
      left.appendChild(meta);
      left.appendChild(targetBadge);

      header.appendChild(left);

      const body = document.createElement("div");
      body.className = "news-body";
      body.textContent = item.content || "";

      const actions = document.createElement("div");
      actions.className = "news-actions";

      const btnPin = document.createElement("button");
      btnPin.className = "btn-ghost btn-small";
      btnPin.textContent = item.pinned ? "ยกเลิกปักหมุด" : "ปักหมุด";
      btnPin.addEventListener("click", () => togglePin(item.id));

      const btnDelete = document.createElement("button");
      btnDelete.className = "btn-ghost btn-small";
      btnDelete.textContent = "ลบ";
      btnDelete.addEventListener("click", () => deleteNews(item.id));

      actions.appendChild(btnPin);
      actions.appendChild(btnDelete);

      card.appendChild(header);
      card.appendChild(body);
      card.appendChild(actions);

      newsListDiv.appendChild(card);
    });
  }

  // เพิ่มข่าวใหม่
  function addNews() {
    const title = newsTitle.value.trim();
    const date = newsDate.value;
    const category = newsCategory.value;
    const content = newsContent.value.trim();

    if (!title) {
      alert("กรุณากรอกหัวข้อข่าว");
      return;
    }

    const checkboxes = targetGroup.querySelectorAll("input[type='checkbox']");
    const targets = [];
    checkboxes.forEach(cb => {
      if (cb.checked) targets.push(cb.value);
    });

    const item = {
      id: Date.now(),
      title,
      date,
      category,
      content,
      targets,
      pinned: false,
      createdAt: Date.now()
    };

    newsItems.push(item);
    saveNews();
    renderNews();
    clearForm();
  }

  function togglePin(id) {
    const index = newsItems.findIndex(i => i.id === id);
    if (index !== -1) {
      newsItems[index].pinned = !newsItems[index].pinned;
      saveNews();
      renderNews();
    }
  }

  function deleteNews(id) {
    if (!confirm("ต้องการลบข่าวนี้หรือไม่?")) return;
    newsItems = newsItems.filter(i => i.id !== id);
    saveNews();
    renderNews();
  }

  function clearAllNews() {
    if (!confirm("ต้องการลบข่าวทั้งหมดหรือไม่?")) return;
    newsItems = [];
    saveNews();
    renderNews();
  }

  // Event listeners
  btnSave.addEventListener("click", addNews);
  btnClearForm.addEventListener("click", clearForm);
  btnClearAll.addEventListener("click", clearAllNews);
  searchInput.addEventListener("input", renderNews);
  filterCategory.addEventListener("change", renderNews);

  // เริ่มต้น
  setTodayDefault();
  loadNews();
  renderNews();
</script>
</body>
</html>
