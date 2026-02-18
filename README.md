
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>RIFA LOS COMPÁS</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <style>
    body {
      font-family: Arial, sans-serif;
      background: #eaffea;
      margin: 0;
      padding: 20px;
      text-align: center;
    }

    img.logo {
      width: 160px;
      margin: 10px auto;
      display: block;
      border-radius: 15px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.15);
    }

    h1 { color: #1e7d32; margin-bottom: 5px; }

    .descripcion {
      font-size: 16px;
      margin: 10px auto 15px;
      color: #2e7d32;
      max-width: 500px;
      font-weight: bold;
    }

    .contador {
      font-size: 18px;
      font-weight: bold;
      color: #1e7d32;
      margin-bottom: 20px;
    }

    h2 { color: #145a23; margin-top: 30px; }

    .info { margin-bottom: 20px; font-size: 16px; }

    .premios {
      background: white;
      padding: 15px;
      border-radius: 12px;
      max-width: 500px;
      margin: 20px auto;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      font-size: 16px;
    }

    .boletos {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(60px, 1fr));
      gap: 10px;
      max-width: 600px;
      margin: 0 auto 15px;
    }

    .boleto {
      padding: 10px;
      border-radius: 6px;
      background: #fff;
      border: 2px solid #1e7d32;
      cursor: pointer;
      font-weight: bold;
      user-select: none;
    }

    .boleto.seleccionado {
      background: #1e7d32;
      color: #fff;
    }

    .boleto.vendido {
      background: #ccc;
      color: #666;
      cursor: not-allowed;
      border-color: #999;
    }

    .resumen {
      font-size: 18px;
      margin: 15px auto;
      padding: 15px;
      background: #ffffff;
      border-radius: 12px;
      max-width: 400px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      font-weight: bold;
    }

    .pago {
      background: #fff;
      padding: 15px;
      border-radius: 12px;
      max-width: 400px;
      margin: 20px auto;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }

    button {
      background: #1e7d32;
      color: #fff;
      border: none;
      padding: 15px 25px;
      font-size: 16px;
      border-radius: 8px;
      cursor: pointer;
      margin-top: 15px;
    }

    button:hover { background: #145a23; }
  </style>
</head>

<body>

<img src="logo.JPG" alt="Logo Rifa Los Compás" class="logo">

<h1>🎟️ RIFA LOS COMPÁS 🎟️</h1>

<p class="descripcion">
Por cada boleto que compres tienes <strong>4 posibilidades más de ganar</strong>
totalmente <strong>GRATIS</strong><br><br>
🎰 Nos basamos en la <strong>Lotería Nacional</strong><br>
✅ Nuestras rifas son <strong>totalmente confiables</strong>
</p>

<div id="contador" class="contador">
🎟️ Boletos vendidos: 0 / 200
</div>

<div class="info">
Costo del boleto: <strong>$50</strong><br>
Total de boletos: <strong>200</strong>
</div>

<h2>🏆 Premios</h2>
<div class="premios">
🥇 <strong>1er lugar:</strong> $5,000<br><br>
🥈 <strong>2do lugar:</strong> $1,000<br><br>
🥉 <strong>3er lugar:</strong> $500
</div>

<h2>Selecciona tus boletos</h2>
<div class="boletos" id="boletos"></div>

<div class="resumen">
🎟️ Boletos seleccionados: <span id="cantidad">0</span><br>
💰 Total a pagar: $<span id="total">0</span>
</div>

<div class="pago">
<strong>Formas de pago</strong><br><br>
💵 Efectivo<br>
🏦 Transferencia bancaria
</div>

<button onclick="pagar()">Pagar</button>

<script>
const PRECIO_BOLETO = 50;
const TOTAL_BOLETOS = 200;

const URL_SCRIPT = "https://script.google.com/macros/s/AKfycbyS65zM0Yogdj7m-IGwWL8k1Aaikx_WOSmFIsVMnQUPB6m09G2-a7uSkhhr6v9iLLGu/exec";

const FORM_URL =
"https://docs.google.com/forms/d/e/1FAIpQLSdQT3I0GSMZ_QEB5Wq-TEXoIK-VHeKegK2q8UdLJQPZ0Ba8nw/viewform?usp=pp_url&entry.1324693116=";

const contenedor = document.getElementById("boletos");
const seleccionados = new Set();
let vendidos = [];

fetch(URL_SCRIPT + "?accion=vendidos")
.then(res => res.json())
.then(data => {
  vendidos = data;
  actualizarContador();
  generarBoletos();
})
.catch(() => {
  actualizarContador();
  generarBoletos();
});

function generarBoletos() {
  contenedor.innerHTML = "";
  for (let i = 0; i < TOTAL_BOLETOS; i++) {
    const num = i.toString().padStart(3,"0");
    const div = document.createElement("div");
    div.className = "boleto";
    div.textContent = num;

    if (vendidos.includes(num)) {
      div.classList.add("vendido");
    } else {
      div.onclick = () => toggleBoleto(num, div);
    }

    contenedor.appendChild(div);
  }
}

function toggleBoleto(num, div) {
  if (seleccionados.has(num)) {
    seleccionados.delete(num);
    div.classList.remove("seleccionado");
  } else {
    seleccionados.add(num);
    div.classList.add("seleccionado");
  }
  actualizarResumen();
}

function actualizarResumen() {
  document.getElementById("cantidad").textContent = seleccionados.size;
  document.getElementById("total").textContent =
    seleccionados.size * PRECIO_BOLETO;
}

function actualizarContador() {
  document.getElementById("contador").textContent =
    `🎟️ Boletos vendidos: ${vendidos.length} / ${TOTAL_BOLETOS}`;
}

function pagar() {
  if (seleccionados.size === 0) {
    alert("Selecciona al menos un boleto");
    return;
  }

  const boletosArray = Array.from(seleccionados);
  const boletosTexto = boletosArray.join(",");

  fetch(URL_SCRIPT + "?accion=gratis&numeros=" + boletosTexto)
  .then(res => res.json())
  .then(gratis => {

    let mensaje = "🎟️ Boletos comprados:\n" + boletosArray.join(", ");

    if (gratis.length > 0) {
      mensaje += "\n\n🎁 Tus boletos GRATIS son:\n" + gratis.join(", ");
    }

    alert(mensaje);

    window.location.href = FORM_URL + encodeURIComponent(boletosArray.join(", "));
  })
  .catch(() => {
    window.location.href = FORM_URL + encodeURIComponent(boletosArray.join(", "));
  });
}
</script>

</body>
</html>
