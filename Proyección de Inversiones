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
      cursor: pointer;
    }
    button:focus {
      outline: none;
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
    let modoOscuro = false;

    function toggleModoOscuro() {
      modoOscuro = !modoOscuro;
      document.body.classList.toggle('dark', modoOscuro);
    }

    // Función para calcular la inversión
    function calcularInversion() {
      let capitalInicial = parseFloat(document.getElementById('capitalInicial').value);
      let tasa = parseFloat(document.getElementById('tasa').value) / 100;
      let plazo = parseInt(document.getElementById('plazo').value);
      let aportacionMensual = parseFloat(document.getElementById('aportacionMensual').value);
      let retiroMensual = parseFloat(document.getElementById('retiroMensual').value) || 0;
      
      // Calcular los rendimientos mes a mes
      let resultado = [];
      let saldo = capitalInicial;
      let totalAportaciones = capitalInicial;
      let totalRendimiento = 0;

      for (let i = 1; i <= plazo; i++) {
        saldo += aportacionMensual;
        saldo *= (1 + tasa / 12);
        saldo -= retiroMensual;
        totalAportaciones += aportacionMensual;
        totalRendimiento = saldo - totalAportaciones;

        resultado.push({
          mes: i,
          saldo: saldo.toFixed(2),
          rendimiento: totalRendimiento.toFixed(2),
          aportaciones: totalAportaciones.toFixed(2)
        });
      }

      mostrarResultados(resultado);
    }

    // Función para mostrar los resultados en la tabla
    function mostrarResultados(resultado) {
      let tabla = document.getElementById('tablaResultados');
      tabla.innerHTML = `
        <tr>
          <th>Mes</th>
          <th>Saldo Final</th>
          <th>Rendimiento</th>
          <th>Aportaciones Totales</th>
        </tr>
      `;

      resultado.forEach(item => {
        tabla.innerHTML += `
          <tr>
            <td>${item.mes}</td>
            <td>${item.saldo}</td>
            <td>${item.rendimiento}</td>
            <td>${item.aportaciones}</td>
          </tr>
        `;
      });
    }

    // Función para sugerir una aportación
    function sugerirAportacion() {
      let capitalObjetivo = parseFloat(document.getElementById('capitalObjetivo').value);
      let tasa = parseFloat(document.getElementById('tasa').value) / 100;
      let plazo = parseInt(document.getElementById('plazo').value);

      if (!capitalObjetivo) {
        alert("Por favor, ingresa un capital objetivo");
        return;
      }

      let aportacion = (capitalObjetivo - (capitalObjetivo * (1 + tasa / 12) ** plazo)) / plazo;
      alert("Aportación mensual sugerida: $" + aportacion.toFixed(2));
    }

    // Función para generar el PDF
    function descargarPDF() {
      let doc = new jsPDF();
      let tabla = document.getElementById('tablaResultados').outerHTML;
      doc.html(tabla, {
        callback: function (doc) {
          doc.save('inversion.pdf');
        }
      });
    }

    // Función para exportar a Excel
    function exportarExcel() {
      let tabla = document.getElementById('tablaResultados');
      let wb = XLSX.utils.table_to_book(tabla, { sheet: "Resultados" });
      XLSX.writeFile(wb, "inversion.xlsx");
    }

    // Función para agregar una meta intermedia
    function agregarMeta() {
      const metasContainer = document.getElementById('metas');
      const metaInput = document.createElement('input');
      metaInput.type = 'number';
      metaInput.placeholder = 'Meta intermedia';
      metasContainer.appendChild(metaInput);
    }
  </script>
</body>
</html>
