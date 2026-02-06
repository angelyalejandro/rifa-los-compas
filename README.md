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
header,
.page-header,
#header,
iframe,
.google-header {
  display: none !important;
}

    h1 {
      color: #2e7d32;
    }

    .numeros {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(60px, 1fr));
      gap: 10px;
      max-width: 500px;
      margin: 20px auto;
    }

    .numero {
      padding: 12px;
      background: #4caf50;
      color: white;
      font-weight: bold;
      border-radius: 6px;
      cursor: pointer;
      user-select: none;
    }

    .numero.seleccionado {
      background: #1b5e20;
    }

    .pagar {
      margin-top: 20px;
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
  </style>
</head>

<body>

  <h1>🎟️ RIFA LOS COMPÁS 🎟️</h1>
  <p><strong>Costo del boleto:</strong> $50</p>

  <h2>Selecciona tus boletos</h2>

  <div class="numeros" id="numeros"></div>

  <button class="pagar" onclick="irAPago()">Pagar</button>

  <script>
    const contenedor = document.getElementById("numeros");
    const seleccionados = [];

    // Crear boletos 000 - 199
    for (let i = 0; i < 200; i++) {
      const num = i.toString().padStart(3, "0");
      const div = document.createElement("div");
      div.className = "numero";
      div.textContent = num;

      div.onclick = () => {
        if (seleccionados.includes(num)) {
          seleccionados.splice(seleccionados.indexOf(num), 1);
          div.classList.remove("seleccionado");
        } else {
          seleccionados.push(num);
          div.classList.add("seleccionado");
        }
      };

      contenedor.appendChild(div);
    }

    function irAPago() {
      if (seleccionados.length === 0) {
        alert("Selecciona al menos un boleto");
        return;
      }

      const boletos = seleccionados.join(", ");
      const url =
        "https://docs.google.com/forms/d/e/1FAIpQLSdQT3I0GSMZ_QEB5Wq-TEXoIK-VHeKegK2q8UdLJQPZ0Ba8nw/viewform?usp=pp_url" +
        "&entry.1324693116=" + encodeURIComponent(boletos);

      window.open(url, "_blank");
    }
  </script>

</body>
</html>
