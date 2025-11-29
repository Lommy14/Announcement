<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="utf-8" />
  <title>เครื่องมือสุขภาพ — BMI & อัตราการเต้นของหัวใจ (ไทย / English)</title>
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;600;700&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg:#f5f9ff; --card:#ffffff; --accent:#0f4fd8; --muted:#475569;
      --good:#10b981; --warn:#f59e0b; --bad:#ef4444;
    }
    *{box-sizing:border-box;font-family:"Sarabun",system-ui,-apple-system,"Segoe UI",sans-serif}
    body{margin:0;background:var(--bg);display:flex;justify-content:center;padding:28px 12px}
    .container{max-width:900px;width:100%;background:var(--card);border-radius:12px;padding:20px;border:1px solid rgba(15,23,42,0.04);box-shadow:0 10px 30px rgba(15,23,42,0.05)}
    h1{margin:0;font-size:20px;color:var(--accent)}
    p.lead{margin:8px 0 16px;color:var(--muted);font-size:14px}
    .grid{display:grid;grid-template-columns:1fr 320px;gap:16px}
    @media(max-width:900px){.grid{grid-template-columns:1fr}}
    .card{background:#fcfeff;border-radius:10px;padding:14px;border:1px solid #e6f0ff}
    label{display:block;font-weight:700;margin-bottom:6px;color:#0b2447}
    input,select,button{font-size:14px}
    input{width:100%;padding:10px;border-radius:8px;border:1px solid #dbeafe;background:#fff}
    button{background:linear-gradient(90deg,#2563eb,#06b6d4);color:#fff;border:0;padding:8px 12px;border-radius:999px;cursor:pointer;font-weight:700}
    button.secondary{background:#64748b}
    .small{padding:6px 10px;font-size:13px}
    .muted{color:var(--muted);font-size:13px}
    .result{margin-top:12px;padding:12px;border-radius:8px;background:#eef6ff;border:1px solid #dbeafe}
    .error{background:#fff1f2;border:1px solid #fed7d7;color:#b91c1c}
    .step{background:#fff;border:1px dashed #dbeafe;padding:10px;border-radius:8px;margin-top:8px;font-size:13px;color:#263348}
    .bmi-bar{height:12px;border-radius:999px;background:#e6f6f0;margin-top:8px;overflow:hidden}
    .bmi-fill{height:100%;border-radius:999px}
    table{width:100%;border-collapse:collapse;margin-top:8px}
    th,td{padding:8px;border:1px solid #eef6ff;text-align:left;font-size:13px}
    footer{margin-top:14px;font-size:12px;color:var(--muted);text-align:right}
    .label-en{display:block;font-size:12px;color:#6b7280;margin-top:4px}
  </style>
</head>
<body>
  <div class="container" role="main">
    <h1>เครื่องมือสุขภาพ — BMI & อัตราการเต้นของหัวใจ (ไทย / English)</h1>
    <p class="lead">กรอกอายุ น้ำหนัก ส่วนสูง (และถ้ามี Resting HR) แล้วกด “คำนวณ” — ระบบจะแสดงผลทั้งภาษาไทยและภาษาอังกฤษ พร้อมวิธีคิด BMI ทีละขั้นตอน</p>

    <div class="grid">
      <!-- LEFT: Inputs & calculators -->
      <div>
        <div class="card" aria-labelledby="input-title">
          <h2 id="input-title">ข้อมูลผู้ใช้ / User input</h2>

          <label for="inputAge">อายุ (ปี) — Age (years)</label>
          <input id="inputAge" type="number" min="5" step="1" value="30">

          <label for="inputWeight" style="margin-top:8px;">น้ำหนัก (กก.) — Weight (kg)</label>
          <input id="inputWeight" type="number" min="10" step="0.1" value="60">

          <label for="inputHeight" style="margin-top:8px;">ส่วนสูง (ซม.) — Height (cm)</label>
          <input id="inputHeight" type="number" min="50" step="0.1" value="165">

          <label for="inputRestHR" style="margin-top:8px;">อัตราการเต้นหัวใจขณะพัก (Resting HR) — ถ้าทราบ / Resting HR (bpm)</label>
          <input id="inputRestHR" type="number" min="20" step="1" placeholder="เช่น 60">

          <div style="display:flex;gap:8px;margin-top:12px;">
            <button id="btnCalc" class="small">คำนวณ / Calculate</button>
            <button id="btnReset" class="small secondary">รีเซ็ต / Reset</button>
          </div>

          <div id="calcResult" class="result" style="display:none;" aria-live="polite"></div>

          <div id="bmiSteps" class="step" style="display:none;" aria-live="polite">
            <strong>วิธีคิด BMI ทีละขั้นตอน / Step-by-step BMI</strong>
            <div id="bmiStepsContent" style="margin-top:6px;"></div>
          </div>
        </div>

        <div class="card" style="margin-top:12px;">
          <h2>สูตรที่ใช้ (ไทย / English)</h2>
          <ul class="muted" style="margin:6px 0 0 18px;">
            <li><strong>BMI</strong> = น้ำหนัก (kg) ÷ (ส่วนสูง (m))² — BMI = weight (kg) ÷ (height (m))²</li>
            <li><strong>Max HR (มาตรฐาน)</strong> = 220 − อายุ — Max HR (standard) = 220 − age</li>
            <li><strong>Max HR (Tanaka)</strong> = 208 − 0.7 × อายุ — Max HR (Tanaka) = 208 − 0.7 × age</li>
            <li><strong>Target zones</strong> — เบา 50–60%, ปานกลาง 60–70%, หนัก 70–85% — Target zones (50–60%, 60–70%, 70–85%)</li>
            <li><strong>Karvonen</strong> — ถ้ามี Resting HR: Target = ((MaxHR − RestHR) × intensity) + RestHR</li>
          </ul>
          <div class="muted" style="margin-top:8px;">หมายเหตุ: ผลเป็นการประมาณเพื่อการศึกษา / Note: results are approximate for learning purposes</div>
        </div>
      </div>

      <!-- RIGHT: Reference & zones -->
      <aside>
        <div class="card">
          <h2>Target zones & ตัวอย่าง / Example</h2>
          <table>
            <thead><tr><th>โซน / Zone</th><th>ความเข้มข้น / Intensity</th><th>คำอธิบาย / For</th></tr></thead>
            <tbody>
              <tr><td>โซนเบา / Light</td><td>50–60% ของ Max HR</td><td>เดินเร็ว, อุ่นเครื่อง / walking, warm-up</td></tr>
              <tr><td>โซนปานกลาง / Moderate</td><td>60–70% ของ Max HR</td><td>วิ่งช้า/ปั่นจักรยานสบาย / moderate cardio</td></tr>
              <tr><td>โซนหนัก / Vigorous</td><td>70–85% ของ Max HR</td><td>HIIT, interval / high intensity</td></tr>
            </tbody>
          </table>
        </div>

        <div class="card" style="margin-top:12px;">
          <h2>คำแนะนำสั้น ๆ / Quick tips</h2>
          <div class="muted">
            - เด็กและวัยรุ่น: เน้นเล่นและเคลื่อนไหวเป็นเวลา 60 นาที/วัน<br>
            - ผู้ใหญ่: พยายามออกกำลังกายแบบปานกลาง 150 นาที/สัปดาห์ หรือหนัก 75 นาที/สัปดาห์<br>
            - ถ้าจะฝึกด้วย HR ให้ตรวจ Resting HR ก่อนและเริ่มจากโซนเบา → ปานกลาง
          </div>
        </div>
      </aside>
    </div>

    <footer>© เครื่องมือเพื่อการเรียนรู้ — ค่าเป็นการประมาณ / For learning only</footer>
  </div>

<script>
  // DOM refs
  const inputAge = document.getElementById('inputAge');
  const inputWeight = document.getElementById('inputWeight');
  const inputHeight = document.getElementById('inputHeight');
  const inputRestHR = document.getElementById('inputRestHR');
  const btnCalc = document.getElementById('btnCalc');
  const btnReset = document.getElementById('btnReset');
  const calcResult = document.getElementById('calcResult');
  const bmiSteps = document.getElementById('bmiSteps');
  const bmiStepsContent = document.getElementById('bmiStepsContent');

  const round1 = n => Math.round(n*10)/10;

  btnCalc.addEventListener('click', () => {
    calcResult.style.display = 'block';
    bmiSteps.style.display = 'none';

    const age = parseFloat(inputAge.value);
    const weight = parseFloat(inputWeight.value);
    const heightCm = parseFloat(inputHeight.value);
    const rest = parseFloat(inputRestHR.value);

    // validation
    if (isNaN(age) || age <= 0) {
      calcResult.className = 'result error';
      calcResult.innerHTML = 'กรุณากรอกอายุที่ถูกต้อง / Please enter a valid age';
      return;
    }
    if (isNaN(weight) || weight <= 0 || isNaN(heightCm) || heightCm <= 0) {
      calcResult.className = 'result error';
      calcResult.innerHTML = 'กรุณากรอกน้ำหนักและส่วนสูงให้ถูกต้อง / Please enter valid weight and height';
      return;
    }

    // BMI
    const heightM = heightCm / 100;
    const bmi = weight / (heightM * heightM);
    let bmiCatThai = '', bmiCatEn = '';
    if (bmi < 18.5) { bmiCatThai = 'น้ำหนักน้อย'; bmiCatEn = 'Underweight'; }
    else if (bmi < 25) { bmiCatThai = 'ปกติ (สมดุล)'; bmiCatEn = 'Normal weight'; }
    else if (bmi < 30) { bmiCatThai = 'น้ำหนักเกิน'; bmiCatEn = 'Overweight'; }
    else { bmiCatThai = 'อ้วน'; bmiCatEn = 'Obesity'; }

    // BMI steps (show)
    bmiSteps.style.display = 'block';
    bmiStepsContent.innerHTML =
      `น้ำหนัก = ${weight} กก. / weight = ${weight} kg<br>` +
      `ส่วนสูง = ${heightCm} ซม. = ${round1(heightM)} เมตร / height = ${round1(heightM)} m<br>` +
      `BMI = น้ำหนัก ÷ (ส่วนสูง(ม.) × ส่วนสูง(ม.)) / BMI = weight ÷ (height × height)<br>` +
      `BMI = ${weight} ÷ (${round1(heightM)} × ${round1(heightM)}) = ${round1(bmi)} kg/m²<br>` +
      `<strong>สรุป: BMI = ${round1(bmi)} → ${bmiCatThai} / ${bmiCatEn}</strong>`;

    // Max HR formulas
    const maxStd = Math.round(220 - age);
    const maxTanaka = Math.round(208 - 0.7 * age);

    // zones (percent)
    const zones = [
      {nameThai:'โซนเบา', nameEn:'Light', lowP:0.50, highP:0.60},
      {nameThai:'โซนปานกลาง', nameEn:'Moderate', lowP:0.60, highP:0.70},
      {nameThai:'โซนหนัก', nameEn:'Vigorous', lowP:0.70, highP:0.85}
    ];
    const zonesStd = zones.map(z=>({
      nameThai: z.nameThai, nameEn: z.nameEn,
      low: Math.round(z.lowP * maxStd), high: Math.round(z.highP * maxStd),
      lowP: z.lowP, highP: z.highP
    }));
    const zonesTanaka = zones.map(z=>({
      nameThai: z.nameThai, nameEn: z.nameEn,
      low: Math.round(z.lowP * maxTanaka), high: Math.round(z.highP * maxTanaka)
    }));

    // Karvonen if Resting HR provided
    let karvonenHtml = '';
    if (!isNaN(rest) && rest > 20) {
      karvonenHtml += `<strong>Karvonen (ใช้ Resting HR = ${rest} bpm)</strong><br>`;
      zones.forEach(z=>{
        const low = Math.round(((maxStd - rest) * z.lowP) + rest);
        const high = Math.round(((maxStd - rest) * z.highP) + rest);
        karvonenHtml += `${z.nameThai} / ${z.nameEn} (${Math.round(z.lowP*100)}–${Math.round(z.highP*100)}%): ${low} – ${high} bpm<br>`;
      });
    } else {
      karvonenHtml = `<div class="muted">หากทราบ Resting HR ให้กรอกเพื่อคำนวณ Karvonen (แม่นขึ้น) / If you know Resting HR, enter it to calculate Karvonen (more accurate)</div>`;
    }

    // build output bilingual
    let html = `<strong>ผลการคำนวณ (ไทย) — Results (English)</strong><br><br>`;

    html += `<strong>BMI:</strong> ${round1(bmi)} — ${bmiCatThai} / ${bmiCatEn}<br><br>`;

    html += `<strong>Max HR (มาตรฐาน):</strong> ${maxStd} bpm — <em>Max HR (standard) = 220 − age</em><br>`;
    html += `<strong>Max HR (Tanaka):</strong> ${maxTanaka} bpm — <em>Max HR (Tanaka) = 208 − 0.7 × age</em><br><br>`;

    html += `<strong>Target zones (อ้างอิง Max HR แบบมาตรฐาน):</strong><br>`;
    zonesStd.forEach(z=> {
      html += `${z.nameThai} / ${z.nameEn} (${Math.round(z.lowP*100)}–${Math.round(z.highP*100)}%): ${z.low} – ${z.high} bpm<br>`;
    });

    html += `<br><strong>Target zones (อ้างอิง Tanaka):</strong><br>`;
    zonesTanaka.forEach(z=> {
      html += `${z.nameThai} / ${z.nameEn}: ${z.low} – ${z.high} bpm<br>`;
    });

    html += `<br>${karvonenHtml}`;

    html += `<hr style="border:none;border-top:1px solid #e6f0ff;margin:8px 0">`;
    html += `<div class="muted">คำแนะนำ / Tips: เริ่มจากโซนเบาแล้วค่อยเพิ่มความเข้มข้น หากมีข้อสงสัยให้ปรึกษาอาจารย์หรือนักสุขภาพ / Start from light zone and progress gradually. Consult teacher or health professional for concerns.</div>`;

    calcResult.className = 'result';
    calcResult.innerHTML = html;
  });

  btnReset.addEventListener('click', () => {
    inputAge.value = 30;
    inputWeight.value = 60;
    inputHeight.value = 165;
    inputRestHR.value = '';
    calcResult.style.display = 'none';
    bmiSteps.style.display = 'none';
  });

  // init defaults
  (function init(){
    inputAge.value = 30;
    inputWeight.value = 60;
    inputHeight.value = 165;
  })();
</script>
</body>
</html>
