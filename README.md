<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Jewelry Store</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        /* Стили остаются без изменений */
        body { font-family: Arial, sans-serif; padding: 20px; background: #f5f5f5; }
        .product-card { background: white; border-radius: 12px; padding: 15px; margin-bottom: 15px; }
        .cart { position: fixed; bottom: 20px; right: 20px; background: white; padding: 15px; border-radius: 12px; }
        button { background: #007bff; color: white; border: none; padding: 8px 16px; border-radius: 6px; }
    </style>
</head>
<body>
    <h1>💎 F 63.9 </h1>
    
    <!-- Товары (остаются без изменений) -->
    <div class="product-card" id="product1">
        <h3>Золотое кольцо</h3>
        <p>Проба 585, с бриллиантом</p>
        <p class="price">$599</p>
        <button onclick="addToCart('product1')">В корзину</button>
    </div>

    <!-- ... остальные товары ... -->

    <!-- Корзина -->
    <div class="cart" id="cart">
        <h3>🛒 Корзина</h3>
        <div id="cart-items"></div>
        <p>Итого: $<span id="total">0</span></p>
        <button onclick="checkout()">Оформить заказ</button>
    </div>

    <script>
        // Конфигурация
        const BOT_TOKEN = '7547523492:AAEx9wa6GiiLUfWBaaEBXSynjONyG4xUaWE';
        const ADMIN_CHAT_ID = '1077071564';
        
        let cart = [];
        const prices = { product1: 599, product2: 299, product3: 899 };

        // Добавление в корзину (без изменений)
        function addToCart(productId) {
            cart.push(productId);
            updateCart();
        }

        // Обновление корзины (без изменений)
        function updateCart() {
            /* ... */
        }

        // Отправка уведомления администратору
        async function sendToAdmin(orderDetails) {
            try {
                const response = await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        chat_id: ADMIN_CHAT_ID,
                        text: `📦 Новый заказ!\n\n${orderDetails}`
                    })
                });
                
                if (!response.ok) throw new Error('Ошибка отправки');
            } catch (error) {
                console.error('Ошибка:', error);
            }
        }

        // Оформление заказа
        async function checkout() {
            if (cart.length === 0) {
                Telegram.WebApp.showAlert("Корзина пуста!");
                return;
            }

            const deliveryAddress = prompt("Введите адрес доставки:");
            if (!deliveryAddress) return;

            // Формируем детали заказа
            let orderDetails = "Список товаров:\n";
            cart.forEach(item => {
                orderDetails += `- ${document.getElementById(item).querySelector('h3').innerText}\n`;
            });
            
            orderDetails += `\nАдрес доставки: ${deliveryAddress}\n`;
            orderDetails += `Общая сумма: $${document.getElementById('total').textContent}`;

            // Отправляем уведомление
            await sendToAdmin(orderDetails);
            
            Telegram.WebApp.showAlert("Заказ оформлен! Администратор свяжется с вами.");
            cart = [];
            updateCart();
        }

        // Инициализация Telegram Web App
        Telegram.WebApp.ready();
        Telegram.WebApp.expand();
    </script>
</body>
</html>
