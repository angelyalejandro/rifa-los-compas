
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rifa Los Compas</title>

<style>
body{
  font-family: 'Segoe UI', sans-serif;
  background: linear-gradient(135deg,#0f2027,#203a43,#2c5364);
  margin:0;
  padding:20px;
  color:white;
  text-align:center;
}

.container{
  max-width:1000px;
  margin:auto;
}

.logo{
  width:170px;
  border-radius:15px;
  margin-bottom:15px;
  box-shadow:0 10px 25px rgba(0,0,0,.4);
}

h1{
  margin-bottom:5px;
  font-size:28px;
}

.descripcion{
  font-size:15px;
  opacity:.95;
  margin-bottom:20px;
}

.card{
  background:white;
  color:#333;
  border-radius:15px;
  padding:20px;
  margin-bottom:25px;
  box-shadow:0 10px 25px rgba(0,0,0,.3);
}

.premios{
  font-size:16px;
  line-height:1.8;
}

.boletos{
  display:grid;
  grid-template-columns:repeat(auto-fill,minmax(60px,1fr));
  gap:8px;
  margin-top:15px;
}

.boleto{
  padding:10px;
  border-radius:8px;
  font-weight:bold;
  cursor:pointer;
  background:#f2f2f2;
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

.resumen{
  margin-top:15px;
  font-weight:bold;
}

.preview{
  margin-top:10px;
  font-size:15px;
  color:#00c853;
  font-weight:bold;
}

input{
  padding:12px;
  width:260px;
  border-radius:8px;
  border:none;
  margin-top:15px;
  font-size:15px;
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
  transition:.2s;
}

button:hover{
  background:#00a843;
}

</style>
</head>

<body>

<div class="container">

<img src="logo.JPG" class="logo">

<h1>🎟️ RIFA LOS COMPAS 🎟️</h1>

<p class="descripcion">
Por cada boleto que compres tienes <strong>4 oportunidades EXTRA totalmente GRATIS</strong> 🎁<br>
🎰 Basado en la Lotería Nacional<br>
✅ Rifa totalmente confiable
</p>

<div class="card premios">
<h2>🏆 Premios</h2>
🥇 1er lugar: $5,000<br>
🥈 2do lugar: $1,000<br>
🥉 3er lugar: $500
</div>

<div class="card">

<h2>Selecciona tus boletos</h2>

<div class="boletos" id="boletos"></div>

<div class="resumen">
Boletos seleccionados: <span id="cantidad">0</span><br>
Total: $<span id="total">0</span>
</div>

<div class="preview" id="previewExtras"></div>

<input type="text" id="nombreCliente" placeholder="Tu nombre completo">

<br>

<button onclick="pagar()">Finalizar Compra</button>

</div>

</div>

<script>

const PRECIO_BOLETO=50;
const TOTAL_BOLETOS=200;
const URL_SCRIPT="https://script.google.com/macros/s/AKfycbzTOWYzJCyo8by2wIQVea_okYTKMMmzDyJhNWfAEF4l44J9IRqsw-T6JpRNzP7k6fE/exec";

const contenedor=document.getElementById("boletos");
const seleccionados=new Set();
let vendidos=[];

fetch(URL_SCRIPT)
.then(res=>res.json())
.then(data=>{
  vendidos=data;
  generarBoletos();
});

function generarBoletos(){
  for(let i=0;i<TOTAL_BOLETOS;i++){
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
  document.getElementById("total").textContent=
  seleccionados.size*PRECIO_BOLETO;

  if(seleccionados.size>0){
    document.getElementById("previewExtras").innerHTML=
    "🎁 Tus números EXTRA aparecerán antes de enviarte a WhatsApp";
  }else{
    document.getElementById("previewExtras").innerHTML="";
  }
}

function pagar(){

  if(seleccionados.size===0){
    alert("Selecciona al menos un boleto");
    return;
  }

  const nombre=document.getElementById("nombreCliente").value.trim();

  if(nombre===""){
    alert("Escribe tu nombre");
    return;
  }

  const boletosArray=Array.from(seleccionados);
  const total = boletosArray.length * PRECIO_BOLETO;

  // MENSAJE PROVISIONAL (WhatsApp se abre inmediatamente)
  const mensajeBase =
`🎟️ *RIFA LOS COMPAS*

━━━━━━━━━━━━━━
🎫 BOLETOS: ${boletosArray.join(", ")}

💰 TOTAL: $${total}

👤 Nombre: ${nombre}
━━━━━━━━━━━━━━

Envío comprobante enseguida.`;

  // ABRE WHATSAPP INMEDIATAMENTE (esto evita bloqueos)
  window.open(
    "https://wa.me/527421199270?text="+
    encodeURIComponent(mensajeBase),
    "_blank"
  );

  // AHORA GUARDAMOS EN GOOGLE SHEETS
  fetch(URL_SCRIPT,{
    method:"POST",
    body:JSON.stringify({
      nombre:nombre,
      boletos:boletosArray
    })
  })
  .then(res=>res.json())
  .then(data=>{
      console.log("Guardado correctamente");
      location.reload();
  })
  .catch(err=>{
      console.log("Error al guardar");
  });

}


      location.reload();

    },1500);

  });
}

</script>

</body>
</html>
