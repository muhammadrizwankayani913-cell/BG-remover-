# BG-remover-
<!DOCTYPE html>
<html>
<head>
<script src="https://cdn.jsdelivr.net/npm/@imgly/background-removal@1.3.0/dist/browser.js"></script>
<style>
    body {text-align:center; padding:30px; font-family:Arial}
    button {padding:10px 20px; background:#2196F3; color:white; border:none; border-radius:6px; cursor:pointer}
    img {max-width:400px; margin:10px; border:2px dashed #ccc}
</style>
</head>
<body>

<h2>Remove Background - No API Key</h2>
<input type="file" id="file" accept="image/*">
<button onclick="remove()">Remove BG</button>
<br><br>
<img id="before">
<img id="after">

<script>
async function remove() {
    const file = document.getElementById('file').files[0];
    if(!file) return alert("Select image first");

    document.getElementById('before').src = URL.createObjectURL(file);

    const blob = await window.removeBackground(file);
    const url = URL.createObjectURL(blob);
    document.getElementById('after').src = url;
}
</script>

</body>
</html>
