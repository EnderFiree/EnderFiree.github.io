<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Adopt Me - Negozio Pet</title>
    <!-- Favicon -->
    <link rel="icon" href="favicon.ico" type="image/x-icon">
    <style>
        /* Global Styles */
        body {
            margin: 0;
            font-family: 'Roboto', Arial, sans-serif;
            background: linear-gradient(to bottom, #f7f8fa, #e2e6ef);
            color: #333;
        }

        header {
            background: #4a90e2;
            color: white;
            padding: 20px 0;
            text-align: center;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }

        header h1 {
            margin: 0;
            font-size: 2.5rem;
        }

        .container {
            max-width: 1200px;
            margin: 20px auto;
            padding: 20px;
            background: #fff;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        .products {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
        }

        .product {
            flex: 1 1 calc(33.333% - 20px);
            margin: 10px;
            border: 1px solid #ddd;
            border-radius: 10px;
            background: #f9f9f9;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }

        .product:hover {
            transform: translateY(-5px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
        }

        .product h3 {
            font-size: 1.25rem;
            margin: 10px 0;
            color: #555;
        }

        .product p {
            margin: 5px 0;
            color: #777;
        }

        .product button {
            width: 100%;
            padding: 10px 15px;
            border: none;
            background-color: #4a90e2;
            color: white;
            border-radius: 0 0 10px 10px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: bold;
        }

        .product button:hover {
            background-color: #357ab8;
        }

        .cart-section {
            margin-top: 20px;
            padding: 20px;
            background: #fefefe;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        .cart-section h2 {
            margin: 0 0 10px;
            font-size: 1.8rem;
            color: #333;
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
            font-size: 1.1rem;
        }

        .cart-items li button {
            background: #ff4a4a;
            color: white;
            border: none;
            padding: 5px 10px;
            border-radius: 5px;
            cursor: pointer;
        }

        .cart-items li button:hover {
            background: #d43c3c;
        }

        .total {
            margin-top: 20px;
            font-size: 1.5rem;
            text-align: right;
        }

        .paypal-button {
            text-align: right;
            margin-top: 20px;
        }

        @media (max-width: 768px) {
            .products {
                flex-direction: column;
            }

            .product {
                flex: 1 1 100%;
                margin: 10px 0;
            }
        }
    </style>
</head>
<body>
    <header>
        <h1>Adopt Me - Negozio Pet</h1>
    </header>
    <div class="container">
        <div class="products" id="product-list">
            <!-- Dynamically Generated Products -->
        </div>
        <div class="cart-section">
            <h2>Carrello</h2>
            <ul class="cart-items" id="cart"></ul>
            <div class="total" id="total">Totale: €0</div>
            <div class="paypal-button" id="paypal-button-container"></div>
        </div>
    </div>

    <!-- PayPal SDK -->
    <script src="https://www.paypal.com/sdk/js?client-id=AZxfqNzIB_vmIsIyI-j8y5effKA3WbJRsj0xRNUbV519wBwaI3UfCTP9OuTRtJ4p4Hx_LM0oP8TCzW2f&currency=EUR"></script>
    <script>
        const products = [
            { name: "Neon Dragon Fly Ride", price: 2.99 },
            { name: "Golden Unicorn Fly Ride", price: 10.99 },
            { name: "Frost Fury Fly Ride", price: 8.99 },
            { name: "Shadow Dragon Fly Ride", price: 55.99 },
            { name: "Neon Turtle Fly Ride", price: 23.99 },
            { name: "Neon Kangaroo Fly Ride", price: 48.99 },
            { name: "Frost Dragon Fly Ride", price: 67.99 },
            { name: "Arctic Reindeer Fly Ride", price: 13.99 },
            { name: "Evil Unicorn Fly Ride", price: 29.99 },
            { name: "Neon Owl Fly Ride", price: 13.99 },
            { name: "Neon Giraffe Fly Ride", price: 37.99 },
            { name: "Mega Crow Fly Ride", price: 67.80 },
            { name: "Neon Bat Dragon Fly Ride", price: 85.99 },
        ];

        let cart = [];
        let total = 0;

        function addToCart(name, price) {
            cart.push({ name, price });
            total += price;
            updateCartUI();
        }

        function removeFromCart(index) {
            total -= cart[index].price;
            cart.splice(index, 1);
            updateCartUI();
        }

        function updateCartUI() {
            const cartList = document.getElementById("cart");
            cartList.innerHTML = "";
            cart.forEach((item, index) => {
                const li = document.createElement("li");
                li.innerHTML = `<span>${item.name} - €${item.price}</span> 
                    <button onclick="removeFromCart(${index})">Rimuovi</button>`;
                cartList.appendChild(li);
            });
            document.getElementById("total").textContent = `Totale: €${total.toFixed(2)}`;
            renderPayPalButton();
        }

        function renderPayPalButton() {
            document.getElementById("paypal-button-container").innerHTML = ""; // Reset PayPal button
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
                        alert("Pagamento completato! Grazie per il tuo acquisto.");
                        cart = [];
                        total = 0;
                        updateCartUI();
                    });
                },
                onCancel: function(data) {
                    alert("Pagamento annullato.");
                },
                onError: function(err) {
                    console.error("Errore durante il pagamento: ", err);
                    alert("Si è verificato un errore durante il pagamento.");
                }
            }).render("#paypal-button-container");
        }

        function renderProducts() {
            const productList = document.getElementById("product-list");
            products.forEach(product => {
                const div = document.createElement("div");
                div.classList.add("product");
                div.innerHTML = `
                    <h3>${product.name}</h3>
                    <p>Prezzo: €${product.price}</p>
                    <button onclick="addToCart('${product.name}', ${product.price})">Aggiungi al Carrello</button>
                `;
                productList.appendChild(div);
            });
        }

        renderProducts();
    </script>
</body>
</html>
