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
