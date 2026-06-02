# BG-remove 
<!DOCTYPE html>l
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Enterprise BG Remover Pro - Real</title>
<style>
:root{--bg-main:#0a0e17;--bg-card:#121824;--accent:#00ff88;--text-main:#fff;--text-muted:#6b7c96;--border:#1f293d}
body{background:var(--bg-main);color:var(--text-main);font-family:Inter,sans-serif;margin:0;display:flex;justify-content:center;align-items:center;min-height:100vh}
.app-container{width:100%;max-width:800px;background:var(--bg-card);border:1px solid var(--border);border-radius:16px;padding:32px;box-shadow:0 20px 40px rgba(0,0,0,.4)}
.header{text-align:center;margin-bottom:30px}
.header h1{font-size:28px;margin:0 0 8px 0}
.header p{color:var(--text-muted);margin:0;font-size:14px}
.workspace{display:grid;gap:24px;margin-bottom:24px}
.drop-zone{border:2px dashed var(--border);border-radius:12px;padding:40px 20px;text-align:center;cursor:pointer;background:rgba(255,255,255,.01)}
.drop-zone:hover,.drop-zone.drag-over{border-color:var(--accent);background:rgba(0,255,136,.02)}
.preview-container{display:none;width:100%;text-align:center}
canvas{max-width:100%;max-height:450px;border-radius:8px;box-shadow:0 8px 16px rgba(0,0,0,.2);background-image:linear-gradient(45deg,#1d2433 25%,transparent 25%),linear-gradient(-45deg,#1d2433 25%,transparent 25%),linear-gradient(45deg,transparent 75%,#1d2433 75%),linear-gradient(-45deg,transparent 75%,#1d2433 75%);background-size:20px 20px;background-position:0 0,0 10px,10px -10px,-10px 0}
.controls{display:flex;gap:12px;justify-content:center}
.btn{padding:12px 24px;border-radius:8px;font-weight:600;font-size:14px;cursor:pointer;border:none;transition:.2s}
.btn-primary{background:var(--accent);color:#000}
.btn-primary:hover:not(:disabled){background:#00dd76}
.btn-secondary{background:#1f293d;color:var(--text-main)}
.btn:disabled{opacity:.5;cursor:not-allowed}
.status-bar{margin-top:20px;padding:12px;border-radius:6px;background:#161f30;font-size:13px;color:var(--accent);display:none;text-align:center}
</style>
</head>
<body>

<div class="app-container">
<div class="header">
<h1>Cloud AI Background Isolation</h1>
<p>Real async image matting pipeline</p>
</div>

<div class="workspace">
<div class="drop-zone" id="dropZone">
<svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="17 8 12 3 7 8"/><line x1="12" y1="3" x2="12" y2="15"/></svg>
<p style="margin:0;font-size:15px">Drag & drop یا <span style="color:var(--accent)">browse</span> کرو</p>
<p style="margin:4px 0 0 0;font-size:12px;color:var(--text-muted)">PNG, JPG تک 10MB</p>
</div>

<div class="preview-container" id="previewContainer">
<canvas id="mainCanvas"></canvas>
</div>
</div>

<div class="controls">
<button class="btn btn-secondary" id="resetBtn" disabled>Reset</button>
<button class="btn btn-primary" id="processBtn" disabled>Execute AI Removal</button>
</div>

<div class="status-bar" id="statusBar">Ready</div>
<input type="file" id="fileInput" accept="image/*" style="display:none">
</div>

<!-- اصل AI Library لگ گئی بھائی -->
<script src="https://cdn.jsdelivr.net/npm/@imgly/background-removal@1.5.0/dist/bg-removal.js"></script>
<script>
const App={
state:{img:null},
dom:{
dropZone:document.getElementById('dropZone'),
fileInput:document.getElementById('fileInput'),
previewContainer:document.getElementById('previewContainer'),
canvas:document.getElementById('mainCanvas'),
ctx:document.getElementById('mainCanvas').getContext('2d'),
processBtn:document.getElementById('processBtn'),
resetBtn:document.getElementById('resetBtn'),
statusBar:document.getElementById('statusBar')
},
init(){
this.dom.dropZone.onclick=()=>this.dom.fileInput.click();
this.dom.fileInput.onchange=e=>this.loadFile(e.target.files[0]);
this.dom.resetBtn.onclick=()=>this.reset();
this.dom.processBtn.onclick=()=>this.removeBG();
['dragenter','dragover'].forEach(n=>this.dom.dropZone.addEventListener(n,e=>{e.preventDefault();this.dom.dropZone.classList.add('drag-over')}));
['dragleave','drop'].forEach(n=>this.dom.dropZone.addEventListener(n,e=>{e.preventDefault();this.dom.dropZone.classList.remove('drag-over')}));
this.dom.dropZone.addEventListener('drop',e=>this.loadFile(e.dataTransfer.files[0]));
},
log(msg,err=0){this.dom.statusBar.style.display='block';this.dom.statusBar.style.color=err?'#ff4a4a':'var(--accent)';this.dom.statusBar.innerText='[SYSTEM] '+msg},
loadFile(file){
if(!file||!file.type.startsWith('image/'))return this.log('Invalid file',1);
let r=new FileReader();
r.onload=e=>{let i=new Image();i.onload=()=>{this.dom.canvas.width=i.width>800?800:i.width;this.dom.canvas.height=i.height>600?600:i.height;this.dom.ctx.drawImage(i,0,0);this.dom.dropZone.style.display='none';this.dom.previewContainer.style.display='block';this.dom.processBtn.disabled=0;this.dom.resetBtn.disabled=0;this.log('Image loaded. Ready for AI');this.state.img=i};i.src=e.target.result};
r.readAsDataURL(file)
},
async removeBG(){
if(!this.state.img)return;
this.dom.processBtn.disabled=1;
this.log('AI processing... 10-15 seconds');
try{
let blob=await removeBackground(this.state.img);
let url=URL.createObjectURL(blob);
let i=new Image();
i.onload=()=>{this.dom.ctx.clearRect(0,0,this.dom.canvas.width,this.dom.canvas.height);this.dom.ctx.drawImage(i,0,0);this.log('Done! Transparent BG ready');this.dom.processBtn.disabled=0};
i.src=url
}catch(e){this.log('Error: '+e.message,1);this.dom.processBtn.disabled=0}
},
reset(){
this.state.img=null;this.dom.fileInput.value='';this.dom.dropZone.style.display='block';this.dom.previewContainer.style.display='none';this.dom.processBtn.disabled=1;this.dom.resetBtn.disabled=1;this.dom.statusBar.style.display='none'
}
};
document.addEventListener('DOMContentLoaded',()=>App.init());
</script>
</body>
</html>
