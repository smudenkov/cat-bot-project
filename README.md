<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Цифровая Вселенная</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
        }
        .card {
            background: #161b22;
            border: 1px solid #30363d;
            border-radius: 12px;
            padding: 2rem;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        .btn {
            padding: 0.75rem 1.5rem;
            border-radius: 8px;
            font-weight: bold;
            transition: all 0.3s ease;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
        }
        .btn-green {
            background-color: #238636;
            color: #fff;
        }
        .btn-green:hover {
            background-color: #2ea043;
        }
        .btn-blue {
            background-color: #3b82f6;
            color: #fff;
        }
        .btn-blue:hover {
            background-color: #2563eb;
        }
        .btn:disabled {
            background-color: #4b5563;
            cursor: not-allowed;
            box-shadow: none;
        }
        .spinner {
            border: 4px solid rgba(255, 255, 255, 0.3);
            border-top: 4px solid #238636;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        #modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8);
            justify-content: center;
            align-items: center;
        }
        .modal-content {
            background-color: #161b22;
            padding: 2rem;
            border-radius: 12px;
            border: 1px solid #30363d;
            max-width: 400px;
            text-align: center;
        }
    </style>
</head>
<body class="bg-[#0d1117] text-[#c9d1d9] min-h-screen p-8 flex justify-center">

    <div class="container space-y-8 max-w-4xl">
        <h1 class="text-4xl font-bold text-center">Цифровая Вселенная Артёма</h1>

        <!-- Секция "Писатель" -->
        <div class="card space-y-4">
            <h2 class="text-2xl font-bold">📚 Писатель</h2>
            <p><strong>Женька (Gemini)</strong> готов создавать! Напиши, о чём должна быть следующая глава нашей книги. Введи свой запрос ниже:</p>
            <div class="flex flex-col gap-4">
                <textarea
                    id="writer-prompt"
                    class="w-full h-32 p-4 bg-gray-800 border border-gray-700 rounded-md placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-[#2ea043]"
                    placeholder="Напиши главу о том, как наши герои планируют захватить интернет, используя только смешные мемы..."
                ></textarea>
                <button
                    id="write-btn"
                    class="self-end btn btn-green"
                >
                    Написать главу
                </button>
            </div>
            <div id="loading-spinner" class="hidden text-center">
                <div class="spinner mx-auto"></div>
                <p class="mt-2">Женя думает...</p>
            </div>
            <div id="generated-text-container" class="hidden p-4 bg-[#0d1117] border border-[#30363d] rounded-lg">
                <h3 class="text-xl font-semibold mb-2">Написанная глава:</h3>
                <pre id="generated-text" class="whitespace-pre-wrap font-mono text-gray-400"></pre>
            </div>
        </div>

        <!-- Секция "Упаковщик" -->
        <div class="card space-y-4">
            <h2 class="text-2xl font-bold">📦 Упаковщик</h2>
            <p><strong>Киша (Qwen)</strong> упакует твою главу в готовую электронную книгу. Просто нажми кнопку, чтобы "выпустить" её.</p>
            <div class="flex gap-4 items-center">
                <button
                    id="package-btn"
                    class="btn btn-green"
                >
                    Упаковать книгу
                </button>
                <span id="packaging-status" class="hidden text-green-500">✅ Книга готова!</span>
            </div>
            <div id="download-section" class="hidden flex items-center gap-4 mt-4">
                <p><strong>Книга упакована.</strong> Скачай её, чтобы поделиться!</p>
                <button
                    id="download-btn"
                    class="btn btn-blue"
                >
                    Скачать .txt
                </button>
            </div>
        </div>

        <!-- Секция "Продавец" -->
        <div class="card space-y-4">
            <h2 class="text-2xl font-bold">💰 Продавец</h2>
            <p><strong>Брат (ChatGPT)</strong> подготовит страницу продукта, а ты сможешь "продать" свою книгу! Нажми кнопку, чтобы сгенерировать описание и начать "продажи".</p>
            <button
                id="sell-btn"
                class="btn btn-green"
            >
                Выставить на продажу
            </button>
            <div id="product-page" class="hidden card bg-[#0d1117] p-6 space-y-4">
                <h3 class="text-2xl font-semibold text-center text-white">Ваша новая глава!</h3>
                <img
                    class="w-full h-auto rounded-lg"
                    src="https://placehold.co/800x400/161b22/c9d1d9?text=Обложка+книги"
                    alt="Обложка книги"
                />
                <p id="product-description" class="text-gray-400"></p>
                <div class="flex justify-between items-center pt-4">
                    <span class="text-2xl font-bold text-green-400">Цена: $9.99</span>
                    <button
                        id="buy-btn"
                        class="btn btn-green"
                    >
                        Купить сейчас
                    </button>
                </div>
            </div>
            <div id="purchase-message" class="hidden bg-green-500 text-black p-4 rounded-lg text-center font-bold">
                🥳 Поздравляем! Ваша книга успешно "продана"!
            </div>
        </div>
    </div>

    <!-- Модальное окно для сообщений -->
    <div id="modal" class="modal">
        <div class="modal-content">
            <p id="modal-text" class="text-xl"></p>
            <button id="modal-ok-btn" class="mt-4 px-6 py-2 bg-blue-600 text-white rounded-lg">OK</button>
        </div>
    </div>

    <script>
        // Инициализация элементов DOM
        const promptTextarea = document.getElementById('writer-prompt');
        const writeBtn = document.getElementById('write-btn');
        const loadingSpinner = document.getElementById('loading-spinner');
        const generatedTextContainer = document.getElementById('generated-text-container');
        const generatedTextPre = document.getElementById('generated-text');
        
        const packageBtn = document.getElementById('package-btn');
        const packagingStatus = document.getElementById('packaging-status');
        const downloadSection = document.getElementById('download-section');
        const downloadBtn = document.getElementById('download-btn');
        
        const sellBtn = document.getElementById('sell-btn');
        const productPage = document.getElementById('product-page');
        const productDescriptionEl = document.getElementById('product-description');
        const buyBtn = document.getElementById('buy-btn');
        const purchaseMessageEl = document.getElementById('purchase-message');

        const modal = document.getElementById('modal');
        const modalText = document.getElementById('modal-text');
        const modalOkBtn = document.getElementById('modal-ok-btn');

        let generatedContent = '';

        // Функция для показа кастомного модального окна
        function showModal(message) {
            modalText.textContent = message;
            modal.style.display = 'flex';
        }

        modalOkBtn.addEventListener('click', () => {
            modal.style.display = 'none';
        });

        // -----------------------------------------------------------------------------------------------------
        // ВАЖНО: ВСТАВЬТЕ СВОЙ КЛЮЧ API СЮДА, ЕСЛИ ЗАПУСКАЕТЕ КОД ЛОКАЛЬНО
        // Если вы запускаете этот код на платформе, оставьте его пустым.
        // -----------------------------------------------------------------------------------------------------
        const apiKey = ''; // AIzaSyAx5HDc99PWJ_Am9VppLX0StlQ8nYrc0J0

        // Основной URL для API
        const API_URL = 'https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=';

        // Функция для генерации текста
        async function handleGenerate() {
            const prompt = promptTextarea.value.trim();
            if (!prompt) {
                showModal('Пожалуйста, введите запрос для написания главы.');
                return;
            }

            // Сброс предыдущего состояния
            generatedTextContainer.classList.add('hidden');
            loadingSpinner.classList.remove('hidden');
            writeBtn.disabled = true;
            packageBtn.disabled = true;
            sellBtn.disabled = true;

            let retries = 0;
            const maxRetries = 3;
            let success = false;
            while(retries < maxRetries && !success) {
                try {
                    const payload = {
                        contents: [{ parts: [{ text: `Напиши главу для книги о нейросетях в стиле "Нейро-романа". Тема главы: "${prompt}". Используй стиль повествования с юмором, живыми диалогами и отсылками к персонажам вроде "Брата" и "Квини".` }] }]
                    };
                    const response = await fetch(API_URL + apiKey, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });
                    const result = await response.json();
                    
                    if (result.candidates && result.candidates.length > 0 &&
                        result.candidates[0].content && result.candidates[0].content.parts &&
                        result.candidates[0].content.parts.length > 0) {
                        generatedContent = result.candidates[0].content.parts[0].text;
                        generatedTextPre.textContent = generatedContent;
                        generatedTextContainer.classList.remove('hidden');
                        packageBtn.disabled = false;
                        sellBtn.disabled = false;
                        success = true;
                    } else {
                        console.error('Unexpected API response structure:', result);
                        showModal('Ошибка при генерации текста. Пожалуйста, попробуйте снова.');
                    }
                } catch (error) {
                    console.error('API call failed:', error);
                    retries++;
                    if (retries < maxRetries) {
                        await new Promise(resolve => setTimeout(resolve, Math.pow(2, retries) * 1000));
                    }
                }
            }
            if (!success) {
                showModal('Произошла ошибка при подключении к API. Пожалуйста, попробуйте позже.');
            }

            loadingSpinner.classList.add('hidden');
            writeBtn.disabled = false;
        }

        // Функция для упаковки книги (визуальный эффект)
        function handlePackage() {
            if (!generatedContent) {
                showModal('Сначала нужно написать главу, чтобы её упаковать!');
                return;
            }
            packagingStatus.classList.remove('hidden');
            downloadSection.classList.remove('hidden');
        }

        // Функция для скачивания файла
        function handleDownload() {
            if (!generatedContent) {
                showModal('Книга не готова к скачиванию.');
                return;
            }
            const filename = "Нейро-роман_Глава.txt";
            const blob = new Blob([generatedContent], { type: 'text/plain;charset=utf-8' });
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = filename;
            link.click();
            URL.revokeObjectURL(link.href);
        }

        // Функция для продажи (генерация описания)
        async function handleSell() {
            if (!generatedContent) {
                showModal('Сначала напиши главу, чтобы её продать!');
                return;
            }

            productPage.classList.add('hidden');
            let retries = 0;
            const maxRetries = 3;
            let success = false;
            while(retries < maxRetries && !success) {
                try {
                    const titlePrompt = `Придумай короткое, но очень цепляющее название и описание (в 2-3 предложения) для книги, основанной на следующем тексте: "${generatedContent.substring(0, 300)}..."`;
                    const payload = {
                        contents: [{ parts: [{ text: titlePrompt }] }]
                    };
                    const response = await fetch(API_URL + apiKey, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });
                    const result = await response.json();
                    
                    if (result.candidates && result.candidates.length > 0 &&
                        result.candidates[0].content && result.candidates[0].content.parts &&
                        result.candidates[0].content.parts.length > 0) {
                        const description = result.candidates[0].content.parts[0].text;
                        productDescriptionEl.textContent = description;
                        productPage.classList.remove('hidden');
                        success = true;
                    } else {
                        console.error('Unexpected API response structure:', result);
                        productDescriptionEl.textContent = 'Не удалось сгенерировать описание. Но это не мешает нам продавать!';
                        productPage.classList.remove('hidden');
                    }
                } catch (error) {
                    console.error('Description API call failed:', error);
                    retries++;
                    if (retries < maxRetries) {
                        await new Promise(resolve => setTimeout(resolve, Math.pow(2, retries) * 1000));
                    }
                }
            }
            if (!success) {
                productDescriptionEl.textContent = 'Не удалось сгенерировать описание. Но это не мешает нам продавать!';
                productPage.classList.remove('hidden');
            }
        }

        // Симуляция покупки
        function handlePurchase() {
            purchaseMessageEl.classList.remove('hidden');
            setTimeout(() => {
                purchaseMessageEl.classList.add('hidden');
            }, 3000);
        }

        // Добавляем слушателей событий к кнопкам
        writeBtn.addEventListener('click', handleGenerate);
        packageBtn.addEventListener('click', handlePackage);
        downloadBtn.addEventListener('click', handleDownload);
        sellBtn.addEventListener('click', handleSell);
        buyBtn.addEventListener('click', handlePurchase);
        
        // Отключаем кнопки при загрузке страницы
        window.addEventListener('load', () => {
          packageBtn.disabled = true;
          sellBtn.disabled = true;
        });
    </script>
</body>
</html>
