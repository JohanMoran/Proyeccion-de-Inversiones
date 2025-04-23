<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Calculadora de Inversión</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.28/jspdf.plugin.autotable.min.js"></script>
  <style>
    :root {
      --fondo-claro: #f4f6f8;
      --texto-claro: #333;
      --primario: #2b6777;
      --hover: #1b4d5b;
      --tabla-head: #ddeeee;
      --boton-texto: #fff;
    }

    body.dark {
      --fondo-claro: #121212;
      --texto-claro: #e0e0e0;
      --tabla-head: #1f1f1f;
      --boton-texto: #fff;
    }

    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: var(--fondo-claro);
      color: var(--texto-claro);
      padding: 20px;
      max-width: 900px;
      margin: auto;
      transition: background-color 0.4s, color 0.4s;
    }

    header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 30px;
    }

    .logo-container {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .logo-container img {
      height: 60px;
    }

    .slogan {
      font-weight: bold;
      color: var(--primario);
      font-size: 1.2em;
    }

    label {
      margin-top: 15px;
      display: block;
      font-weight: 600;
    }

    input {
      padding: 10px;
      border: 1px solid #ccc;
      width: 100%;
      border-radius: 5px;
      margin-top: 5px;
      background-color: #fff;
      transition: background-color 0.3s, color 0.3s;
    }

    body.dark input {
      background-color: #2a2a2a;
      color: #e0e0e0;
      border: 1px solid #555;
    }

    button {
      padding: 10px 16px;
      background-color: var(--primario);
      color: var(--boton-texto);
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-size: 14px;
      font-weight: 600;
      transition: background-color 0.3s, transform 0.2s;
    }

    button:hover {
      background-color: var(--hover);
      transform: scale(1.03);
    }

    .result {
      margin-top: 20px;
      font-size: 16px;
      font-weight: bold;
      color: var(--primario);
    }

    table {
      width: 100%;
      margin-top: 20px;
      border-collapse: collapse;
    }

    th, td {
      padding: 8px;
      text-align: center;
      border: 1px solid #ccc;
    }

    th {
      background-color: var(--tabla-head);
    }

    canvas {
      margin-top: 30px;
    }

    .buttons {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-top: 15px;
    }

    .dark-mode-btn {
      position: fixed;
      bottom: 20px;
      right: 20px;
      z-index: 999;
      box-shadow: 0 4px 10px rgba(0,0,0,0.2);
    }
  </style>
</head>
<body>
  <header>
    <div class="logo-container">
      <img src="https://www.bailmex.com.mx/images/logo.png" alt="Bailmex" />
      <div class="slogan">GARANTÍAS REALES</div>
    </div>
  </header>

  <button class="dark-mode-btn" onclick="toggleDarkMode()">🌙 Modo Oscuro</button>

  <label>Monto Inicial:</label>
  <input type="number" id="capitalInicial" />

  <label>Tasa Anual (%):</label>
  <input type="number" id="tasa" />

  <label>Plazo (en meses):</label>
  <input type="number" id="plazo" />

  <label>Aportación mensual:</label>
  <input type="number" id="aportacion" />

  <label>Fecha de inicio:</label>
  <input type="date" id="fechaInicio" />

  <label>Capital objetivo (opcional):</label>
  <input type="number" id="capitalObjetivo" placeholder="Ej: 500000" />

  <div class="buttons">
    <button onclick="calcular()">Calcular</button>
    <button onclick="descargarCSV()">Descargar Excel</button>
    <button onclick="descargarPDF()">Descargar PDF</button>
  </div>

  <div class="result" id="resultado"></div>
  <div class="result" id="resumenFinal"></div>

  <canvas id="grafica" height="120"></canvas>

  <table id="tablaResultados" style="display:none">
    <thead>
      <tr>
        <th>Mes</th>
        <th>Fecha</th>
        <th>Aportación</th>
        <th>Interés</th>
        <th>Total</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <script>
    if (localStorage.getItem("modoOscuro") === "true") {
      document.body.classList.add("dark");
    }

    function toggleDarkMode() {
      document.body.classList.toggle("dark");
      localStorage.setItem("modoOscuro", document.body.classList.contains("dark"));
    }
  </script>

  <!-- Scripts originales de cálculo, gráfica, descarga CSV/PDF se mantienen intactos -->
  <script>
    // ... Aquí va todo tu script original (desde calcular() hasta descargarPDF()) sin cambios ...
  </script>
</body>
</html>
