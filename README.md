<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Calculadora de Inversión Comparativa</title>
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
      --rojo: #dc3545;
      --rojo-hover: #c82333;
      --texto-grande: 16px;
      --border-radius: 8px;
      --sombra: 0 4px 12px rgba(0,0,0,0.1);
    }
    
    body.dark {
      --fondo-claro: #121212;
      --texto-claro: #e0e0e0;
      --tabla-head: #1f1f1f;
      --boton-texto: #fff;
      --sombra: 0 4px 12px rgba(0,0,0,0.3);
    }
    
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background-color: var(--fondo-claro);
      color: var(--texto-claro);
      padding: 25px;
      max-width: 1400px;
      margin: 0 auto;
      transition: background-color 0.4s, color 0.4s;
      line-height: 1.6;
    }
    
    #portada {
      background-color: var(--portada);
      color: white;
      text-align: center;
      padding: 25px;
      border-radius: var(--border-radius);
      margin-bottom: 30px;
      display: flex;
      align-items: center;
      justify-content: center;
      min-height: 180px;
      box-shadow: var(--sombra);
    }
    
    #portada img {
      width: 100%;
      max-height: 200px;
      object-fit: contain;
      border-radius: var(--border-radius);
    }
    
    .contenedor-principal {
      display: flex;
      gap: 40px;
      flex-wrap: wrap;
    }
    
    .formulario-container {
      flex: 1;
      min-width: 500px;
      padding: 25px;
      background: rgba(255, 255, 255, 0.8);
      border-radius: var(--border-radius);
      box-shadow: var(--sombra);
      backdrop-filter: blur(5px);
    }
    
    body.dark .formulario-container {
      background: rgba(30, 30, 30, 0.8);
    }
    
    .resultados-container {
      flex: 1.5;
      min-width: 600px;
    }
    
    .seccion-inversion {
      margin-bottom: 30px;
      padding: 20px;
      background: rgba(255, 255, 255, 0.9);
      border-radius: var(--border-radius);
      box-shadow: var(--sombra);
      transition: all 0.3s ease;
    }
    
    body.dark .seccion-inversion {
      background: rgba(40, 40, 40, 0.9);
    }
    
    .seccion-inversion:hover {
      transform: translateY(-3px);
      box-shadow: 0 6px 16px rgba(0,0,0,0.15);
    }
    
    body.dark .seccion-inversion:hover {
      box-shadow: 0 6px 16px rgba(0,0,0,0.4);
    }
    
    .titulo-seccion {
      color: var(--primario);
      font-size: 24px;
      margin-bottom: 25px;
      padding-bottom: 10px;
      border-bottom: 2px solid var(--primario);
      display: flex;
      align-items: center;
      gap: 10px;
    }
    
    .titulo-seccion.comparativa {
      color: var(--primario2);
      border-bottom-color: var(--primario2);
    }
    
    .titulo-seccion i {
      font-size: 1.2em;
    }
    
    label {
      margin-top: 18px;
      display: block;
      font-weight: 600;
      font-size: 17px;
      color: var(--texto-claro);
    }
    
    input {
      padding: 14px;
      border: 2px solid #ddd;
      width: 100%;
      border-radius: var(--border-radius);
      margin-top: 8px;
      background-color: rgba(255, 255, 255, 0.9);
      transition: all 0.3s ease;
      font-size: 16px;
    }
    
    input:focus {
      border-color: var(--primario);
      outline: none;
      box-shadow: 0 0 0 3px rgba(43, 103, 119, 0.2);
    }
    
    body.dark input {
      background-color: rgba(30, 30, 30, 0.9);
      border-color: #555;
      color: #f0f0f0;
    }
    
    body.dark input:focus {
      border-color: var(--primario);
      box-shadow: 0 0 0 3px rgba(43, 103, 119, 0.4);
    }
    
    .input-container {
      display: flex;
      align-items: center;
      gap: 15px;
      margin-top: 10px;
      margin-bottom: 20px;
    }
    
    .input-container input {
      width: 35%;
      min-width: 150px;
      padding: 12px;
    }
    
    .input-container span {
      font-weight: normal;
      font-size: 15px;
      color: var(--texto-claro);
      width: 100%;
      padding-left: 10px;
      word-wrap: break-word;
      line-height: 1.5;
    }
    
    button {
      padding: 14px 24px;
      background-color: var(--primario);
      color: var(--boton-texto);
      border: none;
      border-radius: var(--border-radius);
      cursor: pointer;
      font-size: 16px;
      font-weight: 600;
      transition: all 0.3s ease;
      margin-top: 15px;
      margin-bottom: 15px;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    
    button:hover {
      transform: translateY(-2px);
      box-shadow: 0 4px 8px rgba(0,0,0,0.2);
    }
    
    button:active {
      transform: translateY(0);
    }
    
    .boton-calcular {
      background-color: var(--verde);
      width: 100%;
    }
    
    .boton-calcular:hover {
      background-color: var(--verde-hover);
    }
    
    .boton-comparar {
      background-color: var(--primario2);
      width: 100%;
    }
    
    .boton-comparar:hover {
      background-color: #6a2465;
    }
    
    .boton-exportar {
      background-color: var(--primario);
      flex: 1;
    }
    
    .boton-exportar:hover {
      background-color: var(--hover);
    }
    
    .boton-limpiar {
      background-color: var(--rojo);
      margin-left: 10px;
    }
    
    .boton-limpiar:hover {
      background-color: var(--rojo-hover);
    }
    
    .result {
      margin-top: 25px;
      font-size: 17px;
      font-weight: bold;
      padding: 20px;
      background: rgba(43, 103, 119, 0.1);
      border-radius: var(--border-radius);
      line-height: 1.8;
    }
    
    body.dark .result {
      background: rgba(43, 103, 119, 0.2);
    }
    
    #resultado1 {
      color: var(--primario);
      border-left: 4px solid var(--primario);
    }
    
    #resultado2 {
      color: var(--primario2);
      background: rgba(125, 43, 119, 0.1);
      border-left: 4px solid var(--primario2);
    }
    
    body.dark #resultado2 {
      background: rgba(125, 43, 119, 0.2);
    }
    
    #tablaResultados1, #tablaResultados2 {
      width: 100%;
      margin-top: 20px;
      border-collapse: collapse;
      background-color: #fff;
      box-shadow: var(--sombra);
      font-size: 15px;
    }
    
    #tablaResultados1 th, 
    #tablaResultados1 td,
    #tablaResultados2 th,
    #tablaResultados2 td {
      padding: 14px 12px;
      text-align: center;
      border-bottom: 1px solid #eee;
    }
    
    #tablaResultados1 th {
      background-color: var(--primario);
      color: white;
      position: sticky;
      top: 0;
      font-size: 15px;
      font-weight: 600;
    }
    
    #tablaResultados2 th {
      background-color: var(--primario2);
      color: white;
      position: sticky;
      top: 0;
      font-size: 15px;
      font-weight: 600;
    }
    
    body.dark #tablaResultados1,
    body.dark #tablaResultados2 {
      background-color: #1f1f1f;
      color: #e0e0e0;
    }
    
    body.dark #tablaResultados1 th {
      background-color: #2c2c2c;
    }
    
    body.dark #tablaResultados2 th {
      background-color: #4c2c4c;
    }
    
    body.dark #tablaResultados1 td,
    body.dark #tablaResultados2 td {
      border-color: #444;
    }
    
    canvas {
      margin-top: 30px;
      background-color: #fff;
      border-radius: var(--border-radius);
      padding: 20px;
      width: 100% !important;
      height: 400px !important;
      box-shadow: var(--sombra);
    }
    
    body.dark canvas {
      background-color: #1f1f1f;
    }
    
    .buttons {
      display: flex;
      flex-wrap: wrap;
      gap: 15px;
      margin-top: 30px;
      margin-bottom: 25px;
    }
    
    .dark-mode-btn {
      position: fixed;
      bottom: 30px;
      right: 30px;
      z-index: 999;
      box-shadow: 0 4px 15px rgba(0,0,0,0.2);
      padding: 14px 18px;
      font-size: 17px;
      border-radius: 50%;
      width: 60px;
      height: 60px;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    
    .leyenda {
      font-size: 15px;
      font-style: italic;
      margin-top: 30px;
      margin-bottom: 10px;
      color: #666;
      text-align: center;
      padding: 15px;
      background: rgba(0,0,0,0.03);
      border-radius: var(--border-radius);
    }
    
    body.dark .leyenda {
      color: #aaa;
      background: rgba(255,255,255,0.05);
    }
    
    .tabla-container {
      overflow-x: auto;
      margin-top: 20px;
      border-radius: var(--border-radius);
      box-shadow: var(--sombra);
      margin-bottom: 35px;
      max-height: 500px;
      overflow-y: auto;
    }
    
    .tabla-titulo {
      background-color: var(--primario);
      color: white;
      padding: 16px;
      border-radius: var(--border-radius) var(--border-radius) 0 0;
      font-weight: bold;
      font-size: 17px;
      position: sticky;
      top: 0;
      z-index: 10;
    }
    
    .tabla-titulo.comparativa {
      background-color: var(--primario2);
    }
    
    .separador {
      height: 2px;
      background: linear-gradient(90deg, transparent, var(--primario), transparent);
      margin: 30px 0;
      border: none;
    }
    
    body.dark .separador {
      background: linear-gradient(90deg, transparent, var(--primario2), transparent);
    }
    
    .resumen-comparativo {
      margin-top: 30px;
      padding: 20px;
      background: linear-gradient(135deg, rgba(43, 103, 119, 0.1), rgba(125, 43, 119, 0.1));
      border-radius: var(--border-radius);
      font-size: 16px;
      line-height: 1.8;
      box-shadow: var(--sombra);
    }
    
    body.dark .resumen-comparativo {
      background: linear-gradient(135deg, rgba(43, 103, 119, 0.2), rgba(125, 43, 119, 0.2));
    }
    
    .diferencia {
      font-weight: bold;
      font-size: 18px;
      margin-top: 15px;
      padding: 10px;
      border-radius: var(--border-radius);
      text-align: center;
    }
    
    .positivo {
      color: #28a745;
      background-color: rgba(40, 167, 69, 0.1);
    }
    
    .negativo {
      color: #dc3545;
      background-color: rgba(220, 53, 69, 0.1);
    }
    
    .neutral {
      color: #6c757d;
      background-color: rgba(108, 117, 125, 0.1);
    }
    
    @media (max-width: 1200px) {
      .contenedor-principal {
        flex-direction: column;
      }
      
      .formulario-container,
      .resultados-container {
        min-width: 100%;
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
      
      .boton-exportar,
      .boton-limpiar {
        width: 100%;
        margin-left: 0;
        margin-top: 10px;
      }
    }
    
    /* Animaciones */
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }
    
    .fade-in {
      animation: fadeIn 0.5s ease-out forwards;
    }
    
    /* Scroll personalizado */
    ::-webkit-scrollbar {
      width: 10px;
      height: 10px;
    }
    
    ::-webkit-scrollbar-track {
      background: #f1f1f1;
      border-radius: 10px;
    }
    
    ::-webkit-scrollbar-thumb {
      background: var(--primario);
      border-radius: 10px;
    }
    
    ::-webkit-scrollbar-thumb:hover {
      background: var(--hover);
    }
    
    body.dark ::-webkit-scrollbar-track {
      background: #2a2a2a;
    }
    
    body.dark ::-webkit-scrollbar-thumb {
      background: var(--primario2);
    }
    
    body.dark ::-webkit-scrollbar-thumb:hover {
      background: #6a2465;
    }
    
    /* Tooltips */
    [data-tooltip] {
      position: relative;
      cursor: help;
    }
    
    [data-tooltip]:hover::after {
      content: attr(data-tooltip);
      position: absolute;
      bottom: 100%;
      left: 50%;
      transform: translateX(-50%);
      background: #333;
      color: white;
      padding: 8px 12px;
      border-radius: 4px;
      font-size: 14px;
      font-weight: normal;
      white-space: nowrap;
      z-index: 100;
      box-shadow: 0 2px 5px rgba(0,0,0,0.2);
    }
    
    body.dark [data-tooltip]:hover::after {
      background: #555;
    }
  </style>
</head>
<body>
  <div id="portada">
    <img src="https://raw.githubusercontent.com/JohanMoran/Proyeccion-de-Inversiones/main/ROBPAIERO_TUASESORDECONFIANZA.PNG" 
         alt="Calculadora de Inversión"
         style="width: 100%; max-width: 900px; height: auto; border-radius: 8px;">
  </div>
  <button class="dark-mode-btn" onclick="toggleDarkMode()" data-tooltip="Cambiar tema">🌙</button>

  <div class="contenedor-principal">
    <!-- FORMULARIO AMPLIADO -->
    <div class="formulario-container">
      <div class="seccion-inversion">
        <h2 class="titulo-seccion"><i>📈</i> Inversión Principal</h2>
        
        <label for="capitalInicial">MONTO INICIAL:</label>
        <div class="input-container">
          <input type="number" id="capitalInicial" placeholder="Ej: 10000" />
          <span>Capital inicial para comenzar tu inversión principal</span>
        </div>

        <label for="tasa">TASA ANUAL (%):</label>
        <div class="input-container">
          <input type="number" id="tasa" step="0.01" placeholder="Ej: 10.5" />
          <span>Tasa de interés anual esperada para la inversión principal</span>
        </div>

        <label for="plazo">PLAZO (MESES):</label>
        <div class="input-container">
          <input type="number" id="plazo" min="1" placeholder="Ej: 24" />
          <span>Número de meses que mantendrás la inversión principal</span>
        </div>

        <label for="aportacion">APORTACIÓN MENSUAL:</label>
        <div class="input-container">
          <input type="number" id="aportacion" placeholder="Ej: 2000" />
          <span>Cantidad que agregarás cada mes a la inversión principal</span>
        </div>

        <label for="fechaInicio">FECHA DE INICIO:</label>
        <div class="input-container">
          <input type="date" id="fechaInicio" />
          <span>Fecha en que comenzará tu inversión principal</span>
        </div>

        <label for="capitalObjetivo">CAPITAL OBJETIVO (OPCIONAL):</label>
        <div class="input-container">
          <input type="number" id="capitalObjetivo" placeholder="Ej: 500000" />
          <span>Si tienes una meta financiera específica, ingrésala aquí</span>
        </div>

        <button class="boton-calcular" onclick="calcularInversion(1)">
          <i>🧮</i> CALCULAR INVERSIÓN PRINCIPAL
        </button>
      </div>

      <div class="separador"></div>

      <div class="seccion-inversion">
        <h2 class="titulo-seccion comparativa"><i>📊</i> Inversión Comparativa</h2>
        
        <label for="capitalInicial2">MONTO INICIAL:</label>
        <div class="input-container">
          <input type="number" id="capitalInicial2" placeholder="Ej: 10000" />
          <span>Capital inicial para la inversión alternativa</span>
        </div>

        <label for="tasa2">TASA ANUAL (%):</label>
        <div class="input-container">
          <input type="number" id="tasa2" step="0.01" placeholder="Ej: 12.5" />
          <span>Tasa de interés anual para comparar con la inversión principal</span>
        </div>

        <label for="plazo2">PLAZO (MESES):</label>
        <div class="input-container">
          <input type="number" id="plazo2" min="1" placeholder="Ej: 24" />
          <span>Plazo en meses para la inversión comparativa</span>
        </div>

        <label for="aportacion2">APORTACIÓN MENSUAL:</label>
        <div class="input-container">
          <input type="number" id="aportacion2" placeholder="Ej: 2000" />
          <span>Aportación mensual para la inversión comparativa</span>
        </div>

        <label for="fechaInicio2">FECHA DE INICIO:</label>
        <div class="input-container">
          <input type="date" id="fechaInicio2" />
          <span>Fecha de inicio para la inversión comparativa</span>
        </div>

        <label for="capitalObjetivo2">CAPITAL OBJETIVO (OPCIONAL):</label>
        <div class="input-container">
          <input type="number" id="capitalObjetivo2" placeholder="Ej: 500000" />
          <span>Objetivo financiero para la inversión comparativa</span>
        </div>

        <button class="boton-comparar" onclick="calcularInversion(2)">
          <i>🔍</i> CALCULAR INVERSIÓN COMPARATIVA
        </button>
      </div>
    </div>

    <!-- RESULTADOS MEJORADOS -->
    <div class="resultados-container">
      <div id="resultado1" class="fade-in" style="display: none;"></div>
      <div id="resultado2" class="fade-in" style="display: none;"></div>

      <div id="resumenComparativo" class="resumen-comparativo fade-in" style="display: none;">
        <h3 style="margin-top: 0; color: var(--primario);">Comparación Detallada</h3>
        <div id="comparacionDetallada"></div>
        <div id="diferenciaFinal" class="diferencia"></div>
      </div>

      <canvas id="graficaComparativa" height="400" style="display: none;"></canvas>

      <div class="tabla-titulo fade-in" id="tablaTitulo1" style="display: none;">
        <i>📋</i> DETALLE COMPLETO - INVERSIÓN PRINCIPAL
      </div>
      <div class="tabla-container">
        <table id="tablaResultados1" style="display: none;">
          <thead>
            <tr>
              <th>MES</th>
              <th>FECHA</th>
              <th>APORTACIÓN ($)</th>
              <th>INTERÉS ($)</th>
              <th>TOTAL ($)</th>
            </tr>
          </thead>
          <tbody></tbody>
        </table>
      </div>

      <div class="tabla-titulo comparativa fade-in" id="tablaTitulo2" style="display: none;">
        <i>📋</i> DETALLE COMPLETO - INVERSIÓN COMPARATIVA
      </div>
      <div class="tabla-container">
        <table id="tablaResultados2" style="display: none;">
          <thead>
            <tr>
              <th>MES</th>
              <th>FECHA</th>
              <th>APORTACIÓN ($)</th>
              <th>INTERÉS ($)</th>
              <th>TOTAL ($)</th>
            </tr>
          </thead>
          <tbody></tbody>
        </table>
      </div>

      <div class="buttons">
        <button class="boton-exportar" onclick="descargarPDF()">
          <i>📄</i> DESCARGAR PDF COMPLETO
        </button>
        <button class="boton-exportar" onclick="descargarCSV(1)">
          <i>💾</i> EXPORTAR EXCEL (PRINCIPAL)
        </button>
        <button class="boton-exportar" onclick="descargarCSV(2)">
          <i>💾</i> EXPORTAR EXCEL (COMPARATIVA)
        </button>
        <button class="boton-limpiar" onclick="limpiarCalculadora()">
          <i>🔄</i> LIMPIAR CALCULADORA
        </button>
      </div>
    </div>
  </div>

  <div class="leyenda">
    Calculadora de Interés Compuesto con Aportación Mensual | Versión Mejorada 2024 | Herramienta con fines informativos y educativos
  </div>

  <script>
    // Variables globales mejor organizadas
    const state = {
      inversiones: {
        1: { datos: [], totalAportaciones: 0, totalInteres: 0, capital: 0 },
        2: { datos: [], totalAportaciones: 0, totalInteres: 0, capital: 0 }
      },
      graficos: {
        comparativo: null,
        individuales: { 1: null, 2: null }
      },
      config: {
        decimales: 2,
        maxMeses: 600
      }
    };

    // Función principal para calcular inversiones
    function calcularInversion(inversionNum) {
      const prefix = inversionNum === 1 ? '' : '2';
      const capitalInicial = parseFloat(document.getElementById(`capitalInicial${prefix}`).value) || 0;
      const tasa = parseFloat(document.getElementById(`tasa${prefix}`).value) || 0;
      const plazo = parseInt(document.getElementById(`plazo${prefix}`).value) || 0;
      const aportacion = parseFloat(document.getElementById(`aportacion${prefix}`).value) || 0;
      const capitalObjetivo = parseFloat(document.getElementById(`capitalObjetivo${prefix}`).value) || null;
      const fechaInicio = new Date(document.getElementById(`fechaInicio${prefix}`).value);

      // Validaciones mejoradas
      if (plazo <= 0 || isNaN(plazo)) {
        mostrarError(`Por favor, ingresa un plazo válido para la inversión ${inversionNum === 1 ? 'principal' : 'comparativa'}.`);
        return;
      }

      if (tasa <= 0 || isNaN(tasa)) {
        mostrarError(`Por favor, ingresa una tasa anual válida para la inversión ${inversionNum === 1 ? 'principal' : 'comparativa'}.`);
        return;
      }

      // Reiniciar datos de la inversión
      state.inversiones[inversionNum] = {
        datos: [],
        totalAportaciones: 0,
        totalInteres: 0,
        capital: capitalInicial
      };

      const tasaMensual = tasa / 12 / 100;
      const tabla = document.querySelector(`#tablaResultados${inversionNum} tbody`);
      tabla.innerHTML = '';
      
      let meses = plazo;
      let cumpleObjetivo = false;

      // Cálculo con objetivo si está definido
      if (capitalObjetivo && capitalObjetivo > 0) {
        let capitalTemp = capitalInicial;
        for (let i = 1; i <= state.config.maxMeses; i++) {
          const interes = capitalTemp * tasaMensual;
          capitalTemp += interes + aportacion;
          if (capitalTemp >= capitalObjetivo) {
            meses = i;
            cumpleObjetivo = true;
            break;
          }
        }
        state.inversiones[inversionNum].capital = capitalInicial; // Reset para cálculo detallado
      }

      // Cálculo mes a mes
      for (let i = 1; i <= meses; i++) {
        const interes = state.inversiones[inversionNum].capital * tasaMensual;
        state.inversiones[inversionNum].totalInteres += interes;
        state.inversiones[inversionNum].capital += interes + aportacion;
        state.inversiones[inversionNum].totalAportaciones += aportacion;

        const fecha = new Date(fechaInicio);
        fecha.setMonth(fecha.getMonth() + i);

        // Guardar datos para tabla
        tabla.innerHTML += `
          <tr>
            <td>${i}</td>
            <td>${fecha.toLocaleDateString('es-MX', { day: '2-digit', month: '2-digit', year: 'numeric' })}</td>
            <td>${formatCurrency(aportacion)}</td>
            <td>${formatCurrency(interes)}</td>
            <td>${formatCurrency(state.inversiones[inversionNum].capital)}</td>
          </tr>
        `;

        // Guardar datos para gráficos
        state.inversiones[inversionNum].datos.push({
          mes: i,
          total: state.inversiones[inversionNum].capital,
          interes: interes,
          aportacion: aportacion
        });
      }

      // Mostrar resultados
      mostrarResultados(inversionNum, capitalInicial, tasa, meses, aportacion, cumpleObjetivo);

      // Mostrar elementos ocultos
      document.getElementById(`resultado${inversionNum}`).style.display = 'block';
      document.getElementById(`tablaResultados${inversionNum}`).style.display = 'table';
      document.getElementById(`tablaTitulo${inversionNum}`).style.display = 'block';

      // Generar gráficos si ambas inversiones están calculadas
      if (state.inversiones[1].datos.length > 0 && state.inversiones[2].datos.length > 0) {
        generarGraficoComparativo();
        mostrarComparacion();
      }
    }

    // Función para mostrar resultados formateados
    function mostrarResultados(inversionNum, capitalInicial, tasa, meses, aportacion, cumpleObjetivo) {
      const inv = state.inversiones[inversionNum];
      const color = inversionNum === 1 ? 'var(--primario)' : 'var(--primario2)';
      const icono = inversionNum === 1 ? '📊' : '📈';
      
      document.getElementById(`resultado${inversionNum}`).innerHTML = `
        <div style="color: ${color}; font-size: 18px; margin-bottom: 15px;">
          <strong>${icono} RESUMEN INVERSIÓN ${inversionNum === 1 ? 'PRINCIPAL' : 'COMPARATIVA'}</strong>
        </div>
        <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px;">
          <div>
            <strong>• Capital inicial:</strong><br>
            ${formatCurrency(capitalInicial)}
          </div>
          <div>
            <strong>• Tasa anual:</strong><br>
            ${tasa.toFixed(2)}%
          </div>
          <div>
            <strong>• Plazo:</strong><br>
            ${meses} meses (${(meses/12).toFixed(1)} años)
          </div>
          <div>
            <strong>• Aportación mensual:</strong><br>
            ${formatCurrency(aportacion)}
          </div>
          <div>
            <strong>• Total aportado:</strong><br>
            ${formatCurrency(inv.totalAportaciones)}
          </div>
          <div>
            <strong>• Interés generado:</strong><br>
            ${formatCurrency(inv.totalInteres)}
          </div>
        </div>
        <div style="margin-top: 20px; padding: 15px; background: rgba(255,255,255,0.3); border-radius: var(--border-radius); text-align: center;">
          <strong style="font-size: 18px;">TOTAL FINAL: ${formatCurrency(inv.capital)}</strong>
          ${cumpleObjetivo ? `<div style="margin-top: 10px; color: var(--verde);">🎉 ¡OBJETIVO ALCANZADO EN ${meses} MESES!</div>` : ''}
        </div>
      `;
    }

    // Función para mostrar comparación entre inversiones
    function mostrarComparacion() {
      const inv1 = state.inversiones[1];
      const inv2 = state.inversiones[2];
      
      const diferencia = inv2.capital - inv1.capital;
      const porcentaje = (Math.abs(diferencia) / inv1.capital * 100).toFixed(2);
      
      let diferenciaHtml = '';
      if (diferencia > 0) {
        diferenciaHtml = `
          La inversión comparativa genera <span style="color: var(--verde);">${formatCurrency(diferencia)} más</span> 
          (${porcentaje}% mayor) que la inversión principal.
        `;
      } else if (diferencia < 0) {
        diferenciaHtml = `
          La inversión principal genera <span style="color: var(--rojo);">${formatCurrency(Math.abs(diferencia))} más</span> 
          (${porcentaje}% mayor) que la inversión comparativa.
        `;
      } else {
        diferenciaHtml = `Ambas inversiones generan el mismo resultado final.`;
      }
      
      document.getElementById('comparacionDetallada').innerHTML = `
        <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 20px;">
          <div>
            <strong style="color: var(--primario);">Inversión Principal</strong><br>
            • Total final: ${formatCurrency(inv1.capital)}<br>
            • Interés generado: ${formatCurrency(inv1.totalInteres)}<br>
            • Total aportado: ${formatCurrency(inv1.totalAportaciones)}
          </div>
          <div>
            <strong style="color: var(--primario2);">Inversión Comparativa</strong><br>
            • Total final: ${formatCurrency(inv2.capital)}<br>
            • Interés generado: ${formatCurrency(inv2.totalInteres)}<br>
            • Total aportado: ${formatCurrency(inv2.totalAportaciones)}
          </div>
        </div>
      `;
      
      document.getElementById('diferenciaFinal').innerHTML = diferenciaHtml;
      document.getElementById('diferenciaFinal').className = `diferencia ${
        diferencia > 0 ? 'positivo' : diferencia < 0 ? 'negativo' : 'neutral'
      }`;
      
      document.getElementById('resumenComparativo').style.display = 'block';
    }

    // Función para generar gráfico comparativo
    function generarGraficoComparativo() {
      const ctx = document.getElementById('graficaComparativa').getContext('2d');
      document.getElementById('graficaComparativa').style.display = 'block';
      
      if (state.graficos.comparativo) {
        state.graficos.comparativo.destroy();
      }

      const inv1 = state.inversiones[1];
      const inv2 = state.inversiones[2];
      
      // Determinar el máximo de meses entre ambas inversiones
      const maxMeses = Math.max(
        inv1.datos.length > 0 ? inv1.datos[inv1.datos.length - 1].mes : 0,
        inv2.datos.length > 0 ? inv2.datos[inv2.datos.length - 1].mes : 0
      );

      // Crear etiquetas para todos los meses
      const labels = Array.from({ length: maxMeses }, (_, i) => `Mes ${i + 1}`);

      // Mapear datos para que coincidan con todas las etiquetas
      const data1 = labels.map((_, i) => {
        const mes = i + 1;
        const dato = inv1.datos.find(d => d.mes === mes);
        return dato ? dato.total : null;
      });

      const data2 = labels.map((_, i) => {
        const mes = i + 1;
        const dato = inv2.datos.find(d => d.mes === mes);
        return dato ? dato.total : null;
      });

      state.graficos.comparativo = new Chart(ctx, {
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
              borderWidth: 3,
              pointRadius: 0
            },
            {
              label: 'Inversión Comparativa',
              data: data2,
              borderColor: '#7d2b77',
              backgroundColor: 'rgba(125, 43, 119, 0.1)',
              fill: true,
              tension: 0.3,
              borderWidth: 3,
              pointRadius: 0
            }
          ]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: {
              position: 'top',
              labels: {
                font: {
                  size: 14,
                  weight: 'bold'
                },
                padding: 20
              }
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
                callback: (value) => formatCurrency(value),
                font: {
                  size: 12
                }
              },
              grid: {
                color: 'rgba(0, 0, 0, 0.1)'
              }
            },
            x: {
              grid: {
                display: false
              },
              ticks: {
                font: {
                  size: 12
                },
                maxTicksLimit: 12
              }
            }
          },
          interaction: {
            intersect: false,
            mode: 'index'
          },
          animation: {
            duration: 1000
          }
        }
      });
    }

    // Función para formatear moneda
    function formatCurrency(value) {
      return new Intl.NumberFormat('es-MX', { 
        style: 'currency', 
        currency: 'MXN',
        minimumFractionDigits: state.config.decimales,
        maximumFractionDigits: state.config.decimales
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
      
      // Configuración inicial
      doc.setFont('helvetica');
      
      // Título
      doc.setFontSize(22);
      doc.setTextColor(43, 103, 119);
      doc.setFont(undefined, 'bold');
      doc.text("Reporte Comparativo de Inversiones", 105, 20, { align: 'center' });
      
      // Línea divisoria
      doc.setDrawColor(43, 103, 119);
      doc.setLineWidth(0.8);
      doc.line(20, 25, 190, 25);
      
      // Datos de inversión principal
      doc.setFontSize(12);
      doc.setTextColor(0, 0, 0);
      doc.setFont(undefined, 'normal');
      
      const inv1 = state.inversiones[1];
      const inv2 = state.inversiones[2];
      
      // Resumen ejecutivo
      doc.setFontSize(14);
      doc.setTextColor(43, 103, 119);
      doc.text("Resumen Ejecutivo", 20, 35);
      doc.setFontSize(12);
      doc.setTextColor(0, 0, 0);
      
      const diferencia = inv2.capital - inv1.capital;
      const textoDiferencia = diferencia > 0 
        ? `La inversión comparativa supera a la principal por ${formatCurrency(diferencia)}`
        : `La inversión principal supera a la comparativa por ${formatCurrency(Math.abs(diferencia))}`;
      
      doc.text(textoDiferencia, 20, 42);
      
      // Datos de ambas inversiones
      doc.setFontSize(14);
      doc.setTextColor(43, 103, 119);
      doc.text("Datos de las Inversiones", 20, 55);
      doc.setFontSize(12);
      doc.setTextColor(0, 0, 0);
      
      doc.text("Inversión Principal:", 20, 63);
      doc.text(`• Capital inicial: ${formatCurrency(document.getElementById('capitalInicial').value || 0)}`, 25, 70);
      doc.text(`• Tasa anual: ${document.getElementById('tasa').value || 0}%`, 25, 77);
      doc.text(`• Plazo: ${document.getElementById('plazo').value || 0} meses`, 25, 84);
      doc.text(`• Aportación mensual: ${formatCurrency(document.getElementById('aportacion').value || 0)}`, 25, 91);
      
      doc.text("Inversión Comparativa:", 110, 63);
      doc.text(`• Capital inicial: ${formatCurrency(document.getElementById('capitalInicial2').value || 0)}`, 115, 70);
      doc.text(`• Tasa anual: ${document.getElementById('tasa2').value || 0}%`, 115, 77);
      doc.text(`• Plazo: ${document.getElementById('plazo2').value || 0} meses`, 115, 84);
      doc.text(`• Aportación mensual: ${formatCurrency(document.getElementById('aportacion2').value || 0)}`, 115, 91);
      
      // Resultados finales
      doc.setFontSize(14);
      doc.setTextColor(43, 103, 119);
      doc.text("Resultados Finales", 20, 105);
      doc.setFontSize(12);
      doc.setTextColor(0, 0, 0);
      
      doc.text("Inversión Principal:", 20, 113);
      doc.text(`• Total final: ${formatCurrency(inv1.capital)}`, 25, 120);
      doc.text(`• Interés generado: ${formatCurrency(inv1.totalInteres)}`, 25, 127);
      doc.text(`• Total aportado: ${formatCurrency(inv1.totalAportaciones)}`, 25, 134);
      
      doc.text("Inversión Comparativa:", 110, 113);
      doc.text(`• Total final: ${formatCurrency(inv2.capital)}`, 115, 120);
      doc.text(`• Interés generado: ${formatCurrency(inv2.totalInteres)}`, 115, 127);
      doc.text(`• Total aportado: ${formatCurrency(inv2.totalAportaciones)}`, 115, 134);
      
      // Gráfico comparativo
      const canvas = document.getElementById('graficaComparativa');
      if (canvas) {
        const imgData = canvas.toDataURL('image/png');
        doc.addImage(imgData, 'PNG', 20, 140, 170, 80);
      }
      
      // Tablas (páginas adicionales)
      doc.addPage();
      doc.setFontSize(16);
      doc.setTextColor(43, 103, 119);
      doc.text("Detalle de Inversión Principal", 105, 20, { align: 'center' });
      
      doc.autoTable({
        html: '#tablaResultados1',
        startY: 30,
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
        }
      });
      
      doc.addPage();
      doc.setFontSize(16);
      doc.setTextColor(125, 43, 119);
      doc.text("Detalle de Inversión Comparativa", 105, 20, { align: 'center' });
      
      doc.autoTable({
        html: '#tablaResultados2',
        startY: 30,
        theme: 'grid',
        headStyles: {
          fillColor: [125, 43, 119],
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
        }
      });
      
      // Pie de página
      doc.setPage(1);
      doc.setFontSize(10);
      doc.setTextColor(100);
      doc.text("© Calculadora de Inversión - " + new Date().toLocaleDateString('es-MX'), 105, 285, { align: 'center' });
      
      doc.save('reporte_comparativo_inversiones.pdf');
    }

    // Función para descargar CSV
    function descargarCSV(inversionNum) {
      const inv = state.inversiones[inversionNum];
      
      if (inv.datos.length === 0) {
        mostrarError(`Primero debes calcular la inversión ${inversionNum === 1 ? 'principal' : 'comparativa'}.`);
        return;
      }
      
      let csv = "Mes,Fecha,Aportación ($),Interés ($),Total ($)\n";
      const rows = document.querySelectorAll(`#tablaResultados${inversionNum} tbody tr`);
      
      rows.forEach(row => {
        const cells = row.querySelectorAll('td');
        const mes = cells[0].textContent;
        const fecha = cells[1].textContent;
        const aportacion = cells[2].textContent.replace('$', '').replace(',', '');
        const interes = cells[3].textContent.replace('$', '').replace(',', '');
        const total = cells[4].textContent.replace('$', '').replace(',', '');
        
        csv += `"${mes}","${fecha}","${aportacion}","${interes}","${total}"\n`;
      });
      
      const blob = new Blob(["\uFEFF" + csv], { type: 'text/csv;charset=utf-8;' });
      const link = document.createElement('a');
      link.href = URL.createObjectURL(blob);
      link.download = `inversion_${inversionNum === 1 ? 'principal' : 'comparativa'}_${new Date().toISOString().slice(0,10)}.csv`;
      link.click();
    }

    // Función para limpiar la calculadora
    function limpiarCalculadora() {
      // Limpiar inputs
      document.getElementById('capitalInicial').value = '';
      document.getElementById('tasa').value = '';
      document.getElementById('plazo').value = '';
      document.getElementById('aportacion').value = '';
      document.getElementById('fechaInicio').value = '';
      document.getElementById('capitalObjetivo').value = '';
      
      document.getElementById('capitalInicial2').value = '';
      document.getElementById('tasa2').value = '';
      document.getElementById('plazo2').value = '';
      document.getElementById('aportacion2').value = '';
      document.getElementById('fechaInicio2').value = '';
      document.getElementById('capitalObjetivo2').value = '';
      
      // Limpiar resultados
      document.getElementById('resultado1').style.display = 'none';
      document.getElementById('resultado2').style.display = 'none';
      document.getElementById('resumenComparativo').style.display = 'none';
      document.getElementById('graficaComparativa').style.display = 'none';
      
      // Limpiar tablas
      document.querySelector('#tablaResultados1 tbody').innerHTML = '';
      document.querySelector('#tablaResultados2 tbody').innerHTML = '';
      document.getElementById('tablaResultados1').style.display = 'none';
      document.getElementById('tablaResultados2').style.display = 'none';
      document.getElementById('tablaTitulo1').style.display = 'none';
      document.getElementById('tablaTitulo2').style.display = 'none';
      
      // Reiniciar estado
      state.inversiones = {
        1: { datos: [], totalAportaciones: 0, totalInteres: 0, capital: 0 },
        2: { datos: [], totalAportaciones: 0, totalInteres: 0, capital: 0 }
      };
      
      // Destruir gráficos
      if (state.graficos.comparativo) {
        state.graficos.comparativo.destroy();
        state.graficos.comparativo = null;
      }
    }

    // Función para mostrar errores
    function mostrarError(mensaje) {
      alert(mensaje);
    }

    // Función para cambiar modo oscuro/claro
    function toggleDarkMode() {
      document.body.classList.toggle("dark");
      
      // Actualizar gráficos si existen
      if (state.graficos.comparativo) {
        state.graficos.comparativo.update();
      }
      
      // Cambiar icono del botón
      const boton = document.querySelector('.dark-mode-btn');
      if (document.body.classList.contains('dark')) {
        boton.innerHTML = '☀️';
        boton.setAttribute('data-tooltip', 'Modo claro');
      } else {
        boton.innerHTML = '🌙';
        boton.setAttribute('data-tooltip', 'Modo oscuro');
      }
    }

    // Inicialización
    document.addEventListener('DOMContentLoaded', function() {
      // Establecer fecha mínima para inputs de fecha
      const hoy = new Date();
      const fechaHoy = hoy.toISOString().split('T')[0];
      document.getElementById('fechaInicio').min = fechaHoy;
      document.getElementById('fechaInicio2').min = fechaHoy;
      
      // Establecer fecha por defecto a hoy
      document.getElementById('fechaInicio').value = fechaHoy;
      document.getElementById('fechaInicio2').value = fechaHoy;
      
      // Tooltips para iconos
      const tooltips = {
        'capitalInicial': 'Ingresa el monto con el que comenzarás tu inversión',
        'tasa': 'Porcentaje de rendimiento anual esperado',
        'plazo': 'Tiempo en meses que mantendrás la inversión',
        'aportacion': 'Cantidad adicional que agregarás cada mes',
        'fechaInicio': 'Fecha en que comenzará tu inversión',
        'capitalObjetivo': 'Opcional: Meta financiera que deseas alcanzar'
      };
      
      Object.keys(tooltips).forEach(id => {
        const input = document.getElementById(id);
        const input2 = document.getElementById(id + '2');
        if (input) input.setAttribute('data-tooltip', tooltips[id]);
        if (input2) input2.setAttribute('data-tooltip', tooltips[id]);
      });
    });
  </script>
</body>
</html>
