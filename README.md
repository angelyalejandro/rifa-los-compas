
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

    h1 {
      color: #1e7d32;
      margin-bottom: 10px;
    }

    .info {
      margin-bottom: 20px;
      font-size: 16px;
    }

    .boletos {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(60px, 1fr));
      gap: 10px;
      max-width: 600px;
      margin: 0 auto 20px;
    }

    .boleto {
      padding: 10px;
      border-radius: 6px;
      background: #fff;
      border: 2px solid #1e7d32;
      cursor: pointer;
      user-select: none;
      font-weight: bold;
    }

    .boleto.seleccionado {
      background: #1e7d32;
      color: #fff;
    }

    .pago {
      background: #fff;
      padding: 15px;
      border-radius: 10px;
      max-width: 400px;
      margin: 20px auto;
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

    button:hover {
      background: #145a23;
    }
  </style>
</head>

<body>

  <h1>🎟️ RIFA LOS COMPÁS 🎟️</h1>

  <div class="info">
    Costo del boleto: <strong>$50</strong><br>
    200 boletos disponibles (000 – 199)
  </div>

  <h3>Selecciona tus boletos</h3>

  <div class="boletos" id="boletos"></div>

  <div class="pago">
    <strong>Formas de pago</strong><br><br>
    💵 Efectivo<br>
    🏦 Transferencia (se muestra en el formulario)
  </div>

  <button onclick="pagar()">Pagar</button>

  <script>
    const contenedor = document.getElementById("boletos");
    const seleccionados = new Set();

    for (let i = 0; i < 200; i++) {
      const num = i.toString().padStart(3, "0");
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

    function pagar() {
      if (seleccionados.size === 0) {
        alert("Selecciona al menos un boleto");
        return;
      }

      const boletos = Array.from(seleccionados).join(", ");

      const url = "https://docs.google.com/forms/d/e/1FAIpQLSdQT3I0GSMZ_QEB5Wq-TEXoIK-VHeKegK2q8UdLJQPZ0Ba8nw/viewform?usp=pp_url&entry.1324693116="
        + encodeURIComponent(boletos);

      window.location.href = url;
    }
  </script>

</body>
</html>
