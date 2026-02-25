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
.menu button:hover{
  background:#ffe082;
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
.precio{
  font-size:26px;
  color:#d50000;
  font-weight:bold;
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
  box-shadow:0 3px 6px rgba(0,0,0,.15);
  transition:.2s;
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
    <img src="https://raw.githubusercontent.com/angelyalejandro/rifa-los-compas/main/logo.JPG" style="max-width:200px; display:block; margin:auto;">
    <h1>RIFAS LOS COMPAS</h1>
    <div class="precio">BOLETO $50</div>
  </div>

  <div class="container">
    <div class="card">
      <img class="player" src="https://raw.githubusercontent.com/angelyalejandro/rifa-los-compas/main/flayer.jpeg">

      <div class="boletos" id="boletos"></div>

      <div>
        Boletos: <span id="cantidad">0</span><br>
        Total: $<span id="total">0</span>
      </div>

      <input type="text" id="nombreCliente" placeholder="Tu nombre completo" style="width:100%; padding:10px; margin-top:10px;">
      <button id="btnPagar" onclick="pagar()">Finalizar Compra</button>
    </div>
  </div>
</div>

<div id="pagos" class="seccion" style="display:none;">
  <div class="container">
    <div class="card">
      <h2 style="text-align:center;">VENDEDORES AUTORIZADOS</h2>
      <hr>
      <h3>✅ Luis Alejandro Romero Sebastian</h3>
      <p><strong>WhatsApp:</strong> 7421199270</p>
      <hr>
      <h3>✅ Angel Gabriel Urioste Luciano</h3>
      <p><strong>WhatsApp:</strong> 7421292436</p>
    </div>
  </div>
</div>

<script>
const PRECIO_BOLETO = 50;
const TOTAL_BOLETOS = 400;
const TELEFONO = "527421199270"; // NUMERO CORRECTO
const URL_SCRIPT = "https://script.google.com/macros/s/AKfycbz1yoGLxiuGVeEBaLvQFjmRkcNLQWntqcPnflpSGC2HF-g1gqMnxg1HNQlYQh0mRfs/exec";

const contenedor = document.getElementById("boletos");
const seleccionados = new Set();
let vendidos = [];

function mostrarSeccion(id){
  document.querySelectorAll(".seccion").forEach(sec=>sec.style.display="none");
  document.getElementById(id).style.display="block";
}

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

function generarBoletos(){
  contenedor.innerHTML="";
  for(let i=1;i<=TOTAL_BOLETOS;i++){
    const num=i.toString().padStart(4,"0");
    const div=document.createElement("div");
    div.className="boleto";
    div.textContent=num;

    if(vendidos.includes(num)){
      div.classList.add("vendido");
    } else {
      if(seleccionados.has(num)) div.classList.add("seleccionado");
      div.onclick=()=>toggle(num,div);
    }
    contenedor.appendChild(div);
  }
}

function toggle(num,div){
  if(seleccionados.has(num)){
    seleccionados.delete(num);
    div.classList.remove("seleccionado");
  } else {
    seleccionados.add(num);
    div.classList.add("seleccionado");
  }
  actualizarResumen();
}

function actualizarResumen(){
  document.getElementById("cantidad").textContent = seleccionados.size;
  document.getElementById("total").textContent = seleccionados.size * PRECIO_BOLETO;
}

function pagar(){
  if(seleccionados.size===0){
    alert("Selecciona boletos");
    return;
  }

  const nombre = document.getElementById("nombreCliente").value.trim();
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
    headers:{
      "Content-Type":"application/json"
    },
    body:JSON.stringify({
      nombre:nombre,
      boletos:boletosArray
    })
  })
  .then(res=>res.json())
  .then(data=>{
    console.log("Guardado en hoja:",data);
  })
  .catch(err=>{
    console.error("Error:",err);
    alert("Error al registrar boletos");
  })
  .finally(()=>{
    const total = boletosArray.length * PRECIO_BOLETO;

    const mensaje =
`🎟️ RIFA LOS COMPAS
Hola! Reservé los siguientes boletos:

🎫 BOLETOS: ${boletosArray.join(", ")}

💰 TOTAL: $${total}

👤 Nombre: ${nombre}

ENVIARÉ MI COMPROBANTE EN UN MOMENTO.`;

    const url = `https://wa.me/${TELEFONO}?text=${encodeURIComponent(mensaje)}`;
    window.open(url,"_blank");

    seleccionados.clear();
    actualizarResumen();
    boton.disabled=false;
    boton.textContent="Finalizar Compra";
    cargarVendidos();
  });
}
</script>

</body>
</html>
