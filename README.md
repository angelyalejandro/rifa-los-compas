
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rifa Los Compas</title>

<style>
body{
  font-family: Arial, sans-serif;
  background:#eaffea;
  text-align:center;
  padding:20px;
}

h1{color:#1e7d32;}

.boletos{
  display:grid;
  grid-template-columns:repeat(auto-fill,minmax(60px,1fr));
  gap:8px;
  max-width:600px;
  margin:20px auto;
}

.boleto{
  padding:10px;
  border:2px solid #1e7d32;
  border-radius:6px;
  cursor:pointer;
  font-weight:bold;
  background:white;
}

.boleto.seleccionado{
  background:#1e7d32;
  color:white;
}

.boleto.vendido{
  background:#ccc;
  color:#666;
  border-color:#999;
  cursor:not-allowed;
}

.resumen{
  margin-top:20px;
  font-size:18px;
  font-weight:bold;
}

button{
  margin-top:20px;
  padding:15px 25px;
  background:#1e7d32;
  color:white;
  border:none;
  border-radius:8px;
  font-size:16px;
  cursor:pointer;
}
</style>
</head>

<body>

<h1>🎟️ RIFA LOS COMPAS 🎟️</h1>

<p>
Por cada boleto que compres tienes <strong>4 oportunidades GRATIS</strong><br>
Nos basamos en la Lotería Nacional<br>
Rifas totalmente confiables
</p>

<div id="contador">Boletos vendidos: 0 / 200</div>

<div class="boletos" id="boletos"></div>

<div class="resumen">
Boletos seleccionados: <span id="cantidad">0</span><br>
Total a pagar: $<span id="total">0</span>
</div>

<input type="text" id="nombre" placeholder="Escribe tu nombre completo" style="padding:10px;width:250px;margin-top:15px;">

<br>

<button onclick="pagar()">Pagar</button>

<script>

const PRECIO = 50;
const TOTAL = 200;

const URL_SCRIPT = "https://script.google.com/macros/s/AKfycbyS65zM0Yogdj7m-IGwWL8k1Aaikx_WOSmFIsVMnQUPB6m09G2-a7uSkhhr6v9iLLGu/exec";

const NUMERO_WHATSAPP = "527421199270";

let vendidos = [];
let seleccionados = new Set();

const contenedor = document.getElementById("boletos");

fetch(URL_SCRIPT + "?accion=vendidos")
.then(r=>r.json())
.then(data=>{
  vendidos=data;
  generarBoletos();
  actualizarContador();
});

function generarBoletos(){
  contenedor.innerHTML="";
  for(let i=0;i<TOTAL;i++){
    const num=i.toString().padStart(3,"0");
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
}

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

function actualizarResumen(){
  document.getElementById("cantidad").textContent=seleccionados.size;
  document.getElementById("total").textContent=seleccionados.size*PRECIO;
}

function actualizarContador(){
  document.getElementById("contador").textContent=
  "Boletos vendidos: "+vendidos.length+" / "+TOTAL;
}

function pagar(){

  const nombre=document.getElementById("nombre").value.trim();

  if(seleccionados.size===0){
    alert("Selecciona al menos un boleto");
    return;
  }

  if(nombre===""){
    alert("Escribe tu nombre");
    return;
  }

  const boletosArray=Array.from(seleccionados);
  const total=boletosArray.length*PRECIO;

  fetch(URL_SCRIPT+"?accion=gratis&numeros="+boletosArray.join(","))
  .then(r=>r.json())
  .then(gratis=>{

    const mensaje=
`Hola! Reserve los siguientes boletos:

BOLETOS🎫: ${boletosArray.join(", ")}
EXTRAS🎫: (${gratis.join(", ")})

COSTO TOTAL: $${total}
NOMBRE DE LA RIFA: Los Compas
————————
🟥Nombre: ${nombre}

EL SIGUIENTE PASO ES ENVIAR LA FOTO DEL COMPROBANTE DE PAGO AQUI.`;

    const url="https://wa.me/"+NUMERO_WHATSAPP+"?text="+encodeURIComponent(mensaje);

    window.open(url,"_blank");

  });

}

</script>

</body>
</html>
