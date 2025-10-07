<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Voto Electrónico</title>
    <meta name="theme-color" content="#2c3e50">
    <meta name="description" content="Sistema seguro de voto electrónico">
    <link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>🗳️</text></svg>">
    <link rel="apple-touch-icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>🗳️</text></svg>">
    <link rel="manifest" href="manifest.json">
    <style>
        :root {
            --primary-color: #2c3e50;
            --secondary-color: #3498db;
            --success-color: #27ae60;
            --danger-color: #e74c3c;
            --light-color: #ecf0f1;
            --dark-color: #34495e;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: #f5f7fa;
            color: var(--dark-color);
            line-height: 1.6;
            min-height: 100vh;
            padding: env(safe-area-inset-top) env(safe-area-inset-right) env(safe-area-inset-bottom) env(safe-area-inset-left);
        }
        
        .container {
            max-width: 100%;
            margin: 0 auto;
            padding: 15px;
        }
        
        header {
            background-color: var(--primary-color);
            color: white;
            padding: 1rem 0;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            position: sticky;
            top: 0;
            z-index: 100;
        }
        
        .header-content {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0 15px;
        }
        
        .logo {
            font-size: 1.3rem;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .logo-icon {
            font-size: 1.5rem;
        }
        
        .auth-buttons button {
            background-color: var(--secondary-color);
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 0.9rem;
        }
        
        .install-btn {
            background-color: var(--success-color) !important;
            margin-right: 10px;
        }
        
        .page {
            display: none;
            animation: fadeIn 0.5s;
        }
        
        .active {
            display: block;
        }
        
        .card {
            background: white;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            padding: 20px;
            margin-bottom: 20px;
        }
        
        h1, h2, h3 {
            margin-bottom: 15px;
            color: var(--primary-color);
        }
        
        .form-group {
            margin-bottom: 15px;
        }
        
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: 600;
        }
        
        input, select {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 16px;
        }
        
        button {
            background-color: var(--secondary-color);
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s;
            width: 100%;
        }
        
        button:hover {
            background-color: #2980b9;
        }
        
        button.danger {
            background-color: var(--danger-color);
        }
        
        button.danger:hover {
            background-color: #c0392b;
        }
        
        button.success {
            background-color: var(--success-color);
        }
        
        button.success:hover {
            background-color: #219653;
        }
        
        .user-type-selector {
            display: flex;
            flex-direction: column;
            gap: 15px;
            margin: 20px 0;
        }
        
        .user-card {
            text-align: center;
            cursor: pointer;
            transition: transform 0.3s;
            padding: 20px;
        }
        
        .user-card:hover {
            transform: translateY(-5px);
        }
        
        .user-card h3 {
            margin-top: 10px;
        }
        
        .user-icon {
            font-size: 3rem;
            margin-bottom: 10px;
        }
        
        .election-list {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        
        .election-card {
            border-left: 4px solid var(--secondary-color);
        }
        
        .candidate-list {
            display: flex;
            flex-direction: column;
            gap: 15px;
            margin-top: 20px;
        }
        
        .candidate-card {
            text-align: center;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .candidate-card:hover {
            border-color: var(--secondary-color);
            box-shadow: 0 2px 8px rgba(52, 152, 219, 0.2);
        }
        
        .candidate-card.selected {
            border-color: var(--success-color);
            background-color: rgba(39, 174, 96, 0.1);
        }
        
        .candidate-image {
            width: 80px;
            height: 80px;
            border-radius: 50%;
            object-fit: cover;
            margin: 0 auto 10px;
            display: block;
            background-color: #f0f0f0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
        }
        
        .results-chart {
            margin-top: 20px;
        }
        
        .result-bar {
            display: flex;
            align-items: center;
            margin-bottom: 10px;
        }
        
        .result-label {
            width: 120px;
            font-weight: 600;
            font-size: 0.9rem;
        }
        
        .result-bar-inner {
            height: 25px;
            background-color: var(--secondary-color);
            display: flex;
            align-items: center;
            padding: 0 10px;
            color: white;
            border-radius: 4px;
            min-width: 40px;
        }
        
        .alert {
            padding: 10px 15px;
            border-radius: 4px;
            margin-bottom: 15px;
        }
        
        .alert-success {
            background-color: rgba(39, 174, 96, 0.2);
            border-left: 4px solid var(--success-color);
        }
        
        .alert-error {
            background-color: rgba(231, 76, 60, 0.2);
            border-left: 4px solid var(--danger-color);
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
            font-size: 0.9rem;
        }
        
        th, td {
            padding: 10px 12px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
        
        th {
            background-color: #f2f2f2;
            font-weight: 600;
        }
        
        tr:hover {
            background-color: #f5f5f5;
        }
        
        .back-button {
            background-color: #95a5a6;
            margin-bottom: 15px;
            width: auto;
            display: inline-block;
            padding: 10px 15px;
        }
        
        .action-buttons {
            display: flex;
            gap: 10px;
            margin-top: 10px;
        }
        
        .action-buttons button {
            width: auto;
            flex: 1;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        /* Estilos específicos para móviles */
        @media (min-width: 768px) {
            .container {
                max-width: 600px;
            }
            
            .user-type-selector {
                flex-direction: row;
            }
            
            .action-buttons {
                flex-direction: row;
            }
        }
        
        /* Mejoras de usabilidad para móviles */
        input, select, button {
            -webkit-appearance: none;
            -moz-appearance: none;
            appearance: none;
        }
        
        /* Efectos táctiles para móviles */
        button:active {
            transform: scale(0.98);
        }
        
        .user-card:active, .candidate-card:active {
            transform: scale(0.98);
        }
    </style>
</head>
<body>
    <header>
        <div class="header-content">
            <div class="logo">
                <span class="logo-icon">🗳️</span>
                <span>Voto Electrónico</span>
            </div>
            <div class="auth-buttons">
                <button id="installBtn" class="install-btn" style="display: none;">Instalar App</button>
                <button id="logoutBtn" style="display: none;">Cerrar Sesión</button>
            </div>
        </div>
    </header>

    <div class="container">
        <!-- Página de inicio -->
        <div id="homePage" class="page active">
            <div class="card">
                <h1>Voto Electrónico Seguro</h1>
                <p>Seleccione su tipo de usuario para continuar:</p>
                
                <div class="user-type-selector">
                    <div class="user-card card" id="adminCard">
                        <div class="user-icon">👨‍💼</div>
                        <h3>Administrador</h3>
                        <p>Acceso con correo, contraseña y DNI</p>
                    </div>
                    <div class="user-card card" id="voterCard">
                        <div class="user-icon">👤</div>
                        <h3>Votante</h3>
                        <p>Acceso solo con correo electrónico</p>
                    </div>
                </div>
            </div>
            
            <div class="card">
                <h3>Instalar la aplicación</h3>
                <p>Para una mejor experiencia, instale esta aplicación en su dispositivo.</p>
                <button id="installInfoBtn">Cómo instalar</button>
            </div>
        </div>

        <!-- Página de login para administrador -->
        <div id="adminLoginPage" class="page">
            <button class="back-button" id="backToHomeFromAdminLogin">← Volver</button>
            <div class="card">
                <h2>Administrador</h2>
                <form id="adminLoginForm">
                    <div class="form-group">
                        <label for="adminEmail">Correo Electrónico</label>
                        <input type="email" id="adminEmail" value="edujavi01@gmail.com" required>
                    </div>
                    <div class="form-group">
                        <label for="adminPassword">Contraseña</label>
                        <input type="password" id="adminPassword" value="alfa72567" required>
                    </div>
                    <div class="form-group">
                        <label for="adminDNI">DNI</label>
                        <input type="text" id="adminDNI" value="46122321" required>
                    </div>
                    <button type="submit" class="success">Ingresar</button>
                </form>
            </div>
        </div>

        <!-- Página de login para votante -->
        <div id="voterLoginPage" class="page">
            <button class="back-button" id="backToHomeFromVoterLogin">← Volver</button>
            <div class="card">
                <h2>Votante</h2>
                <form id="voterLoginForm">
                    <div class="form-group">
                        <label for="voterEmail">Correo Electrónico</label>
                        <input type="email" id="voterEmail" required>
                    </div>
                    <button type="submit" class="success">Ingresar</button>
                </form>
            </div>
        </div>

        <!-- Panel de administración -->
        <div id="adminPanelPage" class="page">
            <button class="back-button" id="backFromAdminPanel">← Cerrar Sesión</button>
            <div class="card">
                <h2>Panel de Administración</h2>
                <div id="adminAlert"></div>
                
                <h3>Crear Nueva Elección</h3>
                <form id="createElectionForm">
                    <div class="form-group">
                        <label for="electionName">Nombre de la Elección</label>
                        <input type="text" id="electionName" required>
                    </div>
                    <div class="form-group">
                        <label for="electionDescription">Descripción</label>
                        <input type="text" id="electionDescription" required>
                    </div>
                    <div class="form-group">
                        <label for="candidates">Candidatos (separados por coma)</label>
                        <input type="text" id="candidates" placeholder="Candidato 1, Candidato 2, Candidato 3" required>
                    </div>
                    <div class="form-group">
                        <label for="voterEmails">Correos de votantes (separados por coma)</label>
                        <input type="text" id="voterEmails" placeholder="email1@ejemplo.com, email2@ejemplo.com" required>
                    </div>
                    <button type="submit" class="success">Crear Elección</button>
                </form>
            </div>

            <div class="card">
                <h3>Elecciones Activas</h3>
                <div id="electionsList" class="election-list">
                    <!-- Las elecciones se cargarán aquí dinámicamente -->
                </div>
            </div>

            <div class="card">
                <h3>Reporte de Votantes</h3>
                <table id="votersTable">
                    <thead>
                        <tr>
                            <th>Correo</th>
                            <th>Elección</th>
                            <th>Estado</th>
                        </tr>
                    </thead>
                    <tbody>
                        <!-- Los datos de votantes se cargarán aquí dinámicamente -->
                    </tbody>
                </table>
            </div>
        </div>

        <!-- Panel de votación -->
        <div id="votingPage" class="page">
            <button class="back-button" id="backFromVoting">← Volver</button>
            <div class="card">
                <h2 id="electionTitle">Elección en Curso</h2>
                <p id="electionDescriptionText">Descripción de la elección</p>
                
                <div id="candidatesList" class="candidate-list">
                    <!-- Los candidatos se cargarán aquí dinámicamente -->
                </div>
                
                <button id="submitVote" class="success" style="display: none; margin-top: 20px;">Confirmar Voto</button>
                <div id="voteAlert"></div>
            </div>
        </div>

        <!-- Página de confirmación de voto -->
        <div id="voteConfirmationPage" class="page">
            <div class="card">
                <div style="text-align: center; margin: 20px 0;">
                    <div style="font-size: 4rem;">✅</div>
                </div>
                <h2 style="text-align: center;">¡Voto Registrado Exitosamente!</h2>
                <p style="text-align: center;">Su voto ha sido registrado de manera segura y anónima.</p>
                <p style="text-align: center;">Gracias por participar en el proceso electoral.</p>
                <button id="backToHomeFromConfirmation" class="success">Volver al Inicio</button>
            </div>
        </div>

        <!-- Página de información de instalación -->
        <div id="installInfoPage" class="page">
            <button class="back-button" id="backFromInstallInfo">← Volver</button>
            <div class="card">
                <h2>Instalar la Aplicación</h2>
                <div class="install-step">
                    <h3>📱 En Android:</h3>
                    <p>1. Abra el menú de opciones (tres puntos) en Chrome</p>
                    <p>2. Seleccione "Agregar a la pantalla de inicio"</p>
                    <p>3. Confirme la instalación</p>
                </div>
                <div class="install-step">
                    <h3>📱 En iPhone/iPad:</h3>
                    <p>1. Pulse el ícono de compartir (cuadrado con flecha)</p>
                    <p>2. Desplácese y seleccione "Agregar a pantalla de inicio"</p>
                    <p>3. Confirme la instalación</p>
                </div>
                <button id="tryInstallBtn">Intentar Instalación Automática</button>
            </div>
        </div>
    </div>

    <script>
        // Datos de ejemplo para la demostración
        let elections = [
            {
                id: 1,
                name: "Elección de Representante Estudiantil",
                description: "Elección para elegir al representante estudiantil del año 2023",
                candidates: ["María González", "Carlos Rodríguez", "Ana Martínez"],
                votes: {},
                voters: ["votante1@ejemplo.com", "votante2@ejemplo.com", "votante3@ejemplo.com"]
            },
            {
                id: 2,
                name: "Elección de Comité de Empresa",
                description: "Elección para los miembros del comité de empresa",
                candidates: ["Juan Pérez", "Laura Sánchez", "Pedro López", "Sofía García"],
                votes: {},
                voters: ["empleado1@empresa.com", "empleado2@empresa.com", "empleado3@empresa.com"]
            }
        ];

        let currentUser = null;
        let currentElection = null;
        let selectedCandidate = null;
        let deferredPrompt = null;

        // Elementos del DOM
        const pages = {
            home: document.getElementById('homePage'),
            adminLogin: document.getElementById('adminLoginPage'),
            voterLogin: document.getElementById('voterLoginPage'),
            adminPanel: document.getElementById('adminPanelPage'),
            voting: document.getElementById('votingPage'),
            voteConfirmation: document.getElementById('voteConfirmationPage'),
            installInfo: document.getElementById('installInfoPage')
        };

        // Botones de navegación
        document.getElementById('adminCard').addEventListener('click', () => showPage('adminLogin'));
        document.getElementById('voterCard').addEventListener('click', () => showPage('voterLogin'));
        document.getElementById('backToHomeFromAdminLogin').addEventListener('click', () => showPage('home'));
        document.getElementById('backToHomeFromVoterLogin').addEventListener('click', () => showPage('home'));
        document.getElementById('backFromAdminPanel').addEventListener('click', () => {
            currentUser = null;
            showPage('home');
        });
        document.getElementById('backFromVoting').addEventListener('click', () => {
            if (currentUser && currentUser.type === 'voter') {
                showPage('voterLogin');
            } else {
                showPage('home');
            }
        });
        document.getElementById('backToHomeFromConfirmation').addEventListener('click', () => {
            currentUser = null;
            showPage('home');
        });
        document.getElementById('logoutBtn').addEventListener('click', () => {
            currentUser = null;
            document.getElementById('logoutBtn').style.display = 'none';
            showPage('home');
        });
        document.getElementById('installInfoBtn').addEventListener('click', () => showPage('installInfo'));
        document.getElementById('backFromInstallInfo').addEventListener('click', () => showPage('home'));
        document.getElementById('tryInstallBtn').addEventListener('click', () => {
            if (deferredPrompt) {
                deferredPrompt.prompt();
                deferredPrompt.userChoice.then((choiceResult) => {
                    if (choiceResult.outcome === 'accepted') {
                        console.log('Usuario aceptó la instalación');
                    }
                    deferredPrompt = null;
                });
            } else {
                showAlert('adminAlert', 'La instalación automática no está disponible. Siga las instrucciones manuales.', 'error');
            }
        });

        // Formularios
        document.getElementById('adminLoginForm').addEventListener('submit', handleAdminLogin);
        document.getElementById('voterLoginForm').addEventListener('submit', handleVoterLogin);
        document.getElementById('createElectionForm').addEventListener('submit', handleCreateElection);
        document.getElementById('submitVote').addEventListener('click', handleVoteSubmission);

        // Función para mostrar una página específica
        function showPage(pageName) {
            // Ocultar todas las páginas
            Object.values(pages).forEach(page => {
                page.classList.remove('active');
            });
            
            // Mostrar la página solicitada
            pages[pageName].classList.add('active');
        }

        // Manejo del login del administrador
        function handleAdminLogin(e) {
            e.preventDefault();
            
            const email = document.getElementById('adminEmail').value;
            const password = document.getElementById('adminPassword').value;
            const dni = document.getElementById('adminDNI').value;
            
            // Validación con las credenciales proporcionadas
            if (email === 'edujavi01@gmail.com' && password === 'alfa72567' && dni === '46122321') {
                currentUser = { type: 'admin', email };
                document.getElementById('logoutBtn').style.display = 'block';
                showPage('adminPanel');
                loadAdminPanel();
            } else {
                showAlert('adminAlert', 'Credenciales incorrectas. Intente nuevamente.', 'error');
            }
        }

        // Manejo del login del votante
        function handleVoterLogin(e) {
            e.preventDefault();
            
            const email = document.getElementById('voterEmail').value;
            
            // Verificar si el correo está registrado en alguna elección
            const voterElections = elections.filter(election => 
                election.voters.includes(email)
            );
            
            if (voterElections.length > 0) {
                currentUser = { type: 'voter', email };
                // Mostrar la primera elección disponible para este votante
                currentElection = voterElections[0];
                showPage('voting');
                loadVotingInterface();
            } else {
                showAlert('voteAlert', 'Su correo no está registrado para votar en ninguna elección.', 'error');
            }
        }

        // Cargar el panel de administración
        function loadAdminPanel() {
            // Cargar lista de elecciones
            const electionsList = document.getElementById('electionsList');
            electionsList.innerHTML = '';
            
            if (elections.length === 0) {
                electionsList.innerHTML = '<p>No hay elecciones creadas aún.</p>';
            } else {
                elections.forEach(election => {
                    const totalVotes = Object.keys(election.votes).length;
                    const totalVoters = election.voters.length;
                    
                    const electionCard = document.createElement('div');
                    electionCard.className = 'election-card card';
                    electionCard.innerHTML = `
                        <h3>${election.name}</h3>
                        <p>${election.description}</p>
                        <p><strong>Progreso:</strong> ${totalVotes}/${totalVoters} votos</p>
                        <div class="action-buttons">
                            <button onclick="viewElectionResults(${election.id})" class="success">Ver Resultados</button>
                            <button onclick="deleteElection(${election.id})" class="danger">Eliminar</button>
                        </div>
                    `;
                    electionsList.appendChild(electionCard);
                });
            }
            
            // Cargar reporte de votantes
            loadVotersReport();
        }

        // Cargar reporte de votantes
        function loadVotersReport() {
            const votersTable = document.getElementById('votersTable').querySelector('tbody');
            votersTable.innerHTML = '';
            
            if (elections.length === 0) {
                votersTable.innerHTML = '<tr><td colspan="3" style="text-align: center;">No hay datos de votantes</td></tr>';
            } else {
                elections.forEach(election => {
                    election.voters.forEach(voterEmail => {
                        const hasVoted = election.votes && election.votes[voterEmail] !== undefined;
                        
                        const row = document.createElement('tr');
                        row.innerHTML = `
                            <td>${voterEmail}</td>
                            <td>${election.name}</td>
                            <td>${hasVoted ? '✅ Votó' : '⏳ Pendiente'}</td>
                        `;
                        votersTable.appendChild(row);
                    });
                });
            }
        }

        // Manejar la creación de una nueva elección
        function handleCreateElection(e) {
            e.preventDefault();
            
            const name = document.getElementById('electionName').value;
            const description = document.getElementById('electionDescription').value;
            const candidatesInput = document.getElementById('candidates').value;
            const voterEmailsInput = document.getElementById('voterEmails').value;
            
            // Separar los candidatos por comas y limpiar espacios
            const candidates = candidatesInput.split(',').map(candidate => candidate.trim());
            const voters = voterEmailsInput.split(',').map(email => email.trim());
            
            // Validar que haya al menos 2 candidatos
            if (candidates.length < 2) {
                showAlert('adminAlert', 'Debe haber al menos 2 candidatos.', 'error');
                return;
            }
            
            // Validar que haya al menos 1 votante
            if (voters.length < 1) {
                showAlert('adminAlert', 'Debe haber al menos 1 votante.', 'error');
                return;
            }
            
            // Crear nueva elección
            const newElection = {
                id: elections.length > 0 ? Math.max(...elections.map(e => e.id)) + 1 : 1,
                name,
                description,
                candidates,
                votes: {},
                voters
            };
            
            elections.push(newElection);
            
            // Limpiar el formulario
            document.getElementById('createElectionForm').reset();
            
            // Recargar el panel de administración
            loadAdminPanel();
            
            showAlert('adminAlert', 'Elección creada exitosamente.', 'success');
            
            // Guardar en localStorage
            saveData();
        }

        // Cargar la interfaz de votación
        function loadVotingInterface() {
            document.getElementById('electionTitle').textContent = currentElection.name;
            document.getElementById('electionDescriptionText').textContent = currentElection.description;
            
            const candidatesList = document.getElementById('candidatesList');
            candidatesList.innerHTML = '';
            
            currentElection.candidates.forEach((candidate, index) => {
                const candidateCard = document.createElement('div');
                candidateCard.className = 'candidate-card';
                candidateCard.innerHTML = `
                    <div class="candidate-image">${candidate.charAt(0)}</div>
                    <h3>${candidate}</h3>
                `;
                candidateCard.addEventListener('click', () => selectCandidate(index, candidateCard));
                candidatesList.appendChild(candidateCard);
            });
            
            // Verificar si el usuario ya votó
            if (currentElection.votes[currentUser.email] !== undefined) {
                document.getElementById('submitVote').style.display = 'none';
                showAlert('voteAlert', 'Usted ya ha votado en esta elección.', 'error');
            } else {
                document.getElementById('submitVote').style.display = 'block';
            }
        }

        // Seleccionar un candidato
        function selectCandidate(candidateIndex, candidateElement) {
            // Deseleccionar todos los candidatos
            document.querySelectorAll('.candidate-card').forEach(card => {
                card.classList.remove('selected');
            });
            
            // Seleccionar el candidato clickeado
            candidateElement.classList.add('selected');
            selectedCandidate = candidateIndex;
        }

        // Manejar el envío del voto
        function handleVoteSubmission() {
            if (selectedCandidate === null) {
                showAlert('voteAlert', 'Por favor, seleccione un candidato antes de votar.', 'error');
                return;
            }
            
            // Registrar el voto
            currentElection.votes[currentUser.email] = selectedCandidate;
            
            // Guardar en localStorage
            saveData();
            
            // Mostrar confirmación
            showPage('voteConfirmation');
        }

        // Ver resultados de una elección
        function viewElectionResults(electionId) {
            const election = elections.find(e => e.id === electionId);
            if (!election) return;
            
            // Calcular resultados
            const results = {};
            election.candidates.forEach((candidate, index) => {
                results[index] = 0;
            });
            
            Object.values(election.votes).forEach(vote => {
                results[vote] = (results[vote] || 0) + 1;
            });
            
            // Mostrar resultados en un modal
            let resultsHTML = `<h3>Resultados: ${election.name}</h3>`;
            resultsHTML += `<p>Total de votos: ${Object.values(election.votes).length}</p>`;
            resultsHTML += '<div class="results-chart">';
            
            election.candidates.forEach((candidate, index) => {
                const votes = results[index] || 0;
                const percentage = Object.values(election.votes).length > 0 ? 
                    (votes / Object.values(election.votes).length * 100).toFixed(1) : 0;
                
                resultsHTML += `
                    <div class="result-bar">
                        <div class="result-label">${candidate}</div>
                        <div class="result-bar-inner" style="width: ${percentage}%">
                            ${votes} votos (${percentage}%)
                        </div>
                    </div>
                `;
            });
            
            resultsHTML += '</div>';
            
            // Crear modal de resultados
            const modal = document.createElement('div');
            modal.style.position = 'fixed';
            modal.style.top = '0';
            modal.style.left = '0';
            modal.style.width = '100%';
            modal.style.height = '100%';
            modal.style.backgroundColor = 'rgba(0,0,0,0.5)';
            modal.style.display = 'flex';
            modal.style.alignItems = 'center';
            modal.style.justifyContent = 'center';
            modal.style.zIndex = '1000';
            
            const modalContent = document.createElement('div');
            modalContent.className = 'card';
            modalContent.style.maxWidth = '90%';
            modalContent.style.maxHeight = '80%';
            modalContent.style.overflow = 'auto';
            modalContent.innerHTML = resultsHTML;
            
            const closeButton = document.createElement('button');
            closeButton.textContent = 'Cerrar';
            closeButton.onclick = () => document.body.removeChild(modal);
            modalContent.appendChild(closeButton);
            
            modal.appendChild(modalContent);
            document.body.appendChild(modal);
            
            // Cerrar modal al hacer clic fuera
            modal.onclick = (e) => {
                if (e.target === modal) {
                    document.body.removeChild(modal);
                }
            };
        }

        // Eliminar una elección
        function deleteElection(electionId) {
            if (confirm('¿Está seguro de que desea eliminar esta elección? Esta acción no se puede deshacer.')) {
                elections = elections.filter(e => e.id !== electionId);
                loadAdminPanel();
                saveData();
            }
        }

        // Mostrar alertas
        function showAlert(containerId, message, type) {
            const alertContainer = document.getElementById(containerId);
            alertContainer.innerHTML = `
                <div class="alert alert-${type}">${message}</div>
            `;
            
            // Auto-ocultar la alerta después de 5 segundos
            setTimeout(() => {
                alertContainer.innerHTML = '';
            }, 5000);
        }

        // Guardar datos en localStorage
        function saveData() {
            localStorage.setItem('electionsData', JSON.stringify(elections));
        }

        // Cargar datos desde localStorage
        function loadData() {
            const savedData = localStorage.getItem('electionsData');
            if (savedData) {
                elections = JSON.parse(savedData);
            }
        }

        // Service Worker para PWA
        if ('serviceWorker' in navigator) {
            window.addEventListener('load', () => {
                navigator.serviceWorker.register('/sw.js')
                    .then((registration) => {
                        console.log('SW registered: ', registration);
                    })
                    .catch((registrationError) => {
                        console.log('SW registration failed: ', registrationError);
                    });
            });
        }

        // Manejar la instalación de la PWA
        window.addEventListener('beforeinstallprompt', (e) => {
            e.preventDefault();
            deferredPrompt = e;
            document.getElementById('installBtn').style.display = 'block';
        });

        document.getElementById('installBtn').addEventListener('click', () => {
            if (deferredPrompt) {
                deferredPrompt.prompt();
                deferredPrompt.userChoice.then((choiceResult) => {
                    if (choiceResult.outcome === 'accepted') {
                        console.log('Usuario aceptó la instalación');
                        document.getElementById('installBtn').style.display = 'none';
                    }
                    deferredPrompt = null;
                });
            }
        });

        // Inicialización
        document.addEventListener('DOMContentLoaded', () => {
            showPage('home');
            loadData();
            
            // Verificar si la app ya está instalada
            if (window.matchMedia('(display-mode: standalone)').matches) {
                document.getElementById('installBtn').style.display = 'none';
            }
        });
    </script>

    <script>
        // Service Worker simplificado (se ejecutaría en un archivo separado en producción)
        const staticCacheName = 'voto-electronico-v1';
        const assets = [
            '/',
            '/index.html',
            '/manifest.json'
        ];

        self.addEventListener('install', event => {
            event.waitUntil(
                caches.open(staticCacheName)
                    .then(cache => {
                        return cache.addAll(assets);
                    })
            );
        });

        self.addEventListener('fetch', event => {
            event.respondWith(
                caches.match(event.request)
                    .then(response => {
                        return response || fetch(event.request);
                    })
            );
        });
    </script>
</body>
</html>
