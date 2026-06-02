# BG-remove 
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
