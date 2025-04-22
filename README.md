<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Calculadora de Inversión</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.28/jspdf.plugin.autotable.min.js"></script>
  <!-- Fuentes elegantes -->
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Roboto', sans-serif; /* Fuente más profesional */
      background-color: #f5f5f5; /* Fondo más claro y profesional */
      padding: 40px 20px;
      max-width: 900px;
      margin: auto;
      color: #333;
    }

    h1 {
      text-align: center;
      color: #2a4d63; /* Azul más oscuro para un toque ejecutivo */
      font-size: 32px;
      margin-bottom: 30px;
    }

    label {
      margin-top: 15px;
      display: block;
      font-weight: 500;
      color: #555;
    }

    input {
      padding: 12px;
      border: 1px solid #ddd;
      width: 100%;
      border-radius: 8px;
      margin-top: 8px;
      font-size: 14px;
      color: #333;
    }

    button {
      margin-top: 25px;
      padding: 12px 18px;
      background-color: #007bff; /* Azul profesional para el botón Calcular */
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-weight: 600;
      font-size: 14px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Suave sombra para darle profundidad */
    }

    button:hover {
      background-color: #0056b3; /* Hover con un azul más oscuro */
      box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15); /* Sombra más pronunciada en hover */
    }

    .buttons {
      display: flex;
      gap: 15px;
      margin-top: 20px;
      justify-content: center;
    }

    .result {
      margin-top: 25px;
      font-size: 18px;
      font-weight: 500;
      color: #11698e;
    }

    table {
      width: 100%;
      margin-top: 30px;
      border-collapse: collapse;
      background-color: #fff;
      border-radius: 8px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05); /* Suave sombra para las tablas */
    }

    th, td {
      padding: 12px;
      text-align: center;
      border: 1px solid #ddd;
    }

    th {
      background-color: #007bff;
      color: white;
      font-weight: 600;
    }

    canvas {
      margin-top: 30px;
      border-radius: 8px;
    }

    .buttons button {
      min-width: 150px;
    }
  </style>
</head>
