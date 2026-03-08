<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RIFAS LOS COMPAS</title>
    <style>
        body { font-family: 'Segoe UI', sans-serif; margin: 0; background: #e9f7e9; text-align: center; }
        .menu { display: flex; justify-content: center; gap: 10px; padding: 15px; background: #0d47a1; }
        .menu button { background: white; color: #0d47a1; font-weight: bold; border: none; padding: 10px 20px; border-radius: 8px; cursor: pointer; }
        .banner { background: linear-gradient(135deg, #ffe082, #80deea); padding: 25px; }
        .banner h1 { font-size: 34px; color: #0d47a1; margin: 10px 0; text-shadow: 1px 1px 2px rgba(0,0,0,0.1); }
        .banner img { max-width: 180px; border-radius: 50%; box-shadow: 0 4px 8px rgba(0,0,0,0.2); }
        .container { max-width: 1000px; margin: auto; padding: 20px; }
        .card { background: white; border-radius: 15px; padding: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); }
        .boletos { display: grid; grid-template-columns: repeat(auto-fill, minmax(65px, 1fr)); gap: 8px; margin-top: 20px; }
        .boleto { padding: 12px 2px; border-radius: 8px; font-weight: bold; background: #eeeeee; cursor: pointer; font-size: 14px; transition: 0.2s; border: 1px solid #ddd; color: #333; }
        .boleto.seleccionado { background: #00c853 !important; color: white !important; border-color: #00a042; }
        .boleto.vendido { background: #d32f2f !important; color: white !important; cursor: not-allowed; opacity: 0.5; text-decoration: line-through; }
        .resumen { background: #f1f8e9; padding: 15px; margin: 20px 0; border-radius: 10px; border: 1px solid #c8e6c9; }
        #btnPagar { width: 100%; padding: 15px; background: #00c853; color: white; border: none; border-radius: 10px; font-size: 18px; font-weight: bold; cursor: pointer; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
        #btnPagar:disabled { background: #aaa; cursor: not-allowed; }
        input[type="text"] { width: 90%; padding: 12px; border-radius: 8px; border: 1px solid #ccc; font-size: 16px; margin-top: 10px; }
    </style>
</head>
<body>

<nav class="menu">
    <button onclick="location.reload()">Actualizar Boletos</button>
</nav>

<div class="banner">
    <img src="https://raw.githubusercontent.com/angelyalejandro/rifa-los-compas/main/logo.JPG">
    <h1>RIFAS LOS COMPAS</h1>
</div>

<div class="container">
    <div class="card">
        <img style="width:100%; border-radius:10px; margin-bottom:15px;" src="https://raw.githubusercontent.com/angelyalejandro/rifa-los-compas/main/flayer.jpeg">
        
        <div id="boletos" class="boletos">Cargando lista de boletos...</div>

        <div class="resumen">
            <p style="font-size: 18px;">Boletos: <b id="cantidad">0</b> | Total: $<b id="total">0</b></p>
            <input type="text" id="nombreCliente" placeholder="Escribe tu nombre completo">
        </div>

        <button id="btnPagar" onclick="pagar()">Finalizar y Apartar</button>
    </div>
</div>

<script>
    const PRECIO = 50;
    const TOTAL_BOLETOS = 400;
    const TELEFONO = "527421199270";
    
    // URL de lectura CSV (Publicada)
    const URL_LECTURA = "https://docs.google.com/spreadsheets/d/e/2PACX-1vQPcFnbquGADSgLL6WHeSYdCyl6aCa3VlouguKC57RIxAf0yYbM1HifCC10fgcMnpFwWmv8FVsnQrxU/pub?gid=1689723674&single=true&output=csv";
    
    // TU NUEVA URL DE APPS SCRIPT
    const URL_SCRIPT = "https://script.google.com/macros/s/AKfycbxApWBqhsLG-aT3e7gf2xwbi26h2-3a5ZelAcbwcV2m0afndEFlRVHJWYrymDWI1IN7Xw/exec";

    let seleccionados = new Set();
    let vendidos = [];

    async function cargarVendidos() {
        try {
            const res = await fetch(URL_LECTURA + "?t=" + Date.now());
            const csv = await res.text();
            const filas = csv.split("\n").slice(1);
            
            vendidos = filas
                .filter(f => {
                    const cols = f.split(",");
                    return cols[1] && cols[1].trim().toUpperCase() === "VENDIDO";
                })
                .map(f => f.split(",")[0].trim().padStart(4, "0"));
            
            generarVisual();
        } catch (e) {
            console.error("Error al cargar datos:", e);
            generarVisual(); 
        }
    }

    function generarVisual() {
        const contenedor = document.getElementById("boletos");
        contenedor.innerHTML = "";
        for (let i = 1; i <= TOTAL_BOLETOS; i++) {
            const num = i.toString().padStart(4, "0");
            const btn = document.createElement("div");
            btn.textContent = num;
            btn.className = "boleto";
            
            if (vendidos.includes(num)) {
                btn.classList.add("vendido");
            } else {
                if (seleccionados.has(num)) btn.classList.add("seleccionado");
                btn.onclick = () => {
                    if (seleccionados.has(num)) seleccionados.delete(num);
                    else seleccionados.add(num);
                    actualizarResumen();
                    generarVisual();
                };
            }
            contenedor.appendChild(btn);
        }
    }

    function actualizarResumen() {
        document.getElementById("cantidad").textContent = seleccionados.size;
        document.getElementById("total").textContent = seleccionados.size * PRECIO;
    }

    async function pagar() {
        const nombre = document.getElementById("nombreCliente").value.trim();
        if (seleccionados.size === 0) return alert("Por favor, selecciona al menos un boleto.");
        if (!nombre) return alert("Por favor, ingresa tu nombre completo.");

        const btn = document.getElementById("btnPagar");
        btn.disabled = true;
        btn.textContent = "Procesando...";

        const seleccionadosArray = Array.from(seleccionados);
        let listaRegalos = [];

        try {
            // 1. Obtener regalos desde el Script (vía GET)
            for (let bol of seleccionadosArray) {
                try {
                    const r = await fetch(`${URL_SCRIPT}?boleto=${bol}`);
                    const resJson = await r.json();
                    if (resJson.regalos && resJson.regalos.length > 0) {
                        listaRegalos.push(...resJson.regalos);
                    }
                } catch(e) { console.log("Sin regalos para " + bol); }
            }

            // 2. Registrar venta en la hoja (vía POST)
            await fetch(URL_SCRIPT, {
                method: "POST",
                mode: "no-cors",
                body: JSON.stringify({ nombre: nombre, boletos: seleccionadosArray })
            });

            // 3. Preparar mensaje y enviar a WhatsApp
            let textoRegalos = listaRegalos.length > 0 ? `%0A🎁 *REGALOS:* ${listaRegalos.join(", ")}` : "";
            const msg = `Hola RIFAS LOS COMPAS, quiero apartar boletos%0A%0A*Nombre:* ${nombre}%0A*Boletos:* ${seleccionadosArray.join(", ")}${textoRegalos}%0A*Total:* $${seleccionadosArray.length * PRECIO}`;
            
            window.open(`https://wa.me/${TELEFONO}?text=${msg}`);
            
            // Limpiar estado
            seleccionados.clear();
            document.getElementById("nombreCliente").value = "";
            actualizarResumen();
            
            // Esperar un momento y recargar la lista
            setTimeout(cargarVendidos, 2500);

        } catch (err) {
            alert("Hubo un error de conexión, pero tus boletos podrían haberse registrado. Verifica en WhatsApp.");
        } finally {
            btn.disabled = false;
            btn.textContent = "Finalizar y Apartar";
        }
    }

    // Iniciar carga
    cargarVendidos();
    // Auto-actualizar cada 30 segundos para ver boletos vendidos por otros
    setInterval(cargarVendidos, 30000);
</script>

</body>
</html>
