<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Calculadora de inversión</title>
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
      --verde: #28a745;
      --verde-hover: #218838;
      --texto-grande: 16px;
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
      background-color: var(--portada);
      color: #fff;
      text-align: center;
      padding: 20px;
      border-radius: 8px;
      margin-bottom: 20px;
    }
    label {
      margin-top: 15px;
      display: block;
      font-weight: 600;
      font-size: var(--texto-grande);
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
      width: 20%;
    }
    .input-container span {
      font-weight: normal;
      font-size: 14px;
      color: var(--texto-claro);
      display: inline-block;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      width: 80%;
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
    .boton-calcular {
      background-color: var(--verde);
      width: 160px;
    }
    .boton-calcular:hover {
      background-color: var(--verde-hover);
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
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }
    th, td {
      padding: 10px;
      text-align: center;
      border-bottom: 1px solid #eee;
    }
    th {
      background-color: var(--primario);
      color: white;
    }
    body.dark table {
      background-color: #1f1f1f;
      color: #e0e0e0;
    }
    body.dark th {
      background-color: #2c2c2c;
    }
    body.dark td {
      border-color: #444;
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
    .leyenda {
      font-size: 14px;
      font-style: italic;
      margin-top: 20px;
      margin-bottom: -5px;
      color: #555;
    }
    body.dark .leyenda {
      color: #aaa;
    }
  </style>
</head>
<body>
  <div id="portada">
    <h1>Calculadora de Inversión</h1>
    <p>Optimiza tu inversión y alcanza tus objetivos financieros con nuestra herramienta.</p>
  </div>
  <button class="dark-mode-btn" onclick="toggleDarkMode()">🌙 Modo Oscuro</button>

  <label>MONTO INICIAL:</label>
  <div class="input-container">
    <input type="number" id="capitalInicial" />
    <span>¿Con qué cantidad cuentas en este momento? ¿Con cuánto empezarás tu inversión?</span>
  </div>

  <label>Tasa Anual (%):</label>
  <div class="input-container">
    <input type="number" id="tasa" step="0.01" />
    <span>Tasa de Interés anual, inversionistas conservadores (renta fija) 10% - 15%.</span>
  </div>

  <label>Plazo (en meses):</label>
  <div class="input-container">
    <input type="number" id="plazo" min="1" />
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

  <div class="leyenda">
    Calculadora de Interés Compuesto con Aportación mensual. Herramienta para uso estrictamente con fines informativos y educativos.
  </div>

  <div class="buttons">
    <button class="boton-calcular" onclick="calcular()">Calcular</button>
    <button onclick="descargarCSV()">Descargar Excel</button>
    <button onclick="descargarPDF()">Descargar PDF</button>
  </div>

  <div class="result" id="resultado"></div>
  <div class="result" id="resumenFinal"></div>
  <canvas id="grafica" height="120"></canvas>

  <table id="tablaResultados">
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
    let chart = null;

    function toggleDarkMode() {
      document.body.classList.toggle("dark");
      if (chart) {
        chart.update();
      }
    }

    function calcular() {
      const capitalInicial = parseFloat(document.getElementById('capitalInicial').value) || 0;
      const tasa = parseFloat(document.getElementById('tasa').value) || 0;
      const plazo = parseInt(document.getElementById('plazo').value) || 0;
      const aportacion = parseFloat(document.getElementById('aportacion').value) || 0;
      const capitalObjetivo = parseFloat(document.getElementById('capitalObjetivo').value) || null;
      const fechaInicio = new Date(document.getElementById('fechaInicio').value);

      if (plazo <= 0 || tasa <= 0) {
        alert("Por favor, ingresa un plazo y una tasa válidos.");
        return;
      }

      capital = capitalInicial;
      totalInteres = 0;
      totalAportaciones = 0;
      const tasaMensual = tasa / 12 / 100;
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

        const fecha = new Date(fechaInicio);
        fecha.setMonth(fecha.getMonth() + i);

        tabla.innerHTML += `
          <tr>
            <td>${i}</td>
            <td>${fecha.toLocaleDateString('es-MX')}</td>
            <td>${aportacion.toLocaleString('es-MX')}</td>
            <td>${interes.toLocaleString('es-MX')}</td>
            <td>${capital.toLocaleString('es-MX')}</td>
          </tr>
        `;

        datosGrafica.push({ mes: i, total: capital });
      }

      document.getElementById('resultado').innerHTML = `
        <strong>Resumen de Inversión:</strong><br>
        Capital inicial: ${capitalInicial.toLocaleString('es-MX')}<br>
        Tasa de interés anual: ${tasa}%<br>
        Plazo: ${meses} meses<br>
        Aportación mensual: ${aportacion.toLocaleString('es-MX')}<br>
        Total aportado: ${totalAportaciones.toLocaleString('es-MX')}<br>
        Total interés generado: ${totalInteres.toLocaleString('es-MX')}<br>
        <strong>Total al final del plazo: ${capital.toLocaleString('es-MX')}</strong>
      `;

      if (cumpleObjetivo) {
        document.getElementById('resumenFinal').innerHTML = `
          🎉 <strong>¡Objetivo alcanzado en ${meses} meses!</strong>
        `;
      }

      generarGrafico();
    }

    function generarGrafico() {
      const ctx = document.getElementById('grafica').getContext('2d');
      
      if (chart) {
        chart.destroy();
      }

      chart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: datosGrafica.map(d => `Mes ${d.mes}`),
          datasets: [{
            label: 'Total acumulado (MXN)',
            data: datosGrafica.map(d => d.total),
            borderColor: '#2b6777',
            backgroundColor: 'rgba(43, 103, 119, 0.1)',
            fill: true,
            tension: 0.3
          }]
        },
        options: {
          responsive: true,
          plugins: {
            legend: {
              position: 'top',
            },
            tooltip: {
              callbacks: {
                label: (context) => {
                  return ` ${context.raw.toLocaleString('es-MX')} MXN`;
                }
              }
            }
          },
          scales: {
            y: {
              beginAtZero: false,
              ticks: {
                callback: (value) => `${value.toLocaleString('es-MX')}`
              }
            }
          }
        }
      });
    }

    function descargarPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();
      
      doc.setFontSize(18);
      doc.setTextColor(43, 103, 119);
      doc.text("Resumen de Inversión", 10, 15);
      
      doc.setFontSize(12);
      doc.setTextColor(0, 0, 0);
      doc.text(`Capital inicial: ${document.getElementById('capitalInicial').value}`, 10, 25);
      doc.text(`Tasa anual: ${document.getElementById('tasa').value}%`, 10, 35);
      doc.text(`Plazo: ${document.getElementById('plazo').value} meses`, 10, 45);
      
      doc.autoTable({
        html: '#tablaResultados',
        startY: 55,
        theme: 'grid',
        headStyles: {
          fillColor: [43, 103, 119],
          textColor: 255
        }
      });
      
      const canvas = document.getElementById('grafica');
      const imgData = canvas.toDataURL('image/png');
      doc.addImage(imgData, 'PNG', 10, doc.lastAutoTable.finalY + 10, 180, 100);
      
      doc.save('inversion.pdf');
    }

    function descargarCSV() {
      if (datosGrafica.length === 0) {
        alert("Primero calcula una inversión.");
        return;
      }
      
      let csv = "Mes,Fecha,Aportación,Interés,Total\n";
      const rows = document.querySelectorAll('#tablaResultados tbody tr');
      
      rows.forEach(row => {
        const cells = row.querySelectorAll('td');
        csv += `"${cells[0].textContent}","${cells[1].textContent}","${cells[2].textContent}","${cells[3].textContent}","${cells[4].textContent}"\n`;
      });
      
      const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
      const link = document.createElement('a');
      link.href = URL.createObjectURL(blob);
      link.download = 'inversion.csv';
      link.click();
    }
  </script>
</body>
</html>
