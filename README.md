<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>RIFA LOS COMPÁS</title>

  <style>
    header, .page-header, #header, .header {
      display: none !important;
    }

    body {
      font-family: Arial, sans-serif;
      background: #e6ffe6;
      text-align: center;
      padding: 20px;
      margin: 0;
    }

    .logo-rifa {
      width: 150px; 
      margin-top: 20px;
      margin-bottom: 10px;
      border-radius: 15px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    }

    h1 {
      color: #2e7d32;
      margin-top: 5px;
    }

    .numeros {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(60px, 1fr));
      gap: 10px;
      max-width: 500px;
      margin: 20px auto;
      padding: 10px;
    }

    button {
      padding: 10px;
      border: none;
      background: #4caf50;
      color: white;
      font-weight: bold;
      cursor: pointer;
      border-radius: 5px;
      transition: transform 0.2s;
    }

    button:hover {
      background: #388e3c;
      transform: scale(1.05);
    }

    button.vendido {
      background: #9e9e9e;
      cursor: not-allowed;
      transform: none;
    }

    .pago {
      margin-top: 20px;
      font-size: 18px;
      background: white;
      padding: 20px;
      border-radius: 15px;
      display: inline-block;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
  </style>
</head>

<body>

  <!-- LOGO (luego lo arreglamos si quieres) -->
  <!-- <img src="logo.jpeg" class="logo-rifa"> -->

  <h1>🎟️ RIFA LOS COMPÁS 🎟️</h1>
  <p>Costo del boleto: <strong>$50</strong></p>
  <p>200 boletos disponibles (000 – 199)</p>

  <h2>Selecciona tu boleto</h2>

  <!-- CONTENEDOR DE BOLETOS -->
  <div id="numeros" class="numeros"></div>

  <div class="pago">
    <p><strong>Formas de pago:</strong></p>
    <p>💵 Efectivo</p>
    <p>🏦 Transferencia (se muestra en el formulario)</p>
  </div>

  <script>
    const formBaseURL = "https://docs.google.com/forms/d/e/1FAIpQLSdQT3I0GSMZ_QEB5Wq-TEXoIK-VHeKegK2q8UdLJQPZ0Ba8nw/viewform?usp=pp_url&entry.1324693116=";

    const apiURL = "PEGA_AQUÍ_TU_URL_DEL_WEB_APP";

    const contenedor = document.getElementById("numeros");

    fetch(apiURL)
      .then(res => res.json())
      .then(vendidos => {
        for (let i = 0; i < 200; i++) {
          const numero = i.toString().padStart(3, "0");
          const btn = document.createElement("button");
          btn.textContent = numero;

          if (vendidos.includes(numero)) {
            btn.classList.add("vendido");
            btn.disabled = true;
          } else {
            btn.onclick = () => {
              window.open(formBaseURL + numero, "_blank");
            };
          }

          contenedor.appendChild(btn);
        }
      })
      .catch(err => {
        contenedor.innerHTML = "Error cargando boletos";
        console.error(err);
      });
  </script>

</body>
</html>
