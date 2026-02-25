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

/* MENU */

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

.menu button:hover{
background:#ffe082;
}

/* BANNER */

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

.precio{
font-size:26px;
color:#d50000;
font-weight:bold;
}

/* CONTENIDO */

.container{
max-width:1000px;
margin:auto;
padding:20px;
}

.card{
background:white;
border-radius:15px;
padding:20px;
}

/* PLAYER */

.player{
width:100%;
border-radius:15px;
margin-bottom:20px;
box-shadow:0 4px 10px rgba(0,0,0,.2);
}

/* BOLETOS */

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
position:relative;
transition:all .2s ease;
}

.boleto:hover{
transform:scale(1.05);
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

.boleto.vendido::after{
content:"✖";
position:absolute;
top:2px;
right:5px;
font-size:12px;
}

/* BOTON */

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
cursor:not-allowed;
}

</style>

</head>
<body>

<!-- MENU -->

<nav class="menu">
<button onclick="mostrarSeccion('rifa')">Inicio</button>
<button onclick="mostrarSeccion('pagos')">Formas de Pago</button>
</nav>

<!-- SECCION RIFA -->

<div id="rifa" class="seccion">

<div class="banner">

<img src="https://raw.githubusercontent.com/angelyalejandro/rifa-los-compas/main/logo.JPG"
style="max-width:200px; display:block; margin:auto;">

<h1>RIFAS LOS COMPAS</h1>
<div class="precio">BOLETO $50</div>

</div>

<div class="container">

<div class="card">

<!-- PLAYER PREMIOS -->

<img class="player"
src="https://raw.githubusercontent.com/angelyalejandro/rifa-los-compas/main/flayer.jpeg">

<!-- BOLETOS -->

<div class="boletos" id="boletos"></div>

<div>
Boletos: <span id="cantidad">0</span><br>
Total: $<span id="total">0</span>
</div>

<input type="text" id="nombreCliente"
placeholder="Tu nombre completo"
style="width:100%; padding:10px; margin-top:10px;">

<br>

<button id="btnPagar" onclick="pagar()">Finalizar Compra</button>

</div>
</div>
</div>

<!-- PAGOS -->

<div id="pagos" class="seccion" style="display:none;">
<div class="container">
<div class="card">

<h2>WhatsApp</h2>
<p>7421199220</p>

</div>
</div>
</div>

<script>

const PRECIO_BOLETO = 50;
const TOTAL_BOLETOS = 400;

const URL_SCRIPT =
"https://script.google.com/macros/s/AKfycbwNNIRy0cxHTAttdssLt74wEmGWMBXbWhB2vVXDreEQprMraNc8psO1sHYPwCOq8ds4/exec";

const TELEFONO = "527421199220";

const contenedor = document.getElementById("boletos");

const seleccionados = new Set();

let vendidos = [];

/* SECCIONES */

function mostrarSeccion(id){

document.querySelectorAll(".seccion")
.forEach(sec=>sec.style.display="none");

document.getElementById(id).style.display="block";

}

/* CARGAR VENDIDOS */

function cargarVendidos(){

fetch(URL_SCRIPT)
.then(res=>res.json())
.then(data=>{
vendidos = data || [];
generarBoletos();
})
.catch(()=>generarBoletos());

}

cargarVendidos();

setInterval(cargarVendidos,10000);

/* GENERAR BOLETOS */

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

if(seleccionados.has(num)){
div.classList.add("seleccionado");
}

div.onclick=()=>toggle(num,div);

}

contenedor.appendChild(div);

}

}

/* TOGGLE */

function toggle(num,div){

if(seleccionados.has(num)){

seleccionados.delete(num);
div.classList.remove("seleccionado");

}else{

seleccionados.add(num);
div.classList.add("seleccionado");

}

actualizarResumen();

}

/* RESUMEN */

function actualizarResumen(){

document.getElementById("cantidad").textContent =
seleccionados.size;

document.getElementById("total").textContent =
seleccionados.size * PRECIO_BOLETO;

}

/* PAGAR */

function pagar(){

if(seleccionados.size===0){
alert("Selecciona boletos");
return;
}

const nombre =
document.getElementById("nombreCliente").value.trim();

if(nombre===""){
alert("Escribe tu nombre");
return;
}

const boletosArray = Array.from(seleccionados);

const boton = document.getElementById("btnPagar");

boton.disabled=true;
boton.textContent="Procesando...";

fetch(URL_SCRIPT,{
method:"POST",
body:JSON.stringify({
nombre:nombre,
boletos:boletosArray
})
})
.then(res=>res.json())
.then(()=>{

const total = boletosArray.length * PRECIO_BOLETO;

const mensaje =
`🎟️ RIFA LOS COMPAS
Hola! Reserve los siguientes boletos:

🎫 BOLETOS: ${boletosArray.join(", ")}

🎁 BOLETOS GRATIS:

💰 TOTAL: $${total}

👤 Nombre: ${nombre}

ENVIARÉ MI COMPROBANTE EN UN MOMENTO.`;

const url =
`https://wa.me/${TELEFONO}?text=${encodeURIComponent(mensaje)}`;

window.open(url,"_blank");

seleccionados.clear();

boton.disabled=false;
boton.textContent="Finalizar Compra";

cargarVendidos();

});

}

</script>

</body>
</html>
