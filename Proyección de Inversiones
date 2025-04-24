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
      --portada: #1a2035;
    }
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: var(--fondo-claro);
      color: var(--texto-claro);
      padding: 15px;
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
      flex-direction: column;
      gap: 5px;
      margin-top: 5px;
    }
    @media (min-width: 768px) {
      .input-container {
        flex-direction: row;
        align-items: center;
      }
      .input-container input {
        width: 30%;
      }
      .input-container span {
        width: 70%;
      }
    }
    .input-container span {
      font-weight: normal;
      font-size: 14px;
      color: var(--texto-claro);
      line-height: 1.4;
    }
    button {
      padding: 12px 18px;
      background-color: var(--primario);
      color: var(--boton-texto);
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-size: 14px;
      font-weight: 600;
      transition: all 0.3s;
    }
    .boton-calcular {
      background-color: var(--verde);
      width: 160px;
    }
    .boton-calcular:hover {
      background-color: var(--verde-hover);
      transform: translateY(-2px);
    }
    .result {
      margin-top: 20px;
      font-size: 16px;
      font-weight: bold;
      color: var(--primario);
      line-height: 1.6;
    }
    .tabla-container {
      overflow-x: auto;
      margin-top: 20px;
      border-radius: 8px;
      box-shadow: 0 2px 15px rgba(0,0,0,0.1);
      background-color: var(--fondo-claro);
      transition: all 0.4s;
    }
    body.dark .tabla-container {
      box-shadow: 0 2px 15px rgba(0,0,0,0.3);
    }
    #tablaResultados {
      width: 100%;
      border-collapse: collapse;
      background-color: var(--fondo-claro);
      transition: all 0.4s;
    }
    #tablaResultados th, 
    #tablaResultados td {
      padding: 12px 10px;
      text-align: center;
      border-bottom: 1px solid #ddd;
    }
    #tablaResultados th {
      background-color: var(--primario);
      color: white;
      position: sticky;
      top: 0;
      font-weight: 600;
    }
    body.dark #tablaResultados th {
      background-color: #2c2c2c;
    }
    body.dark #tablaResultados td {
      border-color: #444;
      background-color: #1f1f1f;
    }
    canvas {
      margin-top: 30px;
      background-color: var(--fondo-claro);
      border-radius: 8px;
      padding: 15px;
      width: 100% !important;
      box-shadow: 0 2px 15px rgba(0,0,0,0.1);
      transition: all 0.4s;
    }
    body.dark canvas {
      box-shadow: 0 2px 15px rgba(0,0,0,0.3);
    }
    .buttons {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-top: 20px;
    }
    .dark-mode-btn {
      position: fixed;
      bottom: 20px;
      right: 20px;
      z-index: 999;
      box-shadow: 0 4px 12px rgba(0,0,0,0.2);
      width: 50px;
      height: 50px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 20px;
    }
    .leyenda {
      font-size: 14px;
      font-style: italic;
      margin-top: 20px;
      margin-bottom: -5px;
      color: #666;
      text-align: center;
    }
    body.dark .leyenda {
      color: #aaa;
    }
    .tabla-titulo {
      background-color: var(--primario);
      color: white;
      padding: 15px;
      border-radius: 8px 8px 0 0;
      font-weight: bold;
      text-align: center;
      display: none;
      margin-top: 30px;
    }
    body.dark .tabla-titulo {
      background-color: #2c2c2c;
    }
    .grafica-titulo {
      background-color: var(--primario);
      color: white;
      padding: 15px;
      border-radius: 8px 8px 0 0;
      font-weight: bold;
      text-align: center;
      margin-top: 30px;
      display: none;
    }
    body.dark .grafica-titulo {
      background-color: #2c2c2c;
    }
  </style>
</head>
<body>
  <div id="portada">
    <h1>Calculadora de Inversión</h1>
    <p>Optimiza tu inversión y alcanza tus objetivos financieros con nuestra herramienta.</p>
  </div>
  <button class="dark-mode-btn" onclick="toggleDarkMode()">🌙</button>

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

  <div class="grafica-titulo" id="graficaTitulo">Progresión de tu Inversión</div>
  <canvas id="grafica" height="120"></canvas>

  <div class="tabla-titulo" id="tablaTitulo">Tabla Amortizada de Inversión</div>
  <div class="tabla-container">
    <table id="tablaResultados">
      <thead>
        <tr>
          <th>Mes</th>
          <th>Fecha</th>
          <th>Aportación ($)</th>
          <th>Interés ($)</th>
          <th>Total ($)</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
  </div>

  <script>
    let datosGrafica = [];
    let totalAportaciones = 0, totalInteres = 0, capital = 0;
    let chart = null;

    function toggleDarkMode() {
      document.body.classList.toggle("dark");
      updateChartColors();
      if (chart) {
        chart.update();
      }
    }

    function updateChartColors() {
      const isDarkMode = document.body.classList.contains('dark');
      const bgColor = isDarkMode ? '#1f1f1f' : '#fff';
      const textColor = isDarkMode ? '#e0e0e0' : '#666';
      const gridColor = isDarkMode ? 'rgba(255, 255, 255, 0.1)' : 'rgba(0, 0, 0, 0.1)';
      
      if (chart) {
        chart.options.scales.y.grid.color = gridColor;
        chart.options.scales.x.grid.color = gridColor;
        chart.options.scales.y.ticks.color = textColor;
        chart.options.scales.x.ticks.color = textColor;
        chart.options.plugins.legend.labels.color = textColor;
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
            <td>${formatCurrency(aportacion)}</td>
            <td>${formatCurrency(interes)}</td>
            <td>${formatCurrency(capital)}</td>
          </tr>
        `;

        datosGrafica.push({ mes: i, total: capital });
      }

      document.getElementById('resultado').innerHTML = `
        <strong>Resumen de Inversión:</strong><br>
        • Capital inicial: ${formatCurrency(capitalInicial)}<br>
        • Tasa anual: ${tasa}%<br>
        • Plazo: ${meses} meses (${(meses/12).toFixed(1)} años)<br>
        • Aportación mensual: ${formatCurrency(aportacion)}<br>
        <br>
        <strong>Totales:</strong><br>
        • Total aportado: ${formatCurrency(totalAportaciones)}<br>
        • Interés generado: ${formatCurrency(totalInteres)}<br>
        <br>
        <strong style="font-size: 1.1em;">➔ Total final: ${formatCurrency(capital)}</strong>
      `;

      if (cumpleObjetivo) {
        document.getElementById('resumenFinal').innerHTML = `
          🎉 <strong>¡Objetivo alcanzado en ${meses} meses (${(meses/12).toFixed(1)} años)!</strong>
        `;
      }

      // Mostrar elementos ocultos
      document.getElementById('tablaResultados').style.display = "table";
      document.getElementById('tablaTitulo').style.display = "block";
      document.getElementById('graficaTitulo').style.display = "block";

      generarGrafico();
    }

    function formatCurrency(value) {
      return new Intl.NumberFormat('es-MX', { 
        style: 'currency', 
        currency: 'MXN',
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      }).format(value);
    }

    function generarGrafico() {
      const ctx = document.getElementById('grafica').getContext('2d');
      const isDarkMode = document.body.classList.contains('dark');
      
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
            backgroundColor: isDarkMode ? 'rgba(43, 103, 119, 0.2)' : 'rgba(43, 103, 119, 0.1)',
            fill: true,
            tension: 0.3,
            borderWidth: 2,
            pointRadius: 3,
            pointBackgroundColor: '#fff',
            pointBorderColor: '#2b6777'
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: {
              position: 'top',
              labels: {
                color: isDarkMode ? '#e0e0e0' : '#666',
                font: {
                  weight: 'bold'
                }
              }
            },
            tooltip: {
              backgroundColor: isDarkMode ? '#2c2c2c' : '#fff',
              titleColor: isDarkMode ? '#fff' : '#333',
              bodyColor: isDarkMode ? '#e0e0e0' : '#666',
              borderColor: isDarkMode ? '#444' : '#ddd',
              borderWidth: 1,
              callbacks: {
                label: (context) => {
                  return ` ${formatCurrency(context.raw)}`;
                }
              }
            }
          },
          scales: {
            y: {
              beginAtZero: false,
              grid: {
                color: isDarkMode ? 'rgba(255, 255, 255, 0.1)' : 'rgba(0, 0, 0, 0.1)'
              },
              ticks: {
                color: isDarkMode ? '#e0e0e0' : '#666',
                callback: (value) => formatCurrency(value)
              }
            },
            x: {
              grid: {
                color: isDarkMode ? 'rgba(255, 255, 255, 0.1)' : 'rgba(0, 0, 0, 0.1)'
              },
              ticks: {
                color: isDarkMode ? '#e0e0e0' : '#666'
              }
            }
          }
        }
      });
    }

    function descargarPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF({
        orientation: 'portrait',
        unit: 'mm',
        format: 'a4'
      });
      
      const isDarkMode = document.body.classList.contains('dark');
      const textColor = isDarkMode ? '#e0e0e0' : '#333';
      const bgColor = isDarkMode ? '#121212' : '#f4f6f8';
      
      // Configuración inicial
      doc.setDocumentProperties({
        title: 'Reporte de Inversión',
        subject: 'Cálculo de interés compuesto',
        creator: 'Calculadora de Inversión'
      });
      
      // Logo o título
      doc.setFontSize(20);
      doc.setTextColor(43, 103, 119);
      doc.setFont('helvetica', 'bold');
      doc.text("Reporte de Inversión", 105, 20, { align: 'center' });
      
      // Línea decorativa
      doc.setDrawColor(43, 103, 119);
      doc.setLineWidth(0.5);
      doc.line(20, 25, 190, 25);
      
      // Información resumida
      doc.setFontSize(12);
      doc.setTextColor(textColor);
      doc.setFont('helvetica', 'normal');
      
      const capitalInicial = parseFloat(document.getElementById('capitalInicial').value) || 0;
      const tasa = parseFloat(document.getElementById('tasa').value) || 0;
      const plazo = parseInt(document.getElementById('plazo').value) || 0;
      const aportacion = parseFloat(document.getElementById('aportacion').value) || 0;
      
      // Sección de datos
      doc.setFillColor(isDarkMode ? '#2c2c2c' : '#e6f2f5');
      doc.rect(20, 30, 170, 30, 'F');
      doc.text("Datos de la inversión", 25, 35);
      doc.text(`• Capital inicial: ${formatCurrency(capitalInicial)}`, 25, 42);
      doc.text(`• Tasa anual: ${tasa}% | Plazo: ${plazo} meses (${(plazo/12).toFixed(1)} años)`, 25, 49);
      doc.text(`• Aportación mensual: ${formatCurrency(aportacion)}`, 25, 56);
      
      // Sección de resultados
      doc.setFillColor(isDarkMode ? '#2a3f2a' : '#e8f5e9');
      doc.rect(20, 65, 170, 25, 'F');
      doc.text("Resultados finales", 25, 70);
      doc.setFont('helvetica', 'bold');
      doc.text(`➔ Total acumulado: ${formatCurrency(capital)}`, 25, 77);
      doc.setFont('helvetica', 'normal');
      doc.text(`• Total aportado: ${formatCurrency(totalAportaciones)}`, 25, 84);
      doc.text(`• Interés generado: ${formatCurrency(totalInteres)}`, 25, 91);
      
      // Tabla de amortización
      doc.autoTable({
        html: '#tablaResultados',
        startY: 95,
        theme: isDarkMode ? 'plain' : 'grid',
        headStyles: {
          fillColor: isDarkMode ? '#2c2c2c' : '#2b6777',
          textColor: 255,
          fontSize: 10,
          cellPadding: 5
        },
        bodyStyles: {
          fontSize: 8,
          textColor: textColor,
          cellPadding: 4,
          fillColor: isDarkMode ? '#1f1f1f' : bgColor
        },
        margin: { top: 10 },
        styles: {
          overflow: 'linebreak',
          cellWidth: 'wrap'
        },
        columnStyles: {
          0: { cellWidth: 15 },
          1: { cellWidth: 25 },
          2: { cellWidth: 25 },
          3: { cellWidth: 25 },
          4: { cellWidth: 25 }
        }
      });
      
      // Gráfico
      const canvas = document.getElementById('grafica');
      const imgData = canvas.toDataURL('image/png');
      doc.addImage(imgData, 'PNG', 20, doc.lastAutoTable.finalY + 10, 170, 80);
      
      // Pie de página
      doc.setFontSize(10);
      doc.setTextColor(100);
      doc.text("Generado el " + new Date().toLocaleDateString('es-MX'), 105, 285, { align: 'center' });
      
      doc.save('reporte_inversion.pdf');
    }

    function descargarCSV() {
      if (datosGrafica.length === 0) {
        alert("Primero calcula una inversión.");
        return;
      }
      
      let csv = "Mes,Fecha,Aportación ($),Interés ($),Total ($)\n";
      const rows = document.querySelectorAll('#tablaResultados tbody tr');
      
      rows.forEach(row => {
        const cells = row.querySelectorAll('td');
        csv += `"${cells[0].textContent}","${cells[1].textContent}","${cells[2].textContent.replace('$','')}","${cells[3].textContent.replace('$','')}","${cells[4].textContent.replace('$','')}"\n`;
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
