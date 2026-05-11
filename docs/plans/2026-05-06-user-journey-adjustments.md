# User Journey Adjustments — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** สร้าง screens ใหม่และปรับ screens ที่มีอยู่ตาม User Journey "ใบอนุญาต" ที่คุยกับลูกค้า แบ่ง Phase 1 (Registration + Field Officer) และ Phase 2 (AI Card + ใบขวาง)

**Architecture:** HTML mockup files แบบ self-contained — ทุกไฟล์มี CSS inline ใน `<style>` tag และ JS inline ใน `<script>` tag ไม่มี external stylesheet หรือ JS bundle ใช้ CSS variables system จาก `:root` block เดียวกันกับไฟล์ที่มีอยู่แล้ว

**Tech Stack:** HTML5, CSS3 (CSS variables), Vanilla JS, Google Fonts (Sarabun + Noto Serif Thai), Leaflet.js (สำหรับแผนที่ใน Field Officer)

---

## CSS Variables & Design Tokens (อ้างอิงตลอดทุก task)

```css
:root {
  --navy:#1B3A6B; --navy-dark:#0F2447; --navy-mid:#2C5AA0; --navy-lt:#E8EEF7; --navy-xlt:#F4F7FC;
  --gold:#B8860B; --gold-bright:#D4A017; --gold-lt:#FBF4E0;
  --teal:#0A5C47; --teal-mid:#0F7A5E; --teal-lt:#E2F4EE;
  --ok:#166534; --ok-lt:#DCFCE7; --ok-mid:#16A34A;
  --warn:#92400E; --warn-lt:#FEF3C7; --warn-mid:#D97706;
  --err:#991B1B; --err-lt:#FEE2E2; --err-mid:#DC2626;
  --surface:#F0F2F7; --border:rgba(27,58,107,0.10); --border-mid:rgba(27,58,107,0.18);
  --text:#0F172A; --text-2:#374151; --text-3:#94A3B8; --white:#fff;
  --shadow-sm:0 1px 3px rgba(15,20,40,.06),0 1px 2px rgba(15,20,40,.04);
  --shadow-md:0 4px 16px rgba(15,36,71,.08),0 1px 4px rgba(15,36,71,.04);
  --shadow-lg:0 12px 40px rgba(15,36,71,.13),0 4px 12px rgba(15,36,71,.06);
  --radius:12px; --radius-lg:18px; --radius-xl:24px;
}
```

**Font import (วางใน `<head>` ทุกไฟล์):**
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Noto+Serif+Thai:wght@400;500;600;700;800&family=Sarabun:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
```

**Button classes (ใช้ซ้ำทุก task):**
```css
.btn { padding:10px 20px; border-radius:10px; border:none; font-size:14px; font-weight:700;
       cursor:pointer; font-family:'Sarabun',sans-serif; transition:all .2s; }
.btn-primary { background:var(--navy); color:#fff; }
.btn-primary:hover { background:var(--navy-mid); }
.btn-outline { background:transparent; color:var(--navy); border:1.5px solid var(--navy); }
.btn-outline:hover { background:var(--navy-lt); }
.btn-teal { background:var(--teal); color:#fff; }
.btn-teal:hover { background:#0a5442; }
.btn-sm { padding:6px 14px; font-size:12px; border-radius:7px; }
```

---

## ═══════════════════════════════
## PHASE 1
## ═══════════════════════════════

---

## Task 1: แก้ไขหน้า Login — เพิ่มปุ่ม "สมัครใช้งาน"

**Files:**
- Modify: `html-screens/01_ทั่วไป/01_หน้าเข้าสู่ระบบ.html`

- [ ] **Step 1: เปิดไฟล์และหาตำแหน่งปุ่ม Login**

  เปิดไฟล์ `html-screens/01_ทั่วไป/01_หน้าเข้าสู่ระบบ.html` หา element ที่มี class `.btn-login` จะอยู่ในส่วนฟอร์ม login ด้านขวา

- [ ] **Step 2: เพิ่มปุ่มและ link สมัครใช้งานใต้ปุ่ม Login**

  หลังจาก `<button class="btn-login">` เพิ่ม:
  ```html
  <div class="register-link" style="text-align:center;margin-top:16px;">
    <span style="font-size:13px;color:var(--text-3);">ยังไม่มีบัญชีใช้งาน?</span>
    <a href="03_หน้าสมัครใช้งาน_Step1_เลือกประเภท.html"
       style="font-size:13px;font-weight:700;color:var(--navy-mid);text-decoration:none;margin-left:6px;">
      สมัครใช้งาน →
    </a>
  </div>
  ```

- [ ] **Step 3: เพิ่ม demo button สำหรับ Field Officer ใน demo section**

  หา `.login-demo` div ที่มี demo buttons แล้วเพิ่ม button สำหรับ Field Officer:
  ```html
  <button class="demo-btn d-field" onclick="/* navigate to field officer */">
    <span class="demo-btn-icon">🏗️</span>
    เจ้าหน้าที่<br>ภาคสนาม
  </button>
  ```
  เพิ่ม CSS:
  ```css
  .demo-btn.d-field { color:var(--warn); border-color:rgba(146,64,14,.25); }
  .demo-btn.d-field:hover { background:var(--warn-lt); border-color:var(--warn); }
  ```

- [ ] **Step 4: เปิดไฟล์ในเบราว์เซอร์และตรวจสอบ**
  - ปุ่ม "สมัครใช้งาน" แสดงใต้ปุ่ม Login
  - link คลิกได้และนำไปที่ Step 1 (ถ้า Step 1 ทำแล้ว)
  - Demo button Field Officer แสดงใน demo section

---

## Task 2: หน้าสมัครใช้งาน Step 1 — เลือกประเภทผู้ประกอบการ

**Files:**
- Create: `html-screens/01_ทั่วไป/03_หน้าสมัครใช้งาน_Step1_เลือกประเภท.html`

- [ ] **Step 1: สร้างโครงสร้าง HTML พื้นฐาน**

  Copy header (DOCTYPE, head, font links, CSS variables) จาก `14_ยื่นคำขอ_Step1_ข้อมูลผู้ขอ.html` มาเป็นฐาน แล้วแก้ `<title>`:
  ```html
  <title>สมัครใช้งาน — ระบบน้ำบาดาล GCL</title>
  ```

- [ ] **Step 2: สร้าง Step progress bar (4 steps)**

  ```html
  <div class="form-step-banner">
    <div class="step-track">
      <!-- Step 1: active -->
      <div class="step-item step-active">
        <div class="step-circle">1</div>
        <div class="step-label">เลือกประเภท</div>
      </div>
      <div class="step-connector"><div class="step-connector-line" style="width:100%;background:#E8ECF4"></div></div>
      <!-- Step 2: todo -->
      <div class="step-item step-todo">
        <div class="step-circle">2</div>
        <div class="step-label">ข้อมูลส่วนตัว</div>
      </div>
      <div class="step-connector"><div class="step-connector-line" style="width:100%;background:#E8ECF4"></div></div>
      <!-- Step 3: todo -->
      <div class="step-item step-todo">
        <div class="step-circle">3</div>
        <div class="step-label">ตั้งค่าบัญชี</div>
      </div>
      <div class="step-connector"><div class="step-connector-line" style="width:100%;background:#E8ECF4"></div></div>
      <!-- Step 4: todo -->
      <div class="step-item step-todo">
        <div class="step-circle">4</div>
        <div class="step-label">ยืนยัน</div>
      </div>
    </div>
    <div class="step-progress-bar"><div class="step-progress-fill" style="width:12%"></div></div>
  </div>
  ```

  CSS สำหรับ step classes (copy จาก existing file):
  ```css
  .step-circle{width:28px;height:28px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:12px;font-weight:700;flex-shrink:0}
  .step-label{font-size:11px;font-weight:600;margin-top:5px;white-space:nowrap}
  .step-item{display:flex;flex-direction:column;align-items:center}
  .step-connector{display:flex;align-items:center;flex:1;margin-top:-11px}
  .step-connector-line{height:2px;width:100%}
  .step-todo .step-circle{background:#E8ECF4;color:var(--text-3)}
  .step-active .step-circle{background:var(--navy);color:#fff;box-shadow:0 0 0 4px rgba(27,58,107,.15)}
  .step-done .step-circle{background:var(--ok);color:#fff}
  .step-todo .step-label{color:var(--text-3)}
  .step-active .step-label{color:var(--navy);font-weight:700}
  .step-done .step-label{color:var(--ok)}
  .step-progress-bar{height:3px;background:#E8ECF4;position:relative;overflow:hidden}
  .step-progress-fill{height:100%;background:linear-gradient(90deg,var(--navy),var(--navy-mid));transition:width .5s;border-radius:0 3px 3px 0}
  ```

- [ ] **Step 3: สร้าง type selection cards**

  ```html
  <main style="flex:1;overflow-y:auto;padding:40px;display:flex;align-items:flex-start;justify-content:center;">
    <div style="width:100%;max-width:680px;">
      <h2 style="font-family:'Noto Serif Thai',sans-serif;font-size:22px;font-weight:700;color:var(--navy);margin-bottom:8px;">
        เลือกประเภทผู้ประกอบการ
      </h2>
      <p style="color:var(--text-3);font-size:14px;margin-bottom:32px;">กรุณาเลือกประเภทที่ตรงกับท่าน เพื่อให้ระบบเตรียมแบบฟอร์มที่เหมาะสม</p>

      <div class="type-cards" style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:16px;margin-bottom:32px;">

        <!-- บุคคลธรรมดา -->
        <label class="type-card" for="type-individual">
          <input type="radio" name="org-type" id="type-individual" value="individual" style="display:none">
          <div class="type-card-icon">👤</div>
          <div class="type-card-title">บุคคลธรรมดา</div>
          <div class="type-card-desc">ใช้เลขบัตรประชาชน 13 หลักในการสมัคร</div>
        </label>

        <!-- นิติบุคคล -->
        <label class="type-card" for="type-company">
          <input type="radio" name="org-type" id="type-company" value="company" style="display:none">
          <div class="type-card-icon">🏢</div>
          <div class="type-card-title">นิติบุคคล</div>
          <div class="type-card-desc">บริษัท ห้างหุ้นส่วน หรือองค์กรธุรกิจ</div>
        </label>

        <!-- วัด/โรงเรียน -->
        <label class="type-card" for="type-temple">
          <input type="radio" name="org-type" id="type-temple" value="temple" style="display:none">
          <div class="type-card-icon">🏛️</div>
          <div class="type-card-title">วัดหรือโรงเรียน</div>
          <div class="type-card-desc">สถาบันการศึกษาหรือศาสนสถาน</div>
        </label>
      </div>

      <!-- ปุ่ม navigation -->
      <div style="display:flex;justify-content:space-between;align-items:center;">
        <a href="01_หน้าเข้าสู่ระบบ.html" class="btn btn-outline">← กลับ</a>
        <button id="btn-next" class="btn btn-primary" disabled onclick="goToStep2()">ถัดไป →</button>
      </div>
    </div>
  </main>
  ```

  CSS สำหรับ type cards:
  ```css
  .type-card{
    display:flex;flex-direction:column;align-items:center;text-align:center;
    padding:28px 20px;border-radius:16px;border:2px solid var(--border-mid);
    background:#fff;cursor:pointer;transition:all .2s;
  }
  .type-card:hover{border-color:var(--navy-mid);background:var(--navy-xlt);transform:translateY(-2px);box-shadow:var(--shadow-md)}
  .type-card:has(input:checked){border-color:var(--navy);background:var(--navy-lt);box-shadow:0 0 0 4px rgba(27,58,107,.1)}
  .type-card-icon{font-size:40px;margin-bottom:12px}
  .type-card-title{font-size:15px;font-weight:700;color:var(--navy);margin-bottom:6px}
  .type-card-desc{font-size:12px;color:var(--text-3);line-height:1.5}
  ```

- [ ] **Step 4: เพิ่ม JS เปิด/ปิดปุ่ม และ navigate**

  ```html
  <script>
    const radios = document.querySelectorAll('input[name="org-type"]');
    const btnNext = document.getElementById('btn-next');
    radios.forEach(r => r.addEventListener('change', () => {
      btnNext.disabled = false;
      localStorage.setItem('gcl_org_type', r.value);
    }));
    function goToStep2() {
      window.location.href = '04_หน้าสมัครใช้งาน_Step2_ข้อมูลส่วนตัว.html';
    }
  </script>
  ```

- [ ] **Step 5: เปิดในเบราว์เซอร์ ตรวจสอบ**
  - Step 1 ใน progress bar แสดงสี navy (active)
  - Card 3 ใบแสดงครบ ครบ grid 3 columns
  - เลือก card แล้วปุ่ม "ถัดไป" เปิดใช้งาน
  - Hover effect ทำงาน

---

## Task 3: หน้าสมัครใช้งาน Step 2 — ข้อมูลส่วนตัว (dynamic form)

**Files:**
- Create: `html-screens/01_ทั่วไป/04_หน้าสมัครใช้งาน_Step2_ข้อมูลส่วนตัว.html`

- [ ] **Step 1: สร้าง layout และ step progress (Step 2 active, Step 1 done)**

  ใช้โครงสร้างเดียวกับ Task 2 แต่:
  - Step 1: `step-done` — วงกลมแสดง ✓
  - Step 2: `step-active`
  - Step 3, 4: `step-todo`
  - `step-progress-fill` width = `37%`

- [ ] **Step 2: สร้างฟอร์ม 3 variants ในหน้าเดียว (toggle by JS)**

  ```html
  <main style="flex:1;overflow-y:auto;padding:40px;display:flex;justify-content:center;">
    <div style="width:100%;max-width:680px;">
      <h2 style="font-family:'Noto Serif Thai',sans-serif;font-size:22px;font-weight:700;color:var(--navy);margin-bottom:24px;">
        ข้อมูล<span id="form-type-label">ส่วนตัว</span>
      </h2>

      <!-- Variant: บุคคลธรรมดา -->
      <div id="form-individual" class="type-form">
        <div class="form-row">
          <div class="field-group"><label>ชื่อ *</label><input type="text" placeholder="ชื่อ"></div>
          <div class="field-group"><label>นามสกุล *</label><input type="text" placeholder="นามสกุล"></div>
        </div>
        <div class="field-group">
          <label>เลขบัตรประชาชน 13 หลัก *</label>
          <input type="text" maxlength="13" placeholder="x-xxxx-xxxxx-xx-x">
        </div>
        <div class="field-group">
          <label>วันเกิด *</label>
          <input type="date">
        </div>
        <div class="field-group">
          <label>ที่อยู่ปัจจุบัน *</label>
          <textarea rows="3" placeholder="บ้านเลขที่ ถนน แขวง/ตำบล เขต/อำเภอ จังหวัด รหัสไปรษณีย์"></textarea>
        </div>
      </div>

      <!-- Variant: นิติบุคคล -->
      <div id="form-company" class="type-form" style="display:none">
        <div class="field-group"><label>ชื่อบริษัท/ห้างหุ้นส่วน *</label><input type="text" placeholder="ชื่อนิติบุคคล"></div>
        <div class="field-group">
          <label>เลขทะเบียนนิติบุคคล 13 หลัก *</label>
          <input type="text" maxlength="13" placeholder="0-xxxx-xxxxx-xx-x">
        </div>
        <div class="field-group"><label>ที่อยู่จดทะเบียน *</label><textarea rows="3" placeholder="ที่อยู่ตามหนังสือรับรองบริษัท"></textarea></div>
        <div class="field-group"><label>ชื่อผู้มีอำนาจลงนาม *</label><input type="text" placeholder="ชื่อ-นามสกุล กรรมการผู้มีอำนาจ"></div>
      </div>

      <!-- Variant: วัด/โรงเรียน -->
      <div id="form-temple" class="type-form" style="display:none">
        <div class="field-group"><label>ชื่อวัด/โรงเรียน *</label><input type="text" placeholder="ชื่อสถาบัน"></div>
        <div class="field-group"><label>รหัสองค์กร (ถ้ามี)</label><input type="text" placeholder="รหัสประจำสถาบัน"></div>
        <div class="field-group"><label>ที่อยู่ *</label><textarea rows="3" placeholder="ที่อยู่ของวัด/โรงเรียน"></textarea></div>
        <div class="field-group"><label>ชื่อผู้ดูแล/ผู้แทน *</label><input type="text" placeholder="ชื่อ-นามสกุล"></div>
      </div>

      <!-- Navigation -->
      <div style="display:flex;justify-content:space-between;margin-top:32px;">
        <a href="03_หน้าสมัครใช้งาน_Step1_เลือกประเภท.html" class="btn btn-outline">← ย้อนกลับ</a>
        <button class="btn btn-primary" onclick="window.location.href='05_หน้าสมัครใช้งาน_Step3_ตั้งค่าบัญชี.html'">ถัดไป →</button>
      </div>
    </div>
  </main>
  ```

  CSS:
  ```css
  .form-row{display:grid;grid-template-columns:1fr 1fr;gap:16px}
  .field-group{display:flex;flex-direction:column;gap:6px;margin-bottom:16px}
  .field-group label{font-size:13px;font-weight:600;color:var(--text-2)}
  .field-group input,.field-group textarea,.field-group select{
    padding:11px 14px;border-radius:9px;border:1.5px solid #D8DCE8;
    font-size:14px;font-family:'Sarabun',sans-serif;color:var(--text);outline:none;
    transition:border-color .2s;background:#FAFBFD;
  }
  .field-group input:focus,.field-group textarea:focus{border-color:var(--navy-mid)}
  ```

- [ ] **Step 3: JS แสดง form ตาม type ที่เลือกใน Step 1**

  ```html
  <script>
    const orgType = localStorage.getItem('gcl_org_type') || 'individual';
    const labels = { individual:'ส่วนตัว', company:'องค์กร', temple:'สถาบัน' };
    document.getElementById('form-type-label').textContent = labels[orgType];
    ['individual','company','temple'].forEach(t => {
      document.getElementById('form-'+t).style.display = t === orgType ? 'block' : 'none';
    });
  </script>
  ```

- [ ] **Step 4: เปิดในเบราว์เซอร์ ตรวจสอบ**
  - ทดสอบโดยตั้งค่า `localStorage.setItem('gcl_org_type','company')` แล้ว refresh — ฟอร์มนิติบุคคลควรแสดง
  - ทำซ้ำกับ `'temple'`
  - ฟิลด์ทุกอันมี label และ placeholder ถูกต้อง

---

## Task 4: หน้าสมัครใช้งาน Step 3 — ตั้งค่าบัญชี

**Files:**
- Create: `html-screens/01_ทั่วไป/05_หน้าสมัครใช้งาน_Step3_ตั้งค่าบัญชี.html`

- [ ] **Step 1: สร้าง layout + Step progress (1=done, 2=done, 3=active, 4=todo)**

  Progress fill width = `62%`

- [ ] **Step 2: สร้างฟอร์มตั้งค่าบัญชี**

  ```html
  <div style="width:100%;max-width:520px;margin:0 auto;padding:40px 0">
    <h2 style="font-family:'Noto Serif Thai',sans-serif;font-size:22px;font-weight:700;color:var(--navy);margin-bottom:24px;">ตั้งค่าบัญชีผู้ใช้งาน</h2>

    <div class="field-group">
      <label>อีเมล *</label>
      <input type="email" id="email" placeholder="example@email.com">
    </div>
    <div class="field-group">
      <label>เบอร์โทรศัพท์ *</label>
      <input type="tel" id="phone" maxlength="10" placeholder="0812345678">
    </div>
    <div class="field-group">
      <label>รหัสผ่าน *</label>
      <input type="password" id="pw" placeholder="อย่างน้อย 8 ตัวอักษร">
      <span style="font-size:11px;color:var(--text-3)">ต้องมีตัวอักษรพิมพ์ใหญ่ พิมพ์เล็ก และตัวเลขอย่างน้อย 1 ตัว</span>
    </div>
    <div class="field-group">
      <label>ยืนยันรหัสผ่าน *</label>
      <input type="password" id="pw2" placeholder="กรอกรหัสผ่านอีกครั้ง">
      <span id="pw-match-msg" style="font-size:11px;display:none"></span>
    </div>

    <div style="display:flex;justify-content:space-between;margin-top:32px;">
      <a href="04_หน้าสมัครใช้งาน_Step2_ข้อมูลส่วนตัว.html" class="btn btn-outline">← ย้อนกลับ</a>
      <button class="btn btn-primary" onclick="goNext()">ถัดไป →</button>
    </div>
  </div>
  ```

- [ ] **Step 3: เพิ่ม JS validation**

  ```html
  <script>
    function goNext() {
      const pw = document.getElementById('pw').value;
      const pw2 = document.getElementById('pw2').value;
      const msg = document.getElementById('pw-match-msg');
      if (pw !== pw2) {
        msg.textContent = '❌ รหัสผ่านไม่ตรงกัน';
        msg.style.color = 'var(--err)';
        msg.style.display = 'block';
        return;
      }
      msg.style.display = 'none';
      window.location.href = '06_หน้าสมัครใช้งาน_Step4_ยืนยัน.html';
    }
  </script>
  ```

- [ ] **Step 4: เปิดในเบราว์เซอร์ ตรวจสอบ**
  - รหัสผ่านไม่ตรงกัน → error message แสดง
  - รหัสผ่านตรงกัน → navigate ไป Step 4

---

## Task 5: หน้าสมัครใช้งาน Step 4 — ยืนยัน + หน้าสำเร็จ

**Files:**
- Create: `html-screens/01_ทั่วไป/06_หน้าสมัครใช้งาน_Step4_ยืนยัน.html`
- Create: `html-screens/01_ทั่วไป/07_หน้าสมัครสำเร็จ.html`

- [ ] **Step 1: Step 4 — สร้าง summary card + OTP section**

  Progress: Steps 1-3 = done, Step 4 = active, fill = `88%`

  ```html
  <!-- Summary card -->
  <div style="background:#fff;border-radius:16px;border:1px solid var(--border);padding:28px;margin-bottom:24px;">
    <div style="font-size:13px;font-weight:700;color:var(--text-3);text-transform:uppercase;letter-spacing:.06em;margin-bottom:16px;">สรุปข้อมูลการสมัคร</div>
    <div style="display:grid;gap:10px;">
      <div style="display:flex;justify-content:space-between;">
        <span style="font-size:13px;color:var(--text-3)">ประเภท</span>
        <span style="font-size:13px;font-weight:600;" id="sum-type">—</span>
      </div>
      <div style="display:flex;justify-content:space-between;">
        <span style="font-size:13px;color:var(--text-3)">ชื่อ/องค์กร</span>
        <span style="font-size:13px;font-weight:600;">ตัวอย่าง ผู้สมัคร</span>
      </div>
      <div style="display:flex;justify-content:space-between;">
        <span style="font-size:13px;color:var(--text-3)">อีเมล</span>
        <span style="font-size:13px;font-weight:600;">example@email.com</span>
      </div>
      <div style="display:flex;justify-content:space-between;">
        <span style="font-size:13px;color:var(--text-3)">เบอร์โทร</span>
        <span style="font-size:13px;font-weight:600;">081-234-5678</span>
      </div>
    </div>
  </div>

  <!-- OTP -->
  <div style="background:var(--navy-xlt);border-radius:16px;border:1px solid var(--border-mid);padding:24px;margin-bottom:24px;text-align:center;">
    <div style="font-size:14px;font-weight:600;color:var(--navy);margin-bottom:8px;">ยืนยันตัวตนผ่านอีเมล</div>
    <div style="font-size:13px;color:var(--text-3);margin-bottom:16px;">รหัส OTP 6 หลักถูกส่งไปที่ example@email.com</div>
    <div style="display:flex;gap:8px;justify-content:center;margin-bottom:12px;">
      <input type="text" maxlength="1" style="width:44px;height:52px;text-align:center;font-size:20px;font-weight:700;border-radius:10px;border:2px solid var(--border-mid);font-family:'Sarabun',sans-serif;">
      <input type="text" maxlength="1" style="width:44px;height:52px;text-align:center;font-size:20px;font-weight:700;border-radius:10px;border:2px solid var(--border-mid);font-family:'Sarabun',sans-serif;">
      <input type="text" maxlength="1" style="width:44px;height:52px;text-align:center;font-size:20px;font-weight:700;border-radius:10px;border:2px solid var(--border-mid);font-family:'Sarabun',sans-serif;">
      <input type="text" maxlength="1" style="width:44px;height:52px;text-align:center;font-size:20px;font-weight:700;border-radius:10px;border:2px solid var(--border-mid);font-family:'Sarabun',sans-serif;">
      <input type="text" maxlength="1" style="width:44px;height:52px;text-align:center;font-size:20px;font-weight:700;border-radius:10px;border:2px solid var(--border-mid);font-family:'Sarabun',sans-serif;">
      <input type="text" maxlength="1" style="width:44px;height:52px;text-align:center;font-size:20px;font-weight:700;border-radius:10px;border:2px solid var(--border-mid);font-family:'Sarabun',sans-serif;">
    </div>
    <a href="#" style="font-size:12px;color:var(--navy-mid);">ส่งรหัสอีกครั้ง (02:59)</a>
  </div>

  <div style="display:flex;justify-content:space-between;">
    <a href="05_หน้าสมัครใช้งาน_Step3_ตั้งค่าบัญชี.html" class="btn btn-outline">← ย้อนกลับ</a>
    <button class="btn btn-teal" onclick="window.location.href='07_หน้าสมัครสำเร็จ.html'">✓ ยืนยันและสมัคร</button>
  </div>
  ```

  JS แสดง org type ใน summary:
  ```html
  <script>
    const labels = { individual:'บุคคลธรรมดา', company:'นิติบุคคล', temple:'วัดหรือโรงเรียน' };
    document.getElementById('sum-type').textContent = labels[localStorage.getItem('gcl_org_type')] || '—';
  </script>
  ```

- [ ] **Step 2: หน้าสมัครสำเร็จ (`07_หน้าสมัครสำเร็จ.html`)**

  ```html
  <div style="display:flex;align-items:center;justify-content:center;min-height:100vh;background:var(--surface);">
    <div style="text-align:center;background:#fff;border-radius:24px;padding:56px 48px;box-shadow:var(--shadow-lg);max-width:440px;">
      <div style="width:80px;height:80px;border-radius:50%;background:var(--ok-lt);display:flex;align-items:center;justify-content:center;font-size:36px;margin:0 auto 24px;">✓</div>
      <h2 style="font-family:'Noto Serif Thai',sans-serif;font-size:24px;font-weight:700;color:var(--navy);margin-bottom:12px;">สมัครใช้งานสำเร็จ</h2>
      <p style="font-size:14px;color:var(--text-3);line-height:1.7;margin-bottom:32px;">
        ระบบได้รับการสมัครของท่านแล้ว<br>กรุณาตรวจสอบอีเมลเพื่อยืนยันการสมัคร
      </p>
      <a href="01_หน้าเข้าสู่ระบบ.html" class="btn btn-primary" style="display:inline-block;text-decoration:none;">เข้าสู่ระบบ →</a>
    </div>
  </div>
  ```

- [ ] **Step 3: เปิดทั้ง 2 ไฟล์ในเบราว์เซอร์ ตรวจสอบ**
  - Summary card แสดงข้อมูลถูกต้อง
  - OTP boxes: กรอกตัวเลขได้, maxlength=1
  - หน้าสำเร็จ: icon ✓ แสดง, ปุ่มกลับ login ทำงาน

---

## Task 6: เจ้าหน้าที่ภาคสนาม — หน้าหลัก

**Files:**
- Create: `html-screens/05_เจ้าหน้าที่ภาคสนาม/01_หน้าหลัก.html`

- [ ] **Step 1: สร้าง App shell (sidebar + main layout)**

  Copy layout structure จาก `03_เจ้าหน้าที่/01_หน้าหลัก.html` แต่ปรับ sidebar color เป็น `--warn` (สีส้มเข้ม) เพื่อแยก role ชัดเจน:

  ```html
  <!-- Sidebar -->
  <aside style="width:240px;background:linear-gradient(180deg,var(--warn) 0%,#7A3408 100%);
                display:flex;flex-direction:column;padding:24px 0;flex-shrink:0;">
    <!-- Logo/Brand -->
    <div style="padding:0 20px 24px;border-bottom:1px solid rgba(255,255,255,.15)">
      <div style="font-family:'Noto Serif Thai',sans-serif;font-size:15px;font-weight:700;color:#fff;line-height:1.4">
        กรมทรัพยากรน้ำบาดาล
      </div>
      <div style="font-size:11px;color:rgba(255,255,255,.5);margin-top:4px">เจ้าหน้าที่ภาคสนาม</div>
    </div>

    <!-- Nav -->
    <nav style="padding:16px 12px;flex:1">
      <a href="01_หน้าหลัก.html" class="field-nav active">🏠 งานที่ได้รับมอบหมาย</a>
    </nav>

    <!-- User info -->
    <div style="padding:16px 20px;border-top:1px solid rgba(255,255,255,.15)">
      <div style="font-size:13px;font-weight:600;color:#fff">นายสมชาย ภาคสนาม</div>
      <div style="font-size:11px;color:rgba(255,255,255,.5)">เจ้าหน้าที่ภาคสนาม เขต 5</div>
    </div>
  </aside>
  ```

  CSS สำหรับ nav:
  ```css
  .field-nav{
    display:flex;align-items:center;gap:10px;padding:10px 12px;
    border-radius:10px;font-size:13px;font-weight:600;
    color:rgba(255,255,255,.7);text-decoration:none;transition:all .2s;margin-bottom:4px;
  }
  .field-nav:hover,.field-nav.active{background:rgba(255,255,255,.15);color:#fff}
  ```

- [ ] **Step 2: สร้าง job list ในส่วน main**

  ```html
  <main style="flex:1;overflow-y:auto;padding:32px 40px">
    <!-- Header -->
    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:24px">
      <div>
        <h1 style="font-family:'Noto Serif Thai',sans-serif;font-size:22px;font-weight:700;color:var(--navy)">งานที่ได้รับมอบหมาย</h1>
        <p style="font-size:13px;color:var(--text-3);margin-top:4px">รายการคำขอที่ต้องลงพื้นที่ตรวจสอบ</p>
      </div>
      <!-- Status filter tabs -->
      <div style="display:flex;gap:8px">
        <button class="btn btn-sm btn-primary">ทั้งหมด (5)</button>
        <button class="btn btn-sm btn-outline">รอลงพื้นที่ (3)</button>
        <button class="btn btn-sm btn-outline">ตรวจแล้ว (2)</button>
      </div>
    </div>

    <!-- Job cards -->
    <div style="display:flex;flex-direction:column;gap:12px">

      <!-- Job card: รอลงพื้นที่ -->
      <div class="job-card" onclick="window.location.href='02_บันทึกผลการตรวจสถานที่.html'">
        <div style="display:flex;align-items:flex-start;justify-content:space-between">
          <div>
            <div style="display:flex;align-items:center;gap:10px;margin-bottom:8px">
              <span class="status-badge status-pending">รอลงพื้นที่</span>
              <span style="font-size:12px;color:var(--text-3)">คำขอ #GCL-2025-00142</span>
            </div>
            <div style="font-size:15px;font-weight:700;color:var(--navy);margin-bottom:4px">นายสมศักดิ์ ใจดี</div>
            <div style="font-size:13px;color:var(--text-2)">เจาะน้ำบาดาลใหม่ — บ้านเลขที่ 12 ถ.พหลโยธิน อ.เมือง จ.เชียงใหม่</div>
          </div>
          <div style="text-align:right;flex-shrink:0;margin-left:20px">
            <div style="font-size:12px;color:var(--text-3)">กำหนดตรวจ</div>
            <div style="font-size:14px;font-weight:700;color:var(--warn)">08 พ.ค. 2568</div>
          </div>
        </div>
      </div>

      <!-- Job card: ตรวจแล้ว -->
      <div class="job-card" onclick="window.location.href='03_รายละเอียดผลที่ส่งแล้ว.html'" style="opacity:.7">
        <div style="display:flex;align-items:flex-start;justify-content:space-between">
          <div>
            <div style="display:flex;align-items:center;gap:10px;margin-bottom:8px">
              <span class="status-badge status-done">ตรวจแล้ว ✓</span>
              <span style="font-size:12px;color:var(--text-3)">คำขอ #GCL-2025-00138</span>
            </div>
            <div style="font-size:15px;font-weight:700;color:var(--navy);margin-bottom:4px">บริษัท น้ำไทย จำกัด</div>
            <div style="font-size:13px;color:var(--text-2)">ต่ออายุใบอนุญาต — นิคมอุตสาหกรรมภาคเหนือ จ.ลำพูน</div>
          </div>
          <div style="text-align:right;flex-shrink:0;margin-left:20px">
            <div style="font-size:12px;color:var(--text-3)">ส่งผลเมื่อ</div>
            <div style="font-size:14px;font-weight:700;color:var(--ok)">05 พ.ค. 2568</div>
          </div>
        </div>
      </div>

    </div>
  </main>
  ```

  CSS:
  ```css
  .job-card{background:#fff;border-radius:14px;border:1px solid var(--border);padding:20px 24px;
            cursor:pointer;transition:all .2s;}
  .job-card:hover{box-shadow:var(--shadow-md);transform:translateY(-1px);border-color:var(--border-mid)}
  .status-badge{padding:3px 10px;border-radius:20px;font-size:11px;font-weight:700}
  .status-pending{background:var(--warn-lt);color:var(--warn)}
  .status-done{background:var(--ok-lt);color:var(--ok)}
  ```

- [ ] **Step 3: เปิดในเบราว์เซอร์ ตรวจสอบ**
  - Sidebar สีส้ม แยกจาก role อื่น
  - Job cards แสดงครบ 2 states (รอ/ตรวจแล้ว)
  - คลิก card → navigate ไปหน้า 02 หรือ 03

---

## Task 7: เจ้าหน้าที่ภาคสนาม — บันทึกผลการตรวจสถานที่

**Files:**
- Create: `html-screens/05_เจ้าหน้าที่ภาคสนาม/02_บันทึกผลการตรวจสถานที่.html`

- [ ] **Step 1: สร้าง App shell เหมือน Task 6**

  Copy sidebar จาก `01_หน้าหลัก.html` (Task 6)

- [ ] **Step 2: สร้าง header ของ case ที่กำลังตรวจ**

  ```html
  <!-- Case header -->
  <div style="background:#fff;border-bottom:1px solid var(--border);padding:20px 40px;display:flex;align-items:center;gap:16px;flex-shrink:0">
    <a href="01_หน้าหลัก.html" style="font-size:20px;color:var(--text-3);text-decoration:none">←</a>
    <div>
      <div style="font-size:12px;color:var(--text-3);margin-bottom:2px">คำขอ #GCL-2025-00142</div>
      <div style="font-size:16px;font-weight:700;color:var(--navy)">นายสมศักดิ์ ใจดี — เจาะน้ำบาดาลใหม่</div>
    </div>
    <div style="margin-left:auto">
      <span class="status-badge status-pending">รอลงพื้นที่</span>
    </div>
  </div>
  ```

- [ ] **Step 3: สร้างฟอร์มบันทึก — GPS Section**

  ```html
  <!-- Section: พิกัด GPS -->
  <div class="form-section">
    <div class="section-title">📍 พิกัด GPS</div>
    <div style="background:var(--navy-xlt);border-radius:12px;border:1px solid var(--border-mid);overflow:hidden;margin-bottom:12px">
      <!-- Map placeholder -->
      <div id="map" style="height:220px;background:#D1D5DB;display:flex;align-items:center;justify-content:center;color:#9CA3AF;font-size:14px">
        [แผนที่ — ต้องการ Leaflet.js สำหรับ production]
      </div>
    </div>
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:12px">
      <div class="field-group">
        <label>Latitude</label>
        <input type="number" id="lat" step="0.000001" placeholder="18.796143" value="18.796143">
      </div>
      <div class="field-group">
        <label>Longitude</label>
        <input type="number" id="lng" step="0.000001" placeholder="98.979263" value="98.979263">
      </div>
    </div>
    <button class="btn btn-outline btn-sm" onclick="detectLocation()">📡 ใช้ตำแหน่งปัจจุบัน</button>
  </div>
  ```

  JS:
  ```html
  <script>
    function detectLocation() {
      if (!navigator.geolocation) { alert('เบราว์เซอร์ไม่รองรับ GPS'); return; }
      navigator.geolocation.getCurrentPosition(pos => {
        document.getElementById('lat').value = pos.coords.latitude.toFixed(6);
        document.getElementById('lng').value = pos.coords.longitude.toFixed(6);
      }, () => alert('ไม่สามารถดึงพิกัดได้'));
    }
  </script>
  ```

- [ ] **Step 4: สร้างฟอร์ม — รูปถ่าย Section**

  ```html
  <!-- Section: รูปถ่าย -->
  <div class="form-section">
    <div class="section-title">📷 รูปถ่ายสถานที่</div>
    <div id="photo-grid" style="display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-bottom:12px">
      <!-- Placeholder photos (mockup) -->
      <div class="photo-thumb">🏗️</div>
      <div class="photo-thumb">🌿</div>
      <div class="photo-add" onclick="document.getElementById('file-input').click()">+ เพิ่มรูป</div>
    </div>
    <input type="file" id="file-input" accept="image/*" multiple style="display:none">
    <p style="font-size:11px;color:var(--text-3)">ต้องการอย่างน้อย 1 รูป — รองรับ JPG, PNG</p>
  </div>
  ```

  CSS:
  ```css
  .photo-thumb{aspect-ratio:1;border-radius:10px;background:var(--surface);border:1px solid var(--border);
               display:flex;align-items:center;justify-content:center;font-size:32px}
  .photo-add{aspect-ratio:1;border-radius:10px;border:2px dashed var(--border-mid);
             display:flex;align-items:center;justify-content:center;font-size:13px;
             color:var(--text-3);cursor:pointer;transition:all .2s}
  .photo-add:hover{border-color:var(--navy-mid);color:var(--navy-mid);background:var(--navy-xlt)}
  ```

- [ ] **Step 5: สร้างฟอร์ม — ผลการตรวจ Section**

  ```html
  <!-- Section: ผลการตรวจ -->
  <div class="form-section">
    <div class="section-title">📋 ผลการตรวจสอบ</div>
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:16px">

      <label class="result-option" for="result-pass">
        <input type="radio" name="result" id="result-pass" value="pass" style="display:none">
        <div class="result-icon">✅</div>
        <div class="result-label">ผ่าน</div>
        <div class="result-desc">สถานที่เหมาะสม สามารถดำเนินการได้</div>
      </label>

      <label class="result-option" for="result-fail">
        <input type="radio" name="result" id="result-fail" value="fail" style="display:none">
        <div class="result-icon">❌</div>
        <div class="result-label">ไม่ผ่าน</div>
        <div class="result-desc">สถานที่ไม่เหมาะสมหรือมีข้อขัดแย้ง</div>
      </label>
    </div>

    <div class="field-group">
      <label>หมายเหตุ <span id="note-required" style="color:var(--err);display:none">*</span></label>
      <textarea id="note" rows="4" placeholder="รายละเอียดเพิ่มเติมจากการตรวจสอบ..."></textarea>
    </div>
  </div>

  <!-- Submit -->
  <div style="padding:20px 40px;border-top:1px solid var(--border);background:#fff;display:flex;justify-content:flex-end;gap:12px">
    <a href="01_หน้าหลัก.html" class="btn btn-outline">ยกเลิก</a>
    <button class="btn btn-teal" onclick="submitResult()">✓ ส่งผลการตรวจ</button>
  </div>
  ```

  CSS:
  ```css
  .form-section{padding:24px 40px;border-bottom:1px solid var(--border)}
  .section-title{font-size:14px;font-weight:700;color:var(--navy);margin-bottom:16px}
  .result-option{display:flex;flex-direction:column;align-items:center;text-align:center;
                 padding:20px;border-radius:14px;border:2px solid var(--border-mid);
                 background:#fff;cursor:pointer;transition:all .2s}
  .result-option:hover{border-color:var(--navy-mid);background:var(--navy-xlt)}
  .result-option:has(input:checked){border-color:var(--navy);background:var(--navy-lt)}
  .result-icon{font-size:32px;margin-bottom:8px}
  .result-label{font-size:15px;font-weight:700;color:var(--navy);margin-bottom:4px}
  .result-desc{font-size:11px;color:var(--text-3);line-height:1.5}
  ```

  JS submit:
  ```html
  <script>
    document.querySelectorAll('input[name="result"]').forEach(r => {
      r.addEventListener('change', () => {
        document.getElementById('note-required').style.display = r.value==='fail' ? 'inline' : 'none';
      });
    });
    function submitResult() {
      const result = document.querySelector('input[name="result"]:checked');
      if (!result) { alert('กรุณาเลือกผลการตรวจสอบ'); return; }
      if (result.value==='fail' && !document.getElementById('note').value.trim()) {
        alert('กรุณากรอกหมายเหตุเมื่อผลไม่ผ่าน'); return;
      }
      window.location.href = '03_รายละเอียดผลที่ส่งแล้ว.html';
    }
  </script>
  ```

- [ ] **Step 6: เปิดในเบราว์เซอร์ ตรวจสอบ**
  - ปุ่ม "ใช้ตำแหน่งปัจจุบัน" ขอ permission GPS
  - photo-add box คลิกได้ (เปิด file picker)
  - เลือก "ไม่ผ่าน" → asterisk แสดงที่ label หมายเหตุ
  - Submit โดยไม่เลือกผล → alert แสดง
  - Submit ครบ → navigate ไปหน้า 03

---

## Task 8: เจ้าหน้าที่ภาคสนาม — รายละเอียดผลที่ส่งแล้ว

**Files:**
- Create: `html-screens/05_เจ้าหน้าที่ภาคสนาม/03_รายละเอียดผลที่ส่งแล้ว.html`

- [ ] **Step 1: สร้าง readonly view ของผลที่ submit แล้ว**

  Copy App shell จาก Task 6, แล้วสร้าง content:

  ```html
  <!-- Result summary (readonly) -->
  <main style="flex:1;overflow-y:auto;padding:32px 40px">

    <!-- Success banner -->
    <div style="background:var(--ok-lt);border:1px solid var(--ok-mid);border-radius:14px;padding:16px 20px;display:flex;align-items:center;gap:12px;margin-bottom:24px">
      <div style="font-size:24px">✅</div>
      <div>
        <div style="font-weight:700;color:var(--ok)">ส่งผลการตรวจแล้ว</div>
        <div style="font-size:12px;color:var(--ok);opacity:.8">ส่งเมื่อ 05 พ.ค. 2568 เวลา 14:32 น.</div>
      </div>
    </div>

    <!-- Info grid -->
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:20px;margin-bottom:24px">

      <!-- พิกัด -->
      <div style="background:#fff;border-radius:14px;border:1px solid var(--border);padding:20px">
        <div class="section-title">📍 พิกัด GPS</div>
        <div style="font-size:14px;font-weight:600;color:var(--navy);margin-bottom:4px">18.796143, 98.979263</div>
        <div style="background:#E5E7EB;border-radius:8px;height:120px;display:flex;align-items:center;justify-content:center;color:#9CA3AF;font-size:12px">[แผนที่]</div>
      </div>

      <!-- ผลการตรวจ -->
      <div style="background:#fff;border-radius:14px;border:1px solid var(--border);padding:20px">
        <div class="section-title">📋 ผลการตรวจ</div>
        <div style="display:flex;align-items:center;gap:10px;margin-bottom:12px">
          <span style="font-size:28px">✅</span>
          <span style="font-size:18px;font-weight:700;color:var(--ok)">ผ่าน</span>
        </div>
        <div style="font-size:13px;color:var(--text-2);line-height:1.6">พื้นที่เหมาะสม ไม่พบสิ่งกีดขวาง ระยะห่างจากแหล่งน้ำผิวดินเป็นไปตามเกณฑ์</div>
      </div>
    </div>

    <!-- รูปถ่าย -->
    <div style="background:#fff;border-radius:14px;border:1px solid var(--border);padding:20px;margin-bottom:24px">
      <div class="section-title">📷 รูปถ่ายสถานที่</div>
      <div style="display:grid;grid-template-columns:repeat(4,1fr);gap:10px">
        <div style="aspect-ratio:1;border-radius:10px;background:var(--surface);display:flex;align-items:center;justify-content:center;font-size:32px">🏗️</div>
        <div style="aspect-ratio:1;border-radius:10px;background:var(--surface);display:flex;align-items:center;justify-content:center;font-size:32px">🌿</div>
      </div>
    </div>

    <a href="01_หน้าหลัก.html" class="btn btn-outline">← กลับหน้าหลัก</a>
  </main>
  ```

- [ ] **Step 2: เปิดในเบราว์เซอร์ ตรวจสอบ**
  - Success banner สีเขียวแสดงด้านบน
  - Grid 2 columns แสดง GPS + ผลการตรวจ
  - รูปภาพ grid แสดง
  - ปุ่มกลับหน้าหลักทำงาน

---

## Task 9: เพิ่ม Tab "ผลการตรวจสถานที่" ในหน้าเจ้าหน้าที่

**Files:**
- Modify: `html-screens/03_เจ้าหน้าที่/03_ตรวจสอบคำขอ_แท็บเอกสาร.html`
- Create: `html-screens/03_เจ้าหน้าที่/06_ตรวจสอบคำขอ_แท็บผลตรวจสถานที่.html`

- [ ] **Step 1: หา tab nav ใน 03_ตรวจสอบคำขอ_แท็บเอกสาร.html**

  ค้นหา `.review-tabs` หรือ `.rtab` div ในไฟล์ จะเห็น tabs: เอกสาร / ฟอร์ม / แผนที่

- [ ] **Step 2: เพิ่ม Tab ที่ 4 ใน tab nav**

  เพิ่ม tab button หลัง tab แผนที่:
  ```html
  <a href="06_ตรวจสอบคำขอ_แท็บผลตรวจสถานที่.html" class="rtab">
    🏗️ ผลตรวจสถานที่
  </a>
  ```
  ทำซ้ำใน `04_ตรวจสอบคำขอ_แท็บฟอร์ม.html` และ `05_ตรวจสอบคำขอ_แท็บแผนที่.html` (ให้ครบทั้ง 3 ไฟล์)

- [ ] **Step 3: สร้างหน้า Tab ผลตรวจสถานที่ (`06_ตรวจสอบคำขอ_แท็บผลตรวจสถานที่.html`)**

  Copy structure จาก `05_ตรวจสอบคำขอ_แท็บแผนที่.html` (layout เหมือนกัน ต่างแค่ content) แล้วเพิ่ม content:

  ```html
  <!-- Tab content: ผลการตรวจสถานที่ -->
  <div class="rtab-body active" style="padding:20px">

    <!-- Status banner -->
    <div style="background:var(--ok-lt);border:1px solid var(--ok-mid);border-radius:12px;padding:14px 18px;display:flex;align-items:center;gap:12px;margin-bottom:20px">
      <span style="font-size:20px">✅</span>
      <div>
        <div style="font-weight:700;color:var(--ok);font-size:14px">ตรวจสถานที่แล้ว</div>
        <div style="font-size:12px;color:var(--ok)">โดย นายสมชาย ภาคสนาม — 05 พ.ค. 2568 เวลา 14:32 น.</div>
      </div>
    </div>

    <!-- Info grid -->
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:16px;margin-bottom:16px">
      <div style="background:var(--surface);border-radius:12px;padding:16px">
        <div style="font-size:11px;color:var(--text-3);font-weight:700;text-transform:uppercase;letter-spacing:.06em;margin-bottom:8px">พิกัด GPS</div>
        <div style="font-size:14px;font-weight:600;color:var(--navy)">18.796143, 98.979263</div>
      </div>
      <div style="background:var(--surface);border-radius:12px;padding:16px">
        <div style="font-size:11px;color:var(--text-3);font-weight:700;text-transform:uppercase;letter-spacing:.06em;margin-bottom:8px">ผลการตรวจ</div>
        <div style="font-size:14px;font-weight:700;color:var(--ok)">✅ ผ่าน</div>
      </div>
    </div>

    <div style="background:var(--surface);border-radius:12px;padding:16px;margin-bottom:16px">
      <div style="font-size:11px;color:var(--text-3);font-weight:700;text-transform:uppercase;letter-spacing:.06em;margin-bottom:8px">หมายเหตุ</div>
      <div style="font-size:13px;color:var(--text-2);line-height:1.6">พื้นที่เหมาะสม ไม่พบสิ่งกีดขวาง ระยะห่างจากแหล่งน้ำผิวดินเป็นไปตามเกณฑ์</div>
    </div>

    <!-- รูปถ่าย -->
    <div style="font-size:11px;color:var(--text-3);font-weight:700;text-transform:uppercase;letter-spacing:.06em;margin-bottom:10px">รูปถ่าย (2)</div>
    <div style="display:grid;grid-template-columns:repeat(4,1fr);gap:10px">
      <div style="aspect-ratio:1;border-radius:10px;background:#E5E7EB;display:flex;align-items:center;justify-content:center;font-size:28px">🏗️</div>
      <div style="aspect-ratio:1;border-radius:10px;background:#E5E7EB;display:flex;align-items:center;justify-content:center;font-size:28px">🌿</div>
    </div>
  </div>
  ```

- [ ] **Step 4: เปิดทั้ง 4 ไฟล์ ตรวจสอบ**
  - Tab ที่ 4 "ผลตรวจสถานที่" แสดงใน nav bar ของทุก tab
  - Tab ที่ 4 เมื่อคลิก → navigate ไปหน้า 06
  - Content แสดง GPS, ผล, หมายเหตุ, รูปถ่าย

---

## ═══════════════════════════════
## PHASE 2
## ═══════════════════════════════

---

## Task 10: AI Recommendation Card

**Files:**
- Create: `html-screens/03_เจ้าหน้าที่/12_ตรวจสอบคำขอ_AI_Recommendation.html`

  *(เลข 12 ต่อจาก 11_Invoice ที่มีอยู่แล้ว)*

- [ ] **Step 1: Copy หน้า `03_ตรวจสอบคำขอ_แท็บเอกสาร.html` เป็น base แล้วเพิ่ม AI Card**

  AI Card วางด้านบนของ content area ก่อน tab navigation:

  ```html
  <!-- AI Recommendation Card -->
  <div class="ai-card" style="margin:16px 20px 0">
    <div class="ai-card-header">
      <div style="display:flex;align-items:center;gap:10px">
        <div style="width:32px;height:32px;border-radius:8px;background:rgba(255,255,255,.2);
                    display:flex;align-items:center;justify-content:center;font-size:18px">🤖</div>
        <div>
          <div style="font-size:13px;font-weight:700;color:#fff">ผลวิเคราะห์ AI</div>
          <div style="font-size:11px;color:rgba(255,255,255,.6)">ประมวลผลข้อมูลช่วยตัดสินใจ</div>
        </div>
      </div>
      <div class="ai-result-badge ai-approve">
        ✅ แนะนำอนุมัติ
        <span style="font-size:10px;opacity:.8;margin-left:6px">ความเชื่อมั่น 87%</span>
      </div>
    </div>

    <div class="ai-card-body">
      <div style="font-size:12px;font-weight:700;color:var(--text-2);margin-bottom:8px;">เหตุผลประกอบ</div>
      <ul style="list-style:none;display:flex;flex-direction:column;gap:6px">
        <li class="ai-reason ai-ok">✓ ชั้นน้ำบาดาลในพื้นที่มีปริมาณน้ำเพียงพอ (ระดับ 3/5)</li>
        <li class="ai-reason ai-ok">✓ ไม่พบบ่อเจาะอื่นในรัศมี 200 เมตร</li>
        <li class="ai-reason ai-ok">✓ เอกสารครบถ้วนตามระเบียบ</li>
        <li class="ai-reason ai-warn">⚠ พื้นที่อยู่ในโซนเฝ้าระวัง — ควรกำหนดปริมาณการใช้น้ำสูงสุด</li>
      </ul>
      <div style="margin-top:12px;padding:10px 14px;background:rgba(27,58,107,.06);border-radius:8px;font-size:11px;color:var(--text-3);line-height:1.6">
        ⚠️ ผลนี้เป็นข้อมูลประกอบการตัดสินใจเท่านั้น เจ้าหน้าที่มีอำนาจในการตัดสินใจขั้นสุดท้าย
      </div>
    </div>
  </div>
  ```

  CSS:
  ```css
  .ai-card{border-radius:14px;overflow:hidden;border:1px solid rgba(27,58,107,.15);box-shadow:var(--shadow-md)}
  .ai-card-header{background:linear-gradient(135deg,var(--navy-dark),var(--navy-mid));
                  padding:14px 20px;display:flex;align-items:center;justify-content:space-between}
  .ai-result-badge{padding:6px 14px;border-radius:20px;font-size:13px;font-weight:700}
  .ai-approve{background:rgba(22,163,74,.25);color:#86EFAC}
  .ai-reject{background:rgba(220,38,38,.25);color:#FCA5A5}
  .ai-card-body{background:#fff;padding:16px 20px}
  .ai-reason{font-size:12px;padding:6px 10px;border-radius:8px;line-height:1.5}
  .ai-ok{background:var(--ok-lt);color:var(--ok)}
  .ai-warn{background:var(--warn-lt);color:var(--warn)}
  ```

- [ ] **Step 2: เพิ่ม Loading state และ Error state สำหรับ future integration**

  เพิ่ม hidden divs ที่ switch ได้ด้วย JS:
  ```html
  <!-- Loading state (hidden by default) -->
  <div id="ai-loading" style="display:none;padding:20px;text-align:center;background:#fff;border-radius:14px;border:1px solid var(--border)">
    <div style="font-size:13px;color:var(--text-3)">🤖 กำลังประมวลผลข้อมูล...</div>
  </div>

  <!-- Error state (hidden by default) -->
  <div id="ai-error" style="display:none;padding:14px 20px;background:var(--warn-lt);border-radius:14px;border:1px solid var(--warn-mid);font-size:13px;color:var(--warn)">
    ⚠️ ไม่สามารถโหลดผลวิเคราะห์ได้ในขณะนี้ — เจ้าหน้าที่ยังสามารถตัดสินใจได้ตามปกติ
  </div>
  ```

- [ ] **Step 3: เปิดในเบราว์เซอร์ ตรวจสอบ**
  - AI Card แสดงด้านบน content area
  - สี gradient header navy ชัดเจน
  - Badge "แนะนำอนุมัติ" สีเขียว
  - Reason list แสดง ok และ warn items ต่างสีกัน
  - Disclaimer แสดงด้านล่าง

---

## Task 11: Modal สร้างใบขวาง

**Files:**
- Modify: `html-screens/03_เจ้าหน้าที่/06_ผลการตัดสิน_อนุมัติ.html`
- Create: `html-screens/03_เจ้าหน้าที่/13_Modal_สร้างใบขวาง.html`

- [ ] **Step 1: เพิ่มปุ่ม "สร้างใบขวาง" ใน `06_ผลการตัดสิน_อนุมัติ.html`**

  หา action buttons ในหน้านั้น แล้วเพิ่มปุ่ม:
  ```html
  <button class="btn btn-outline" onclick="window.location.href='13_Modal_สร้างใบขวาง.html'">
    📄 สร้างใบขวาง
  </button>
  ```

- [ ] **Step 2: สร้าง Modal page `13_Modal_สร้างใบขวาง.html`**

  Copy modal shell จาก `09_Modal_คณะกรรมการ.html` เป็น base แล้วปรับ content:

  ```html
  <!-- Modal content -->
  <div class="modal-box" style="width:680px;max-height:85vh;overflow-y:auto">

    <div class="modal-header">
      <div>
        <div style="font-size:18px;font-weight:700;color:var(--navy)">สร้างใบขวาง</div>
        <div style="font-size:12px;color:var(--text-3)">เอกสารสำหรับนำเสนอต่อคณะกรรมการ</div>
      </div>
      <a href="06_ผลการตัดสิน_อนุมัติ.html" class="modal-close">✕</a>
    </div>

    <!-- Preview area -->
    <div style="background:var(--surface);border-radius:12px;padding:24px;margin:20px;border:1px solid var(--border)">
      <div style="text-align:center;margin-bottom:20px">
        <div style="font-family:'Noto Serif Thai',sans-serif;font-size:16px;font-weight:700;color:var(--navy)">ใบขวาง</div>
        <div style="font-size:12px;color:var(--text-3)">คำขอรับใบอนุญาตเจาะน้ำบาดาล / ใช้น้ำบาดาล</div>
      </div>

      <!-- Auto-filled fields (editable) -->
      <div style="display:grid;gap:14px">

        <div class="field-group">
          <label>เรื่อง</label>
          <input type="text" value="ขออนุมัติคำขอเจาะน้ำบาดาลใหม่ กรณีบุคคลธรรมดา">
        </div>

        <div style="display:grid;grid-template-columns:1fr 1fr;gap:14px">
          <div class="field-group">
            <label>ชื่อผู้ยื่นคำขอ</label>
            <input type="text" value="นายสมศักดิ์ ใจดี">
          </div>
          <div class="field-group">
            <label>เลขที่คำขอ</label>
            <input type="text" value="GCL-2025-00142">
          </div>
        </div>

        <div class="field-group">
          <label>สถานที่ตั้ง</label>
          <input type="text" value="12 ถ.พหลโยธิน อ.เมือง จ.เชียงใหม่">
        </div>

        <div style="display:grid;grid-template-columns:1fr 1fr;gap:14px">
          <div class="field-group">
            <label>ผลการตรวจสถานที่</label>
            <input type="text" value="ผ่าน (05 พ.ค. 2568)" readonly style="background:#F8F9FA">
          </div>
          <div class="field-group">
            <label>ผล AI วิเคราะห์</label>
            <input type="text" value="แนะนำอนุมัติ (87%)" readonly style="background:#F8F9FA">
          </div>
        </div>

        <div class="field-group">
          <label>ความเห็นของเจ้าหน้าที่</label>
          <textarea rows="3" placeholder="กรอกความเห็น/ข้อเสนอแนะเพิ่มเติม..."></textarea>
        </div>

        <div class="field-group">
          <label>วันที่นำเสนอต่อคณะกรรมการ</label>
          <input type="date">
        </div>
      </div>
    </div>

    <!-- Actions -->
    <div style="display:flex;justify-content:flex-end;gap:12px;padding:0 20px 20px">
      <a href="06_ผลการตัดสิน_อนุมัติ.html" class="btn btn-outline">ยกเลิก</a>
      <button class="btn btn-outline" onclick="alert('Preview PDF')">👁 Preview PDF</button>
      <button class="btn btn-teal" onclick="alert('สร้างใบขวางสำเร็จ')">📄 Generate PDF</button>
    </div>
  </div>
  ```

- [ ] **Step 3: เปิดในเบราว์เซอร์ ตรวจสอบ**
  - Fields ถูก auto-fill ด้วยข้อมูล mockup
  - Fields readonly (ผลตรวจ, AI) มีสี background ต่าง
  - ปุ่ม Preview และ Generate PDF แสดง alert (mockup behavior)
  - ปุ่ม close (✕) และยกเลิก → กลับหน้า ผลการตัดสิน

---

## Self-Review Checklist

### Spec Coverage
- [x] Registration screens: Tasks 1-5 ครอบคลุม spec Section 1
- [x] Field Officer portal: Tasks 6-8 ครอบคลุม spec Section 2
- [x] Officer tab เพิ่ม: Task 9 ครอบคลุม "ผลกระทบต่อเจ้าหน้าที่หลัก"
- [x] AI Card: Task 10 ครอบคลุม spec Section 3
- [x] ใบขวาง Modal: Task 11 ครอบคลุม spec Section 4
- [x] Login ปุ่มสมัคร: Task 1 step 2

### File Numbering
- `05_เจ้าหน้าที่ภาคสนาม/` — folder ใหม่, เริ่ม 01-03
- `01_ทั่วไป/` — ต่อจาก 02 ที่มีอยู่ → 03-07
- `03_เจ้าหน้าที่/` — ต่อจาก 11 → ใช้ 12-13

### CSS Class Consistency
- ทุก Task ใช้ `.btn`, `.btn-primary`, `.btn-outline`, `.btn-teal`, `.btn-sm` ตาม convention เดียวกัน
- `.field-group`, `.form-section` ใช้สม่ำเสมอ
- `:root` variables อ้างอิงทุก property

### Known Out-of-Scope
- Portal คณะกรรมการ — TBD
- Backend/API integration
- Mobile App
