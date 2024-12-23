<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Negozio Pet Adopt Me</title>
    <!-- Favicon -->
    <link rel="icon" href="favicon.ico" type="image/x-icon">
    <style>
        body {
            font-family: Arial, sans-serif;
        }
        .pet {
            display: inline-block;
            margin: 10px;
            border: 1px solid #ddd;
            border-radius: 8px;
            width: 200px;
            text-align: center;
            padding: 10px;
            background-color: #f9f9f9;
        }
        .pet img {
            width: 100%;
            height: auto;
            border-radius: 8px;
        }
        .cart {
            margin-top: 20px;
        }
        .paypal-button {
            margin: 20px;
        }
    </style>
</head>
<body>
    <h1>Negozio Pet Adopt Me</h1>
    <div id="pets">
        <!-- Pet 1 -->
        <div class="pet">
            <img src="neon_dragon.jpg" alt="Neon Dragon Fly Ride" onerror="this.src='placeholder.jpg';">
            <h3>Neon Dragon Fly Ride</h3>
            <p>Prezzo: €25</p>
            <button onclick="addToCart('Neon Dragon Fly Ride', 25)">Aggiungi al Carrello</button>
        </div>
        <!-- Pet 2 -->
        <div class="pet">
            <img src="golden_unicorn.jpg" alt="Golden Unicorn Fly Ride" onerror="this.src='placeholder.jpg';">
            <h3>Golden Unicorn Fly Ride</h3>
            <p>Prezzo: €30</p>
            <button onclick="addToCart('Golden Unicorn Fly Ride', 30)">Aggiungi al Carrello</button>
        </div>
        <!-- Pet 3 -->
        <div class="pet">
            <img src="frost_fury.jpg" alt="Frost Fury Fly Ride" onerror="this.src='placeholder.jpg';">
            <h3>Frost Fury Fly Ride</h3>
            <p>Prezzo: €35</p>
            <button onclick="addToCart('Frost Fury Fly Ride', 35)">Aggiungi al Carrello</button>
        </div>
    </div>

    <div class="cart">
        <h2>Carrello</h2>
        <ul id="cart"></ul>
        <p id="total">Totale: €0</p>
        <button onclick="checkout()">Paga</button>
    </div>

    <!-- PayPal SDK -->
    <script src="https://www.paypal.com/sdk/js?client-id=AZxfqNzIB_vmIsIyI-j8y5effKA3WbJRsj0xRNUbV519wBwaI3UfCTP9OuTRtJ4p4Hx_LM0oP8TCzW2f&currency=EUR"></script>

    <script>
        let cart = [];
        let total = 0;

        function addToCart(name, price) {
            cart.push({ name, price });
            total += price;
            updateCartUI();
        }

        function updateCartUI() {
            const cartList = document.getElementById("cart");
            cartList.innerHTML = "";
            cart.forEach((item, index) => {
                const li = document.createElement("li");
                li.textContent = `${item.name} - €${item.price}`;
                cartList.appendChild(li);
            });
            document.getElementById("total").textContent = `Totale: €${total}`;
        }

        function checkout() {
            paypal.Buttons({
                createOrder: function(data, actions) {
                    return actions.order.create({
                        purchase_units: [{
                            amount: {
                                value: total.toFixed(2) // Utilizza il totale aggiornato
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
            }).render(".paypal-button");
        }
    </script>
</body>
</html>
