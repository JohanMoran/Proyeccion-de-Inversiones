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
      --primario: #3a8da8;
      --hover: #2b6777;
      --tabla-head: #1f1f1f;
      --boton-texto: #fff;
      --portada: #1a2035;
      --verde: #28a745;
      --verde-hover: #218838;
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
    .boton-calcular:hover {
      background-color: var(--verde-hover);
    }
    .result {
      margin-top: 20px;
      font-size: 16px;
      font-weight: bold;
      color: var(--primario);
    }
    #tablaResultados {
      width: 100%;
      margin-top: 20px;
      border-collapse: collapse;
      background-color: #fff;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
      display: none;
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
      background-color: var(--primario);
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
      display: none;
    }
    body.dark .tabla-titulo {
      background-color: var(--primario);
    }
    select {
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 5px;
      margin-top: 5px;
      background-color: #fff;
      transition: background-color 0.3s, color 0.3s;
    }
    body.dark select {
      background-color: #2a2a2a;
      color: #e0e0e0;
      border: 1px solid #555;
    }
    .highlight {
      color: var(--verde);
      font-weight: bold;
    }
    .resumen-titulo {
      font-size: 18px;
      margin-bottom: 10px;
      display: block;
    }
    .resumen-datos {
      color: var(--primario);
      font-weight: normal;
    }
    @media only screen and (max-width: 768px) {
      body {
        padding: 15px;
        font-size: 14px;
      }
      #portada {
        padding: 15px;
        min-height: 120px;
      }
      .input-container {
        flex-direction: column;
        align-items: flex-start;
        gap: 5px;
      }
      .input-container input,
      .input-container select {
        width: 100% !important;
      }
      .input-container span {
        padding-left: 0;
        font-size: 13px;
        margin-bottom: 10px;
      }
      .buttons {
        flex-direction: column;
      }
      .buttons button {
        width: 100%;
        margin-bottom: 8px;
      }
      .dark-mode-btn {
        bottom: 10px;
        right: 10px;
        padding: 8px 12px;
      }
      #tablaResultados th, 
      #tablaResultados td {
        padding: 8px 5px;
        font-size: 12px;
      }
      .result {
        font-size: 14px;
      }
      .resumen-titulo {
        font-size: 16px;
      }
      canvas {
        height: 250px !important;
      }
    }
    @media only screen and (max-width: 480px) {
      body {
        padding: 10px;
      }
      #portada {
        padding: 10px;
        min-height: 100px;
      }
      label {
        font-size: 15px;
      }
      .input-container span {
        font-size: 12px;
      }
      #tablaResultados {
        font-size: 11px;
      }
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

  <label>Aportación:</label>
  <div class="input-container">
    <input type="number" id="aportacion" />
    <span>¿Cuánto puedes destinar a tu inversión periódicamente para incrementar tus rendimientos?</span>
  </div>

  <label>Periodicidad de aportación:</label>
  <div class="input-container">
    <select id="periodicidad" style="width: 20%; padding: 10px; border-radius: 5px;">
      <option value="1">Mensual</option>
      <option value="2">Bimestral</option>
      <option value="3">Trimestral</option>
      <option value="6">Semestral</option>
      <option value="12">Anual</option>
    </select>
    <span>Selecciona con qué frecuencia realizarás tus aportaciones</span>
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
    Calculadora de Interés Compuesto con Aportación periódica. Herramienta para uso estrictamente con fines informativos y educativos.
  </div>

  <div class="buttons">
    <button class="boton-calcular" onclick="calcular()">Calcular</button>
    <button onclick="descargarCSV()">Descargar Excel</button>
    <button onclick="descargarPDF()">Descargar PDF</button>
  </div>

  <div class="result" id="resultado"></div>
  <div class="result" id="resumenFinal"></div>
  <canvas id="grafica" height="300"></canvas>

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
    let darkMode = false;

    function toggleDarkMode() {
      darkMode = !darkMode;
      document.body.classList.toggle("dark");
      localStorage.setItem('darkMode', darkMode);
      
      if (chart) {
        chart.options.scales.y.grid.color = darkMode ? 'rgba(255, 255, 255, 0.1)' : 'rgba(0, 0, 0, 0.1)';
        chart.options.scales.x.grid.color = darkMode ? 'rgba(255, 255, 255, 0.1)' : 'rgba(0, 0, 0, 0.1)';
        chart.update();
      }
    }

    // Verificar preferencia de modo oscuro al cargar
    if (localStorage.getItem('darkMode') === 'true') {
      toggleDarkMode();
    }

    function calcular() {
      const capitalInicial = parseFloat(document.getElementById('capitalInicial').value) || 0;
      const tasa = parseFloat(document.getElementById('tasa').value) || 0;
      const plazo = parseInt(document.getElementById('plazo').value) || 0;
      const aportacion = parseFloat(document.getElementById('aportacion').value) || 0;
      const periodicidad = parseInt(document.getElementById('periodicidad').value) || 1;
      const capitalObjetivo = parseFloat(document.getElementById('capitalObjetivo').value) || null;
      const fechaInput = document.getElementById('fechaInicio').value;
      const fechaInicio = fechaInput ? new Date(fechaInput) : new Date();

      if (plazo <= 0 || tasa <= 0) {
        alert("Por favor, ingresa un plazo y una tasa válidos.");
        return;
      }

      if (plazo > 600) {
        alert("El plazo máximo es de 600 meses (50 años)");
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
          capital += interes;
          if ((i - 1) % periodicidad === 0) {
            capital += aportacion;
            totalAportaciones += aportacion;
          }
          if (capital >= capitalObjetivo) {
            meses = i;
            cumpleObjetivo = true;
            break;
          }
        }
        capital = capitalInicial;
        totalAportaciones = 0;
      }

      for (let i = 1; i <= meses; i++) {
        const interes = capital * tasaMensual;
        totalInteres += interes;
        capital += interes;
        
        const esAportacion = (i - 1) % periodicidad === 0;
        if (esAportacion) {
          capital += aportacion;
          totalAportaciones += aportacion;
        }

        const fecha = new Date(fechaInicio);
        fecha.setMonth(fecha.getMonth() + i);

        tabla.innerHTML += `
          <tr>
            <td>${i}</td>
            <td>${fecha.toLocaleDateString('es-MX')}</td>
            <td>${esAportacion ? formatCurrency(aportacion) : '$0.00'}</td>
            <td>${formatCurrency(interes)}</td>
            <td>${formatCurrency(capital)}</td>
          </tr>
        `;

        datosGrafica.push({ mes: i, total: capital });
      }

      let periodicidadTexto = '';
      switch(periodicidad) {
        case 1: periodicidadTexto = 'mensual'; break;
        case 2: periodicidadTexto = 'bimestral'; break;
        case 3: periodicidadTexto = 'trimestral'; break;
        case 6: periodicidadTexto = 'semestral'; break;
        case 12: periodicidadTexto = 'anual'; break;
      }

      // Cálculo de la Tasa Efectiva Generada
      const inversionTotal = capitalInicial + totalAportaciones;
      const gananciaNeta = capital - inversionTotal;
      const tasaEfectiva = (gananciaNeta / inversionTotal) * 100;

      // Cálculo del Porcentaje de Utilidad Anualizado (CAGR)
      let utilidadAnualizada = 0;
      if (meses > 12) {
          const años = meses / 12;
          utilidadAnualizada = (Math.pow(capital / inversionTotal, 1/años) - 1;
          utilidadAnualizada = (utilidadAnualizada * 100).toFixed(2);
      }

      document.getElementById('resultado').innerHTML = `
        <span class="resumen-titulo"><strong>RESUMEN DE INVERSIÓN</strong></span>
        <span class="resumen-datos">Capital inicial: ${formatCurrency(capitalInicial)}</span><br>
        <span class="resumen-datos">Tasa nominal anual: ${tasa}%</span><br>
        ${meses > 12 ? `<span class="resumen-datos highlight">Tasa efectiva generada: ${tasaEfectiva.toFixed(2)}%</span><br>` : ''}
        ${meses > 12 ? `<span class="resumen-datos highlight">Rendimiento anualizado: ${utilidadAnualizada}%</span><br>` : ''}
        <span class="resumen-datos">Plazo: ${meses} meses (${(meses/12).toFixed(1)} años)</span><br>
        <span class="resumen-datos">Aportación ${periodicidadTexto}: ${formatCurrency(aportacion)}</span><br>
        <span class="resumen-datos">Total aportado: ${formatCurrency(totalAportaciones)}</span><br>
        <span class="resumen-datos">Total interés generado: ${formatCurrency(totalInteres)}</span><br>
        <span class="resumen-datos"><strong>Total al final del plazo: ${formatCurrency(capital)}</strong></span>
      `;

      if (cumpleObjetivo) {
        const años = Math.floor(meses / 12);
        const mesesRestantes = meses % 12;
        
        let textoMeses = "";
        if (años > 0) {
          textoMeses += `${años} ${años === 1 ? 'año' : 'años'}`;
        }
        if (mesesRestantes > 0) {
          if (años > 0) textoMeses += " y ";
          textoMeses += `${mesesRestantes} ${mesesRestantes === 1 ? 'mes' : 'meses'}`;
        }
        if (meses < 12) {
          textoMeses = `${meses} ${meses === 1 ? 'mes' : 'meses'}`;
        }

        document.getElementById('resumenFinal').innerHTML = `
          🎉 <strong>¡Objetivo de ${formatCurrency(capitalObjetivo)} alcanzado en ${textoMeses}!</strong>
        `;
      }

      document.getElementById('tablaResultados').style.display = "table";
      document.getElementById('tablaTitulo').style.display = "block";

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
      
      if (chart) {
        chart.destroy();
      }

      const gridColor = darkMode ? 'rgba(255, 255, 255, 0.1)' : 'rgba(0, 0, 0, 0.1)';

      chart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: datosGrafica.map(d => `Mes ${d.mes}`),
          datasets: [{
            label: 'Total acumulado (MXN)',
            data: datosGrafica.map(d => d.total),
            borderColor: darkMode ? '#3a8da8' : '#2b6777',
            backgroundColor: darkMode ? 'rgba(58, 141, 168, 0.1)' : 'rgba(43, 103, 119, 0.1)',
            fill: true,
            tension: 0.3
          }]
        },
        options: {
          responsive: true,
          plugins: {
            legend: {
              position: 'top',
              labels: {
                color: darkMode ? '#e0e0e0' : '#333'
              }
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
                color: darkMode ? '#e0e0e0' : '#333',
                callback: (value) => formatCurrency(value)
              },
              grid: {
                color: gridColor
              }
            },
            x: {
              ticks: {
                color: darkMode ? '#e0e0e0' : '#333'
              },
              grid: {
                color: gridColor
              }
            }
          }
        }
      });
    }

    function descargarPDF() {
      if (datosGrafica.length === 0) {
        alert("Primero calcula una inversión.");
        return;
      }

      const { jsPDF } = window.jspdf;
      const doc = new jsPDF({
        orientation: 'portrait',
        unit: 'mm',
        format: 'a4'
      });
      
      doc.setFontSize(20);
      doc.setTextColor(43, 103, 119);
      doc.setFont('helvetica', 'bold');
      doc.text("Reporte de Inversión", 105, 15, { align: 'center' });
      doc.setDrawColor(43, 103, 119);
      doc.setLineWidth(0.5);
      doc.line(20, 20, 190, 20);
      
      doc.setFontSize(12);
      doc.setTextColor(0, 0, 0);
      doc.setFont('helvetica', 'normal');
      doc.setFillColor(240, 240, 240);
      doc.rect(20, 25, 170, 30, 'F');
      doc.text("Datos de la inversión", 25, 30);
      
      const capitalInicial = parseFloat(document.getElementById('capitalInicial').value) || 0;
      const tasa = parseFloat(document.getElementById('tasa').value) || 0;
      const plazo = parseInt(document.getElementById('plazo').value) || 0;
      const aportacion = parseFloat(document.getElementById('aportacion').value) || 0;
      const periodicidad = parseInt(document.getElementById('periodicidad').value) || 1;
      
      let periodicidadTexto = '';
      switch(periodicidad) {
        case 1: periodicidadTexto = 'mensual'; break;
        case 2: periodicidadTexto = 'bimestral'; break;
        case 3: periodicidadTexto = 'trimestral'; break;
        case 6: periodicidadTexto = 'semestral'; break;
        case 12: periodicidadTexto = 'anual'; break;
      }

      doc.text(`Capital inicial: ${formatCurrency(capitalInicial)}`, 25, 37);
      doc.text(`Tasa anual: ${tasa}% | Plazo: ${plazo} meses`, 25, 44);
      doc.text(`Aportación ${periodicidadTexto}: ${formatCurrency(aportacion)}`, 25, 51);
      
      doc.setFillColor(230, 245, 230);
      doc.rect(20, 60, 170, 20, 'F');
      doc.text("Resultados finales", 25, 65);
      doc.text(`Total aportado: ${formatCurrency(totalAportaciones)}`, 25, 72);
      doc.text(`Interés generado: ${formatCurrency(totalInteres)}`, 100, 72);
      doc.setFont('helvetica', 'bold');
      doc.text(`Total acumulado: ${formatCurrency(capital)}`, 25, 79);
      doc.setFont('helvetica', 'normal');

      if (chart) {
        chart.update();
      }

      setTimeout(() => {
        const canvas = document.getElementById('grafica');
        const imgData = canvas.toDataURL('image/png', 1.0);
        doc.addImage(imgData, 'PNG', 20, 85, 170, 80);

        doc.autoTable({
          html: '#tablaResultados',
          startY: 170,
          theme: 'grid',
          headStyles: {
            fillColor: [43, 103, 119],
            textColor: 255,
            fontSize: 10
          },
          bodyStyles: {
            fontSize: 8
          },
          columnStyles: {
            0: { cellWidth: 15 },
            1: { cellWidth: 25 },
            2: { cellWidth: 25 },
            3: { cellWidth: 25 },
            4: { cellWidth: 25 }
          }
        });

        doc.setFontSize(10);
        doc.setTextColor(100);
        doc.text("© Calculadora de Inversión - " + new Date().toLocaleDateString(), 105, 285, { align: 'center' });
        
        doc.save('reporte_inversion.pdf');
      }, 300);
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
        const aportacionValue = cells[2].textContent === '$0.00' ? '0' : cells[2].textContent.replace('$','');
        csv += `"${cells[0].textContent}","${cells[1].textContent}","${aportacionValue}","${cells[3].textContent.replace('$','')}","${cells[4].textContent.replace('$','')}"\n`;
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
