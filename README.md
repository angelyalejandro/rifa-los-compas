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

    .precio {
      font-size: 18px;
      margin-bottom: 10px;
    }

    .numeros {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(55px, 1fr));
      gap: 10px;
      max-width: 500px;
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

    .boleto:hover {
      background: #388e3c;
    }

    .boleto.seleccionado {
      background: #ff9800;
      color: black;
    }

    .resumen {
      margin-top: 20px;
      font-size: 18px;
      background: white;
      padding: 15px;
      border-radius: 12px;
      display: inline-block;
    }

    .pagar {
      margin-top: 15px;
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

  <div class="precio">Costo del boleto: <strong>$50</strong></div>

  <h2>Selecciona tus boletos</h2>

  <div class="numeros" id="listaBoletos"></div>

  <div class="resumen">
    Boletos seleccionados:
    <div id="boletosSeleccionados">Ninguno</div>
    <br>
    Total a pagar: <strong>$<span id="total">0</span></strong>
  </div>

  <br>

  <button class="pagar" onclick="irAPago()">PAGAR</button>

  <script>
    const precio = 50;
    const maxBoletos = 200;
    const boletosElegidos = [];

    const contenedor = document.getElementById("listaBoletos");

    for (let

