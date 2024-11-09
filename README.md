<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Tarea Fase 2</title>
    <style>
        /* Configuración de estilos para toda la página */
        body { font-family: 'Times New Roman', Times, serif, sans-serif; background-color: #f9f4f6; color: #333333; margin: 0; padding: 0; }
        h1, h2, h3 { color: #800040; }
        #main-container { max-width: 800px; margin: 20px auto; padding: 20px; background-color: #ffffff; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); border-radius: 8px; }
        #login-section, #task-form, #tasks-section { margin-bottom: 20px; }
        #task-form input, #task-form textarea, #task-form select { width: calc(100% - 20px); padding: 8px; margin: 10px 0; border: 1px solid #ccc; border-radius: 4px; }
        table { width: 100%; margin-top: 20px; border-collapse: collapse; }
        table, th, td { border: 1px solid #ccc; padding: 10px; text-align: left; }
        th { background-color: #80003c; color: white; }
        #task-detail-section { margin-top: 20px; display: none; }
    </style>
</head>
<body>
<div id="main-container">
    <h1><center>Tarea Fase 2</center></h1>
    <p><center>Bienvenido registro de tarea fase 2</center></p>

    <!-- Sección de inicio de sesión -->
    <div id="login-section">
        <h2>Inicio de Sesión</h2>
        <label for="username">Usuario:</label>
        <input type="text" id="username" /><br />
        <label for="password">Contraseña:</label>
        <input type="password" id="password" /><br />
        <button onclick="login()">Iniciar Sesión</button>
        <p id="error-msg" style="display: none; color: red; font-weight: bold;">Usuario o contraseña incorrectos.</p>
    </div>

    <!-- Sección principal después de autenticarse -->
    <div id="main-section" style="display: none;">
        <h2>Bienvenido, <span id="logged-username"></span></h2>

        <!-- Formulario de nueva tarea -->
        <div id="task-form">
            <h3>Registrar Nueva Tarea</h3>
            <label for="taskId">Código de la tarea (ID):</label>
            <input type="number" id="taskId" min="1" /><br />
            <label for="taskTitle">Título de la tarea:</label>
            <input type="text" id="taskTitle" /><br />
            <label for="taskDescription">Descripción de la tarea:</label>
            <textarea id="taskDescription"></textarea><br />
            <label for="startDate">Fecha de inicio:</label>
            <input type="date" id="startDate" /><br />
            <label for="clientName">Nombre del cliente:</label>
            <input type="text" id="clientName" /><br />
            <label for="projectId">ID del proyecto:</label>
            <input type="number" id="projectId" min="1" /><br />
            <label for="comments">Comentarios:</label>
            <textarea id="comments"></textarea><br />
            <label for="status">Estatus:</label>
            <select id="status">
                <option value="Por hacer">Por hacer</option>
                <option value="En progreso">En progreso</option>
                <option value="Rechazado">Rechazado</option>
                <option value="Cancelado">Cancelado</option>
                <option value="Cerrado">Cerrado</option>
                <option value="En pruebas de calidad">En pruebas de calidad</option>
                <option value="En validación por el usuario">En validación por el usuario</option>
                <option value="En proceso de liberación">En proceso de liberación</option>
            </select><br />
            <button onclick="addTask()">Agregar Tarea</button>
        </div>

        <!-- Filtro de estatus y tabla de tareas registradas -->
        <div id="tasks-section">
            <h3>Tareas Registradas</h3>
            <label for="statusFilter">Filtrar por estatus:</label>
            <select id="statusFilter" onchange="filterTasks()">
                <option value="">Mostrar todos</option>
                <option value="Por hacer">Por hacer</option>
                <option value="En progreso">En progreso</option>
                <option value="Rechazado">Rechazado</option>
                <option value="Cancelado">Cancelado</option>
                <option value="Cerrado">Cerrado</option>
                <option value="En pruebas de calidad">En pruebas de calidad</option>
                <option value="En validación por el usuario">En validación por el usuario</option>
                <option value="En proceso de liberación">En proceso de liberación</option>
            </select>
            <table>
                <thead>
                    <tr>
                        <th>ID</th>
                        <th>Título</th>
                        <th>Descripción</th>
                        <th>Fecha de Inicio</th>
                        <th>Cliente</th>
                        <th>ID del Proyecto</th>
                        <th>Comentarios</th>
                        <th>Estatus</th>
                        <th>Acción</th>
                    </tr>
                </thead>
                <tbody id="taskTableBody"></tbody>
            </table>
        </div>

        <!-- Sección de consulta de tarea seleccionada -->
        <div id="task-detail-section">
            <h3>Detalles de la Tarea Seleccionada</h3>
            <label for="editTaskId">ID de la tarea:</label>
            <input type="number" id="editTaskId" disabled /><br />
            <label for="editTaskTitle">Título de la tarea:</label>
            <input type="text" id="editTaskTitle" /><br />
            <label for="editTaskDescription">Descripción de la tarea:</label>
            <textarea id="editTaskDescription"></textarea><br />
            <label for="editStartDate">Fecha de inicio:</label>
            <input type="date" id="editStartDate" /><br />
            <label for="editClientName">Nombre del cliente:</label>
            <input type="text" id="editClientName" /><br />
            <label for="editProjectId">ID del proyecto:</label>
            <input type="number" id="editProjectId" min="1" /><br />
            <label for="editComments">Comentarios:</label>
            <textarea id="editComments"></textarea><br />
            <label for="editStatus">Estatus:</label>
            <select id="editStatus">
                <option value="Por hacer">Por hacer</option>
                <option value="En progreso">En progreso</option>
                <option value="Rechazado">Rechazado</option>
                <option value="Cancelado">Cancelado</option>
                <option value="Cerrado">Cerrado</option>
                <option value="En pruebas de calidad">En pruebas de calidad</option>
                <option value="En validación por el usuario">En validación por el usuario</option>
                <option value="En proceso de liberación">En proceso de liberación</option>
            </select><br />
            
            <!-- Campo para agregar nuevo comentario -->
            <label for="newComment">Nuevo Comentario:</label>
            <textarea id="newComment"></textarea><br />
            <button onclick="addNewComment()">Agregar Comentario</button>
            <br />

            <button onclick="updateTaskDetails()">Actualizar Tarea</button>
        </div>
    </div>
</div>

<script>
    const validUsername = "Hilda";
    const validPassword = "berron";

    function login() {
        const username = document.getElementById("username").value;
        const password = document.getElementById("password").value;
        const errorMsg = document.getElementById("error-msg");
        if (username === validUsername && password === validPassword) {
            document.getElementById("login-section").style.display = "none";
            document.getElementById("main-section").style.display = "block";
            document.getElementById("logged-username").innerText = username;
            errorMsg.style.display = "none";
        } else {
            errorMsg.style.display = "block";
        }
    }

    // Array de tareas
    let tasks = [];

    function addTask() {
        const taskId = document.getElementById("taskId").value;
        const taskTitle = document.getElementById("taskTitle").value;
        const taskDescription = document.getElementById("taskDescription").value;
        const startDate = document.getElementById("startDate").value;
        const clientName = document.getElementById("clientName").value;
        const projectId = document.getElementById("projectId").value;
        const comments = document.getElementById("comments").value;
        const status = document.getElementById("status").value;

        const task = {
            taskId,
            taskTitle,
            taskDescription,
            startDate,
            clientName,
            projectId,
            comments,
            status
        };

        tasks.push(task);
        updateTaskTable();
    }

    function updateTaskTable() {
        const taskTableBody = document.getElementById("taskTableBody");
        taskTableBody.innerHTML = "";
        tasks.forEach(task => {
            const row = document.createElement("tr");
            row.innerHTML = `
                <td>${task.taskId}</td>
                <td>${task.taskTitle}</td>
                <td>${task.taskDescription}</td>
                <td>${task.startDate}</td>
                <td>${task.clientName}</td>
                <td>${task.projectId}</td>
                <td>${task.comments}</td>
                <td>${task.status}</td>
                <td><button onclick="viewTaskDetails(${task.taskId})">Ver</button></td>
            `;
            taskTableBody.appendChild(row);
        });
    }

    function viewTaskDetails(taskId) {
        const task = tasks.find(t => t.taskId == taskId);
        if (task) {
            document.getElementById("task-detail-section").style.display = "block";
            document.getElementById("editTaskId").value = task.taskId;
            document.getElementById("editTaskTitle").value = task.taskTitle;
            document.getElementById("editTaskDescription").value = task.taskDescription;
            document.getElementById("editStartDate").value = task.startDate;
            document.getElementById("editClientName").value = task.clientName;
            document.getElementById("editProjectId").value = task.projectId;
            document.getElementById("editComments").value = task.comments;
            document.getElementById("editStatus").value = task.status;
        }
    }

    function updateTaskDetails() {
        const taskId = document.getElementById("editTaskId").value;
        const task = tasks.find(t => t.taskId == taskId);
        if (task) {
            task.taskTitle = document.getElementById("editTaskTitle").value;
            task.taskDescription = document.getElementById("editTaskDescription").value;
            task.startDate = document.getElementById("editStartDate").value;
            task.clientName = document.getElementById("editClientName").value;
            task.projectId = document.getElementById("editProjectId").value;
            task.comments = document.getElementById("editComments").value;
            task.status = document.getElementById("editStatus").value;

            updateTaskTable();
        }
    }

    function addNewComment() {
        const taskId = document.getElementById("editTaskId").value;
        const newComment = document.getElementById("newComment").value;
        const task = tasks.find(t => t.taskId == taskId);
        if (task) {
            // Obtener la fecha actual
            const currentDate = new Date().toLocaleString();
            // Concatenar el comentario con la fecha
            task.comments += `\n[${currentDate}] ${newComment}`;
            document.getElementById("editComments").value = task.comments;
        }
    }

    function filterTasks() {
        const statusFilter = document.getElementById("statusFilter").value;
        const filteredTasks = tasks.filter(task => !statusFilter || task.status === statusFilter);
        const taskTableBody = document.getElementById("taskTableBody");
        taskTableBody.innerHTML = "";
        filteredTasks.forEach(task => {
            const row = document.createElement("tr");
            row.innerHTML = `
                <td>${task.taskId}</td>
                <td>${task.taskTitle}</td>
                <td>${task.taskDescription}</td>
                <td>${task.startDate}</td>
                <td>${task.clientName}</td>
                <td>${task.projectId}</td>
                <td>${task.comments}</td>
                <td>${task.status}</td>
                <td><button onclick="viewTaskDetails(${task.taskId})">Ver</button></td>
            `;
            taskTableBody.appendChild(row);
        });
    }
</script>
</body>
</html>
