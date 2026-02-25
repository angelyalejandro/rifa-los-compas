<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RIFAS LOS COMPAS</title>
    <style>
        body { font-family: 'Segoe UI', sans-serif; margin: 0; background: #e9f7e9; }
        .menu { display: flex; justify-content: center; gap: 10px; padding: 15px; background: #0d47a1; position: sticky; top: 0; z-index: 1000; }
        .menu button { background: white; color: #0d47a1; font-weight: bold; border: none; padding: 10px 15px; border-radius: 8px; cursor: pointer; }
        
        /* BANNER CORREGIDO */
        .banner { background: linear-gradient(135deg, #ffe082, #80deea); padding: 25px 15px; text-align: center; }
        .banner img { width: 120px; height: 120px; border-radius: 50%; border: 3px solid white; margin-bottom: 10px; }
        .banner h1 { font-size: 28px; margin: 5px 0; color: #0d47a1; }
        .banner p { font-size: 15px; color: #01579b; font-weight: bold; margin: 5px 0; line-height: 1.2; }

        .container { max-width: 1100px; margin: auto; padding: 15px; }
        .card { background: white; border-radius: 15px; padding: 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); }

        /* FLYER RESTAURADO */
        .player-container { width: 100%; text-align: center; margin-bottom: 20px; }
        .player { width: 100%; max-width: 600px; border-radius: 15px; box-shadow: 0 4px 10px rgba(0,0,0,0.2); height: auto; display: inline-block; }

        /* GRILLA BOLETOS */
        .boletos { display: grid; grid-template-columns: repeat(auto-fill, minmax(65px, 1fr)); gap: 8px; }
        .boleto { padding: 12px 2px; border-radius: 8px; font-weight: bold; cursor: pointer; background: #eeeeee; text-align: center; font-size: 14px; box-shadow: 0 2px 4px rgba(0,0,0,0.1); transition: 0.2s; }
        
        .boleto.seleccionado { background: #00c853 !important; color: white !important; }
        .boleto.vendido { background: #666666 !important; color: white !important; cursor: not-allowed !important; opacity: 0.6; }

        .resumen { background: #f1f8e9; padding: 15px; border-radius: 10px; margin: 20px 0; border-left: 5px solid #00c853; }
        input { width: 100%; padding: 12px; margin-top: 10px; border-radius: 8px; border: 1px solid #ccc; box-sizing: border-box; }
        #btnPagar { width: 100%; margin-top: 15px; padding: 16px; border: none; border-radius: 10px; background: #00c853; color: white; font-size: 18px; font-weight: bold; cursor: pointer; }
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
        <p>POR CADA BOLETO QUE COMPRES TIENES 10 OPORTUNIDADES MÁS TOTALMENTE GRATIS</p>
    </div>

    <div class="container">
        <div class="card">
            <div class="player-container">
                <img class="player" src="https://raw.githubusercontent.com/angelyalejandro/rifa-los-compas/main/flayer.jpeg" alt="Flyer Informativo">
            </div>
            
            <h3 style="text-align: center;">Selecciona tus boletos:</h3>
            <div class="boletos" id="boletos"></div>

            <div class="resumen">
                <strong>Boletos:</strong> <span id="cantidad">0</span> | <strong>Total:</strong> $<span id="total">0</span>
            </div>

            <input type="text" id="nombreCliente" placeholder="Tu nombre completo">
            <button id="btnPagar" onclick="pagar()">Finalizar Compra</button>
        </div>
    </div>
</div>

<div id="pagos" class="seccion" style="display:none;">
    <div class="container">
        <div class="card" style="text-align:center; background:#0f6c6c; color:white; padding: 30px 15px;">
            <h2>VENDEDORES AUTORIZADOS</h2>
            <div style="margin-bottom:20px;">
                <p><strong>Luis Alejandro Romero Sebastian</strong><br>📱 7421199270</p>
            </div>
            <hr style="border: 0; border-top: 1px solid rgba(255,255,255,0.2);">
            <div style="margin-top:20px;">
                <p><strong>Angel Gabriel Urioste Luciano</strong><br>📱 7421292436</p>
            </div>
        </div>
    </div>
</div>

<script>
const PRECIO_BOLETO = 50;
const TOTAL_BOLETOS = 400;
const TELEFONO = "527421199270";
const URL_SCRIPT = "https://script.google.com/macros/s/AKfycbwECOIKV6pNBp2gi8Q51cPnNmVnTULWO0bRVi7-VK7xXUHN2isPL6HMgElhfHTkKSaN/exec";

let seleccionados = new Set();
let vendidos = [];
let gratisPorBoleto = {};

function mostrarSeccion(id) {
    document.querySelectorAll(".seccion").forEach(s => s.style.display = "none");
    document.getElementById(id).style.display = "block";
    window.scrollTo(0,0);
}

function cargarDatos() {
    fetch(URL_SCRIPT + "?t=" + new Date().getTime())
    .then(res => res.json())
    .then(data => {
        // Marcado de vendidos según estado en Excel
        vendidos = (data.vendidos || []).map(n => n.toString().trim().padStart(4, "0"));
        gratisPorBoleto = data.gratisPorBoleto || {};
        generarBoletos();
    })
    .catch(err => {
        console.error("Error:", err);
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
    if(seleccionados.size === 0 || nombre === "") return alert("Faltan datos por completar");

    const boletosArray = Array.from(seleccionados);
    let todosLosGratis = [];

    // Jalar boletos gratis de columnas D a L
    boletosArray.forEach(b => {
        if(gratisPorBoleto[b]) todosLosGratis.push(...gratisPorBoleto[b]);
    });

    const msg = `*RIFA LOS COMPAS*%0A👤 *Nombre:* ${nombre}%0A🎫 *Boletos:* ${boletosArray.join(", ")}%0A🎁 *Gratis:* ${todosLosGratis.length > 0 ? todosLosGratis.join(", ") : "Ninguno"}%0A💰 *Total:* $${boletosArray.length * PRECIO_BOLETO}`;

    window.open(`https://wa.me/${TELEFONO}?text=${msg}`, "_blank");

    fetch(URL_SCRIPT, {
        method: "POST",
        mode: "no-cors",
        body: JSON.stringify({ nombre: nombre, boletos: boletosArray })
    }).finally(() => {
        alert("¡Apartado solicitado correctamente!");
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
