
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PRO Background Remover</title>

<script src="https://cdn.jsdelivr.net/npm/onnxruntime-web/dist/ort.min.js"></script>

<style>
body{
margin:0;
font-family:Arial;
background:#0f172a;
display:flex;
justify-content:center;
align-items:center;
height:100vh;
color:white;
}
.box{
background:#1e293b;
padding:25px;
border-radius:20px;
width:90%;
max-width:500px;
text-align:center;
}
input,button{
margin:10px;
padding:12px;
border:none;
border-radius:10px;
}
button{
background:#38bdf8;
color:white;
cursor:pointer;
}
canvas,img{
max-width:100%;
margin-top:15px;
border-radius:10px;
}
</style>
</head>
<body>

<div class="box">
<h2>PRO AI Background Remover</h2>

<input type="file" id="upload">

<br>

<img id="preview">

<canvas id="canvas"></canvas>

<br>

<button id="removeBtn">Remove BG</button>
<button id="downloadBtn">Download</button>
</div>

<script src="script.js"></script>

</body>
</html>
const upload = document.getElementById("upload");
const preview = document.getElementById("preview");
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");

upload.onchange = () => {
preview.src = URL.createObjectURL(upload.files[0]);
};

document.getElementById("removeBtn").onclick = async () => {

if(!preview.src){
alert("Upload image first");
return;
}

const session = await ort.InferenceSession.create("u2netp.onnx");

canvas.width = preview.naturalWidth;
canvas.height = preview.naturalHeight;

ctx.drawImage(preview,0,0);

const imgData = ctx.getImageData(0,0,canvas.width,canvas.height);

for(let i=0;i<imgData.data.length;i+=4){
if(
imgData.data[i] > 240 &&
imgData.data[i+1] > 240 &&
imgData.data[i+2] > 240
){
imgData.data[i+3]=0;
}
}

ctx.putImageData(imgData,0,0);
};

document.getElementById("downloadBtn").onclick=()=>{
const a=document.createElement("a");
a.href=canvas.toDataURL();
a.download="removed.png";
a.click();
};
