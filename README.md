<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Card Generator System by SHIV BHAVSAR</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
  
  <!-- jsPDF Library for A4 PDF -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

  <!-- Cropper.js for Manual Selection & Crop -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/cropperjs/1.5.13/cropper.min.css"/>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/cropperjs/1.5.13/cropper.min.js"></script>

  <style>
    :root {
      --bg-gradient: linear-gradient(135deg, #0f172a 0%, #1e1b4b 50%, #0f172a 100%);
      --card-bg: rgba(30, 41, 59, 0.75);
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
      padding: 20px 15px; 
      display: flex; 
      flex-direction: column; 
      align-items: center; 
      justify-content: center;
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
      transition: 0.3s;
    }

    .login-input:focus {
      border-color: var(--accent-blue);
      box-shadow: 0 0 12px rgba(56, 189, 248, 0.4);
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

    .login-btn:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 18px rgba(37, 99, 235, 0.4);
    }

    .error-msg {
      color: #ef4444;
      font-size: 13px;
      margin-top: 12px;
      display: none;
      font-weight: 500;
    }

    #mainApp {
      display: none;
      width: 100%;
      max-width: 1050px;
    }

    .container { 
      background: var(--card-bg); 
      backdrop-filter: blur(16px);
      -webkit-backdrop-filter: blur(16px);
      border: 1px solid var(--border-color);
      padding: 35px 25px; 
      border-radius: 20px; 
      box-shadow: 0 20px 50px rgba(0, 0, 0, 0.4); 
      width: 100%; 
      text-align: center; 
      position: relative;
    }

    .logout-btn {
      position: absolute;
      top: 20px;
      right: 20px;
      background: rgba(239, 68, 68, 0.2);
      border: 1px solid rgba(239, 68, 68, 0.4);
      color: #fca5a5;
      padding: 6px 14px;
      font-size: 12px;
      border-radius: 8px;
      cursor: pointer;
      transition: 0.2s;
    }

    .logout-btn:hover {
      background: rgba(239, 68, 68, 0.4);
    }

    h1 { 
      background: linear-gradient(to right, #38bdf8, #a855f7, #ec4899);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      font-size: 28px; 
      font-weight: 700;
      margin-bottom: 6px; 
    }

    .subtitle { 
      color: var(--text-muted); 
      margin-bottom: 30px; 
      font-size: 13px; 
    }
    
    .upload-section { 
      display: flex; 
      gap: 20px; 
      justify-content: center; 
      margin-bottom: 25px; 
      flex-wrap: wrap; 
    }

    .upload-box { 
      border: 2px dashed rgba(56, 189, 248, 0.4); 
      padding: 22px 15px; 
      border-radius: 14px; 
      cursor: pointer; 
      background: rgba(15, 23, 42, 0.6); 
      flex: 1; 
      min-width: 280px; 
      text-align: center; 
      transition: all 0.3s ease;
    }

    .upload-box:hover { 
      border-color: var(--accent-blue);
      background: rgba(56, 189, 248, 0.05);
      transform: translateY(-2px);
    }

    .upload-box strong {
      display: block;
      font-size: 14px;
      color: var(--text-main);
      margin-bottom: 4px;
    }

    .file-status {
      font-size: 12px;
      color: var(--text-muted);
    }
    
    input[type="file"] { display: none; }
    
    .preview-container { 
      display: flex; 
      justify-content: center; 
      gap: 20px; 
      margin: 25px 0; 
      flex-wrap: wrap; 
    }

    .preview-box { 
      border: 1px solid var(--border-color); 
      padding: 12px; 
      background: rgba(15, 23, 42, 0.8); 
      border-radius: 12px; 
      box-shadow: 0 8px 20px rgba(0,0,0,0.3);
    }

    .preview-box h4 { 
      font-size: 12px; 
      color: var(--text-muted); 
      margin-bottom: 8px; 
      font-weight: 500;
      text-transform: uppercase;
    }
    
    canvas { 
      max-width: 100%; 
      height: auto; 
      display: block; 
      margin: 0 auto; 
      border-radius: 6px;
      background: #fff; 
    }
    
    .merged-section { 
      margin-top: 30px; 
      border-top: 1px solid var(--border-color); 
      padding-top: 25px; 
    }

    .merged-section h3 {
      font-size: 17px;
      color: var(--accent-blue);
      margin-bottom: 6px;
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
      transition: all 0.3s ease; 
      display: inline-flex;
      align-items: center;
      gap: 8px;
      color: #fff;
    }

    .action-btn:hover:not(:disabled) {
      transform: translateY(-2px);
      box-shadow: 0 8px 20px rgba(0,0,0,0.3);
    }

    .btn-merge { background: var(--btn-merge); }
    .btn-download { background: var(--btn-download); }

    .action-btn:disabled { 
      background: #334155; 
      color: #64748b; 
      cursor: not-allowed; 
      transform: none;
      box-shadow: none;
    }

    /* Manual Crop Modal Popup */
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

    footer {
      margin-top: 25px;
      font-size: 12px;
      color: var(--text-muted);
    }
  </style>
</head>
<body>

<!-- Login Screen -->
<div id="loginScreen">
  <div class="badge">Protected Access</div>
  <h2 style="font-size: 22px; margin-bottom: 6px;">Sign In</h2>
  <p style="font-size: 12px; color: var(--text-muted); margin-bottom: 25px;">Card Generator System by Shiv Bhavsar</p>

  <input type="email" id="loginEmail" class="login-input" placeholder="ईमेल आईडी दर्ज करें">
  <input type="password" id="loginPass" class="login-input" placeholder="पासवर्ड दर्ज करें">
  <button id="authBtn" class="login-btn">लॉगिन करें</button>
  <div id="errorMsg" class="error-msg">⚠️ गलत ईमेल आईडी या पासवर्ड!</div>
</div>

<!-- Main App -->
<div id="mainApp">
  <div class="container">
    <button id="logoutBtn" class="logout-btn">🔒 Logout</button>
    <div class="badge">Manual Select & Crop</div>
    <h1>Card Generator System</h1>
    <div style="font-size: 13px; color: var(--accent-purple); font-weight: 600; margin-bottom: 4px;">by Shiv Bhavsar</div>
    <p class="subtitle">Select Custom Area • 1013 × 638 Fit • Direct A4 PDF</p>

    <div class="upload-section">
      <label class="upload-box" for="card1Input">
        <strong>📁 Front Side (Card 1)</strong>
        <div id="file1Name" class="file-status">फ़ोटो चुनें व क्रॉप करें</div>
      </label>
      <input type="file" id="card1Input" accept="image/*">

      <label class="upload-box" for="card2Input">
        <strong>📁 Back Side (Card 2)</strong>
        <div id="file2Name" class="file-status">फ़ोटो चुनें व क्रॉप करें</div>
      </label>
      <input type="file" id="card2Input" accept="image/*">
    </div>

    <div class="preview-container">
      <div class="preview-box">
        <h4>Card 1 Selected (1013x638)</h4>
        <canvas id="canvas1" width="1013" height="638" style="width: 220px;"></canvas>
      </div>
      <div class="preview-box">
        <h4>Card 2 Selected (1013x638)</h4>
        <canvas id="canvas2" width="1013" height="638" style="width: 220px;"></canvas>
      </div>
    </div>

    <div class="btn-group">
      <button id="mergeBtn" class="action-btn btn-merge" disabled>⚡ Merge Cards (Zero Gap)</button>
    </div>

    <div class="merged-section">
      <h3>A4 Sheet Preview (2480 × 3508 px)</h3>
      <p style="font-size: 12px; color: var(--text-muted); margin-bottom: 15px;">बिना किसी गैप के दोनों कार्ड A4 शीट पर सीधे PDF डाउनलोड के लिए तैयार हैं।</p>
      
      <div class="preview-box" style="display:inline-block; max-width: 260px;">
        <canvas id="a4Canvas" width="2480" height="3508" style="width: 100%; border: 1px solid rgba(255,255,255,0.2);"></canvas>
      </div>

      <div class="btn-group">
        <button id="downloadPdfBtn" class="action-btn btn-download" disabled>📥 Direct A4 PDF Download</button>
      </div>
    </div>

    <footer>
      Designed & Developed by <strong>Shiv Bhavsar</strong>
    </footer>
  </div>
</div>

<!-- Crop Modal Box -->
<div id="cropModal">
  <div style="color:#fff; margin-bottom: 10px; font-weight: 600;">कार्ड का सही हिस्सा सेलेक्ट (Crop) करें:</div>
  <div class="crop-wrapper">
    <img id="imageToCrop" src="">
  </div>
  <div class="btn-group">
    <button id="cropSaveBtn" class="action-btn btn-download">✂️ Crop & Set (1013x638)</button>
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

  function handleLogin() {
    const inputEmail = loginEmail.value.trim();
    const inputPass = loginPass.value.trim();

    if (inputEmail === AUTH_EMAIL && inputPass === AUTH_PASS) {
      sessionStorage.setItem('isLoggedIn', 'true');
      loginScreen.style.display = 'none';
      mainApp.style.display = 'block';
      errorMsg.style.display = 'none';
      initCanvases();
    } else {
      errorMsg.style.display = 'block';
    }
  }

  authBtn.addEventListener('click', handleLogin);
  loginPass.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') handleLogin();
  });

  logoutBtn.addEventListener('click', () => {
    sessionStorage.removeItem('isLoggedIn');
    mainApp.style.display = 'none';
    loginScreen.style.display = 'block';
    loginEmail.value = '';
    loginPass.value = '';
  });

  const card1Input = document.getElementById('card1Input');
  const card2Input = document.getElementById('card2Input');
  const file1Name = document.getElementById('file1Name');
  const file2Name = document.getElementById('file2Name');
  
  const canvas1 = document.getElementById('canvas1');
  const ctx1 = canvas1.getContext('2d');
  
  const canvas2 = document.getElementById('canvas2');
  const ctx2 = canvas2.getContext('2d');
  
  const a4Canvas = document.getElementById('a4Canvas');
  const a4Ctx = a4Canvas.getContext('2d');

  const mergeBtn = document.getElementById('mergeBtn');
  const downloadPdfBtn = document.getElementById('downloadPdfBtn');

  const CARD_W = 1013;
  const CARD_H = 638;
  const A4_W = 2480;
  const A4_H = 3508;

  let img1Loaded = false;
  let img2Loaded = false;

  function initCanvases() {
    [ctx1, ctx2].forEach((ctx, i) => {
      ctx.fillStyle = '#ffffff';
      ctx.fillRect(0, 0, CARD_W, CARD_H);
      ctx.fillStyle = '#94a3b8';
      ctx.font = 'bold 24px Poppins, sans-serif';
      ctx.textAlign = 'center';
      ctx.fillText(`Card ${i+1} Preview`, CARD_W / 2, CARD_H / 2);
    });

    a4Ctx.fillStyle = '#ffffff';
    a4Ctx.fillRect(0, 0, A4_W, A4_H);
    a4Ctx.fillStyle = '#94a3b8';
    a4Ctx.font = 'bold 60px Poppins, sans-serif';
    a4Ctx.textAlign = 'center';
    a4Ctx.fillText('A4 Sheet Canvas (2480 x 3508)', A4_W / 2, A4_H / 2);
  }

  // Cropper Variables
  let cropper = null;
  let currentTarget = null;
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
        aspectRatio: CARD_W / CARD_H, // 1013 / 638 ratio lock
        viewMode: 1,
        autoCropArea: 0.95,
        responsive: true
      });
    };
    reader.readAsDataURL(file);
  }

  card1Input.addEventListener('change', (e) => {
    if (!e.target.files[0]) return;
    file1Name.innerText = e.target.files[0].name;
    openCropper(e.target.files[0], 1);
  });

  card2Input.addEventListener('change', (e) => {
    if (!e.target.files[0]) return;
    file2Name.innerText = e.target.files[0].name;
    openCropper(e.target.files[0], 2);
  });

  cropSaveBtn.addEventListener('click', () => {
    if (!cropper) return;

    // Get exact 1013x638 cropped canvas
    const croppedCanvas = cropper.getCroppedCanvas({
      width: CARD_W,
      height: CARD_H,
      imageSmoothingEnabled: true,
      imageSmoothingQuality: 'high'
    });

    if (currentTarget === 1) {
      ctx1.clearRect(0, 0, CARD_W, CARD_H);
      ctx1.drawImage(croppedCanvas, 0, 0);
      img1Loaded = true;
    } else if (currentTarget === 2) {
      ctx2.clearRect(0, 0, CARD_W, CARD_H);
      ctx2.drawImage(croppedCanvas, 0, 0);
      img2Loaded = true;
    }

    if (img1Loaded && img2Loaded) {
      mergeBtn.disabled = false;
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
    card1Input.value = '';
    card2Input.value = '';
  }

  mergeBtn.addEventListener('click', () => {
    a4Ctx.fillStyle = '#ffffff';
    a4Ctx.fillRect(0, 0, A4_W, A4_H);

    const totalCardsWidth = CARD_W * 2; 
    const startX = (A4_W - totalCardsWidth) / 2;
    const startY = 150;

    a4Ctx.drawImage(canvas1, startX, startY);
    a4Ctx.drawImage(canvas2, startX + CARD_W, startY);

    downloadPdfBtn.disabled = false;
  });

  downloadPdfBtn.addEventListener('click', () => {
    const { jsPDF } = window.jspdf;
    const pdf = new jsPDF({
      orientation: 'portrait',
      unit: 'mm',
      format: 'a4'
    });

    const imgData = a4Canvas.toDataURL('image/jpeg', 1.0);
    pdf.addImage(imgData, 'JPEG', 0, 0, 210, 297);
    pdf.save('A4_Print_Card_Sheet.pdf');
  });
</script>

</body>
</html>
