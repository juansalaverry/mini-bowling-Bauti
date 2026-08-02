<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mini Bowling de Bauti</title>
    <style>
        :root {
            --primary: #2563eb;
            --bg: #f8fafc;
            --card-bg: #ffffff;
            --text: #1e293b;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background-color: var(--bg);
            color: var(--text);
            margin: 0;
            padding: 16px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .container {
            width: 100%;
            max-width: 400px;
        }

        h1 {
            text-align: center;
            font-size: 1.5rem;
            margin-bottom: 20px;
            color: var(--primary);
        }

        .card {
            background: var(--card-bg);
            padding: 16px;
            border-radius: 12px;
            box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
            margin-bottom: 16px;
        }

        .input-group {
            display: flex;
            gap: 8px;
            margin-bottom: 12px;
        }

        input {
            flex: 1;
            padding: 12px;
            font-size: 1rem;
            border: 1px solid #cbd5e1;
            border-radius: 8px;
            outline: none;
        }

        button {
            background-color: var(--primary);
            color: white;
            border: none;
            padding: 12px 20px;
            font-size: 1rem;
            border-radius: 8px;
            font-weight: bold;
            cursor: pointer;
        }

        button:active {
            transform: scale(0.98);
        }

        .btn-danger {
            background-color: #ef4444;
            width: 100%;
            margin-top: 8px;
        }

        .score-list {
            list-style: none;
            padding: 0;
            margin: 0;
            max-height: 200px;
            overflow-y: auto;
        }

        .score-list li {
            display: flex;
            justify-content: space-between;
            padding: 8px 0;
            border-bottom: 1px solid #f1f5f9;
        }

        .total-box {
            text-align: center;
            background: #dbeafe;
            padding: 16px;
            border-radius: 12px;
            font-size: 1.25rem;
            font-weight: bold;
            color: #1e40af;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>🎳 Mini Bowling de Bauti</h1>

        <div class="card">
            <div class="input-group">
                <input type="number" id="pinosInput" placeholder="Pinos tirados" min="0" max="10" inputmode="numeric">
                <button onclick="agregarTiro()">Anotar</button>
            </div>
        </div>

        <div class="card">
            <h3>Historial de Tiros</h3>
            <ul id="listaTiros" class="score-list">
                <!-- Los tiros se cargan acá dinámicamente -->
            </ul>
        </div>

        <div class="total-box">
            Total acumulado: <span id="totalPinos">0</span>
        </div>

        <button class="btn-danger" onclick="reiniciarJuego()">Nueva Partida</button>
    </div>

    <script>
        let tiros = [];

        function agregarTiro() {
            const input = document.getElementById('pinosInput');
            const valor = parseInt(input.value);

            if (isNaN(valor) || valor < 0 || valor > 10) {
                alert("Por favor ingresa un número válido entre 0 y 10.");
                return;
            }

            tiros.push(valor);
            input.value = '';
            input.focus();
            actualizarUI();
        }

        function actualizarUI() {
            const lista = document.getElementById('listaTiros');
            const totalSpan = document.getElementById('totalPinos');
            
            lista.innerHTML = '';
            let total = 0;

            tiros.forEach((pinos, index) => {
                total += pinos;
                const li = document.createElement('li');
                li.innerHTML = `<span>Tiro ${index + 1}</span> <strong>${pinos} pinos</strong>`;
                lista.appendChild(li);
            });

            totalSpan.textContent = total;
        }

        function reiniciarJuego() {
            if (confirm("¿Querés empezar una nueva partida?")) {
                tiros = [];
                actualizarUI();
            }
        }
    </script>
</body>
</html>
