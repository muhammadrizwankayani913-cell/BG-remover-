# BG-remove 
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>VIP Pro Photo Editor Studio v4.0 - Works 100%</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@500;700&display=swap');
*{margin:0;padding:0;box-sizing:border-box;font-family:'Poppins',sans-serif;-webkit-tap-highlight-color:transparent}
body{background:#0d0d1a;color:#fff;overflow:hidden;height:100vh}
.header{background:linear-gradient(90deg,#1a1a2e,#16213e);padding:15px;display:flex;justify-content:space-between;align-items:center;border-bottom:3px solid #00d4ff;box-shadow:0 5px 25px rgba(0,212,255,0.5)}
.logo{font-size:22px;font-weight:700;background:linear-gradient(90deg,#00d4ff,#00ff88);-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.toolbar{background:rgba(15,15,30,0.98);padding:12px;display:flex;gap:8px;overflow-x:auto;border-bottom:2px solid #333}
.toolbar::-webkit-scrollbar{height:4px;background:#222}
.toolbar::-webkit-scrollbar-thumb{background:#00d4ff;border-radius:10px}
.btn{flex:0 0 auto;background:linear-gradient(135deg,#2d2d44,#1f1f33);color:#fff;border:2px solid #3a3a5a;padding:12px 18px;border-radius:12px;cursor:pointer;font-size:14px;font-weight:600;transition:0.3s;white-space:nowrap}
.btn:active{transform:scale(0.95)}
.btn.active{background:linear-gradient(135deg,#00d4ff,#0099cc);color:#000;border-color:#00ff88;box-shadow:0 0 20px rgba(0,255,136,0.7)}
.canvas-area{display:flex;justify-content:center;align-items:center;height:calc(100vh - 175px);background:#000;position:relative;overflow:auto}
#mainCanvas,#maskCanvas{border:4px solid #00d4ff;border-radius:20px;max-width:95%;max-height:95%;box-shadow:0 0 40px rgba(0,212,255,0.6);touch-action:none}
#maskCanvas{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);pointer-events:none;opacity:0.4}
#fileInput{display:none}
.loading{position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:rgba(0,0,0,0.95);padding:35px 50px;border-radius:25px;border:3px solid #00d4ff;z-index:9999;text-align:center;display:none;box-shadow:0 0 50px rgba(0,212,255,0.8)}
.loading h2{color:#00d4ff;font-size:26px;margin-bottom:10px;animation:blink 1s infinite}
@keyframes blink{50%{opacity:0.5}}
.panel{position:fixed;right:20px;top:170px;background:rgba(20,20,40,0.98);padding:20px;border-radius:18px;border:2px solid #00d4ff;width:260px;box-shadow:0 10px 40px rgba(0,0,0,0.7)}
.panel h3{color:#00d4ff;margin-bottom:15px;font-size:18px;border-bottom:2px solid #333;padding-bottom:8px}
.control{margin:15px 0}
.control label{display:block;color:#ccc;margin-bottom:6px;font-size:14px;font-weight:600}
.control input[type=range]{width:100%;height:6px;background:#333;border-radius:5px;outline:none}
.control input[type=range]::-webkit-slider-thumb{width:18px;height:18px;background:linear-gradient(135deg,#00d4ff,#00ff88);border-radius:50%;cursor:pointer}
.status{position:fixed;bottom:15px;left:50%;transform:translateX(-50%);background:rgba(0,0,0,0.9);padding:12px 30px;border-radius:50px;border:2px solid #00d4ff;color:#00ff88;font-weight:600;font-size:15px;z-index:100}
.toast{position:fixed;top:80px;left:50%;transform:translateX(-50%);background:linear-gradient(90deg,#00ff88,#00d4ff);color:#000;padding:12px 25px;border-radius:50px;font-weight:700;display:none;z-index:9999;box-shadow:0 5px 25px rgba(0,255,136,0.6)}
@media(max-width:768px){.panel{display:none}}
</style>
</head>
<body>
<div class="header">
<div class="logo">🎨 VIP Pro Editor v4.0</div>
<div style="color:#00ff88;font-size:13px">100% Working</div>
</div>

<div class="toolbar">
<button class="btn" onclick="loadImage()">📁 Load Photo</button>
<button class="btn" id="aiBtn" onclick="removeBG()">🤖 AI Remove BG</button>
<button class="btn" id="eraseBtn" onclick="setTool('erase')">🖌️ Erase Tool</button>
<button class="btn" id="paintBtn" onclick="setTool('paint')">🖊️ Paint Back</button>
<button class="btn" onclick="undo()">↩️ Undo</button>
<button class="btn" onclick="redo()">↪️ Redo</button>
<button class="btn" onclick="zoomIn()">🔍 Zoom +</button>
<button class="btn" onclick="zoomOut()">🔍 Zoom -</button>
<button class="btn" style="background:linear-gradient(135deg,#00ff88,#00d4ff);color:#000" onclick="exportHD()">💾 Export HD</button>
</div>

<div class="canvas-area">
<canvas id="mainCanvas"></canvas>
<canvas id="maskCanvas"></canvas>
<input type="file" id="fileInput" accept="image/*">
</div>

<div class="panel">
<h3>⚙️ Brush Controls</h3>
<div class="control">
<label>Size: <span id="sizeVal">40</span>px</label>
<input type="range" id="brushSize" min="10" max="200" value="40" oninput="updateSize(this.value)">
</div>
<div class="control">
<label>Strength: <span id="strengthVal">100</span>%</label>
<input type="range" id="strength" min="20" max="100" value="100" oninput="updateStrength(this.value)">
</div>
<h3 style="margin-top:20px">📊 Live Info</h3>
<p id="infoText" style="color:#aaa;font-size:13px;line-height:1.7">Load image to start<br>Touch & drag to edit</p>
</div>

<div class="loading" id="loading">
<h2>🤖 AI Processing...</h2>
<p style="color:#aaa">Neural Network Working...</p>
</div>

<div class="toast" id="toast"></div>
<div class="status" id="status">🚀 System Ready - VIP Mode Active</div>

<script src="https://cdn.jsdelivr.net/npm/@imgly/background-removal@1.5.0/dist/background-removal.js"></script>
<script>
const mainCanvas = document.getElementById('mainCanvas');
const maskCanvas = document.getElementById('maskCanvas');
const mainCtx = mainCanvas.getContext('2d');
const maskCtx = maskCanvas.getContext('2d');
const fileInput = document.getElementById('fileInput');
const loading = document.getElementById('loading');
const status = document.getElementById('status');
const infoText = document.getElementById('infoText');
const toast = document.getElementById('toast');

let originalImg = null;
let currentTool = 'erase';
let brushSize = 40;
let strength = 100;
let isDrawing = false;
let history = [];
let historyIndex = -1;
let zoom = 1;

// Touch + Mouse support for mobile
mainCanvas.addEventListener('mousedown', startDraw);
mainCanvas.addEventListener('mousemove', draw);
mainCanvas.addEventListener('mouseup', stopDraw);
mainCanvas.addEventListener('mouseleave', stopDraw);

mainCanvas.addEventListener('touchstart', e => {e.preventDefault(); startDraw(e.touches[0])});
mainCanvas.addEventListener('touchmove', e => {e.preventDefault(); draw(e.touches[0])});
mainCanvas.addEventListener('touchend', stopDraw);

function loadImage(){
    fileInput.click();
    showToast('📂 Select high quality image');
}

fileInput.onchange = async function(e){
    const file = e.target.files[0];
    if(!file) return;
    showLoading(true);
    updateStatus('📥 Loading image...');

    const img = await loadImageFile(file);
    originalImg = img;

    mainCanvas.width = maskCanvas.width = img.width;
    mainCanvas.height = maskCanvas.height = img.height;

    mainCtx.clearRect(0,0,mainCanvas.width,mainCanvas.height);
    mainCtx.drawImage(img,0,0);

    maskCtx.clearRect(0,0,maskCanvas.width,maskCanvas.height);

    saveHistory();
    showLoading(false);
    updateStatus('✅ Image loaded! Use Erase Tool or AI Remove BG');

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pro BG Remover - Fixed</title>
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:Arial}
body{background:#111;color:#fff}
.header{background:#1a1a1a;padding:15px;text-align:center;border-bottom:2px solid #00d4ff}
.header h1{color:#00d4ff;font-size:22px}
.toolbar{padding:15px;display:flex;gap:10px;justify-content:center;flex-wrap:wrap;background:#222}
button{background:#333;color:#fff;border:2px solid #00d4ff;padding:12px 25px;border-radius:8px;cursor:pointer;font-size:16px;font-weight:bold}
button:hover{background:#444;border-color:#00ff88}
button:active{transform:scale(0.95)}
.canvas-box{display:flex;justify-content:center;padding:20px;min-height:400px;background:#000}
#canvas{border:3px solid #00d4ff;border-radius:15px;max-width:100%;max-height:500px;background:#fff;cursor:crosshair}
#fileInput{display:none}
.loading{position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:rgba(0,0,0,0.9);padding:30px;border-radius:15px;border:2px solid #00d4ff;display:none;z-index:999}
.loading h2{color:#00d4ff;text-align:center}
.status{position:fixed;bottom:10px;left:50%;transform:translateX(-50%);background:#000;padding:10px 25px;border-radius:50px;border:2px solid #00ff88;color:#00ff88;font-weight:bold}
</style>
</head>
<body>
<div class="header">
<h1>🎨 Pro BG Remover v5.0 - FIXED</h1>
</div>

<div class="toolbar">
<button onclick="document.getElementById('fileInput').click()">📁 Load Photo</button>
<button onclick="removeBG()">🤖 AI Remove BG</button>
<button onclick="setTool('erase')">🖌️ Erase Brush</button>
<button onclick="download()">💾 Save PNG</button>
</div>

<div class="canvas-box">
<canvas id="canvas" width="800" height="600"></canvas>
<input type="file" id="fileInput" accept="image/*">
</div>

<div class="loading" id="loading">
<h2>⏳ AI Working...<br>Please wait 5 seconds</h2>
</div>

<div class="status" id="status">✅ Ready - Click Load Photo</div>

<script src="https://cdn.jsdelivr.net/npm/@imgly/background-removal@1.5.0/dist/background-removal.js"></script>
<script>
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');
const fileInput = document.getElementById('fileInput');
const loading = document.getElementById('loading');
const status = document.getElementById('status');

let originalImg = null;
let isDrawing = false;
let tool = 'erase';

// File load
fileInput.onchange = function(e){
    const file = e.target.files[0];
    if(!file){alert('File select karo bhai');return;}

    status.innerText = '📥 Loading image...';
    const reader = new FileReader();
    reader.onload = function(event){
        const img = new Image();
        img.onload = function(){
            canvas.width = img.width > 800? 800 : img.width;
            canvas.height = img.height > 600? 600 : img.height;
            ctx.clearRect(0,0,canvas.width,canvas.height);
            ctx.drawImage(img,0,0,canvas.width,canvas.height);
            originalImg = img;
            status.innerText = '✅ Image loaded! Now click AI Remove BG';
            alert('Image loaded successfully!');
        }
        img.src = event.target.result;
    }
    reader.readAsDataURL(file);
}

// AI Remove Background
async function removeBG(){
    if(!originalImg){
        alert('⚠️ Pehle Load Photo dabao aur image select karo!');
        return;
    }
    loading.style.display = 'block';
    status.innerText = '🤖 AI processing... 5 seconds wait';

    try{
        const blob = await removeBackground(originalImg);
        const imgURL = URL.createObjectURL(blob);
        const img = new Image();
        img.onload = function(){
            ctx.clearRect(0,0,canvas.width,canvas.height);
            ctx.drawImage(img,0,0,canvas.width,canvas.height);
            loading.style.display = 'none';
            status.innerText = '✅ Background removed! Now use Erase Brush';
            alert('Background removed successfully!');
        }
        img.src = imgURL;
    }catch(err){
        loading.style.display = 'none';
        alert('Error: '+err.message);
        status.innerText = '❌ Error occurred';
    }
}

// Simple Erase Brush - 100% working method
canvas.onmousedown = function(e){isDrawing=true; erase(e);}
canvas.onmousemove = function(e){if(isDrawing) erase(e);}
canvas.onmouseup = function(){isDrawing=false;}
canvas.ontouchstart = function(e){e.preventDefault(); isDrawing=true; erase(e.touches[0]);}
canvas.ontouchmove = function(e){e.preventDefault(); if(isDrawing) erase(e.touches[0]);}
canvas.ontouchend = function(){isDrawing=false;}

function erase(e){
    if(!isDrawing) return;
    const rect = canvas.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const y = e.clientY - rect.top;

    ctx.globalCompositeOperation = 'destination-out';
    ctx.beginPath();
    ctx.arc(x, y, 30, 0, Math.PI*2);
    ctx.fill();
    ctx.globalCompositeOperation = 'source-over';
}

function setTool(t){tool=t; status.innerText='🖌️ Erase mode ON - Drag on image to remove';}

function download(){
    const link = document.createElement('a');
    link.download = 'edited-image.png';
    link.href = canvas.toDataURL();
    link.click();
    alert('Image downloaded!');
}
</script>
</bo
