<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>F 63.9</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 20px;
            background: #000;
            color: #fff;
            min-height: 100vh;
        }
        h1 {
            text-align: center;
            font-size: 2em;
            margin-bottom: 20px;
        }
        .categories {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-bottom: 20px;
        }
        .category-btn {
            background: #333;
            color: #fff;
            border: none;
            padding: 10px 20px;
            border-radius: 20px;
            cursor: pointer;
            transition: background 0.3s;
        }
        .category-btn.active {
            background: #007bff;
        }
        .product-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            margin-bottom: 80px;
        }
        .product-card {
            background: #1a1a1a;
            border-radius: 12px;
            padding: 15px;
            box-shadow: 0 2px 4px rgba(255,255,255,0.1);
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .product-image {
            width: 100%;
            height: 150px;
            object-fit: cover;
            border-radius: 8px;
            margin-bottom: 10px;
        }
        .price {
            color: #00ff00;
            font-weight: bold;
            font-size: 1.1em;
            margin: 10px 0;
        }
        button {
            background: #007bff;
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 6px;
            cursor: pointer;
            width: 100%;
            transition: opacity 0.3s;
        }
        button:hover {
            opacity: 0.8;
        }
        .cart-button {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: #007bff;
            color: white;
            border: none;
            padding: 8px;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 2px 10px rgba(255,255,255,0.2);
        }
        .add-product-btn {
            position: fixed;
            bottom: 70px;
            right: 20px;
            background: #28a745;
            color: white;
            border: none;
            padding: 8px;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 2px 10px rgba(255,255,255,0.2);
        }
        .admin-panel {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: #1a1a1a;
            padding: 20px;
            border-radius: 12px;
            z-index: 1001;
            width: 90%;
            max-width: 500px;
            color: #fff;
        }
        .admin-form input {
            width: 100%;
            padding: 10px;
            margin: 5px 0;
            background: #333;
            border: 1px solid #444;
            color: #fff;
            border-radius: 4px;
        }
        .cart-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.9);
            z-index: 1000;
        }
        .cart-content {
            background: #1a1a1a;
            width: 90%;
            max-width: 400px;
            margin: 20px auto;
            padding: 20px;
            border-radius: 12px;
            max-height: 80vh;
            overflow-y: auto;
            color: #fff;
        }
        ::placeholder {
            color: #ccc !important;
        }
    </style>
</head>
<body>
    <h1>F 63.9</h1>

    <!-- Категории -->
    <div class="categories" id="categories">
        <button class="category-btn active" onclick="showCategory('all')">Все</button>
        <button class="category-btn" onclick="showCategory('Колье')">Колье</button>
        <button class="category-btn" onclick="showCategory('Чокеры')">Чокеры</button>
        <button class="category-btn" onclick="showCategory('Серьги')">Серьги</button>
        <button class="category-btn" onclick="showCategory('Кольца')">Кольца</button>
        <button class="category-btn" onclick="showCategory('Браслеты')">Браслеты</button>
        <button class="category-btn" onclick="showCategory('Сеты')">Сеты</button>
    </div>

    <!-- Админ-панель -->
    <button class="add-product-btn" onclick="showAdminPanel()">+</button>
    <div class="admin-panel" id="adminPanel">
        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px;">
            <h3>Добавить товар</h3>
            <button onclick="hideAdminPanel()" style="background: none; border: none; color: #fff; font-size: 1.5em;">×</button>
        </div>
        <form class="admin-form" onsubmit="addNewProduct(event)">
            <select id="categorySelect" required>
                <option value="">Выберите категорию</option>
                <option>Колье</option>
                <option>Чокеры</option>
                <option>Серьги</option>
                <option>Кольца</option>
                <option>Браслеты</option>
                <option>Сеты</option>
            </select>
            <input type="text" placeholder="Название товара" required>
            <input type="text" placeholder="Описание" required>
            <input type="number" placeholder="Цена в рублях" required>
            <input type="file" accept="image/*" required>
            <button type="submit" style="margin-top: 10px;">Добавить</button>
        </form>
    </div>

    <!-- Сетка товаров -->
    <div class="product-grid" id="productGrid"></div>

    <!-- Корзина -->
    <button class="cart-button" onclick="openCart()">🛒</button>
    <div class="cart-modal" id="cartModal">
        <div class="cart-content">
            <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px;">
                <h2>Корзина</h2>
                <button onclick="closeCart()" style="background: none; border: none; color: #fff; font-size: 1.5em;">×</button>
            </div>
            <div id="cart-items"></div>
            <p style="text-align: right; margin-top: 20px;">
                Итого: <span class="price" id="total">0 ₽</span>
            </p>
            <button onclick="checkout()" style="width: 100%; margin-top: 15px;">Оформить заказ</button>
        </div>
    </div>

    <script>
        const BOT_TOKEN = 'ВАШ_BOT_TOKEN';
        const ADMIN_CHAT_ID = 'ВАШ_CHAT_ID';
        let products = JSON.parse(localStorage.getItem('products')) || [];
        let cart = [];
        let lastOrderTime = 0;
        let currentCategory = 'all';

        function showCategory(category) {
            currentCategory = category;
            document.querySelectorAll('.category-btn').forEach(btn => {
                btn.classList.toggle('active', btn.textContent === category || 
                    (category === 'all' && btn.textContent === 'Все'));
            });
            renderProducts();
        }

        function showAdminPanel() {
            document.getElementById('adminPanel').style.display = 'block';
        }

        function hideAdminPanel() {
            document.getElementById('adminPanel').style.display = 'none';
        }

        function addNewProduct(event) {
            event.preventDefault();
            const form = event.target;
            const [categorySelect, nameInput, descInput, priceInput, fileInput] = form.elements;
            
            const reader = new FileReader();
            reader.onload = function(e) {
                const newProduct = {
                    id: Date.now(),
                    category: categorySelect.value,
                    name: nameInput.value,
                    description: descInput.value,
                    price: parseInt(priceInput.value) * 100,
                    image: e.target.result
                };
                
                products.push(newProduct);
                localStorage.setItem('products', JSON.stringify(products));
                renderProducts();
                hideAdminPanel();
                form.reset();
            };
            reader.readAsDataURL(fileInput.files[0]);
        }

        function renderProducts() {
            const productGrid = document.getElementById('productGrid');
            const filteredProducts = currentCategory === 'all' ? 
                products : 
                products.filter(p => p.category === currentCategory);
            
            productGrid.innerHTML = filteredProducts.map(product => `
                <div class="product-card" id="product${product.id}">
                    <img src="${product.image}" class="product-image" alt="${product.name}">
                    <h3>${product.name}</h3>
                    <p>${product.description}</p>
                    <p class="price">${(product.price/100).toLocaleString('ru-RU')} ₽</p>
                    <button onclick="addToCart('product${product.id}')">В корзину</button>
                </div>
            `).join('');
        }

        // ... остальные функции без изменений ...

        // Инициализация
        function init() {
            Telegram.WebApp.ready();
            Telegram.WebApp.expand();
            renderProducts();
        }

        init();
    </script>
</body>
</html>
