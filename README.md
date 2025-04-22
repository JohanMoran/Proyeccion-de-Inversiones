<!DOCTYPE html>
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
    const metas = [];
    const colores = ['#ff9999', '#99ccff', '#ccffcc', '#ffff99'];

    function toggleModoOscuro() {
      document.body.classList.toggle('dark');
    }

    function agregarMeta() {
      const id = metas.length;
      const div = document.createElement('div');
      div.className = 'meta-entry';
      div.innerHTML = `
        <input type="text" placeholder="Nombre de la meta" id="metaNombre${id}" />
        <input type="number" placeholder="Monto" id="metaMonto${id}" />
      `;
      document.getElementById('metas').appendChild(div);
      metas.push(id);
    }

    let resultadosGlobales = [];

    function calcularInversion() {
      const capitalInicial = parseFloat(document.getElementById('capitalInicial').value) || 0;
      const tasaAnual = parseFloat(document.getElementById('tasa').value) / 100 || 0;
      const plazo = parseInt(document.getElementById('plazo').value) || 0;
      const aportacion = parseFloat(document.getElementById('aportacionMensual').value) || 0;
      const capitalObjetivo = parseFloat(document.getElementById('capitalObjetivo').value) || null;
      const retiroMensual = parseFloat(document.getElementById('retiroMensual').value) || 0;

      resultadosGlobales = [];
      let totalAportado = capitalInicial;
      let monto = capitalInicial;
      const metasInfo = metas.map((id, i) => ({
        nombre: document.getElementById(`metaNombre${id}`).value,
        monto: parseFloat(document.getElementById(`metaMonto${id}`).value) || 0,
        color: colores[i % colores.length],
        alcanzada: false
      }));

      for (let i = 1; i <= plazo; i++) {
        monto += aportacion;
        monto -= retiroMensual;
        monto += monto * (tasaAnual / 12);
        totalAportado += aportacion;
        resultadosGlobales.push({ mes: i, monto: monto.toFixed(2) });

        metasInfo.forEach(meta => {
          if (!meta.alcanzada && monto >= meta.monto) meta.alcanzada = i;
        });

        if (monto <= 0) break;
      }

      mostrarTabla(resultadosGlobales, metasInfo);
      mostrarGrafico(resultadosGlobales, metasInfo);
      mostrarResumen(totalAportado, monto - totalAportado, monto);
      mostrarRiesgo(tasaAnual);
    }

    function mostrarTabla(resultados) {
      const tabla = document.getElementById('tablaResultados');
      tabla.innerHTML = '<tr><th>Mes</th><th>Monto acumulado</th></tr>';
      resultados.forEach(r => {
        tabla.innerHTML += `<tr><td>${r.mes}</td><td>$${r.monto}</td></tr>`;
      });
    }

    function mostrarGrafico(resultados, metasInfo) {
      const ctx = document.getElementById('grafico').getContext('2d');
      if (window.miGrafico) window.miGrafico.destroy();
      const labels = resultados.map(r => `Mes ${r.mes}`);
      const data = resultados.map(r => r.monto);
      const metasData = metasInfo.filter(m => m.alcanzada).map(m => ({
        label: `${m.nombre} (${m.monto})`,
        borderColor: m.color,
        borderWidth: 2,
        data: Array(resultados.length).fill(m.monto),
        type: 'line',
        fill: false,
        pointRadius: 0
      }));

      window.miGrafico = new Chart(ctx, {
        type: 'line',
        data: {
          labels,
          datasets: [
            { label: 'Capital acumulado', data, fill: false, borderColor: '#007bff', tension: 0.1 },
            ...metasData
          ]
        }
      });
    }

    function mostrarResumen(aportado, rendimiento, final) {
      document.getElementById('resumen').innerHTML = `
        <h3>Resumen</h3>
        <p>Total aportado: <strong>$${aportado.toFixed(2)}</strong></p>
        <p>Rendimientos generados: <strong>$${rendimiento.toFixed(2)}</strong></p>
        <p>Monto final: <strong>$${final.toFixed(2)}</strong></p>
      `;
    }

    function mostrarRiesgo(tasa) {
      const cont = document.getElementById('resumen');
      let label = '';
      if (tasa < 0.07) label = '<span class="risk-label conservador">Perfil: Conservador</span>';
      else if (tasa < 0.12) label = '<span class="risk-label moderado">Perfil: Moderado</span>';
      else label = '<span class="risk-label agresivo">Perfil: Agresivo</span>';
      cont.innerHTML += `<p>${label}</p>`;
    }

    function sugerirAportacion() {
      const objetivo = parseFloat(document.getElementById('capitalObjetivo').value) || 0;
      const plazo = parseInt(document.getElementById('plazo').value) || 0;
      const tasaAnual = parseFloat(document.getElementById('tasa').value) / 100 || 0;
      const inicial = parseFloat(document.getElementById('capitalInicial').value) || 0;

      let aportacion = 0;
      for (let i = 0; i < 10000; i++) {
        let monto = inicial;
        for (let m = 1; m <= plazo; m++) {
          monto += aportacion;
          monto += monto * (tasaAnual / 12);
        }
        if (monto >= objetivo) break;
        aportacion += 10;
      }
      alert(`Aportación sugerida para alcanzar el objetivo: $${aportacion.toFixed(2)} / mes`);
    }

    async function descargarPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();
      const resumen = document.getElementById('resumen').innerText;
      doc.text(resumen, 10, 10);
      doc.save('resumen_inversion.pdf');
    }

    function exportarExcel() {
      const ws = XLSX.utils.json_to_sheet(resultadosGlobales);
      const wb = XLSX.utils.book_new();
      XLSX.utils.book_append_sheet(wb, ws, "Resultados");
      XLSX.writeFile(wb, "resultados_inversion.xlsx");
    }
  </script>
</body>
</html>
