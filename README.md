<!doctype html>
<html lang="th">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>ระบบคำนวณภาษี ปี 2569</title>
  <style>
    :root{
      --bg:#eef5ff
;
      --card:#ffffff
;
      --line:#d8e4f5
;
      --text:#16324f
;
      --muted:#5f7288
;
      --primary:#2563eb
;
      --primary2:#0ea5e9
;
      --accent:#f59e0b
;
      --accent2:#f97316
;
      --success:#16a34a
;
      --danger:#ef4444
;
      --shadow: 0 20px 50px rgba(32, 68, 136, .12);
      --radius:22px;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      font-family:system-ui,-apple-system,BlinkMacSystemFont,"Segoe UI",sans-serif;
      background:
        radial-gradient(circle at top left, rgba(14,165,233,.15), transparent 22%),
        radial-gradient(circle at top right, rgba(245,158,11,.13), transparent 18%),
        linear-gradient(180deg, #f8fbff
, var(--bg));
      color:var(--text);
    }
    .wrap{
      max-width:1260px;
      margin:0 auto;
      padding:24px;
    }
    .hero{
      background: linear-gradient(135deg, #1d4ed8
, #0ea5e9
);
      color:#fff;
      border-radius:28px;
      padding:28px;
      box-shadow:var(--shadow);
      position:relative;
      overflow:hidden;
    }
    .hero:after{
      content:"";
      position:absolute;
      right:-60px; top:-60px;
      width:220px; height:220px;
      border-radius:50%;
      background:rgba(255,255,255,.12);
      filter:blur(2px);
    }
    .badge{
      display:inline-flex;
      align-items:center;
      gap:8px;
      background:rgba(255,255,255,.16);
      border:1px solid rgba(255,255,255,.22);
      color:#fff;
      padding:8px 14px;
      border-radius:999px;
      font-size:14px;
      margin-bottom:12px;
      position:relative;
      z-index:1;
    }
    h1{
      margin:0 0 10px;
      font-size:clamp(28px,4vw,42px);
      line-height:1.15;
      position:relative;
      z-index:1;
    }
    .hero p{
      margin:0;
      max-width:860px;
      font-size:15px;
      line-height:1.7;
      opacity:.96;
      position:relative;
      z-index:1;
    }

    .layout{
      display:grid;
      grid-template-columns: 1.4fr .95fr;
      gap:20px;
      margin-top:22px;
      align-items:start;
    }
    .card{
      background:var(--card);
      border:1px solid rgba(216,228,245,.85);
      border-radius:var(--radius);
      box-shadow:var(--shadow);
    }

    .stepper{
      padding:14px;
      display:flex;
      gap:10px;
      flex-wrap:wrap;
    }
    .step{
      flex:1 1 180px;
      border:1px solid var(--line);
      background:#f8fbff
;
      color:var(--muted);
      border-radius:18px;
      padding:12px 14px;
      text-align:left;
      cursor:pointer;
      transition:.2s ease;
    }
    .step strong{display:block;color:var(--text);margin-bottom:4px}
    .step.active{
      background:linear-gradient(135deg, rgba(37,99,235,.11), rgba(14,165,233,.08));
      border-color:rgba(37,99,235,.35);
      color:var(--text);
    }
    .step.done{
      border-color:rgba(22,163,74,.24);
      background:rgba(22,163,74,.05);
    }

    .pane{
      padding:22px;
      display:none;
    }
    .pane.active{display:block}
    .pane h2{
      margin:0 0 8px;
      font-size:22px;
    }
    .pane .sub{
      margin:0 0 18px;
      color:var(--muted);
      line-height:1.6;
      font-size:14px;
    }
    .grid{
      display:grid;
      grid-template-columns:repeat(2, minmax(0,1fr));
      gap:14px;
    }
    .grid-3{
      display:grid;
      grid-template-columns:repeat(3, minmax(0,1fr));
      gap:14px;
    }
    .field{
      border:1px solid var(--line);
      border-radius:18px;
      padding:14px;
      background:#fbfdff
;
    }
    .field.wide{grid-column:1 / -1}
    .label{
      display:flex;
      justify-content:space-between;
      gap:10px;
      font-weight:700;
      margin-bottom:8px;
    }
    .label small{
      font-weight:600;
      color:var(--muted);
    }
    .hint{
      font-size:12px;
      color:var(--muted);
      margin-top:8px;
      line-height:1.55;
    }
    input[type="text"],
    input[type="tel"],
    input[type="number"],
    select{
      width:100%;
      border:1px solid #cbd9ea
;
      background:#fff;
      border-radius:14px;
      padding:12px 14px;
      font-size:15px;
      outline:none;
      transition:.18s ease;
      color:var(--text);
    }
    input:focus, select:focus{
      border-color:rgba(37,99,235,.65);
      box-shadow:0 0 0 4px rgba(37,99,235,.08);
    }
    .radio-group{
      display:flex;
      gap:12px;
      flex-wrap:wrap;
    }
    .radio{
      flex:1 1 240px;
      border:1px solid #cad7e8
;
      background:#fff;
      border-radius:16px;
      padding:14px;
      display:flex;
      gap:10px;
      align-items:flex-start;
    }
    .radio input{margin-top:4px}
    .radio strong{display:block;margin-bottom:2px}
    .radio span{display:block;color:var(--muted);font-size:13px;line-height:1.45}
    .toggle-box{
      margin-top:10px;
      padding:14px;
      border-radius:16px;
      background:rgba(14,165,233,.05);
      border:1px dashed rgba(14,165,233,.28);
    }

    .actions{
      display:flex;
      gap:10px;
      justify-content:space-between;
      flex-wrap:wrap;
      margin-top:18px;
    }
    .btn{
      border:none;
      border-radius:14px;
      padding:13px 18px;
      font-weight:800;
      cursor:pointer;
      transition:.2s ease;
      font-size:15px;
    }
    .btn:hover{transform:translateY(-1px)}
    .btn.secondary{
      background:#eef4ff
;
      color:#234b8a
;
      border:1px solid #cbdaf7
;
    }
    .btn.primary{
      background:linear-gradient(135deg, var(--primary), var(--primary2));
      color:white;
      box-shadow:0 12px 25px rgba(37,99,235,.22);
    }
    .btn.accent{
      background:linear-gradient(135deg, var(--accent), var(--accent2));
      color:#fff;
      box-shadow:0 12px 25px rgba(245,158,11,.23);
    }
    .btn:disabled{
      opacity:.55;
      cursor:not-allowed;
      transform:none;
    }

    .summary{
      position:sticky;
      top:20px;
      padding:20px;
    }
    .summary h3{
      margin:0 0 10px;
      font-size:20px;
    }
    .summary .mini{
      color:var(--muted);
      margin:0 0 16px;
      line-height:1.55;
      font-size:14px;
    }
    .big-tax{
      background:linear-gradient(135deg, #fff8e6
, #fff3d4
);
      border:1px solid rgba(245,158,11,.24);
      border-radius:20px;
      padding:18px;
      margin-bottom:16px;
    }
    .big-tax .amount{
      font-size:34px;
      font-weight:900;
      color:#b45309
;
      letter-spacing:.2px;
      margin-top:6px;
    }
    .stat-grid{
      display:grid;
      grid-template-columns:1fr 1fr;
      gap:12px;
      margin-bottom:16px;
    }
    .stat{
      background:#f8fbff
;
      border:1px solid var(--line);
      border-radius:18px;
      padding:14px;
    }
    .stat small{color:var(--muted);display:block;margin-bottom:6px}
    .stat strong{font-size:18px}
    .breakdown{
      border-top:1px solid var(--line);
      padding-top:14px;
      margin-top:4px;
    }
    .breakdown h4{
      margin:0 0 10px;
      font-size:16px;
    }
    .item{
      display:flex;
      justify-content:space-between;
      gap:10px;
      padding:10px 0;
      border-bottom:1px dashed #e1e9f4
;
      font-size:14px;
    }
    .item:last-child{border-bottom:none}
    .item span:first-child{color:var(--muted)}
    .note{
      margin-top:12px;
      font-size:13px;
      color:var(--muted);
      line-height:1.55;
      background:#f7fbff
;
      border:1px solid var(--line);
      padding:12px;
      border-radius:14px;
    }

    .contact-box{
      margin-top:16px;
      background:linear-gradient(135deg, #eff6ff
, #ffffff
);
      border:1px solid #cfe2ff
;
      border-radius:20px;
      padding:16px;
      display:none;
    }
    .contact-box.show{display:block}
    .contact-box h4{margin:0 0 8px;font-size:18px}
    .contact-box p{margin:0 0 14px;color:var(--muted);font-size:14px;line-height:1.6}
    .send-state{
      margin-top:10px;
      font-size:14px;
      font-weight:700;
    }
    .send-state.ok{color:var(--success)}
    .send-state.err{color:var(--danger)}
    .footer{
      margin:20px 0 4px;
      color:var(--muted);
      font-size:12px;
      text-align:center;
    }

    @media (max-width: 980px){
      .layout{grid-template-columns:1fr}
      .summary{position:static}
      .grid-3{grid-template-columns:1fr}
      .grid{grid-template-columns:1fr}
      .stat-grid{grid-template-columns:1fr}
    }
  </style>
</head>
<body>
  <div class="wrap">
    <section class="hero">
      <div class="badge">🧾 ปีภาษี 2569 • เครื่องมือจับมือสอนตั้งแต่เริ่มต้น</div>
      <h1>ระบบคำนวณภาษี ปี 2569</h1>
      <p>
        สำหรับพ่อค้าแม่ค้าออนไลน์และผู้ประกอบการธุรกิจบุคคลธรรมดา
        กรอกง่าย คำนวณอัตโนมัติ แสดงผลภาษีแบบเข้าใจได้ทันที พร้อมฟอร์มติดต่อกลับเพื่อส่งข้อมูลเข้า Google Sheets
      </p>
    </section>

    <div class="layout">
      <!-- LEFT: FORM -->
      <section class="card">
        <div class="stepper">
          <button class="step active" data-step="1"><strong>Step 1</strong>รายได้และค่าใช้จ่าย</button>
          <button class="step" data-step="2"><strong>Step 2</strong>รายการลดหย่อน</button>
          <button class="step" data-step="3"><strong>Step 3</strong>สรุปผลและติดต่อกลับ</button>
        </div>

        <!-- STEP 1 -->
        <div class="pane active" id="step1">
          <h2>Step 1: รายได้และค่าใช้จ่าย</h2>
          <p class="sub">เริ่มจากกรอกรายได้รวมของทั้งปี และเลือกวิธีหักค่าใช้จ่ายเพียง 1 วิธี</p>

          <div class="grid">
            <div class="field wide">
              <div class="label">
                <span>รายได้รวมทั้งปี</span>
                <small title="ใส่ยอดรายได้รวมจากธุรกิจ/ขายของออนไลน์ทั้งปี">💡 คำอธิบาย</small>
              </div>
              <input id="grossIncome" type="number" min="0" step="1" placeholder="เช่น 1,200,000" />
              <div class="hint">ใส่รายได้รวมก่อนหักค่าใช้จ่ายและค่าลดหย่อน</div>
            </div>

            <div class="field wide">
              <div class="label">
                <span>เลือกวิธีหักค่าใช้จ่าย</span>
                <small>เลือกได้เพียงวิธีเดียว</small>
              </div>
              <div class="radio-group">
                <label class="radio">
                  <input type="radio" name="expenseMethod" value="presumptive" checked />
                  <div>
                    <strong>หักแบบเหมา 60%</strong>
                    <span>ระบบคำนวณค่าใช้จ่ายให้ทันทีจากรายได้รวม</span>
                  </div>
                </label>
                <label class="radio">
                  <input type="radio" name="expenseMethod" value="actual" />
                  <div>
                    <strong>หักตามจริง</strong>
                    <span>กรอกยอดค่าใช้จ่ายที่เกิดขึ้นจริงตามเอกสารของคุณ</span>
                  </div>
                </label>
              </div>
            </div>

            <div class="field wide" id="actualExpenseBox" style="display:none">
              <div class="label">
                <span>ค่าใช้จ่ายตามจริง</span>
                <small>แสดงเมื่อเลือกหักตามจริง</small>
              </div>
              <input id="actualExpense" type="number" min="0" step="1" placeholder="เช่น 450,000" />
              <div class="hint">ระบบจะนำค่านี้ไปใช้แทนการหักแบบเหมา</div>
            </div>
          </div>

          <div class="actions">
            <button class="btn secondary" type="button" disabled>ย้อนกลับ</button>
            <button class="btn primary" type="button" onclick="goStep(2)">ถัดไป</button>
          </div>
        </div>

        <!-- STEP 2 -->
        <div class="pane" id="step2">
          <h2>Step 2: รายการลดหย่อน</h2>
          <p class="sub">กรอกเฉพาะรายการที่มีจริง ระบบจะช่วยคุมเพดานให้อัตโนมัติ</p>

          <div class="grid-3">
            <div class="field">
              <div class="label"><span>ค่าลดหย่อนส่วนตัว</span><small>อัตโนมัติ</small></div>
              <input id="personalDeduction" type="number" value="60000" disabled />
              <div class="hint">60,000 บาท ใส่ให้อัตโนมัติ</div>
            </div>

            <div class="field">
              <div class="label"><span>คู่สมรส (ไม่มีรายได้)</span><small>บาท</small></div>
              <input id="spouseDeduction" type="number" min="0" step="1" value="0" />
              <div class="hint">ใส่ 60,000 หากมีสิทธิ์</div>
            </div>

            <div class="field">
              <div class="label"><span>บุตรคนละ 30,000</span><small>คน</small></div>
              <input id="children30Count" type="number" min="0" step="1" value="0" />
              <div class="hint">สำหรับบุตรที่เข้าเงื่อนไข 30,000 บาทต่อคน</div>
            </div>

            <div class="field">
              <div class="label"><span>บุตรคนละ 60,000</span><small>คน</small></div>
              <input id="children60Count" type="number" min="0" step="1" value="0" />
              <div class="hint">สำหรับบุตรคนที่ 2 เป็นต้นไปที่เกิดตั้งแต่ปี 2561</div>
            </div>

            <div class="field">
              <div class="label"><span>บิดามารดา</span><small>คน</small></div>
              <input id="parentsCount" type="number" min="0" max="2" step="1" value="0" />
              <div class="hint">คนละ 30,000 บาท (อายุ 60 ปีขึ้นไป)</div>
            </div>

            <div class="field">
              <div class="label"><span>ผู้พิการ/ทุพพลภาพ</span><small>คน</small></div>
              <input id="disabledCount" type="number" min="0" step="1" value="0" />
              <div class="hint">คนละ 60,000 บาท</div>
            </div>

            <div class="field">
              <div clas
