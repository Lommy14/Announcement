<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="utf-8" />
  <title>เครื่องมือสุขภาพ — คำนวณ BMI และอัตราการเต้นของหัวใจตามช่วงอายุ</title>
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;600;700&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg:#f5f9ff;
      --card:#ffffff;
      --accent:#0f4fd8;
      --muted:#6b7280;
      --glass: rgba(15,23,42,0.04);
    }
    *{box-sizing:border-box;font-family:"Sarabun",system-ui,-apple-system,"Segoe UI",sans-serif}
    body{margin:0;background:var(--bg);display:flex;justify-content:center;padding:28px 12px}
    .container{max-width:1000px;width:100%;background:var(--card);border-radius:14px;padding:20px;border:1px solid var(--glass);box-shadow:0 12px 30px rgba(15,23,42,0.06)}
    h1{margin:0;font-size:22px;color:var(--accent)}
    p.lead{margin:8px 0 16px;color:var(--muted);font-size:14px}
    .grid{display:grid;grid-template-columns:1fr 380px;gap:16px}
    @media(max-width:980px){.grid{grid-template-columns:1fr}}
    .card{background:#fdfeff;border-radius:12px;padding:14px;border:1px solid #e6f0ff}
    label{display:block;font-weight:700;margin-bottom:6px;color:#0b2447}
    input,select,button{font-size:14px}
    input,select{width:100%;padding:10px;border-radius:10px;border:1px solid #dbeafe;background:#fff}
    button{background:linear-gradient(90deg,#2563eb,#06b6d4);color:#fff;border:0;padding:8px 14px;border-radius:999px;cursor:pointer;font-weight:700}
    button.secondary{background:#64748b}
    .small{padding:6px 10px;font-size:13px}
    .muted{color:var(--muted);font-size:13px}
    .result{margin-top:12px;padding:12px;border-radius:10px;background:#eef6ff;border:1px solid #dbeafe}
    .error{background:#fff1f2;border:1px solid #fed7d7;color:#b91c1c}
    table{width:100%;border-collapse:collapse;margin-top:8px}
    th,td{padding:8px;border:1px solid #eef6ff;text-align:left;font-size:13px}
    .pill{display:inline-block;background:#eef2ff;color:#1d4ed8;padding:5px 10px;border-radius:999px;font-weight:700;margin-right:6px}
    .hint{font-size:12px;color:#475569;margin-top:8px}
    footer{margin-top:14px;font-size:12px;color:var(--muted);text-align:right}
  </style>
</head>
<body>
  <div class="container" role="main">
    <h1>เครื่องมือสุขภาพ — คำนวณ BMI และอัตราการเต้นของหัวใจ</h1>
    <p class="lead">เลือกช่วงอายุเพื่อดูค่าประมาณ หรือกรอกข้อมูลจริง (อายุ น้ำหนัก ส่วนสูง Resting HR) เพื่อคำนวณ BMI, Max HR (สองสูตร), Target zones และ Karvonen</p>

    <div class="grid">
      <!-- LEFT: Inputs & calculators -->
      <div>
        <div class="card" aria-labelledby="age-group-title">
          <h2 id="age-group-title">1. เลือกช่วงอายุ</h2>
          <label for="ageGroup">ช่วงอายุ</label>
          <select id="ageGroup" aria-describedby="ageHint">
            <option value="child">7–12 ปี (เด็ก)</option>
            <option value="teen">13–18 ปี (วัยรุ่น)</option>
            <option value="adult">19–59 ปี (ผู้ใหญ่)</option>
            <option value="senior">60 ปีขึ้นไป (ผู้สูงอายุ)</option>
          </select>
          <div id="ageHint" class="muted hint">ระบบจะตั้งค่าอายุตัวอย่างเมื่ิอเลือกช่วงอายุ (คุณสามารถแก้ไขอายุจริงด้านล่าง)</div>
        </div>

        <div class="card" style="margin-top:12px;">
          <h2>2. ข้อมูลผู้ใช้ (กรอกเพื่อคำนวณ)</h2>
          <label for="inputAge">อายุ (ปี)</label>
          <input id="inputAge" type="number" min="5" step="1" value="39">

          <label for="inputWeight" style="margin-top:8px;">น้ำหนัก (กก.)</label>
          <input id="inputWeight" type="number" min="10" step="0.1" value="60">

          <label for="inputHeight" style="margin-top:8px;">ส่วนสูง (ซม.)</label>
          <input id="inputHeight" type="number" min="50" step="0.1" value="165">

          <label for="inputRestHR" style="margin-top:8px;">อัตราการเต้นหัวใจขณะพัก (Resting HR) — ถ้าทราบ</label>
          <input id="inputRestHR" type="number" min="20" step="1" placeholder="เช่น 60">

          <div style="display:flex;gap:8px;margin-top:12px;">
            <button id="btnCalc" class="small">คำนวณ BMI & HR</button>
            <button id="btnReset" class="small secondary">รีเซ็ต</button>
          </div>

          <div id="calcResult" class="result" style="display:none;" aria-live="polite"></div>
        </div>

        <div class="card" style="margin-top:12px;">
          <h2>3. สูตรที่ใช้ (สรุป)</h2>
          <ul class="muted" style="margin:6px 0 0 18px;">
            <li><strong>BMI</strong> = น้ำหนัก (kg) ÷ (ส่วนสูง (m))²</li>
            <li><strong>Max HR (มาตรฐาน)</strong> = 220 − อายุ</li>
            <li><strong>Max HR (Tanaka)</strong> = 208 − 0.7 × อายุ</li>
            <li><strong>Target zones</strong> — เบา 50–60%, ปานกลาง 60–70%, หนัก 70–85%</li>
            <li><strong>Karvonen</strong> — หากมี Resting HR: Target = ((MaxHR − RestHR) × intensity) + RestHR</li>
          </ul>
          <div class="hint">หมายเหตุ: เป็นค่าประมาณเพื่อการให้ความรู้ หากใช้ทางการแพทย์หรือฝึกหนัก ควรปรึกษาผู้เชี่ยวชาญ</div>
        </div>
      </div>

      <!-- RIGHT: Age-summary & reference -->
      <aside>
        <div class="card">
          <h2>สรุปตามช่วงอายุ (ค่าประมาณ)</h2>
          <div id="ageSummary" class="muted">เลือกช่วงอายุด้านซ้ายเพื่อดูสรุป (ระบบจะใช้อายุตัวอย่างที่เหมาะสม)</div>
        </div>

        <div class="card" style="margin-top:12px;">
          <h2>ตารางอ้างอิงโซน HR</h2>
          <table>
            <thead><tr><th>โซน</th><th>ความเข้มข้น</th><th>คำอธิบาย</th></tr></thead>
            <tbody>
              <tr><td>โซนเบา</td><td>50–60% ของ Max HR</td><td>ออกกำลังกายเบา เช่น เดินเร็ว</td></tr>
              <tr><td>โซนปานกลาง</td><td>60–70% ของ Max HR</td><td>ฝึกความอึด เหมาะสำหรับ cardio ปานกลาง</td></tr>
              <tr><td>โซนหนัก</td><td>70–85% ของ Max HR</td><td>ฝึกเพื่อเพิ่มสมรรถภาพ ฟิตเนสช่วงเข้มข้น</td></tr>
            </tbody>
          </table>
        </div>

        <div class="card" style="margin-top:12px;">
          <h2>คำแนะนำสั้นตามช่วงอายุ</h2>
          <div id="tipsBox" class="muted">เลือกช่วงอายุหรือคำนวณเพื่อดูคำแนะนำ</div>
        </div>
      </aside>
    </div>

    <footer>© เครื่องมือเพื่อการเรียนรู้ — ค่าเป็นการประมาณเท่านั้น</footer>
  </div>

<script>
  // ตัวอย่างอายุกลางของแต่ละช่วง
  const ageGroupMid = {
    child: 10,
    teen: 16,
    adult: 39,
    senior: 70
  };

  // DOM refs
  const ageGroup = document.getElementById('ageGroup');
  const inputAge = document.getElementById('inputAge');
  const inputWeight = document.getElementById('inputWeight');
  const inputHeight = document.getElementById('inputHeight');
  const inputRestHR = document.getElementById('inputRestHR');
  const btnCalc = document.getElementById('btnCalc');
  const btnReset = document.getElementById('btnReset');
  const calcResult = document.getElementById('calcResult');
  const ageSummary = document.getElementById('ageSummary');
  const tipsBox = document.getElementById('tipsBox');

  // helper
  const round1 = n => Math.round(n*10)/10;

  function showAgeSummaryFor(key){
    const sampleAge = ageGroupMid[key];
    const maxStd = 220 - sampleAge;
    const maxTanaka = Math.round(208 - 0.7 * sampleAge);
    const zones = [
      {name:'โซนเบา (50–60%)', low: Math.round(0.5*maxStd), high: Math.round(0.6*maxStd)},
      {name:'โซนปานกลาง (60–70%)', low: Math.round(0.6*maxStd), high: Math.round(0.7*maxStd)},
      {name:'โซนหนัก (70–85%)', low: Math.round(0.7*maxStd), high: Math.round(0.85*maxStd)}
    ];
    let html = `<strong>ตัวอย่างอายุ:</strong> ${sampleAge} ปี<br>`;
    html += `<strong>Max HR (220−age):</strong> ${maxStd} bpm<br>`;
    html += `<strong>Max HR (Tanaka):</strong> ${maxTanaka} bpm<br><br>`;
    html += `<strong>Target zones (อ้างอิงสูตรมาตรฐาน):</strong><br>`;
    zones.forEach(z => html += `${z.name}: ${z.low} – ${z.high} bpm<br>`);
    ageSummary.innerHTML = html;

    const tips = {
      child: 'เด็ก: ส่งเสริมการเล่นที่หลากหลายและกิจกรรมที่เน้นทักษะพื้นฐาน หลีกเลี่ยงการฝึกหนักเกินไป',
      teen: 'วัยรุ่น: แนะนำกิจกรรมแอโรบิกอย่างน้อย 60 นาทีต่อวัน ร่วมกับการฝึกความแข็งแรง 2–3 วัน/สัปดาห์',
      adult: 'ผู้ใหญ่: ตั้งเป้าแอโรบิกปานกลาง 150 นาที/สัปดาห์ หรือหนัก 75 นาที/สัปดาห์ รวมการฝึกความแข็งแรง 2 วัน/สัปดาห์',
      senior: 'ผู้สูงอายุ: เน้นกิจกรรมเบา-ปานกลาง ฝึกการทรงตัวและความแข็งแรงตามความสามารถ ปรึกษาแพทย์หากมีโรคประจำตัว'
    };
    tipsBox.textContent = tips[key];
  }

  // init
  (function init(){
    const g = ageGroup.value;
    inputAge.value = ageGroupMid[g];
    showAgeSummaryFor(g);
  })();

  // when age group changes set sample age and update summary
  ageGroup.addEventListener('change', ()=>{
    const g = ageGroup.value;
    inputAge.value = ageGroupMid[g];
    showAgeSummaryFor(g);
    calcResult.style.display = 'none';
  });

  // main calculation
  btnCalc.addEventListener('click', ()=>{
    calcResult.style.display = 'block';
    const age = parseFloat(inputAge.value);
    const weight = parseFloat(inputWeight.value);
    const heightCm = parseFloat(inputHeight.value);
    const restHR = parseFloat(inputRestHR.value);

    // basic validation
    if(isNaN(age) || age <= 0){
      calcResult.className = 'result error';
      calcResult.innerHTML = 'กรุณากรอกอายุที่ถูกต้อง';
      return;
    }
    if(isNaN(weight) || weight <= 0 || isNaN(heightCm) || heightCm <= 0){
      calcResult.className = 'result error';
      calcResult.innerHTML = 'กรุณากรอกน้ำหนักและส่วนสูงให้ถูกต้อง';
      return;
    }

    // BMI
    const heightM = heightCm / 100;
    const bmi = weight / (heightM * heightM);
    let bmiCat = '';
    if(bmi < 18.5) bmiCat = 'น้ำหนักน้อย (Underweight)';
    else if(bmi < 25) bmiCat = 'ปกติ (Normal weight)';
    else if(bmi < 30) bmiCat = 'น้ำหนักเกิน (Overweight)';
    else bmiCat = 'อ้วน (Obesity)';

    // Max HR
    const maxStd = Math.round(220 - age);
    const maxTanaka = Math.round(208 - 0.7 * age);

    // Target zones (standard)
    const zonesPerc = [
      {name:'โซนเบา', lowP:0.50, highP:0.60},
      {name:'โซนปานกลาง', lowP:0.60, highP:0.70},
      {name:'โซนหนัก', lowP:0.70, highP:0.85}
    ];
    const zonesStd = zonesPerc.map(z => ({
      name: z.name,
      low: Math.round(z.lowP * maxStd),
      high: Math.round(z.highP * maxStd),
      lowP: z.lowP, highP: z.highP
    }));
    const zonesTanaka = zonesPerc.map(z => ({
      name: z.name,
      low: Math.round(z.lowP * maxTanaka),
      high: Math.round(z.highP * maxTanaka)
    }));

    // Karvonen if resting HR provided
    let karvonenHtml = '';
    if(!isNaN(restHR) && restHR > 20){
      karvonenHtml += `<strong>Karvonen (Resting HR = ${restHR} bpm)</strong><br>`;
      zonesPerc.forEach(z=>{
        const low = Math.round(((maxStd - restHR) * z.lowP) + restHR);
        const high = Math.round(((maxStd - restHR) * z.highP) + restHR);
        karvonenHtml += `${z.name} (${Math.round(z.lowP*100)}–${Math.round(z.highP*100)}%): ${low} – ${high} bpm<br>`;
      });
    } else {
      karvonenHtml = `<div class="muted">หากทราบ Resting HR ให้กรอกเพื่อคำนวณ Karvonen (จะแม่นขึ้น)</div>`;
    }

    // build result HTML
    let html = `<strong>BMI:</strong> ${round1(bmi)} kg/m² — <em>${bmiCat}</em><br><br>`;
    html += `<strong>Max HR (มาตรฐาน 220 − อายุ):</strong> ${maxStd} bpm<br>`;
    html += `<strong>Max HR (Tanaka):</strong> ${maxTanaka} bpm<br><br>`;

    html += `<strong>Target zones (อ้างอิง Max HR แบบมาตรฐาน)</strong><br>`;
    zonesStd.forEach(z => {
      html += `${z.name} (${Math.round(z.lowP*100)}–${Math.round(z.highP*100)}%): ${z.low} – ${z.high} bpm<br>`;
    });

    html += `<br><strong>Target zones (อ้างอิง Tanaka)</strong><br>`;
    zonesTanaka.forEach(z => {
      html += `${z.name}: ${z.low} – ${z.high} bpm<br>`;
    });

    html += `<br>${karvonenHtml}`;

    // quick tips by age
    const groupKey = (age>=60)?'senior':(age>=19)?'adult':(age>=13)?'teen':'child';
    const tips = {
      child: 'เด็ก: เน้นกิจกรรมหลากหลาย เล่นกลางแจ้ง และหลีกเลี่ยงการฝึกหนักเกินไป',
      teen: 'วัยรุ่น: ควรมีแอโรบิกวันละ ~60 นาที ร่วมกับฝึกความแข็งแรงเป็นช่วงๆ',
      adult: 'ผู้ใหญ่: ตั้งเป้าแอโรบิกปานกลาง 150 นาที/สัปดาห์ หรือหนัก 75 นาที/สัปดาห์ และฝึกความแข็งแรง 2 ครั้ง/สัปดาห์',
      senior: 'ผู้สูงอายุ: เน้นกิจกรรมเบา-ปานกลาง ฝึกการทรงตัวและความแข็งแรง ปรึกษาแพทย์หากมีโรคเรื้อรัง'
    };
    html += `<hr style="border:none;border-top:1px solid #e6f0ff;margin:10px 0">`;
    html += `<div class="muted"><strong>คำแนะนำ:</strong> ${tips[groupKey]}</div>`;

    calcResult.className = 'result';
    calcResult.innerHTML = html;
  });

  // reset handler
  btnReset.addEventListener('click', ()=>{
    ageGroup.value = 'adult';
    inputAge.value = ageGroupMid['adult'];
    inputWeight.value = 60;
    inputHeight.value = 165;
    inputRestHR.value = '';
    calcResult.style.display = 'none';
    showAgeSummaryFor('adult');
  });

  // when selecting age group, fill sample age & update summary
  ageGroup.addEventListener('change', ()=>{
    const g = ageGroup.value;
    inputAge.value = ageGroupMid[g];
    showAgeSummaryFor(g);
    calcResult.style.display = 'none';
  });

  // show summary function (declared earlier usage)
  function showAgeSummaryFor(key){
    const sampleAge = ageGroupMid[key];
    const maxStd = 220 - sampleAge;
    const maxTanaka = Math.round(208 - 0.7 * sampleAge);
    const zones = [
      { name: 'โซนเบา (50–60%)', low: Math.round(0.5*maxStd), high: Math.round(0.6*maxStd) },
      { name: 'โซนปานกลาง (60–70%)', low: Math.round(0.6*maxStd), high: Math.round(0.7*maxStd) },
      { name: 'โซนหนัก (70–85%)', low: Math.round(0.7*maxStd), high: Math.round(0.85*maxStd) }
    ];
    let html = `<strong>ตัวอย่างอายุ:</strong> ${sampleAge} ปี<br>`;
    html += `<strong>Max HR (220−อายุ):</strong> ${maxStd} bpm<br>`;
    html += `<strong>Max HR (Tanaka):</strong> ${maxTanaka} bpm<br><br>`;
    html += `<strong>Target zones (อ้างอิงสูตรมาตรฐาน)</strong><br>`;
    zones.forEach(z=> html += `${z.name}: ${z.low} – ${z.high} bpm<br>`);
    ageSummary.innerHTML = html;

    const tipsByGroup = {
      child: 'เด็ก: เน้นการเล่นและการเคลื่อนไหวที่หลากหลาย',
      teen: 'วัยรุ่น: แนะนำแอโรบิกทุกวัน ผสมการฝึกแรง 2–3 วัน/สัปดาห์',
      adult: 'ผู้ใหญ่: รักษาการเคลื่อนไหวสม่ำเสมอและฝึกความแข็งแรงเป็นประจำ',
      senior: 'ผู้สูงอายุ: เน้นกิจกรรมที่ปลอดภัย ปรึกษาแพทย์หากมีเงื่อนไข'
    };
    tipsBox.textContent = tipsByGroup[key];
  }

  // initialize
  (function boot(){
    const g = ageGroup.value;
    inputAge.value = ageGroupMid[g];
    showAgeSummaryFor(g);
  })();
</script>
</body>
</html>
