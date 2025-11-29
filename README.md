<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <title>เกมคำนวณแคลอรี่ & การออกกำลังกาย</title>
  <style>
    * {
      box-sizing: border-box;
      font-family: "Sarabun", system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    body {
      margin: 0;
      padding: 0;
      background: #f3f6fb;
      display: flex;
      justify-content: center;
      align-items: flex-start;
      min-height: 100vh;
    }

    .container {
      max-width: 1100px;
      width: 100%;
      margin: 24px;
      background: #ffffff;
      border-radius: 16px;
      padding: 24px 28px 32px;
      box-shadow: 0 12px 30px rgba(0, 0, 0, 0.08);
    }

    h1 {
      margin-top: 0;
      font-size: 26px;
      text-align: center;
      color: #1f3b70;
    }

    h2 {
      font-size: 20px;
      margin-bottom: 8px;
      color: #1f3b70;
    }

    p {
      margin: 4px 0 8px;
      font-size: 14px;
      color: #444;
    }

    .flex {
      display: flex;
      gap: 16px;
      flex-wrap: wrap;
    }

    .card {
      background: #f9fbff;
      border-radius: 12px;
      padding: 16px 18px;
      flex: 1 1 320px;
      min-width: 260px;
      border: 1px solid #e2e8f0;
    }

    label {
      font-size: 14px;
      font-weight: 600;
      color: #1e293b;
      display: block;
      margin-bottom: 6px;
    }

    select, input[type="number"] {
      width: 100%;
      padding: 8px 10px;
      border-radius: 8px;
      border: 1px solid #cbd5e1;
      font-size: 14px;
      outline: none;
    }

    select:focus, input[type="number"]:focus {
      border-color: #2563eb;
      box-shadow: 0 0 0 2px rgba(37, 99, 235, 0.18);
    }

    button {
      border: none;
      border-radius: 999px;
      padding: 8px 16px;
      font-size: 14px;
      cursor: pointer;
      background: #2563eb;
      color: #ffffff;
      font-weight: 600;
      display: inline-flex;
      align-items: center;
      gap: 4px;
      margin-top: 8px;
    }

    button.secondary {
      background: #64748b;
    }

    button.small {
      padding: 6px 12px;
      font-size: 13px;
    }

    button:disabled {
      opacity: 0.6;
      cursor: not-allowed;
    }

    .badge {
      display: inline-block;
      padding: 3px 10px;
      border-radius: 999px;
      font-size: 12px;
      background: #e0edff;
      color: #1d4ed8;
      margin-right: 4px;
    }

    .age-info {
      font-size: 13px;
      margin-top: 6px;
      color: #475569;
    }

    .tabs {
      display: flex;
      margin-top: 16px;
      margin-bottom: 8px;
      border-radius: 999px;
      background: #e2e8f0;
      padding: 4px;
    }

    .tab {
      flex: 1;
      text-align: center;
      padding: 8px 10px;
      font-size: 14px;
      cursor: pointer;
      border-radius: 999px;
      transition: background 0.2s, color 0.2s;
      user-select: none;
    }

    .tab.active {
      background: #ffffff;
      color: #1d4ed8;
      font-weight: 600;
      box-shadow: 0 1px 4px rgba(15, 23, 42, 0.15);
    }

    .tab-content {
      margin-top: 12px;
    }

    .question-box {
      margin-top: 8px;
      padding: 12px;
      border-radius: 12px;
      background: #ffffff;
      border: 1px solid #e2e8f0;
    }

    .question-title {
      font-size: 16px;
      font-weight: 600;
      margin-bottom: 6px;
      color: #0f172a;
    }

    .question-sub {
      font-size: 13px;
      color: #64748b;
      margin-bottom: 8px;
    }

    .status-row {
      display: flex;
      justify-content: space-between;
      font-size: 13px;
      color: #475569;
      margin-top: 8px;
    }

    .result {
      margin-top: 8px;
      padding: 8px 10px;
      border-radius: 8px;
      font-size: 13px;
      background: #eff6ff;
      color: #1d4ed8;
    }

    .result.error {
      background: #fef2f2;
      color: #b91c1c;
    }

    .summary {
      margin-top: 10px;
      padding: 10px;
      border-radius: 10px;
      background: #f1f5f9;
      font-size: 13px;
      color: #0f172a;
    }

    .summary strong {
      color: #1d4ed8;
    }

    .pill-row {
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
      margin-top: 4px;
    }

    .pill {
      padding: 4px 10px;
      border-radius: 999px;
      font-size: 12px;
      background: #e5e7eb;
      color: #374151;
    }

    .hint {
      font-size: 12px;
      color: #6b7280;
      margin-top: 4px;
    }

    .disclaimer {
      margin-top: 16px;
      font-size: 11px;
      color: #6b7280;
      border-top: 1px dashed #cbd5e1;
      padding-top: 8px;
    }

    @media (max-width: 768px) {
      .container {
        margin: 12px;
        padding: 20px;
      }
    }
  </style>
</head>
<body>
<div class="container">
  <h1>🎮 เกมคำนวณแคลอรี่ & การออกกำลังกายในชีวิตประจำวัน</h1>
  <p style="text-align:center;font-size:13px;color:#64748b;">
    เลือกช่วงอายุ → เล่นเกมทายแคลอรี่อาหาร → ดูว่าจะเผาผลาญด้วยการออกกำลังกายส่วนไหนของร่างกายได้บ้าง
  </p>

  <!-- ส่วนเลือกช่วงอายุ -->
  <div class="card" style="margin-bottom:12px;">
    <h2>1️⃣ เลือกช่วงอายุ</h2>
    <label for="ageGroup">ช่วงอายุของผู้เล่น</label>
    <select id="ageGroup">
      <option value="teen">13–18 ปี (วัยรุ่น)</option>
      <option value="adult">19–59 ปี (ผู้ใหญ่)</option>
      <option value="child">7–12 ปี (เด็ก)</option>
      <option value="senior">60 ปีขึ้นไป (ผู้สูงอายุ)</option>
    </select>
    <div id="ageInfo" class="age-info"></div>
  </div>

  <!-- แท็บเลือกโหมดเกม -->
  <div class="tabs">
    <div class="tab active" data-tab="food">โหมดที่ 1: เกมทายแคลอรี่อาหาร</div>
    <div class="tab" data-tab="exercise">โหมดที่ 2: เกมเลือกการออกกำลังกาย</div>
  </div>

  <div class="tab-content">
    <div id="tab-food">
      <div class="flex">
        <!-- การเล่นเกมอาหาร -->
        <div class="card">
          <h2>2️⃣ เกมทายแคลอรี่</h2>
          <p>ระบบจะสุ่มอาหาร/เครื่องดื่มมาให้ 1 อย่าง ให้ลองเดาว่ามีแคลอรี่ประมาณเท่าไร (ต่อ 1 หน่วยที่ระบุ)</p>

          <button id="btnNewQuestion">เริ่มคำถามใหม่ / เปลี่ยนเมนู</button>

          <div id="foodQuestionBox" class="question-box" style="display:none;">
            <div class="badge" id="foodIndexBadge">คำถามข้อ 1/5</div>
            <div class="question-title" id="foodName">ชื่ออาหาร</div>
            <div class="question-sub">ลองเดาแคลอรี่ (กิโลแคลอรี่) ของเมนูนี้</div>

            <label for="calInput">กรอกแคลอรี่ที่คุณเดา</label>
            <input type="number" id="calInput" placeholder="เช่น 250" min="0">
            <button id="btnCheckFood" class="small">ตรวจคำตอบ</button>

            <div class="hint">💡 เฉลยจะถือว่าถูก ถ้าเดาอยู่ในช่วง ±50 kcal จากค่าจริง</div>

            <div id="foodResult" class="result" style="display:none;"></div>

            <div class="status-row">
              <span>คะแนนสะสม: <strong id="scoreText">0</strong> คะแนน</span>
              <span>แคลอรี่ที่กินจากเกมนี้: <strong id="totalEaten">0</strong> kcal</span>
            </div>
          </div>

          <div id="foodSummary" class="summary" style="display:none;"></div>
        </div>

        <!-- ข้อมูลตัวอย่างเมนู -->
        <div class="card">
          <h2>🍚 ตัวอย่างเมนูในเกม</h2>
          <p>ใช้ตัวอย่างอาหาร/เครื่องดื่มในชีวิตประจำวันของคนไทย</p>
          <div class="pill-row" id="foodListPreview"></div>
          <p class="hint">
            * ตัวเลขแคลอรี่เป็นค่าใกล้เคียง ใช้เพื่อการเรียนรู้ ไม่ใช่คำแนะนำทางการแพทย์
          </p>
        </div>
      </div>
    </div>

    <div id="tab-exercise" style="display:none;">
      <div class="flex">
        <!-- เลือกส่วนของร่างกาย -->
        <div class="card">
          <h2>3️⃣ เลือกส่วนของร่างกาย & ท่าบริหาร</h2>
          <label for="bodyPart">เลือกส่วนของร่างกายที่อยากเน้นออกกำลังกาย</label>
          <select id="bodyPart">
            <option value="legs">ขา / หัวใจและปอด</option>
            <option value="arms">แขน / ไหล่</option>
            <option value="core">ลำตัว / หน้าท้อง</option>
            <option value="full">ทั้งตัว</option>
          </select>

          <label for="exerciseSelect" style="margin-top:10px;">เลือกท่าออกกำลังกาย</label>
          <select id="exerciseSelect"></select>

          <label for="minutesInput" style="margin-top:10px;">ระยะเวลาที่ออกกำลังกาย (นาที)</label>
          <input type="number" id="minutesInput" value="30" min="5" step="5">

          <button id="btnCalcBurn">คำนวณแคลอรี่ที่เผาผลาญ</button>

          <div id="exerciseResult" class="result" style="display:none;"></div>

          <div class="summary" id="compareSummary" style="display:none;"></div>
        </div>

        <!-- ตัวอย่างท่าออกกำลังกาย -->
        <div class="card">
          <h2>🏃‍♀️ ตัวอย่างท่าออกกำลังกายในเกม</h2>
          <p>ใช้ค่าประมาณแคลอรี่ที่เผาผลาญได้ต่อ 1 นาที</p>
          <ul style="font-size:13px;color:#475569;padding-left:18px;">
            <li>เดินเร็ว (ขา): ~4 kcal/นาที</li>
            <li>วิ่งเบา (ขา): ~8 kcal/นาที</li>
            <li>ปั่นจักรยาน (ทั้งตัว): ~7 kcal/นาที</li>
            <li>กระโดดเชือก (ทั้งตัว): ~10 kcal/นาที</li>
            <li>วิดพื้น (แขน/หน้าอก): ~6 kcal/นาที</li>
            <li>แพลงก์ / ซิทอัพ (หน้าท้อง): ~5 kcal/นาที</li>
            <li>โยคะเบา ๆ (ทั้งตัว): ~3 kcal/นาที</li>
          </ul>
          <p class="hint">
            ผู้เล่นสามารถทดลองเปลี่ยนเวลา / ท่าออกกำลังกาย เพื่อดูว่าเผาผลาญแคลอรี่ใกล้เคียงกับที่กินจากเกมโหมดที่ 1 ได้หรือไม่
          </p>
        </div>
      </div>
    </div>
  </div>

  <div class="disclaimer">
    ⚠️ <strong>หมายเหตุสำคัญ:</strong> เกมนี้ใช้ตัวเลขแคลอรี่แบบประมาณการเพื่อการศึกษาและฝึกคิดวิเคราะห์เท่านั้น
    ไม่ใช่คำแนะนำทางการแพทย์หรือโภชนาการจริง ๆ หากต้องการควบคุมน้ำหนักหรือวางแผนอาหาร
    ควรปรึกษาแพทย์หรือผู้เชี่ยวชาญด้านโภชนาการเพิ่มเติม
  </div>
</div>

<script>
  // -------------------------------
  // ข้อมูลช่วงอายุ & พลังงานที่แนะนำต่อวัน (ค่าประมาณ)
  // -------------------------------
  const ageGroups = {
    child: {
      label: "7–12 ปี (เด็ก)",
      recommend: "ประมาณ 1,600–2,000 kcal/วัน (ขึ้นกับเพศและกิจกรรม)"
    },
    teen: {
      label: "13–18 ปี (วัยรุ่น)",
      recommend: "ประมาณ 2,000–2,400 kcal/วัน (ถ้าออกกำลังกายปานกลาง)"
    },
    adult: {
      label: "19–59 ปี (ผู้ใหญ่)",
      recommend: "ประมาณ 1,800–2,400 kcal/วัน (ขึ้นกับงานและการเคลื่อนไหว)"
    },
    senior: {
      label: "60 ปีขึ้นไป (ผู้สูงอายุ)",
      recommend: "ประมาณ 1,600–2,000 kcal/วัน (มักใช้พลังงานน้อยลง)"
    }
  };

  // -------------------------------
  // ข้อมูลอาหารตัวอย่าง (ค่าประมาณต่อ 1 หน่วยที่ระบุ)
  // -------------------------------
  const foods = [
    { name: "ข้าวมันไก่ 1 จาน", calories: 600 },
    { name: "ข้าวผัดกะเพราไก่ + ไข่ดาว 1 จาน", calories: 650 },
    { name: "ส้มตำไทย 1 จาน", calories: 120 },
    { name: "ขนมครก 2 ชิ้น", calories: 200 },
    { name: "ไก่ปิ้ง 1 ไม้", calories: 100 },
    { name: "ข้าวสวย 1 ทัพพี", calories: 80 },
    { name: "ชานมไข่มุก 1 แก้ว (500 ml)", calories: 300 },
    { name: "โค้กกระป๋อง 1 กระป๋อง (330 ml)", calories: 140 },
    { name: "น้ำส้ม 1 แก้ว (250 ml)", calories: 110 },
    { name: "นมจืด UHT 1 กล่อง (250 ml)", calories: 130 }
  ];

  // -------------------------------
  // ข้อมูลท่าออกกำลังกาย (แคลอรี่ที่เผาผลาญต่อ 1 นาที)
  // -------------------------------
  const exercises = [
    { part: "legs",  name: "เดินเร็ว",             burnPerMin: 4 },
    { part: "legs",  name: "วิ่งเหยาะ ๆ",          burnPerMin: 8 },
    { part: "legs",  name: "วิ่งสลับเดิน (อินเตอร์วอล)", burnPerMin: 7 },

    { part: "arms",  name: "วิดพื้น",              burnPerMin: 6 },
    { part: "arms",  name: "ดัมเบลเบา ๆ",         burnPerMin: 5 },
    { part: "arms",  name: "แพลงก์ท่าดันแขน",     burnPerMin: 6 },

    { part: "core",  name: "ซิทอัพ",              burnPerMin: 5 },
    { part: "core",  name: "แพลงก์",              burnPerMin: 5 },
    { part: "core",  name: "ครันช์หน้าท้อง",      burnPerMin: 5 },

    { part: "full",  name: "ปั่นจักรยาน",         burnPerMin: 7 },
    { part: "full",  name: "กระโดดเชือก",         burnPerMin: 10 },
    { part: "full",  name: "โยคะเบา ๆ",           burnPerMin: 3 }
  ];

  // ตัวแปรสเตทของเกมอาหาร
  let currentFood = null;
  let questionIndex = 0;
  const maxQuestions = 5;
  let score = 0;
  let totalEatenCalories = 0;
  let gameFinished = false;

  // อ้างอิง element ต่าง ๆ
  const ageGroupSelect = document.getElementById("ageGroup");
  const ageInfoDiv = document.getElementById("ageInfo");

  const tabs = document.querySelectorAll(".tab");
  const tabFood = document.getElementById("tab-food");
  const tabExercise = document.getElementById("tab-exercise");

  const btnNewQuestion = document.getElementById("btnNewQuestion");
  const foodQuestionBox = document.getElementById("foodQuestionBox");
  const foodNameDiv = document.getElementById("foodName");
  const foodIndexBadge = document.getElementById("foodIndexBadge");
  const calInput = document.getElementById("calInput");
  const btnCheckFood = document.getElementById("btnCheckFood");
  const foodResult = document.getElementById("foodResult");
  const scoreText = document.getElementById("scoreText");
  const totalEaten = document.getElementById("totalEaten");
  const foodSummary = document.getElementById("foodSummary");
  const foodListPreview = document.getElementById("foodListPreview");

  const bodyPartSelect = document.getElementById("bodyPart");
  const exerciseSelect = document.getElementById("exerciseSelect");
  const minutesInput = document.getElementById("minutesInput");
  const btnCalcBurn = document.getElementById("btnCalcBurn");
  const exerciseResult = document.getElementById("exerciseResult");
  const compareSummary = document.getElementById("compareSummary");

  // -------------------------------
  // แสดงข้อมูลช่วงอายุ
  // -------------------------------
  function updateAgeInfo() {
    const key = ageGroupSelect.value;
    const info = ageGroups[key];
    ageInfoDiv.innerHTML = `<span class="badge">ช่วงอายุ: ${info.label}</span> แคลอรี่ที่ร่างกายต้องการต่อวันโดยประมาณ: <strong>${info.recommend}</strong>`;
  }

  ageGroupSelect.addEventListener("change", updateAgeInfo);
  updateAgeInfo(); // เรียกครั้งแรก

  // -------------------------------
  // ระบบแท็บ (โหมดเกม)
  // -------------------------------
  tabs.forEach(tab => {
    tab.addEventListener("click", () => {
      tabs.forEach(t => t.classList.remove("active"));
      tab.classList.add("active");

      const tabName = tab.getAttribute("data-tab");
      if (tabName === "food") {
        tabFood.style.display = "block";
        tabExercise.style.display = "none";
      } else {
        tabFood.style.display = "none";
        tabExercise.style.display = "block";
      }
    });
  });

  // -------------------------------
  // แสดงรายการตัวอย่างเมนูทางขวา
  // -------------------------------
  function renderFoodPreview() {
    foodListPreview.innerHTML = "";
    foods.forEach(f => {
      const pill = document.createElement("div");
      pill.className = "pill";
      pill.textContent = `${f.name} ~ ${f.calories} kcal`;
      foodListPreview.appendChild(pill);
    });
  }
  renderFoodPreview();

  // -------------------------------
  // เกมอาหาร: สุ่มคำถามใหม่
  // -------------------------------
  function newFoodQuestion() {
    if (gameFinished) {
      // ถ้าเคลียร์จบไปแล้ว เริ่มเกมใหม่
      questionIndex = 0;
      score = 0;
      totalEatenCalories = 0;
      gameFinished = false;
      foodSummary.style.display = "none";
      scoreText.textContent = "0";
      totalEaten.textContent = "0";
    }

    if (questionIndex >= maxQuestions) {
      showFoodSummary();
      return;
    }

    // สุ่มเมนู
    const randomIndex = Math.floor(Math.random() * foods.length);
    currentFood = foods[randomIndex];

    questionIndex++;
    foodIndexBadge.textContent = `คำถามข้อ ${questionIndex}/${maxQuestions}`;
    foodNameDiv.textContent = currentFood.name;
    calInput.value = "";
    foodResult.style.display = "none";
    foodResult.classList.remove("error");
    foodQuestionBox.style.display = "block";
  }

  btnNewQuestion.addEventListener("click", newFoodQuestion);

  // -------------------------------
  // เกมอาหาร: ตรวจคำตอบ
  // -------------------------------
  btnCheckFood.addEventListener("click", () => {
    if (!currentFood) return;

    const val = parseFloat(calInput.value);
    if (isNaN(val) || val < 0) {
      foodResult.textContent = "กรุณากรอกตัวเลขแคลอรี่ให้ถูกต้องก่อนนะ";
      foodResult.classList.add("error");
      foodResult.style.display = "block";
      return;
    }

    const diff = Math.abs(val - currentFood.calories);
    const tolerance = 50; // ยอมให้ต่างได้ ±50 kcal

    if (diff <= tolerance) {
      score++;
      foodResult.classList.remove("error");
      foodResult.innerHTML =
        `✅ ดีมาก! นับว่าตอบได้ใกล้เคียง<br>` +
        `เมนู <strong>${currentFood.name}</strong> มีประมาณ <strong>${currentFood.calories} kcal</strong> (คำตอบของคุณ: ${val} kcal)`;
    } else {
      foodResult.classList.add("error");
      foodResult.innerHTML =
        `❌ ตอบห่างไปหน่อยนะ ลองสังเกตเมนูรอบตัวในชีวิตจริงดูอีกที<br>` +
        `เฉลย: <strong>${currentFood.name}</strong> มีประมาณ <strong>${currentFood.calories} kcal</strong> (คำตอบของคุณ: ${val} kcal)`;
    }

    foodResult.style.display = "block";
    totalEatenCalories += currentFood.calories;
    scoreText.textContent = score.toString();
    totalEaten.textContent = totalEatenCalories.toString();

    if (questionIndex >= maxQuestions) {
      gameFinished = true;
      showFoodSummary();
    }
  });

  // -------------------------------
  // สรุปเกมอาหาร
  // -------------------------------
  function showFoodSummary() {
    const ageKey = ageGroupSelect.value;
    const info = ageGroups[ageKey];

    foodSummary.style.display = "block";
    foodSummary.innerHTML =
      `<strong>สรุปโหมดเกมอาหาร</strong><br>` +
      `คุณเล่นครบ ${maxQuestions} เมนู ได้คะแนนถูกใกล้เคียงทั้งหมด <strong>${score} ข้อ</strong><br>` +
      `รวมแคลอรี่ที่ได้จากเมนูในเกม: <strong>${totalEatenCalories} kcal</strong><br><br>` +
      `หากเทียบกับช่วงอายุ <strong>${info.label}</strong> ที่ต้องการพลังงานต่อวันประมาณ <strong>${info.recommend}</strong><br>` +
      `ลองชวนผู้เล่นคิดต่อว่า ถ้าในชีวิตจริงกินคล้าย ๆ เกมในวันนี้ แล้วออกกำลังกายพอหรือไม่?`;
  }

  // -------------------------------
  // ส่วนออกกำลังกาย: โหลดท่าตามส่วนร่างกาย
  // -------------------------------
  function populateExerciseOptions() {
    const part = bodyPartSelect.value;
    const options = exercises.filter(e => e.part === part);

    exerciseSelect.innerHTML = "";
    options.forEach(e => {
      const opt = document.createElement("option");
      opt.value = e.name;
      opt.textContent = `${e.name} (~${e.burnPerMin} kcal/นาที)`;
      exerciseSelect.appendChild(opt);
    });
  }

  bodyPartSelect.addEventListener("change", populateExerciseOptions);
  populateExerciseOptions(); // ครั้งแรก

  // -------------------------------
  // คำนวณแคลอรี่ที่เผาผลาญจากการออกกำลังกาย
  // -------------------------------
  btnCalcBurn.addEventListener("click", () => {
    const minutes = parseFloat(minutesInput.value);
    if (isNaN(minutes) || minutes <= 0) {
      exerciseResult.textContent = "กรุณากรอกจำนวนนาทีให้ถูกต้อง";
      exerciseResult.classList.add("error");
      exerciseResult.style.display = "block";
      compareSummary.style.display = "none";
      return;
    }

    const selectedName = exerciseSelect.value;
    const ex = exercises.find(e => e.name === selectedName && e.part === bodyPartSelect.value);
    if (!ex) return;

    const burned = Math.round(ex.burnPerMin * minutes);
    exerciseResult.classList.remove("error");
    exerciseResult.innerHTML =
      `🔥 ถ้าออกกำลังกายท่า <strong>${ex.name}</strong> เป็นเวลา <strong>${minutes} นาที</strong><br>` +
      `จะเผาผลาญได้ประมาณ <strong>${burned} kcal</strong> (เป็นค่าประมาณ)`;
    exerciseResult.style.display = "block";

    // เปรียบเทียบกับแคลอรี่ที่กินจากโหมดเกมอาหาร
    if (totalEatenCalories > 0) {
      const ratio = (burned / totalEatenCalories) * 100;
      let msg;
      if (burned >= totalEatenCalories) {
        msg = `✅ พลังงานที่เผาผลาญจากการออกกำลังกายในรอบนี้ใกล้เคียงหรือมากกว่าจำนวนแคลอรี่ที่คุณได้จากเกมอาหาร (${totalEatenCalories} kcal) แล้ว`;
      } else {
        msg = `ℹ️ พลังงานที่เผาผลาญจากการออกกำลังกายยังน้อยกว่าที่ได้จากเกมอาหาร (${totalEatenCalories} kcal) อาจต้องเพิ่มเวลา หรือสลับท่าออกกำลังกาย`;
      }

      compareSummary.style.display = "block";
      compareSummary.innerHTML =
        `<strong>เปรียบเทียบกับแคลอรี่จากเกมอาหาร</strong><br>` +
        `แคลอรี่จากอาหารในเกม: <strong>${totalEatenCalories} kcal</strong><br>` +
        `แคลอรี่ที่เผาผลาญจากการออกกำลังกายครั้งนี้: <strong>${burned} kcal</strong> (~${ratio.toFixed(1)}% ของที่กินจากเกม)<br>` +
        msg +
        `<br><br>💡 ผู้สอนสามารถชวนผู้เรียนลองสลับท่า/เพิ่มเวลาการออกกำลังกาย แล้วให้ผู้เรียนสะท้อนว่า<br>` +
        `"จะจัดสมดุลระหว่างการกินกับการใช้พลังงานในชีวิตจริงอย่างไรให้เหมาะกับตัวเอง"`;
    } else {
      compareSummary.style.display = "block";
      compareSummary.innerHTML =
        `ตอนนี้คุณยังไม่ได้เล่นโหมดเกมอาหาร หรือยังไม่สะสมแคลอรี่จากเกม<br>` +
        `ลองกลับไปเล่นโหมดที่ 1 แล้วกลับมาดูว่าต้องออกกำลังกายเท่าไรถึงจะใกล้เคียงกัน 😊`;
    }
  });
</script>
</body>
</html>
