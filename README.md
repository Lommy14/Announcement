<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="utf-8" />
  <title>เครื่องมือสุขภาพ — BMI & อัตราการเต้นของหัวใจ</title>
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;600;700&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg:#f5f8ff; --card:#ffffff; --accent:#0f4fd8; --muted:#475569;
      --good:#10b981; --warn:#f59e0b; --bad:#ef4444;
    }
    *{box-sizing:border-box;font-family:"Sarabun",system-ui,-apple-system,"Segoe UI",sans-serif}
    body{margin:0;background:var(--bg);display:flex;justify-content:center;padding:28px 12px}
    .container{max-width:980px;width:100%;background:var(--card);border-radius:12px;padding:20px;border:1px solid rgba(15,23,42,0.04);box-shadow:0 10px 30px rgba(15,23,42,0.05)}
    h1{margin:0;font-size:22px;color:var(--accent)}
    p.lead{margin:8px 0 16px;color:var(--muted);font-size:14px}
    .grid{display:grid;grid-template-columns:1fr 340px;gap:16px}
    @media(max-width:900px){.grid{grid-template-columns:1fr}}
    .card{background:#fcfeff;border-radius:10px;padding:14px;border:1px solid #e6f0ff}
    label{display:block;font-weight:700;margin-bottom:6px;color:#0b2447}
    input,button,select{font-size:14px}
    input{width:100%;padding:10px;border-radius:8px;border:1px solid #dbeafe;background:#fff}
    button{background:linear-gradient(90deg,#2563eb,#06b6d4);color:#fff;border:0;padding:10px 14px;border-radius:999px;cursor:pointer;font-weight:700}
    button.secondary{background:#64748b}
    .small{padding:6px 10px;font-size:13px}
    .muted{color:var(--muted);font-size:13px}
    .result{margin-top:12px;padding:14px;border-radius:10px;background:#eef6ff;border:1px solid #dbeafe}
    .error{background:#fff1f2;border:1px solid #fed7d7;color:#b91c1c}
    table{width:100%;border-collapse:collapse;margin-top:8px}
    th,td{padding:8px;border:1px solid #eef6ff;text-align:left;font-size:13px}
    .step{background:#fff;border:1px dashed #dbeafe;padding:12px;border-radius:8px;margin-top:8px;font-size:13px;color:#263348}
    .section-title{font-weight:700;margin-top:10px;color:#0b2447}
    .note{font-size:13px;color:#475569;margin-top:8px}
    footer{margin-top:14px;font-size:12px;color:var(--muted);text-align:right}
  </style>
</head>
<body>
  <div class="container" role="main">
    <h1>เครื่องมือสุขภาพ — คำนวณ BMI และอัตราการเต้นของหัวใจ</h1>
    <p class="lead">กรอกอายุ น้ำหนัก ส่วนสูง และ (ถ้ามี) Resting HR แล้วกด "คำนวณ" — จะแสดงผล คำอธิบายการคำนวณ และตารางความรู้สรุปผล</p>

    <div class="grid">
      <!-- LEFT: Inputs & calculators -->
      <div>
        <div class="card" aria-labelledby="input-title">
          <h2 id="input-title">ข้อมูลผู้ใช้</h2>

          <label for="inputAge">อายุ (ปี)</label>
          <input id="inputAge" type="number" min="5" step="1" value="30">

          <label for="inputWeight" style="margin-top:8px;">น้ำหนัก (กิโลกรัม)</label>
          <input id="inputWeight" type="number" min="10" step="0.1" value="60">

          <label for="inputHeight" style="margin-top:8px;">ส่วนสูง (เซนติเมตร)</label>
          <input id="inputHeight" type="number" min="50" step="0.1" value="165">

          <label for="inputRestHR" style="margin-top:8px;">อัตราการเต้นขณะพัก (Resting HR) — ถ้าทราบ</label>
          <input id="inputRestHR" type="number" min="20" step="1" placeholder="เช่น 60">

          <div style="display:flex;gap:8px;margin-top:12px;">
            <button id="btnCalc" class="small">คำนวณ</button>
            <button id="btnReset" class="small secondary">รีเซ็ต</button>
          </div>

          <div id="calcResult" class="result" style="display:none;" aria-live="polite"></div>

          <div id="bmiSteps" class="step" style="display:none;" aria-live="polite">
            <strong>วิธีคิด BMI ทีละขั้นตอน</strong>
            <div id="bmiStepsContent" style="margin-top:6px;"></div>
          </div>
        </div>

        <div class="card" style="margin-top:12px;">
          <h2 class="section-title">คำอธิบายสูตร (สรุป)</h2>
          <div class="note">
            <strong>BMI</strong> — เป็นตัวชี้วัดง่าย ๆ ว่าน้ำหนักเทียบกับส่วนสูงเป็นอย่างไร  
            <strong>สูตร:</strong> BMI = น้ำหนัก (kg) ÷ (ส่วนสูง (m))²  
            <br><br>
            <strong>Max HR (สูตรมาตรฐาน)</strong> = 220 − อายุ  
            <br>
            <strong>Max HR (Tanaka)</strong> = 208 − 0.7 × อายุ  
            <br><br>
            <strong>Target zones</strong> — แบ่งตามเปอร์เซ็นต์ของ Max HR:  
            โซนเบา 50–60% | โซนปานกลาง 60–70% | โซนหนัก 70–85%  
            <br><br>
            <strong>Karvonen (ถ้ารู้ Resting HR)</strong> = ((MaxHR − RestHR) × intensity) + RestHR  
            <br><br>
            หมายเหตุ: ค่าทั้งหมดเป็นการประมาณเพื่อการศึกษา หากต้องการใช้จริงหรือฝึกหนัก ควรปรึกษาผู้เชี่ยวชาญ
          </div>
        </div>
      </div>

      <!-- RIGHT: Reference & table of knowledge -->
      <aside>
        <div class="card">
          <h2 class="section-title">ตารางความรู้ (สรุปผล)</h2>
          <table>
            <thead>
              <tr><th>หัวข้อ</th><th>ความหมาย / วิธีดู</th></tr>
            </thead>
            <tbody>
              <tr>
                <td>BMI</td>
                <td>
                  ค่า BMI แสดงสัดส่วนน้ำหนักต่อความสูง<br>
                  <strong>ช่วง</strong>: &lt;18.5 = ผอม, 18.5–24.9 = ปกติ, 25–29.9 = น้ำหนักเกิน, ≥30 = อ้วน
                </td>
              </tr>
              <tr>
                <td>Max HR</td>
                <td>
                  อัตราการเต้นหัวใจสูงสุดที่คาดว่าจะทำได้ (ประมาณ) <br>
                  สูตรมาตรฐาน: 220 − อายุ<br>
                  สูตร Tanaka: 208 − 0.7 × อายุ
                </td>
              </tr>
              <tr>
                <td>Target zones</td>
                <td>
                  โซนการออกกำลังกายเพื่อกำหนดความเข้มข้น<br>
                  เบา 50–60% = เหมาะสำหรับอุ่นเครื่อง / เดินเร็ว<br>
                  ปานกลาง 60–70% = เพิ่มความฟิต (วิ่งสบายๆ) <br>
                  หนัก 70–85% = ฝึกเข้มข้น / HIIT
                </td>
              </tr>
              <tr>
                <td>Karvonen formula</td>
                <td>
                  วิธีคำนวณ heart rate เป้าหมายเมื่อนำ Resting HR มาพิจารณา (แม่นขึ้น) <br>
                  Target = ((MaxHR − RestHR) × intensity) + RestHR
                </td>
              </tr>
              <tr>
                <td>คำแนะนำทั่วไป</td>
                <td>
                  เริ่มจากโซนเบาก่อน เพิ่มความเข้มข้นทีละน้อย หากมีโรคประจำตัวหรืออาการผิดปกติ ปรึกษาแพทย์
                </td>
              </tr>
            </tbody>
          </table>
        </div>

        <div class="card" style="margin-top:12px;">
          <h2 class="section-title">ตารางค่าอ้างอิง (ตัวอย่าง)</h2>
          <table>
            <thead><tr><th>ประเภท</th><th>ค่าที่ใช้</th></tr></thead>
            <tbody>
              <tr><td>ตัวอย่าง Max HR (อายุ 30)</td><td>220 − 30 = 190 bpm (มาตรฐาน)<br>Tanaka = 208 − 0.7×30 ≈ 187 bpm</td></tr>
              <tr><td>ตัวอย่าง Target Zone (30 ปี)</td><td>โซนปานกลาง 60–70% ของ 190 ≈ 114–133 bpm</td></tr>
              <tr><td>ตัวอย่าง Karvonen (Rest = 60)</td><td>Target 50% = ((190−60)×0.5)+60 = 125 bpm</td></tr>
            </tbody>
          </table>
        </div>
      </aside>
    </div>

    <footer>© ข้อมูลเพื่อการเรียนรู้ — ค่าทั้งหมดเป็นการประมาณ</footer>
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
      calcResult.innerHTML = 'กรุณากรอกอายุที่ถูกต้อง';
      return;
    }
    if (isNaN(weight) || weight <= 0 || isNaN(heightCm) || heightCm <= 0) {
      calcResult.className = 'result error';
      calcResult.innerHTML = 'กรุณากรอกน้ำหนักและส่วนสูงให้ถูกต้อง';
      return;
    }

    // BMI
    const heightM = heightCm / 100;
    const bmi = weight / (heightM * heightM);
    let bmiCat = '';
    if (bmi < 18.5) bmiCat = 'ผอม (Underweight)';
    else if (bmi < 25) bmiCat = 'ปกติ (Normal weight)';
    else if (bmi < 30) bmiCat = 'น้ำหนักเกิน (Overweight)';
    else bmiCat = 'อ้วน (Obesity)';

    // Show BMI steps
    bmiSteps.style.display = 'block';
    bmiStepsContent.innerHTML =
      `น้ำหนัก = ${weight} กก.<br>` +
      `ส่วนสูง = ${heightCm} ซม. = ${round1(heightM)} เมตร<br>` +
      `BMI = น้ำหนัก ÷ (ส่วนสูง(ม.) × ส่วนสูง(ม.))<br>` +
      `BMI = ${weight} ÷ (${round1(heightM)} × ${round1(heightM)}) = ${round1(bmi)} (กก./ม²)<br>` +
      `<strong>สรุป: BMI = ${round1(bmi)} → ${bmiCat}</strong>`;

    // Max HR
    const maxStd = Math.round(220 - age);
    const maxTanaka = Math.round(208 - 0.7 * age);

    // Zones (standard)
    const zonesPerc = [
      {name:'โซนเบา', lowP:0.50, highP:0.60},
      {name:'โซนปานกลาง', lowP:0.60, highP:0.70},
      {name:'โซนหนัก', lowP:0.70, highP:0.85}
    ];
    const zonesStd = zonesPerc.map(z => ({
      name: z.name,
      low: Math.round(z.lowP * maxStd),
      high: Math.round(z.highP * maxStd),
      lowPct: Math.round(z.lowP*100), highPct: Math.round(z.highP*100)
    }));
    const zonesTanaka = zonesPerc.map(z => ({
      name: z.name,
      low: Math.round(z.lowP * maxTanaka),
      high: Math.round(z.highP * maxTanaka)
    }));

    // Karvonen
    let karvonenHtml = '';
    if (!isNaN(rest) && rest > 20) {
      karvonenHtml += `<strong>Karvonen (ใช้ Resting HR = ${rest} bpm)</strong><br>`;
      zonesPerc.forEach(z => {
        const low = Math.round(((maxStd - rest) * z.lowP) + rest);
        const high = Math.round(((maxStd - rest) * z.highP) + rest);
        karvonenHtml += `${z.name} (${Math.round(z.lowP*100)}–${Math.round(z.highP*100)}%): ${low} – ${high} bpm<br>`;
      });
    } else {
      karvonenHtml = `<div class="muted">ถ้าทราบ Resting HR ให้กรอกเพื่อคำนวณ Karvonen (จะได้ค่าเป้าหมายแม่นขึ้น)</div>`;
    }

    // Build result HTML
    let html = `<strong>ผลลัพธ์การคำนวณ</strong><br><br>`;

    html += `<strong>BMI:</strong> ${round1(bmi)} — <em>${bmiCat}</em><br><br>`;

    html += `<strong>Max HR (สูตรมาตรฐาน):</strong> ${maxStd} bpm<br>`;
    html += `<strong>Max HR (Tanaka):</strong> ${maxTanaka} bpm<br><br>`;

    html += `<strong>Target zones (อ้างอิงสูตรมาตรฐาน):</strong><br>`;
    zonesStd.forEach(z => {
      html += `${z.name} (${z.lowPct}–${z.highPct}%): ${z.low} – ${z.high} bpm<br>`;
    });

    html += `<br><strong>Target zones (อ้างอิง Tanaka):</strong><br>`;
    zonesTanaka.forEach(z => {
      html += `${z.name}: ${z.low} – ${z.high} bpm<br>`;
    });

    html += `<br>${karvonenHtml}`;

    html += `<hr style="border:none;border-top:1px solid #e6f0ff;margin:8px 0">`;
    html += `<div class="muted"><strong>สรุปความรู้สั้น ๆ:</strong> BMI ใช้วัดสัดส่วน น้ำหนักกับส่วนสูง; Max HR เป็นค่าประมาณของหัวใจที่เต้นได้สูงสุด; โซน HR ช่วยกำหนดความเข้มข้นการออกกำลังกาย; Karvonen ช่วยให้ได้ค่าเป้าหมายที่เหมาะกับคนแต่ละคน</div>`;

    calcResult.className = 'result';
    calcResult.innerHTML = html;
  });

  // reset
  btnReset.addEventListener('click', () => {
    inputAge.value = 30;
    inputWeight.value = 60;
    inputHeight.value = 165;
    inputRestHR.value = '';
    calcResult.style.display = 'none';
    bmiSteps.style.display = 'none';
  });

  // init default values
  (function init(){
    inputAge.value = 30;
    inputWeight.value = 60;
    inputHeight.value = 165;
  })();
</script>
</body>
</html>
