<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<title>เครื่องมือสุขภาพสำหรับนักเรียน — BMI & Heart Rate</title>
<link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;600&display=swap" rel="stylesheet">

<style>
  body {
    font-family: "Sarabun", sans-serif;
    background: #f3f7ff;
    padding: 20px;
    display: flex;
    justify-content: center;
  }
  .box {
    width: 100%;
    max-width: 700px;
    background: white;
    padding: 24px;
    border-radius: 16px;
    box-shadow: 0 4px 20px rgba(0,0,0,0.08);
  }
  h1 {
    text-align: center;
    color: #0e3a8c;
    margin-bottom: 4px;
  }
  p.desc {
    text-align: center;
    font-size: 14px;
    color: #475569;
    margin-top: 0;
  }
  label {
    font-weight: 600;
    margin-top: 12px;
  }
  input {
    width: 100%;
    padding: 10px;
    margin-top: 4px;
    border-radius: 8px;
    border: 1px solid #cdd5e1;
    font-size: 15px;
  }
  button {
    width: 100%;
    margin-top: 16px;
    background: #2563eb;
    color: white;
    padding: 12px;
    border: none;
    border-radius: 10px;
    font-size: 16px;
    cursor: pointer;
  }
  .result {
    margin-top: 20px;
    padding: 16px;
    border-radius: 12px;
    background: #f1f5ff;
    color: #1e3a8a;
    font-size: 15px;
    border-left: 5px solid #2563eb;
  }
  .section-title {
    font-size: 16px;
    font-weight: 700;
    margin-top: 14px;
    color: #0f172a;
  }
</style>
</head>

<body>

<div class="box">
  <h1>📘 เครื่องมือสุขภาพสำหรับนักเรียน</h1>
  <p class="desc">คำนวณ BMI + อัตราการเต้นของหัวใจแบบเข้าใจง่าย</p>

  <!-- Input -->
  <label>อายุ (ปี)</label>
  <input type="number" id="age" placeholder="เช่น 15">

  <label>น้ำหนัก (กิโลกรัม)</label>
  <input type="number" id="weight" placeholder="เช่น 50">

  <label>ส่วนสูง (เซนติเมตร)</label>
  <input type="number" id="height" placeholder="เช่น 160">

  <label>อัตราการเต้นขณะพัก (Resting HR) *ถ้ามี</label>
  <input type="number" id="rest" placeholder="เช่น 70 (ไม่ใส่ก็ได้)">

  <button onclick="calculate()">คำนวณผล</button>

  <!-- Output -->
  <div id="output" class="result" style="display:none;"></div>
</div>

<script>
function calculate() {
  const age = parseFloat(document.getElementById("age").value);
  const weight = parseFloat(document.getElementById("weight").value);
  const height = parseFloat(document.getElementById("height").value);
  const rest = parseFloat(document.getElementById("rest").value);

  const out = document.getElementById("output");

  if (!age || !weight || !height) {
    out.style.display = "block";
    out.innerHTML = "⚠️ กรุณากรอกข้อมูลให้ครบก่อนคำนวณ";
    return;
  }

  /* --- BMI --- */
  const h = height / 100;
  const bmi = weight / (h * h);

  let bmiText = "";
  if (bmi < 18.5) bmiText = "น้ำหนักน้อย (ผอม)";
  else if (bmi < 25) bmiText = "ปกติ";
  else if (bmi < 30) bmiText = "น้ำหนักเกิน";
  else bmiText = "อ้วน";

  /* --- Max HR --- */
  const maxHR = 220 - age;             // สูตรมาตรฐาน
  const maxTanaka = Math.round(208 - 0.7 * age);

  /* --- HR Zones --- */
  const zone50 = Math.round(maxHR * 0.50);
  const zone60 = Math.round(maxHR * 0.60);
  const zone70 = Math.round(maxHR * 0.70);
  const zone85 = Math.round(maxHR * 0.85);

  /* --- Karvonen --- */
  let karvonen = "";
  if (!isNaN(rest)) {
    const low = Math.round(((maxHR - rest) * 0.60) + rest);
    const high = Math.round(((maxHR - rest) * 0.80) + rest);
    karvonen = `
      <br><b>Karvonen Heart Rate:</b><br>
      ช่วงฝึกที่เหมาะสม = ${low} - ${high} ครั้ง/นาที
    `;
  }

  /* --- Show Result --- */
  out.style.display = "block";
  out.innerHTML = `
    <div class='section-title'>💙 ผลลัพธ์ของคุณ</div>
    <b>BMI:</b> ${bmi.toFixed(1)} (${bmiText})<br><br>

    <div class='section-title'>❤️ อัตราการเต้นหัวใจสูงสุด (Max HR)</div>
    สูตรมาตรฐาน: <b>${maxHR}</b> ครั้ง/นาที<br>
    สูตร Tanaka: <b>${maxTanaka}</b> ครั้ง/นาที<br><br>

    <div class='section-title'>🎯 โซนออกกำลังกาย</div>
    โซนเบา (50–60%): ${zone50} - ${zone60} ครั้ง/นาที<br>
    โซนปานกลาง (60–70%): ${zone60} - ${zone70} ครั้ง/นาที<br>
    โซนหนัก (70–85%): ${zone70} - ${zone85} ครั้ง/นาที<br>

    ${karvonen}
  `;
}
</script>

</body>
</html>
