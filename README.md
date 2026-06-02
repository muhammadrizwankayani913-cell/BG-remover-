# BG-remove
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>Pro Camera Editor Studio v3.0 - AI Powered</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap');
*{margin:0;padding:0;box-sizing:border-box;font-family:'Poppins',Arial}
body{background:linear-gradient(135deg,#0f0c29,#302b63,#24243e);color:#fff;overflow:hidden;height:100vh}
.header{background:rgba(0,0,0,0.7);backdrop-filter:blur(20px);padding:15px 25px;display:flex;align-items:center;justify-content:space-between;border-bottom:2px solid rgba(0,212,255,0.5);box-shadow:0 5px 30px rgba(0,212,255,0.3)}
.logo{font-size:24px;font-weight:700;background:linear-gradient(90deg,#00d4ff,#00ff88);-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.toolbar{background:rgba(20,20,30,0.9);backdrop-filter:blur(15px);padding:12px;display:flex;gap:10px;flex-wrap:wrap;justify-content:center;border-bottom:2px solid rgba(255,255,255,0.1)}
.btn{background:linear-gradient(135deg,#2a2a3a,#1a1a2a);color:#fff;border:2px solid rgba(0,212,255,0.3);padding:12px 20px;border-radius:10px;cursor:pointer;font-size:14px;font-weight:600;transition:all 0.3s;min-width:120px}
.btn:hover{background:linear-gradient(135deg,#3a3a4a,#2a2a3a);border-color:#00d4ff;transform:translateY(-3px);box-shadow:0 8px 20px rgba(0,212,255,0.4)}
.btn.active{background:linear-gradient(135deg,#00d4ff,#00ff88);color:#000;border-color:#00ff88;box-shadow:0 0 20px rgba(0,255,136,0.6)}
.canvas-container{display:flex;justify-content:center;align-items:center;height:calc(100vh - 180px);padding:20px;position:relative}
#canvas{border:4px solid rgba(0,212,255,0.5);border-radius:20px;max-width:100%;max-height:100%;cursor:crosshair;background:#fff;box-shadow:0 0 50px rgba(0,212,255,0.5),inset 0 0 30px rgba(0,0,0,0.3);transition:0.3s}
#canvas:hover{border-color:#00ff88;box-shadow:0 0 60px rgba(0,255,136,0.6)}
#fileInput{display:none}
.loading{position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:rgba(0,0,0,0.95);padding:40px 60px;border-radius:25px;border:3px solid #00d4ff;display:none;z-index:9999;text-align:center;box-shadow:0 0 50px rgba(0,212,255,0.8)}
.loading h2{color:#00d4ff;margin-bottom:15px;font-size:28px;animation:pulse 1.5s infinite}
.loading p{color:#aaa;font-size:18px}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:0.5}}
.side-panel{position:fixed;right:25px;top:180px;background:rgba(20,20,30,0.95);backdrop-filter:blur(20px);padding:25px;border-radius:20px;border:2px solid rgba(0,212,255,0.4);width:280px;box-shadow:0 10px 40px rgba(0,0,0,0.5)}
.side-panel h3{color:#00d4ff;margin-bottom:20px;font-size:20px;border-bottom:2px solid rgba(0,212,255,0.3);padding-bottom:10px}
.slider{margin:20px 0}
.slider label{display:block;margin-bottom:8px;color:#ccc;font-size:15px;font-weight:600}
.slider input{width:100%;height:8px;border-radius:5px;background:#333;outline:none;-webkit-appearance:none}
.slider input::-webkit-slider-thumb{-webkit-appearance:none;width:20px;height:20px;border-radius:50%;background:linear-gradient(135deg,#00d4ff,#00ff88);cursor:pointer;box-shadow:0 0 10px rgba(0,212,255,0.8)}
.status-bar{position:fixed;bottom:20px;left:50%;transform:translateX(-50%);background:rgba(0,0,0,0.8);padding:12px 30px;border-radius:50px;border:2px solid #00d4ff;color:#00d4ff;font-size:15px;font-weight:600;box-shadow:0 5px 25px rgba(0,212,255,0.4)}
.filters{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-top:15px}
.filter-btn{background:#2a2a3a;border:1px solid #444;padding:8px;border-radius:8px;cursor:pointer;font-size:12px;color:#ccc}
.filter-btn:hover{background:#3a3a4a;border-color:#00d4ff;color:#00d4ff}
.footer-info{position:fixed;bottom:10px;right:20px;font-size:12px;color:rgba(255,255,255,0.4)}
@media(max-width:768px){.side-panel{display:none}.btn{min-width:100px;font-size:12px;padding:10px 15px}}
</style>
</head>
<body>
<div class="header">
<div class="logo">🎨 Pro Camera Editor AI</div>
<div style="font-size:14px;color:#00ff88">Version 3.0 Ultimate</div>
</div>

<div class="toolbar">
<button class="btn" onclick="loadImage()">📁 Load Image</button>
<button class="btn" id="aiBtn" onclick="removeBackgroundAI()">🤖 AI Remove BG</button>
<button class="btn" id="brushBtn" onclick="setTool('brush')">🖌️ Erase Brush</button>
<button class="btn" id="restoreBtn" onclick="setTool('restore')">✨ Restore</button>
<button class="btn" id="rectBtn" onclick="setTool('rect')">⬜ Box Select</button>
<button class="btn" onclick="undo()">↩️ Undo</button>
<button class="btn" onclick="redo()">↪️
