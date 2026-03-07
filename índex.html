<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>RIFAS LOS COMPAS</title>

<style>

body{
font-family:'Segoe UI',sans-serif;
margin:0;
background:#e9f7e9;
}

.menu{
display:flex;
justify-content:center;
gap:10px;
padding:15px;
background:#0d47a1;
}

.menu button{
background:white;
color:#0d47a1;
font-weight:bold;
border:none;
padding:10px 20px;
border-radius:8px;
cursor:pointer;
}

.banner{
background:linear-gradient(135deg,#ffe082,#80deea);
padding:25px;
text-align:center;
}

.banner h1{
font-size:34px;
margin:10px 0;
color:#0d47a1;
}

.banner img{
max-width:200px;
display:block;
margin:auto;
}

.container{
max-width:1100px;
margin:auto;
padding:20px;
}

.card{
background:white;
border-radius:15px;
padding:20px;
}

.player{
width:100%;
border-radius:15px;
margin-bottom:20px;
box-shadow:0 4px 10px rgba(0,0,0,.2);
}

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
font-size:15px;
}

.boleto.seleccionado{
background:#00c853;
color:white;
}

.boleto.vendido{
background:#ccc;
color:#666;
cursor:not-allowed;
}

button{
margin-top:15px;
padding:14px 25px;
border:none;
border-radius:10px;
background:#00c853;
color:white;
font-size:16px;
cursor:pointer;
}

button:disabled{
background:gray;
}

</style>
</head>

<body>

<nav class="menu">
<button onclick="mostrarSeccion('rifa')">Inicio</button>
<button onclick="mostrarSeccion('pagos')">Formas de Pago</button>
</nav>

<div id="rifa" class="seccion">

<div class="banner">
<img src="https://raw.githubusercontent.com/angelyalejandro/rifa-los-compas/main/logo.JPG">
<h1>RIFAS LOS COMPAS</h1>
</div>

<div class="container">
<div class="card">

<img class="player" src="https://raw.githubusercontent.com/angelyalejandro/rifa-los-compas/main/flayer.jpeg">

<div class="boletos" id="boletos"></div>

<div>
Boletos: <span id="cantidad">0</span><br>
Total: $<span id="total">0</span>
</div>

<input type="text" id="nombreCliente" placeholder="Tu nombre completo" style="width:100%;padding:10px;margin-top:10px;">

<button id="btnPagar" onclick="pagar()">Finalizar Compra</button>

</div>
</div>
</div>

<div id="pagos" class="seccion" style="display:none;">
<div class="container">
<div class="card" style="text-align:center;background:#0f6c6c;color:white;">

<h1>VENDEDORES AUTORIZADOS</h1>

<h2>Luis Alejandro Romero Sebastian</h2>
<p>📱 7421199270</p>

<hr>

<h2>Angel Gabriel Urioste Luciano</h2>
<p>📱 7421292436</p>

</div>
</div>
</div>

<script>

const PRECIO = 50;
const TOTAL_BOLETOS = 400;

const TELEFONO = "527421199270";

const URL_SCRIPT = "https://script.google.com/macros/s/AKfycbwWtSHDck7DhkXeE4afOPilmsbUfCIfT_kR2CN9uMZttIrvLi-6k5MWvW5b4qYVUns5/exec";

const contenedor = document.getElementById("boletos");

let seleccionados = new Set();
let vendidos = [];

function mostrarSeccion(id){
document.querySelectorAll(".seccion").forEach(s=>s.style.display="none");
document.getElementById(id).style.display="block";
}

function generarBoletos(){

contenedor.innerHTML="";

for(let i=1;i<=TOTAL_BOLETOS;i++){

const num=i.toString().padStart(4,"0");

const div=document.createElement("div");

div.textContent=num;

div.className="boleto";

if(vendidos.includes(num)){
div.classList.add("vendido");
}else{

if(seleccionados.has(num)) div.classList.add("seleccionado");

div.onclick=()=>toggleBoleto(num);

}

contenedor.appendChild(div);

}

}

function toggleBoleto(num){

if(vendidos.includes(num)) return;

if(seleccionados.has(num)){
seleccionados.delete(num);
}else{
seleccionados.add(num);
}

actualizarResumen();
generarBoletos();

}

function actualizarResumen(){

document.getElementById("cantidad").textContent=seleccionados.size;

document.getElementById("total").textContent=seleccionados.size*PRECIO;

}

function cargarVendidos(){

fetch(URL_SCRIPT+"?t="+Date.now())

.then(r=>r.json())

.then(data=>{

vendidos=data.vendidos || [];

generarBoletos();

})

.catch(()=>{

generarBoletos();

});

}

function pagar(){

if(seleccionados.size===0){
alert("Selecciona boletos");
return;
}

const nombre=document.getElementById("nombreCliente").value.trim();

if(nombre===""){
alert("Escribe tu nombre");
return;
}

const boletos=Array.from(seleccionados);

const mensaje=`Hola quiero apartar boletos%0A%0ANombre: ${nombre}%0ABoletos: ${boletos.join(", ")}%0ATotal: $${boletos.length*PRECIO}`;

window.open(`https://wa.me/${TELEFONO}?text=${mensaje}`);

fetch(URL_SCRIPT,{
method:"POST",
body:JSON.stringify({nombre,boletos})
});

seleccionados.clear();

actualizarResumen();

setTimeout(cargarVendidos,2000);

}

generarBoletos();

cargarVendidos();

setInterval(cargarVendidos,10000);

</script>

</body>
</html>
