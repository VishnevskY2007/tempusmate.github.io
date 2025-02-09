<!DOCTYPE html>
<html>
<head>
    <title>Schedule Manager</title>
    <meta charset="UTF-8">
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        .container { padding: 20px; }
        .section { margin-bottom: 30px; }
        .form-group { margin-bottom: 15px; }
        input, button { width: 100%; padding: 10px; }
    </style>
</head>
<body>
    <div class="container">
        <!-- Форма добавления задачи -->
        <div class="section">
            <h2>Добавить задачу</h2>
            <div class="form-group">
                <input type="date" id="taskDate" required>
            </div>
            <div class="form-group">
                <input type="time" id="startTime" required>
                <input type="time" id="endTime" required>
            </div>
            <div class="form-group">
                <input type="text" id="taskDesc" placeholder="Описание" required>
            </div>
            <button onclick="addTask()">Добавить</button>
        </div>

        <!-- Форма таймера -->
        <div class="section">
            <h2>Установить таймер</h2>
            <div class="form-group">
                <input type="number" id="timerSeconds" placeholder="Секунды" required>
            </div>
            <button onclick="setTimer()">Установить</button>
        </div>
    </div>

    <script>
        function addTask() {
            const data = {
                action: 'add_task',
                date: document.getElementById('taskDate').value,
                start_time: document.getElementById('startTime').value,
                end_time: document.getElementById('endTime').value,
                description: document.getElementById('taskDesc').value
            };
            Telegram.WebApp.sendData(JSON.stringify(data));
        }

        function setTimer() {
            const data = {
                action: 'set_timer',
                duration: document.getElementById('timerSeconds').value
            };
            Telegram.WebApp.sendData(JSON.stringify(data));
        }

        Telegram.WebApp.ready();
        Telegram.WebApp.expand();
    </script>
</body>
</html>
