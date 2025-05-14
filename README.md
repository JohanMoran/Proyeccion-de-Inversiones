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
      --primario2: #7d2b77;
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
      max-width: 1200px;
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
    .contenedor-principal {
      display: flex;
      gap: 30px;
      flex-wrap: wrap;
    }
    .formulario-container {
      flex: 1;
      min-width: 300px;
    }
    .resultados-container {
      flex: 2;
      min-width: 600px;
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
      margin-top: 10px;
    }
    .boton-calcular {
      background-color: var(--verde);
      width: 100%;
    }
    .boton-comparar {
      background-color: var(--primario2);
      width: 100%;
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
    #resultado1 {
      color: var(--primario);
    }
    #resultado2 {
      color: var(--primario2);
    }
    #tablaResultados {
      width: 100%;
      margin-top: 20px;
      border-collapse: collapse;
      background-color: #fff;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }
    #tablaResultados th, 
    #tablaResultados td {
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
    body.dark #tablaResultados {
      background-color: #1f1f1f;
      color: #e0e0e0;
    }
    body.dark #tablaResultados th {
      background-color: #2c2c2c;
    }
    body.dark #tablaResultados td {
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
    }
    .separador {
      height: 2px;
      background-color: #eee;
      margin: 20px 0;
    }
    body.dark .separador {
      background-color: #444;
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

  <div class="contenedor-principal">
    <!-- FORMULARIO -->
    <div class="formulario-container">
      <h2 style="color: var(--primario);">Inversión Principal</h2>
      
      <label>MONTO INICIAL:</label>
      <div class="input-container">
        <input type="number" id="capitalInicial" />
        <span>Capital inicial para la inversión principal</span>
      </div>

      <label>Tasa Anual (%):</label>
      <div class="input-container">
        <input type="number" id="tasa" step="0.01" />
        <span>Tasa de interés anual para la inversión principal</span>
      </div>

      <label>Plazo (en meses):</label>
      <div class="input-container">
        <input type="number" id="plazo" min="1" />
        <span>Plazo en meses para la inversión principal</span>
      </div>

      <label>Aportación mensual:</label>
      <div class="input-container">
        <input type="number" id="aportacion" />
        <span>Aportación mensual para la inversión principal</span>
      </div>

      <label>Fecha de inicio:</label>
      <div class="input-container">
        <input type="date" id="fechaInicio" />
        <span>Fecha de inicio para la inversión principal</span>
      </div>

      <label>Capital objetivo (opcional):</label>
      <div class="input-container">
        <input type="number" id="capitalObjetivo" placeholder="Ej: 500000" />
        <span>Objetivo financiero para la inversión principal</span>
      </div>

      <button class="boton-calcular" onclick="calcularInversion(1)">Calcular Inversión Principal</button>

      <div class="separador"></div>

      <h2 style="color: var(--primario2);">Inversión Comparativa</h2>
      
      <label>MONTO INICIAL:</label>
      <div class="input-container">
        <input type="number" id="capitalInicial2" />
        <span>Capital inicial para la inversión comparativa</span>
      </div>

      <label>Tasa Anual (%):</label>
      <div class="input-container">
        <input type="number" id="tasa2" step="0.01" />
        <span>Tasa de interés anual para la inversión comparativa</span>
      </div>

      <label>Plazo (en meses):</label>
      <div class="input-container">
        <input type="number" id="plazo2" min="1" />
        <span>Plazo en meses para la inversión comparativa</span>
      </div>

      <label>Aportación mensual:</label>
      <div class="input-container">
        <input type="number" id="aportacion2" />
        <span>Aportación mensual para la inversión comparativa</span>
      </div>

      <label>Fecha de inicio:</label>
      <div class="input-container">
        <input type="date" id="fechaInicio2" />
        <span>Fecha de inicio para la inversión comparativa</span>
      </div>

      <label>Capital objetivo (opcional):</label>
      <div class="input-container">
        <input type="number" id="capitalObjetivo2" placeholder="Ej: 500000" />
        <span>Objetivo financiero para la inversión comparativa</span>
      </div>

      <button class="boton-comparar" onclick="calcularInversion(2)">Calcular Inversión Comparativa</button>
    </div>

    <!-- RESULTADOS -->
    <div class="resultados-container">
      <div id="resultado1"></div>
      <div id="resultado2"></div>

      <canvas id="graficaComparativa" height="300"></canvas>

      <div class="tabla-titulo">Detalle de Inversión Principal</div>
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

      <div class="tabla-titulo" style="background-color: var(--primario2);">Detalle de Inversión Comparativa</div>
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

      <div class="buttons">
        <button onclick="descargarPDF()">Descargar PDF Completo</button>
        <button onclick="descargarCSV(1)">Descargar Excel (Principal)</button>
        <button onclick="descargarCSV(2)">Descargar Excel (Comparativa)</button>
      </div>
    </div>
  </div>

  <div class="leyenda">
    Calculadora de Interés Compuesto con Aportación mensual. Herramienta para uso estrictamente con fines informativos y educativos.
  </div>

  <script>
    // Variables globales
    let datosInversion1 = [], datosInversion2 = [];
    let totalAportaciones1 = 0, totalInteres1 = 0, capital1 = 0;
    let totalAportaciones2 = 0, totalInteres2 = 0, capital2 = 0;
    let chartComparativo = null;

    // Función para calcular una inversión (1: principal, 2: comparativa)
    function calcularInversion(inversionNum) {
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

      // Reiniciar variables
      if (inversionNum === 1) {
        capital1 = capitalInicial;
        totalInteres1 = 0;
        totalAportaciones1 = 0;
        datosInversion1 = [];
      } else {
        capital2 = capitalInicial;
        totalInteres2 = 0;
        totalAportaciones2 = 0;
        datosInversion2 = [];
      }

      const tasaMensual = tasa / 12 / 100;
      const tabla = document.querySelector(`#tablaResultados${inversionNum} tbody`);
      tabla.innerHTML = '';
      let meses = plazo;
      let cumpleObjetivo = false;

      // Calcular meses necesarios si hay objetivo
      if (capitalObjetivo) {
        let capitalTemp = capitalInicial;
        for (let i = 1; i <= 600; i++) {
          const interes = capitalTemp * tasaMensual;
          capitalTemp += interes + aportacion;
          if (capitalTemp >= capitalObjetivo) {
            meses = i;
            cumpleObjetivo = true;
            break;
          }
        }
        if (inversionNum === 1) capital1 = capitalInicial;
        else capital2 = capitalInicial;
      }

      // Calcular mes a mes
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

        // Agregar fila a la tabla
        tabla.innerHTML += `
          <tr>
            <td>${i}</td>
            <td>${fecha.toLocaleDateString('es-MX')}</td>
            <td>${formatCurrency(aportacion)}</td>
            <td>${formatCurrency(interes)}</td>
            <td>${formatCurrency(capital)}</td>
          </tr>
        `;

        // Guardar datos para gráfico
        if (inversionNum === 1) {
          datosInversion1.push({ mes: i, total: capital1 });
        } else {
          datosInversion2.push({ mes: i, total: capital2 });
        }
      }

      // Mostrar resultados
      document.getElementById(`resultado${inversionNum}`).innerHTML = `
        <strong>Resumen Inversión ${inversionNum === 1 ? 'Principal' : 'Comparativa'}:</strong><br>
        Capital inicial: ${formatCurrency(capitalInicial)}<br>
        Tasa anual: ${tasa}% | Plazo: ${meses} meses<br>
        Aportación mensual: ${formatCurrency(aportacion)}<br>
        Total aportado: ${formatCurrency(inversionNum === 1 ? totalAportaciones1 : totalAportaciones2)}<br>
        Interés generado: ${formatCurrency(inversionNum === 1 ? totalInteres1 : totalInteres2)}<br>
        <strong>Total final: ${formatCurrency(inversionNum === 1 ? capital1 : capital2)}</strong>
        ${cumpleObjetivo ? `<br>🎉 <strong>¡Objetivo alcanzado en ${meses} meses!</strong>` : ''}
      `;

      // Generar gráfico comparativo si hay datos en ambas inversiones
      if (datosInversion1.length > 0 && datosInversion2.length > 0) {
        generarGraficoComparativo();
      }
    }

    // Función para generar el gráfico comparativo
    function generarGraficoComparativo() {
      const ctx = document.getElementById('graficaComparativa').getContext('2d');
      
      if (chartComparativo) {
        chartComparativo.destroy();
      }

      // Determinar el máximo de meses entre ambas inversiones
      const maxMeses = Math.max(
        datosInversion1.length > 0 ? datosInversion1[datosInversion1.length - 1].mes : 0,
        datosInversion2.length > 0 ? datosInversion2[datosInversion2.length - 1].mes : 0
      );

      // Crear etiquetas para todos los meses
      const labels = [];
      for (let i = 1; i <= maxMeses; i++) {
        labels.push(`Mes ${i}`);
      }

      // Mapear datos para que coincidan con todas las etiquetas
      const data1 = labels.map((_, index) => {
        const mes = index + 1;
        const dato = datosInversion1.find(d => d.mes === mes);
        return dato ? dato.total : null;
      });

      const data2 = labels.map((_, index) => {
        const mes = index + 1;
        const dato = datosInversion2.find(d => d.mes === mes);
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
              tension: 0.3,
              borderWidth: 2
            },
            {
              label: 'Inversión Comparativa',
              data: data2,
              borderColor: '#7d2b77',
              backgroundColor: 'rgba(125, 43, 119, 0.1)',
              fill: true,
              tension: 0.3,
              borderWidth: 2
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

    // Función para formatear moneda
    function formatCurrency(value) {
      return new Intl.NumberFormat('es-MX', { 
        style: 'currency', 
        currency: 'MXN',
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      }).format(value);
    }

    // Función para descargar PDF
    function descargarPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF({
        orientation: 'portrait',
        unit: 'mm',
        format: 'a4'
      });
      
      // Título
      doc.setFontSize(20);
      doc.setTextColor(43, 103, 119);
      doc.setFont('helvetica', 'bold');
      doc.text("Reporte Comparativo de Inversiones", 105, 15, { align: 'center' });
      
      // Línea divisoria
      doc.setDrawColor(43, 103, 119);
      doc.setLineWidth(0.5);
      doc.line(20, 20, 190, 20);
      
      // Datos de inversión principal
      doc.setFontSize(12);
      doc.setTextColor(0, 0, 0);
      doc.setFont('helvetica', 'normal');
      doc.text("Inversión Principal", 20, 30);
      
      const capitalInicial1 = parseFloat(document.getElementById('capitalInicial').value) || 0;
      const tasa1 = parseFloat(document.getElementById('tasa').value) || 0;
      const plazo1 = parseInt(document.getElementById('plazo').value) || 0;
      const aportacion1 = parseFloat(document.getElementById('aportacion').value) || 0;
      
      doc.text(`Capital inicial: ${formatCurrency(capitalInicial1)}`, 20, 37);
      doc.text(`Tasa anual: ${tasa1}% | Plazo: ${plazo1} meses`, 20, 44);
      doc.text(`Aportación mensual: ${formatCurrency(aportacion1)}`, 20, 51);
      doc.text(`Total final: ${formatCurrency(capital1)}`, 20, 58);
      
      // Datos de inversión comparativa
      doc.text("Inversión Comparativa", 20, 70);
      
      const capitalInicial2 = parseFloat(document.getElementById('capitalInicial2').value) || 0;
      const tasa2 = parseFloat(document.getElementById('tasa2').value) || 0;
      const plazo2 = parseInt(document.getElementById('plazo2').value) || 0;
      const aportacion2 = parseFloat(document.getElementById('aportacion2').value) || 0;
      
      doc.text(`Capital inicial: ${formatCurrency(capitalInicial2)}`, 20, 77);
      doc.text(`Tasa anual: ${tasa2}% | Plazo: ${plazo2} meses`, 20, 84);
      doc.text(`Aportación mensual: ${formatCurrency(aportacion2)}`, 20, 91);
      doc.text(`Total final: ${formatCurrency(capital2)}`, 20, 98);
      
      // Gráfico comparativo
      const canvas = document.getElementById('graficaComparativa');
      const imgData = canvas.toDataURL('image/png');
      doc.addImage(imgData, 'PNG', 20, 105, 170, 80);
      
      // Tablas
      doc.autoTable({
        html: '#tablaResultados1',
        startY: 190,
        theme: 'grid',
        headStyles: {
          fillColor: [43, 103, 119],
          textColor: 255,
          fontSize: 10
        },
        bodyStyles: {
          fontSize: 8
        },
        margin: { top: 10 },
        columnStyles: {
          0: { cellWidth: 15 },
          1: { cellWidth: 25 },
          2: { cellWidth: 25 },
          3: { cellWidth: 25 },
          4: { cellWidth: 25 }
        }
      });
      
      doc.autoTable({
        html: '#tablaResultados2',
        startY: doc.lastAutoTable.finalY + 10,
        theme: 'grid',
        headStyles: {
          fillColor: [125, 43, 119],
          textColor: 255,
          fontSize: 10
        },
        bodyStyles: {
          fontSize: 8
        },
        margin: { top: 10 },
        columnStyles: {
          0: { cellWidth: 15 },
          1: { cellWidth: 25 },
          2: { cellWidth: 25 },
          3: { cellWidth: 25 },
          4: { cellWidth: 25 }
        }
      });
      
      // Pie de página
      doc.setFontSize(10);
      doc.setTextColor(100);
      doc.text("© Calculadora de Inversión - " + new Date().toLocaleDateString(), 105, 285, { align: 'center' });
      
      doc.save('reporte_comparativo_inversiones.pdf');
    }

    // Función para descargar CSV
    function descargarCSV(inversionNum) {
      const prefix = inversionNum === 1 ? '' : '2';
      const datos = inversionNum === 1 ? datosInversion1 : datosInversion2;
      
      if (datos.length === 0) {
        alert(`Primero calcula la inversión ${inversionNum === 1 ? 'principal' : 'comparativa'}.`);
        return;
      }
      
      let csv = "Mes,Fecha,Aportación ($),Interés ($),Total ($)\n";
      const rows = document.querySelectorAll(`#tablaResultados${inversionNum} tbody tr`);
      
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

    // Función para cambiar modo oscuro/claro
    function toggleDarkMode() {
      document.body.classList.toggle("dark");
      if (chartComparativo) {
        chartComparativo.update();
      }
    }
  </script>
</body>
</html>
