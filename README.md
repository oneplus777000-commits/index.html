<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>LIVE PDF EDITOR PORTAL</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700;800&display=swap" rel="stylesheet">
  
  <!-- PDF.js for Reading PDF -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
  
  <!-- jsPDF for PDF Generation -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

  <!-- Fabric.js for Interactive Live Editing -->
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

    /* Auth Box */
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

    .login-btn:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 18px rgba(37, 99, 235, 0.4);
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

    /* Main App */
    #mainApp {
      display: none;
      width: 100%;
      max-width: 1200px;
    }

    .container { 
      background: var(--card-bg); 
      backdrop-filter: blur(16px);
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
      font-size: 24px; 
      font-weight: 700;
      margin-bottom: 8px; 
    }

    .upload-box-pdf {
      border: 2px dashed rgba(56, 189, 248, 0.5);
      background: rgba(15, 23, 42, 0.7);
      padding: 25px 20px;
      border-radius: 14px;
      cursor: pointer;
      max-width: 500px;
      margin: 15px auto;
      transition: 0.3s;
    }

    .upload-box-pdf:hover {
      border-color: var(--accent-blue);
      background: rgba(56, 189, 248, 0.1);
    }

    input[type="file"] { display: none; }

    /* Editor Toolbar */
    .editor-toolbar {
      display: flex;
      gap: 10px;
      justify-content: center;
      align-items: center;
      margin: 15px 0;
      flex-wrap: wrap;
      background: rgba(15, 23, 42, 0.8);
      padding: 12px 15px;
      border-radius: 12px;
      border: 1px solid var(--border-color);
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
      display: flex;
      align-items: center;
      gap: 5px;
    }

    .tool-btn:hover { background: #475569; }
    .tool-btn.active { background: #0284c7; }

    .btn-action {
      padding: 12px 28px;
      font-size: 14px;
      font-weight: 600;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      color: #fff;
      transition: 0.3s;
    }

    .btn-download { background: var(--btn-download); }
    .btn-download:hover { transform: translateY(-2px); box-shadow: 0 6px 20px rgba(16, 185, 129, 0.4); }

    /* Canvas Viewport */
    .canvas-viewport {
      display: inline-block;
      margin: 15px auto;
      border: 2px solid #38bdf8;
      border-radius: 8px;
      background: #fff;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
      max-width: 100%;
      overflow: auto;
    }

    /* Page Navigation */
    .page-nav {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 12px;
      margin-top: 10px;
      font-size: 13px;
      font-weight: 600;
      color: var(--accent-blue);
    }

    .nav-btn {
      padding: 5px 12px;
      background: #334155;
      border: 1px solid rgba(255, 255, 255, 0.1);
      color: #fff;
      border-radius: 6px;
      cursor: pointer;
    }
  </style>
</head>
<body>

<div class="portal-main-heading">
  LIVE PDF EDITOR PORTAL
</div>

<!-- 1. Login Screen -->
<div id="loginScreen" class="auth-box">
  <div class="badge">Protected Access</div>
  <h2 style="font-size: 22px; margin-bottom: 6px;">Sign In</h2>
  <p style="font-size: 12px; color: var(--text-muted); margin-bottom: 20px;">Dedicated PDF Live Editor</p>

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

<!-- 3. Main Live PDF Editor Application -->
<div id="mainApp">
  <div class="container">
    <button id="logoutBtn" class="logout-btn">🔒 Logout</button>
    
    <div class="badge">Live PDF Interactive Editor</div>
    <h1>Live PDF Editor & Exporter</h1>
    <p style="font-size: 12px; color: var(--text-muted); margin-bottom: 10px;">PDF फाइल अपलोड करें, उसमें टेक्स्ट, इमेज, वाइटआउट या साइन जोड़ें और डाउनलोड करें।</p>

    <!-- PDF File Upload Input -->
    <label class="upload-box-pdf" for="pdfFileInput">
      <strong style="display:block; font-size:16px; margin-bottom:4px; color:var(--accent-blue);">📄 Select PDF File to Edit</strong>
      <div id="pdfFileName" style="font-size: 12px; color: var(--text-muted);">क्लिक करके .pdf फाइल चुनें</div>
    </label>
    <input type="file" id="pdfFileInput" accept="application/pdf">

    <!-- Editor Toolbar -->
    <div id="editorToolbarSection" class="editor-toolbar" style="display:none;">
      <button id="addTextToolBtn" class="tool-btn">✍️ Add Text</button>
      
      <label class="tool-btn" for="stampImageInput" style="cursor: pointer; margin-bottom: 0;">
        🖼️ Add Image / Stamp
      </label>
      <input type="file" id="stampImageInput" accept="image/*">

      <button id="addWhiteoutBtn" class="tool-btn">⬜ Whiteout / Eraser Box</button>
      <button id="drawModeBtn" class="tool-btn">✏️ Draw / Sign</button>
      <button id="deleteSelectedBtn" class="tool-btn" style="background: rgba(239, 68, 68, 0.3); color:#fca5a5;">🗑️ Delete</button>
      <button id="clearAnnotationsBtn" class="tool-btn" style="background: rgba(239, 68, 68, 0.2);">🔄 Reset Page</button>
    </div>

    <!-- Live Canvas Container -->
    <div id="canvasViewportWrapper" style="display:none;">
      <div class="canvas-viewport">
        <canvas id="pdfFabricCanvas"></canvas>
      </div>

      <!-- Multi-page controls -->
      <div id="pageNavControls" class="page-nav" style="display:none;">
        <button id="prevPageBtn" class="nav-btn">◀ Prev Page</button>
        <span>Page: <span id="pageNumDisplay">1</span> / <span id="pageCountDisplay">1</span></span>
        <button id="nextPageBtn" class="nav-btn">Next Page ▶</button>
      </div>

      <div style="margin-top: 20px;">
        <button id="downloadEditedPdfBtn" class="btn-action btn-download">📥 Download Edited PDF</button>
      </div>
    </div>

    <footer style="margin-top: 25px; font-size: 12px; color: var(--text-muted);">
      Designed & Developed by <strong>JAYESH BHAVSAR @ 2026 ALL RIGHTS RESERVED</strong>
    </footer>
  </div>
</div>

<script>
  // Set PDF.js Worker
  pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js';

  const AUTH_EMAIL = "oneplus777000@gmail.com";
  const DEFAULT_PASS = "Pass@123";

  function getStoredPassword() {
    return localStorage.getItem('system_auth_pwd') || DEFAULT_PASS;
  }

  // Auth Elements
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
  // LIVE PDF EDITOR ENGINE (PDF.js + Fabric.js)
  // ==========================================
  let pdfDoc = null;
  let currentPageNum = 1;
  let totalPageCount = 1;
  let fabricCanvas = null;
  let currentPdfPageDataURL = null;
  let isDrawing = false;
  let originalViewportWidth = 595;
  let originalViewportHeight = 842;

  const pdfFileInput = document.getElementById('pdfFileInput');
  const pdfFileName = document.getElementById('pdfFileName');
  const editorToolbarSection = document.getElementById('editorToolbarSection');
  const canvasViewportWrapper = document.getElementById('canvasViewportWrapper');
  const pageNavControls = document.getElementById('pageNavControls');
  const pageNumDisplay = document.getElementById('pageNumDisplay');
  const pageCountDisplay = document.getElementById('pageCountDisplay');

  // Handle PDF Selection
  pdfFileInput.addEventListener('change', function(e) {
    const file = e.target.files[0];
    if (!file || file.type !== 'application/pdf') {
      alert('कृपया केवल एक वैध PDF (.pdf) फाइल चुनें!');
      return;
    }

    pdfFileName.innerText = `✅ ${file.name}`;
    const fileReader = new FileReader();

    fileReader.onload = function() {
      const typedarray = new Uint8Array(this.result);
      pdfjsLib.getDocument(typedarray).promise.then(function(pdf) {
        pdfDoc = pdf;
        totalPageCount = pdf.numPages;
        currentPageNum = 1;
        
        pageCountDisplay.innerText = totalPageCount;
        pageNumDisplay.innerText = currentPageNum;

        if (totalPageCount > 1) {
          pageNavControls.style.display = 'flex';
        } else {
          pageNavControls.style.display = 'none';
        }

        editorToolbarSection.style.display = 'flex';
        canvasViewportWrapper.style.display = 'block';

        renderPdfPage(currentPageNum);
      });
    };

    fileReader.readAsArrayBuffer(file);
  });

  // Render Specified PDF Page to Background
  function renderPdfPage(pageNum) {
    if (!pdfDoc) return;

    pdfDoc.getPage(pageNum).then(function(page) {
      // Scale for standard crisp rendering (1.5x scale)
      const viewport = page.getViewport({ scale: 1.5 });
      originalViewportWidth = viewport.width;
      originalViewportHeight = viewport.height;

      const tempCanvas = document.createElement('canvas');
      const tempCtx = tempCanvas.getContext('2d');
      tempCanvas.width = viewport.width;
      tempCanvas.height = viewport.height;

      const renderContext = {
        canvasContext: tempCtx,
        viewport: viewport
      };

      page.render(renderContext).promise.then(function() {
        currentPdfPageDataURL = tempCanvas.toDataURL('image/png');

        if (!fabricCanvas) {
          fabricCanvas = new fabric.Canvas('pdfFabricCanvas', {
            preserveObjectStacking: true
          });
        }

        fabricCanvas.setWidth(viewport.width);
        fabricCanvas.setHeight(viewport.height);

        fabric.Image.fromURL(currentPdfPageDataURL, function(img) {
          img.set({
            selectable: false,
            evented: false,
            originX: 'left',
            originY: 'top'
          });
          fabricCanvas.setBackgroundImage(img, fabricCanvas.renderAll.bind(fabricCanvas));
        });
      });
    });
  }

  // Page Navigators
  document.getElementById('prevPageBtn').addEventListener('click', () => {
    if (currentPageNum <= 1) return;
    currentPageNum--;
    pageNumDisplay.innerText = currentPageNum;
    fabricCanvas.clear();
    renderPdfPage(currentPageNum);
  });

  document.getElementById('nextPageBtn').addEventListener('click', () => {
    if (currentPageNum >= totalPageCount) return;
    currentPageNum++;
    pageNumDisplay.innerText = currentPageNum;
    fabricCanvas.clear();
    renderPdfPage(currentPageNum);
  });

  // 1. Add Text Tool
  document.getElementById('addTextToolBtn').addEventListener('click', () => {
    if (!fabricCanvas) return;
    const text = new fabric.IText('यहाँ टेक्स्ट टाइप करें', {
      left: 100,
      top: 100,
      fontFamily: 'Poppins',
      fontSize: 20,
      fill: '#000000',
      cornerColor: '#38bdf8',
      cornerSize: 8,
      transparentCorners: false
    });
    fabricCanvas.add(text);
    fabricCanvas.setActiveObject(text);
  });

  // 2. Add Stamp / Image Tool
  document.getElementById('stampImageInput').addEventListener('change', function(e) {
    if (!fabricCanvas) return;
    const file = e.target.files[0];
    if (!file) return;

    const reader = new FileReader();
    reader.onload = function(f) {
      fabric.Image.fromURL(f.target.result, function(img) {
        img.scaleToWidth(180);
        img.set({
          left: 100,
          top: 100,
          cornerColor: '#38bdf8',
          cornerSize: 8,
          transparentCorners: false
        });
        fabricCanvas.add(img);
        fabricCanvas.setActiveObject(img);
      });
    };
    reader.readAsDataURL(file);
    this.value = '';
  });

  // 3. Whiteout (Eraser Box) Tool
  document.getElementById('addWhiteoutBtn').addEventListener('click', () => {
    if (!fabricCanvas) return;
    const rect = new fabric.Rect({
      left: 100,
      top: 100,
      width: 150,
      height: 40,
      fill: '#ffffff',
      stroke: '#e2e8f0',
      strokeWidth: 1,
      cornerColor: '#38bdf8',
      cornerSize: 8,
      transparentCorners: false
    });
    fabricCanvas.add(rect);
    fabricCanvas.setActiveObject(rect);
  });

  // 4. Freehand Draw / Sign Mode Toggle
  const drawModeBtn = document.getElementById('drawModeBtn');
  drawModeBtn.addEventListener('click', () => {
    if (!fabricCanvas) return;
    isDrawing = !isDrawing;
    fabricCanvas.isDrawingMode = isDrawing;

    if (isDrawing) {
      drawModeBtn.classList.add('active');
      drawModeBtn.innerText = '🛑 Stop Drawing';
      fabricCanvas.freeDrawingBrush.color = '#000000';
      fabricCanvas.freeDrawingBrush.width = 3;
    } else {
      drawModeBtn.classList.remove('active');
      drawModeBtn.innerText = '✏️ Draw / Sign';
    }
  });

  // 5. Delete Selected Tool
  document.getElementById('deleteSelectedBtn').addEventListener('click', () => {
    if (!fabricCanvas) return;
    const activeObj = fabricCanvas.getActiveObject();
    if (activeObj) {
      fabricCanvas.remove(activeObj);
    }
  });

  // 6. Reset Page Tool
  document.getElementById('clearAnnotationsBtn').addEventListener('click', () => {
    if (!fabricCanvas) return;
    if (confirm('क्या आप इस पेज के सभी जोड़े गए बदलाव मिटाना चाहते हैं?')) {
      fabricCanvas.clear();
      renderPdfPage(currentPageNum);
    }
  });

  // 7. High Resolution Final PDF Download
  document.getElementById('downloadEditedPdfBtn').addEventListener('click', () => {
    if (!fabricCanvas) return;

    fabricCanvas.discardActiveObject();
    fabricCanvas.renderAll();

    // Export Canvas with 2x Multiplier for High Crisp Quality
    const dataURL = fabricCanvas.toDataURL({
      format: 'jpeg',
      quality: 1.0,
      multiplier: 2.0
    });

    const { jsPDF } = window.jspdf;
    
    // Determine orientation based on aspect ratio
    const orientation = originalViewportWidth > originalViewportHeight ? 'landscape' : 'portrait';
    const pdf = new jsPDF({
      orientation: orientation,
      unit: 'pt',
      format: [originalViewportWidth, originalViewportHeight]
    });

    pdf.addImage(dataURL, 'JPEG', 0, 0, originalViewportWidth, originalViewportHeight);
    pdf.save(`Edited_Document_Page_${currentPageNum}.pdf`);
  });
</script>

</body>
</html>
