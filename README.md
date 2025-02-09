<!DOCTYPE html>
<html>
<head>
    <title>Schedule Manager</title>
    <meta charset="UTF-8">
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        body { font-family: Arial, sans-serif; padding: 20px; }
        .container { max-width: 600px; margin: 0 auto; }
        .section { margin-bottom: 30px; }
        .form-group { margin-bottom: 15px; }
        input, button { width: 100%; padding: 10px; margin-bottom: 10px; }
        .nav { display: flex; justify-content: space-between; margin-bottom: 20px; }
        .nav button { flex: 1; margin: 0 5px; }
        .task-list { margin-top: 20px; }
        .task-item { padding: 10px; border: 1px solid #ccc; margin-bottom: 10px; border-radius: 5px; }
    </style>
</head>
<body>
    <div class="container">
        <!-- Навигация -->
        <div class="nav">
            <button onclick="showMainPage()">Главная</button>
            <button onclick="showAddTaskPage()">Добавить задачу</button>
            <button onclick="showAddTimerPage()">Добавить таймер</button>
        </div>

        <!-- Основная страница -->
        <div id="mainPage">
            <h2>Ваше расписание</h2>
            <div id="scheduleList" class="task-list">
                <!-- Сюда будут загружены задачи -->
            </div>
        </div>

        <!-- Страница добавления задачи -->
        <div id="addTaskPage" style="display: none;">
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
            <button onclick="showMainPage()">Назад</button>
        </div>

        <!-- Страница добавления таймера -->
        <div id="addTimerPage" style="display: none;">
            <h2>Установить таймер</h2>
            <div class="form-group">
                <input type="number" id="timerSeconds" placeholder="Секунды" required>
            </div>
            <button onclick="setTimer()">Установить</button>
            <button onclick="showMainPage()">Назад</button>
        </div>
    </div>

    <script>
        // Функции для навигации
        function showMainPage() {
            document.getElementById('mainPage').style.display = 'block';
            document.getElementById('addTaskPage').style.display = 'none';
            document.getElementById('addTimerPage').style.display = 'none';
            loadSchedule(); // Загружаем расписание при возврате на главную страницу
        }

        function showAddTaskPage() {
            document.getElementById('mainPage').style.display = 'none';
            document.getElementById('addTaskPage').style.display = 'block';
            document.getElementById('addTimerPage').style.display = 'none';
        }

        function showAddTimerPage() {
            document.getElementById('mainPage').style.display = 'none';
            document.getElementById('addTaskPage').style.display = 'none';
            document.getElementById('addTimerPage').style.display = 'block';
        }

        // Загрузка расписания
        function loadSchedule() {
            Telegram.WebApp.sendData(JSON.stringify({ action: 'view_schedule' }));
        }

        // Обработка данных от бота
        Telegram.WebApp.onEvent('mainButtonClicked', function() {
            const data = JSON.parse(Telegram.WebApp.sendData());
            if (data.action === 'view_schedule') {
                const scheduleList = document.getElementById('scheduleList');
                scheduleList.innerHTML = '';
                if (data.tasks && data.tasks.length > 0) {
                    data.tasks.forEach(task => {
                        const taskItem = document.createElement('div');
                        taskItem.className = 'task-item';
                        taskItem.innerHTML = `
                            <strong>📅 ${task.date}</strong><br>
                            ⏰ ${task.time_range}: ${task.description}
                        `;
                        scheduleList.appendChild(taskItem);
                    });
                } else {
                    scheduleList.innerHTML = '<p>Расписание пусто.</p>';
                }
            }
        });

        // Добавление задачи
        function addTask() {
            const data = {
                action: 'add_task',
                date: document.getElementById('taskDate').value,
                start_time: document.getElementById('startTime').value,
                end_time: document.getElementById('endTime').value,
                description: document.getElementById('taskDesc').value
            };
            Telegram.WebApp.sendData(JSON.stringify(data));
            showMainPage(); // Возврат на главную страницу после добавления
        }

        // Установка таймера
        function setTimer() {
            const data = {
                action: 'set_timer',
                duration: document.getElementById('timerSeconds').value
            };
            Telegram.WebApp.sendData(JSON.stringify(data));
            showMainPage(); // Возврат на главную страницу после установки
        }

        // Инициализация
        Telegram.WebApp.ready();
        Telegram.WebApp.expand();
        showMainPage(); // Показываем главную страницу при загрузке
    </script>
</body>
</html>
