<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Calculadora de Inversión</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
  <style>
    :root {
      --bg-color: #f9f9f9;
      --text-color: #333;
      --box-bg: #fff;
    }

    body.dark {
      --bg-color: #121212;
      --text-color: #eee;
      --box-bg: #1e1e1e;
    }

    body {
      font-family: 'Segoe UI', sans-serif;
      margin: 20px;
      background: var(--bg-color);
      color: var(--text-color);
    }
    h1 {
      color: #0066cc;
    }
    label {
      display: block;
      margin-top: 10px;
    }
    input, select, button {
      padding: 8px;
      margin-top: 5px;
      border: 1px solid #ccc;
      border-radius: 6px;
      width: 100%;
    }
    .container {
      max-width: 800px;
      margin: auto;
      background: var(--box-bg);
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    }
    table {
      width: 100%;
      margin-top: 20px;
      border-collapse: collapse;
    }
    th, td {
      padding: 10px;
      border-bottom: 1px solid #ddd;
      text-align: right;
    }
    th {
      background: #e6f2ff;
    }
    .meta-entry {
      display: flex;
      gap: 10px;
      margin-top: 5px;
    }
    .risk-label {
      padding: 6px 10px;
      border-radius: 8px;
      display: inline-block;
      font-weight: bold;
    }
    .conservador { background-color: #d4edda; color: #155724; }
    .moderado { background-color: #fff3cd; color: #856404; }
    .agresivo { background-color: #f8d7da; color: #721c24; }
    .top-bar {
      text-align: right;
      max-width: 800px;
      margin: auto;
      margin-bottom: 10px;
    }
    button {
      font-size: 0.85rem;
      padding: 6px 10px;
      width: auto;
    }
  </style>
</head>
<body>
  <div class="top-bar">
    <button onclick="toggleModoOscuro()">🌓 Modo oscuro</button>
  </div>
  <div class="container">
    <h1>Calculadora de Inversión Avanzada</h1>

    <label>Capital inicial:</label>
    <input type="number" id="capitalInicial" />

    <label>Tasa anual (%):</label>
    <input type="number" id="tasa" />

    <label>Plazo (en meses):</label>
    <input type="number" id="plazo" />

    <label>Aportaciones mensuales:</label>
    <input type="number" id="aportacionMensual" />

    <label>Capital objetivo (opcional):</label>
    <input type="number" id="capitalObjetivo" />

    <label>Agregar metas intermedias:</label>
    <div id="metas"></div>
    <button onclick="agregarMeta()">➕ Agregar nueva meta</button>

    <label>¿Quieres simular un retiro mensual?</label>
    <input type="number" id="retiroMensual" placeholder="Monto mensual a retirar (opcional)" />

    <br><br>
    <button onclick="calcularInversion()">Calcular inversión</button>
    <button onclick="sugerirAportacion()">💡 Calcular aportación sugerida</button>
    <button onclick="descargarPDF()">📄 Descargar PDF</button>
    <button onclick="exportarExcel()">📊 Exportar Excel</button>

    <div id="resumen"></div>
    <canvas id="grafico" style="max-width:100%; margin-top:30px;"></canvas>
    <table id="tablaResultados"></table>
  </div>

  <script>
    // Código JavaScript permanece igual
  </script>
</body>
</html>
