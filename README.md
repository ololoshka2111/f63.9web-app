<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>F 63.9</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        /* Все предыдущие стили остаются без изменений */
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
            position: relative;
        }
        .carousel {
            position: relative;
            width: 100%;
            height: 150px;
            overflow: hidden;
            border-radius: 8px;
            margin-bottom: 10px;
        }
        .carousel-inner {
            display: flex;
            transition: transform 0.5s ease;
            height: 100%;
        }
        .carousel-item {
            min-width: 100%;
            height: 100%;
            object-fit: cover;
        }
        .carousel-dots {
            position: absolute;
            bottom: 5px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 5px;
        }
        .carousel-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: rgba(255,255,255,0.5);
            cursor: pointer;
        }
        .carousel-dot.active {
            background: #fff;
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
        .admin-form input, .admin-form select {
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
        .admin-mode-btn {
            position: fixed;
            top: 20px;
            left: 20px;
            background: #333;
            color: #fff;
            border: none;
            padding: 10px;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            cursor: pointer;
            z-index: 1000;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 2px 10px rgba(255,255,255,0.2);
        }
        .delete-btn {
            background: #dc3545 !important;
            margin-top: 10px;
        }
        .edit-btn {
            background: #ffc107 !important;
            margin-top: 10px;
        }
        .admin-mode-btn.active {
            background: #28a745;
        }
        .admin-controls {
            width: 100%;
            margin-top: 10px;
            display: flex;
            gap: 5px;
        }
        .admin-form button[type="submit"] {
            transition: transform 0.2s;
        }
        .admin-form button[type="submit"]:active {
            transform: scale(0.95);
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
            <option value="Колье">Колье</option>
            <option value="Чокеры">Чокеры</option>
            <option value="Серьги">Серьги</option>
            <option value="Кольца">Кольца</option>
            <option value="Браслеты">Браслеты</option>
            <option value="Сеты">Сеты</option>
    	</select>
             <input type="text" id="productName" placeholder="Название товара" required>
            <input type="text" id="productDescription" placeholder="Описание" required>
            <input type="number" id="productPrice" placeholder="Цена в рублях" min="1" step="0.01" required>
            <div id="imagePreview" style="display: flex; gap: 5px; flex-wrap: wrap; margin: 10px 0;"></div>
            <input type="file" id="productImages" accept="image/*" multiple>
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
            <button onclick="checkout()" style="width: 100%; margin-top: 15px;">Оформить заказ</button>
        </div>
    </div>

    <script>
        let products = JSON.parse(localStorage.getItem('products')) || [];
        let cart = JSON.parse(localStorage.getItem('cart')) || [];
        let currentCategory = 'all';
        let isAdminMode = false;
        let editingProductId = null;

        function initCarousels() {
            document.querySelectorAll('.carousel').forEach(carousel => {
                let currentIndex = 0;
                const items = carousel.querySelectorAll('.carousel-item');
                const dots = carousel.querySelectorAll('.carousel-dot');
                
                const updateCarousel = () => {
                    items.forEach((item, i) => 
                        item.style.transform = `translateX(${(i - currentIndex) * 100}%)`
                    );
                    dots.forEach((dot, i) => 
                        dot.classList.toggle('active', i === currentIndex)
                    );
                };
                
                dots.forEach((dot, i) => {
                    dot.addEventListener('click', () => {
                        currentIndex = i;
                        updateCarousel();
                    });
                });
            });
        }

        function showCategory(category) {
            currentCategory = category;
            document.querySelectorAll('.category-btn').forEach(btn => {
                btn.classList.toggle('active', btn.textContent === category || (category === 'all' && btn.textContent === 'Все'));
            });
            renderProducts();
        }

        function renderProducts() {
            const filteredProducts = currentCategory === 'all' 
                ? products 
                : products.filter(p => p.category === currentCategory);

            document.getElementById('productGrid').innerHTML = filteredProducts.map(product => `
                <div class="product-card">
                    <div class="carousel">
                        <div class="carousel-inner">
                            ${product.images.map(img => `
                                <img src="${img}" class="carousel-item" alt="${product.name}">
                            `).join('')}
                        </div>
                        ${product.images.length > 1 ? `
                        <div class="carousel-dots">
                            ${product.images.map((_, i) => `
                                <div class="carousel-dot ${i === 0 ? 'active' : ''}"></div>
                            `).join('')}
                        </div>` : ''}
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
            
            initCarousels();
        }

        async function saveProduct(e) {
            e.preventDefault();
            const formData = new FormData(e.target);
            const productData = {
                id: editingProductId || Date.now(),
                category: document.getElementById('categorySelect').value,
                name: document.getElementById('productName').value.trim(),
                description: document.getElementById('productDescription').value.trim(),
                price: Math.round(parseFloat(document.getElementById('productPrice').value) * 100),
                images: []
            };

            // Валидация
            if (!productData.category) return alert('Выберите категорию!');
            if (!productData.name) return alert('Введите название товара!');
            if (isNaN(productData.price) || productData.price <= 0) return alert('Некорректная цена!');

            // Обработка изображений
            const existingImages = Array.from(document.querySelectorAll('#imagePreview img'))
                .map(img => img.src);
            const newImages = await Promise.all(
                Array.from(document.getElementById('productImages').files)
                    .map(file => new Promise((resolve) => {
                        const reader = new FileReader();
                        reader.onload = () => resolve(reader.result);
                        reader.readAsDataURL(file);
                    }))
            );

            productData.images = [...existingImages, ...newImages];
            if (productData.images.length === 0) return alert('Добавьте хотя бы одно изображение!');

            if (editingProductId) {
                const index = products.findIndex(p => p.id === editingProductId);
                products[index] = productData;
            } else {
                products.push(productData);
            }

            localStorage.setItem('products', JSON.stringify(products));
            renderProducts();
            hideAdminPanel();
        }

        function addToCart(productId) {
            const product = products.find(p => p.id === productId);
            if (!product) return;
            
            cart.push({...product, id: Date.now()});
            localStorage.setItem('cart', JSON.stringify(cart));
            updateCart();
            alert('Товар добавлен в корзину!');
        }

        function updateCart() {
            const total = cart.reduce((sum, item) => sum + item.price/100, 0);
            document.getElementById('total').textContent = total.toLocaleString('ru-RU') + ' ₽';
            document.getElementById('cart-items').innerHTML = cart.map((item, index) => `
                <div class="cart-item">
                    <p>${item.name} - ${(item.price/100).toLocaleString('ru-RU')} ₽</p>
                    <button onclick="removeFromCart(${index})">🗑️</button>
                </div>
            `).join('');
        }

        function removeFromCart(index) {
            cart.splice(index, 1);
            localStorage.setItem('cart', JSON.stringify(cart));
            updateCart();
        }

        function editProduct(productId) {
            const product = products.find(p => p.id === productId);
            if (!product) return;

            editingProductId = productId;
            document.getElementById('categorySelect').value = product.category;
            document.getElementById('productName').value = product.name;
            document.getElementById('productDescription').value = product.description;
            document.getElementById('productPrice').value = (product.price/100).toFixed(2);
            
            const preview = document.getElementById('imagePreview');
            preview.innerHTML = product.images.map(img => `
                <div style="position:relative">
                    <img src="${img}" style="width:80px;height:80px;object-fit:cover;border-radius:4px">
                    <button onclick="this.parentElement.remove()" 
                        style="position:absolute;top:2px;right:2px;background:red;border:none;color:white;border-radius:50%;width:16px;height:16px;font-size:10px">×</button>
                </div>
            `).join('');

            document.getElementById('adminPanelTitle').textContent = 'Редактировать товар';
            showAdminPanel();
        }

        // Создание объекта товара
        const productData = {
            id: editingProductId || Date.now(),
            category: category,
            name: productName,
            description: description,
            price: Math.round(price * 100),
            images: [...existingImages, ...newImages]
        };

        // Обновление массива товаров
        if (editingProductId) {
            const index = products.findIndex(p => p.id === editingProductId);
            if (index > -1) products[index] = productData;
        } else {
            products.push(productData);
        }

        // Сохранение и обновление
        localStorage.setItem('products', JSON.stringify(products));
        renderProducts();
        hideAdminPanel();
        
    } catch (error) {
        console.error('Ошибка сохранения:', error);
        alert('Ошибка: ' + error.message);
    }
}

        function hideAdminPanel() {
            document.getElementById('adminPanel').style.display = 'none';
            document.getElementById('productImages').value = '';
            document.getElementById('imagePreview').innerHTML = '';
            document.querySelector('.admin-form').reset();
            editingProductId = null;
        }

        // Остальные функции остаются без изменений
        function addToCart(productId) {
            const product = products.find(p => p.id === productId);
            if (product) {
                cart.push(product);
                saveCart();
                updateCart();
                alert('Товар добавлен в корзину!');
            }
        }

        function updateCart() {
            const total = cart.reduce((sum, item) => sum + item.price/100, 0);
            document.getElementById('total').textContent = total.toLocaleString('ru-RU');
            document.getElementById('cart-items').innerHTML = cart.map(item => `
                <div class="cart-item">
                    <p>${item.name} - ${(item.price/100).toLocaleString('ru-RU')} ₽</p>
                </div>
            `).join('');
        }

        function checkout() {
            if (cart.length === 0) {
                alert('Корзина пуста!');
                return;
            }
            
            cart = [];
            saveCart();
            updateCart();
            closeCart();
            alert('Заказ оформлен!');
        }

        function toggleAdminMode() {
            isAdminMode = !isAdminMode;
            document.querySelector('.admin-mode-btn').classList.toggle('active', isAdminMode);
            renderProducts();
        }

        function deleteProduct(productId) {
            if (confirm('Удалить товар?')) {
                products = products.filter(p => p.id !== productId);
                saveProducts();
                renderProducts();
            }
        }

        function editProduct(productId) {
            const product = products.find(p => p.id === productId);
            editingProductId = productId;
            
            document.getElementById('categorySelect').value = product.category;
            document.getElementById('productName').value = product.name;
            document.getElementById('productDescription').value = product.description;
            document.getElementById('productPrice').value = (product.price/100).toFixed(2);

            const preview = document.getElementById('imagePreview');
            preview.innerHTML = product.images.map(img => `
                <div style="position: relative;">
                    <img src="${img}" style="width: 80px; height: 80px; object-fit: cover; border-radius: 4px;">
                    <button onclick="removeImage(this, '${img}')" 
                            style="position: absolute; top: 2px; right: 2px; background: red; border: none; color: white; border-radius: 50%; width: 16px; height: 16px; font-size: 10px; line-height: 16px; padding: 0;">×</button>
                </div>
            `).join('');
            
            document.getElementById('adminPanelTitle').textContent = 'Редактировать товар';
            showAdminPanel();
        }

        function removeImage(button, imgToRemove) {
            const product = products.find(p => p.id === editingProductId);
            product.images = product.images.filter(img => img !== imgToRemove);
            button.parentElement.remove();
            saveProducts();
        }

        async function saveProduct(e) {
            e.preventDefault();
            const form = e.target;
            const priceInput = document.getElementById('productPrice');
            const price = parseFloat(priceInput.value);
            const productName = document.getElementById('productName').value.trim();
            const category = document.getElementById('categorySelect').value;

            if (!category) {
                alert('Выберите категорию');
                document.getElementById('categorySelect').focus();
                return;
            }

            if (isNaN(price) || price <= 0) {
                alert('Введите корректную цену (число больше 0)');
                priceInput.focus();
                return;
            }

            if (!productName) {
                alert('Введите название товара');
                document.getElementById('productName').focus();
                return;
            }

            const files = Array.from(document.getElementById('productImages').files);
            const existingImages = editingProductId 
                ? [...document.querySelectorAll('#imagePreview img')].map(img => img.src)
                : [];

            if (existingImages.length === 0 && files.length === 0) {
                alert('Добавьте минимум одно изображение');
                return;
            }

            try {
                const newImages = await Promise.all(
                    files.map(file => readFileAsDataURL(file))
                );

                const productData = {
                    id: editingProductId || Date.now(),
                    category: category,
                    name: productName,
                    description: document.getElementById('productDescription').value.trim(),
                    price: Math.round(price * 100),
                    images: [...existingImages, ...newImages]
                };

                if (editingProductId) {
                    const index = products.findIndex(p => p.id === editingProductId);
                    if (index === -1) throw new Error('Товар не найден');
                    products[index] = productData;
                } else {
                    products.push(productData);
                }

                saveProducts();
                renderProducts();
                hideAdminPanel();
                
            } catch (error) {
                console.error('Ошибка сохранения:', error);
                alert('Произошла ошибка при сохранении: ' + error.message);
            }
        }

        function readFileAsDataURL(file) {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.onload = () => resolve(reader.result);
                reader.onerror = reject;
                reader.readAsDataURL(file);
            });
        }

        function saveProducts() {
            localStorage.setItem('products', JSON.stringify(products));
        }

        function saveCart() {
            localStorage.setItem('cart', JSON.stringify(cart));
        }

        function showAdminPanel() {
            document.getElementById('adminPanel').style.display = 'block';
        }

        function hideAdminPanel() {
            document.getElementById('adminPanel').style.display = 'none';
            document.getElementById('adminPanelTitle').textContent = 'Добавить товар';
            document.querySelector('.admin-form').reset();
            document.getElementById('imagePreview').innerHTML = '';
            editingProductId = null;
        }

        function openCart() {
            document.getElementById('cartModal').style.display = 'block';
        }

        function closeCart() {
            document.getElementById('cartModal').style.display = 'none';
        }

        function init() {
            Telegram.WebApp.ready();
            Telegram.WebApp.expand();
            showCategory('all');
            renderProducts();
            updateCart();
        }

        init();
    </script>
</body>
</html>
