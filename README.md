<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Adopt Me - Negozio Pet</title>
    <link rel="icon" href="favicon.ico" type="image/x-icon">
    <!-- Font Awesome per l'icona del carrello -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        /* Global Styles */
        body {
            margin: 0;
            font-family: 'Roboto', Arial, sans-serif;
            background: #f0f0f0;
            color: #333;
            box-sizing: border-box;
        }

        header {
            background-color: #ff5a5f;
            color: white;
            padding: 20px 0;
            text-align: center;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        header h1 {
            margin: 0;
            font-size: 2.2rem;
            letter-spacing: 1px;
        }

        .container {
            max-width: 1000px;
            margin: 20px auto;
            padding: 20px;
            background: white;
            border-radius: 15px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }

        .products {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-around;
            gap: 15px;
        }

        .product {
            flex: 1 1 calc(33.333% - 15px);
            border: 1px solid #ddd;
            border-radius: 10px;
            background: #fff;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            text-align: center;
            margin-bottom: 15px;
            padding: 10px;
        }

        .product:hover {
            transform: translateY(-6px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
        }

        .product img {
            width: 100%; /* Forza tutte le immagini ad avere la stessa larghezza */
            height: 150px; /* Imposta un'altezza fissa per tutte le immagini */
            object-fit: contain; /* Mantiene l'immagine intera senza ritagliarla */
            border-bottom: 1px solid #eee;
            margin-bottom: 10px;
        }

        .product h3 {
            font-size: 1.2rem;
            margin: 10px 0;
            color: #333;
        }

        .product p {
            margin: 5px 0;
            color: #777;
            font-size: 0.9rem;
        }

        .product button {
            width: 100%;
            padding: 10px;
            background-color: #ff5a5f;
            color: white;
            font-weight: bold;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-size: 1rem;
            transition: background-color 0.3s;
        }

        .product button:hover {
            background-color: #ff3a3f;
        }

        /* Carrello Styles */
        .cart-button {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px;
            background-color: #ff5a5f;
            color: white;
            font-size: 1.5rem;
            font-weight: bold;
            border: none;
            border-radius: 50%;
            cursor: pointer;
            z-index: 1000;
        }

        .cart-counter {
            position: fixed;
            top: 50px;
            right: 60px;
            background-color: #ff5a5f;
            color: white;
            font-size: 1.2rem;
            padding: 5px 10px;
            border-radius: 50%;
            z-index: 1000;
            display: none; /* Inizialmente nascosto */
        }

        .cart-section {
            position: fixed;
            top: 60px;
            right: 20px;
            background: white;
            padding: 15px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            width: 300px;
            display: none; /* Nascondiamo il carrello di default */
            z-index: 999;
            max-height: 70%;
            overflow-y: auto;
        }

        .cart-items {
            list-style: none;
            padding: 0;
            margin: 0;
        }

        .cart-items li {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #eee;
            padding: 10px 0;
        }

        .cart-items li:last-child {
            border-bottom: none;
        }

        .cart-items li span {
            font-size: 1rem;
        }

        .cart-items li button {
            background: #ff4a4a;
            color: white;
            border: none;
            padding: 6px 12px;
            border-radius: 10px;
            cursor: pointer;
            font-size: 0.9rem;
        }

        .cart-items li button:hover {
            background: #d43c3c;
        }

        .total {
            margin-top: 15px;
            font-size: 1.5rem;
            text-align: right;
            font-weight: bold;
        }

        .paypal-button {
            text-align: right;
            margin-top: 20px;
        }

        @media (max-width: 768px) {
            .product {
                flex: 1 1 100%;
                margin: 10px 0;
            }

            header h1 {
                font-size: 2rem;
            }

            .product img {
                width: 90%;
            }

            .product h3 {
                font-size: 1.1rem;
            }

            .product p {
                font-size: 0.9rem;
            }

            .total {
                font-size: 1.3rem;
            }

            .cart-section {
                right: 10px;
                width: 250px;
            }
        }

        /* Stile della notifica */
        .notification {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background-color: #4CAF50;
            color: white;
            padding: 10px 20px;
            border-radius: 5px;
            font-size: 1rem;
            display: none;
            z-index: 999;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
        }
    </style>
</head>
<body>
    <header>
        <h1>Adopt Me - Negozio Pet</h1>
    </header>

    <!-- Bottone Carrello con icona -->
    <button class="cart-button" id="toggle-cart">
        <i class="fas fa-shopping-cart"></i>
    </button>

    <!-- Contatore carrello -->
    <div class="cart-counter" id="cart-counter"></div>

    <div class="cart-section" id="cart-section">
        <h2>Carrello</h2>
        <ul class="cart-items" id="cart"></ul>
        <div class="total" id="total">Totale: €0</div>
        <div class="paypal-button" id="paypal-button-container"></div>
    </div>

    <!-- Notifica -->
    <div class="notification" id="notification">Product added to cart!</div>

    <div class="container">
        <div class="products" id="product-list"></div>
    </div>

    <!-- PayPal SDK -->
    <script src="https://www.paypal.com/sdk/js?client-id=AZxfqNzIB_vmIsIyI-j8y5effKA3WbJRsj0xRNUbV519wBwaI3UfCTP9OuTRtJ4p4Hx_LM0oP8TCzW2f&currency=EUR"></script>
    <script>
        const products = [
            { name: "Pet 1", price: 2.99, img: "https://starpets.ams3.cdn.digitaloceanspaces.com/AM/dragon_124403728485816668684803314274571297384.png?v=2" },
            { name: "Pet 2", price: 2.99, img: "https://starpets.ams3.cdn.digitaloceanspaces.com/AM/dragon_124403728485816668684803314274571297384.png?v=2" },
            { name: "Pet 3", price: 2.99, img: "https://starpets.ams3.cdn.digitaloceanspaces.com/AM/dragon_124403728485816668684803314274571297384.png?v=2" },
            { name: "Pet 4", price: 2.99, img: "https://starpets.ams3.cdn.digitaloceanspaces.com/AM/dragon_124403728485816668684803314274571297384.png?v=2" },
            { name: "Pet 5", price: 2.99, img: "https://starpets.ams3.cdn.digitaloceanspaces.com/AM/dragon_124403728485816668684803314274571297384.png?v=2" },
            { name: "Pet 6", price: 2.99, img: "https://starpets.ams3.cdn.digitaloceanspaces.com/AM/dragon_124403728485816668684803314274571297384.png?v=2" },
            { name: "Pet 7", price: 2.99, img: "https://starpets.ams3.cdn.digitaloceanspaces.com/AM/dragon_124403728485816668684803314274571297384.png?v=2" },
            { name: "Pet 8", price: 2.99, img: "https://starpets.ams3.cdn.digitaloceanspaces.com/AM/dragon_124403728485816668684803314274571297384.png?v=2" },
            { name: "Pet 9", price: 2.99, img: "https://starpets.ams3.cdn.digitaloceanspaces.com/AM/dragon_124403728485816668684803314274571297384.png?v=2" },
            { name: "Pet 10", price: 2.99, img: "https://starpets.ams3.cdn.digitaloceanspaces.com/AM/dragon_124403728485816668684803314274571297384.png?v=2" },
        ];

        let cart = [];
        let total = 0;

        function addToCart(name, price) {
            cart.push({ name, price });
            total += price;
            updateCartUI();
            showNotification(`${name} added to cart!`);
            updateCartCounter();
        }

        function removeFromCart(index) {
            total -= cart[index].price;
            cart.splice(index, 1);
            updateCartUI();
            updateCartCounter();
        }

        function updateCartUI() {
            const cartList = document.getElementById("cart");
            cartList.innerHTML = "";
            cart.forEach((item, index) => {
                const li = document.createElement("li");
                li.innerHTML = `<span>${item.name} - €${item.price}</span> 
                    <button onclick="removeFromCart(${index})">Remove</button>`;
                cartList.appendChild(li);
            });
            document.getElementById("total").textContent = `Total: €${total.toFixed(2)}`;
            renderPayPalButton();
        }

        function updateCartCounter() {
            const cartCounter = document.getElementById("cart-counter");
            cartCounter.textContent = cart.length;
            cartCounter.style.display = cart.length > 0 ? "block" : "none";
        }

        function renderPayPalButton() {
            if (cart.length > 0) {
                paypal.Buttons({
                    createOrder: function(data, actions) {
                        return actions.order.create({
                            purchase_units: [{
                                amount: {
                                    value: total.toFixed(2)
                                }
                            }]
                        });
                    },
                    onApprove: function(data, actions) {
                        return actions.order.capture().then(function(details) {
                            alert('Transaction completed by ' + details.payer.name.given_name);
                        });
                    }
                }).render('#paypal-button-container');
            }
        }

        function showNotification(message) {
            const notification = document.getElementById("notification");
            notification.textContent = message;
            notification.style.display = "block";
            setTimeout(function() {
                notification.style.display = "none";
            }, 3000);
        }

        function toggleCart() {
            const cartSection = document.getElementById("cart-section");
            cartSection.style.display = cartSection.style.display === "none" || !cartSection.style.display ? "block"
