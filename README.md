<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Schedule & Timer Manager</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 15px;
            background: #f0f2f5;
        }
        .container {
            max-width: 600px;
            margin: 0 auto;
        }
        .btn {
            background: #0088cc;
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 5px;
            margin: 5px;
            cursor: pointer;
            width: 100%;
        }
        .calendar {
            background: white;
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 15px;
        }
        .task-list {
            background: white;
            padding: 15px;
            border-radius: 10px;
        }
        input, textarea {
            width: 100%;
            padding: 8px;
            margin: 5px 0;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div id="mainMenu">
            <button class="btn" onclick="showSection('schedule')">🗓️ Расписание</button>
            <button class="btn" onclick="showSection('timer')">⏱️ Таймеры</button>
            <button class="btn" onclick="showAddTaskForm()">➕ Новая задача</button>
        </div>

        <div id="scheduleSection" class="hidden">
            <div class="calendar">
                <h3>Календарь</h3>
                <input type="date" id="calendarDate" onchange="loadSchedule()">
            </div>
            <div class="task-list" id="tasks"></div>
        </div>

        <div id="timerSection" class="hidden">
            <div class="task-list">
                <h3>Активные таймеры</h3>
                <div id="activeTimers"></div>
                <button class="btn" onclick="showTimerForm()">⏰ Новый таймер</button>
            </div>
        </div>

        <div id="addTaskForm" class="hidden">
            <h3>Новая задача</h3>
            <input type="date" id="taskDate" required>
            <input type="time" id="startTime" required>
            <input type="time" id="endTime" required>
            <textarea id="taskDesc" placeholder="Описание" required></textarea>
            <button class="btn" onclick="saveTask()">Сохранить</button>
            <button class="btn" onclick="showMainMenu()">Отмена</button>
        </div>

        <div id="addTimerForm" class="hidden">
            <h3>Новый таймер</h3>
            <input type="number" id="timerSeconds" placeholder="Секунды" required>
            <button class="btn" onclick="setTimer()">Установить</button>
            <button class="btn" onclick="showMainMenu()">Отмена</button>
        </div>
    </div>

    <script>
        function showSection(section) {
            document.querySelectorAll('div[id$="Section"], div[id$="Form"]').forEach(el => {
                el.style.display = 'none';
            });
            document.getElementById(section + 'Section').style.display = 'block';
        }

        function showAddTaskForm() {
            document.getElementById('mainMenu').style.display = 'none';
            document.getElementById('addTaskForm').style.display = 'block';
        }

        function showTimerForm() {
            document.getElementById('mainMenu').style.display = 'none';
            document.getElementById('addTimerForm').style.display = 'block';
        }

        function showMainMenu() {
            document.querySelectorAll('div').forEach(el => {
                el.style.display = 'none';
            });
            document.getElementById('mainMenu').style.display = 'block';
        }

        // Взаимодействие с ботом через Telegram API
        function saveTask() {
            const task = {
                date: document.getElementById('taskDate').value,
                start: document.getElementById('startTime').value,
                end: document.getElementById('endTime').value,
                desc: document.getElementById('taskDesc').value
            };
            
            Telegram.WebApp.sendData(JSON.stringify({
                action: 'add_task',
                data: task
            }));
            showMainMenu();
        }

        function setTimer() {
            const seconds = document.getElementById('timerSeconds').value;
            Telegram.WebApp.sendData(JSON.stringify({
                action: 'set_timer',
                data: seconds
            }));
            showMainMenu();
        }

        // Инициализация Web App
        Telegram.WebApp.ready();
        Telegram.WebApp.expand();
    </script>
</body>
</html>
