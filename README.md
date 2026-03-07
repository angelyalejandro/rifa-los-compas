
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
        .boleto { padding: 12px 2px; border-radius: 8px; font-weight: bold; cursor: pointer; background: #eeeeee; text-align: center; font-size: 14px; border: 1px solid #ddd; }
        
        .boleto.seleccionado { background: #00c853 !important; color: white !important; }
        .boleto.vendido { background: #666666 !important; color: white !important; cursor: not-allowed !important; opacity: 0.6; pointer-events: none; }

        /* PAGOS (ESTILO ANTERIOR) */
        .vendedor-card { text-align: left; border-bottom: 1px solid #eee; padding: 20px 0; }
        .vendedor-nombre { font-size: 18px; font-weight: bold; color: #333; margin-bottom: 10px; }
        .vendedor-info { font-size: 15px; line-height: 1.6; color: #555; }

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
            <div id="status-conexion" style="text-align: center; color: orange; font-weight: bold; margin-bottom: 10px;">Verificando disponibilidad...</div>
            <div class="boletos" id="boletos"></div>
            
            <div class="resumen">
                <strong>Boletos:</strong> <span id="cantidad">0</span> | <strong>Total:</strong> $<span id="total">0</span>
            </div>
            <input type="text" id="nombreCliente" placeholder="Nombre completo del cliente">
            <button id="btnPagar" onclick="pagar()">Finalizar Compra</button>
        </div>
    </div>
</div>

<div id="pagos" class="seccion" style="display:none;">
    <div class="container">
        <div class="card">
            <h2 style="text-align: center; color: #0d47a1;">VENDEDORES AUTORIZADOS</h2>
            
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
            
            <button onclick="mostrarSeccion('rifa')" style="margin-top:20px; width:100%; padding:10px; cursor:pointer;">Volver a los boletos</button>
        </div>
    </div>
</div>

<script>
const TOTAL_BOLETOS = 400;
const PRECIO = 50;
const WHATSAPP = "527421199270";
const URL_API = "https://script.google.com/macros/s/AKfycbzcYZiz7bNrlN7xEdr-OYSZX7V-GZYs_VNK3nkZhrU1Df4sTO-b3gsYF2an8XMxSGM-/exec";

let seleccionados = new Set();
let vendidos = [];
let regalos = {};

function mostrarSeccion(id) {
    document.querySelectorAll('.seccion').forEach(s => s.style.display = 'none');
    document.getElementById(id).style.display = 'block';
    window.scrollTo(0,0);
}

function dibujarBoletos() {
    const contenedor = document.getElementById("boletos");
    if(!contenedor) return;
    let html = "";
    for (let i = 1; i <= TOTAL_BOLETOS; i++) {
        const num = String(i).padStart(4, "0");
        let clase = "boleto";
        if (vendidos.includes(num)) clase += " vendido";
        else if (seleccionados.has(num)) clase += " seleccionado";
        
        html += `<div class="${clase}" onclick="toggleBoleto('${num}')">${num}</div>`;
    }
    contenedor.innerHTML = html;
}

function toggleBoleto(num) {
    if (vendidos.includes(num)) return;
    if (seleccionados.has(num)) seleccionados.delete(num);
    else seleccionados.add(num);
    
    dibujarBoletos();
    document.getElementById("cantidad").innerText = seleccionados.size;
    document.getElementById("total").innerText = seleccionados.size * PRECIO;
}

async function actualizarDesdeGoogle() {
    try {
        const respuesta = await fetch(URL_API + "?t=" + Date.now());
        const json = await respuesta.json();
        
        if (json && json.vendidos) {
            vendidos = json.vendidos.map(n => String(n).padStart(4, "0"));
            regalos = json.gratisPorBoleto || {};
            document.getElementById("status-conexion").innerText = "✓ Disponibilidad actualizada";
            document.getElementById("status-conexion").style.color = "green";
            dibujarBoletos();
        }
    } catch (e) {
        document.getElementById("status-conexion").innerText = "⚠ Modo Catálogo (Sin conexión)";
        document.getElementById("status-conexion").style.color = "red";
    }
}

function pagar() {
    const nombre = document.getElementById("nombreCliente").value.trim();
    if (seleccionados.size === 0 || !nombre) return alert("Selecciona boletos y escribe tu nombre");

    const lista = Array.from(seleccionados);
    let misRegalos = [];
    lista.forEach(n => { if(regalos[n]) misRegalos.push(...regalos[n]); });

    const texto = `*RIFA LOS COMPAS*%0A👤 *Nombre:* ${nombre}%0A🎫 *Boletos:* ${lista.join(", ")}%0A🎁 *Regalos:* ${misRegalos.join(", ") || "Ninguno"}%0A💰 *Total:* $${lista.length * PRECIO}`;
    
    window.open(`https://wa.me/${WHATSAPP}?text=${texto}`, "_blank");

    fetch(URL_API, { method: "POST", mode: "no-cors", body: JSON.stringify({ nombre: nombre, boletos: lista }) })
    .finally(() => {
        alert("¡Apartado enviado!");
        seleccionados.clear();
        dibujarBoletos();
        actualizarDesdeGoogle();
    });
}

// INICIO
dibujarBoletos();
actualizarDesdeGoogle();
</script>
</body>
</html>
