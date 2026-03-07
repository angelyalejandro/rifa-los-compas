

<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RIFAS LOS COMPAS</title>
    <style>
        body { font-family: 'Segoe UI', sans-serif; margin: 0; background: #e9f7e9; color: #333; }
        
        /* MENÚ */
        .menu { display: flex; justify-content: center; gap: 10px; padding: 15px; background: #0d47a1; position: sticky; top: 0; z-index: 1000; }
        .menu button { background: white; color: #0d47a1; font-weight: bold; border: none; padding: 10px 15px; border-radius: 8px; cursor: pointer; }

        /* BANNER */
        .banner { background: linear-gradient(135deg, #ffe082, #80deea); padding: 25px 15px; text-align: center; }
        .banner img { width: 110px; height: 110px; border-radius: 50%; border: 3px solid white; margin-bottom: 10px; }
        .banner h1 { font-size: 26px; margin: 5px 0; color: #0d47a1; }
        .banner p { font-size: 14px; color: #01579b; font-weight: bold; margin: 5px 0; }

        .container { max-width: 1100px; margin: auto; padding: 15px; }
        .card { background: white; border-radius: 15px; padding: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); margin-bottom: 20px; }

        /* FLYER */
        .player { width: 100%; max-width: 500px; display: block; margin: 0 auto 20px auto; border-radius: 15px; box-shadow: 0 4px 10px rgba(0,0,0,0.2); }

        /* BOLETOS */
        .boletos { display: grid; grid-template-columns: repeat(auto-fill, minmax(65px, 1fr)); gap: 8px; }
        .boleto { padding: 12px 2px; border-radius: 8px; font-weight: bold; cursor: pointer; background: #eeeeee; text-align: center; font-size: 14px; border: 1px solid #ddd; transition: 0.3s; }
        
        /* ESTADOS DE LOS BOLETOS */
        .boleto.seleccionado { background: #00c853 !important; color: white !important; transform: scale(1.05); }
        .boleto.vendido { background: #666666 !important; color: white !important; cursor: not-allowed !important; opacity: 0.6; pointer-events: none; }

        /* RESUMEN Y BOTONES */
        .resumen { background: #f1f8e9; padding: 15px; border-radius: 10px; margin: 20px 0; border-left: 5px solid #00c853; }
        input { width: 100%; padding: 12px; margin-top: 10px; border-radius: 8px; border: 1px solid #ccc; box-sizing: border-box; font-size: 16px; }
        #btnPagar { width: 100%; margin-top: 15px; padding: 16px; border: none; border-radius: 10px; background: #00c853; color: white; font-size: 18px; font-weight: bold; cursor: pointer; }
        
        .vendedor-card { text-align: left; border-bottom: 1px solid #eee; padding: 20px 0; }
        .vendedor-nombre { font-size: 18px; font-weight: bold; color: #333; margin-bottom: 10px; }
        .vendedor-info { font-size: 15px; line-height: 1.6; color: #555; }
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
            <div class="boletos" id="boletos"></div>
            
            <div class="resumen">
                <strong>Boletos seleccionados:</strong> <span id="cantidad">0</span> | <strong>Total a pagar:</strong> $<span id="total">0</span>
            </div>
            
            <input type="text" id="nombreCliente" placeholder="Ingresa tu nombre completo">
            <button id="btnPagar" onclick="pagar()">Finalizar Compra / Apartar</button>
        </div>
    </div>
</div>

<div id="pagos" class="seccion" style="display:none;">
    <div class="container">
        <div class="card">
            <h2 style="text-align: center; color: #0d47a1;">INFORMACIÓN DE PAGO</h2>
            <div class="vendedor-card">
                <div class="vendedor-nombre">✅ Luis Alejandro Romero Sebastian</div>
                <div class="vendedor-info">
                    <strong>WhatsApp:</strong> 7421199270<br>
                    <strong>BBVA:</strong> 4152 3140 2646 1213
                </div>
            </div>
            <div class="vendedor-card">
                <div class="vendedor-nombre">✅ Angel Gabriel Urioste Luciano</div>
                <div class="vendedor-info">
                    <strong>WhatsApp:</strong> 7421292436<br>
                    <strong>BBVA:</strong> 4152 3145 7352 6715
                </div>
            </div>
        </div>
    </div>
</div>

<script>
// CONFIGURACIÓN GLOBAL
const PRECIO_BOLETO = 50;
const TOTAL_BOLETOS = 400;
const TELEFONO_WA = "527421199270";
const URL_SCRIPT = "https://script.google.com/macros/s/AKfycbzcYZiz7bNrlN7xEdr-OYSZX7V-GZYs_VNK3nkZhrU1Df4sTO-b3gsYF2an8XMxSGM-/exec";

let seleccionados = new Set();
let listaVendidos = [];
let diccRegalos = {};

function mostrarSeccion(id) {
    document.querySelectorAll(".seccion").forEach(s => s.style.display = "none");
    document.getElementById(id).style.display = "block";
    window.scrollTo(0,0);
}

// 1. CARGAR DATOS DESDE GOOGLE SHEETS
async function cargarDatos() {
    try {
        const res = await fetch(URL_SCRIPT + "?t=" + new Date().getTime());
        const data = await res.json();
        
        // Guardamos los números vendidos que vienen del Excel
        listaVendidos = (data.vendidos || []).map(n => String(n).padStart(4, "0"));
        diccRegalos = data.gratisPorBoleto || {};
        
        generarGridBoletos();
    } catch (err) {
        console.error("Error al conectar con Google Sheets:", err);
        generarGridBoletos();
    }
}

// 2. DIBUJAR LOS BOLETOS EN PANTALLA
function generarGridBoletos() {
    const contenedor = document.getElementById("boletos");
    contenedor.innerHTML = "";

    for (let i = 1; i <= TOTAL_BOLETOS; i++) {
        const numStr = String(i).padStart(4, "0");
        const divBoleto = document.createElement("div");
        divBoleto.className = "boleto";
        divBoleto.innerText = numStr;

        // Si el número está en la lista de vendidos de Google
        if (listaVendidos.includes(numStr)) {
            divBoleto.classList.add("vendido");
        } else {
            // Si está seleccionado por el usuario en esta sesión
            if (seleccionados.has(numStr)) {
                divBoleto.classList.add("seleccionado");
            }

            divBoleto.onclick = () => {
                if (seleccionados.has(numStr)) {
                    seleccionados.delete(numStr);
                } else {
                    seleccionados.add(numStr);
                }
                generarGridBoletos();
                actualizarTotales();
            };
        }
        contenedor.appendChild(divBoleto);
    }
}

function actualizarTotales() {
    const cant = seleccionados.size;
    document.getElementById("cantidad").innerText = cant;
    document.getElementById("total").innerText = cant * PRECIO_BOLETO;
}

// 3. PROCESO DE PAGO Y ENVÍO
function pagar() {
    const nombre = document.getElementById("nombreCliente").value.trim();
    if (seleccionados.size === 0) return alert("Por favor, selecciona al menos un boleto.");
    if (nombre === "") return alert("Por favor, ingresa tu nombre completo.");

    const arrayBoletos = Array.from(seleccionados);
    let todosRegalos = [];

    // Buscar regalos asociados en la información traída de Google
    arrayBoletos.forEach(num => {
        if (diccRegalos[num]) {
            todosRegalos.push(...diccRegalos[num]);
        }
    });

    // Crear mensaje para WhatsApp
    const mensajeWA = `*RIFA LOS COMPAS*%0A` +
                      `✅ *Nuevo Apartado*%0A%0A` +
                      `👤 *Cliente:* ${nombre}%0A` +
                      `🎫 *Boletos:* ${arrayBoletos.join(", ")}%0A` +
                      `🎁 *Regalos:* ${todosRegalos.length > 0 ? todosRegalos.join(", ") : "Ninguno"}%0A` +
                      `💰 *Total:* $${arrayBoletos.length * PRECIO_BOLETO}`;

    // Abrir WhatsApp
    window.open(`https://wa.me/${TELEFONO_WA}?text=${mensajeWA}`, "_blank");

    // Enviar datos al Excel por POST
    fetch(URL_SCRIPT, {
        method: "POST",
        mode: "no-cors",
        body: JSON.stringify({ nombre: nombre, boletos: arrayBoletos })
    }).finally(() => {
        alert("¡Apartado solicitado! Por favor, envía el comprobante por WhatsApp.");
        seleccionados.clear();
        actualizarTotales();
        cargarDatos(); // Recargar para mostrar los nuevos vendidos
    });
}

// Iniciar
cargarDatos();
// Refrescar automáticamente cada minuto por si alguien más compra
setInterval(cargarDatos, 60000);
</script>

</body>
</html>
