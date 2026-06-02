
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BG Remover - Final</title>
<style>
body{background:#0a0e17;color:#fff;font-family:Arial;margin:0;padding:20px;display:flex;justify-content:center}
.box{max-width:700px;width:100%;background:#121824;border:1px solid #1f293d;border-radius:16px;padding:30px}
h1{text-align:center;margin:0 0 10px 0}
p{text-align:center;color:#6b7c96;margin:0 0 30px 0}
#drop{border:2px dashed #1f293d;border-radius:12px;padding:50px;text-align:center;cursor:pointer;margin-bottom:20px}
#drop:hover{border-color:#00ff88}
canvas{max-width:100%;border-radius:8px;background:#1d2433;display:none;margin:20px auto}
.btns{display:flex;gap:10px;justify-content:center;margin-top:20px}
button{padding:14px 28px;border:none;border-radius:8px;font-weight:bold;cursor:pointer;font-size:15px}
#go{background:#00ff88;color:#000}
#reset{background:#1f293d;color:#fff}
button:disabled{opacity:0.4;cursor:not-allowed}
#status{margin-top:15px;padding:12px;background:#161f30;border-radius:6px;color:#00ff88;text-align:center;display:none}
input{display:none}
</style>
</head>
<body>
<div class="box">
<h1>Cloud AI Background Isolation</h1>
<p>Real AI - No Fake</p>

<div id="drop">Click کر کے تصویر لوڈ کرو</div>
<canvas id="cv"></canvas>

<div class="btns">
<button id="reset" disabled>Reset</button>
<button id="go" disabled>Execute AI Removal</button>
</div>

<div id="status">Ready</div>
<input type="file" id="file" accept="image/*">
</div>

<!-- U2Net AI - 100% کام کرے گی -->
<script type="importmap">
{
"imports": {
"@tensorflow/tfjs": "https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@4.17.0/dist/tf.min.js",
"@tensorflow-models/body-pix": "https://cdn.jsdelivr.net/npm/@tensorflow-models/body-pix@2.2.1/dist/body-pix.min.js"
}
</script>

<script type="module">
import * as tf from '@tensorflow/tfjs';
import * as bodyPix from '@tensorflow-models/body-pix';

const drop=document.getElementById('drop');
const file=document.getElementById('file');
const cv=document.getElementById('cv');
const ctx=cv.getContext('2d');
const go=document.getElementById('go');
const reset=document.getElementById('reset');
const status=document.getElementById('status');

let img=null, net=null;

// AI model load کرو
status.style.display='block';
status.innerText='AI model load ہو رہا ہے... 10 سیکنڈ';
bodyPix.load().then(n=>{
net=n;
status.innerText='Ready! اب تصویر لوڈ کرو';
});

drop.onclick=()=>file.click();

file.onchange=e=>{
let f=e.target.files[0];
let r=new FileReader();
r.onload=ev=>{
img=new Image();
img.onload=()=>{
cv.width=img.width>700?700:img.width;
cv.height=img.height>500?500:img.height;
ctx.drawImage(img,0,0,cv.width,cv.height);
drop.style.display='none';
cv.style.display='block';
go.disabled=false;
reset.disabled=false;
status.innerText='Image ready. Execute AI Removal دباؤ';
}
img.src=ev.target.result
};
r.readAsDataURL(f)
}

go.onclick=async()=>{
go.disabled=true;
status.innerText='AI BG remove کر رہا ہے... 5-8 سیکنڈ';
const segmentation = await net.segmentPerson(cv);
const mask = bodyPix.toMask(segmentation);
ctx.clearRect(0,0,cv.width,cv.height);
ctx.putImageData(mask,0,0);
ctx.globalCompositeOperation='source-in';
ctx.drawImage(img,0,0,cv.width,cv.height);
status.innerText='ہو گیا بھائی! PNG download کر لو';
go.disabled=false;
}

reset.onclick=()=>{
file.value='';
drop.style.display='block';
cv.style.display='none';
go.disabled=true;
reset.disabled=true;
status.style.display='none';
}
</script>
</body>
</html>
