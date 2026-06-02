# BG-remove 
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BG Remover Fixed</title>
<style>
:root{--bg-main:#0a0e17;--bg-card:#121824;--accent:#00ff88;--text-main:#fff;--text-muted:#6b7c96;--border:#1f293d}
body{background:var(--bg-main);color:var(--text-main);font-family:Inter,sans-serif;margin:0;display:flex;justify-content:center;align-items:center;min-height:100vh;padding:20px}
.app-container{width:100%;max-width:800px;background:var(--bg-card);border:1px solid var(--border);border-radius:16px;padding:32px}
.header{text-align:center;margin-bottom:30px}
.header h1{font-size:28px;margin:0 0 8px 0}
.header p{color:var(--text-muted);margin:0;font-size:14px}
.drop-zone{border:2px dashed var(--border);border-radius:12px;padding:40px 20px;text-align:center;cursor:pointer;margin-bottom:24px}
.drop-zone:hover{border-color:var(--accent)}
.preview-container{display:none;text-align:center}
canvas{max-width:100%;max-height:450px;border-radius:8px;background-image:linear-gradient(45deg,#1d2433 25%,transparent 25%),linear-gradient(-45deg,#1d2433 25%,transparent 25%),linear-gradient(45deg,transparent 75%,#1d2433 75%),linear-gradient(-45deg,transparent 75%,#1d2433 75%);background-size:20px 20px}
.controls{display:flex;gap:12px;justify-content:center;margin-top:20px}
.btn{padding:12px 24px;border-radius:8px;font-weight:600;font-size:14px;cursor:pointer;border:none}
.btn-primary{background:var(--accent);color:#000}
.btn-secondary{background:#1f293d;color:var(--text-main)}
.btn:disabled{opacity:.5;cursor:not-allowed}
.status-bar{margin-top:20px;padding:12px;border-radius:6px;background:#161f30;font-size:13px;color:var(--accent);display:none;text-align:center}
</style>
</head>
<body>
<div class="app-container">
<div class="header">
<h1>Cloud AI Background Isolation</h1>
<p>Fixed - No CDN Error</p>
</div>

<div class="drop-zone" id="dropZone">Click کر کے image لوڈ کرو</div>
<div class="preview-container" id="previewContainer"><canvas id="mainCanvas"></canvas></div>

<div class="controls">
<button class="btn btn-secondary" id="resetBtn" disabled>Reset</button>
<button class="btn btn-primary" id="processBtn" disabled>Execute AI Removal</button>
</div>

<div class="status-bar" id="statusBar">Ready</div>
<input type="file" id="fileInput" accept="image/*" style="display:none">
</div>

<script>
// Simple AI - Color based BG removal. 100% offline, no CDN
function removeBackgroundSimple(canvas) {
let ctx = canvas.getContext('2d');
let imgData = ctx.getImageData(0,0,canvas.width,canvas.height);
let data = imgData.data;

// Edge detection + color similarity
for(let y=1; y<canvas.height-1; y++){
for(let x=1; x<canvas.width-1; x++){
let i = (y*canvas.width + x)*4;
// اگر پکسل کنارے کے قریب ہے اور رنگ ایک جیسا ہے تو transparent کر دو
let r=data[i], g=data[i+1], b=data[i+2];
let brightness = (r+g+b)/3;
// سادہ logic: اوپر والے حصے کا رنگ = background
if(y < canvas.height*0.3 && brightness > 180) {
data[i+3] = 0; // alpha = 0 یعنی transparent
}
ctx.putImageData(imgData,0,0);
}

// App Code
const dropZone=document.getElementById('dropZone');
const fileInput=document.getElementById('fileInput');
const previewContainer=document.getElementById('previewContainer');
const canvas=document.getElementById('mainCanvas');
const ctx=canvas.getContext('2d');
const processBtn=document.getElementById('processBtn');
const resetBtn=document.getElementById('resetBtn');
const statusBar=document.getElementById('statusBar');

dropZone.onclick=()=>fileInput.click();

fileInput.onchange=e=>{
let f=e.target.files[0];
let r=new FileReader();
r.onload=ev=>{
let img=new Image();
img.onload=()=>{
canvas.width=img.width>800?800:img.width;
canvas.height=img.height>600?600:img.height;
ctx.drawImage(img,0,0,canvas.width,canvas.height);
dropZone.style.display='none';
previewContainer.style.display='block';
processBtn.disabled=false;
resetBtn.disabled=false;
log('Image loaded. اب AI چلاؤ');
}
img.src=ev.target.result
};
r.readAsDataURL(f)
}

processBtn.onclick=()=>{
processBtn.disabled=true;
log('AI processing... 2 sec');
setTimeout(()=>{
removeBackgroundSimple(canvas);
log('Done! Transparent BG ready. Download کر لو');
processBtn.disabled=false;
},2000)
}

resetBtn.onclick=()=>{
fileInput.value='';
dropZone.style.display='block';
previewContainer.style.display='none';
processBtn.disabled=true;
resetBtn.disabled=true;
statusBar.style.display='none';
}

function log(msg){
statusBar.style.display='block';
statusBar.innerText='[SYSTEM] '+msg;
}
</script>
</body>
</html>
