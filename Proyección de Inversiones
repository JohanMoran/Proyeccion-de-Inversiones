<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <!-- Eliminado el título del head ya que usamos la imagen de portada -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.28/jspdf.plugin.autotable.min.js"></script>
  <style>
    :root {
      --fondo-claro: #f4f6f8;
      --texto-claro: #333;
      --primario: #2b6777;
      --primario2: #7d2b77; /* Color para segunda inversión */
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
      max-width: 1200px; /* Aumentado para acomodar dos columnas */
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
      display: flex;
      align-items: center;
      justify-content: center;
      min-height: 150px;
    }
    #portada img {
      width: 100%;
      object-fit: cover;
      border-radius: 8px;
    }
    .contenedor-comparacion {
      display: flex;
      gap: 20px;
      flex-wrap: wrap;
    }
    .inversion-container {
      flex: 1;
      min-width: 300px;
      background: #fff;
      padding: 15px;
      border-radius: 8px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }
    body.dark .inversion-container {
      background: #1f1f1f;
    }
    .inversion-container h2 {
      color: var(--primario);
      text-align: center;
      margin-top: 0;
    }
    .inversion-container:nth-child(2) h2 {
      color: var(--primario2);
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
      width: 100%;
      padding-left: 10px;
      word-wrap: break-word;
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
    .boton-comparar {
      background-color: var(--primario2);
      width: 160px;
    }
    .boton-calcular:hover {
      background-color: var(--verde-hover);
    }
    .boton-comparar:hover {
      background-color: #6a2465;
    }
    .result {
      margin-top: 20px;
      font-size: 16px;
      font-weight: bold;
    }
    .inversion-container:nth-child(1) .result {
      color: var(--primario);
    }
    .inversion-container:nth-child(2) .result {
      color: var(--primario2);
    }
    #tablaResultados, #tablaResultados2 {
      width: 100%;
      margin-top: 20px;
      border-collapse: collapse;
      background-color: #fff;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
      display: none;
    }
    #tablaResultados th, 
    #tablaResultados td,
    #tablaResultados2 th,
    #tablaResultados2 td {
      padding: 12px 8px;
      text-align: center;
      border-bottom: 1px solid #eee;
      white-space: nowrap;
    }
    #tablaResultados th {
      background-color: var(--primario);
      color: white;
      position: sticky;
      top: 0;
    }
    #tablaResultados2 th {
      background-color: var(--primario2);
      color: white;
      position: sticky;
      top: 0;
    }
    body.dark #tablaResultados,
    body.dark #tablaResultados2 {
      background-color: #1f1f1f;
      color: #e0e0e0;
    }
    body.dark #tablaResultados th {
      background-color: #2c2c2c;
    }
    body.dark #tablaResultados2 th {
      background-color: #4c2c4c;
    }
    body.dark #tablaResultados td,
    body.dark #tablaResultados2 td {
      border-color: #444;
    }
    canvas {
      margin-top: 30px;
      background-color: #fff;
      border-radius: 8px;
      padding: 10px;
      width: 100% !important;
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
    .tabla-container {
      overflow-x: auto;
      margin-top: 20px;
      border-radius: 8px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }
    .tabla-titulo {
      background-color: var(--primario);
      color: white;
      padding: 12px;
      border-radius: 8px 8px 0 0;
      font-weight: bold;
      display: none;
    }
    .tabla-titulo2 {
      background-color: var(--primario2);
      color: white;
      padding: 12px;
      border-radius: 8px 8px 0 0;
      font-weight: bold;
      display: none;
    }
    body.dark .tabla-titulo {
      background-color: #2c2c2c;
    }
    body.dark .tabla-titulo2 {
      background-color: #4c2c4c;
    }
    .grafica-comparativa {
      margin-top: 30px;
      display: none;
    }
  </style>
</head>
<body>
  <div id="portada">
    <img src="https://raw.githubusercontent.com/JohanMoran/Proyeccion-de-Inversiones/main/ROBPAIERO_TUASESORDECONFIANZA.PNG" 
         alt="Calculadora de Inversión"
         style="width: 100%; max-width: 900px; height: auto; border-radius: 8px;">
  </div>
  <button class="dark-mode-btn" onclick="toggleDarkMode()">🌙 Modo Oscuro</button>

  <div class="contenedor-comparacion">
    <!-- PRIMERA INVERSIÓN -->
    <div class="inversion-container">
      <h2>Inversión Principal</h2>
      
      <label>MONTO INICIAL:</label>
      <div class="input-container">
        <input type="number" id="capitalInicial" />
        <span>¿Con qué cantidad cuentas en este momento?</span>
      </div>

      <label>Tasa Anual (%):</label>
      <div class="input-container">
        <input type="number" id="tasa" step="0.01" />
        <span>Tasa de Interés anual (10% - 15% conservador)</span>
      </div>

      <label>Plazo (en meses):</label>
      <div class="input-container">
        <input type="number" id="plazo" min="1" />
        <span>¿Cuántos meses vas a invertir?</span>
      </div>

      <label>Aportación mensual:</label>
      <div class="input-container">
        <input type="number" id="aportacion" />
        <span>¿Cuánto puedes aportar cada mes?</span>
      </div>

      <label>Fecha de inicio:</label>
      <div class="input-container">
        <input type="date" id="fechaInicio" />
        <span>Fecha de inicio de la inversión</span>
      </div>

      <label>Capital objetivo (opcional):</label>
      <div class="input-container">
        <input type="number" id="capitalObjetivo" placeholder="Ej: 500000" />
        <span>Monto que deseas alcanzar</span>
      </div>

      <div class="buttons">
        <button class="boton-calcular" onclick="calcular(1)">Calcular</button>
        <button onclick="descargarCSV(1)">Descargar Excel</button>
        <button onclick="descargarPDF(1)">Descargar PDF</button>
      </div>

      <div class="result" id="resultado1"></div>
      <canvas id="grafica1" height="300"></canvas>

      <div class="tabla-titulo" id="tablaTitulo1">Tabla Amortizada de Inversión</div>
      <div class="tabla-container">
        <table id="tablaResultados1">
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
    </div>

    <!-- SEGUNDA INVERSIÓN (COMPARACIÓN) -->
    <div class="inversion-container">
      <h2>Inversión Comparativa</h2>
      
      <label>MONTO INICIAL:</label>
      <div class="input-container">
        <input type="number" id="capitalInicial2" />
        <span>¿Con qué cantidad cuentas en este momento?</span>
      </div>

      <label>Tasa Anual (%):</label>
      <div class="input-container">
        <input type="number" id="tasa2" step="0.01" />
        <span>Tasa de Interés anual (10% - 15% conservador)</span>
      </div>

      <label>Plazo (en meses):</label>
      <div class="input-container">
        <input type="number" id="plazo2" min="1" />
        <span>¿Cuántos meses vas a invertir?</span>
      </div>

      <label>Aportación mensual:</label>
      <div class="input-container">
        <input type="number" id="aportacion2" />
        <span>¿Cuánto puedes aportar cada mes?</span>
      </div>

      <label>Fecha de inicio:</label>
      <div class="input-container">
        <input type="date" id="fechaInicio2" />
        <span>Fecha de inicio de la inversión</span>
      </div>

      <label>Capital objetivo (opcional):</label>
      <div class="input-container">
        <input type="number" id="capitalObjetivo2" placeholder="Ej: 500000" />
        <span>Monto que deseas alcanzar</span>
      </div>

      <div class="buttons">
        <button class="boton-comparar" onclick="calcular(2)">Calcular</button>
        <button onclick="descargarCSV(2)">Descargar Excel</button>
        <button onclick="descargarPDF(2)">Descargar PDF</button>
      </div>

      <div class="result" id="resultado2"></div>
      <canvas id="grafica2" height="300"></canvas>

      <div class="tabla-titulo2" id="tablaTitulo2">Tabla Amortizada de Inversión</div>
      <div class="tabla-container">
        <table id="tablaResultados2">
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
    </div>
  </div>

  <!-- GRÁFICA COMPARATIVA -->
  <div class="grafica-comparativa" id="graficaComparativaContainer">
    <h2 style="text-align: center; color: var(--primario);">Comparación de Inversiones</h2>
    <canvas id="graficaComparativa" height="300"></canvas>
  </div>

  <div class="leyenda">
    Calculadora de Interés Compuesto con Aportación mensual. Herramienta para uso estrictamente con fines informativos y educativos.
  </div>

  <script>
    let datosGrafica1 = [], datosGrafica2 = [];
    let totalAportaciones1 = 0, totalInteres1 = 0, capital1 = 0;
    let totalAportaciones2 = 0, totalInteres2 = 0, capital2 = 0;
    let chart1 = null, chart2 = null, chartComparativo = null;

    function toggleDarkMode() {
      document.body.classList.toggle("dark");
      if (chart1) chart1.update();
      if (chart2) chart2.update();
      if (chartComparativo) chartComparativo.update();
    }

    function calcular(inversionNum) {
      const prefix = inversionNum === 1 ? '' : '2';
      const capitalInicial = parseFloat(document.getElementById(`capitalInicial${prefix}`).value) || 0;
      const tasa = parseFloat(document.getElementById(`tasa${prefix}`).value) || 0;
      const plazo = parseInt(document.getElementById(`plazo${prefix}`).value) || 0;
      const aportacion = parseFloat(document.getElementById(`aportacion${prefix}`).value) || 0;
      const capitalObjetivo = parseFloat(document.getElementById(`capitalObjetivo${prefix}`).value) || null;
      const fechaInicio = new Date(document.getElementById(`fechaInicio${prefix}`).value);

      if (plazo <= 0 || tasa <= 0) {
        alert("Por favor, ingresa un plazo y una tasa válidos.");
        return;
      }

      if (inversionNum === 1) {
        capital1 = capitalInicial;
        totalInteres1 = 0;
        totalAportaciones1 = 0;
        datosGrafica1 = [];
      } else {
        capital2 = capitalInicial;
        totalInteres2 = 0;
        totalAportaciones2 = 0;
        datosGrafica2 = [];
      }

      const tasaMensual = tasa / 12 / 100;
      const tabla = document.querySelector(`#tablaResultados${prefix} tbody`);
      tabla.innerHTML = '';
      let meses = plazo;
      let cumpleObjetivo = false;

      if (capitalObjetivo) {
        for (let i = 1; i <= 600; i++) {
          const interes = (inversionNum === 1 ? capital1 : capital2) * tasaMensual;
          if (inversionNum === 1) {
            capital1 += interes + aportacion;
            if (capital1 >= capitalObjetivo) {
              meses = i;
              cumpleObjetivo = true;
              break;
            }
          } else {
            capital2 += interes + aportacion;
            if (capital2 >= capitalObjetivo) {
              meses = i;
              cumpleObjetivo = true;
              break;
            }
          }
        }
        if (inversionNum === 1) capital1 = capitalInicial;
        else capital2 = capitalInicial;
      }

      for (let i = 1; i <= meses; i++) {
        let interes, capital;
        if (inversionNum === 1) {
          interes = capital1 * tasaMensual;
          totalInteres1 += interes;
          capital1 += interes + aportacion;
          totalAportaciones1 += aportacion;
          capital = capital1;
        } else {
          interes = capital2 * tasaMensual;
          totalInteres2 += interes;
          capital2 += interes + aportacion;
          totalAportaciones2 += aportacion;
          capital = capital2;
        }

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

        if (inversionNum === 1) {
          datosGrafica1.push({ mes: i, total: capital1 });
        } else {
          datosGrafica2.push({ mes: i, total: capital2 });
        }
      }

      document.getElementById(`resultado${prefix}`).innerHTML = `
        <strong>Resumen de Inversión:</strong><br>
        Capital inicial: ${formatCurrency(capitalInicial)}<br>
        Tasa de interés anual: ${tasa}%<br>
        Plazo: ${meses} meses<br>
        Aportación mensual: ${formatCurrency(aportacion)}<br>
        Total aportado: ${formatCurrency(inversionNum === 1 ? totalAportaciones1 : totalAportaciones2)}<br>
        Total interés generado: ${formatCurrency(inversionNum === 1 ? totalInteres1 : totalInteres2)}<br>
        <strong>Total al final del plazo: ${formatCurrency(inversionNum === 1 ? capital1 : capital2)}</strong>
      `;

      if (cumpleObjetivo) {
        document.getElementById(`resultado${prefix}`).innerHTML += `
          <br>🎉 <strong>¡Objetivo alcanzado en ${meses} meses!</strong>
        `;
      }

      document.getElementById(`tablaResultados${prefix}`).style.display = "table";
      document.getElementById(`tablaTitulo${prefix}`).style.display = "block";

      generarGrafico(inversionNum);
      
      // Mostrar gráfica comparativa si ambas inversiones están calculadas
      if (datosGrafica1.length > 0 && datosGrafica2.length > 0) {
        generarGraficoComparativo();
      }
    }

    function formatCurrency(value) {
      return new Intl.NumberFormat('es-MX', { 
        style: 'currency', 
        currency: 'MXN',
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      }).format(value);
    }

    function generarGrafico(inversionNum) {
      const prefix = inversionNum === 1 ? '1' : '2';
      const ctx = document.getElementById(`grafica${prefix}`).getContext('2d');
      const color = inversionNum === 1 ? '#2b6777' : '#7d2b77';
      
      if (inversionNum === 1 && chart1) {
        chart1.destroy();
      } else if (inversionNum === 2 && chart2) {
        chart2.destroy();
      }

      const chart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: (inversionNum === 1 ? datosGrafica1 : datosGrafica2).map(d => `Mes ${d.mes}`),
          datasets: [{
            label: `Total acumulado (MXN) - Inv. ${inversionNum}`,
            data: (inversionNum === 1 ? datosGrafica1 : datosGrafica2).map(d => d.total),
            borderColor: color,
            backgroundColor: inversionNum === 1 ? 'rgba(43, 103, 119, 0.1)' : 'rgba(125, 43, 119, 0.1)',
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
                  return ` ${formatCurrency(context.raw)}`;
                }
              }
            }
          },
          scales: {
            y: {
              beginAtZero: false,
              ticks: {
                callback: (value) => formatCurrency(value)
              }
            }
          }
        }
      });

      if (inversionNum === 1) {
        chart1 = chart;
      } else {
        chart2 = chart;
      }
    }

    function generarGraficoComparativo() {
      const ctx = document.getElementById('graficaComparativa').getContext('2d');
      document.getElementById('graficaComparativaContainer').style.display = 'block';
      
      if (chartComparativo) {
        chartComparativo.destroy();
      }

      // Determinar el máximo de meses entre ambas inversiones
      const maxMeses = Math.max(
        datosGrafica1.length > 0 ? datosGrafica1[datosGrafica1.length - 1].mes : 0,
        datosGrafica2.length > 0 ? datosGrafica2[datosGrafica2.length - 1].mes : 0
      );

      // Crear etiquetas para todos los meses
      const labels = [];
      for (let i = 1; i <= maxMeses; i++) {
        labels.push(`Mes ${i}`);
      }

      // Mapear datos para que coincidan con todas las etiquetas
      const data1 = labels.map((_, index) => {
        const mes = index + 1;
        const dato = datosGrafica1.find(d => d.mes === mes);
        return dato ? dato.total : null;
      });

      const data2 = labels.map((_, index) => {
        const mes = index + 1;
        const dato = datosGrafica2.find(d => d.mes === mes);
        return dato ? dato.total : null;
      });

      chartComparativo = new Chart(ctx, {
        type: 'line',
        data: {
          labels: labels,
          datasets: [
            {
              label: 'Inversión Principal',
              data: data1,
              borderColor: '#2b6777',
              backgroundColor: 'rgba(43, 103, 119, 0.1)',
              fill: true,
              tension: 0.3
            },
            {
              label: 'Inversión Comparativa',
              data: data2,
              borderColor: '#7d2b77',
              backgroundColor: 'rgba(125, 43, 119, 0.1)',
              fill: true,
              tension: 0.3
            }
          ]
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
                  return ` ${context.dataset.label}: ${formatCurrency(context.raw)}`;
                }
              }
            }
          },
          scales: {
            y: {
              beginAtZero: false,
              ticks: {
                callback: (value) => formatCurrency(value)
              }
            }
          }
        }
      });
    }

    function descargarPDF(inversionNum) {
      const prefix = inversionNum === 1 ? '' : '2';
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF({
        orientation: 'portrait',
        unit: 'mm',
        format: 'a4'
      });
      
      doc.setFontSize(20);
      doc.setTextColor(inversionNum === 1 ? 43 : 125, 103 : 43, 119 : 119);
      doc.setFont('helvetica', 'bold');
      doc.text(`Reporte de Inversión ${inversionNum === 1 ? 'Principal' : 'Comparativa'}`, 105, 15, { align: 'center' });
      
      doc.setDrawColor(inversionNum === 1 ? 43 : 125, 103 : 43, 119 : 119);
      doc.setLineWidth(0.5);
      doc.line(20, 20, 190, 20);
      
      doc.setFontSize(12);
      doc.setTextColor(0, 0, 0);
      doc.setFont('helvetica', 'normal');
      
      const capitalInicial = parseFloat(document.getElementById(`capitalInicial${prefix}`).value) || 0;
      const tasa = parseFloat(document.getElementById(`tasa${prefix}`).value) || 0;
      const plazo = parseInt(document.getElementById(`plazo${prefix}`).value) || 0;
      const aportacion = parseFloat(document.getElementById(`aportacion${prefix}`).value) || 0;
      
      doc.setFillColor(240, 240, 240);
      doc.rect(20, 25, 170, 30, 'F');
      doc.text("Datos de la inversión", 25, 30);
      doc.text(`Capital inicial: ${formatCurrency(capitalInicial)}`, 25, 37);
      doc.text(`Tasa anual: ${tasa}% | Plazo: ${plazo} meses`, 25, 44);
      doc.text(`Aportación mensual: ${formatCurrency(aportacion)}`, 25, 51);
      
      doc.setFillColor(230, 245, 230);
      doc.rect(20, 60, 170, 20, 'F');
      doc.text("Resultados finales", 25, 65);
      doc.text(`Total aportado: ${formatCurrency(inversionNum === 1 ? totalAportaciones1 : totalAportaciones2)}`, 25, 72);
      doc.text(`Interés generado: ${formatCurrency(inversionNum === 1 ? totalInteres1 : totalInteres2)}`, 100, 72);
      doc.setFont('helvetica', 'bold');
      doc.text(`Total acumulado: ${formatCurrency(inversionNum === 1 ? capital1 : capital2)}`, 25, 79);
      doc.setFont('helvetica', 'normal');
      
      doc.autoTable({
        html: `#tablaResultados${prefix}`,
        startY: 85,
        theme: 'grid',
        headStyles: {
          fillColor: [inversionNum === 1 ? 43 : 125, inversionNum === 1 ? 103 : 43, 119],
          textColor: 255,
          fontSize: 10
        },
        bodyStyles: {
          fontSize: 8
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
      
      const canvas = document.getElementById(`grafica${prefix}`);
      const imgData = canvas.toDataURL('image/png');
      doc.addImage(imgData, 'PNG', 20, doc.lastAutoTable.finalY + 10, 170, 80);
      
      doc.setFontSize(10);
      doc.setTextColor(100);
      doc.text("© Calculadora de Inversión - " + new Date().toLocaleDateString(), 105, 285, { align: 'center' });
      
      doc.save(`reporte_inversion_${inversionNum === 1 ? 'principal' : 'comparativa'}.pdf`);
    }

    function descargarCSV(inversionNum) {
      const prefix = inversionNum === 1 ? '' : '2';
      const datos = inversionNum === 1 ? datosGrafica1 : datosGrafica2;
      
      if (datos.length === 0) {
        alert("Primero calcula la inversión.");
        return;
      }
      
      let csv = "Mes,Fecha,Aportación ($),Interés ($),Total ($)\n";
      const rows = document.querySelectorAll(`#tablaResultados${prefix} tbody tr`);
      
      rows.forEach(row => {
        const cells = row.querySelectorAll('td');
        csv += `"${cells[0].textContent}","${cells[1].textContent}","${cells[2].textContent.replace('$','')}","${cells[3].textContent.replace('$','')}","${cells[4].textContent.replace('$','')}"\n`;
      });
      
      const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
      const link = document.createElement('a');
      link.href = URL.createObjectURL(blob);
      link.download = `inversion_${inversionNum === 1 ? 'principal' : 'comparativa'}.csv`;
      link.click();
    }
  </script>
</body>
</html>
