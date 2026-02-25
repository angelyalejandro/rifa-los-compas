
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rifa Los Compas</title>

<style>
body{font-family:'Segoe UI',sans-serif;margin:0;background:#e9f7e9;}
.header{background:#27c24c;padding:10px 15px;display:flex;justify-content:space-between;align-items:center;position:sticky;top:0;}
.logo{width:70px;border-radius:50%;}
.menu-btn{font-size:26px;color:white;cursor:pointer;}

.banner{background:linear-gradient(135deg,#ffe082,#80deea);padding:25px 15px;text-align:center;}
.banner h1{font-size:36px;margin:10px 0;color:#0d47a1;}
.banner h2{margin:5px 0;color:#e65100;}
.precio{font-size:28px;color:#d50000;font-weight:bold;}
.btn-banner{margin-top:15px;padding:14px 30px;border:none;border-radius:25px;background:#4caf50;color:white;font-size:18px;cursor:pointer;}

.container{max-width:1000px;margin:auto;padding:20px;}
.card{background:white;border-radius:15px;padding:20px;margin-bottom:20px;}

.restantes{font-weight:bold;margin-bottom:10px;}

.boletos{
display:grid;
grid-template-columns:repeat(auto-fill,minmax(70px,1fr));
gap:10px;
}

.boleto{
padding:14px 6px;
border-radius:12px;
font-weight:bold;
cursor:pointer;
background:#eeeeee;
text-align:center;
white-space:nowrap;
font-size:15px;
box-shadow:0 3px 6px rgba(0,0,0,.15);
transition:.15s;
}

.boleto:active{transform:scale(.9);}
.boleto.seleccionado{background:#00c853;color:white;animation:pop .2s;}
.boleto.vendido{background:#ccc;color:#666;cursor:not-allowed;}

@keyframes pop{50%{transform:scale(1.2);}100%{transform:scale(1);}}

button{margin-top:15px;padding:14px 25px;border:none;border-radius:10px;background:#00c853;color:white;font-size:16px;cursor:pointer;}
</style>
</head>

<body>

<div class="header">
<img src="logo.JPG" class="logo">
<div class="menu-btn">☰</div>
</div>

<div class="banner">

<h1>RIFAS LOS COMPAS</h1>
<div class="precio">COSTO $50</div>
<h2>🥇 1ER LUGAR $8000</h2>
<h2>🥈 2DO LUGAR $3000</h2>
<h2>🥉 3ER LUGAR $1000</h2>
<h2>🏅 4TO LUGAR $500</h2>
<h2>🎖️ 5TO LUGAR $500</h2>

<button class="btn-banner"
onclick="document.getElementById('boletos').scrollIntoView({behavior:'smooth'})">
COMPRAR BOLETO
</button>
</div>

<div class="container">
<div class="card">

<div class="restantes">Boletos disponibles: <span id="restantes">400</span></div>

<div class="boletos" id="boletos"></div>

<div>
Boletos: <span id="cantidad">0</span><br>
Total: $<span id="total">0</span>
</div>

<input type="text" id="nombreCliente" placeholder="Tu nombre completo">
<br>
<button onclick="pagar()">Finalizar Compra</button>

</div>
</div>

<!-- 🔊 SONIDO -->
<audio id="clickSound" src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_7b1b2f3f6e.mp3?filename=click-124467.mp3"></audio>

<script>
const PRECIO_BOLETO=50;
const TOTAL_BOLETOS=400;
const URL_SCRIPT="https://script.google.com/macros/s/AKfycbzTOWYzJCyo8by2wIQVea_okYTKMMmzDyJhNWfAEF4l44J9IRqsw-T6JpRNzP7k6fE/exec";

const contenedor=document.getElementById("boletos");
const seleccionados=new Set();
let vendidos=[];

fetch(URL_SCRIPT)
.then(res=>res.json())
.then(data=>{vendidos=data;generarBoletos();})
.catch(()=>generarBoletos());

function generarBoletos(){
contenedor.innerHTML="";
for(let i=1;i<=TOTAL_BOLETOS;i++){
const num=i.toString().padStart(4,"0");
const div=document.createElement("div");
div.className="boleto";
div.textContent=num;

if(vendidos.includes(num)){
div.classList.add("vendido");
}else{
div.onclick=()=>toggle(num,div);
}

contenedor.appendChild(div);
}
actualizarRestantes();
}

function toggle(num,div){
document.getElementById("clickSound").play();

if(seleccionados.has(num)){
seleccionados.delete(num);
div.classList.remove("seleccionado");
}else{
seleccionados.add(num);
div.classList.add("seleccionado");
}
actualizarResumen();
}

function actualizarResumen(){
document.getElementById("cantidad").textContent=seleccionados.size;
document.getElementById("total").textContent=seleccionados.size*PRECIO_BOLETO;
}

function actualizarRestantes(){
document.getElementById("restantes").textContent=TOTAL_BOLETOS-vendidos.length;
}

function pagar(){
if(seleccionados.size===0)return alert("Selecciona boletos");
const nombre=document.getElementById("nombreCliente").value.trim();
if(nombre==="")return alert("Escribe tu nombre");

const boletosArray=Array.from(seleccionados);

fetch(URL_SCRIPT,{
method:"POST",
body:JSON.stringify({nombre:nombre,boletos:boletosArray})
})
.then(res=>res.json())
.then(data=>{
const extras=data.gratis||[];
const total=data.total || boletosArray.length*PRECIO_BOLETO;

const mensaje =
`🎟️ RIFA LOS COMPAS
🎫 BOLETOS: ${boletosArray.join(", ")}
🎁 GRATIS: ${extras.join(", ")}
💰 TOTAL: $${total}
👤 ${nombre}`;

window.location.href =
"https://api.whatsapp.com/send?phone=527421199270&text="+
encodeURIComponent(mensaje);
});
}
</script>

</body>
</html>
