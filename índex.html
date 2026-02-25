<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RIFAS LOS COMPAS</title>
    <style>
        body{font-family:'Segoe UI',sans-serif;margin:0;background:#e9f7e9;}
        .menu{display:flex;justify-content:center;gap:10px;padding:15px;background:#0d47a1;position:sticky;top:0;z-index:1000;}
        .menu button{background:white;color:#0d47a1;font-weight:bold;border:none;padding:10px 20px;border-radius:8px;cursor:pointer;transition: 0.3s;}
        .menu button:hover{background:#ffe082;}
        .banner{background:linear-gradient(135deg,#ffe082,#80deea);padding:25px;text-align:center;}
        .banner h1{font-size:34px;margin:10px 0;color:#0d47a1;}
        .banner img{max-width:200px;display:block;margin:auto;border-radius: 50%;}
        .container{max-width:1100px;margin:auto;padding:20px;}
        .card{background:white;border-radius:15px;padding:20px;box-shadow: 0 4px 15px rgba(0,0,0,0.1);}
        .player{width:100%;border-radius:15px;margin-bottom:20px;box-shadow:0 4px 10px rgba(0,0,0,.2);}
        .boletos{display:grid;grid-template-columns:repeat(auto-fill,minmax(70px,1fr));gap:10px;margin-top: 20px;}
        .boleto{padding:14px 6px;border-radius:12px;font-weight:bold;cursor:pointer;background:#eeeeee;text-align:center;font-size:15px;box-shadow:0 3px 6px rgba(0,0,0,.15);transition:.2s;}
        .boleto:hover{transform:scale(1.05); background: #e0e0e0;}
        .boleto.seleccionado{background:#00c853 !important;color:white;}
        .boleto.vendido{background:#ccc !important;color:#666 !important;cursor:not-allowed;transform: none;}
        .resumen{background: #f8f9fa; padding: 15px; border-radius: 10px; margin-top: 20px; border-left: 5px solid #00c853;}
        input[type="text"]{width:100%; padding:12px; margin-top:10px; border: 1px solid #ccc; border-radius: 8px; box-sizing: border-box;}
        #btnPagar{width: 100%; margin-top:15px; padding:15px; border:none; border-radius:10px; background:#00c853; color:white; font-size:18px; font-weight:bold; cursor:pointer;}
        #btnPagar:disabled{background:gray;}
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
    </div>

    <div class="container">
        <div class="card">
            <img class="player" src="https://raw.githubusercontent.com/angelyalejandro/rifa-los-compas/main/flayer.jpeg" alt="Flyer">

            <h3>Selecciona tus boletos:</h3>
            <div class="boletos" id="boletos"></div>

            <div class="resumen">
                <strong>Boletos seleccionados:</strong> <span id="cantidad">0</span><br>
                <strong>Total a pagar:</strong> $<span id="total">0</span>
            </div>

            <input type="text" id="nombreCliente" placeholder="Escribe tu nombre completo">
            <button id="btnPagar" onclick="pagar()">Finalizar Compra y Apartar</button>
        </div>
    </div>
</div>

<div id="pagos" class="seccion" style="display:none;">
    <div class="container">
        <div class="card" style="text-align:center; background:#0f6c6c; color:white;">
            <h1 style="font-size:40px;">VENDEDORES AUTORIZADOS</h1>
            <div style="margin-bottom:40px;">
                <h2>Luis Alejandro Romero Sebastian ✅</h2>
                <div style="background:red; padding:15px; border-radius:12px; font-size:26px; font-weight:bold; margin: 10px 0;">
                    📱 7421199270
                </div>
                <p><strong>Tarjeta Débito BBVA:</strong><br>4152 3140 2646 1213</p>
                <p><strong>Cuenta Clabe BBVA:</strong><br>012180015406075891</p>
            </div>
            <hr style="margin:40px 0; border: 0; border-top: 1px solid rgba(255,255,255,0.3);">
            <div>
                <h2>Angel Gabriel Urioste Luciano ✅</h2>
                <div style="background:red; padding:15px; border-radius:12px; font-size:26px; font-weight:bold; margin: 10px 0;">
                    📱 7421292436
                </div>
                <p><strong>Tarjeta Débito BBVA:</strong><br>4152 3145 7352 6715</p>
                <p><strong>Cuenta Clabe BBVA:</strong><br>012180015751433706</p>
            </div>
        </div>
    </div>
</div>

<script>
const PRECIO_BOLETO = 50;
const TOTAL_BOLETOS = 400;
const TELEFONO_WHATSAPP = "527421199270";
// SUSTITUYE ESTA URL POR TU ÚLTIMA IMPLEMENTACIÓN DE APPS SCRIPT
const URL_SCRIPT = "https://script.google.com/macros/s/AKfycbyxSwV7JliEFpRq5nuGHBP2OAQqAV3JsbqElFc6t51Bl5oH4So0gmWJ7eYlPIVXKjHw/exec";

const contenedor = document.getElementById("boletos");
const seleccionados = new Set();
let vendidos = [];
let gratisPorBoleto = {};

/* NAVEGACIÓN ENTRE SECCIONES */
function mostrarSeccion(id){
    const secciones = document.querySelectorAll(".seccion");
    secciones.forEach(sec => sec.style.display = "none");
    document.getElementById(id).style.display = "block";
    window.scrollTo(0,0);
}

/* CARGAR ESTADO DE BOLETOS */
function cargarVendidos(){
    fetch(URL_SCRIPT)
    .then(res => res.json())
    .then(data => {
        console.log("Datos actualizados:", data);
        vendidos = data.vendidos || [];
        gratisPorBoleto = data.gratisPorBoleto || {};
        generarBoletos();
    })
    .catch(err => {
        console.error("Error cargando datos:", err);
        // Si hay error de red, generamos los boletos igual para que la web no sea vea vacía
        if(contenedor.innerHTML === "") generarBoletos();
    });
}

/* DIBUJAR LOS CUADRITOS DE LOS BOLETOS */
function generarBoletos(){
    contenedor.innerHTML = "";
    for(let i=1; i<=TOTAL_BOLETOS; i++){
        const num = i.toString().padStart(4, "0");
        const div = document.createElement("div");
        div.className = "boleto";
        div.textContent = num;

        // Si el boleto está en la lista de vendidos
        if(vendidos.includes(num)){
            div.classList.add("vendido");
        } else {
            if(seleccionados.has(num)) div.classList.add("seleccionado");
            div.onclick = () => seleccionarBoleto(num, div);
        }

        contenedor.appendChild(div);
    }
}

/* SELECCIONAR / DESELECCIONAR */
function seleccionarBoleto(num, div){
    if(div.classList.contains("vendido")) return;
    
    if(seleccionados.has(num)){
        seleccionados.delete(num);
        div.classList.remove("seleccionado");
    } else {
        seleccionados.add(num);
        div.classList.add("seleccionado");
    }
    actualizarTotales();
}

function actualizarTotales(){
    document.getElementById("cantidad").textContent = seleccionados.size;
    document.getElementById("total").textContent = seleccionados.size * PRECIO_BOLETO;
}

/* PROCESO DE PAGO / WHATSAPP */
function pagar(){
    const nombre = document.getElementById("nombreCliente").value.trim();
    
    if(seleccionados.size === 0){
        alert("Por favor, selecciona al menos un boleto.");
        return;
    }
    if(nombre === ""){
        alert("Por favor, ingresa tu nombre completo.");
        return;
    }

    const boletosArray = Array.from(seleccionados);
    let boletosRegalo = [];

    // Buscar si los boletos seleccionados tienen boletos gratis asociados
    boletosArray.forEach(b => {
        if(gratisPorBoleto[b] && gratisPorBoleto[b].length > 0){
            boletosRegalo.push(...gratisPorBoleto[b]);
        }
    });

    // Formatear mensaje de WhatsApp
    const mensaje = `*RIFAS LOS COMPAS*%0A` +
                    `--------------------------%0A` +
                    `👤 *Nombre:* ${nombre}%0A` +
                    `🎫 *Boletos:* ${boletosArray.join(", ")}%0A` +
                    `🎁 *Boletos Gratis:* ${boletosRegalo.length > 0 ? boletosRegalo.join(", ") : "Ninguno"}%0A` +
                    `💰 *Total a pagar:* $${boletosArray.length * PRECIO_BOLETO}%0A` +
                    `--------------------------%0A` +
                    `_Envío comprobante de pago en un momento._`;

    // Abrir WhatsApp
    window.open(`https://wa.me/${TELEFONO_WHATSAPP}?text=${mensaje}`, "_blank");

    // Enviar a Google Sheets para marcar como "VENDIDO" (Apartado)
    fetch(URL_SCRIPT, {
        method: "POST",
        mode: "no-cors",
        body: JSON.stringify({ nombre: nombre, boletos: boletosArray })
    }).finally(() => {
        alert("¡Pedido enviado! No olvides mandar tu comprobante por WhatsApp.");
        seleccionados.clear();
        actualizarTotales();
        cargarVendidos();
    });
}

// Carga inicial
cargarVendidos();
// Refrescar cada 20 segundos para ver si alguien más ya compró
setInterval(cargarVendidos, 20000);
</script>

</body>
</html>
