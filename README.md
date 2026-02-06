<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Rifa Los Compás</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #e6ffe6;
      text-align: center;
      padding: 20px;
      margin: 0;
    }

    h1 {
      color: #2e7d32;
    }

    .numeros {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(60px, 1fr));
      gap: 10px;
      max-width: 600px;
      margin: 20px auto;
    }

    .boleto {
      padding: 12px;
      background: #4caf50;
      color: white;
      border-radius: 6px;
      cursor: pointer;
      font-weight: bold;
      user-select: none;
    }

    .boleto.seleccionado {
      background: #1b5e20;
    }

    .pagar {
      margin-top: 25px;
      padding: 15px 25px;
      font-size: 18px;
      background: #2e7d32;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
    }

    .pagar:hover {
      background: #1b5e20;
    }

    .info {
      background: white;
      display: inline-block;
      padding: 15px 25px;
      border-radius: 12px;
      margin-top: 20px;
      box-shadow: 0 2px 6px rgba(0,0,0,.15);
    }
  </style>
</head>
<body>

  <h1>🎟️ RIFA LOS COMPÁS 🎟️</h1>
  <p><strong>Costo del boleto:</strong> $50</p>
  <p>200 boletos disponibles (000 – 199)</p>

  <h2>Selecciona tus boletos</h2>

  <div class="numeros" id="numeros"></div>

  <button class="pagar" onclick="irAPago()">Pagar</button>

  <div class="info">
    <h3>Formas de pago</h3>
    💵 Efectivo <br>
    🏦 Transferencia (se muestra en el formulario)
  </div>

<script>
  const contenedor = document.getElementById("numeros");
  const seleccionados = new Set();

  for (let i = 0; i < 200; i++) {
    const num = String(i).padStart(3, "0");
    const div = document.createElement("div");
    div.className = "boleto";
    div.textContent = num;

    div.onclick = () => {
      if (seleccionados.has(num)) {
        seleccionados.delete(num);
        div.classList.remove("seleccionado");
      } else {
        seleccionados.add(num);
        div.classList.add("seleccionado");
      }
    };

    contenedor.appendChild(div);
  }

  function irAPago() {
    if (seleccionados.size === 0) {
      alert("Selecciona al menos un boleto");
      return;
    }

    const boletos = Array.from(seleccionados).join(", ");

    const url = https://docs.google.com/forms/d/e/1FAIpQLSdQT3I0GSMZ_QEB5Wq-TEXoIK-VHeKegK2q8UdLJQPZ0Ba8nw/viewform?usp=pp_url&entry.1324693116=000
      + encodeURIComponent(boletos);

    window.open(url, "_blank");
  }
</script>

</body>
</html>

