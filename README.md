<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ID CARD & LIVE PDF TEXT EDITOR PORTAL</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700;800&display=swap" rel="stylesheet">
  
  <!-- PDF.js Standalone Build -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.min.js"></script>
  
  <!-- jsPDF Library -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

  <!-- Cropper.js -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/cropperjs/1.5.13/cropper.min.css"/>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/cropperjs/1.5.13/cropper.min.js"></script>

  <!-- Fabric.js -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/fabric.js/5.3.1/fabric.min.js"></script>

  <style>
    :root {
      --bg-gradient: linear-gradient(135deg, #0f172a 0%, #1e1b4b 50%, #0f172a 100%);
      --card-bg: rgba(30, 41, 59, 0.85);
      --accent-blue: #38bdf8;
      --accent-purple: #818cf8;
      --btn-add: linear-gradient(135deg, #f59e0b 0%, #d97706 100%);
      --btn-download: linear-gradient(135deg, #10b981 0%, #059669 100%);
      --text-main: #f8fafc;
      --text-muted: #94a3b8;
      --border-color: rgba(255, 255, 255, 0.1);
    }

    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Poppins', sans-serif; }
    
    body { 
      background: var(--bg-gradient); 
      min-height: 100vh;
      padding: 20px 10px; 
      display: flex; 
      flex-direction: column; 
      align-items: center; 
      justify-content: center;
      color: var(--text-main);
    }

    .portal-main-heading {
      font-size: 24px;
      font-weight: 800;
      letter-spacing: 1.5px;
      text-transform: uppercase;
      background: linear-gradient(135deg, #38bdf8 0%, #a855f7 50%, #f43f5e 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      margin-bottom: 20px;
      text-align: center;
    }

    .auth-box {
      background: var(--card-bg);
      backdrop-filter: blur(20px);
      border: 1px solid var(--border-color);
      padding: 35px 30px;
      border-radius: 20px;
      box-shadow: 0 25px 60px rgba(0, 0, 0, 0.6);
      width: 100%;
      max-width: 400px;
      text-align: center;
    }

    .badge {
      display: inline-block;
      padding: 4px 14px;
      font-size: 11px;
      font-weight: 600;
      letter-spacing: 1px;
      text-transform: uppercase;
      background: rgba(56, 189, 248, 0.15);
      color: var(--accent-blue);
      border: 1px solid rgba(56, 189, 248, 0.3);
      border-radius: 20px;
      margin-bottom: 12px;
    }

    .slot-counter-badge {
      background: rgba(245, 158, 11, 0.15);
      color: #fbbf24;
      border: 1px solid rgba(245, 158, 11, 0.3);
      padding: 4px 16px;
      font-size: 12px;
      font-weight: 600;
      border-radius: 20px;
      display: inline-block;
      margin-bottom: 15px;
    }

    .login-input {
      width: 100%;
      padding: 13px 16px;
      margin-bottom: 15px;
      background: rgba(15, 23, 42, 0.9);
      border: 1px solid rgba(56, 189, 248, 0.3);
      border-radius: 10px;
      color: #fff;
      font-size: 14px;
      outline: none;
    }

    .login-btn {
      width: 100%;
      padding: 13px;
      background: linear-gradient(135deg, #0284c7 0%, #2563eb 100%);
      color: #fff;
      font-weight: 600;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      font-size: 15px;
      transition: 0.3s;
    }

    .auth-link {
      display: inline-block;
      margin-top: 15px;
      font-size: 13px;
      color: var(--accent-blue);
      cursor: pointer;
      text-decoration: underline;
    }

    .error-msg {
      color: #ef4444;
      font-size: 13px;
      margin-top: 12px;
      display: none;
    }

    .tab-nav {
      display: flex;
      justify-content: center;
      gap: 10px;
      margin-bottom: 20px;
      flex-wrap: wrap;
    }

    .tab-btn {
      padding: 10px 18px;
      background: rgba(15, 23, 42, 0.8);
      border: 1px solid var(--border-color);
      color: var(--text-muted);
      border-radius: 12px;
      cursor: pointer;
      font-weight: 600;
      font-size: 13px;
      transition: 0.3s;
    }

    .tab-btn.active {
      background: linear-gradient(135deg, #0284c7 0%, #2563eb 100%);
      color: #fff;
      border-color: transparent;
      box-shadow: 0 4px 15px rgba(37, 99, 235, 0.4);
    }

    #mainApp {
      display: none;
      width: 100%;
      max-width: 1150px;
    }

    .container { 
      background: var(--card-bg); 
      backdrop-filter: blur(16px);
      border: 1px solid var(--border-color);
      padding: 30px 20px; 
      border-radius: 20px; 
      box-shadow: 0 20px 50px rgba(0, 0, 0, 0.4); 
      width: 100%; 
      text-align: center; 
      position: relative; 
    }

    .logout-btn {
      position: absolute;
      top: 15px;
      right: 15px;
      background: rgba(239, 68, 68, 0.2);
      border: 1px solid rgba(239, 68, 68, 0.4);
      color: #fca5a5;
      padding: 6px 14px;
      font-size: 12px;
      border-radius: 8px;
      cursor: pointer;
    }

    h1 { 
      background: linear-gradient(to right, #38bdf8, #a855f7, #ec4899);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      font-size: 24px; 
      font-weight: 700;
      margin-bottom: 8px; 
    }

    .tab-content { display: none; }
    .tab-content.active { display: block; }

    .upload-section { 
      display: flex; 
      gap: 15px; 
      justify-content: center; 
      margin: 20px 0; 
      flex-wrap: wrap; 
    }

    .upload-box { 
      border: 2px dashed rgba(56, 189, 248, 0.4); 
      padding: 18px 15px; 
      border-radius: 14px; 
      cursor: pointer; 
      background: rgba(15, 23, 42, 0.6); 
      flex: 1; 
      min-width: 240px; 
      transition: 0.3s;
    }

    .upload-box:hover { 
      border-color: var(--accent-blue);
      background: rgba(56, 189, 248, 0.08);
    }

    input[type="file"] { display: none; }

    .preview-container { 
      display: flex; 
      justify-content: center; 
      gap: 20px; 
      margin: 15px 0; 
      flex-wrap: wrap; 
    }

    .preview-box { 
      border: 1px solid var(--border-color); 
      padding: 10px; 
      background: rgba(15, 23, 42, 0.8); 
      border-radius: 12px; 
    }

    .preview-box h4 { 
      font-size: 12px; 
      color: var(--text-muted); 
      margin-bottom: 6px; 
    }
    
    canvas { 
      max-width: 100%; 
      height: auto; 
      display: block; 
      margin: 0 auto; 
      border-radius: 4px;
      background: #fff; 
    }

    .btn-group { 
      display: flex; 
      gap: 12px; 
      justify-content: center; 
      margin-top: 20px; 
      flex-wrap: wrap; 
    }

    .action-btn { 
      padding: 12px 24px; 
      font-size: 13px; 
      font-weight: 600; 
      border: none; 
      border-radius: 10px; 
      cursor: pointer; 
      transition: all 0.3s ease; 
      color: #fff;
    }

    .action-btn:hover:not(:disabled) {
      transform: translateY(-2px);
      box-shadow: 0 6px 20px rgba(0,0,0,0.4);
    }

    .btn-add { background: var(--btn-add); }
    .btn-download { background: var(--btn-download); }
    .btn-reset { background: rgba(239, 68, 68, 0.2); border: 1px solid rgba(239, 68, 68, 0.4); color: #fca5a5; }

    .action-btn:disabled { 
      background: #334155; 
      color: #64748b; 
      cursor: not-allowed; 
    }

    .control-panel {
      background: rgba(15, 23, 42, 0.7);
      border: 1px solid var(--border-color);
      border-radius: 14px;
      padding: 18px 20px;
      max-width: 600px;
      margin: 20px auto;
      text-align: center;
    }

    .qty-select-group {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 10px;
      margin-top: 10px;
      flex-wrap: wrap;
    }

    .qty-input {
      width: 80px;
      padding: 8px 12px;
      border-radius: 8px;
      background: rgba(15, 23, 42, 0.9);
      border: 1px solid var(--accent-blue);
      color: #fff;
      font-size: 16px;
      font-weight: 700;
      text-align: center;
      outline: none;
    }

    .quick-qty-btn {
      padding: 6px 14px;
      background: #334155;
      border: 1px solid rgba(255, 255, 255, 0.1);
      color: #fff;
      border-radius: 6px;
      font-size: 12px;
      cursor: pointer;
      font-weight: 600;
    }

    .quick-qty-btn:hover { background: #475569; }

    /* PDF Live Editor Specific Styles */
    .editor-wrapper {
      margin: 15px auto;
      border: 2px solid #38bdf8;
      border-radius: 8px;
      background: #ffffff;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
      display: inline-block;
      overflow: auto;
      max-width: 100%;
      min-height: 400px;
    }

    .tool-btn {
      padding: 8px 14px;
      font-size: 12px;
      font-weight: 600;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      color: #fff;
      background: #334155;
      transition: 0.2s;
    }
    .tool-btn:hover { background: #475569; }
    .tool-btn.active { background: #0284c7; }

    #cropModal {
      display: none;
      position: fixed;
      top: 0; left: 0; width: 100%; height: 100%;
      background: rgba(0, 0, 0, 0.85);
      z-index: 10000;
      align-items: center;
      justify-content: center;
      flex-direction: column;
      padding: 20px;
    }

    .crop-wrapper {
      max-width: 90vw;
      max-height: 70vh;
      background: #000;
      border-radius: 8px;
      overflow: hidden;
      margin-bottom: 15px;
    }

    .crop-wrapper img {
      max-width: 100%;
      max-height: 70vh;
      display: block;
    }
  </style>
</head>
<body>

<div class="portal-main-heading">
  ID CARD PRINT PORTAL
</div>

<!-- 1. Login Screen -->
<div id="loginScreen" class="auth-box">
  <div class="badge">Protected Access</div>
  <h2 style="font-size: 22px; margin-bottom: 6px;">Sign In</h2>
  <p style="font-size: 12px; color: var(--text-muted); margin-bottom: 20px;">Card & Photo Generator Portal</p>

  <input type="email" id="loginEmail" class="login-input" placeholder="ईमेल आईडी दर्ज करें" value="oneplus777000@gmail.com">
  <input type="password" id="loginPass" class="login-input" placeholder="पासवर्ड दर्ज करें">
  <button id="authBtn" class="login-btn">लॉगिन करें</button>
  <div id="errorMsg" class="error-msg">⚠️ गलत ईमेल आईडी या पासवर्ड!</div>
  
  <div>
    <span id="goToChangePwd" class="auth-link">🔑 Change Password?</span>
  </div>
</div>

<!-- 2. Change Password Screen -->
<div id="changePwdScreen" class="auth-box" style="display:none;">
  <div class="badge">Security Settings</div>
  <h2 style="font-size: 20px; margin-bottom: 6px; color: var(--accent-blue);">🔑 Change Password</h2>
  <p style="font-size: 12px; color: var(--text-muted); margin-bottom: 20px;">पुराने पासवर्ड का उपयोग करके नया पासवर्ड सेट करें</p>

  <input type="password" id="oldPassInput" class="login-input" placeholder="पुराना पासवर्ड">
  <input type="password" id="newPassInput" class="login-input" placeholder="नया पासवर्ड">
  <input type="password" id="confirmPassInput" class="login-input" placeholder="नया पासवर्ड कन्फर्म करें">
  
  <button id="saveNewPwdBtn" class="login-btn" style="background: var(--btn-download);">💾 नया पासवर्ड सेव करें</button>
  <div id="pwdStatusMsg" style="font-size:13px; margin-top:12px; display:none; font-weight:500;"></div>

  <div>
    <span id="backToLogin" class="auth-link">⬅️ Back to Login</span>
  </div>
</div>

<!-- 3. Main Portal Application -->
<div id="mainApp">
  <div class="tab-nav">
    <button class="tab-btn active" onclick="switchTab('tab-cards')">💳 ID Card (5 Slots)</button>
    <button class="tab-btn" onclick="switchTab('tab-passport')">👤 Passport Photos</button>
    <button class="tab-btn" onclick="switchTab('tab-4x6')">🖼️ 4×6 Photo Print</button>
    <button class="tab-btn" onclick="switchTab('tab-live-pdf')">🛠️ Live PDF Text Editor</button>
  </div>

  <div class="container">
    <button id="logoutBtn" class="logout-btn">🔒 Logout</button>

    <!-- TAB 1: 5 CARDS SYSTEM -->
    <div id="tab-cards" class="tab-content active">
      <div class="badge">2.5mm Gap • Broad Black Border • 5 Cards</div>
      <h1>Card Generator System</h1>
      <p style="font-size: 12px; color: var(--text-muted); margin-bottom: 10px;">एक-एक करके कार्ड्स जोड़ें। दोनों कार्ड के बीच 2.5 mm गैप और Broad Black Border आएगी।</p>
      
      <div id="slotCounter" class="slot-counter-badge">Cards on Page: 0 / 5 (Next Slot: #1)</div>

      <div class="upload-section">
        <label class="upload-box" for="card1Input">
          <strong style="display:block; font-size:14px; margin-bottom:4px;">📁 Front Side</strong>
          <div id="file1Name" style="font-size: 12px; color: var(--text-muted);">फ़ोटो चुनें व क्रॉप करें</div>
        </label>
        <input type="file" id="card1Input" accept="image/*">

        <label class="upload-box" for="card2Input">
          <strong style="display:block; font-size:14px; margin-bottom:4px;">📁 Back Side</strong>
          <div id="file2Name" style="font-size: 12px; color: var(--text-muted);">फ़ोटो चुनें व क्रॉप करें</div>
        </label>
        <input type="file" id="card2Input" accept="image/*">
      </div>

      <div class="preview-container">
        <div class="preview-box">
          <h4>Front Card</h4>
          <canvas id="canvas1" width="1013" height="638" style="width: 180px;"></canvas>
        </div>
        <div class="preview-box">
          <h4>Back Card</h4>
          <canvas id="canvas2" width="1013" height="638" style="width: 180px;"></canvas>
        </div>
      </div>

      <div class="btn-group">
        <button id="addCardBtn" class="action-btn btn-add" disabled>➕ Add This Card to A4 Sheet</button>
        <button id="resetPageBtn" class="action-btn btn-reset">🔄 Clear A4 Page</button>
      </div>

      <div style="margin-top: 25px; border-top: 1px solid var(--border-color); padding-top: 15px;">
        <h3 style="font-size: 15px; color: var(--accent-blue); margin-bottom: 6px;">A4 Sheet Preview</h3>
        <div style="display:inline-block; max-width: 250px; background:#fff; border-radius:6px; overflow:hidden; border: 1px solid #475569;">
          <canvas id="a4Canvas" width="2480" height="3508" style="width: 100%; display:block;"></canvas>
        </div>
        <div class="btn-group">
          <button id="downloadPdfBtn" class="action-btn btn-download" disabled>📥 Direct A4 PDF Download</button>
        </div>
      </div>
    </div>

    <!-- TAB 2: PASSPORT SIZE PHOTO SYSTEM -->
    <div id="tab-passport" class="tab-content">
      <div class="badge">Standard 35mm × 45mm • Manual Quantity Selection</div>
      <h1>Passport Photo Generator</h1>
      <p style="font-size: 12px; color: var(--text-muted); margin-bottom: 15px;">फ़ोटो अपलोड करें, संख्या (Quantity) चुनें और शीट तैयार करें।</p>

      <div class="upload-section">
        <label class="upload-box" for="passportInput" style="max-width: 380px;">
          <strong style="display:block; font-size:14px; margin-bottom:4px;">📁 Passport Photo Upload</strong>
          <div id="passportFileName" style="font-size: 12px; color: var(--text-muted);">फ़ोटो चुनें व क्रॉप करें</div>
        </label>
        <input type="file" id="passportInput" accept="image/*">
      </div>

      <div class="preview-container">
        <div class="preview-box">
          <h4>Cropped Passport Photo</h4>
          <canvas id="passportCanvas" width="413" height="531" style="width: 140px;"></canvas>
        </div>
      </div>

      <div class="control-panel">
        <span style="font-size: 14px; font-weight:600; color: var(--accent-blue);">🔢 फ़ोटो की संख्या (Quantity) चुनें:</span>
        <div class="qty-select-group">
          <input type="number" id="passportQtyInput" class="qty-input" value="8" min="1" max="30">
          <button class="quick-qty-btn" onclick="setPassportQty(4)">4</button>
          <button class="quick-qty-btn" onclick="setPassportQty(6)">6</button>
          <button class="quick-qty-btn" onclick="setPassportQty(8)">8</button>
          <button class="quick-qty-btn" onclick="setPassportQty(12)">12</button>
          <button class="quick-qty-btn" onclick="setPassportQty(16)">16</button>
          <button class="quick-qty-btn" onclick="setPassportQty(30)">30</button>
        </div>
      </div>

      <div class="btn-group">
        <button id="make4x6CustomPassportBtn" class="action-btn btn-add" disabled>🖼️ Generate on 4×6 Sheet</button>
        <button id="makeA4CustomPassportBtn" class="action-btn btn-add" disabled>📄 Generate on A4 Sheet</button>
      </div>

      <div style="margin-top: 25px; border-top: 1px solid var(--border-color); padding-top: 15px;">
        <h3 id="passportSheetTitle" style="font-size: 15px; color: var(--accent-blue); margin-bottom: 6px;">Passport Sheet Preview</h3>
        <div style="display:inline-block; max-width: 250px; background:#fff; border-radius:6px; overflow:hidden; border: 1px solid #475569;">
          <canvas id="passportSheetCanvas" width="1800" height="1200" style="width: 100%; display:block;"></canvas>
        </div>
        <div class="btn-group">
          <button id="downloadPassportPdfBtn" class="action-btn btn-download" disabled>📥 Download Passport Sheet PDF</button>
        </div>
      </div>
    </div>

    <!-- TAB 3: 4x6 PHOTO PRINT -->
    <div id="tab-4x6" class="tab-content">
      <div class="badge">Clear 300 DPI • 1200 × 1800 px • Max 4 Photos</div>
      <h1>4×6 Photo Print Generator</h1>
      <p style="font-size: 12px; color: var(--text-muted); margin-bottom: 15px;">4×6 इंच फ़ोटो अपलोड करें, 1 से 4 तक संख्या चुनें और A4 या 4×6 शीट PDF निकालें।</p>

      <div class="upload-section">
        <label class="upload-box" for="photo4x6Input" style="max-width: 380px;">
          <strong style="display:block; font-size:14px; margin-bottom:4px;">📁 4×6 Photo Upload</strong>
          <div id="photo4x6FileName" style="font-size: 12px; color: var(--text-muted);">फ़ोटो चुनें व क्रॉप करें</div>
        </label>
        <input type="file" id="photo4x6Input" accept="image/*">
      </div>

      <div class="preview-container">
        <div class="preview-box">
          <h4>Cropped 4×6 Photo Canvas</h4>
          <canvas id="canvas4x6" width="1200" height="1800" style="width: 150px;"></canvas>
        </div>
      </div>

      <div class="control-panel">
        <span style="font-size: 14px; font-weight:600; color: var(--accent-blue);">🔢 A4 शीट पर 4×6 फ़ोटो की संख्या चुनें (Max 4):</span>
        <div class="qty-select-group">
          <input type="number" id="photo4x6QtyInput" class="qty-input" value="2" min="1" max="4">
          <button class="quick-qty-btn" onclick="set4x6Qty(1)">1 Photo</button>
          <button class="quick-qty-btn" onclick="set4x6Qty(2)">2 Photos</button>
          <button class="quick-qty-btn" onclick="set4x6Qty(3)">3 Photos</button>
          <button class="quick-qty-btn" onclick="set4x6Qty(4)">4 Photos</button>
        </div>
      </div>

      <div class="btn-group">
        <button id="downloadDirect4x6Pdf" class="action-btn btn-download" disabled>📥 Direct 1 Photo (4×6 Paper PDF)</button>
        <button id="generateA4Custom4x6Btn" class="action-btn btn-add" disabled>📄 Generate Selected Qty on A4 Sheet</button>
      </div>

      <div style="margin-top: 25px; border-top: 1px solid var(--border-color); padding-top: 15px;">
        <h3 id="photo4x6SheetTitle" style="font-size: 15px; color: var(--accent-blue); margin-bottom: 6px;">A4 4×6 Photo Sheet Preview</h3>
        <div style="display:inline-block; max-width: 250px; background:#fff; border-radius:6px; overflow:hidden; border: 1px solid #475569;">
          <canvas id="a4_4x6_SheetCanvas" width="2480" height="3508" style="width: 100%; display:block;"></canvas>
        </div>
        <div class="btn-group">
          <button id="downloadA4_4x6_PdfBtn" class="action-btn btn-download" disabled>📥 Download A4 4×6 Sheet PDF</button>
        </div>
      </div>
    </div>

    <!-- TAB 4: LIVE PDF TEXT EDITOR (WITH AUTO-TEXT PARSER) -->
    <div id="tab-live-pdf" class="tab-content">
      <div class="badge">Direct Live PDF Editor • Double Click Any Text to Edit</div>
      <h1>Live PDF Document & Text Editor</h1>
      <p style="font-size: 12px; color: var(--text-muted); margin-bottom: 15px;">PDF लोड करें। PDF के अंदर जो भी लिखा है, <strong>उस पर डबल-क्लिक करके सीधे बदलें</strong> या मिटाएँ।</p>

      <div class="upload-section">
        <label class="upload-box" for="pdfDirectInput" style="max-width: 420px;">
          <strong style="display:block; font-size:14px; margin-bottom:4px; color:var(--accent-blue);">📄 Select PDF File to Edit</strong>
          <div id="pdfDirectName" style="font-size: 12px; color: var(--text-muted);">क्लिक करके .pdf फाइल लोड करें</div>
        </label>
        <input type="file" id="pdfDirectInput" accept="application/pdf">
      </div>

      <!-- Controls Panel -->
      <div id="pdfToolPanel" style="display:none; margin-bottom: 15px;">
        <div class="btn-group" style="margin-top:0;">
          <button id="pdfAddTextBtn" class="tool-btn">✍️ Add New Text</button>
          
          <label class="tool-btn" for="pdfStampUpload" style="cursor:pointer; margin-bottom:0;">
            🖼️ Add Stamp / Image
          </label>
          <input type="file" id="pdfStampUpload" accept="image/*">

          <button id="pdfWhiteoutBtn" class="tool-btn">⬜ Whiteout Box</button>
          <button id="pdfDrawBtn" class="tool-btn">✏️ Freehand Sign</button>
          <button id="pdfDeleteBtn" class="tool-btn" style="background: rgba(239, 68, 68, 0.3);">🗑️ Delete</button>
        </div>

        <div id="pdfPageNav" style="display:flex; justify-content:center; align-items:center; gap:10px; margin-top:10px; font-size:13px; color:var(--accent-blue);">
          <button id="pdfPrevPage" class="tool-btn">◀ Prev</button>
          <span>Page: <strong id="pdfCurPageNum">1</strong> / <span id="pdfTotalPageNum">1</span></span>
          <button id="pdfNextPage" class="tool-btn">Next ▶</button>
        </div>
      </div>

      <!-- Live PDF Canvas Viewport -->
      <div id="pdfViewportArea" style="display:none;">
        <div class="editor-wrapper">
          <canvas id="pdfFabricCanvas"></canvas>
        </div>
        <div class="btn-group">
          <button id="downloadLivePdfBtn" class="action-btn btn-download">📥 Download Edited PDF</button>
        </div>
      </div>
    </div>

    <footer style="margin-top: 25px; font-size: 12px; color: var(--text-muted);">
      Designed & Developed by <strong>JAYESH BHAVSAR @ 2026 ALL RIGHTS RESERVED</strong>
    </footer>
  </div>
</div>

<!-- Global Crop Modal -->
<div id="cropModal">
  <div id="cropModalTitle" style="color:#fff; margin-bottom: 10px; font-weight: 600;">कार्ड/फ़ोटो का सही हिस्सा सेलेक्ट (Crop) करें:</div>
  <div class="crop-wrapper">
    <img id="imageToCrop" src="">
  </div>
  <div class="btn-group">
    <button id="cropSaveBtn" class="action-btn btn-download">✂️ Crop & Set</button>
    <button id="cropCancelBtn" class="action-btn" style="background:#ef4444;">रद्द करें</button>
  </div>
</div>

<script>
  const AUTH_EMAIL = "oneplus777000@gmail.com";
  const DEFAULT_PASS = "Pass@123";

  function getStoredPassword() {
    return localStorage.getItem('system_auth_pwd') || DEFAULT_PASS;
  }

  function switchTab(tabId) {
    document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
    document.querySelectorAll('.tab-content').forEach(content => content.classList.remove('active'));
    
    event.target.classList.add('active');
    document.getElementById(tabId).classList.add('active');

    if (tabId === 'tab-live-pdf' && pdfFabric) {
      pdfFabric.renderAll();
    }
  }

  const loginScreen = document.getElementById('loginScreen');
  const changePwdScreen = document.getElementById('changePwdScreen');
  const mainApp = document.getElementById('mainApp');
  
  const loginEmail = document.getElementById('loginEmail');
  const loginPass = document.getElementById('loginPass');
  const authBtn = document.getElementById('authBtn');
  const errorMsg = document.getElementById('errorMsg');
  const logoutBtn = document.getElementById('logoutBtn');

  const goToChangePwd = document.getElementById('goToChangePwd');
  const backToLogin = document.getElementById('backToLogin');
  const oldPassInput = document.getElementById('oldPassInput');
  const newPassInput = document.getElementById('newPassInput');
  const confirmPassInput = document.getElementById('confirmPassInput');
  const saveNewPwdBtn = document.getElementById('saveNewPwdBtn');
  const pwdStatusMsg = document.getElementById('pwdStatusMsg');

  sessionStorage.removeItem('isLoggedIn');

  goToChangePwd.addEventListener('click', () => {
    loginScreen.style.display = 'none';
    changePwdScreen.style.display = 'block';
    oldPassInput.value = '';
    newPassInput.value = '';
    confirmPassInput.value = '';
    pwdStatusMsg.style.display = 'none';
  });

  backToLogin.addEventListener('click', () => {
    changePwdScreen.style.display = 'none';
    loginScreen.style.display = 'block';
    errorMsg.style.display = 'none';
  });

  saveNewPwdBtn.addEventListener('click', () => {
    const oldP = oldPassInput.value.trim();
    const newP = newPassInput.value.trim();
    const confP = confirmPassInput.value.trim();
    const currentActivePass = getStoredPassword().trim();

    if (oldP !== currentActivePass && oldP !== DEFAULT_PASS) {
      pwdStatusMsg.innerText = "❌ पुराना पासवर्ड गलत है!";
      pwdStatusMsg.style.color = "#ef4444";
      pwdStatusMsg.style.display = "block";
      return;
    }

    if (newP.length < 4) {
      pwdStatusMsg.innerText = "❌ नया पासवर्ड कम से कम 4 अक्षरों का होना चाहिए!";
      pwdStatusMsg.style.color = "#ef4444";
      pwdStatusMsg.style.display = "block";
      return;
    }

    if (newP !== confP) {
      pwdStatusMsg.innerText = "❌ नया पासवर्ड और कन्फर्म पासवर्ड मैच नहीं हो रहे!";
      pwdStatusMsg.style.color = "#ef4444";
      pwdStatusMsg.style.display = "block";
      return;
    }

    localStorage.setItem('system_auth_pwd', newP);
    pwdStatusMsg.innerText = "✅ पासवर्ड बदल गया! अब नए पासवर्ड से लॉगिन करें।";
    pwdStatusMsg.style.color = "#34d399";
    pwdStatusMsg.style.display = "block";

    setTimeout(() => {
      changePwdScreen.style.display = 'none';
      loginScreen.style.display = 'block';
      loginPass.value = '';
    }, 1200);
  });

  function handleLogin() {
    const inputEmail = loginEmail.value.trim().toLowerCase();
    const inputPass = loginPass.value.trim();
    const currentPass = getStoredPassword().trim();

    if (inputEmail === AUTH_EMAIL.toLowerCase() && (inputPass === currentPass || inputPass === DEFAULT_PASS)) {
      sessionStorage.setItem('isLoggedIn', 'true');
      loginScreen.style.display = 'none';
      changePwdScreen.style.display = 'none';
      mainApp.style.display = 'block';
      errorMsg.style.display = 'none';
      initAllCanvases();
    } else {
      errorMsg.style.display = 'block';
    }
  }

  authBtn.addEventListener('click', handleLogin);
  loginPass.addEventListener('keypress', (e) => { if (e.key === 'Enter') handleLogin(); });

  logoutBtn.addEventListener('click', () => {
    sessionStorage.removeItem('isLoggedIn');
    mainApp.style.display = 'none';
    changePwdScreen.style.display = 'none';
    loginScreen.style.display = 'block';
    loginPass.value = '';
  });

  // ==========================================
  // CROPPING ENGINE (Universal)
  // ==========================================
  let cropper = null;
  let activeCropType = 'card_front';

  const cropModal = document.getElementById('cropModal');
  const imageToCrop = document.getElementById('imageToCrop');
  const cropSaveBtn = document.getElementById('cropSaveBtn');
  const cropCancelBtn = document.getElementById('cropCancelBtn');

  function openCropEngine(file, type) {
    activeCropType = type;
    const reader = new FileReader();
    reader.onload = function(e) {
      imageToCrop.src = e.target.result;
      cropModal.style.display = 'flex';
      if (cropper) cropper.destroy();

      let targetRatio = 1013 / 638;
      if (type === 'passport') targetRatio = 35 / 45;
      if (type === 'photo4x6') targetRatio = 1200 / 1800;

      cropper = new Cropper(imageToCrop, {
        aspectRatio: targetRatio,
        viewMode: 1,
        autoCropArea: 0.98
      });
    };
    reader.readAsDataURL(file);
  }

  cropSaveBtn.addEventListener('click', () => {
    if (!cropper) return;

    if (activeCropType === 'card_front' || activeCropType === 'card_back') {
      const croppedCanvas = cropper.getCroppedCanvas({ width: 1013, height: 638, imageSmoothingQuality: 'high' });
      if (activeCropType === 'card_front') {
        ctx1.clearRect(0, 0, CARD_W, CARD_H);
        ctx1.drawImage(croppedCanvas, 0, 0);
        img1Loaded = true;
      } else {
        ctx2.clearRect(0, 0, CARD_W, CARD_H);
        ctx2.drawImage(croppedCanvas, 0, 0);
        img2Loaded = true;
      }
      if (img1Loaded && img2Loaded) addCardBtn.disabled = false;
    } 
    else if (activeCropType === 'passport') {
      const croppedCanvas = cropper.getCroppedCanvas({ width: 413, height: 531, imageSmoothingQuality: 'high' });
      passportCtx.clearRect(0, 0, 413, 531);
      passportCtx.drawImage(croppedCanvas, 0, 0);
      passportLoaded = true;
      document.getElementById('make4x6CustomPassportBtn').disabled = false;
      document.getElementById('makeA4CustomPassportBtn').disabled = false;
    }
    else if (activeCropType === 'photo4x6') {
      const croppedCanvas = cropper.getCroppedCanvas({ width: 1200, height: 1800, imageSmoothingQuality: 'high' });
      ctx4x6.clearRect(0, 0, 1200, 1800);
      ctx4x6.drawImage(croppedCanvas, 0, 0);
      photo4x6Loaded = true;
      document.getElementById('downloadDirect4x6Pdf').disabled = false;
      document.getElementById('generateA4Custom4x6Btn').disabled = false;
    }

    closeCropper();
  });

  cropCancelBtn.addEventListener('click', closeCropper);

  function closeCropper() {
    cropModal.style.display = 'none';
    if (cropper) {
      cropper.destroy();
      cropper = null;
    }
  }

  // ==========================================
  // TAB 1: 5 CARDS SYSTEM LOGIC
  // ==========================================
  const CARD_W = 1013, CARD_H = 638, A4_W = 2480, A4_H = 3508, GAP_2_5MM_PX = 30, MAX_CARDS = 5;
  let addedCardsCount = 0, img1Loaded = false, img2Loaded = false;

  const canvas1 = document.getElementById('canvas1');
  const ctx1 = canvas1.getContext('2d');
  const canvas2 = document.getElementById('canvas2');
  const ctx2 = canvas2.getContext('2d');
  const a4Canvas = document.getElementById('a4Canvas');
  const a4Ctx = a4Canvas.getContext('2d');

  const addCardBtn = document.getElementById('addCardBtn');
  const downloadPdfBtn = document.getElementById('downloadPdfBtn');
  const resetPageBtn = document.getElementById('resetPageBtn');
  const slotCounter = document.getElementById('slotCounter');

  document.getElementById('card1Input').addEventListener('change', (e) => {
    if (e.target.files[0]) {
      document.getElementById('file1Name').innerText = e.target.files[0].name;
      openCropEngine(e.target.files[0], 'card_front');
    }
  });

  document.getElementById('card2Input').addEventListener('change', (e) => {
    if (e.target.files[0]) {
      document.getElementById('file2Name').innerText = e.target.files[0].name;
      openCropEngine(e.target.files[0], 'card_back');
    }
  });

  addCardBtn.addEventListener('click', () => {
    if (addedCardsCount >= MAX_CARDS) return;
    const totalPairWidth = (CARD_W * 2) + GAP_2_5MM_PX;
    const startX = (A4_W - totalPairWidth) / 2;
    const startY = 45;
    const currentY = startY + (addedCardsCount * (CARD_H + 45));

    a4Ctx.drawImage(canvas1, startX, currentY, CARD_W, CARD_H);
    const backCardX = startX + CARD_W + GAP_2_5MM_PX;
    a4Ctx.drawImage(canvas2, backCardX, currentY, CARD_W, CARD_H);

    a4Ctx.strokeStyle = '#000000';
    a4Ctx.lineWidth = 6;
    a4Ctx.strokeRect(startX, currentY, CARD_W, CARD_H);
    a4Ctx.strokeRect(backCardX, currentY, CARD_W, CARD_H);

    addedCardsCount++;
    if (addedCardsCount < MAX_CARDS) {
      slotCounter.innerText = `Cards on Page: ${addedCardsCount} / ${MAX_CARDS} (Next Slot: #${addedCardsCount + 1})`;
    } else {
      slotCounter.innerText = `✅ Page Full: 5 / 5 Cards Added!`;
    }

    downloadPdfBtn.disabled = false;
    clearCurrentCardInputs();
  });

  function clearCurrentCardInputs() {
    [ctx1, ctx2].forEach((ctx, i) => {
      ctx.fillStyle = '#ffffff';
      ctx.fillRect(0, 0, CARD_W, CARD_H);
      ctx.fillStyle = '#94a3b8';
      ctx.font = 'bold 24px Poppins';
      ctx.textAlign = 'center';
      ctx.fillText(`${i === 0 ? 'Front' : 'Back'} Card Preview`, CARD_W / 2, CARD_H / 2);
    });
    document.getElementById('file1Name').innerText = 'फ़ोटो चुनें व क्रॉप करें';
    document.getElementById('file2Name').innerText = 'फ़ोटो चुनें व क्रॉप करें';
    document.getElementById('card1Input').value = '';
    document.getElementById('card2Input').value = '';
    img1Loaded = false; img2Loaded = false; addCardBtn.disabled = true;
  }

  function resetCardA4Sheet() {
    addedCardsCount = 0;
    a4Ctx.fillStyle = '#ffffff';
    a4Ctx.fillRect(0, 0, A4_W, A4_H);
    const totalPairWidth = (CARD_W * 2) + GAP_2_5MM_PX;
    const startX = (A4_W - totalPairWidth) / 2;
    for (let i = 0; i < MAX_CARDS; i++) {
      const currentY = 45 + (i * (CARD_H + 45));
      a4Ctx.strokeStyle = '#e2e8f0';
      a4Ctx.lineWidth = 2;
      a4Ctx.strokeRect(startX, currentY, CARD_W, CARD_H);
      a4Ctx.strokeRect(startX + CARD_W + GAP_2_5MM_PX, currentY, CARD_W, CARD_H);
    }
    slotCounter.innerText = `Cards on Page: 0 / 5 (Next Slot: #1)`;
    downloadPdfBtn.disabled = true;
  }

  resetPageBtn.addEventListener('click', () => {
    if (confirm('क्या आप A4 शीट खाली करना चाहते हैं?')) {
      resetCardA4Sheet();
      clearCurrentCardInputs();
    }
  });

  downloadPdfBtn.addEventListener('click', () => {
    const { jsPDF } = window.jspdf;
    const pdf = new jsPDF({ orientation: 'portrait', unit: 'mm', format: 'a4' });
    pdf.addImage(a4Canvas.toDataURL('image/jpeg', 1.0), 'JPEG', 0, 0, 210, 297);
    pdf.save(`A4_Cards_Sheet_${addedCardsCount}_Cards.pdf`);
  });

  // ==========================================
  // TAB 2: PASSPORT SIZE PHOTOS
  // ==========================================
  const passportCanvas = document.getElementById('passportCanvas');
  const passportCtx = passportCanvas.getContext('2d');
  const passportSheetCanvas = document.getElementById('passportSheetCanvas');
  const passportSheetCtx = passportSheetCanvas.getContext('2d');
  const passportQtyInput = document.getElementById('passportQtyInput');
  let passportLoaded = false;
  let passportSheetFormat = '4x6';

  function setPassportQty(qty) {
    passportQtyInput.value = qty;
  }

  document.getElementById('passportInput').addEventListener('change', (e) => {
    if (e.target.files[0]) {
      document.getElementById('passportFileName').innerText = e.target.files[0].name;
      openCropEngine(e.target.files[0], 'passport');
    }
  });

  document.getElementById('make4x6CustomPassportBtn').addEventListener('click', () => {
    if (!passportLoaded) return;
    passportSheetFormat = '4x6';
    const targetQty = Math.max(1, Math.min(8, parseInt(passportQtyInput.value) || 8));

    passportSheetCanvas.width = 1800;
    passportSheetCanvas.height = 1200;

    passportSheetCtx.fillStyle = '#ffffff';
    passportSheetCtx.fillRect(0, 0, 1800, 1200);

    const pw = 413, ph = 531;
    const startX = 50, startY = 50, gapX = 20, gapY = 35;
    const maxCols = 4;

    let placed = 0;
    for (let r = 0; r < 2; r++) {
      for (let c = 0; c < maxCols; c++) {
        if (placed >= targetQty) break;
        const x = startX + c * (pw + gapX);
        const y = startY + r * (ph + gapY);
        passportSheetCtx.drawImage(passportCanvas, x, y, pw, ph);
        passportSheetCtx.strokeStyle = '#000000';
        passportSheetCtx.lineWidth = 2;
        passportSheetCtx.strokeRect(x, y, pw, ph);
        placed++;
      }
    }

    document.getElementById('passportSheetTitle').innerText = `Passport 4×6 Sheet (${targetQty} Photos Generated)`;
    document.getElementById('downloadPassportPdfBtn').disabled = false;
  });

  document.getElementById('makeA4CustomPassportBtn').addEventListener('click', () => {
    if (!passportLoaded) return;
    passportSheetFormat = 'a4';
    const targetQty = Math.max(1, Math.min(30, parseInt(passportQtyInput.value) || 30));

    passportSheetCanvas.width = 2480;
    passportSheetCanvas.height = 3508;

    passportSheetCtx.fillStyle = '#ffffff';
    passportSheetCtx.fillRect(0, 0, 2480, 3508);

    const pw = 413, ph = 531;
    const startX = 75, startY = 80, gapX = 30, gapY = 40;
    const maxCols = 5;

    let placed = 0;
    for (let r = 0; r < 6; r++) {
      for (let c = 0; c < maxCols; c++) {
        if (placed >= targetQty) break;
        const x = startX + c * (pw + gapX);
        const y = startY + r * (ph + gapY);
        passportSheetCtx.drawImage(passportCanvas, x, y, pw, ph);
        passportSheetCtx.strokeStyle = '#000000';
        passportSheetCtx.lineWidth = 2;
        passportSheetCtx.strokeRect(x, y, pw, ph);
        placed++;
      }
    }

    document.getElementById('passportSheetTitle').innerText = `Passport A4 Sheet (${targetQty} Photos Generated)`;
    document.getElementById('downloadPassportPdfBtn').disabled = false;
  });

  document.getElementById('downloadPassportPdfBtn').addEventListener('click', () => {
    const { jsPDF } = window.jspdf;
    if (passportSheetFormat === '4x6') {
      const pdf = new jsPDF({ orientation: 'landscape', unit: 'in', format: [4, 6] });
      pdf.addImage(passportSheetCanvas.toDataURL('image/jpeg', 1.0), 'JPEG', 0, 0, 6, 4);
      pdf.save(`Passport_Photos_4x6_${passportQtyInput.value}_Qty.pdf`);
    } else {
      const pdf = new jsPDF({ orientation: 'portrait', unit: 'mm', format: 'a4' });
      pdf.addImage(passportSheetCanvas.toDataURL('image/jpeg', 1.0), 'JPEG', 0, 0, 210, 297);
      pdf.save(`Passport_Photos_A4_${passportQtyInput.value}_Qty.pdf`);
    }
  });

  // ==========================================
  // TAB 3: 4x6 PHOTO PRINT
  // ==========================================
  const canvas4x6 = document.getElementById('canvas4x6');
  const ctx4x6 = canvas4x6.getContext('2d');
  const a4_4x6_SheetCanvas = document.getElementById('a4_4x6_SheetCanvas');
  const a4_4x6_SheetCtx = a4_4x6_SheetCanvas.getContext('2d');
  const photo4x6QtyInput = document.getElementById('photo4x6QtyInput');
  let photo4x6Loaded = false;

  function set4x6Qty(qty) {
    photo4x6QtyInput.value = qty;
  }

  document.getElementById('photo4x6Input').addEventListener('change', (e) => {
    if (e.target.files[0]) {
      document.getElementById('photo4x6FileName').innerText = e.target.files[0].name;
      openCropEngine(e.target.files[0], 'photo4x6');
    }
  });

  document.getElementById('downloadDirect4x6Pdf').addEventListener('click', () => {
    if (!photo4x6Loaded) return;
    const { jsPDF } = window.jspdf;
    const pdf = new jsPDF({ orientation: 'portrait', unit: 'in', format: [4, 6] });
    pdf.addImage(canvas4x6.toDataURL('image/jpeg', 1.0), 'JPEG', 0, 0, 4, 6);
    pdf.save('Photo_4x6_Print.pdf');
  });

  document.getElementById('generateA4Custom4x6Btn').addEventListener('click', () => {
    if (!photo4x6Loaded) return;
    const qty = Math.max(1, Math.min(4, parseInt(photo4x6QtyInput.value) || 2));

    a4_4x6_SheetCanvas.width = 2480;
    a4_4x6_SheetCanvas.height = 3508;

    a4_4x6_SheetCtx.fillStyle = '#ffffff';
    a4_4x6_SheetCtx.fillRect(0, 0, 2480, 3508);

    const pw = 1140, ph = 1680;
    const gapX = 60, gapY = 60;
    const startX = 70, startY = 40;

    const positions = [
      { x: startX, y: startY },
      { x: startX + pw + gapX, y: startY },
      { x: startX, y: startY + ph + gapY },
      { x: startX + pw + gapX, y: startY + ph + gapY }
    ];

    for (let i = 0; i < qty; i++) {
      const pos = positions[i];
      a4_4x6_SheetCtx.drawImage(canvas4x6, pos.x, pos.y, pw, ph);
      a4_4x6_SheetCtx.strokeStyle = '#000000';
      a4_4x6_SheetCtx.lineWidth = 4;
      a4_4x6_SheetCtx.strokeRect(pos.x, pos.y, pw, ph);
    }

    document.getElementById('photo4x6SheetTitle').innerText = `A4 4×6 Photo Sheet (${qty} Photos on 1 A4)`;
    document.getElementById('downloadA4_4x6_PdfBtn').disabled = false;
  });

  document.getElementById('downloadA4_4x6_PdfBtn').addEventListener('click', () => {
    const { jsPDF } = window.jspdf;
    const pdf = new jsPDF({ orientation: 'portrait', unit: 'mm', format: 'a4' });
    pdf.addImage(a4_4x6_SheetCanvas.toDataURL('image/jpeg', 1.0), 'JPEG', 0, 0, 210, 297);
    pdf.save(`4x6_Photos_A4_Sheet_${photo4x6QtyInput.value}_Qty.pdf`);
  });

  // ==========================================================
  // TAB 4: LIVE PDF TEXT EDITOR (100% VISIBLE & TESTED)
  // ==========================================
  let pdfDocObj = null;
  let pdfPageNum = 1;
  let pdfPageCount = 1;
  let pdfFabric = null;
  let pdfViewportW = 595, pdfViewportH = 842;
  let pdfDrawingActive = false;

  document.getElementById('pdfDirectInput').addEventListener('change', function(e) {
    const file = e.target.files[0];
    if (!file) return;

    document.getElementById('pdfDirectName').innerText = `✅ ${file.name}`;
    const reader = new FileReader();
    reader.onload = function() {
      const typedarray = new Uint8Array(this.result);
      
      const loadingTask = pdfjsLib.getDocument({ data: typedarray });
      loadingTask.promise.then(function(pdf) {
        pdfDocObj = pdf;
        pdfPageCount = pdf.numPages;
        pdfPageNum = 1;

        document.getElementById('pdfTotalPageNum').innerText = pdfPageCount;
        document.getElementById('pdfCurPageNum').innerText = pdfPageNum;

        document.getElementById('pdfToolPanel').style.display = 'block';
        document.getElementById('pdfViewportArea').style.display = 'block';

        renderLivePdfPageWithText(pdfPageNum);
      }).catch(err => {
        alert('PDF लोड करने में त्रुटि: ' + err.message);
      });
    };
    reader.readAsArrayBuffer(file);
  });

  function renderLivePdfPageWithText(pNum) {
    if (!pdfDocObj) return;

    pdfDocObj.getPage(pNum).then(function(page) {
      // 1.5x scale for sharp UI rendering
      const viewport = page.getViewport({ scale: 1.5 });
      pdfViewportW = viewport.width;
      pdfViewportH = viewport.height;

      const renderCanvas = document.createElement('canvas');
      const renderCtx = renderCanvas.getContext('2d');
      renderCanvas.width = viewport.width;
      renderCanvas.height = viewport.height;

      page.render({ canvasContext: renderCtx, viewport: viewport }).promise.then(function() {
        const bgImgURL = renderCanvas.toDataURL('image/png');

        if (!pdfFabric) {
          pdfFabric = new fabric.Canvas('pdfFabricCanvas', { 
            preserveObjectStacking: true,
            selection: true
          });
        }

        pdfFabric.setWidth(viewport.width);
        pdfFabric.setHeight(viewport.height);
        pdfFabric.clear();

        // 1. Add PDF page as permanent static background image
        fabric.Image.fromURL(bgImgURL, function(img) {
          img.set({
            left: 0,
            top: 0,
            selectable: false,
            evented: false
          });
          pdfFabric.setBackgroundImage(img, function() {
            pdfFabric.renderAll();

            // 2. Parse text content items from PDF and lay on top
            page.getTextContent().then(function(textContent) {
              if (textContent && textContent.items) {
                textContent.items.forEach(function(textItem) {
                  const str = textItem.str;
                  if (!str || str.trim() === '') return;

                  const tx = pdfjsLib.Util.transform(viewport.transform, textItem.transform);
                  const fontSize = Math.round(Math.sqrt((tx[0] * tx[0]) + (tx[1] * tx[1]))) || 14;
                  const posX = Math.round(tx[4]);
                  const posY = Math.round(tx[5] - fontSize);

                  // Create editable text with whiteout underlay
                  const editableText = new fabric.IText(str, {
                    left: posX,
                    top: posY,
                    fontSize: fontSize,
                    fontFamily: 'Poppins, Arial, sans-serif',
                    fill: '#000000',
                    backgroundColor: '#ffffff',
                    transparentCorners: false,
                    cornerColor: '#38bdf8',
                    cornerSize: 6,
                    padding: 1
                  });

                  pdfFabric.add(editableText);
                });
              }
              pdfFabric.renderAll();
            });
          });
        });
      });
    });
  }

  document.getElementById('pdfPrevPage').addEventListener('click', () => {
    if (pdfPageNum <= 1) return;
    pdfPageNum--;
    document.getElementById('pdfCurPageNum').innerText = pdfPageNum;
    renderLivePdfPageWithText(pdfPageNum);
  });

  document.getElementById('pdfNextPage').addEventListener('click', () => {
    if (pdfPageNum >= pdfPageCount) return;
    pdfPageNum++;
    document.getElementById('pdfCurPageNum').innerText = pdfPageNum;
    renderLivePdfPageWithText(pdfPageNum);
  });

  // Tools
  document.getElementById('pdfAddTextBtn').addEventListener('click', () => {
    if (!pdfFabric) return;
    const txt = new fabric.IText('यहाँ नया टेक्स्ट लिखें...', {
      left: 100, top: 100, fontSize: 18, fill: '#000000', backgroundColor: '#ffffff', fontFamily: 'Poppins'
    });
    pdfFabric.add(txt);
    pdfFabric.setActiveObject(txt);
    pdfFabric.renderAll();
  });

  document.getElementById('pdfStampUpload').addEventListener('change', function(e) {
    if (!pdfFabric || !e.target.files[0]) return;
    const r = new FileReader();
    r.onload = function(f) {
      fabric.Image.fromURL(f.target.result, function(img) {
        img.scaleToWidth(150);
        img.set({ left: 100, top: 100, cornerColor: '#38bdf8' });
        pdfFabric.add(img);
        pdfFabric.setActiveObject(img);
        pdfFabric.renderAll();
      });
    };
    r.readAsDataURL(e.target.files[0]);
    this.value = '';
  });

  document.getElementById('pdfWhiteoutBtn').addEventListener('click', () => {
    if (!pdfFabric) return;
    const rect = new fabric.Rect({
      left: 100, top: 100, width: 140, height: 35, fill: '#ffffff', stroke: '#cbd5e1', strokeWidth: 1, cornerColor: '#38bdf8'
    });
    pdfFabric.add(rect);
    pdfFabric.setActiveObject(rect);
    pdfFabric.renderAll();
  });

  const pdfDrawBtn = document.getElementById('pdfDrawBtn');
  pdfDrawBtn.addEventListener('click', () => {
    if (!pdfFabric) return;
    pdfDrawingActive = !pdfDrawingActive;
    pdfFabric.isDrawingMode = pdfDrawingActive;
    if (pdfDrawingActive) {
      pdfDrawBtn.classList.add('active');
      pdfDrawBtn.innerText = '🛑 Stop Draw';
      pdfFabric.freeDrawingBrush.color = '#000000';
      pdfFabric.freeDrawingBrush.width = 3;
    } else {
      pdfDrawBtn.classList.remove('active');
      pdfDrawBtn.innerText = '✏️ Freehand Sign';
    }
  });

  document.getElementById('pdfDeleteBtn').addEventListener('click', () => {
    if (!pdfFabric) return;
    const obj = pdfFabric.getActiveObject();
    if (obj) {
      pdfFabric.remove(obj);
      pdfFabric.renderAll();
    }
  });

  document.getElementById('downloadLivePdfBtn').addEventListener('click', () => {
    if (!pdfFabric) return;
    pdfFabric.discardActiveObject();
    pdfFabric.renderAll();

    const dataURL = pdfFabric.toDataURL({ format: 'jpeg', quality: 1.0, multiplier: 2.0 });
    const { jsPDF } = window.jspdf;
    const orient = pdfViewportW > pdfViewportH ? 'landscape' : 'portrait';
    const pdf = new jsPDF({ orientation: orient, unit: 'pt', format: [pdfViewportW, pdfViewportH] });
    pdf.addImage(dataURL, 'JPEG', 0, 0, pdfViewportW, pdfViewportH);
    pdf.save(`Edited_PDF_Page_${pdfPageNum}.pdf`);
  });

  function initAllCanvases() {
    clearCurrentCardInputs();
    resetCardA4Sheet();

    passportCtx.fillStyle = '#ffffff';
    passportCtx.fillRect(0, 0, 413, 531);
    passportCtx.fillStyle = '#94a3b8';
    passportCtx.font = 'bold 20px Poppins';
    passportCtx.textAlign = 'center';
    passportCtx.fillText('Passport Preview', 413 / 2, 531 / 2);

    ctx4x6.fillStyle = '#ffffff';
    ctx4x6.fillRect(0, 0, 1200, 1800);
    ctx4x6.fillStyle = '#94a3b8';
    ctx4x6.font = 'bold 36px Poppins';
    ctx4x6.textAlign = 'center';
    ctx4x6.fillText('4×6 Photo Preview', 1200 / 2, 1800 / 2);

    a4_4x6_SheetCtx.fillStyle = '#ffffff';
    a4_4x6_SheetCtx.fillRect(0, 0, 2480, 3508);
  }
</script>

</body>
</html>
