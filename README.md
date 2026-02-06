<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Rifa Los Compas</title>
  <style>
    header, .page-header, #header, .header {
      display: none !important;
    }

    /* 2. DISEÑO GENERAL */
    body {
      font-family: Arial, sans-serif;
      background: #e6ffe6;
      text-align: center;
      padding: 20px;
      margin: 0;
    }
    
    .logo-rifa {
      width: 150px; /* Ajustado para que se vea bien */
      margin-top: 20px;
      margin-bottom: 10px;
      border-radius: 15px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    }

    h1 { color: #2e7d32; margin-top: 5px; }

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

  <img src="img/logo.JPG" alt="Logo Rifa Los Compas" class="logo-rifa">

  <h1>🎟️ Rifa Los Compas</h1>
  
  <p><strong>Boleto:</strong> $50</p>
  <p><strong>Premios:</strong><br>
    🥇 1er lugar: $5,000<br>
    🥈 2do lugar: $1,000<br>
    🥉 3er lugar: $500
  </p>

  <h2>Selecciona tu boleto</h2>
  <div class="numeros" id="numeros"></div>

  <div class="pago">
    <h3>Formas de pago</h3>
    💵 Efectivo<br>
    💳 Transferencia
  </div>

  <script>
    const formBaseURL = "https://docs.google.com/forms/d/e/1FAIpQLSdQT3I0GSMZ_QEB5Wq-TEXoIK-VHeKegK2q8UdLJQPZ0Ba8nw/viewform?usp=pp_url&entry.1324693116=";
    const contenedor = document.getElementById("numeros");

    // Generar los 200 números
    for (let i = 0; i < 200; i++) {
      const numero = i.toString().padStart(3, "0");
      const btn = document.createElement("button");
      btn.textContent = numero;
      btn.onclick = () => {
        window.open(formBaseURL + numero, "_blank");
      };
      contenedor.appendChild(btn);
    }
  </script>

</body>
</html>
