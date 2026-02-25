
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
        .boleto.vendido { background: #666666 !important; color: white !important; cursor: not-allowed !important; opacity: 0.6; }

        /* FORMAS DE PAGO (Estilo tipo tarjeta blanca) */
        .vendedor-card { text-align: left; border-bottom: 1px solid #eee; padding: 20px 0; }
        .vendedor-card:last-child { border-bottom: none; }
        .vendedor-nombre { font-size: 18px; font-weight: bold; color: #333; display: flex; align-items: center; gap: 8px; margin-bottom: 10px; }
        .vendedor-info { font-size: 15px; line-height: 1.6; color: #555; margin-left: 28px; }

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
            <h2 style="border-bottom: 2px solid #eee; padding-bottom: 10px; text-align: right; color: #333;">VENDEDORES</h2>
            
            <div class="vendedor-card">
                <div class="vendedor-nombre">✅ Luis Alejandro Romero Sebastian</div>
                <div class="vendedor-info">
                    <strong>WhatsApp:</strong> 7421199270<br>
                    <strong>Tarjeta Débito BBVA:</strong> 4152 3140 2646 1213<br>
                    <strong>Cuenta Clabe BBVA:</strong> 012180015406075891
                </div>
            </div>

            <div class="vendedor-card">
                <div class="vendedor-nombre">✅ Angel Gabriel Urioste Luciano</div>
                <div class="vendedor-info">
                    <strong>WhatsApp:</strong> 7421292436<br>
                    <strong>Tarjeta Débito BBVA:</strong> 4152 3145 7352 6715<br>
                    <strong>Cuenta Clabe BBVA:</strong> 012180015751433706
                </div>
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
        // Bloqueo de boletos VENDIDOS desde Excel
        vendidos = (data.vendidos || []).map(n => n.toString().trim().padStart(4, "0"));
        gratisPorBoleto = data.gratisPorBoleto || {};
        generarBoletos();
    })
    .catch(err => console.error("Error:", err));
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
                if(seleccionados.has(num)) seleccionados.delete(num);
                else seleccionados.add(num);
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
    if(seleccionados.size === 0 || nombre === "") return alert("Faltan datos");

    const boletosArray = Array.from(seleccionados);
    let boletosRegalo = [];

    // Extraer boletos gratis de la memoria (Columnas D a L)
    boletosArray.forEach(b => {
        if(gratisPorBoleto[b]) boletosRegalo.push(...gratisPorBoleto[b]);
    });

    // MENSAJE DE WHATSAPP ACTUALIZADO
    const msg = `*RIFA LOS COMPAS*%0A` +
                `Hola, reserve los siguientes boletos:%0A%0A` +
                `👤 *Nombre:* ${nombre}%0A` +
                `🎫 *Boletos:* ${boletosArray.join(", ")}%0A` +
                `🎁 *Gratis:* ${boletosRegalo.length > 0 ? boletosRegalo.join(", ") : "Ninguno"}%0A` +
                `💰 *Total:* $${boletosArray.length * PRECIO_BOLETO}`;

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

cargarDatos();
setInterval(cargarDatos, 15000);
</script>
</body>
</html>
