<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Calculadora de Inversión</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 800px; margin: auto; padding: 20px; }
    label, input { display: block; margin-top: 10px; }
    input { padding: 6px; width: 100%; }
    table { width: 100%; border-collapse: collapse; margin-top: 20px; }
    th, td { border: 1px solid #ccc; padding: 8px; text-align: center; }
    th { background-color: #f0f0f0; }
    .result { margin-top: 20px; font-weight: bold; }
  </style>
</head>
<body>
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

  <button onclick="calcular()">Calcular</button>

  <div class="result" id="resultado"></div>
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
    function calcular() {
      const capitalInicial = parseFloat(document.getElementById('capitalInicial').value) || 0;
      const tasa = parseFloat(document.getElementById('tasa').value) || 0;
      const plazo = parseInt(document.getElementById('plazo').value) || 0;
      const aportacion = parseFloat(document.getElementById('aportacion').value) || 0;
      const fechaInicio = new Date(document.getElementById('fechaInicio').value);

      let capital = capitalInicial;
      let totalInteres = 0;
      const mensual = tasa / 12 / 100;
      const tabla = document.querySelector('#tablaResultados tbody');
      tabla.innerHTML = '';

      for (let i = 1; i <= plazo; i++) {
        const interes = capital * mensual;
        totalInteres += interes;
        capital += interes + aportacion;

        const fecha = new Date(fechaInicio);
        fecha.setMonth(fecha.getMonth() + i);
        const fila = `<tr>
          <td>${i}</td>
          <td>${fecha.toLocaleDateString()}</td>
          <td>$${aportacion.toFixed(2)}</td>
          <td>$${interes.toFixed(2)}</td>
          <td>$${capital.toFixed(2)}</td>
        </tr>`;
        tabla.innerHTML += fila;
      }

      document.getElementById('resultado').innerText = `Monto final estimado: $${capital.toFixed(2)} (Interés generado: $${totalInteres.toFixed(2)})`;
      document.getElementById('tablaResultados').style.display = 'table';
    }
  </script>
</body>
</html>
