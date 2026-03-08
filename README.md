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
text-align:center;
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
}

.banner h1{
font-size:34px;
color:#0d47a1;
margin:10px 0;
}

.banner img{
max-width:180px;
border-radius:50%;
box-shadow:0 4px 8px rgba(0,0,0,0.2);
}

.container{
max-width:1000px;
margin:auto;
padding:20px;
}

.card{
background:white;
border-radius:15px;
padding:20px;
box-shadow:0 4px 15px rgba(0,0,0,0.1);
}

.boletos{
display:grid;
grid-template-columns:repeat(auto-fill,minmax(65px,1fr));
gap:8px;
margin-top:20px;
}

.boleto{
padding:12px 2px;
border-radius:8px;
font-weight:bold;
background:#eeeeee;
cursor:pointer;
font-size:14px;
border:1px solid #ddd;
color:#333;
}

.boleto.seleccionado{
background:#00c853 !important;
color:white !important;
}

.boleto.vendido{
background:#ff0000 !important;
color:white !important;
cursor:not-allowed;
text-decoration:line-through;
opacity:0.8;
}

.resumen{
background:#f1f8e9;
padding:15px;
margin:20px 0;
border-radius:10px;
border:1px solid #c8e6c9;
}

#btnPagar{
width:100%;
padding:15px;
background:#00c853;
color:white;
border:none;
border-radius:10px;
font-size:18px;
font-weight:bold;
cursor:pointer;
}

input[type="text"]{
width:95%;
padding:12px;
border-radius:8px;
border:1px solid #ccc;
font-size:16px;
margin-top:10px;
box-sizing:border-box;
}

</style>
</head>

<body>

<nav class="menu">
<button onclick="cargarVendidos()">🔄 Actualizar Lista</button>
</nav>

<div class="banner">

<img src="https://raw.githubusercontent.com/angelyalejandro/rifa-los-compas/main/logo.JPG">

<h1>RIFAS LOS COMPAS</h1>

<button id="btnFormasPago" style="
margin-top:10px;
padding:10px 20px;
font-size:16px;
background:#0d47a1;
color:white;
border:none;
border-radius:8px;
cursor:pointer;
">
Ver Formas de Pago
</button>

<div id="formasPagoTop" style="margin-top:10px;font-size:16px;color:#0d47a1;display:none;"></div>

</div>

<div class="container">

<div class="card">

<img style="width:100%;border-radius:10px;margin-bottom:15px;" src="https://raw.githubusercontent.com/angelyalejandro/rifa-los-compas/main/flayer.jpeg">

<div id="boletos" class="boletos">Cargando boletos...</div>

<div class="resumen">

<p style="font-size:18px;">
Seleccionados: <b id="cantidad">0</b> |
Total: $<b id="total">0</b>
</p>

<input type="text" id="nombreCliente" placeholder="Nombre completo del cliente">

</div>

<button id="btnPagar" onclick="pagar()">Finalizar y Enviar WhatsApp</button>

</div>

</div>

<script>

const PRECIO = 50;
const TOTAL_BOLETOS = 400;
const TELEFONO = "5217421199270";

const URL_LECTURA="https://docs.google.com/spreadsheets/d/e/2PACX-1vQPcFnbquGADSgLL6WHeSYdCyl6aCa3VlouguKC57RIxAf0yYbM1HifCC10fgcMnpFwWmv8FVsnQrxU/pub?gid=1689723674&single=true&output=csv";

const URL_SCRIPT="https://script.google.com/macros/s/AKfycbwhCERlRzgSf780cJqVvYN6kMraG-_mCDy8YaRWqwHjXQkgtpM6opJrJqRfmWuX_Vya5g/exec";

let seleccionados=new Set();
let vendidos=[];

async function cargarVendidos(){

try{

const res=await fetch(URL_LECTURA+"&t="+Date.now());
const csv=await res.text();

const filas=csv.split(/\r?\n/);

vendidos=[];

for(let i=1;i<filas.length;i++){

const columnas=filas[i].split(",");

if(columnas.length>=2){

const numero=columnas[0].trim().padStart(4,"0");
const estado=columnas[1].trim().toUpperCase();

if(estado==="VENDIDO") vendidos.push(numero);

}

}

generarVisual();

}catch(e){

console.error(e);
generarVisual();

}

}

function generarVisual(){

const contenedor=document.getElementById("boletos");

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

div.onclick=()=>{

if(seleccionados.has(num)) seleccionados.delete(num);
else seleccionados.add(num);

actualizarCifras();
generarVisual();

};

}

contenedor.appendChild(div);

}

}

function actualizarCifras(){

document.getElementById("cantidad").textContent=seleccionados.size;
document.getElementById("total").textContent=seleccionados.size*PRECIO;

}

async function pagar(){

const nombre=document.getElementById("nombreCliente").value.trim();

if(seleccionados.size===0) return alert("Selecciona boletos");
if(!nombre) return alert("Escribe tu nombre");

const btn=document.getElementById("btnPagar");

btn.disabled=true;
btn.textContent="Procesando...";

const arrayBol=Array.from(seleccionados);

let regalos=[];
let total=arrayBol.length*PRECIO;

try{

const urlRegistro=`${URL_SCRIPT}?nombre=${encodeURIComponent(nombre)}&boletos=${arrayBol.join(",")}`;

const res=await fetch(urlRegistro);

const resultado=await res.json();

if(resultado.regalos && resultado.regalos.length>0){

regalos=resultado.regalos;

}

}catch(e){

console.log("No se pudieron obtener boletos gratis");

}

let mensaje=`🎟️ RIFA LOS COMPAS

👤 Nombre: ${nombre}

🎫 Boletos comprados:
${arrayBol.join(", ")}`;

if(regalos.length>0){

mensaje+=`

🎁 Boletos GRATIS:
${regalos.join(", ")}`;

}

mensaje+=`

💰 Total a pagar: $${total}`;

const urlWhats="https://wa.me/527421199270="+encodeURIComponent(mensaje);

window.location.href = urlWhats;

seleccionados.clear();

document.getElementById("nombreCliente").value="";

actualizarCifras();

cargarVendidos();

btn.disabled=false;

btn.textContent="Finalizar y Enviar WhatsApp";

}

cargarVendidos();

setInterval(cargarVendidos,30000);

</script>

</body>
</html>
