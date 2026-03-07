<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RIFAS LOS COMPAS</title>
    <style>
        body { font-family: 'Segoe UI', sans-serif; margin: 0; background: #e9f7e9; color: #333; }
        .menu { display: flex; justify-content: center; gap: 10px; padding: 15px; background: #0d47a1; position: sticky; top: 0; z-index: 1000; }
        .menu button { background: white; color: #0d47a1; font-weight: bold; border: none; padding: 10px 15px; border-radius: 8px; cursor: pointer; }
        .banner { background: linear-gradient(135deg, #ffe082, #80deea); padding: 25px 15px; text-align: center; }
        .banner img { width: 110px; height: 110px; border-radius: 50%; border: 3px solid white; margin-bottom: 10px; }
        .banner h1 { font-size: 26px; margin: 5px 0; color: #0d47a1; }
        .banner p { font-size: 14px; color: #01579b; font-weight: bold; margin: 5px 0; }
        .container { max-width: 1100px; margin: auto; padding: 15px; }
        .card { background: white; border-radius: 15px; padding: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); margin-bottom: 20px; }
        .player { width: 100%; max-width: 500px; display: block; margin: 0 auto 20px auto; border-radius: 15px; box-shadow: 0 4px 10px rgba(0,0,0,0.2); }
        .boletos { display: grid; grid-template-columns: repeat(auto-fill, minmax(65px, 1fr)); gap: 8px; min-height: 200px; }
        .boleto { padding: 12px 2px; border-radius: 8px; font-weight: bold; cursor: pointer; background: #eeeeee; text-align: center; font-size: 14px; border: 1px solid #ddd; }
        .boleto.seleccionado { background: #00c853 !important; color: white !important; }
        .boleto.vendido { background: #666666 !important; color: white !important; cursor: not-allowed !important; opacity: 0.6; pointer-events: none; }
        .resumen { background: #f1f8e9; padding: 15px; border-radius: 10px; margin: 20px 0; border-left: 5px solid #00c853; }
        input { width: 100%; padding: 12px; margin-top: 10px; border-radius: 8px; border: 1px solid #ccc; box-sizing: border-box; font-size: 16px; }
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
        <p>POR CADA BOLETO QUE COMPRES TIENES 10 OPORTUNIDADES MÁS GRATIS</p>
    </div>
    <div class="container">
        <div class="card">
            <img class="player" src="https://raw.githubusercontent.com/angelyalejandro/rifa-los-compas/main/flayer.jpeg" alt="Flyer">
            <h3 style="text-align: center;">Selecciona tus boletos:</h3>
            <div id="mensaje-carga" style="text-align: center; padding: 20px; font-weight: bold; color: #0d47a1;">Cargando disponibilidad de boletos...</div>
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
        <div class="card">
            <h2 style="text-align: center; color: #0d47a1;">PAGOS</h2>
            <p><strong>Luis Alejandro Romero:</strong> 4152 3140 2646 1213 (BBVA)</p>
            <p><strong>Angel Gabriel Urioste:</strong> 4152 3145 7352 6715 (BBVA)</p>
        </div>
    </div>
</div>

<script>
const PRECIO_BOLETO = 50;
const TOTAL_BOLETOS = 400;
const TELEFONO = "527421199270";
const URL_SCRIPT = "https://script.google.com/macros/s/AKfycbzcYZiz7bNrlN7xEdr-OYSZX7V-GZYs_VNK3nkZhrU1Df4sTO-b3gsYF2an8XMxSGM-/exec";

let seleccionados = new Set();
let vendidos = [];
let gratisPorBoleto = {};

function mostrarSeccion(id) {
    document.querySelectorAll(".seccion").forEach(s => s.style.display = "none");
    document.getElementById(id).style.display = "block";
}

// FUNCIÓN DE CARGA MEJORADA
async function cargarDatos() {
    try {
        const response = await fetch(URL_SCRIPT + "?t=" + new Date().getTime());
        if (!response.ok) throw new Error('Error en la red');
        
        const data = await response.json();
        
        if(data) {
            vendidos = (data.vendidos || []).map(n => String(n).padStart(4, "0"));
            gratisPorBoleto = data.gratisPorBoleto || {};
            document.getElementById("mensaje-carga").style.display = "none";
        }
    } catch (error) {
        console.error("Error cargando datos de Google:", error);
        // Si falla, avisamos pero permitimos que los boletos se dibujen
        document.getElementById("mensaje-carga").innerText = "Mostrando boletos (Modo catálogo - No se pudo verificar disponibilidad)";
        document.getElementById("mensaje-carga").style.color = "red";
    }
    generarBoletos();
}

function generarBoletos() {
    const contenedor = document.getElementById("boletos");
    contenedor.innerHTML = "";

    for (let i = 1; i <= TOTAL_BOLETOS; i++) {
        const num = String(i).padStart(4, "0");
        const div = document.createElement("div");
        div.className = "boleto";
        div.innerText = num;

        if (vendidos.includes(num)) {
            div.classList.add("vendido");
        } else {
            if (seleccionados.has(num)) div.classList.add("seleccionado");
            div.onclick = () => {
                if (seleccionados.has(num)) {
                    seleccionados.delete(num);
                } else {
                    seleccionados.add(num);
                }
                generarBoletos();
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
    if (seleccionados.size === 0 || nombre === "") return alert("Selecciona tus boletos e ingresa tu nombre");

    const boletosArray = Array.from(seleccionados);
    let boletosRegalo = [];
    boletosArray.forEach(b => {
        if (gratisPorBoleto[b]) boletosRegalo.push(...gratisPorBoleto[b]);
    });

    const msg = `*RIFA LOS COMPAS*%0A👤 *Nombre:* ${nombre}%0A🎫 *Boletos:* ${boletosArray.join(", ")}%0A🎁 *Gratis:* ${boletosRegalo.length > 0 ? boletosRegalo.join(", ") : "Ninguno"}%0A💰 *Total:* $${boletosArray.length * PRECIO_BOLETO}`;

    window.open(`https://wa.me/${TELEFONO}?text=${msg}`, "_blank");

    fetch(URL_SCRIPT, {
        method: "POST",
        mode: "no-cors",
        body: JSON.stringify({ nombre: nombre, boletos: boletosArray })
    }).finally(() => {
        alert("Apartado enviado");
        seleccionados.clear();
        actualizarTotales();
        cargarDatos();
    });
}

// INICIO
cargarDatos();
</script>
</body>
</html>
