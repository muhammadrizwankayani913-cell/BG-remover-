# BG-remove 
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Real BG Remover</title>
<style>
body{background:#000;color:#fff;text-align:center;padding:20px;font-family:Arial}
button{padding:15px 30px;margin:10px;background:#00ff88;color:#000;border:none;border-radius:8px;font-size:18px;font-weight:bold;cursor:pointer}
canvas{border:3px solid #00ff88;margin-top:20px;max-width:100%;background:#fff}
input{display:none}
#status{margin:15px;font-size:16px;color:#00ff88}
</style>
</head>
<body>
<h2>Real AI Background Remover</h2>
<button onclick="file.click()">1. Load Photo</button>
<button onclick="removeBG()">2. Remove Background</button>
<button onclick="save()">3. Download PNG</button>
<div id="status">Status: Waiting...</div>
<canvas id="cv"></canvas>
<input type="file" id="file" accept="image/*">

<script src="https://cdn.jsdelivr.net/npm/@imgly/background-removal@1.5.0/dist/bg-removal.js"></script>
<script>
let cv=document.getElementById('cv'),ctx=cv.getContext('2d'),img=null;
let status=document.getElementById('status');

file.onchange=e=>{
let f=e.target.files[0];if(!f)return;
let r=new FileReader();
r.onload=ev=>{
img=new Image();
img.onload=()=>{
cv.width=img.width>800?800:img.width;
cv.height=img.height>600?600:img.height;
ctx.drawImage(img,0,0,cv.width,cv.height);
status.innerText='Status: Image loaded. Click Remove Background';
}
img.src=ev.target.result
};
r.readAsDataURL(f)
}

async function removeBG(){
if(!img){alert('Pehle image load karo');return}
status.innerText='Status: AI processing... 10-15 sec lage ga';
let blob=await removeBackground(img);
let url=URL.createObjectURL(blob);
let i=new Image();
i.onload=()=>{
ctx.clearRect(0,0,cv.width,cv.height);
ctx.drawImage(i,0,0);
status.innerText='Status: Done! Download kar lo';
}
i.src=url;
}

function save(){
let a=document.createElement('a');
a.download='no-bg.png';
a.href=cv.toDataURL('image/png');
a.click();
}
</script>
</body>
</html>
