<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mini Bowling - Tablero</title>
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
            padding: 12px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .container {
            width: 100%;
            max-width: 600px;
        }

        h1 {
            text-align: center;
            font-size: 1.4rem;
            margin-bottom: 16px;
            color: var(--primary);
        }

        .card {
            background: var(--card-bg);
            padding: 16px;
            border-radius: 12px;
            box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
            margin-bottom: 16px;
        }

        .turno-indicator {
            font-size: 1.1rem;
            font-weight: bold;
            margin-bottom: 10px;
            text-align: center;
            color: #1e40af;
        }

        .input-group {
            display: flex;
            gap: 8px;
        }

        input {
            flex: 1;
            padding: 12px;
            font-size: 1rem;
            border: 1px solid #cbd5e1;
            border-radius: 8px;
            outline: none;
        }

        button.action-btn {
            background-color: var(--primary);
            color: white;
            border: none;
            padding: 12px 20px;
            font-size: 1rem;
            border-radius: 8px;
            font-weight: bold;
            cursor: pointer;
        }

        .boards-container {
            display: flex;
            gap: 8px;
            overflow-x: auto;
            margin-bottom: 16px;
            padding-bottom: 4px;
        }

        .board {
            flex: 1;
            min-width: 130px;
            background: var(--card-bg);
            padding: 10px;
            border-radius: 12px;
            box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
            display: flex;
            flex-direction: column;
            border: 2px solid transparent;
        }

        .board.active-board {
            border-color: var(--primary);
            background: #f0fdf4;
        }

        .board h3 {
            font-size: 0.95rem;
            margin: 0 0 6px 0;
            text-align: center;
            color: var(--primary);
            word-break: break-word;
        }

        .score-list {
            list-style: none;
            padding: 0;
            margin: 0 0 8px 0;
            max-height: 140px;
            overflow-y: auto;
            flex: 1;
            font-size: 0.85rem;
        }

        .score-list li {
            display: flex;
            justify-content: space-between;
            padding: 3px 0;
            border-bottom: 1px solid #f1f5f9;
        }

        .total-mini {
            text-align: center;
            background: #dbeafe;
            padding: 6px;
            border-radius: 6px;
            font-size: 0.9rem;
            font-weight: bold;
            color: #1e40af;
        }

        .btn-secondary {
            background-color: #64748b;
            color: white;
            border: none;
            padding: 10px;
            border-radius: 8px;
            font-weight: bold;
            width: 100%;
            cursor: pointer;
            margin-bottom: 8px;
        }

        .btn-danger {
            background-color: #ef4444;
            color: white;
            border: none;
            padding: 10px;
            border-radius: 8px;
            font-weight: bold;
            width: 100%;
            cursor: pointer;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>🎳 Mini Bowling</h1>

        <div class="card">
            <div id="turnoText" class="turno-indicator">Turno de: -</div>
            <div class="input-group">
                <input type="number" id="pinosInput" placeholder="Pinos (0-10)" min="0" max="10" inputmode="numeric">
                <button class="action-btn" onclick="agregarTiro()">Anotar</button>
            </div>
        </div>

        <div id="boardsContainer" class="boards-container">
            <!-- Las columnas de los jugadores se generan acá solas -->
        </div>

        <button class="btn-secondary" onclick="configurarJugadores()">Cambiar Jugadores</button>
        <button class="btn-danger" onclick="reiniciarPartida()">Reiniciar Partida</button>
    </div>

    <script>
        let jugadores = [];
        let indiceTurno = 0;
        let tirosJugadores = {}; // Estructura: { "Bauti": [5, 2], "Juan": [3, 4] }

        function configurarJugadores() {
            let inputNombres = prompt("Ingresá los nombres de los jugadores separados por coma:", "Bauti, Juan");
            
            if (!inputNombres || inputNombres.trim() === "") {
                if (jugadores.length === 0) jugadores = ["Bauti", "Juan"];
                return;
            }

            jugadores = inputNombres.split(',').map(n => n.trim()).filter(n => n.length > 0);
            
            tirosJugadores = {};
            jugadores.forEach(j => {
                tirosJugadores[j] = [];
            });

            indiceTurno = 0;
            actualizarUI();
        }

        function agregarTiro() {
            const input = document.getElementById('pinosInput');
            const valor = parseInt(input.value);

            if (isNaN(valor) || valor < 0 || valor > 10) {
                alert("Por favor ingresa un número válido entre 0 y 10.");
                return;
            }

            const jugadorActual = jugadores[indiceTurno];
            tirosJugadores[jugadorActual].push(valor);

            // Pasa al siguiente jugador en ronda
            indiceTurno = (indiceTurno + 1) % jugadores.length;

            input.value = '';
            input.focus();
            actualizarUI();
        }

        function actualizarUI() {
            const container = document.getElementById('boardsContainer');
            const turnoText = document.getElementById('turnoText');
            
            if (jugadores.length > 0) {
                turnoText.textContent = `Turno de: ${jugadores[indiceTurno]}`;
            }

            container.innerHTML = '';

            jugadores.forEach((jugador, index) => {
                const tiros = tirosJugadores[jugador] || [];
                let total = tiros.reduce((a, b) => a + b, 0);
                const esTurnoActual = (index === indiceTurno);

                const boardDiv = document.createElement('div');
                boardDiv.className = `board ${esTurnoActual ? 'active-board' : ''}`;

                let listaHTML = '';
                tiros.forEach((p, i) => {
                    listaHTML += `<li><span>#${i+1}</span> <strong>${p}</strong></li>`;
                });

                boardDiv.innerHTML = `
                    <h3>${jugador}</h3>
                    <ul class="score-list">${listaHTML}</ul>
                    <div class="total-mini">Total: ${total}</div>
                `;

                container.appendChild(boardDiv);
            });
        }

        // Arranca pidiendo los nombres al cargar la app por primera vez
        configurarJugadores();
    </script>
</body>
</html>
