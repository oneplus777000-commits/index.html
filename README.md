<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sequential 5 Card Generator by SHIV BHAVSAR</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
  
  <!-- jsPDF Library -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

  <!-- Cropper.js for Precise Crop -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/cropperjs/1.5.13/cropper.min.css"/>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/cropperjs/1.5.13/cropper.min.js"></script>

  <style>
    :root {
      --bg-gradient: linear-gradient(135deg, #0f172a 0%, #1e1b4b 50%, #0f172a 100%);
      --card-bg: rgba(30, 41, 59, 0.8);
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
      max-width: 1050px;
    }

    .container { 
      background: var(--card-bg); 
      backdrop-filter: blur(16px);
      -webkit-backdrop-filter: blur(16px);
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
      font-size: 26px; 
      font-weight: 700;
      margin-bottom: 4px; 
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
      padding: 18px 15px; 
      border-radius: 14px; 
      cursor: pointer; 
      background: rgba(15, 23, 42, 0.6); 
      flex: 1; 
      min-width: 260px; 
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
      border-radius: 6px;
      background: #fff; 
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
      color: #fff;
    }

    .action-btn:hover:not(:disabled) {
      transform: translateY(-2px);
      box-shadow: 0 6px 20px rgba(0,0,0,0.4);
    }

    .btn-add { background: var(--btn-add); }
    .btn-download { background: var(--btn-download); }
    .btn-reset { background: rgba(239, 68, 68, 0.2); border: 1px solid rgba(239, 68, 68, 0.4); color: #fca5a5; }
    .btn-reset:hover { background: rgba(239, 68, 68, 0.4); }

    .action-btn:disabled { 
      background: #334155; 
      color: #64748b; 
      cursor: not-allowed; 
      transform: none;
      box-shadow: none;
    }

    /* Modal for Crop */
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
  <p style="font-size: 12px; color: var(--text-muted); margin-bottom: 20px;">Card Generator System by Shiv Bhavsar</p>

  <input type="email" id="loginEmail" class="login-input" placeholder="ईमेल आईडी दर्ज करें">
  <input type="password" id="loginPass" class="login-input" placeholder="पासवर्ड दर्ज करें">
  <button id="authBtn" class="login-btn">लॉगिन करें</button>
  <div id="errorMsg" class="error-msg">⚠️ गलत ईमेल आईडी या पासवर्ड!</div>
</div>

<!-- Main App -->
<div id="mainApp">
  <div class="container">
    <button id="logoutBtn" class="logout-btn">🔒 Logout</button>
    <div class="badge">Step-by-Step 5 Card Accumulator</div>
    <h1>Card Generator System</h1>
    <div style="font-size: 13px; color: var(--accent-purple); font-weight: 600; margin-bottom: 4px;">by Shiv Bhavsar</div>
    <p style="font-size: 12px; color: var(--text-muted); margin-bottom: 10px;">एक-एक करके 5 कार्ड्स जोड़ें, वे A4 शीट पर क्रम से नीचे सेट होते जाएँगे।</p>
    
    <div id="slotCounter" class="slot-counter-badge">Cards on Page: 0 / 5 (Next Slot: #1)</div>

    <!-- Upload Current Card Pair -->
    <div class="upload-section">
      <label class="upload-box" for="card1Input">
        <strong style="display:block; font-size:14px; margin-bottom:4px;">📁 Current Front Side</strong>
        <div id="file1Name" style="font-size: 12px; color: var(--text-muted);">फ़ोटो चुनें व क्रॉप करें</div>
      </label>
      <input type="file" id="card1Input" accept="image/*">

      <label class="upload-box" for="card2Input">
        <strong style="display:block; font-size:14px; margin-bottom:4px;">📁 Current Back Side</strong>
        <div id="file2Name" style="font-size: 12px; color: var(--text-muted);">फ़ोटो चुनें व क्रॉप करें</div>
      </label>
      <input type="file" id="card2Input" accept="image/*">
    </div>

    <div class="preview-container">
      <div class="preview-box">
        <h4>Front Card (1013x638)</h4>
        <canvas id="canvas1" width="1013" height="638" style="width: 200px;"></canvas>
      </div>
      <div class="preview-box">
        <h4>Back Card (1013x638)</h4>
        <canvas id="canvas2" width="1013" height="638" style="width: 200px;"></canvas>
      </div>
    </div>

    <div class="btn-group">
      <button id="addCardBtn" class="action-btn btn-add" disabled>➕ Add This Card to A4 Sheet</button>
      <button id="resetPageBtn" class="action-btn btn-reset">🔄 Clear A4 Page</button>
    </div>

    <!-- Accumulated A4 Canvas Section -->
    <div style="margin-top: 30px; border-top: 1px solid var(--border-color); padding-top: 20px;">
      <h3 style="font-size: 16px; color: var(--accent-blue); margin-bottom: 6px;">Accumulated A4 Sheet Preview (2480 × 3508 px)</h3>
      <p style="font-size: 12px; color: var(--text-muted); margin-bottom: 15px;">हर नया कार्ड पिछले कार्ड के नीचे बिना किसी बदलाव के सुरक्षित जुड़ता रहेगा।</p>
      
      <div style="display:inline-block; max-width: 280px; background:#fff; border-radius:6px; overflow:hidden; border: 1px solid #475569;">
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
    if (loginEmail.value.trim() === AUTH_EMAIL && loginPass.value.trim() === AUTH_PASS) {
      sessionStorage.setItem('isLoggedIn', 'true');
      loginScreen.style.display = 'none';
      mainApp.style.display = 'block';
      errorMsg.style.display = 'none';
      initApp();
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

  const CARD_W = 1013;
  const CARD_H = 638;
  const A4_W = 2480;
  const A4_H = 3508;
  const MAX_CARDS = 5;

  let addedCardsCount = 0;

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

  let img1Loaded = false;
  let img2Loaded = false;

  function initApp() {
    clearCurrentInputs();
    resetA4Sheet();
  }

  function clearCurrentInputs() {
    [ctx1, ctx2].forEach((ctx, i) => {
      ctx.fillStyle = '#ffffff';
      ctx.fillRect(0, 0, CARD_W, CARD_H);
      ctx.fillStyle = '#94a3b8';
      ctx.font = 'bold 24px Poppins, sans-serif';
      ctx.textAlign = 'center';
      ctx.fillText(`${i === 0 ? 'Front' : 'Back'} Card Preview`, CARD_W / 2, CARD_H / 2);
    });

    document.getElementById('file1Name').innerText = 'फ़ोटो चुनें व क्रॉप करें';
    document.getElementById('file2Name').innerText = 'फ़ोटो चुनें व क्रॉप करें';
    document.getElementById('card1Input').value = '';
    document.getElementById('card2Input').value = '';
    img1Loaded = false;
    img2Loaded = false;
    addCardBtn.disabled = true;
  }

  function resetA4Sheet() {
    addedCardsCount = 0;
    a4Ctx.fillStyle = '#ffffff';
    a4Ctx.fillRect(0, 0, A4_W, A4_H);

    // Initial background guide outlines for all 5 slots
    const totalWidth = CARD_W * 2;
    const startX = (A4_W - totalWidth) / 2;
    const startY = 50;
    const verticalGap = 50;

    for (let i = 0; i < MAX_CARDS; i++) {
      const currentY = startY + (i * (CARD_H + verticalGap));
      a4Ctx.strokeStyle = '#f1f5f9';
      a4Ctx.lineWidth = 2;
      a4Ctx.strokeRect(startX, currentY, totalWidth, CARD_H);
    }

    updateCounter();
    downloadPdfBtn.disabled = true;
  }

  function updateCounter() {
    if (addedCardsCount < MAX_CARDS) {
      slotCounter.innerText = `Cards on Page: ${addedCardsCount} / ${MAX_CARDS} (Next Slot: #${addedCardsCount + 1})`;
      slotCounter.style.color = '#fbbf24';
    } else {
      slotCounter.innerText = `✅ Page Full: 5 / 5 Cards Added!`;
      slotCounter.style.color = '#34d399';
    }
  }

  // Cropper Variables
  let cropper = null;
  let currentTarget = 1;
  const cropModal = document.getElementById('cropModal');
  const imageToCrop = document.getElementById('imageToCrop');
  const cropSaveBtn = document.getElementById('cropSaveBtn');
  const cropCancelBtn = document.getElementById('cropCancelBtn');

  function openCropper(file, target) {
    if (addedCardsCount >= MAX_CARDS) {
      alert('A4 शीट पर 5 कार्ड्स की जगह पूरी हो चुकी है! नई शीट के लिए "Clear A4 Page" दबाएँ या PDF डाउनलोड करें।');
      return;
    }

    currentTarget = target;
    const reader = new FileReader();
    reader.onload = function(e) {
      imageToCrop.src = e.target.result;
      cropModal.style.display = 'flex';
      if (cropper) cropper.destroy();
      cropper = new Cropper(imageToCrop, {
        aspectRatio: CARD_W / CARD_H,
        viewMode: 1,
        autoCropArea: 0.98
      });
    };
    reader.readAsDataURL(file);
  }

  document.getElementById('card1Input').addEventListener('change', (e) => {
    if (e.target.files[0]) {
      document.getElementById('file1Name').innerText = e.target.files[0].name;
      openCropper(e.target.files[0], 1);
    }
  });

  document.getElementById('card2Input').addEventListener('change', (e) => {
    if (e.target.files[0]) {
      document.getElementById('file2Name').innerText = e.target.files[0].name;
      openCropper(e.target.files[0], 2);
    }
  });

  cropSaveBtn.addEventListener('click', () => {
    if (!cropper) return;
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
    } else {
      ctx2.clearRect(0, 0, CARD_W, CARD_H);
      ctx2.drawImage(croppedCanvas, 0, 0);
      img2Loaded = true;
    }

    if (img1Loaded && img2Loaded) {
      addCardBtn.disabled = false;
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

  // Sequentially Append Card to A4 Canvas
  addCardBtn.addEventListener('click', () => {
    if (addedCardsCount >= MAX_CARDS) {
      alert('यह A4 शीट भर चुकी है (अधिकतम 5 कार्ड्स)। कृपया PDF डाउनलोड करें।');
      return;
    }

    const totalWidth = CARD_W * 2;
    const startX = (A4_W - totalWidth) / 2;
    const startY = 50;
    const verticalGap = 50;

    const currentY = startY + (addedCardsCount * (CARD_H + verticalGap));

    // Draw Front Side (Left)
    a4Ctx.drawImage(canvas1, startX, currentY, CARD_W, CARD_H);
    // Draw Back Side (Right - Zero Gap)
    a4Ctx.drawImage(canvas2, startX + CARD_W, currentY, CARD_W, CARD_H);

    // Cutting guide boundary
    a4Ctx.strokeStyle = '#cbd5e1';
    a4Ctx.lineWidth = 2;
    a4Ctx.strokeRect(startX, currentY, totalWidth, CARD_H);

    addedCardsCount++;
    updateCounter();

    downloadPdfBtn.disabled = false;
    clearCurrentInputs();
  });

  resetPageBtn.addEventListener('click', () => {
    if (confirm('क्या आप सच में पूरी A4 शीट को खाली (Reset) करना चाहते हैं?')) {
      resetA4Sheet();
      clearCurrentInputs();
    }
  });

  // Direct A4 PDF Download
  downloadPdfBtn.addEventListener('click', () => {
    const { jsPDF } = window.jspdf;
    const pdf = new jsPDF({
      orientation: 'portrait',
      unit: 'mm',
      format: 'a4'
    });

    const imgData = a4Canvas.toDataURL('image/jpeg', 1.0);
    pdf.addImage(imgData, 'JPEG', 0, 0, 210, 297);
    pdf.save(`A4_Cards_Sheet_${addedCardsCount}_Cards.pdf`);
  });
</script>

</body>
</html>
