<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TimeWise - Управление временем</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        * {
            box-sizing: border-box;
            font-family: 'Arial', sans-serif;
        }

        body {
            margin: 0;
            padding: 20px;
            background: #f5f5f5;
        }

        .container {
            max-width: 600px;
            margin: 0 auto;
        }

        .section {
            background: white;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }

        h2 {
            color: #2c3e50;
            margin-top: 0;
        }

        .form-group {
            margin-bottom: 15px;
        }

        input, button {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
        }

        button {
            background: #3498db;
            color: white;
            border: none;
            cursor: pointer;
            transition: background 0.3s;
        }

        button:hover {
            background: #2980b9;
        }

        .time-inputs {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="section">
            <h2>📅 Добавить задачу</h2>
            <div class="form-group">
                <input type="date" id="taskDate" required>
            </div>
            <div class="form-group time-inputs">
                <input type="time" id="startTime" required>
                <input type="time" id="endTime" required>
            </div>
            <div class="form-group">
                <input type="text" id="taskDesc" placeholder="Описание задачи" required>
            </div>
            <button onclick="addTask()">Добавить в расписание</button>
        </div>

        <div class="section">
            <h2>⏱ Установить таймер</h2>
            <div class="form-group">
                <input type="number" id="timerSeconds" 
                       placeholder="Длительность в секундах" 
                       min="1" required>
            </div>
            <button onclick="setTimer()">Запустить таймер</button>
        </div>
    </div>

    <script>
        // Инициализация Telegram Web App
        Telegram.WebApp.ready();
        Telegram.WebApp.expand();

        function addTask() {
            const taskData = {
                action: 'add_task',
                date: document.getElementById('taskDate').value,
                start_time: document.getElementById('startTime').value,
                end_time: document.getElementById('endTime').value,
                description: document.getElementById('taskDesc').value
            };
            Telegram.WebApp.sendData(JSON.stringify(taskData));
        }

        function setTimer() {
            const seconds = document.getElementById('timerSeconds').value;
            if (seconds > 0) {
                const timerData = {
                    action: 'set_timer',
                    duration: seconds
                };
                Telegram.WebApp.sendData(JSON.stringify(timerData));
            }
        }

        // Закрытие веб-приложения после отправки данных
        Telegram.WebApp.onEvent('webAppDataReceived', function() {
            Telegram.WebApp.close();
        });
    </script>
</body>
</html>
