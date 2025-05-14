<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <meta name="description" content="Calculadora de inversión con interés compuesto y aportaciones mensuales">
  <title>Calculadora de inversión con interés compuesto</title>
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
      --error: #dc3545;
    }
    body.dark {
      --fondo-claro: #121212;
      --texto-claro: #e0e0e0;
      --tabla-head: #1f1f1f;
      --boton-texto: #fff;
      --portada: #1a2035;
    }
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background-color: var(--fondo-claro);
      color: var(--texto-claro);
      padding: 20px;
      max-width: 900px;
      margin: auto;
      transition: background-color 0.4s, color 0.4s;
      line-height: 1.6;
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
      position: relative;
      overflow: hidden;
    }
    #portada img {
      width: 100%;
      max-height: 200px;
      object-fit: contain;
      border-radius: 8px;
      transition: transform 0.3s;
    }
    #portada:hover img {
      transform: scale(1.02);
    }
    .form-group {
      margin-bottom: 20px;
    }
    label {
      margin-top: 15px;
      display: block;
      font-weight: 600;
      font-size: var(--texto-grande);
    }
    input {
      padding: 12px;
      border: 1px solid #ccc;
      width: 100%;
      border-radius: 5px;
      margin-top: 5px;
      background-color: #fff;
      transition: all 0.3s;
      font-size: 15px;
    }
    input:focus {
      border-color: var(--primario);
      outline: none;
      box-shadow: 0 0 0 3px rgba(43, 103, 119, 0.2);
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
      min-width: 100px;
    }
    .input-container span {
      font-weight: normal;
      font-size: 14px;
      color: var(--texto-claro);
      width: 100%;
      padding-left: 10px;
      word-wrap: break-word;
      line-height: 1.5;
    }
    button {
      padding: 12px 20px;
      background-color: var(--primario);
      color: var(--boton-texto);
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-size: 15px;
      font-weight: 600;
      transition: all 0.3s;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
    }
    button:hover {
      transform: translateY(-2px);
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    }
    button:active {
      transform: translateY(0);
    }
    .boton-calcular {
      background-color: var(--verde);
      width: 180px;
    }
    .boton-calcular:hover {
      background-color: var(--verde-hover);
    }
    .result {
      margin-top: 20px;
      font-size: 16px;
      padding: 15px;
      border-radius: 8px;
      background-color: rgba(43, 103, 119, 0.1);
      border-left: 4px solid var(--primario);
    }
    .result strong {
      color: var(--primario);
    }
    #resumenFinal {
      background-color: rgba(40, 167, 69, 0.1);
      border-left: 4px solid var(--verde);
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
      padding: 12px 10px;
      text-align: center;
      border-bottom: 1px solid #eee;
    }
    #tablaResultados th {
      background-color: var(--primario);
      color: white;
      position: sticky;
      top: 0;
      font-weight: 600;
    }
    #tablaResultados tr:hover {
      background-color: rgba(43, 103, 119, 0.05);
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
    body.dark #tablaResultados tr:hover {
      background-color: rgba(43, 103, 119, 0.1);
    }
    canvas {
      margin-top: 30px;
      background-color: #fff;
      border-radius: 8px;
      padding: 15px;
      width: 100% !important;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }
    body.dark canvas {
      background-color: #1f1f1f;
    }
    .buttons {
      display: flex;
      flex-wrap: wrap;
      gap: 12px;
      margin-top: 20px;
    }
    .dark-mode-btn {
      position: fixed;
      bottom: 20px;
      right: 20px;
      z-index: 999;
      box-shadow: 0 4px 10px rgba(0,0,0,0.2);
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
      color: #555;
      text-align: center;
      padding: 10px;
    }
    body.dark .leyenda {
      color: #aaa;
    }
    .tabla-container {
      overflow-x: auto;
      margin-top: 20px;
      border-radius: 8px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
      max-height: 500px;
      overflow-y: auto;
    }
    .tabla-titulo {
      background-color: var(--primario);
      color: white;
      padding: 15px;
      border-radius: 8px 8px 0 0;
      font-weight: bold;
      display: none;
      position: sticky;
      top: 0;
      z-index: 10;
    }
    body.dark .tabla-titulo {
      background-color: #2c2c2c;
    }
    .error-message {
      color: var(--error);
      font-size: 14px;
      margin-top: 5px;
      display: none;
    }
    .loading {
      display: none;
      text-align: center;
      margin: 20px 0;
    }
    .loading-spinner {
      border: 4px solid rgba(0, 0, 0, 0.1);
      border-radius: 50%;
      border-top: 4px solid var(--primario);
      width: 40px;
      height: 40px;
      animation: spin 1s linear infinite;
      margin: 0 auto;
    }
    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }
    .tooltip-icon {
      display: inline-block;
      width: 18px;
      height: 18px;
      background-color: var(--primario);
      color: white;
      border-radius: 50%;
      text-align: center;
      font-size: 12px;
      line-height: 18px;
      margin-left: 5px;
      cursor: help;
      position: relative;
    }
    .tooltip-text {
      visibility: hidden;
      width: 200px;
      background-color: #333;
      color: #fff;
      text-align: center;
      border-radius: 6px;
      padding: 10px;
      position: absolute;
      z-index: 1;
      bottom: 125%;
      left: 50%;
      transform: translateX(-50%);
      opacity: 0;
      transition: opacity 0.3s;
      font-size: 13px;
      font-weight: normal;
    }
    .tooltip-icon:hover .tooltip-text {
      visibility: visible;
      opacity: 1;
    }
    @media (max-width: 768px) {
      body {
        padding: 15px;
      }
      .input-container {
        flex-direction: column;
        align-items: flex-start;
      }
      .input-container input {
        width: 100%;
      }
      .buttons {
        flex-direction: column;
      }
      button {
        width: 100%;
      }
      .boton-calcular {
        width: 100%;
      }
      #portada {
        min-height: 120px;
      }
    }
  </style>
</head>
<body>
  <div id="portada">
    <img src="https://raw.githubusercontent.com/JohanMoran/Proyeccion-de-Inversiones/main/ROBPAIERO_TUASESORDECONFIANZA.PNG" 
         alt="Calculadora de Inversión"
         loading="lazy">
  </div>
  <button class="dark-mode-btn" onclick="toggleDarkMode()" aria-label="Cambiar modo oscuro/claro">🌙</button>

  <div class="form-group">
    <label for="capitalInicial">MONTO INICIAL: <span class="tooltip-icon">?<span class="tooltip-text">Cantidad inicial con la que comenzarás tu inversión</span></span></label>
    <div class="input-container">
      <input type="number" id="capitalInicial" min="0" step="1000" />
      <span>¿Con qué cantidad cuentas en este momento? ¿Con cuánto empezarás tu inversión?</span>
    </div>
    <div class="error-message" id="capitalError"></div>
  </div>

  <div class="form-group">
    <label for="tasa">Tasa Anual (%): <span class="tooltip-icon">?<span class="tooltip-text">Tasa de interés anual esperada para tu inversión</span></span></label>
    <div class="input-container">
      <input type="number" id="tasa" min="0" max="100" step="0.01" />
      <span>Tasa de Interés anual, inversionistas conservadores (renta fija) 10% - 15%.</span>
    </div>
    <div class="error-message" id="tasaError"></div>
  </div>

  <div class="form-group">
    <label for="plazo">Plazo (en meses): <span class="tooltip-icon">?<span class="tooltip-text">Duración total de tu inversión en meses</span></span></label>
    <div class="input-container">
      <input type="number" id="plazo" min="1" max="600" />
      <span>¿Cuántos años vas a realizar la inversión? ¿Cuál es tu horizonte de inversión?</span>
    </div>
    <div class="error-message" id="plazoError"></div>
  </div>

  <div class="form-group">
    <label for="aportacion">Aportación mensual: <span class="tooltip-icon">?<span class="tooltip-text">Cantidad que agregarás a tu inversión cada mes</span></span></label>
    <div class="input-container">
      <input type="number" id="aportacion" min="0" />
      <span>¿Cuánto puedes destinar a tu inversión al mes para incrementar tus rendimientos?</span>
    </div>
    <div class="error-message" id="aportacionError"></div>
  </div>

  <div class="form-group">
    <label for="fechaInicio">Fecha de inicio: <span class="tooltip-icon">?<span class="tooltip-text">Fecha en la que planeas comenzar tu inversión</span></span></label>
    <div class="input-container">
      <input type="date" id="fechaInicio" />
      <span>Fecha en que tienes pensado dar inicio a tu inversión</span>
    </div>
    <div class="error-message" id="fechaError"></div>
  </div>

  <div class="form-group">
    <label for="capitalObjetivo">Capital objetivo (opcional): <span class="tooltip-icon">?<span class="tooltip-text">Monto que deseas alcanzar con tu inversión</span></span></label>
    <div class="input-container">
      <input type="number" id="capitalObjetivo" min="0" placeholder="Ej: 500000" />
      <span>¿Ya tienes un objetivo (ir de viaje, comprar un auto, etc.)? Elige un monto con el que alcanzarás ese objetivo</span>
    </div>
  </div>

  <div class="leyenda">
    Calculadora de Interés Compuesto con Aportación mensual. Herramienta para uso estrictamente con fines informativos y educativos.
  </div>

  <div class="loading" id="loadingIndicator">
    <div class="loading-spinner"></div>
    <p>Calculando tu inversión...</p>
  </div>

  <div class="buttons">
    <button class="boton-calcular" onclick="calcular()">
      <svg width="18" height="18" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M9 9H15V15H9V9Z" fill="currentColor"/>
        <path fill-rule="evenodd" clip-rule="evenodd" d="M3 5C3 3.89543 3.89543 3 5 3H19C20.1046 3 21 3.89543 21 5V19C21 20.1046 20.1046 21 19 21H5C3.89543 21 3 20.1046 3 19V5ZM5 5H19V19H5V5Z" fill="currentColor"/>
      </svg>
      Calcular
    </button>
    <button onclick="descargarCSV()" id="btnCSV" disabled>
      <svg width="18" height="18" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M13 9H18.5L13 3.5V9Z" fill="currentColor"/>
        <path d="M6 2H14L20 8V20C20 21.1 19.1 22 18 22H6C4.9 22 4 21.1 4 20V4C4 2.9 4.9 2 6 2Z" fill="currentColor"/>
        <path d="M16 18H8V16H16V18Z" fill="currentColor"/>
        <path d="M16 15H8V13H16V15Z" fill="currentColor"/>
        <path d="M8 11H10V12H8V11Z" fill="currentColor"/>
      </svg>
      Descargar Excel
    </button>
    <button onclick="descargarPDF()" id="btnPDF" disabled>
      <svg width="18" height="18" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M13 9H18.5L13 3.5V9Z" fill="currentColor"/>
        <path d="M6 2H14L20 8V20C20 21.1 19.1 22 18 22H6C4.9 22 4 21.1 4 20V4C4 2.9 4.9 2 6 2Z" fill="currentColor"/>
        <path d="M8 15H16V17H8V15Z" fill="currentColor"/>
        <path d="M16 11H8V13H16V11Z" fill="currentColor"/>
      </svg>
      Descargar PDF
    </button>
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
    let darkMode = localStorage.getItem('darkMode') === 'true';

    // Aplicar modo oscuro si estaba activado
    if (darkMode) {
      document.body.classList.add('dark');
    }

    function toggleDarkMode() {
      document.body.classList.toggle('dark');
      darkMode = !darkMode;
      localStorage.setItem('darkMode', darkMode);
      
      // Actualizar ícono del botón
      const darkModeBtn = document.querySelector('.dark-mode-btn');
      darkModeBtn.textContent = darkMode ? '☀️' : '🌙';
      
      if (chart) {
        chart.update();
      }
    }

    // Validación de campos
    function validarCampos() {
      let valido = true;
      
      const capitalInicial = parseFloat(document.getElementById('capitalInicial').value);
      const tasa = parseFloat(document.getElementById('tasa').value);
      const plazo = parseInt(document.getElementById('plazo').value);
      const aportacion = parseFloat(document.getElementById('aportacion').value);
      const fechaInicio = document.getElementById('fechaInicio').value;
      
      // Resetear mensajes de error
      document.querySelectorAll('.error-message').forEach(el => {
        el.style.display = 'none';
        el.textContent = '';
      });
      
      // Validar capital inicial
      if (isNaN(capitalInicial) || capitalInicial < 0) {
        document.getElementById('capitalError').textContent = 'Ingresa un monto inicial válido';
        document.getElementById('capitalError').style.display = 'block';
        valido = false;
      }
      
      // Validar tasa
      if (isNaN(tasa) || tasa <= 0 || tasa > 100) {
        document.getElementById('tasaError').textContent = 'Ingresa una tasa anual válida (0-100%)';
        document.getElementById('tasaError').style.display = 'block';
        valido = false;
      }
      
      // Validar plazo
      if (isNaN(plazo) || plazo <= 0 || plazo > 600) {
        document.getElementById('plazoError').textContent = 'Ingresa un plazo válido (1-600 meses)';
        document.getElementById('plazoError').style.display = 'block';
        valido = false;
      }
      
      // Validar aportación
      if (isNaN(aportacion) || aportacion < 0) {
        document.getElementById('aportacionError').textContent = 'Ingresa una aportación mensual válida';
        document.getElementById('aportacionError').style.display = 'block';
        valido = false;
      }
      
      // Validar fecha
      if (!fechaInicio) {
        document.getElementById('fechaError').textContent = 'Selecciona una fecha de inicio';
        document.getElementById('fechaError').style.display = 'block';
        valido = false;
      }
      
      return valido;
    }

    function calcular() {
      if (!validarCampos()) {
        return;
      }
      
      // Mostrar indicador de carga
      document.getElementById('loadingIndicator').style.display = 'block';
      
      // Deshabilitar botones mientras se calcula
      document.querySelector('.boton-calcular').disabled = true;
      
      // Usar setTimeout para permitir que la UI se actualice antes del cálculo intensivo
      setTimeout(() => {
        try {
          const capitalInicial = parseFloat(document.getElementById('capitalInicial').value);
          const tasa = parseFloat(document.getElementById('tasa').value);
          const plazo = parseInt(document.getElementById('plazo').value);
          const aportacion = parseFloat(document.getElementById('aportacion').value);
          const capitalObjetivo = parseFloat(document.getElementById('capitalObjetivo').value) || null;
          const fechaInicio = new Date(document.getElementById('fechaInicio').value);

          capital = capitalInicial;
          totalInteres = 0;
          totalAportaciones = 0;
          const tasaMensual = tasa / 12 / 100;
          const tabla = document.querySelector('#tablaResultados tbody');
          tabla.innerHTML = '';
          datosGrafica = [];
          let meses = plazo;
          let cumpleObjetivo = false;

          // Si hay capital objetivo, calcular cuántos meses se necesitan
          if (capitalObjetivo && capitalObjetivo > 0) {
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
          }

          // Generar tabla de resultados
          for (let i = 1; i <= meses; i++) {
            const interes = capital * tasaMensual;
            totalInteres += interes;
            capital += interes + aportacion;
            totalAportaciones += aportacion;

            const fecha = new Date(fechaInicio);
            fecha.setMonth(fecha.getMonth() + i);

            // Solo agregar filas visibles para no sobrecargar el DOM
            if (i <= 100 || i % 10 === 0 || i === meses) {
              tabla.innerHTML += `
                <tr>
                  <td>${i}</td>
                  <td>${fecha.toLocaleDateString('es-MX', { day: '2-digit', month: '2-digit', year: 'numeric' })}</td>
                  <td>${formatCurrency(aportacion)}</td>
                  <td>${formatCurrency(interes)}</td>
                  <td>${formatCurrency(capital)}</td>
                </tr>
              `;
            }

            // Para la gráfica, muestrear puntos para no sobrecargar
            if (i <= 12 || i % 3 === 0 || i === meses) {
              datosGrafica.push({ mes: i, total: capital });
            }
          }

          // Mostrar resultados
          document.getElementById('resultado').innerHTML = `
            <strong>Resumen de Inversión:</strong><br>
            • Capital inicial: ${formatCurrency(capitalInicial)}<br>
            • Tasa de interés anual: ${tasa.toFixed(2)}%<br>
            • Plazo: ${meses} meses (${(meses/12).toFixed(1)} años)<br>
            • Aportación mensual: ${formatCurrency(aportacion)}<br>
            • Total aportado: ${formatCurrency(totalAportaciones)}<br>
            • Total interés generado: ${formatCurrency(totalInteres)}<br>
            <strong>• Total al final del plazo: ${formatCurrency(capital)}</strong>
          `;

          if (cumpleObjetivo) {
            document.getElementById('resumenFinal').innerHTML = `
              🎉 <strong>¡Objetivo alcanzado en ${meses} meses (${(meses/12).toFixed(1)} años)!</strong> Tu inversión llegó a ${formatCurrency(capital)}
            `;
          } else {
            document.getElementById('resumenFinal').innerHTML = '';
          }

          document.getElementById('tablaResultados').style.display = "table";
          document.getElementById('tablaTitulo').style.display = "block";

          // Habilitar botones de descarga
          document.getElementById('btnCSV').disabled = false;
          document.getElementById('btnPDF').disabled = false;

          generarGrafico();
        } catch (error) {
          console.error('Error en el cálculo:', error);
          alert('Ocurrió un error al calcular la inversión. Por favor verifica los datos.');
        } finally {
          // Ocultar indicador de carga
          document.getElementById('loadingIndicator').style.display = 'none';
          document.querySelector('.boton-calcular').disabled = false;
        }
      }, 100);
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

      // Configuración responsive
      const gradient = ctx.createLinearGradient(0, 0, 0, 400);
      gradient.addColorStop(0, 'rgba(43, 103, 119, 0.3)');
      gradient.addColorStop(1, 'rgba(43, 103, 119, 0.05)');

      chart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: datosGrafica.map(d => `Mes ${d.mes}`),
          datasets: [{
            label: 'Total acumulado (MXN)',
            data: datosGrafica.map(d => d.total),
            borderColor: '#2b6777',
            backgroundColor: gradient,
            fill: true,
            tension: 0.3,
            borderWidth: 2,
            pointRadius: 3,
            pointBackgroundColor: '#2b6777',
            pointHoverRadius: 5
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: {
              position: 'top',
              labels: {
                font: {
                  size: 14
                },
                padding: 20,
                usePointStyle: true,
                pointStyle: 'circle'
              }
            },
            tooltip: {
              callbacks: {
                label: (context) => {
                  return ` ${formatCurrency(context.raw)}`;
                },
                title: (context) => {
                  return `Mes ${datosGrafica[context[0].dataIndex].mes}`;
                }
              },
              displayColors: true,
              backgroundColor: darkMode ? '#333' : '#fff',
              titleColor: darkMode ? '#fff' : '#333',
              bodyColor: darkMode ? '#fff' : '#333',
              borderColor: darkMode ? '#555' : '#ddd',
              borderWidth: 1,
              padding: 12,
              bodyFont: {
                size: 14
              },
              titleFont: {
                size: 14,
                weight: 'bold'
              }
            }
          },
          scales: {
            x: {
              grid: {
                color: darkMode ? 'rgba(255, 255, 255, 0.1)' : 'rgba(0, 0, 0, 0.1)',
                drawBorder: false
              },
              ticks: {
                color: darkMode ? '#aaa' : '#666'
              }
            },
            y: {
              beginAtZero: false,
              grid: {
                color: darkMode ? 'rgba(255, 255, 255, 0.1)' : 'rgba(0, 0, 0, 0.1)',
                drawBorder: false
              },
              ticks: {
                color: darkMode ? '#aaa' : '#666',
                callback: (value) => formatCurrency(value)
              }
            }
          },
          interaction: {
            intersect: false,
            mode: 'index'
          },
          animation: {
            duration: 1000,
            easing: 'easeOutQuart'
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
      
      // Configuración inicial del documento
      doc.setDocumentProperties({
        title: 'Reporte de Inversión',
        subject: 'Proyección de inversión con interés compuesto',
        author: 'Calculadora de Inversión',
        keywords: 'inversión, interés compuesto, finanzas',
        creator: 'Calculadora de Inversión'
      });
      
      // Encabezado
      doc.setFontSize(20);
      doc.setTextColor(43, 103, 119);
      doc.setFont('helvetica', 'bold');
      doc.text("Reporte de Inversión", 105, 15, { align: 'center' });
      
      // Línea decorativa
      doc.setDrawColor(43, 103, 119);
      doc.setLineWidth(0.5);
      doc.line(20, 20, 190, 20);
      
      // Información básica
      doc.setFontSize(12);
      doc.setTextColor(0, 0, 0);
      doc.setFont('helvetica', 'normal');
      
      const capitalInicial = parseFloat(document.getElementById('capitalInicial').value) || 0;
      const tasa = parseFloat(document.getElementById('tasa').value) || 0;
      const plazo = parseInt(document.getElementById('plazo').value) || 0;
      const aportacion = parseFloat(document.getElementById('aportacion').value) || 0;
      const fechaInicio = document.getElementById('fechaInicio').value;
      const capitalObjetivo = parseFloat(document.getElementById('capitalObjetivo').value) || null;
      
      // Sección de datos
      doc.setFillColor(240, 240, 240);
      doc.rect(20, 25, 170, 30, 'F');
      doc.text("Datos de la inversión", 25, 30);
      doc.text(`Capital inicial: ${formatCurrency(capitalInicial)}`, 25, 37);
      doc.text(`Tasa anual: ${tasa.toFixed(2)}% | Plazo: ${plazo} meses (${(plazo/12).toFixed(1)} años)`, 25, 44);
      doc.text(`Aportación mensual: ${formatCurrency(aportacion)} | Fecha inicio: ${fechaInicio}`, 25, 51);
      
      if (capitalObjetivo) {
        doc.text(`Capital objetivo: ${formatCurrency(capitalObjetivo)}`, 25, 58);
      }
      
      // Sección de resultados
      doc.setFillColor(230, 245, 230);
      doc.rect(20, 60, 170, 20, 'F');
      doc.text("Resultados finales", 25, 65);
      doc.text(`Total aportado: ${formatCurrency(totalAportaciones)}`, 25, 72);
      doc.text(`Interés generado: ${formatCurrency(totalInteres)}`, 100, 72);
      doc.setFont('helvetica', 'bold');
      doc.text(`Total acumulado: ${formatCurrency(capital)}`, 25, 79);
      doc.setFont('helvetica', 'normal');
      
      // Tabla de amortización (primera página)
      doc.autoTable({
        html: '#tablaResultados',
        startY: 85,
        theme: 'grid',
        headStyles: {
          fillColor: [43, 103, 119],
          textColor: 255,
          fontSize: 10,
          halign: 'center'
        },
        bodyStyles: {
          fontSize: 8,
          halign: 'center'
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
        },
        pageBreak: 'avoid',
        tableWidth: 'wrap'
      });
      
      // Gráfico
      const canvas = document.getElementById('grafica');
      const imgData = canvas.toDataURL('image/png', 1.0);
      const imgWidth = 170;
      const imgHeight = 80;
      const pageHeight = doc.internal.pageSize.getHeight();
      const yPos = doc.lastAutoTable.finalY + 10;
      
      // Asegurarse de que el gráfico quepa en la página
      if (yPos + imgHeight > pageHeight - 20) {
        doc.addPage();
        doc.addImage(imgData, 'PNG', 20, 20, imgWidth, imgHeight);
      } else {
        doc.addImage(imgData, 'PNG', 20, yPos, imgWidth, imgHeight);
      }
      
      // Pie de página
      doc.setFontSize(10);
      doc.setTextColor(100);
      const pageCount = doc.internal.getNumberOfPages();
      for (let i = 1; i <= pageCount; i++) {
        doc.setPage
