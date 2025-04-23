<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
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
      --fondo-oscuro: #1e1e1e;
      --texto-oscuro: #f0f0f0;
      --tabla-head-oscuro: #333;
    }

    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: var(--fondo-claro);
      color: var(--texto-claro);
      padding: 20px;
      max-width: 900px;
      margin: auto;
      transition: background-color 0.3s, color 0.3s;
    }

    body.dark {
      background-color: var(--fondo-oscuro);
      color: var(--texto-oscuro);
    }

    .logo {
      display: block;
      margin: 0 auto 10px;
      max-width: 160px;
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
    }

    button {
      padding: 8px 12px;
      background-color: var(--primario);
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 14px;
      font-weight: 600;
    }

    button:hover {
      background-color: var(--hover);
    }

    .result, #resumenFinal {
      margin-top: 20px;
      font-size: 16px;
      font-weight: bold;
      background-color: rgba(43, 103, 119, 0.05);
      padding: 15px;
      border-radius: 8px;
    }

    body.dark .result,
    body.dark #resumenFinal {
      background-color: rgba(255, 255, 255, 0.05);
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

    body.dark th {
      background-color: var(--tabla-head-oscuro);
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

    .dark-toggle {
      position: fixed;
      bottom: 20px;
      right: 20px;
      z-index: 999;
    }
  </style>
</head>
<body>
  <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAA..." alt="Logo Bailmex" class="logo" />
  <!-- Reemplazado por la versión en base64 real completa (omitido aquí por espacio) -->

  <!-- Formulario -->
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
  <div id="resumenFinal"></div>

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

  <div class="dark-toggle">
    <button onclick="toggleDarkMode()">🌙 Modo Oscuro</button>
  </div>

  <!-- Tu script JavaScript sigue igual que antes... -->
</body>
</html>
