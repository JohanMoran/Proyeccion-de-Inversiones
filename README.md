<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Calculadora de Inversión</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #f4f6f8;
      padding: 20px;
      max-width: 900px;
      margin: auto;
      color: #333;
    }

    h1 {
      text-align: center;
      color: #2b6777;
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
      margin-top: 20px;
      padding: 10px 15px;
      background-color: #2b6777;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-weight: bold;
    }

    button:hover {
      background-color: #1b4d5b;
    }

    .result {
      margin-top: 20px;
      font-size: 18px;
      font-weight: bold;
      color: #11698e;
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
      background-color: #ddeeee;
    }

    canvas {
      margin-top: 30px;
    }

    .buttons {
      display: flex;
      gap: 10px;
      margin-top: 15px;
      flex-wrap: wrap;
    }
  </style>
</head>
<body>
  <h1>Calculadora de Inversión</h1>

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

  <canvas id="grafica" height="80"></canvas>

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
    let datosGrafica = [];

    function calcular() {
      const capitalInicial = parseFloat(document.getElementById('capitalInicial').value) || 0;
      const tasa = parseFloat(document.getElementById('tasa').value) || 0;
      const plazo = parseInt(document.getElementById('plazo').value) || 0;
      const aportacion = parseFloat(document.getElementById('aportacion').value) || 0;
      const capitalObjetivo = parseFloat(document.getElementById('capitalObjetivo').value) || null;
      const fechaInicio = new Date(document.getElementById('fechaInicio').value);

      let capital = capitalInicial;
      let totalInteres = 0;
      let totalAportaciones = 0;
      const mensual = tasa / 12 / 100;
      const tabla = document.querySelector('#tablaResultados tbody');
      tabla.innerHTML = '';
      datosGrafica = [];

      let meses = plazo;
      let cumpleObjetivo = false;

      if (capitalObjetivo) {
        for (let i = 1; i <= 600; i++) {
          const interes = capital * mensual;
          totalInteres += interes;
          capital += interes + aportacion;
          totalAportaciones += aportacion;
          if (capital >= capitalObjetivo) {
            meses = i;
            cumpleObjetivo = true;
            break;
          }
        }
        capital = capitalInicial;
        totalInteres = 0;
        totalAportaciones = 0;
      }

      for (let i = 1; i <= meses; i++) {
        const interes = capital * mensual;
        totalInteres += interes;
        capital += interes + aportacion;
        totalAportaciones += aportacion;

        const fecha = new Date(fechaInicio);
        fecha.setMonth(fecha.getMonth() + i);

        tabla.innerHTML += `
          <tr>
            <td>${i}</td>
            <td>${fecha.toLocaleDateString()}</td>
            <td>$${aportacion.toFixed(2)}</td>
            <td>$${interes.toFixed(2)}</td>
            <td>$${capital.toFixed(2)}</td>
          </tr>`;

        datosGrafica.push({ mes: i, total: capital.toFixed(2) });
      }

      document.getElementById('resultado').innerText =
        cumpleObjetivo
          ? `📈 Alcanzarás el capital objetivo de $${capitalObjetivo.toLocaleString()} en ${meses} meses.`
          : `💰 Monto final estimado: $${capital.toFixed(2)} (Interés generado: $${totalInteres.toFixed(2)})`;

      document.getElementById('resumenFinal').innerHTML = `
        <hr style="margin-top:20px;"/>
        <p><strong>Resumen:</strong></p>
        <ul>
          <li>🔹 Total de aportaciones: <strong>$${totalAportaciones.toFixed(2)}</strong></li>
          <li>🔹 Intereses generados: <strong>$${totalInteres.toFixed(2)}</strong></li>
          <li>🔹 Monto final: <strong>$${capital.toFixed(2)}</strong></li>
        </ul>
      `;

      document.getElementById('tablaResultados').style.display = 'table';
      graficar();
    }

    function graficar() {
      const ctx = document.getElementById('grafica').getContext('2d');
      if (window.miGrafica) window.miGrafica.destroy();

      window.miGrafica = new Chart(ctx, {
        type: 'line',
        data: {
          labels: datosGrafica.map(p => `Mes ${p.mes}`),
          datasets: [{
            label: 'Total acumulado',
            data: datosGrafica.map(p => p.total),
            borderColor: '#2b6777',
            backgroundColor: 'rgba(43,103,119,0.1)',
            tension: 0.3,
            fill: true
          }]
        },
        options: {
          responsive: true,
          plugins: { legend: { display: true } }
        }
      });
    }

    function descargarCSV() {
      let csv = 'Mes,Fecha,Aportación,Interés,Total\n';
      const filas = document.querySelectorAll('#tablaResultados tbody tr');
      filas.forEach(fila => {
        const columnas = fila.querySelectorAll('td');
        const datos = Array.from(columnas).map(td => td.innerText.replace('$', '').replace(',', ''));
        csv += datos.join(',') + '\n';
      });

      const blob = new Blob([csv], { type: 'text/csv' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'resultado-inversion.csv';
      a.click();
      URL.revokeObjectURL(url);
    }

    async function descargarPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();

      doc.setFontSize(16);
      doc.text("Resumen de Inversión", 10, 20);

      const resumen = document.getElementById("resumenFinal").innerText;
      const lineas = resumen.split('\n').filter(Boolean);

      doc.setFontSize(12);
      lineas.forEach((linea, i) => {
        doc.text(linea.trim(), 10, 30 + i * 8);
      });

      doc.save("resumen-inversion.pdf");
    }
  </script>
</body>
</html>
