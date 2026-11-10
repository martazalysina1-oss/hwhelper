# hwhelper
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>hwhelper - Проверка заданий</title>
    <style>
        /* --- ЦВЕТА И ШРИФТЫ --- */
        :root {
            --bg-color: #f4f5f7;
            --sidebar-bg: #1e1e24;
            --pink-accent: #f7d6e0;
            --pink-text: #d86a8e;
            --mint-accent: #d1e8e2;
            --blue-accent: #d0e1f9;
            --text-dark: #333333;
        }
        * { box-sizing: border-box; font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif; margin: 0; padding: 0; }
        body { background-color: var(--bg-color); color: var(--text-dark); display: flex; min-height: 100vh; }

        /* --- БОКОВОЕ МЕНЮ (на ПК) --- */
        .sidebar { width: 220px; background-color: var(--sidebar-bg); color: white; padding: 20px; display: none; flex-direction: column; gap: 20px; }
        .sidebar h2 { font-size: 24px; margin-bottom: 20px; color: var(--pink-accent); }
        .sidebar a { color: #a0a0a0; text-decoration: none; font-size: 15px; padding: 10px; border-radius: 8px; transition: 0.3s; }
        .sidebar a:hover { background-color: #333; color: white; }

        /* --- ОСНОВНОЙ ЭКРАН --- */
        .main-content { flex: 1; padding: 20px; padding-bottom: 80px; }
        .header h1 { font-size: 22px; margin-bottom: 5px; }
        .header p { font-size: 14px; color: #777; margin-bottom: 20px; }

        /* --- КАРТОЧКИ ПРЕДМЕТОВ --- */
        .subjects-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 25px; }
        .subject-card { padding: 15px; border-radius: 16px; cursor: pointer; transition: transform 0.2s; box-shadow: 0 4px 10px rgba(0,0,0,0.03); }
        .subject-card:active { transform: scale(0.98); }
        .subject-card h3 { font-size: 14px; margin-bottom: 5px; }
        .subject-card span { font-size: 11px; opacity: 0.7; }
        
        .card-math { background-color: var(--pink-accent); }
        .card-phys { background-color: var(--mint-accent); }
        .card-info { background-color: var(--blue-accent); }
        .card-rus { background-color: #fcebd5; }

        /* --- ФОРМА ПРОВЕРКИ --- */
        .checker-box { background: white; padding: 20px; border-radius: 20px; box-shadow: 0 6px 15px rgba(0,0,0,0.05); margin-bottom: 20px; }
        .checker-box h2 { font-size: 18px; margin-bottom: 15px; }
        .input-group { margin-bottom: 15px; }
        .input-group label { display: block; font-size: 13px; color: #555; margin-bottom: 5px; }
        .input-group input, .input-group textarea { width: 100%; padding: 12px; border: 1px solid #ddd; border-radius: 10px; font-size: 14px; outline: none; }
        .input-group textarea { resize: vertical; min-height: 80px; }
        .input-group input:focus, .input-group textarea:focus { border-color: var(--pink-text); }

        .btn-check { width: 100%; background-color: var(--sidebar-bg); color: white; border: none; padding: 14px; border-radius: 10px; font-size: 16px; font-weight: bold; cursor: pointer; }
        .btn-check:active { background-color: #333; }

        /* --- РЕЗУЛЬТАТ ПРОВЕРКИ --- */
        .result-box { background: #fff0f5; border: 1px solid var(--pink-accent); padding: 15px; border-radius: 15px; display: none; margin-top: 15px; font-size: 14px; line-height: 1.5; }
        .result-box.show { display: block; animation: fadeIn 0.5s; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* --- НИЖНЕЕ МЕНЮ (для телефона) --- */
        .mobile-nav { position: fixed; bottom: 0; left: 0; width: 100%; background: white; display: flex; justify-content: space-around; padding: 12px 0; box-shadow: 0 -2px 10px rgba(0,0,0,0.05); }
        .mobile-nav div { font-size: 12px; color: #777; text-align: center; }
        .mobile-nav div.active { color: var(--pink-text); font-weight: bold; }

        /* --- ДЕСКТОПНАЯ ВЕРСИЯ --- */
        @media (min-width: 768px) {
            .sidebar { display: flex; }
            .mobile-nav { display: none; }
            .main-content { padding: 40px; padding-bottom: 40px; }
            .subjects-grid { grid-template-columns: repeat(4, 1fr); }
            .header h1 { font-size: 28px; }
            .checker-box { max-width: 600px; }
        }
    </style>
</head>
<body>

    <!-- Боковое меню (для ПК) -->
    <aside class="sidebar">
        <h2>hwhelper</h2>
        <a href="#">📊 Главная</a>
        <a href="#">📝 Мои задания</a>
        <a href="#">✨ Проверка</a>
        <a href="#">⚙️ Настройки</a>
    </aside>

    <!-- Основной контент -->
    <main class="main-content">
        <div class="header">
            <h1>Доброе утро, Марта! ☀️</h1>
            <p>Готова проверить свои знания? Выбирай предмет и отправляй задание.</p>
        </div>

        <!-- Карточки предметов -->
        <div class="subjects-grid">
            <div class="subject-card card-math" onclick="selectSubject('Математика')">
                <h3>Математика</h3>
                <span>12 заданий</span>
            </div>
            <div class="subject-card card-phys" onclick="selectSubject('Физика')">
                <h3>Физика</h3>
                <span>8 заданий</span>
            </div>
            <div class="subject-card card-info" onclick="selectSubject('Информатика')">
                <h3>Информатика</h3>
                <span>5 заданий</span>
            </div>
            <div class="subject-card card-rus" onclick="selectSubject('Русский язык')">
                <h3>Русский</h3>
                <span>15 заданий</span>
            </div>
        </div>

        <!-- Блок проверки -->
        <div class="checker-box">
            <h2 id="form-title">Проверка задания: Выбери предмет выше</h2>
            
            <div class="input-group">
                <label>Условие задачи (текст)</label>
                <textarea id="task-text" placeholder="Вставь сюда текст задачи..."></textarea>
            </div>
            
            <div class="input-group">
                <label>Твой ответ / решение</label>
                <textarea id="student-answer" placeholder="Напиши свой ответ здесь..."></textarea>
            </div>

            <button class="btn-check" onclick="checkHomework()">Проверить ответ ✨</button>

            <!-- Блок для вывода результата -->
            <div id="result-box" class="result-box">
                <!-- Сюда ИИ будет писать ответ -->
            </div>
        </div>
    </main>

    <!-- Нижнее меню (для телефона) -->
    <nav class="mobile-nav">
        <div class="active">🏠<br>Главная</div>
        <div>📝<br>Задания</div>
        <div>⚙️<br>Настройки</div>
    </nav>

    <!-- ЛОГИКА (JavaScript) -->
    <script>
        let currentSubject = "";

        // Функция выбора предмета
        function selectSubject(subject) {
            currentSubject = subject;
            document.getElementById('form-title').innerText = "Проверка задания: " + subject;
            document.getElementById('task-text').focus();
        }

        // Функция проверки (пока имитация работы ИИ)
        function checkHomework() {
            const task = document.getElementById('task-text').value;
            const answer = document.getElementById('student-answer').value;
            const resultBox = document.getElementById('result-box');

            if (!currentSubject) {
                alert("Пожалуйста, сначала выбери предмет (нажми на цветную карточку)!");
                return;
            }

            if (!task || !answer) {
                alert("Пожалуйста, заполни условие задачи и свой ответ!");
                return;
            }

            // Показываем, что идет проверка
            resultBox.classList.add('show');
            resultBox.innerHTML = "⏳ <i>Анализирую твой ответ (это займет пару секунд)...</i>";

            // Имитация задержки ответа от ИИ (через 2 секунды)
            setTimeout(() => {
                resultBox.innerHTML = `
                    <strong>✅ Проверка завершена!</strong><br><br>
                    <b>Предмет:</b> ${currentSubject}<br>
                    <b>Твой ответ:</b> ${answer}<br><br>
                    <span style="color: #d86a8e; font-weight: bold;">Оценка: 85/100 баллов</span><br>
                    <i>В целом решение верное, но в 3-м действии есть небольшая арифметическая ошибка. Рекомендую перепроверить расчеты.</i>
                    <br><br>
                    <small style="color:#777">(Это тестовая проверка. Позже мы подключим сюда настоящий ИИ для глубокого анализа!)</small>
                `;
            }, 2000);
        }
    </script>
</body>
</html>
