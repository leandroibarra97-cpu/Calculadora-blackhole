<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Calculadora Pro Escritorios</title>

<style>
body {
    margin: 0;
    font-family: 'Segoe UI', sans-serif;
    background: #0f172a;
    color: white;
}

.container {
    display: grid;
    grid-template-columns: 1fr 1fr;
    height: 100vh;
}

.panel {
    padding: 30px;
}

.left {
    background: #020617;
}

.right {
    background: #0f172a;
}

h1 {
    margin-bottom: 20px;
}

input, select {
    width: 100%;
    padding: 10px;
    margin: 10px 0;
    border-radius: 8px;
    border: none;
}

.card {
    background: #020617;
    padding: 20px;
    border-radius: 10px;
    margin-top: 20px;
}

.resultado h2 {
    font-size: 28px;
    margin: 10px 0;
}

.slider {
    width: 100%;
}

button {
    background: #22c55e;
    border: none;
    padding: 12px;
    width: 100%;
    border-radius: 8px;
    color: black;
    font-weight: bold;
    cursor: pointer;
}

button:hover {
    background: #16a34a;
}

.small {
    opacity: 0.7;
    font-size: 14px;
}
</style>

</head>

<body>

<div class="container">

<!-- PANEL IZQUIERDO -->
<div class="panel left">
    <h1>Configuración</h1>

    <label>Largo (cm)</label>
    <input type="number" id="largo" value="120">

    <label>Ancho (cm)</label>
    <input type="number" id="ancho" value="60">

    <label>Alto (cm)</label>
    <input type="number" id="alto" value="75">

    <label>Tipo de Madera</label>
    <select id="madera">
        <option value="24254">Paraíso</option>
        <option value="26000">Merlot</option>
        <option value="28000">Roble</option>
    </select>

    <label>Estructura</label>
    <select id="estructura">
        <option value="8000">Hierro 40x20</option>
        <option value="10000">Hierro reforzado</option>
    </select>

    <label>
        <input type="checkbox" id="cpu"> Soporte CPU (+$15.000)
    </label>

    <label>
        <input type="checkbox" id="cajon"> Cajones (+$20.000)
    </label>

    <label>Margen de ganancia</label>
    <input type="range" min="0" max="200" value="100" id="margen" class="slider">
    <p id="margenValor">100%</p>

</div>

<!-- PANEL DERECHO -->
<div class="panel right">

    <h1>Resultado</h1>

    <div class="card resultado">
        <p class="small">Costo total</p>
        <h2>$ <span id="costo">0</span></h2>

        <p class="small">Precio de venta</p>
        <h2>$ <span id="precio">0</span></h2>

        <p class="small">Ganancia</p>
        <h2>$ <span id="ganancia">0</span></h2>
    </div>

    <div class="card">
        <h3>Detalle</h3>
        <p>Madera: $ <span id="detalleMadera"></span></p>
        <p>Estructura: $ <span id="detalleEstructura"></span></p>
        <p>Extras: $ <span id="detalleExtras"></span></p>
    </div>

    <button onclick="copiar()">Copiar presupuesto</button>

</div>

</div>

<script>
function calcular() {
    let largo = document.getElementById("largo").value / 100;
    let ancho = document.getElementById("ancho").value / 100;
    let alto = document.getElementById("alto").value / 100;

    let precioMadera = parseFloat(document.getElementById("madera").value);
    let estructuraMetro = parseFloat(document.getElementById("estructura").value);
    let margen = document.getElementById("margen").value;

    document.getElementById("margenValor").innerText = margen + "%";

    let superficie = largo * ancho;
    let costoMadera = superficie * precioMadera;

    let estructura = (largo*2 + ancho*2 + alto*2) * estructuraMetro;

    let extras = 0;
    if(document.getElementById("cpu").checked) extras += 15000;
    if(document.getElementById("cajon").checked) extras += 20000;

    let costoFijo = 8000;

    let costoTotal = costoMadera + estructura + extras + costoFijo;
    let precioVenta = costoTotal * (1 + margen / 100);
    let ganancia = precioVenta - costoTotal;

    document.getElementById("costo").innerText = Math.round(costoTotal);
    document.getElementById("precio").innerText = Math.round(precioVenta);
    document.getElementById("ganancia").innerText = Math.round(ganancia);

    document.getElementById("detalleMadera").innerText = Math.round(costoMadera);
    document.getElementById("detalleEstructura").innerText = Math.round(estructura);
    document.getElementById("detalleExtras").innerText = extras;
}

function copiar() {
    let texto = `📐 Presupuesto Escritorio

Medidas:
- Largo: ${largo.value}cm
- Ancho: ${ancho.value}cm
- Alto: ${alto.value}cm

💰 Precio final: $${precio.innerText}

Gracias por elegirnos 🙌`;

    navigator.clipboard.writeText(texto);
    alert("Presupuesto copiado");
}

document.querySelectorAll("input, select").forEach(el => {
    el.addEventListener("input", calcular);
});

calcular();
</script>

</body>
</html>
