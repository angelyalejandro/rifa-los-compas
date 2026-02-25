<script>

const PRECIO_BOLETO=50;
const TOTAL_BOLETOS=400;

/* ✅ TU NUEVO SCRIPT */
const URL_SCRIPT="https://script.google.com/macros/s/AKfycbwNNIRy0cxHTAttdssLt74wEmGWMBXbWhB2vVXDreEQprMraNc8psO1sHYPwCOq8ds4/exec";

/* ⚠️ CAMBIA POR TU NUMERO */
const NUMERO_WHATSAPP="527421199270";

const contenedor=document.getElementById("boletos");
const seleccionados=new Set();
let vendidos=[];


/* CAMBIO DE SECCION */
function mostrarSeccion(id){
 document.querySelectorAll(".seccion").forEach(sec=>{
  sec.style.display="none";
 });
 document.getElementById(id).style.display="block";
}


/* CARGA INICIAL */
async function cargarVendidos(){
 try{
  const res=await fetch(URL_SCRIPT);
  const data=await res.json();
  vendidos=data.map(x=>x.toString().padStart(4,"0"));
  generarBoletos();
 }catch(e){
  generarBoletos();
 }
}

cargarVendidos();

/* ACTUALIZA CADA 10 SEG */
setInterval(cargarVendidos,10000);



function generarBoletos(){

 contenedor.innerHTML="";

 for(let i=1;i<=TOTAL_BOLETOS;i++){

  const num=i.toString().padStart(4,"0");
  const div=document.createElement("div");
  div.className="boleto";
  div.textContent=num;

  if(vendidos.includes(num)){
   div.classList.add("vendido");
  }else{
   if(seleccionados.has(num)){
    div.classList.add("seleccionado");
   }
   div.onclick=()=>toggle(num,div);
  }

  contenedor.appendChild(div);
 }
}



function toggle(num,div){

 if(seleccionados.has(num)){
  seleccionados.delete(num);
  div.classList.remove("seleccionado");
 }else{
  seleccionados.add(num);
  div.classList.add("seleccionado");
 }

 actualizarResumen();
}



function actualizarResumen(){

 document.getElementById("cantidad").textContent=seleccionados.size;
 document.getElementById("total").textContent=seleccionados.size*PRECIO_BOLETO;

}



/* PAGAR */
async function pagar(){

 if(seleccionados.size===0) return alert("Selecciona boletos");

 const nombre=document.getElementById("nombreCliente").value.trim();

 if(nombre==="") return alert("Escribe tu nombre");

 const boletosArray=Array.from(seleccionados);

 const boton=document.getElementById("btnPagar");
 boton.disabled=true;
 boton.textContent="Procesando...";


 try{

  await fetch(URL_SCRIPT,{
   method:"POST",
   body:JSON.stringify({
    nombre:nombre,
    boletos:boletosArray
   })
  });


  /* MENSAJE WHATSAPP */
  const total=boletosArray.length*PRECIO_BOLETO;

  const mensaje=
`🎟️ RIFA LOS COMPAS
Hola! Reserve los siguientes boletos:

🎫 BOLETOS: ${boletosArray.join(", ")}

🎁 BOLETOS GRATIS: 0
💰 TOTAL: $${total}

👤 Nombre: ${nombre}

ENVIARÉ MI COMPROBANTE EN UN MOMENTO.`;

  const urlWhats=`https://wa.me/${527421199270}?text=${encodeURIComponent(mensaje)}`;

  window.open(urlWhats,"_blank");


  alert("Boletos apartados correctamente");

  seleccionados.clear();

  boton.disabled=false;
  boton.textContent="Finalizar Compra";

  cargarVendidos();

 }catch(err){

  alert("Error al guardar");

  boton.disabled=false;
  boton.textContent="Finalizar Compra";

 }

}

</script>
