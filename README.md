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
    }

    body.dark {
      --fondo-claro: #1e1e1e;
      --texto-claro: #fff;
      --tabla-head: #333;
    }

    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: var(--fondo-claro);
      color: var(--texto-claro);
      padding: 20px;
      max-width: 900px;
      margin: auto;
      transition: background-color 0.3s ease, color 0.3s ease; /* Transición suave para los colores */
    }

    h1 {
      text-align: center;
      color: var(--primario);
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
      transition: background-color 0.3s ease, transform 0.3s ease; /* Transición en el botón */
    }

    button:hover {
      background-color: var(--hover);
      transform: scale(1.05); /* Animación de "escala" para el botón */
    }

    body.dark button {
      background-color: #333; /* Cambiar color de fondo del botón en modo oscuro */
    }

    body.dark button:hover {
      background-color: #555; /* Cambiar color al pasar el mouse en modo oscuro */
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

    .topbar {
      position: fixed;
      bottom: 20px;
      right: 20px;
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 1000;
    }
  </style>
</head>
<body>
  <div class="topbar">
    <button onclick="toggleDarkMode()">🌚 Modo Oscuro</button>
  </div>

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
    let datosGrafica = [];
    let totalAportaciones = 0, totalInteres = 0, capital = 0;

    function toggleDarkMode() {
      document.body.classList.toggle("dark");
      const modoTexto = document.body.classList.contains("dark") ? "🌙 Modo Claro" : "🌚 Modo Oscuro";
      document.querySelector('button').textContent = modoTexto;
    }

    function calcular() {
      const capitalInicial = parseFloat(document.getElementById('capitalInicial').value) || 0;
      const tasa = parseFloat(document.getElementById('tasa').value) || 0;
      const plazo = parseInt(document.getElementById('plazo').value) || 0;
      const aportacion = parseFloat(document.getElementById('aportacion').value) || 0;
      const capitalObjetivo = parseFloat(document.getElementById('capitalObjetivo').value) || null;
      const fechaInicio = new Date(document.getElementById('fechaInicio').value);

      capital = capitalInicial;
      totalInteres = 0;
      totalAportaciones = 0;
      const mensual = tasa / 12 / 100;
      const tabla = document.querySelector('#tablaResultados tbody');
      tabla.innerHTML = '';
      datosGrafica = [];

      let meses = plazo;
      let cumpleObjetivo = false;

      if (capitalObjetivo) {
        for (let i = 1; i <= 600; i++) {
          const interes = capital * mensual;
          capital += interes + aportacion;
          if (capital >= capitalObjetivo) {
            meses = i;
            cumpleObjetivo = true;
            break;
          }
        }
        capital = capitalInicial;
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
          <li>Total de aportaciones: <strong>$${totalAportaciones.toFixed(2)}</strong></li>
          <li>Intereses generados: <strong>$${totalInteres.toFixed(2)}</strong></li>
          <li>Monto final: <strong>$${capital.toFixed(2)}</strong></li>
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
      a.download = 'resultados.csv';
      a.click();
    }

    function descargarPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();
      doc.text('Resumen de inversión', 20, 10);
      doc.autoTable({ html: '#tablaResultados' });
      doc.save('inversion.pdf');
    }
  </script>
</body>
</html>
