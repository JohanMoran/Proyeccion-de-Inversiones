<!DOCTYPE html>
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

  <div class="leyenda">
    Calculadora de Interés Compuesto con Amortización mensual. Herramienta para uso estrictamente con fines informativos y educativos.
  </div>

  <div class="buttons">
    <button class="boton-calcular" onclick="calcular()">Calcular</button>
    <button onclick="descargarCSV()">Descargar Excel</button>
    <button onclick="descargarPDF()">Descargar PDF</button>
  </div>

  <div class="result" id="resultado"></div>
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

    function toggleDarkMode() {
      document.body.classList.toggle("dark");
    }

    function calcular() {
      const capitalInicial = parseFloat(document.getElementById('capitalInicial').value) || 0;
      const tasa = parseFloat(document.getElementById('tasa').value) || 0;
      const plazo = parseInt(document.getElementById('plazo').value) || 0;
      const aportacion = parseFloat(document.getElementById('aportacion').value) || 0;
      const capitalObjetivo = parseFloat(document.getElementById('capitalObjetivo').value) || null;

      if (!capitalInicial || !tasa || !plazo || !aportacion) {
        alert("Por favor, completa todos los campos");
        return;
      }

      const tasaMensual = (tasa / 100) / 12;
      let totalInteresAcumulado = 0;
      let saldo = capitalInicial;
      const resultados = [];

      for (let mes = 1; mes <= plazo; mes++) {
        const interesMes = saldo * tasaMensual;
        saldo += aportacion + interesMes;
        totalInteresAcumulado += interesMes;
        totalAportaciones += aportacion;

        resultados.push({
          mes,
          fecha: new Date(new Date().setMonth(new Date().getMonth() + mes)),
          aportacion: aportacion,
          interes: interesMes,
          total: saldo
        });
      }

      capital = saldo;

      // Actualiza tabla
      let tablaHTML = '';
      resultados.forEach(result => {
        tablaHTML += `
          <tr>
            <td>${result.mes}</td>
            <td>${result.fecha.toLocaleDateString()}</td>
            <td>$${result.aportacion.toLocaleString('es-MX')}</td>
            <td>$${result.interes.toLocaleString('es-MX')}</td>
            <td>$${result.total.toLocaleString('es-MX')}</td>
          </tr>
        `;
      });

      document.getElementById('tablaResultados').querySelector('tbody').innerHTML = tablaHTML;

      // Mostrar gráfico
      datosGrafica = resultados.map(result => result.total);
      crearGrafica();

      // Mostrar resultado final
      const resultadoFinal = document.getElementById('resultado');
      resultadoFinal.innerHTML = `
        <p><strong>Capital inicial:</strong> $${capitalInicial.toLocaleString('es-MX')}</p>
        <p><strong>Tasa anual:</strong> ${tasa}%</p>
        <p><strong>Plazo:</strong> ${plazo} meses</p>
        <p><strong>Aportación mensual:</strong> $${aportacion.toLocaleString('es-MX')}</p>
        <p><strong>Total aportado:</strong> $${totalAportaciones.toLocaleString('es-MX')}</p>
        <p><strong>Total interés generado:</strong> $${totalInteresAcumulado.toLocaleString('es-MX')}</p>
        <p><strong>Total al final del plazo:</strong> $${capital.toLocaleString('es-MX')}</p>
      `;
    }

    function crearGrafica() {
      const ctx = document.getElementById('grafica').getContext('2d');
      new Chart(ctx, {
        type: 'line',
        data: {
          labels: Array.from({ length: datosGrafica.length }, (_, i) => i + 1),
          datasets: [{
            label: 'Valor acumulado',
            data: datosGrafica,
            borderColor: '#28a745',
            backgroundColor: 'rgba(40, 167, 69, 0.2)',
            fill: true,
            tension: 0.4
          }]
        },
        options: {
          responsive: true,
          scales: {
            x: {
              title: {
                display: true,
                text: 'Meses'
              }
            },
            y: {
              title: {
                display: true,
                text: 'Monto en pesos'
              }
            }
          }
        }
      });
    }

    function descargarPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();

      // Datos calculados
      const capitalInicial = document.getElementById('capitalInicial').value;
      const tasa = document.getElementById('tasa').value;
      const plazo = document.getElementById('plazo').value;
      const aportacion = document.getElementById('aportacion').value;
      const totalAportado = totalAportaciones.toLocaleString('es-MX', { style: 'currency', currency: 'MXN' }).replace('MXN', '').trim();
      const totalInteres = totalInteres.toLocaleString('es-MX', { style: 'currency', currency: 'MXN' }).replace('MXN', '').trim();
      const totalFinal = capital.toLocaleString('es-MX', { style: 'currency', currency: 'MXN' }).replace('MXN', '').trim();

      doc.setFontSize(14);
      doc.text("Resumen de Inversión", 10, 10);
      doc.text(`Capital Inicial: $${capitalInicial}`, 10, 20);
      doc.text(`Tasa Anual: ${tasa}%`, 10, 30);
      doc.text(`Plazo: ${plazo} meses`, 10, 40);
      doc.text(`Aportación mensual: $${aportacion}`, 10, 50);
      doc.text(`Total aportado: $${totalAportado}`, 10, 60);
      doc.text(`Total interés generado: $${totalInteres}`, 10, 70);
      doc.text(`Total al final del plazo: $${totalFinal}`, 10, 80);

      doc.autoTable({
        html: '#tablaResultados',
        startY: 90,
        theme: 'grid'
      });

      const canvas = document.getElementById("grafica");
      const imgData = canvas.toDataURL("image/png");
      doc.addImage(imgData, 'PNG', 10, doc.lastAutoTable.finalY + 10, 180, 100);

      doc.save('inversion.pdf');
    }
  </script>
</body>
</html>
