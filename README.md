
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rifa Los Compas</title>

<style>
body{
    font-family: Arial, sans-serif;
    background:#f4f4f4;
    margin:0;
    padding:0;
    text-align:center;
}

.container{
    max-width:500px;
    margin:20px auto;
    background:white;
    padding:20px;
    border-radius:10px;
    box-shadow:0px 0px 10px rgba(0,0,0,0.1);
}

.logo{
    width:150px;
    margin-bottom:10px;
}

h1{
    color:#333;
}

input{
    width:90%;
    padding:10px;
    margin:8px 0;
    border-radius:5px;
    border:1px solid #ccc;
    font-size:16px;
}

button{
    background:#28a745;
    color:white;
    border:none;
    padding:12px;
    width:95%;
    font-size:18px;
    border-radius:5px;
    cursor:pointer;
}

button:hover{
    background:#218838;
}

#mensaje{
    margin-top:10px;
    font-weight:bold;
}
</style>

</head>
<body>

<div class="container">

<img src="https://github.com/angelyalejandro/rifa-los-compas/blob/main/logo.JPG?raw=true" class="logo">

<h1>Rifa Los Compas</h1>

<input type="text" id="nombre" placeholder="Nombre" required>
<input type="text" id="telefono" placeholder="Teléfono" required>
<input type="number" id="cantidad" placeholder="Cantidad de boletos" required>

<button onclick="enviarDatos()">Participar</button>

<div id="mensaje"></div>

</div>

<script>

const URL_SCRIPT = "https://script.google.com/macros/s/AKfycbwNNIRy0cxHTAttdssLt74wEmGWMBXbWhB2vVXDreEQprMraNc8psO1sHYPwCOq8ds4/exec";

function enviarDatos(){

    let nombre = document.getElementById("nombre").value;
    let telefono = document.getElementById("telefono").value;
    let cantidad = document.getElementById("cantidad").value;

    if(!nombre || !telefono || !cantidad){
        document.getElementById("mensaje").innerHTML = "⚠️ Completa todos los campos";
        return;
    }

    fetch(URL_SCRIPT,{
        method:"POST",
        body:JSON.stringify({
            nombre:nombre,
            telefono:telefono,
            cantidad:cantidad
        }),
        headers:{
            "Content-Type":"application/json"
        }
    })
    .then(res=>res.text())
    .then(data=>{
        document.getElementById("mensaje").innerHTML = "✅ Registro enviado correctamente";
        document.getElementById("nombre").value="";
        document.getElementById("telefono").value="";
        document.getElementById("cantidad").value="";
    })
    .catch(err=>{
        document.getElementById("mensaje").innerHTML = "❌ Error al enviar";
    });

}

</script>

</body>
</html>
