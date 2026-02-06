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

    .logo-rifa {
      width: 140px;
      margin: 15px auto;
      display: block;
      border-radius: 15px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.2);
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
    }

    .vendido {
      background: #9e9e9e;
      cursor: not-allowed;
    }

    .pago {
      margin-top: 25px;
      background: white;
      padding: 20px;
      border-radius: 15px;
      display: inline-block;
      box-shadow: 0 2px 5px rgba(0,0,0,0.15);
    }
  </style>
</head>

<body>

  <!-- LOGO (mañana lo arreglamos bien) -->
  <!-- <img src="logo.jpg" class="logo-rifa"> -->

  <h1>🎟️ RIFA LOS COMPÁS 🎟️</h1>

  <p><strong>Costo del boleto:</strong> $50</p>
  <p>200 boletos disponibles (000 – 199)</p>

  <h2>Selecciona tu boleto</h2>

  <div id="numeros" class="numeros">
    <p>Cargando boletos...</p>
  </div>

  <div class="pago">
    <h3>Formas de pago</h3>
    💵 Efectivo <br>
    🏦 Transferencia (se muestra en el formulario)
  </div>

  <script>
    const apiURL = (https://script.google.com/macros/s/AKfycbxFnl1zxI0TeLOmdEdigJZz7EOyEHIgwJH2XGJVy5yzRx5eg72t4ZEkCoJf8knD9Z6p/exec);

    fetch(apiURL)
      .then(res => res.json())
      .then(vendidos => {
        const contenedor = document.getElementById("numeros");
        contenedor.innerHTML = "";

        for (let i = 0; i < 200; i++) {
          const num = i.toString().pad
