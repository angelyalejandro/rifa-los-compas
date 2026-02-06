<img src="img/logo.png" alt="Logo Rifa Los Compas" style="width:120px; margin-bottom:10px;">
<head>
  <meta charset="UTF-8">
 
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #e6ffe6;
      text-align: center;
      padding: 20px;
    }
    h1 { color: #2e7d32; }
    .numeros {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(60px, 1fr));
      gap: 10px;
      max-width: 500px;
      margin: 20px auto;
    }
    button {
      padding: 10px;
      border: none;
      background: #4caf50;
      color: white;
      font-weight: bold;
      cursor: pointer;
      border-radius: 5px;
    }
    button.vendido {
      background: gray;
      cursor: not-allowed;
    }
    .pago {
      margin-top: 20px;
      font-size: 18px;
    }
  </style>
</head>
<body>

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
    const contenedor = document.getElementById("numeros");
    for (let i = 1; i <= 100; i++) {
      const btn = document.createElement("button");
      btn.textContent = i;
      btn.onclick = () => {
        btn.classList.add("vendido");
        btn.disabled = true;
      };
      contenedor.appendChild(btn);
    }
  </script>

</body>
