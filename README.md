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
      --portada: #2e3552;
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

    #portada {
      position: relative;
      left: 50%;
      right: 50%;
      margin-left: -50vw;
      margin-right: -50vw;
      width: 100vw;
      height: 180px;
      background-color: var(--portada);
      overflow: hidden;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    #imagen-portada {
      height: 100%;
      object-fit: cover;
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

    .input-container {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-top: 5px;
    }

    .input-container input {
      width: 15%;
    }

    .input-container span {
      font-weight: normal;
      font-size: 13px;
      color: var(--texto-claro);
      display: inline-block;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      width: 80%; /* Esto asegura que el texto no se ajuste */
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
      background-color: #fff;
    }

    th, td {
      padding: 8px;
      text-align: center;
      border: 1px solid #ccc;
    }

    th {
      background-color: var(--tabla-head);
    }

    body.dark table {
      background-color: #1f1f1f;
      color: #e0e0e0;
    }

    body.dark th {
      background-color: #2c2c2c;
    }

    canvas {
      margin-top: 30px;
      background-color: #fff;
      border-radius: 8px;
      padding: 10px;
    }

    body.dark canvas {
      background-color: #1f1f1f;
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
  <div id="portada">
    <img id="imagen-portada" src="ruta/de/tu/imagen.jpg" alt="Imagen de portada">
  </div>

  <button class="dark-mode-btn" onclick="toggleDarkMode()">🌙 Modo Oscuro</button>

  <label>MONTO INICIAL:</label>
  <div class="input-container">
    <input type="number" id="capitalInicial" />
    <span>¿Con qué cantidad cuentas en este momento? ¿Con cuánto empezarás tu inversión?</span>
  </div>

  <label>Tasa Anual (%):</label>
  <div class="input-container">
    <input type="number" id="tasa" />
    <span>Tasa de Interés anual, inversionistas conservador (renta fija) 10% - 15%.</span>
  </div>

  <label>Plazo (en meses):</label>
  <div class="input-container">
    <input type="number" id="plazo" />
    <span>¿Cuántos años vas a realizar la inversión? ¿Cuál es tu horizonte de inversión?</span>
  </div>

  <label>Aportación mensual:</label>
  <div class="input-container">
    <input type="number" id="aportacion" />
    <span>¿Cuánto puedes destinar a tu inversión al mes para incrementar tus rendimientos?</span>
  </div>

  <label>Fecha de inicio:</label>
  <div class="input-container">
    <input type="date" id="fechaInicio" />
    <span>Fecha en que tienes pensado dar inicio a tu inversión</span>
  </div>

  <label>Capital objetivo (opcional):</label>
  <div class="input-container">
    <input type="number" id="capitalObjetivo" placeholder="Ej: 500000" />
    <span>¿Ya tienes un objetivo (ir de viaje, comprar un auto, etc.)? Elige un monto con el que alcanzarás ese objetivo</span>
  </div>

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
    }

    function calcular() {
      const capitalInicial = parseFloat(document.getElementById('capitalInicial').value) || 0;
      const tasa = parseFloat(document.getElementById('tasa').value) || 0;
      const plazo = parseInt(document.getElementById('plazo').value) || 0;
      const aportacion = parseFloat(document.getElementById('aportacion').value) || 0;
      const inflacion = parseFloat(document.getElementById('inflacion').value) || 0;
      const capitalObjetivo = parseFloat(document.getElementById('capitalObjetivo').value) || null;
      const fechaInicio = new Date(document.getElementById('fechaInicio').value);

      capital = capitalInicial;
      totalInteres = 0;
      totalAportaciones = 0;
      let tasaMensual = tasa / 12 / 100;
      let inflacionMensual = inflacion / 12 / 100;

      const tabla = document.querySelector('#tablaResultados tbody');
      tabla.innerHTML = '';
      datosGrafica = [];

      let meses = plazo;
      let cumpleObjetivo = false;

      if (capitalObjetivo) {
        for (let i = 1; i <= 600; i++) {
          const interes = capital * tasaMensual;
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
        const interes = capital * tasaMensual;
        totalInteres += interes;
        capital += interes + aportacion;
        totalAportaciones += aportacion;

        let capitalAjustado = capital / Math.pow(1 + inflacionMensual, i);

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
      const tabla = document.querySelector('#tablaResultados tbody');
      tabla.querySelectorAll('tr').forEach(row => {
        row.querySelectorAll('td').forEach((cell, index) => {
          csv += cell.innerText + (index === 4 ? '\n' : ',');
        });
      });

      const blob = new Blob([csv], { type: 'text/csv' });
      const url = URL.createObjectURL(blob);
      const link = document.createElement('a');
      link.href = url;
      link.download = 'resultados_inversion.csv';
      link.click();
      URL.revokeObjectURL(url);
    }

    function descargarPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();
      doc.text('Resumen de Inversión', 14, 20);
      doc.autoTable({ html: '#tablaResultados', startY: 30 });
      doc.save('resultado-inversion.pdf');
    }
  </script>
</body>
</html>
