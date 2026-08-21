<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Card Generator & Document Editor by SHIV BHAVSAR</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
  
  <!-- Fabric.js for Interactive Object Drag/Edit -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/fabric.js/5.3.1/fabric.min.js"></script>
  <!-- jsPDF Library -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <!-- Cropper.js -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/cropperjs/1.5.13/cropper.min.css"/>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/cropperjs/1.5.13/cropper.min.js"></script>

  <style>
    :root {
      --bg-gradient: linear-gradient(135deg, #0f172a 0%, #1e1b4b 50%, #0f172a 100%);
      --card-bg: rgba(30, 41, 59, 0.8);
      --accent-blue: #38bdf8;
      --accent-purple: #818cf8;
      --btn-merge: linear-gradient(135deg, #f59e0b 0%, #d97706 100%);
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
      color: var(--text-main);
    }

    #loginScreen {
      background: var(--card-bg);
      backdrop-filter: blur(20px);
      -webkit-backdrop-filter: blur(20px);
      border: 1px solid var(--border-color);
      padding: 40px 30px;
      border-radius: 20px;
      box-shadow: 0 25px 60px rgba(0, 0, 0, 0.6);
      width: 100%;
      max-width: 400px;
      text-align: center;
      margin-top: 10vh;
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
    }

    .error-msg {
      color: #ef4444;
      font-size: 13px;
      margin-top: 12px;
      display: none;
    }

    #mainApp {
      display: none;
      width: 100%;
      max-width: 1150px;
    }

    .container { 
      background: var(--card-bg); 
      backdrop-filter: blur(16px);
      -webkit-backdrop-filter: blur(16px);
      border: 1px solid var(--border-color);
      padding: 25px 20px; 
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
      font-size: 26px; 
      font-weight: 700;
    }
    
    .upload-section { 
      display: flex; 
      gap: 15px; 
      justify-content: center; 
      margin: 20px 0; 
      flex-wrap: wrap; 
    }

    .upload-box { 
      border: 2px dashed rgba(56, 189, 248, 0.4); 
      padding: 15px; 
      border-radius: 12px; 
      cursor: pointer; 
      background: rgba(15, 23, 42, 0.6); 
      flex: 1; 
      min-width: 260px; 
    }

    input[type="file"] { display: none; }

    /* Custom Toolbar for Editing */
    .editor-toolbar {
      display: flex;
      gap: 10px;
      justify-content: center;
      align-items: center;
      background: rgba(15, 23, 42, 0.9);
      padding: 12px;
      border-radius: 12px;
      border: 1px solid var(--border-color);
      margin-bottom: 20px;
      flex-wrap: wrap;
    }

    .tool-btn {
      padding: 8px 14px;
      background: #334155;
      color: #fff;
      font-size: 12px;
      border: 1px solid rgba(255, 255, 255, 0.1);
      border-radius: 6px;
      cursor: pointer;
      font-weight: 500;
    }

    .tool-btn:hover { background: #475569; }

    .preview-container { 
      display: flex; 
      justify-content: center; 
      gap: 20px; 
      margin: 15px 0; 
      flex-wrap: wrap; 
    }

    .canvas-card-box { 
      border: 1px solid var(--border-color); 
      padding: 10px; 
      background: rgba(15, 23, 42, 0.9); 
      border-radius: 12px; 
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .canvas-card-box h4 { 
      font-size: 12px; 
      color: var(--text-muted); 
      margin-bottom: 8px; 
    }

    .canvas-container {
      border-radius: 6px;
      overflow: hidden;
      box-shadow: 0 4px 15px rgba(0,0,0,0.5);
    }
    
    .btn-group { 
      display: flex; 
      gap: 15px; 
      justify-content: center; 
      margin-top: 20px; 
      flex-wrap: wrap; 
    }

    .action-btn { 
      padding: 12px 28px; 
      font-size: 14px; 
      font-weight: 600; 
      border: none; 
      border-radius: 10px; 
      cursor: pointer; 
      color: #fff;
    }

    .btn-merge { background: var(--btn-merge); }
    .btn-download { background: var(--btn-download); }

    .action-btn:disabled { 
      background: #334155; 
      color: #64748b; 
      cursor: not-allowed; 
    }

    /* Modal */
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

<!-- Login Screen -->
<div id="loginScreen">
  <div class="badge">Protected Access</div>
  <h2 style="font-size: 22px; margin-bottom: 6px;">Sign In</h2>
  <p style="font-size: 12px; color: var(--text-muted); margin-bottom: 20px;">Card Generator & Editor by Shiv Bhavsar</p>

  <input type="email" id="loginEmail" class="login-input" placeholder="ईमेल आईडी दर्ज करें">
  <input type="password" id="loginPass" class="login-input" placeholder="पासवर्ड दर्ज करें">
  <button id="authBtn" class="login-btn">लॉगिन करें</button>
  <div id="errorMsg" class="error-msg">⚠️ गलत ईमेल आईडी या पासवर्ड!</div>
</div>

<!-- Main Application -->
<div id="mainApp">
  <div class="container">
    <button id="logoutBtn" class="logout-btn">🔒 Logout</button>
    <div class="badge">Interactive Canvas Editor</div>
    <h1>Card Generator & Live Editor</h1>
    <div style="font-size: 13px; color: var(--accent-purple); font-weight: 600; margin-bottom: 4px;">by Shiv Bhavsar</div>
    <p style="font-size: 12px; color: var(--text-muted); margin-bottom: 20px;">डॉक्युमेंट अपलोड करें, कुछ भी नया जोड़ें/एडिट करें और सीधे A4 PDF निकालें।</p>

    <div class="upload-section">
      <label class="upload-box" for="card1Input">
        <strong>📁 Front Side (Card 1)</strong>
        <div id="file1Name" style="font-size: 12px; color: var(--text-muted);">फ़ोटो चुनें व एडिट करें</div>
      </label>
      <input type="file" id="card1Input" accept="image/*">

      <label class="upload-box" for="card2Input">
        <strong>📁 Back Side (Card 2)</strong>
        <div id="file2Name" style="font-size: 12px; color: var(--text-muted);">फ़ोटो चुनें व एडिट करें</div>
      </label>
      <input type="file" id="card2Input" accept="image/*">
    </div>

    <!-- Custom Edit Tools -->
    <div class="editor-toolbar">
      <span style="font-size: 12px; color: var(--accent-blue); font-weight:600;">🛠️ एडिट टूल्स:</span>
      <button class="tool-btn" id="addTextBtn">➕ नया टेक्स्ट जोड़ें</button>
      <button class="tool-btn" id="addWhiteBoxBtn">⬜ व्हाइट पैच (इरेज़र/व्हाइटनर)</button>
      <label class="tool-btn" style="cursor:pointer;" for="overlayImgInput">
        🖼️ ऊपर फ़ोटो/लोगो जोड़ें
      </label>
      <input type="file" id="overlayImgInput" accept="image/*">
      <button class="tool-btn" id="deleteSelectedBtn" style="background:#ef4444;">🗑️ सिलेक्टेड डिलीट करें</button>
    </div>

    <div class="preview-container">
      <div class="canvas-card-box">
        <h4>Front Card (1013x638 - Editable)</h4>
        <canvas id="fCanvas1" width="506" height="319"></canvas>
      </div>
      <div class="canvas-card-box">
        <h4>Back Card (1013x638 - Editable)</h4>
        <canvas id="fCanvas2" width="506" height="319"></canvas>
      </div>
    </div>

    <div class="btn-group">
      <button id="mergeBtn" class="action-btn btn-merge" disabled>⚡ Merge Cards (Zero Gap)</button>
    </div>

    <div style="margin-top: 30px; border-top: 1px solid var(--border-color); padding-top: 20px;">
      <h3 style="font-size: 16px; color: var(--accent-blue); margin-bottom: 6px;">A4 Sheet Preview (2480 × 3508 px)</h3>
      <p style="font-size: 12px; color: var(--text-muted); margin-bottom: 15px;">एडिट किया हुआ डॉक्युमेंट A4 शीट पर सीधे PDF डाउनलोड के लिए तैयार है।</p>
      
      <div style="display:inline-block; max-width: 250px; background:#fff; border-radius:6px; overflow:hidden;">
        <canvas id="a4Canvas" width="2480" height="3508" style="width: 100%; display:block;"></canvas>
      </div>

      <div class="btn-group">
        <button id="downloadPdfBtn" class="action-btn btn-download" disabled>📥 Direct A4 PDF Download</button>
      </div>
    </div>

    <footer style="margin-top: 25px; font-size: 12px; color: var(--text-muted);">
      Designed & Developed by <strong>Shiv Bhavsar</strong>
    </footer>
  </div>
</div>

<!-- Crop Modal -->
<div id="cropModal">
  <div style="color:#fff; margin-bottom: 10px; font-weight: 600;">कार्ड का सही हिस्सा सेलेक्ट (Crop) करें:</div>
  <div class="crop-wrapper">
    <img id="imageToCrop" src="">
  </div>
  <div class="btn-group">
    <button id="cropSaveBtn" class="action-btn btn-download">✂️ Set in Editor (1013x638)</button>
    <button id="cropCancelBtn" class="action-btn" style="background:#ef4444;">रद्द करें</button>
  </div>
</div>

<script>
  const AUTH_EMAIL = "oneplus777000@gmail.com";
  const AUTH_PASS = "Pass@123";

  const loginScreen = document.getElementById('loginScreen');
  const mainApp = document.getElementById('mainApp');
  const loginEmail = document.getElementById('loginEmail');
  const loginPass = document.getElementById('loginPass');
  const authBtn = document.getElementById('authBtn');
  const errorMsg = document.getElementById('errorMsg');
  const logoutBtn = document.getElementById('logoutBtn');

  sessionStorage.removeItem('isLoggedIn');
  loginScreen.style.display = 'block';
  mainApp.style.display = 'none';

  let fabric1, fabric2;
  let activeFabric = null;

  function initFabric() {
    fabric1 = new fabric.Canvas('fCanvas1', { selection: true, preserveObjectStacking: true });
    fabric2 = new fabric.Canvas('fCanvas2', { selection: true, preserveObjectStacking: true });

    fabric1.setBackgroundColor('#ffffff', fabric1.renderAll.bind(fabric1));
    fabric2.setBackgroundColor('#ffffff', fabric2.renderAll.bind(fabric2));

    fabric1.on('mouse:down', () => activeFabric = fabric1);
    fabric2.on('mouse:down', () => activeFabric = fabric2);
    activeFabric = fabric1;
  }

  function handleLogin() {
    if (loginEmail.value.trim() === AUTH_EMAIL && loginPass.value.trim() === AUTH_PASS) {
      sessionStorage.setItem('isLoggedIn', 'true');
      loginScreen.style.display = 'none';
      mainApp.style.display = 'block';
      errorMsg.style.display = 'none';
      setTimeout(initFabric, 100);
    } else {
      errorMsg.style.display = 'block';
    }
  }

  authBtn.addEventListener('click', handleLogin);
  loginPass.addEventListener('keypress', (e) => { if (e.key === 'Enter') handleLogin(); });

  logoutBtn.addEventListener('click', () => {
    sessionStorage.removeItem('isLoggedIn');
    mainApp.style.display = 'none';
    loginScreen.style.display = 'block';
  });

  // Cropper Setup
  let cropper = null, currentTarget = 1;
  const cropModal = document.getElementById('cropModal');
  const imageToCrop = document.getElementById('imageToCrop');
  const cropSaveBtn = document.getElementById('cropSaveBtn');
  const cropCancelBtn = document.getElementById('cropCancelBtn');

  function openCropper(file, target) {
    currentTarget = target;
    const reader = new FileReader();
    reader.onload = function(e) {
      imageToCrop.src = e.target.result;
      cropModal.style.display = 'flex';
      if (cropper) cropper.destroy();
      cropper = new Cropper(imageToCrop, {
        aspectRatio: 1013 / 638,
        viewMode: 1,
        autoCropArea: 0.98
      });
    };
    reader.readAsDataURL(file);
  }

  document.getElementById('card1Input').addEventListener('change', (e) => {
    if (e.target.files[0]) openCropper(e.target.files[0], 1);
  });

  document.getElementById('card2Input').addEventListener('change', (e) => {
    if (e.target.files[0]) openCropper(e.target.files[0], 2);
  });

  let img1Loaded = false, img2Loaded = false;

  cropSaveBtn.addEventListener('click', () => {
    if (!cropper) return;
    const croppedDataUrl = cropper.getCroppedCanvas({ width: 1013, height: 638 }).toDataURL('image/png');
    
    fabric.Image.fromURL(croppedDataUrl, (img) => {
      img.scaleToWidth(506);
      img.scaleToHeight(319);
      img.set({ selectable: false, evented: false });

      if (currentTarget === 1) {
        fabric1.clear();
        fabric1.add(img);
        fabric1.sendToBack(img);
        img1Loaded = true;
      } else {
        fabric2.clear();
        fabric2.add(img);
        fabric2.sendToBack(img);
        img2Loaded = true;
      }

      if (img1Loaded && img2Loaded) document.getElementById('mergeBtn').disabled = false;
    });

    cropModal.style.display = 'none';
    cropper.destroy();
    cropper = null;
  });

  cropCancelBtn.addEventListener('click', () => {
    cropModal.style.display = 'none';
    if (cropper) cropper.destroy();
  });

  // Tools: Add Text, White Patch, Overlay Image & Delete
  document.getElementById('addTextBtn').addEventListener('click', () => {
    if (!activeFabric) return;
    const text = new fabric.IText('यहाँ लिखें...', {
      left: 50, top: 50,
      fontFamily: 'Poppins',
      fontSize: 16,
      fill: '#000000',
      editable: true
    });
    activeFabric.add(text);
    activeFabric.setActiveObject(text);
  });

  document.getElementById('addWhiteBoxBtn').addEventListener('click', () => {
    if (!activeFabric) return;
    const rect = new fabric.Rect({
      left: 60, top: 60,
      width: 120, height: 35,
      fill: '#ffffff',
      stroke: '#dddddd',
      strokeWidth: 1
    });
    activeFabric.add(rect);
    activeFabric.setActiveObject(rect);
  });

  document.getElementById('overlayImgInput').addEventListener('change', (e) => {
    if (!e.target.files[0] || !activeFabric) return;
    const reader = new FileReader();
    reader.onload = function(evt) {
      fabric.Image.fromURL(evt.target.result, (img) => {
        img.scaleToWidth(100);
        img.set({ left: 80, top: 80 });
        activeFabric.add(img);
        activeFabric.setActiveObject(img);
      });
    };
    reader.readAsDataURL(e.target.files[0]);
  });

  document.getElementById('deleteSelectedBtn').addEventListener('click', () => {
    if (!activeFabric) return;
    const activeObj = activeFabric.getActiveObject();
    if (activeObj && activeObj.selectable !== false) {
      activeFabric.remove(activeObj);
    }
  });

  // Merge to A4 Logic
  const a4Canvas = document.getElementById('a4Canvas');
  const a4Ctx = a4Canvas.getContext('2d');
  const CARD_W = 1013, CARD_H = 638, A4_W = 2480, A4_H = 3508;

  document.getElementById('mergeBtn').addEventListener('click', () => {
    a4Ctx.fillStyle = '#ffffff';
    a4Ctx.fillRect(0, 0, A4_W, A4_H);

    // Export High-Res (Multiplier 2 to reach 1013x638)
    const imgData1 = fabric1.toDataURL({ format: 'png', multiplier: 2 });
    const imgData2 = fabric2.toDataURL({ format: 'png', multiplier: 2 });

    const img1 = new Image();
    img1.onload = () => {
      const img2 = new Image();
      img2.onload = () => {
        const totalCardsWidth = CARD_W * 2;
        const startX = (A4_W - totalCardsWidth) / 2;
        const startY = 150;

        a4Ctx.drawImage(img1, startX, startY, CARD_W, CARD_H);
        a4Ctx.drawImage(img2, startX + CARD_W, startY, CARD_W, CARD_H);

        document.getElementById('downloadPdfBtn').disabled = false;
      };
      img2.src = imgData2;
    };
    img1.src = imgData1;
  });

  document.getElementById('downloadPdfBtn').addEventListener('click', () => {
    const { jsPDF } = window.jspdf;
    const pdf = new jsPDF({ orientation: 'portrait', unit: 'mm', format: 'a4' });
    const imgData = a4Canvas.toDataURL('image/jpeg', 1.0);
    pdf.addImage(imgData, 'JPEG', 0, 0, 210, 297);
    pdf.save('Edited_A4_Print_Cards.pdf');
  });
</script>

</body>
</html>
