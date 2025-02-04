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
            margin: 0;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
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
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 15px;
            margin-bottom: 80px;
        }

        .product-card {
            background: #1a1a1a;
            border-radius: 12px;
            padding: 15px;
            position: relative;
            display: flex;
            flex-direction: column;
        }

        .product-image-container {
            position: relative;
            width: 100%;
            height: 200px;
            overflow: hidden;
            border-radius: 8px;
            margin-bottom: 10px;
        }

        .image-slider {
            display: flex;
            transition: transform 0.5s ease;
            height: 100%;
        }

        .product-image {
            min-width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .slider-controls {
            position: absolute;
            bottom: 10px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 5px;
        }

        .slider-dot {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            background: rgba(255,255,255,0.5);
            border: none;
            cursor: pointer;
            padding: 0;
        }

        .slider-dot.active {
            background: #007bff;
        }

        .prev-next-buttons {
            position: absolute;
            top: 50%;
            width: 100%;
            display: flex;
            justify-content: space-between;
            transform: translateY(-50%);
            padding: 0 10px;
            pointer-events: none;
        }

        .slide-button {
            background: rgba(0,0,0,0.5);
            border: none;
            color: white;
            padding: 8px 12px;
            border-radius: 50%;
            cursor: pointer;
            pointer-events: all;
        }

        .price {
            color: #00ff00;
            font-weight: bold;
            margin: 10px 0;
        }

        button {
            background: #007bff;
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 6px;
            cursor: pointer;
            transition: opacity 0.3s;
        }

        button:hover {
            opacity: 0.9;
        }

        .cart-button {
            position: fixed;
            bottom: 20px;
            right: 20px;
            padding: 12px;
            border-radius: 50%;
            width: 50px;
            height: 50px;
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
            position: relative;
        }

        .delivery-form input {
            width: 100%;
            padding: 10px;
            margin: 8px 0;
            background: #333;
            border: 1px solid #444;
            color: #fff;
            border-radius: 4px;
        }

        .form-navigation {
            display: flex;
            gap: 10px;
            margin-top: 20px;
        }

        .admin-controls {
            display: flex;
            gap: 8px;
            margin-top: 10px;
        }

        .delete-btn {
            background: #dc3545 !important;
        }

        .edit-btn {
            background: #ffc107 !important;
        }
    </style>
</head>
<body>
    <button class="admin-mode-btn" onclick="toggleAdminMode()">👑</button>
    <h1>F 63.9</h1>

    <div class="categories" id="categories">
        <button class="category-btn active" onclick="showCategory('all')">Все</button>
        <button class="category-btn" onclick="showCategory('Колье')">Колье</button>
        <button class="category-btn" onclick="showCategory('Чокеры')">Чокеры</button>
        <button class="category-btn" onclick="showCategory('Серьги')">Серьги</button>
        <button class="category-btn" onclick="showCategory('Кольца')">Кольца</button>
        <button class="category-btn" onclick="showCategory('Браслеты')">Браслеты</button>
        <button class="category-btn" onclick="showCategory('Сеты')">Сеты</button>
    </div>

    <button class="add-product-btn" onclick="showAdminPanel()">+</button>
    <div class="admin-panel" id="adminPanel">
        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px;">
            <h3 id="adminPanelTitle">Добавить товар</h3>
            <button onclick="hideAdminPanel()" style="background: none; border: none; color: #fff; font-size: 1.5em;">×</button>
        </div>
        <form class="admin-form" onsubmit="saveProduct(event)">
            <select id="categorySelect" required>
                <option value="">Выберите категорию</option>
                <option>Колье</option>
                <option>Чокеры</option>
                <option>Серьги</option>
                <option>Кольца</option>
                <option>Браслеты</option>
                <option>Сеты</option>
            </select>
            <input type="text" id="productName" placeholder="Название товара" required>
            <input type="text" id="productDescription" placeholder="Описание" required>
            <input type="number" id="productPrice" placeholder="Цена в рублях" min="1" required>
            <input type="file" id="productImage" accept="image/*" multiple>
            <button type="submit" style="margin-top: 10px;">Сохранить</button>
        </form>
    </div>

    <div class="product-grid" id="productGrid"></div>

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
            <button onclick="nextStep()" style="width: 100%; margin-top: 15px;">Далее</button>
        </div>
    </div>

    <div class="cart-modal" id="deliveryModal">
        <div class="cart-content">
            <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px;">
                <h2>Оформление заказа</h2>
                <button onclick="closeDeliveryForm()" style="background: none; border: none; color: #fff; font-size: 1.5em;">×</button>
            </div>
            <form class="delivery-form" onsubmit="submitOrder(event)">
                <input type="text" id="clientName" placeholder="Ваше имя" required>
                <input type="tel" id="clientPhone" placeholder="Номер телефона" required>
                <input type="text" id="clientAddress" placeholder="Адрес доставки" required>
                <div class="form-navigation">
                    <button type="button" onclick="backToCart()" style="width: 100px;">Назад</button>
                    <button type="submit" style="flex-grow: 1;">Подтвердить заказ</button>
                </div>
            </form>
        </div>
    </div>

    <script>
        let products = JSON.parse(localStorage.getItem('products')) || [];
        let cart = JSON.parse(localStorage.getItem('cart')) || [];
        let currentCategory = 'all';
        let isAdminMode = false;
        let editingProductId = null;

        function init() {
            cart = cart.map(item => ({
                ...item,
                id: Number(item.id),
                price: Number(item.price),
                quantity: Number(item.quantity) || 1
            }));
            
            products = products.map(product => ({
                ...product,
                id: Number(product.id),
                price: Number(product.price),
                images: product.images || []
            }));

            if (typeof Telegram !== 'undefined' && Telegram.WebApp) {
                Telegram.WebApp.ready();
                Telegram.WebApp.expand();
            }
            showCategory('all');
            renderProducts();
            updateCart();
        }

        function addToCart(productId) {
            const product = products.find(p => p.id === productId);
            if (!product) return;

            const existingItem = cart.find(item => item.id === productId);
            if (existingItem) {
                existingItem.quantity += 1;
            } else {
                cart.push({...product, quantity: 1, id: productId});
            }
            saveCart();
            updateCart();
        }

        function updateCart() {
            const cartItems = document.getElementById('cart-items');
            const totalElement = document.getElementById('total');
            let total = 0;

            cartItems.innerHTML = cart.map(item => {
                const itemTotal = (item.price / 100) * item.quantity;
                total += itemTotal;
                return `
                    <div class="cart-item">
                        <div>
                            <strong>${item.name}</strong><br>
                            ${item.quantity} × ${(item.price / 100).toLocaleString('ru-RU')} ₽ = ${itemTotal.toLocaleString('ru-RU')} ₽
                        </div>
                        <button class="delete-btn" onclick="removeFromCart(${item.id})">❌</button>
                    </div>
                `;
            }).join('');

            totalElement.textContent = total.toLocaleString('ru-RU');
        }

        function nextStep() {
            if (cart.length === 0) return alert('Корзина пуста!');
            document.getElementById('cartModal').style.display = 'none';
            document.getElementById('deliveryModal').style.display = 'block';
        }

        function submitOrder(e) {
            e.preventDefault();
            const orderData = {
                name: document.getElementById('clientName').value.trim(),
                phone: document.getElementById('clientPhone').value.trim(),
                address: document.getElementById('clientAddress').value.trim(),
                cart: cart,
                total: document.getElementById('total').textContent
            };

            if (!validateOrder(orderData)) return;

            console.log('Order Data:', orderData);
            alert('Заказ успешно оформлен!');
            
            cart = [];
            saveCart();
            closeDeliveryForm();
            updateCart();
        }

        function validateOrder(data) {
            const phoneRegex = /^(\+7|8)[\s\-]?\(?\d{3}\)?[\s\-]?\d{3}[\s\-]?\d{2}[\s\-]?\d{2}$/;
            if (!data.name) return alert('Введите ваше имя');
            if (!phoneRegex.test(data.phone)) return alert('Введите корректный номер телефона');
            if (!data.address) return alert('Укажите адрес доставки');
            return true;
        }

        function removeFromCart(productId) {
            cart = cart.filter(item => item.id !== productId);
            saveCart();
            updateCart();
        }

        function showCategory(category) {
            currentCategory = category;
            document.querySelectorAll('.category-btn').forEach(btn => {
                btn.classList.toggle('active', btn.textContent === category || 
                    (category === 'all' && btn.textContent === 'Все'));
            });
            renderProducts();
        }

        function renderProducts() {
            const filteredProducts = currentCategory === 'all' 
                ? products 
                : products.filter(p => p.category === currentCategory);
            
            const productGrid = document.getElementById('productGrid');
            productGrid.innerHTML = filteredProducts.map(product => `
                <div class="product-card">
                    <div class="product-image-container">
                        <div class="image-slider" id="slider-${product.id}">
                            ${product.images.map(img => `
                                <img src="${img}" class="product-image" alt="${product.name}">
                            `).join('')}
                        </div>
                        ${product.images.length > 1 ? `
                            <div class="prev-next-buttons">
                                <button class="slide-button" onclick="slideImage(${product.id}, -1)">❮</button>
                                <button class="slide-button" onclick="slideImage(${product.id}, 1)">❯</button>
                            </div>
                            <div class="slider-controls" id="dots-${product.id}">
                                ${product.images.map((_, index) => `
                                    <button class="slider-dot ${index === 0 ? 'active' : ''}" 
                                        onclick="showSlide(${product.id}, ${index})"></button>
                                `).join('')}
                            </div>
                        ` : ''}
                    </div>
                    <h3>${product.name}</h3>
                    <p>${product.description}</p>
                    <p class="price">${(product.price/100).toLocaleString('ru-RU')} ₽</p>
                    <button onclick="addToCart(${product.id})">В корзину</button>
                    ${isAdminMode ? `
                    <div class="admin-controls">
                        <button class="edit-btn" onclick="editProduct(${product.id})">✏️</button>
                        <button class="delete-btn" onclick="deleteProduct(${product.id})">🗑️</button>
                    </div>` : ''}
                </div>
            `).join('');
        }

        function slideImage(productId, direction) {
            const slider = document.getElementById(`slider-${productId}`);
            const dots = document.getElementById(`dots-${productId}`);
            const slidesCount = slider.children.length;
            const currentSlide = Math.round(slider.scrollLeft / slider.offsetWidth);
            let newSlide = currentSlide + direction;

            if (newSlide < 0) newSlide = slidesCount - 1;
            if (newSlide >= slidesCount) newSlide = 0;

            slider.scrollTo({
                left: newSlide * slider.offsetWidth,
                behavior: 'smooth'
            });

            dots.querySelectorAll('.slider-dot').forEach((dot, index) => {
                dot.classList.toggle('active', index === newSlide);
            });
        }

        function showSlide(productId, index) {
            const slider = document.getElementById(`slider-${productId}`);
            const dots = document.getElementById(`dots-${productId}`);
            
            slider.scrollTo({
                left: index * slider.offsetWidth,
                behavior: 'smooth'
            });

            dots.querySelectorAll('.slider-dot').forEach((dot, i) => {
                dot.classList.toggle('active', i === index);
            });
        }

        function toggleAdminMode() {
            isAdminMode = !isAdminMode;
            document.querySelector('.admin-mode-btn').classList.toggle('active', isAdminMode);
            renderProducts();
        }

        function deleteProduct(productId) {
            if (!confirm('Удалить товар?')) return;
            products = products.filter(p => p.id !== productId);
            saveProducts();
            renderProducts();
        }

        function editProduct(productId) {
            const product = products.find(p => p.id === productId);
            if (!product) return;
            
            editingProductId = productId;
            document.getElementById('categorySelect').value = product.category;
            document.getElementById('productName').value = product.name;
            document.getElementById('productDescription').value = product.description;
            document.getElementById('productPrice').value = (product.price/100).toFixed(2);
            document.getElementById('adminPanelTitle').textContent = 'Редактировать товар';
            showAdminPanel();
        }

        function saveProduct(e) {
            e.preventDefault();
            const form = e.target;
            const price = parseFloat(form.productPrice.value);
            
            if (!validateForm(price)) return;

            const files = Array.from(document.getElementById('productImage').files);
            const readers = files.map(file => {
                return new Promise((resolve) => {
                    const reader = new FileReader();
                    reader.onload = (e) => resolve(e.target.result);
                    reader.readAsDataURL(file);
                });
            });

            Promise.all(readers).then(newImages => {
                const productData = {
                    id: editingProductId || Date.now(),
                    category: document.getElementById('categorySelect').value,
                    name: document.getElementById('productName').value.trim(),
                    description: document.getElementById('productDescription').value.trim(),
                    price: price * 100,
                    images: newImages.length > 0 ? newImages : 
                        (products.find(p => p.id === editingProductId)?.images || [])
                };

                if (editingProductId) {
                    const index = products.findIndex(p => p.id === editingProductId);
                    if (index === -1) return alert('Товар не найден');
                    products[index] = productData;
                } else {
                    products.push(productData);
                }

                saveProducts();
                hideAdminPanel();
                form.reset();
                renderProducts();
            });
        }

        function validateForm(price) {
            if (isNaN(price) || price <= 0) {
                alert('Введите корректную цену');
                return false;
            }
            if (!document.getElementById('productName').value.trim()) {
                alert('Введите название товара');
                return false;
            }
            return true;
        }

        function showAdminPanel() {
            document.getElementById('adminPanel').style.display = 'block';
        }

        function hideAdminPanel() {
            document.getElementById('adminPanel').style.display = 'none';
            document.getElementById('adminPanelTitle').textContent = 'Добавить товар';
            document.querySelector('.admin-form').reset();
            editingProductId = null;
        }

        function openCart() {
            document.getElementById('cartModal').style.display = 'block';
        }

        function closeCart() {
            document.getElementById('cartModal').style.display = 'none';
        }

        function backToCart() {
            document.getElementById('deliveryModal').style.display = 'none';
            document.getElementById('cartModal').style.display = 'block';
        }

        function closeDeliveryForm() {
            document.getElementById('deliveryModal').style.display = 'none';
        }

        function saveProducts() {
            localStorage.setItem('products', JSON.stringify(products));
        }

        function saveCart() {
            localStorage.setItem('cart', JSON.stringify(cart));
        }

        init();
    </script>
</body>
</html>
