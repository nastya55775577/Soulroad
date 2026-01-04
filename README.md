<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Профессиональный навигатор</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Old+Standard+TT:wght@400;700&family=Cormorant+Garamond:wght@300;500;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Cormorant Garamond', serif;
            background: linear-gradient(135deg, #1a2a6c, #2c3e50, #4a569d);
            background-size: 400% 400%;
            animation: gradientBG 15s ease infinite;
            min-height: 100vh;
            overflow-x: hidden;
        }
        
        @keyframes gradientBG {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        
        .mystic-border {
            background: linear-gradient(45deg, transparent 30%, rgba(255,255,255,0.1) 50%, transparent 70%);
            background-size: 200% 200%;
            animation: shimmer 3s infinite;
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3), 
                        inset 0 0 20px rgba(255, 255, 255, 0.1);
        }
        
        @keyframes shimmer {
            0% { background-position: 0% 0%; }
            100% { background-position: 100% 100%; }
        }
        
        .ancient-font {
            font-family: 'Old Standard TT', serif;
            text-shadow: 0 0 10px rgba(255, 255, 255, 0.3);
        }
        
        .rune-pattern {
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='100' height='100' viewBox='0 0 100 100'%3E%3Cg fill-rule='evenodd'%3E%3Cg fill='%23ffffff' fill-opacity='0.05'%3E%3Cpath d='M50 30L70 50L50 70L30 50z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
        }
        
        .glass-effect {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .mystic-glow {
            box-shadow: 0 0 15px rgba(147, 112, 219, 0.6), 
                        0 0 30px rgba(147, 112, 219, 0.4);
        }
        
        .fade-in {
            animation: fadeIn 1.5s ease-in;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .typewriter {
            overflow: hidden;
            white-space: nowrap;
            animation: typing 3.5s steps(40, end);
        }
        
        @keyframes typing {
            from { width: 0 }
            to { width: 100% }
        }
        
        .mystic-bg {
            background: url('https://images.unsplash.com/photo-1503220317375-aaad61436b1b?ixlib=rb-4.0.3') no-repeat center center/cover;
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            opacity: 0.15;
        }
        
        .social-icon {
            transition: all 0.3s ease;
        }
        
        .social-icon:hover {
            transform: translateY(-5px);
            filter: drop-shadow(0 5px 15px rgba(147, 112, 219, 0.7));
        }
    </style>
</head>
<body class="text-gray-100">
    <div class="mystic-bg"></div>
    
    <div class="container mx-auto px-4 py-8 relative z-10">
        <header class="text-center mb-12 fade-in">
            <h1 class="ancient-font text-5xl md:text-7xl font-bold mb-4 text-transparent bg-clip-text bg-gradient-to-r from-purple-300 to-indigo-300">
                Твой Профессиональный Путь
            </h1>
            <p class="text-xl md:text-2xl text-purple-200">Открытие потенциала через древние знания</p>
        </header>
        
        <main class="max-w-3xl mx-auto">
            <div class="mystic-border rounded-2xl p-6 md:p-10 glass-effect mb-12 fade-in">
                <div id="chat-container" class="space-y-8">
                    <!-- Вопрос 1 -->
                    <div class="chat-message fade-in">
                        <div class="flex items-start">
                            <div class="mr-4 mt-1">
                                <div class="w-10 h-10 rounded-full bg-gradient-to-br from-purple-500 to-indigo-700 flex items-center justify-center ancient-font text-xl">
                                    Ω
                                </div>
                            </div>
                            <div class="flex-1">
                                <p class="ancient-font text-xl mb-4">
                                    Велkommen, путник! Меня зовут Геминиус, древний дух мудрости. 
                                    Чтобы открыть твой истинный путь, расскажи мне:
                                </p>
                                <p class="text-lg italic">
                                    "Подробно расскажи о себе: сколько тебе лет, что тебе нравится, 
                                    твои навыки, чем ты занимался на протяжении всей жизни, куда ты хочешь 
                                    двигаться и какие навыки ты хочешь применить в своей любимой профессии?"
                                </p>
                            </div>
                        </div>
                    </div>
                    
                    <!-- Ответ пользователя -->
                    <div class="chat-message fade-in">
                        <div class="flex items-start justify-end">
                            <div class="mr-4 mt-1">
                                <div class="w-10 h-10 rounded-full bg-gradient-to-br from-amber-500 to-orange-700 flex items-center justify-center">
                                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
                                    </svg>
                                </div>
                            </div>
                            <div class="flex-1 max-w-md">
                                <textarea 
                                    id="user-input" 
                                    class="w-full p-4 rounded-xl bg-white/10 border border-purple-500/30 focus:outline-none focus:ring-2 focus:ring-purple-400 ancient-font"
                                    rows="5"
                                    placeholder="Твой ответ..."
                                ></textarea>
                                <button id="analyze-btn" class="mt-4 px-6 py-2 bg-gradient-to-r from-purple-600 to-indigo-700 rounded-lg hover:from-purple-700 hover:to-indigo-800 transition-all duration-300 transform hover:-translate-y-1 shadow-lg">
                                    Анализировать
                                </button>
                            </div>
                        </div>
                    </div>
                    
                    <!-- Результаты анализа -->
                    <div id="analysis-results" class="hidden fade-in">
                        <div class="chat-message">
                            <div class="flex items-start">
                                <div class="mr-4 mt-1">
                                    <div class="w-10 h-10 rounded-full bg-gradient-to-br from-purple-500 to-indigo-700 flex items-center justify-center ancient-font text-xl">
                                        Ω
                                    </div>
                                </div>
                                <div class="flex-1">
                                    <h3 class="ancient-font text-2xl mb-4 text-purple-300">Твой Ведический Анализ</h3>
                                    
                                    <div class="grid md:grid-cols-2 gap-6 mb-6">
                                        <!-- Сильные стороны -->
                                        <div class="glass-effect p-5 rounded-xl border-l-4 border-green-400">
                                            <h4 class="text-lg font-semibold mb-2 flex items-center">
                                                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2 text-green-400" viewBox="0 0 20 20" fill="currentColor">
                                                    <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                                                </svg>
                                                Сильные Стороны
                                            </h4>
                                            <ul id="strengths" class="list-disc list-inside text-gray-200">
                                                <!-- Генерируется динамически -->
                                            </ul>
                                        </div>
                                        
                                        <!-- Зоны роста -->
                                        <div class="glass-effect p-5 rounded-xl border-l-4 border-amber-400">
                                            <h4 class="text-lg font-semibold mb-2 flex items-center">
                                                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2 text-amber-400" viewBox="0 0 20 20" fill="currentColor">
                                                    <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z" clip-rule="evenodd" />
                                                </svg>
                                                Зоны Роста
                                            </h4>
                                            <ul id="growth-areas" class="list-disc list-inside text-gray-200">
                                                <!-- Генерируется динамически -->
                                            </ul>
                                        </div>
                                    </div>
                                    
                                    <!-- Варианты профессий -->
                                    <div class="glass-effect p-5 rounded-xl border-l-4 border-blue-400 mb-6">
                                        <h4 class="text-lg font-semibold mb-3">Направления Профессионального Развития</h4>
                                        <div id="career-options" class="grid grid-cols-1 md:grid-cols-3 gap-4">
                                            <!-- Генерируется динамически -->
                                        </div>
                                    </div>
                                    
                                    <p class="mb-4">Какое направление тебе откликается?</p>
                                    <div class="flex flex-wrap gap-3">
                                        <button class="career-choice px-4 py-2 rounded-lg bg-gradient-to-r from-blue-600 to-cyan-600 hover:from-blue-700 hover:to-cyan-700 transition-all">
                                            Наука и Исследования
                                        </button>
                                        <button class="career-choice px-4 py-2 rounded-lg bg-gradient-to-r from-emerald-600 to-teal-600 hover:from-emerald-700 hover:to-teal-700 transition-all">
                                            Искусство и Творчество
                                        </button>
                                        <button class="career-choice px-4 py-2 rounded-lg bg-gradient-to-r from-amber-600 to-orange-600 hover:from-amber-700 hover:to-orange-700 transition-all">
                                            Предпринимательство
                                        </button>
                                        <button class="career-choice px-4 py-2 rounded-lg bg-gradient-to-r from-rose-600 to-pink-600 hover:from-rose-700 hover:to-pink-700 transition-all">
                                            Социальная Сфера
                                        </button>
                                    </div>
                                    <input id="custom-career" type="text" class="mt-3 w-full p-3 rounded-lg bg-white/10 border border-purple-500/30" placeholder="Или укажи своё направление">
                                    <button id="confirm-choice" class="mt-4 px-6 py-2 bg-gradient-to-r from-purple-600 to-indigo-700 rounded-lg hover:from-purple-700 hover:to-indigo-800 transition-all duration-300">
                                        Подтвердить выбор
                                    </button>
                                </div>
                            </div>
                        </div>
                        
                        <!-- План развития -->
                        <div id="development-plan" class="hidden mt-8 fade-in">
                            <div class="chat-message">
                                <div class="flex items-start">
                                    <div class="mr-4 mt-1">
                                        <div class="w-10 h-10 rounded-full bg-gradient-to-br from-purple-500 to-indigo-700 flex items-center justify-center ancient-font text-xl">
                                            Ω
                                        </div>
                                    </div>
                                    <div class="flex-1">
                                        <h3 class="ancient-font text-2xl mb-4 text-purple-300">Твой Путь на 12 Месяцев</h3>
                                        <div id="plan-content" class="space-y-4">
                                            <!-- Генерируется динамически -->
                                        </div>
                                        
                                        <div id="critical-analysis" class="mt-8 p-5 rounded-xl glass-effect border-l-4 border-rose-400">
                                            <h4 class="text-lg font-semibold mb-3">Критический Анализ</h4>
                                            <p class="text-gray-200">
                                                Основываясь на твоих ответах, твой потенциал лежит в области творческого синтеза. 
                                                Ключевые моменты для развития:
                                            </p>
                                            <ul class="list-disc list-inside mt-2 text-gray-200">
                                                <li>Сфокусируйся на углублении навыков в выбранной области</li>
                                                <li>Развивай навыки коммуникации - они критически важны</li>
                                                <li>Не бойся экспериментировать и выходить из зоны комфорта</li>
                                                <li>Система наставничества значительно ускорит прогресс</li>
                                            </ul>
                                            <p class="mt-3 italic">
                                                "Путь воина не в том, чтобы идти по нему без страха, а в том, чтобы идти, преодолевая страх"
                                            </p>
                                        </div>
                                        
                                        <div class="mt-8 text-center">
                                            <button id="reset-btn" class="px-6 py-2 bg-gradient-to-r from-gray-600 to-gray-800 rounded-lg hover:from-gray-700 hover:to-gray-900 transition-all">
                                                Начать заново
                                            </button>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </main>
        
        <footer class="text-center py-6 border-t border-white/10">
            <p class="mb-3">Создатель сайта:</p>
            <div class="flex justify-center space-x-4">
                <a href="https://www.instagram.com/iwegod?igsh=MWswMDRsbzN4aGhxcA==" target="_blank" class="social-icon">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="currentColor" class="text-purple-300">
                        <path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zM5.838 12a6.162 6.162 0 1112.324 0 6.162 6.162 0 01-12.324 0zM12 16a4 4 0 110-8 4 4 0 010 8zm4.965-10.405a1.44 1.44 0 112.881.001 1.44 1.44 0 01-2.881-.001z"/>
                    </svg>
                </a>
                <a href="https://t.me/ty_iskusstvo" target="_blank" class="social-icon">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="currentColor" class="text-purple-300">
                        <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm4.64 6.8l-1.94 9.18c-.14.64-.52.79-1.06.49l-2.96-2.18-1.42 1.38c-.16.16-.3.29-.6.29-.38 0-.31-.18-.31-.66V9.94c0-.52.2-.78.72-.78l5.57.02c.44 0 .7.22.7.66z"/>
                    </svg>
                </a>
            </div>
            <p class="mt-3 text-sm text-gray-400">© 2023 Твой Профессиональный Путь. Все права защищены.</p>
        </footer>
    </div>
    
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const userInput = document.getElementById('user-input');
            const analyzeBtn = document.getElementById('analyze-btn');
            const analysisResults = document.getElementById('analysis-results');
            const careerChoices = document.querySelectorAll('.career-choice');
            const customCareer = document.getElementById('custom-career');
            const confirmChoiceBtn = document.getElementById('confirm-choice');
            const developmentPlan = document.getElementById('development-plan');
            const resetBtn = document.getElementById('reset-btn');
            
            let selectedCareer = '';
            
            // Анализ введенного текста
            analyzeBtn.addEventListener('click', function() {
                const userText = userInput.value.trim();
                if (!userText) {
                    alert('Пожалуйста, расскажи о себе');
                    return;
                }
                
                // Генерация анализа
                generateAnalysis(userText);
                analysisResults.classList.remove('hidden');
                analysisResults.scrollIntoView({ behavior: 'smooth' });
            });
            
            // Выбор профессии
            careerChoices.forEach(button => {
                button.addEventListener('click', function() {
         careerChoices.forEach(btn => btn.classList.remove('ring-2', 'ring-white'));
                    this.classList.add('ring-2', 'ring-white');
                    selectedCareer = this.textContent;
                    customCareer.value = '';
                });
            });
            
            confirmChoiceBtn.addEventListener('click', function() {
                if (!selectedCareer && !customCareer.value.trim()) {
                    alert('Пожалуйста, выбери направление или укажи своё');
                    return;
                }
                
                const career = selectedCareer || customCareer.value.trim();
                generatePlan(career);
                developmentPlan.classList.remove('hidden');
                developmentPlan.scrollIntoView({ behavior: 'smooth' });
            });
            
            // Сброс
            resetBtn.addEventListener('click', function() {
                location.reload();
            });
            
            // Генерация анализа (заглушка)
            function generateAnalysis(text) {
                // Имитация анализа текста
                const strengths = ['Креативность', 'Аналитический склад ума', 'Коммуникабельность', 'Ответственность'];
                const growthAreas = ['Уверенность в публичных выступлениях', 'Глубокие технические знания', 'Управление временем'];
                const careerOptions = [
                    { name: 'UX-дизайнер', icon: '🎨' },
                    { name: 'Data Scientist', icon: '📊' },
                    { name: 'Digital-стратег', icon: '💡' }
                ];
                
                // Заполнение результатов
                document.getElementById('strengths').innerHTML = strengths.map(s => `<li>${s}</li>`).join('');
                document.getElementById('growth-areas').innerHTML = growthAreas.map(g => `<li>${g}</li>`).join('');
                
                const careerContainer = document.getElementById('career-options');
                careerContainer.innerHTML = careerOptions.map(option => `
                    <div class="glass-effect p-4 rounded-xl text-center">
                        <div class="text-3xl mb-2">${option.icon}</div>
                        <p>${option.name}</p>
                    </div>
                `).join('');
            }
            
            // Генерация плана развития
            function generatePlan(career) {
                const plan = [
                { month: 'Месяц 1-2', title: 'Фундамент', desc: `Изучение основ ${career}. Курсы: "Введение в профессию".` },
                    { month: 'Месяц 3-4', title: 'Практика', desc: 'Первые практические задачи. Создание портфолио.' },
                    { month: 'Месяц 5-6', title: 'Специализация', desc: 'Глубокое изучение ниши. Проекты для реальных клиентов.' },
                    { month: 'Месяц 7-9', title: 'Развитие', desc: 'Развитие soft skills. Нетворкинг.' },
                    { month: 'Месяц 10-12', title: 'Интеграция', desc: 'Собственный проект. Поиск работы/клиентов.' }
                ];
                
                const planContainer = document.getElementById('plan-content');
                planContainer.innerHTML = plan.map(item => `https://chat01.ai/en/chat/01K0NN6GGDJJZSGP37YY96MTN3
                    <div class="glass-effect p-4 rounded-xl border-l-4 border-purple-500">
                        <h5 class="font-semibold">${item.month} - ${item.title}</h5>
                        <p>${item.desc}</p>
                    </div>
                `).join('');
            }
        });
    </script>
</body>
</html>
