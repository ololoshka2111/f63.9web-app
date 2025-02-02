<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Jewelry Store</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 20px;
            background: #f5f5f5;
        }
        .product-card {
            background: white;
            border-radius: 12px;
            padding: 15px;
            margin-bottom: 15px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        .cart {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: white;
            padding: 15px;
            border-radius: 12px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        button {
            background: #007bff;
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 6px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <h1>💎 Ювелирные украшения</h1>
    
    <!-- Товары -->
    <div class="product-card" id="product1">
        <h3>Золотое кольцо</h3>
        <p>Проба 585, с бриллиантом</p>
        <p class="price">$599</p>
        <button onclick="addToCart('product1')">В корзину</button>
    </div>

    <div class="product-card" id="product2">
        <h3>Серебряные серьги</h3>
        <p>Ручная работа</p>
        <p class="price">$299</p>
        <button onclick="addToCart('product2')">В корзину</button>
    </div>

    <div class="product-card" id="product3">
        <h3>Платиновый браслет</h3>
        <p>Эксклюзивный дизайн</p>
        <p class="price">$899</p>
        <button onclick="addToCart('product3')">В корзину</button>
    </div>

    <!-- Корзина -->
    <div class="cart" id="cart">
        <h3>🛒 Корзина</h3>
        <div id="cart-items"></div>
        <p>Итого: $<span id="total">0</span></p>
        <button onclick="checkout()">Оформить заказ</button>
    </div>

    <script>
        let cart = [];
        const prices = {
            product1: 599,
            product2: 299,
            product3: 899
        };

        // Добавление в корзину
        function addToCart(productId) {
            cart.push(productId);
            updateCart();
        }

        // Обновление корзины
        function updateCart() {
            const cartItems = document.getElementById('cart-items');
            const totalElement = document.getElementById('total');
            
            cartItems.innerHTML = '';
            let total = 0;
            
            cart.forEach(item => {
                total += prices[item];
                cartItems.innerHTML += `<p>${document.getElementById(item).querySelector('h3').innerText}</p>`;
            });
            
            totalElement.textContent = total;
        }

        // Оформление заказа
        async function checkout() {
            const deliveryAddress = prompt("Введите адрес доставки:");
            if (!deliveryAddress) return;

            // Интеграция с платежной системой (пример для Stripe)
            const stripe = Stripe('YOUR_STRIPE_KEY');
            const { error } = await stripe.redirectToCheckout({
                lineItems: [{ price: 'PRICE_ID', quantity: 1 }],
                mode: 'payment',
                successUrl: 'https://your-site.com/success',
                cancelUrl: 'https://your-site.com/cancel'
            });

            if (error) {
                Telegram.WebApp.showAlert("Ошибка оплаты!");
            } else {
                // Интеграция с API доставки
                fetch('https://delivery-api.com/create-order', {
                    method: 'POST',
                    body: JSON.stringify({
                        items: cart,
                        address: deliveryAddress
                    })
                });
                
                Telegram.WebApp.showAlert("Заказ оформлен!");
                cart = [];
                updateCart();
            }
        }

        // Инициализация Telegram Web App
        Telegram.WebApp.ready();
        Telegram.WebApp.expand();
    </script>
</body>
</html>
