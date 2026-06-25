<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Муу-рманск | Коровьи Новости</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;800;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- Библиотека Supabase -->
    <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Nunito', 'sans-serif'],
                    },
                    colors: {
                        icy: '#e0f2fe',
                        murmansk: '#0ea5e9',
                        cow: '#1f2937',
                    }
                }
            }
        }
    </script>

    <style>
        body {
            background-color: #f0f9ff;
            background-image: 
                radial-gradient(at 0% 0%, hsla(213,100%,73%,1) 0, transparent 50%), 
                radial-gradient(at 100% 0%, hsla(200,100%,88%,1) 0, transparent 50%),
                url("data:image/svg+xml,%3Csvg width='100' height='100' viewBox='0 0 100 100' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M10 10 Q 20 0 30 10 T 50 10 T 70 30 T 50 50 T 20 40 Z' fill='rgba(255,255,255,0.4)'/%3E%3Cpath d='M80 70 Q 90 60 100 80 T 80 100 T 60 90 T 70 70 Z' fill='rgba(255,255,255,0.4)'/%3E%3C/svg%3E");
            background-attachment: fixed;
            color: #1f2937;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            overflow-x: hidden;
            cursor: crosshair;
        }

        .glass-panel {
            background: rgba(255, 255, 255, 0.7);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.5);
            box-shadow: 0 8px 32px 0 rgba(14, 165, 233, 0.1);
        }

        .news-card {
            transition: transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275), box-shadow 0.3s;
        }
        .news-card:hover {
            transform: translateY(-10px) scale(1.02);
            box-shadow: 0 20px 40px rgba(14, 165, 233, 0.2);
        }

        /* Анимация бегущей строки */
        @keyframes ticker {
            0% { transform: translateX(100%); }
            100% { transform: translateX(-100%); }
        }
        .ticker-wrap {
            width: 100%;
            overflow: hidden;
            background-color: #ef4444; 
            color: white;
            padding: 8px 0;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            position: relative;
            z-index: 30;
        }
        .ticker {
            display: inline-block;
            white-space: nowrap;
            animation: ticker 25s linear infinite;
            font-weight: 800;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        /* Кнопка кликера */
        .coin-btn {
            background: radial-gradient(circle, #fbbf24 0%, #f59e0b 100%);
            box-shadow: 0 10px 25px rgba(245, 158, 11, 0.5), inset 0 -5px 15px rgba(180, 83, 9, 0.5);
            transition: transform 0.1s, box-shadow 0.1s;
            user-select: none;
            -webkit-user-select: none;
            touch-action: manipulation;
        }
        .coin-btn:active {
            transform: scale(0.95);
            box-shadow: 0 5px 10px rgba(245, 158, 11, 0.5), inset 0 -2px 5px rgba(180, 83, 9, 0.5);
        }

        /* Снег */
        .snowflake {
            position: fixed;
            top: -10px;
            color: white;
            font-size: 1.5em;
            user-select: none;
            pointer-events: none;
            z-index: 1000;
            text-shadow: 0 0 5px rgba(0,0,0,0.1);
            animation: fall linear forwards;
        }
        @keyframes fall {
            to { transform: translateY(105vh) rotate(360deg); }
        }

        /* Вылетающий текст при клике */
        .floating-text {
            position: absolute;
            color: #fbbf24;
            font-weight: 900;
            font-size: 24px;
            pointer-events: none;
            animation: floatUp 1s ease-out forwards;
            text-shadow: 0px 2px 4px rgba(0,0,0,0.3);
            z-index: 50;
        }
        @keyframes floatUp {
            0% { opacity: 1; transform: translateY(0) scale(1); }
            100% { opacity: 0; transform: translateY(-50px) scale(1.5); }
        }

        /* Скрытие контента */
        .hidden-view {
            display: none;
        }

        .nav-btn.active {
            color: #0ea5e9;
            border-bottom: 3px solid #0ea5e9;
        }
    </style>
</head>
<body class="relative">

    <!-- Бегущая строка -->
    <div class="ticker-wrap">
        <div class="ticker">
            🚨 СРОЧНО: В порту Мурманска пришвартовался ледокол, полностью груженный сеном! 🚨 МЭР КОРОВКА ОБЪЯВИЛ ВЫХОДНОЙ 🚨 ЦЕНА НА МОЛОКО ВЗЛЕТЕЛА ДО НЕБЕС 🚨 МУУ-КОИН БЬЕТ РЕКОРДЫ НА БИРЖЕ 🚨
        </div>
    </div>

    <!-- Навигация -->
    <nav class="glass-panel sticky top-0 z-40">
        <div class="max-w-4xl mx-auto px-4">
            <div class="flex justify-between items-center py-4">
                <a href="https://t.me/NovostiMyyyrmanska" target="_blank" class="flex items-center gap-2 text-xl font-black text-murmansk hover:scale-105 transition">
                    <i class="fa-solid fa-cow text-2xl"></i>
                    МУУ-РМАНСК
                </a>
                
                <div class="flex gap-2 sm:gap-6 overflow-x-auto pb-2 sm:pb-0 hide-scrollbar">
                    <button onclick="switchView('news')" id="btn-news" class="nav-btn active font-bold text-gray-600 px-2 py-1 whitespace-nowrap"><i class="fa-solid fa-newspaper mr-1"></i> Новости</button>
                    <button onclick="switchView('clicker')" id="btn-clicker" class="nav-btn font-bold text-gray-600 px-2 py-1 whitespace-nowrap"><i class="fa-solid fa-coins mr-1"></i> Игра</button>
                    <button onclick="switchView('leaderboard')" id="btn-leaderboard" class="nav-btn font-bold text-gray-600 px-2 py-1 whitespace-nowrap"><i class="fa-solid fa-trophy mr-1"></i> Топ</button>
                    <button onclick="switchView('translator')" id="btn-translator" class="nav-btn font-bold text-gray-600 px-2 py-1 whitespace-nowrap"><i class="fa-solid fa-language mr-1"></i> Муу-Перевод</button>
                    <button onclick="switchView('profile')" id="btn-profile" class="nav-btn font-bold text-gray-600 px-2 py-1 whitespace-nowrap"><i class="fa-solid fa-user mr-1"></i> Кабинет</button>
                </div>
            </div>
        </div>
    </nav>

    <!-- Основной контент -->
    <main class="flex-grow max-w-4xl mx-auto w-full p-4 pb-20 relative z-10" id="main-content">
        
        <!-- Вкладка: Новости -->
        <section id="view-news" class="space-y-6">
            <div class="text-center mb-8">
                <h1 class="text-4xl md:text-5xl font-black text-murmansk mb-4 uppercase drop-shadow-md">Главные события стада</h1>
                <p class="text-lg text-gray-600 font-semibold">Вся правда о Мурманске, которую от вас скрывают пастухи.</p>
                <a href="https://t.me/NovostiMyyyrmanska" target="_blank" class="inline-block mt-4 bg-murmansk text-white font-bold py-2 px-6 rounded-full hover:bg-sky-600 hover:shadow-lg transition">
                    <i class="fa-brands fa-telegram mr-2"></i> Читать в Telegram
                </a>
            </div>

            <!-- Виджет погоды -->
            <div class="glass-panel rounded-2xl p-4 flex items-center justify-between shadow-sm">
                <div class="flex items-center gap-4">
                    <i class="fa-regular fa-snowflake text-4xl text-sky-400"></i>
                    <div>
                        <h3 class="font-bold text-gray-800">Погода в Муу-рманске</h3>
                        <p class="text-sm text-gray-600">Холодно, снег, местами летит сено.</p>
                    </div>
                </div>
                <div class="text-right">
                    <span class="text-2xl font-black text-sky-500">-15°C</span>
                </div>
            </div>

            <div id="news-container" class="grid grid-cols-1 md:grid-cols-2 gap-6 mt-6">
                <!-- Новости рендерятся через JS -->
            </div>
        </section>

        <!-- Вкладка: Кликер (МууКоин) -->
        <section id="view-clicker" class="hidden-view">
            <div class="glass-panel rounded-3xl p-8 text-center max-w-sm mx-auto shadow-xl relative">
                <div class="absolute top-4 right-4 text-xs font-bold text-gray-400" id="sync-status">☁️ Синхронизировано</div>
                <h2 class="text-2xl font-black text-gray-800 mb-2">Добыча MooCoin</h2>
                <div class="bg-gray-100 rounded-xl p-4 mb-8 shadow-inner">
                    <div class="flex justify-center items-center gap-2 text-4xl font-black text-yellow-500">
                        <i class="fa-solid fa-coins"></i>
                        <span id="coin-balance">0</span>
                    </div>
                    <p class="text-sm text-gray-500 mt-1 font-bold">Ваш баланс</p>
                </div>

                <!-- Монетка -->
                <div class="relative w-48 h-48 mx-auto mb-8 cursor-pointer coin-btn rounded-full flex items-center justify-center border-4 border-yellow-300" onclick="clickCoin(event)">
                    <i class="fa-solid fa-cow text-6xl text-white drop-shadow-md"></i>
                </div>

                <!-- Энергия -->
                <div class="mb-6">
                    <div class="flex justify-between text-sm font-bold text-gray-600 mb-1">
                        <span><i class="fa-solid fa-bolt text-yellow-400"></i> Энергия</span>
                        <span id="energy-text">1000 / 1000</span>
                    </div>
                    <div class="w-full bg-gray-200 rounded-full h-4 shadow-inner">
                        <div id="energy-bar" class="bg-gradient-to-r from-yellow-400 to-orange-500 h-4 rounded-full transition-all duration-300" style="width: 100%"></div>
                    </div>
                </div>

                <div class="grid grid-cols-2 gap-4">
                    <button onclick="getBonus()" id="btn-bonus" class="bg-sky-100 text-sky-600 font-bold py-3 px-4 rounded-xl hover:bg-sky-200 transition shadow-sm">
                        <i class="fa-solid fa-gift mb-1 text-xl block"></i> Бонус
                    </button>
                    <a href="https://t.me/NovostiMyyyrmanska" target="_blank" class="bg-murmansk text-white font-bold py-3 px-4 rounded-xl hover:bg-sky-600 transition shadow-sm">
                        <i class="fa-brands fa-telegram mb-1 text-xl block"></i> Подписка
                    </a>
                </div>
                <p id="bonus-timer" class="text-xs text-gray-400 mt-2 font-bold hidden"></p>
                
                <div id="auth-warning-clicker" class="mt-4 text-sm text-red-500 font-bold hidden">
                    ⚠️ Войдите в Кабинет, чтобы сохранить монеты в облако!
                </div>
            </div>
        </section>

        <!-- Вкладка: Топ Коров (Leaderboard) -->
        <section id="view-leaderboard" class="hidden-view">
            <div class="glass-panel rounded-3xl p-6 max-w-lg mx-auto shadow-xl">
                <h2 class="text-3xl font-black text-center text-gray-800 mb-6 uppercase">🏆 Топ Коров Мурманска</h2>
                <p class="text-center text-sm text-gray-500 mb-6 font-bold">Самые богатые быки на районе (Живые данные из БД)</p>
                
                <div id="leaderboard-container" class="space-y-3">
                    <div class="text-center py-10 text-gray-400"><i class="fa-solid fa-spinner fa-spin text-3xl"></i><br>Загрузка данных из облака...</div>
                </div>
            </div>
        </section>

        <!-- Вкладка: Переводчик -->
        <section id="view-translator" class="hidden-view">
            <div class="glass-panel rounded-3xl p-6 md:p-10 max-w-2xl mx-auto shadow-xl">
                <h2 class="text-3xl font-black text-center text-gray-800 mb-2">Муу-Переводчик</h2>
                <p class="text-center text-gray-600 mb-8 font-semibold">Пойми о чем мычат местные!</p>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-sm font-bold text-gray-700 mb-2">Человеческий (Ввод):</label>
                        <textarea id="human-text" rows="4" class="w-full border-2 border-gray-200 rounded-xl p-3 focus:border-murmansk focus:ring-0 resize-none font-medium" placeholder="Напиши что-нибудь..."></textarea>
                    </div>
                    <div>
                        <label class="block text-sm font-bold text-gray-700 mb-2">Мурманский-коровий (Результат):</label>
                        <div id="cow-text" class="w-full h-full min-h-[100px] bg-sky-50 border-2 border-sky-100 rounded-xl p-3 font-black text-sky-800 text-lg flex items-center justify-center text-center">
                            МУУ...
                        </div>
                    </div>
                </div>
                <div class="text-center mt-6">
                    <button onclick="translateToMoo()" class="bg-murmansk text-white font-bold text-lg py-3 px-8 rounded-full hover:bg-sky-600 shadow-lg hover:shadow-xl transition transform hover:-translate-y-1">
                        <i class="fa-solid fa-language mr-2"></i> Перевести
                    </button>
                </div>
            </div>
        </section>

        <!-- Вкладка: Профиль/Админка -->
        <section id="view-profile" class="hidden-view">
            <div class="glass-panel rounded-3xl p-6 md:p-10 max-w-md mx-auto shadow-xl">
                
                <!-- Форма входа -->
                <div id="auth-form-container">
                    <h2 class="text-3xl font-black text-center text-gray-800 mb-6">Стой, кто идет?</h2>
                    <p class="text-center text-sm font-bold text-gray-500 mb-6">Введите почту для облачного сохранения MooCoins. Пароль не нужен!</p>
                    
                    <form id="auth-form" class="space-y-4" onsubmit="handleAuth(event)">
                        <div>
                            <label class="block text-sm font-bold text-gray-700 mb-1">Имя (кличка)</label>
                            <input type="text" id="auth-name" class="w-full border-2 border-gray-200 rounded-xl p-3 focus:border-murmansk focus:outline-none" placeholder="Например: Буренка" required>
                        </div>
                        <div>
                            <label class="block text-sm font-bold text-gray-700 mb-1">Почта (для связи с БД)</label>
                            <input type="email" id="auth-email" class="w-full border-2 border-gray-200 rounded-xl p-3 focus:border-murmansk focus:outline-none" placeholder="ваша@почта.com" required>
                            <p class="text-xs text-gray-400 mt-1">* Введите mste747@gmail.com для прав админа</p>
                        </div>
                        <button type="submit" id="auth-btn" class="w-full bg-murmansk text-white font-bold text-lg py-3 px-4 rounded-xl hover:bg-sky-600 shadow-md transition flex justify-center items-center">
                            <span id="auth-btn-text">Войти в стадо</span>
                            <i id="auth-btn-spinner" class="fa-solid fa-spinner fa-spin ml-2 hidden"></i>
                        </button>
                    </form>
                </div>

                <!-- Кабинет пользователя (скрыт по умолчанию) -->
                <div id="user-dashboard" class="hidden">
                    <div class="text-center mb-6">
                        <div class="w-24 h-24 bg-sky-100 rounded-full mx-auto flex items-center justify-center border-4 border-white shadow-lg mb-4">
                            <i class="fa-solid fa-cow text-4xl text-murmansk"></i>
                        </div>
                        <h2 class="text-2xl font-black text-gray-800" id="profile-name">Имя</h2>
                        <p class="text-sm font-bold text-gray-500" id="profile-email">email@test.com</p>
                        <span id="profile-role" class="inline-block mt-2 bg-gray-200 text-gray-700 text-xs px-2 py-1 rounded-full font-bold uppercase tracking-wide">Рядовая Корова</span>
                    </div>

                    <!-- Панель Администратора (показывается только Мэру) -->
                    <div id="admin-panel" class="hidden bg-red-50 border-2 border-red-200 rounded-xl p-4 mb-6">
                        <h3 class="font-black text-red-600 text-lg mb-3"><i class="fa-solid fa-crown"></i> Панель Мэра</h3>
                        <p class="text-xs text-red-400 mb-4 font-bold">Опубликовать новость в базу данных</p>
                        <form onsubmit="handlePostNews(event)" class="space-y-3">
                            <input type="text" id="admin-news-title" class="w-full border border-red-200 rounded-lg p-2 text-sm focus:border-red-400 focus:outline-none" placeholder="Заголовок новости..." required>
                            <textarea id="admin-news-text" rows="3" class="w-full border border-red-200 rounded-lg p-2 text-sm focus:border-red-400 focus:outline-none resize-none" placeholder="Текст новости..." required></textarea>
                            <button type="submit" id="admin-submit-btn" class="w-full bg-red-500 text-white font-bold py-2 rounded-lg hover:bg-red-600 transition flex justify-center items-center">
                                <span id="admin-submit-text">Опубликовать в БД</span>
                                <i id="admin-submit-spinner" class="fa-solid fa-spinner fa-spin ml-2 hidden"></i>
                            </button>
                        </form>
                        <div id="admin-success" class="text-green-600 text-xs font-bold mt-2 text-center hidden">Новость успешно добавлена в базу!</div>
                    </div>

                    <button onclick="logout()" class="w-full bg-gray-200 text-gray-700 font-bold py-3 px-4 rounded-xl hover:bg-gray-300 transition">
                        Выйти
                    </button>
                </div>
            </div>
        </section>

    </main>

    <script>
        /* =========================================
           НАСТРОЙКА SUPABASE (БАЗА ДАННЫХ)
           ========================================= */
        const supabaseUrl = 'https://xnlmdyarjkurseycixef.supabase.co';
        const supabaseKey = 'sb_publishable_nt6a6bPPGafs0vAsivmkdQ_pi9SC7rO';
        const supabaseClient = window.supabase.createClient(supabaseUrl, supabaseKey);

        /* =========================================
           СОСТОЯНИЕ ПРИЛОЖЕНИЯ
           ========================================= */
        const state = {
            user: null, // Данные пользователя из БД
            clicker: {
                coins: 0,
                energy: 1000,
                maxEnergy: 1000,
                lastSyncCoins: 0, // Для оптимизации запросов в БД
                lastSyncEnergy: 1000
            },
            bonusCooldown: false,
            news: [
                // Дефолтные новости на случай, если таблица 'news' пустая или ее нет
                { title: "Забастовка на пастбище №4", text: "Коровы требуют включить им Lo-Fi Beats для повышения удоев. Мэр обещал разобраться.", img: "https://images.unsplash.com/photo-1546445317-29f4545e9d53?auto=format&fit=crop&w=400&q=80" },
                { title: "Новый тренд Муу-Тока", text: "Местные телята массово жуют одуванчики на камеру. Родители в шоке, просмотры растут.", img: "https://images.unsplash.com/photo-1527153857715-3908f2bae5e8?auto=format&fit=crop&w=400&q=80" }
            ]
        };

        /* =========================================
           ИНИЦИАЛИЗАЦИЯ И ЗАГРУЗКА
           ========================================= */
        window.onload = async () => {
            // Проверяем, авторизован ли пользователь локально (сохраняем сессию)
            const savedEmail = localStorage.getItem('murmansk_user_email');
            if (savedEmail) {
                await loadUserFromDB(savedEmail);
            }
            
            // Загружаем новости из облака
            await fetchNewsFromDB();
            renderNews();
            
            // Запускаем фоновые процессы
            createSnowflakes();
            setInterval(regenerateEnergy, 2000);
            
            // Запускаем синхронизатор КЛИКЕРА с БАЗОЙ (каждые 5 секунд)
            setInterval(syncGameDataWithCloud, 5000);

            // Клик по фону для эффекта "Муу"
            document.body.addEventListener('click', (e) => {
                if(e.target.tagName !== 'BUTTON' && e.target.tagName !== 'A' && e.target.closest('.glass-panel') === null) {
                    createFloatingText(e.clientX, e.clientY, "МУУУ!");
                }
            });
        };

        /* =========================================
           РАБОТА С БАЗОЙ ДАННЫХ (SUPABASE)
           ========================================= */
        
        // 1. Авторизация / Регистрация
        async function handleAuth(e) {
            e.preventDefault();
            const email = document.getElementById('auth-email').value.trim().toLowerCase();
            const name = document.getElementById('auth-name').value.trim();
            
            const btnText = document.getElementById('auth-btn-text');
            const btnSpinner = document.getElementById('auth-btn-spinner');
            
            btnText.innerText = "Подключение к БД...";
            btnSpinner.classList.remove('hidden');

            try {
                // Ищем пользователя
                const { data: existingUser, error: searchError } = await supabaseClient
                    .from('users')
                    .select('*')
                    .eq('email', email)
                    .single();

                if (existingUser) {
                    // Пользователь найден - входим
                    setupUser(existingUser);
                } else {
                    // Пользователь не найден - создаем нового
                    // Особое условие: админская почта
                    const isAdmin = email === 'mste747@gmail.com';
                    
                    const newUserObj = {
                        email: email,
                        name: name,
                        coins: 0,
                        energy: 1000,
                        is_admin: isAdmin
                    };

                    const { data: newUser, error: insertError } = await supabaseClient
                        .from('users')
                        .insert([newUserObj])
                        .select()
                        .single();

                    if (insertError) throw insertError;
                    if (newUser) setupUser(newUser);
                }
                
                // Сохраняем email для автологина
                localStorage.setItem('murmansk_user_email', email);
                
            } catch (err) {
                console.error("Ошибка авторизации:", err);
                alert("Ошибка подключения к Базе Данных. Убедитесь, что RLS отключен в Supabase.");
            } finally {
                btnText.innerText = "Войти в стадо";
                btnSpinner.classList.add('hidden');
            }
        }

        // 2. Тихий логин при обновлении страницы
        async function loadUserFromDB(email) {
            const { data, error } = await supabaseClient
                .from('users')
                .select('*')
                .eq('email', email)
                .single();
                
            if (data) setupUser(data);
        }

        function setupUser(userData) {
            state.user = userData;
            // Синхронизируем локальный кликер с данными из БД
            state.clicker.coins = userData.coins || 0;
            state.clicker.energy = userData.energy || 1000;
            state.clicker.lastSyncCoins = state.clicker.coins;
            state.clicker.lastSyncEnergy = state.clicker.energy;
            
            updateClickerUI();
            updateProfileUI();
            
            document.getElementById('auth-warning-clicker').classList.add('hidden');
        }

        // 3. Синхронизация кликов с Облаком
        async function syncGameDataWithCloud() {
            // Если пользователь не вошел, или данные не менялись - не нагружаем базу
            if (!state.user) return;
            if (state.clicker.coins === state.clicker.lastSyncCoins && state.clicker.energy === state.clicker.lastSyncEnergy) {
                return; 
            }

            const statusEl = document.getElementById('sync-status');
            statusEl.innerText = "⏳ Сохранение...";
            statusEl.classList.remove('text-gray-400');
            statusEl.classList.add('text-yellow-500');

            const { error } = await supabaseClient
                .from('users')
                .update({ 
                    coins: state.clicker.coins, 
                    energy: state.clicker.energy 
                })
                .eq('id', state.user.id);

            if (!error) {
                state.clicker.lastSyncCoins = state.clicker.coins;
                state.clicker.lastSyncEnergy = state.clicker.energy;
                
                statusEl.innerText = "☁️ Сохранено";
                statusEl.classList.remove('text-yellow-500');
                statusEl.classList.add('text-green-500');
                setTimeout(() => {
                    statusEl.innerText = "☁️ Синхронизировано";
                    statusEl.classList.remove('text-green-500');
                    statusEl.classList.add('text-gray-400');
                }, 2000);
            }
        }

        // 4. Загрузка Лидерборда из Облака
        async function fetchLeaderboard() {
            const container = document.getElementById('leaderboard-container');
            container.innerHTML = `<div class="text-center py-10 text-gray-400"><i class="fa-solid fa-spinner fa-spin text-3xl"></i><br>Загрузка из БД...</div>`;
            
            const { data, error } = await supabaseClient
                .from('users')
                .select('name, coins, is_admin')
                .order('coins', { ascending: false })
                .limit(10);

            if (error) {
                container.innerHTML = `<div class="text-center py-5 text-red-500 font-bold">Ошибка загрузки Топа. Проверьте БД.</div>`;
                return;
            }

            container.innerHTML = '';
            if (data.length === 0) {
                container.innerHTML = `<div class="text-center py-5 text-gray-500 font-bold">База пока пуста. Стань первым!</div>`;
                return;
            }

            data.forEach((user, index) => {
                let badge = '';
                if(index === 0) badge = '🥇';
                else if(index === 1) badge = '🥈';
                else if(index === 2) badge = '🥉';
                else badge = `<span class="text-gray-400 w-6 inline-block">${index + 1}</span>`;

                const crown = user.is_admin ? '<i class="fa-solid fa-crown text-yellow-500 text-xs ml-1" title="Мэр"></i>' : '';

                const el = document.createElement('div');
                el.className = 'flex items-center justify-between bg-white bg-opacity-60 p-3 rounded-xl border border-gray-100 shadow-sm';
                el.innerHTML = `
                    <div class="flex items-center gap-3 font-bold text-gray-800">
                        <span class="text-xl w-8 text-center">${badge}</span>
                        <span>${escapeHtml(user.name)} ${crown}</span>
                    </div>
                    <div class="font-black text-yellow-500 flex items-center gap-1">
                        ${user.coins.toLocaleString('ru-RU')} <i class="fa-solid fa-coins text-sm"></i>
                    </div>
                `;
                container.appendChild(el);
            });
        }

        // 5. Загрузка Новостей из Облака
        async function fetchNewsFromDB() {
            const { data, error } = await supabaseClient
                .from('news')
                .select('*')
                .order('created_at', { ascending: false });
                
            // Если таблица существует и в ней есть данные - заменяем дефолтные
            if (data && data.length > 0) {
                state.news = data;
            }
        }

        // 6. Добавление Новости в Облако (Админ)
        async function handlePostNews(e) {
            e.preventDefault();
            if (!state.user || !state.user.is_admin) return;

            const title = document.getElementById('admin-news-title').value;
            const text = document.getElementById('admin-news-text').value;
            const btnText = document.getElementById('admin-submit-text');
            const btnSpinner = document.getElementById('admin-submit-spinner');
            const successMsg = document.getElementById('admin-success');

            btnText.innerText = "Публикация...";
            btnSpinner.classList.remove('hidden');
            successMsg.classList.add('hidden');

            const { data, error } = await supabaseClient
                .from('news')
                .insert([{ 
                    title: title, 
                    text: text,
                    img: "https://images.unsplash.com/photo-1596733430284-f743727520d6?auto=format&fit=crop&w=400&q=80" // Дефолтная картинка для новых новостей
                }]);

            btnText.innerText = "Опубликовать в БД";
            btnSpinner.classList.add('hidden');

            if (!error) {
                successMsg.classList.remove('hidden');
                document.getElementById('admin-news-title').value = '';
                document.getElementById('admin-news-text').value = '';
                // Перезагружаем новости
                await fetchNewsFromDB();
                renderNews();
                setTimeout(() => successMsg.classList.add('hidden'), 3000);
            } else {
                alert("Ошибка публикации. Возможно, таблица 'news' не создана в Supabase.");
            }
        }


        /* =========================================
           ОБНОВЛЕНИЕ ИНТЕРФЕЙСА (UI)
           ========================================= */
        function updateProfileUI() {
            if (state.user) {
                document.getElementById('auth-form-container').classList.add('hidden');
                document.getElementById('user-dashboard').classList.remove('hidden');
                
                document.getElementById('profile-name').innerText = state.user.name;
                document.getElementById('profile-email').innerText = state.user.email;
                
                const roleSpan = document.getElementById('profile-role');
                const adminPanel = document.getElementById('admin-panel');
                
                if (state.user.is_admin) {
                    roleSpan.innerText = "👑 Мэр Муу-рманска (Админ)";
                    roleSpan.classList.replace('bg-gray-200', 'bg-red-100');
                    roleSpan.classList.replace('text-gray-700', 'text-red-700');
                    adminPanel.classList.remove('hidden');
                } else {
                    roleSpan.innerText = "Рядовая Корова";
                    roleSpan.classList.replace('bg-red-100', 'bg-gray-200');
                    roleSpan.classList.replace('text-red-700', 'text-gray-700');
                    adminPanel.classList.add('hidden');
                }
            } else {
                document.getElementById('auth-form-container').classList.remove('hidden');
                document.getElementById('user-dashboard').classList.add('hidden');
                document.getElementById('auth-warning-clicker').classList.remove('hidden');
            }
        }

        function logout() {
            state.user = null;
            localStorage.removeItem('murmansk_user_email');
            
            // Сбрасываем локальный кликер до 0
            state.clicker.coins = 0;
            state.clicker.energy = 1000;
            
            updateProfileUI();
            updateClickerUI();
        }

        /* =========================================
           НАВИГАЦИЯ (Вкладки)
           ========================================= */
        function switchView(viewId) {
            // Скрытие всех
            document.getElementById('view-news').classList.add('hidden-view');
            document.getElementById('view-clicker').classList.add('hidden-view');
            document.getElementById('view-leaderboard').classList.add('hidden-view');
            document.getElementById('view-translator').classList.add('hidden-view');
            document.getElementById('view-profile').classList.add('hidden-view');
            
            // Сброс кнопок
            document.querySelectorAll('.nav-btn').forEach(btn => btn.classList.remove('active'));
            
            // Показ нужной
            document.getElementById('view-' + viewId).classList.remove('hidden-view');
            document.getElementById('btn-' + viewId).classList.add('active');

            // Если открыли лидерборд - загружаем свежие данные
            if (viewId === 'leaderboard') {
                fetchLeaderboard();
            }
        }

        /* =========================================
           ИГРА: КЛИКЕР
           ========================================= */
        function clickCoin(e) {
            if (state.clicker.energy > 0) {
                state.clicker.coins++;
                state.clicker.energy--;
                
                updateClickerUI();

                // Визуальный эффект
                const rect = e.target.getBoundingClientRect();
                const x = e.clientX || rect.left + rect.width / 2;
                const y = e.clientY || rect.top + rect.height / 2;
                createFloatingText(x, y, "+1");
                
                // Вибрация (для мобилок)
                if (navigator.vibrate) navigator.vibrate(50);
            } else {
                createFloatingText(e.clientX, e.clientY, "Устал!");
            }
        }

        function getBonus() {
            if(state.bonusCooldown) return;
            
            state.clicker.coins += 50;
            state.clicker.energy = Math.min(state.clicker.energy + 100, state.clicker.maxEnergy);
            updateClickerUI();
            
            // Запуск кулдауна (60 сек)
            state.bonusCooldown = true;
            const btn = document.getElementById('btn-bonus');
            const timerText = document.getElementById('bonus-timer');
            
            btn.classList.add('opacity-50', 'cursor-not-allowed');
            timerText.classList.remove('hidden');
            
            let timeLeft = 60;
            const interval = setInterval(() => {
                timeLeft--;
                timerText.innerText = `До следующего бонуса: ${timeLeft} сек`;
                if(timeLeft <= 0) {
                    clearInterval(interval);
                    state.bonusCooldown = false;
                    btn.classList.remove('opacity-50', 'cursor-not-allowed');
                    timerText.classList.add('hidden');
                }
            }, 1000);
        }

        function updateClickerUI() {
            document.getElementById('coin-balance').innerText = state.clicker.coins.toLocaleString('ru-RU');
            document.getElementById('energy-text').innerText = `${state.clicker.energy} / ${state.clicker.maxEnergy}`;
            
            const percentage = (state.clicker.energy / state.clicker.maxEnergy) * 100;
            document.getElementById('energy-bar').style.width = percentage + '%';
        }

        function regenerateEnergy() {
            if (state.clicker.energy < state.clicker.maxEnergy) {
                state.clicker.energy = Math.min(state.clicker.energy + 2, state.clicker.maxEnergy);
                // Обновляем UI только если мы находимся на вкладке кликера (оптимизация)
                if(!document.getElementById('view-clicker').classList.contains('hidden-view')){
                    updateClickerUI();
                }
            }
        }

        function createFloatingText(x, y, text) {
            const el = document.createElement('div');
            el.className = 'floating-text';
            el.innerText = text;
            el.style.left = (x - 20) + 'px';
            el.style.top = (y - 20) + 'px';
            document.body.appendChild(el);
            setTimeout(() => el.remove(), 1000);
        }

        /* =========================================
           НОВОСТИ И ПЕРЕВОДЧИК
           ========================================= */
        function renderNews() {
            const container = document.getElementById('news-container');
            container.innerHTML = '';
            
            state.news.forEach(item => {
                const dateHtml = item.created_at ? `<div class="text-xs text-gray-400 mt-2"><i class="fa-regular fa-clock"></i> ${new Date(item.created_at).toLocaleDateString('ru-RU')}</div>` : '';
                
                const card = document.createElement('div');
                card.className = 'glass-panel rounded-2xl overflow-hidden news-card flex flex-col h-full';
                card.innerHTML = `
                    <div class="h-48 bg-cover bg-center" style="background-image: url('${item.img}')"></div>
                    <div class="p-5 flex-grow flex flex-col justify-between">
                        <div>
                            <h3 class="text-xl font-black text-gray-800 mb-2">${escapeHtml(item.title)}</h3>
                            <p class="text-gray-600 font-medium leading-relaxed">${escapeHtml(item.text)}</p>
                        </div>
                        ${dateHtml}
                    </div>
                `;
                container.appendChild(card);
            });
        }

        function translateToMoo() {
            const text = document.getElementById('human-text').value;
            const cowBox = document.getElementById('cow-text');
            if(!text) {
                cowBox.innerText = "МУУ... (введи текст)";
                return;
            }
            const words = text.split(' ');
            const mooTranslation = words.map(w => {
                const len = w.length;
                if(len <= 3) return "Му";
                if(len <= 5) return "Мууу";
                return "МУУУУУ!";
            }).join(' ');
            
            cowBox.innerText = mooTranslation;
        }

        /* =========================================
           УТИЛИТЫ И ЭФФЕКТЫ
           ========================================= */
        function createSnowflakes() {
            const chars = ['❄', '❅', '❆'];
            setInterval(() => {
                const el = document.createElement('div');
                el.className = 'snowflake';
                el.innerText = chars[Math.floor(Math.random() * chars.length)];
                el.style.left = Math.random() * 100 + 'vw';
                el.style.animationDuration = (Math.random() * 3 + 5) + 's';
                el.style.opacity = Math.random() * 0.5 + 0.3;
                document.body.appendChild(el);
                setTimeout(() => el.remove(), 8000);
            }, 300);
        }

        function escapeHtml(unsafe) {
            if(!unsafe) return '';
            return unsafe
                 .replace(/&/g, "&amp;")
                 .replace(/</g, "&lt;")
                 .replace(/>/g, "&gt;")
                 .replace(/"/g, "&quot;")
                 .replace(/'/g, "&#039;");
        }
    </script>
</body>
</html>
