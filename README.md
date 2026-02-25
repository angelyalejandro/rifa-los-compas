
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RIFAS LOS COMPAS</title>
    <style>
        body{font-family:'Segoe UI',sans-serif;margin:0;background:#e9f7e9;}
        .menu{display:flex;justify-content:center;gap:10px;padding:15px;background:#0d47a1;position:sticky;top:0;z-index:1000;}
        .menu button{background:white;color:#0d47a1;font-weight:bold;border:none;padding:10px 20px;border-radius:8px;cursor:pointer;}
        .banner{background:linear-gradient(135deg,#ffe082,#80deea);padding:25px;text-align:center;}
        .banner h1{font-size:34px;margin:10px 0;color:#0d47a1;}
        .banner img{max-width:180px;display:block;margin:auto;border-radius: 50%;}
        .container{max-width:1100px;margin:auto;padding:20px;}
        .card{background:white;border-radius:15px;padding:20px;box-shadow: 0 4px 15px rgba(0,0,0,0.1);}
        
        /* Ajuste del Player/Flyer */
        .player{width:100%; max-width:500px; display:block; margin: 0 auto 20px auto; border-radius:15px; box-shadow: 0 4px 10px rgba(0,0,0,0.2);}
        
        .boletos{display:grid;grid-template-columns:repeat(auto-fill,minmax(70px,1fr));gap:10px;}
        .boleto{padding:14px 6px;border-radius:12px;font-weight:bold;cursor:pointer;background:#eeeeee;text-align:center;box-shadow:0 3px 6px rgba(0,0,0,0.15);}
        
        /* ESTADOS DE LOS BOLETOS */
        .boleto.seleccionado{background:#00c853 !important;color:white !important;}
        .boleto.vendido{background:#666666 !important;color:#ffffff !important;cursor:not-allowed !important;opacity: 0.7;}
        
        .resumen{background: #f8f9fa; padding: 15px; border-radius: 10px; margin-top: 20px; border-left: 5px solid #00c853;}
        input{width:100%; padding:12px; margin-top:10px; border-radius: 8px; border:1px solid #ccc; box-sizing: border-box;}
        #btnPagar{width: 100%; margin-top:15px; padding:15px; border:none; border-radius:10px; background:#00c853; color:white; font-size:18px; font-weight:bold; cursor:pointer;}
    </style>
</head>
<body>

<nav class="menu">
    <button onclick="mostrarSeccion('rifa')">Inicio</button>
    <button onclick="mostrarSeccion('pagos')">Formas de Pago</button>
</nav>

<div id="rifa" class="seccion">
    <div class="banner">
        <img src="https://raw.githubusercontent.com/angelyalejandro/rifa-los-compas/main/logo.JPG" alt="Logo">
        <h1>RIFAS LOS COMPAS</h1>
        <h1>POR CADA BOLETO QUE COMPRES TIENES 10 OPORTUNIDADES MAS TOTALMENTE GRATIS</h1>
    </div>
    <div class="container">
        <div class="card">
            <img class="player" src="https://raw.githubusercontent.com/angelyalejandro/rifa-los-compas/main/flayer.jpeg" alt="Flyer">
            
            <h3>Selecciona tus boletos:</h3>
            <div class="boletos" id="boletos"></div>
            
            <div class="resumen">
                <strong>Boletos:</strong> <span id="cantidad">0</span> | <strong>Total:</strong> $<span id="total">0</span>
            </div>
            
            <input type="text" id="nombreCliente" placeholder="Nombre completo">
            <button id="btnPagar" onclick="pagar()">Finalizar Compra</button>
        </div>
    </div>
</div>

<div id="pagos" class="seccion" style="display:none;">
    <div class="container">
        <div class="card" style="text-align:center; background:#0f6c6c; color:white;">
            <h1>VENDEDORES AUTORIZADOS</h1>
            <h2>Luis Alejandro Romero Sebastian</h2>
            <p>📱 7421199270 | BBVA: 4152 3140 2646 1213</p>
            <hr>
            <h2>Angel Gabriel Urioste Luciano</h2>
            <p>📱 7421292436 | BBVA: 4152 3145 7352 6715</p>
        </div>
    </div>
</div>

<script>
const PRECIO_BOLETO = 50;
const TOTAL_BOLETOS = 400;
const TELEFONO = "527421199270";
// URL Actualizada
const URL_SCRIPT = "https://script.google.com/macros/s/AKfycbwECOIKV6pNBp2gi8Q51cPnNmVnTULWO0bRVi7-VK7xXUHN2isPL6HMgElhfHTkKSaN/exec";

let seleccionados = new Set();
let vendidos = [];
let gratisPorBoleto = {};

function mostrarSeccion(id) {
    document.querySelectorAll(".seccion").forEach(s => s.style.display = "none");
    document.getElementById(id).style.display = "block";
}

function cargarDatos() {
    fetch(URL_SCRIPT + "?t=" + new Date().getTime())
    .then(res => res.json())
    .then(data => {
        // Normalización estricta: quitar espacios y asegurar 4 dígitos
        vendidos = (data.vendidos || []).map(n => n.toString().trim().padStart(4, "0"));
        gratisPorBoleto = data.gratisPorBoleto || {};
        generarBoletos();
    })
    .catch(err => {
        console.error("Error cargando datos:", err);
        if(document.getElementById("boletos").innerHTML === "") generarBoletos();
    });
}

function generarBoletos() {
    const contenedor = document.getElementById("boletos");
    contenedor.innerHTML = "";
    for(let i=1; i<=TOTAL_BOLETOS; i++){
        const num = i.toString().padStart(4, "0");
        const div = document.createElement("div");
        div.className = "boleto";
        div.textContent = num;

        // Comprobación de vendido
        if(vendidos.includes(num)){
            div.classList.add("vendido");
        } else {
            if(seleccionados.has(num)) div.classList.add("seleccionado");
            div.onclick = () => {
                if(seleccionados.has(num)){
                    seleccionados.delete(num);
                    div.classList.remove("seleccionado");
                } else {
                    seleccionados.add(num);
                    div.classList.add("seleccionado");
                }
                actualizarTotales();
            };
        }
        contenedor.appendChild(div);
    }
}

function actualizarTotales() {
    document.getElementById("cantidad").textContent = seleccionados.size;
    document.getElementById("total").textContent = seleccionados.size * PRECIO_BOLETO;
}

function pagar() {
    const nombre = document.getElementById("nombreCliente").value.trim();
    if(seleccionados.size === 0 || nombre === "") return alert("Faltan datos");

    const boletosArray = Array.from(seleccionados);
    let boletosRegalo = [];

    // Jalar boletos gratis de las columnas D-M del Excel
    boletosArray.forEach(b => {
        if(gratisPorBoleto[b]) boletosRegalo.push(...gratisPorBoleto[b]);
    });

    const msg = `*RIFAS LOS COMPAS*%0A👤 *Nombre:* ${nombre}%0A🎫 *Boletos:* ${boletosArray.join(", ")}%0A🎁 *Boletos Gratis:* ${boletosRegalo.length > 0 ? boletosRegalo.join(", ") : "Ninguno"}%0A💰 *Total:* $${boletosArray.length * PRECIO_BOLETO}`;

    window.open(`https://wa.me/${TELEFONO}?text=${msg}`, "_blank");

    fetch(URL_SCRIPT, {
        method: "POST",
        mode: "no-cors",
        body: JSON.stringify({ nombre: nombre, boletos: boletosArray })
    }).finally(() => {
        alert("Reserva enviada. ¡Suerte!");
        seleccionados.clear();
        actualizarTotales();
        cargarDatos();
    });
}

cargarDatos();
setInterval(cargarDatos, 15000);
</script>
</body>
</html>
