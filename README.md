
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Adopt Me - Negozio Pet</title>
    <link rel="icon" href="images/favicon.ico" type="image/x-icon"> <!-- Assicurati che questa immagine esista nel repository -->

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
        <h3>Il tuo Carrello</h3>
        <ul class="cart-items" id="cart-items"></ul>
        <div class="total" id="total">Totale: €0</div>
        <div class="paypal-button" id="paypal-button"></div>
    </div>

    <div class="container">
        <h2>Adotta un Pet</h2>
        <div class="products" id="products">
            <!-- I prodotti verranno inseriti dinamicamente via JS -->
        </div>
    </div>

    <!-- Notifica per l'aggiunta al carrello -->
    <div class="notification" id="notification">Aggiunto al carrello!</div>

    <script src="https://www.paypal.com/sdk/js?client-id=your-client-id"></script>
    <script>
        let cart = JSON.parse(localStorage.getItem("cart")) || [];
        const cartButton = document.getElementById('toggle-cart');
        const cartSection = document.getElementById('cart-section');
        const cartCounter = document.getElementById('cart-counter');
        const cartItems = document.getElementById('cart-items');
        const totalElement = document.getElementById('total');
        const notification = document.getElementById('notification');
        const productsContainer = document.getElementById('products');

        // Funzione per calcolare il totale
        function updateTotal() {
            let total = cart.reduce((sum, product) => sum + product.price, 0);
            totalElement.textContent = 'Totale: €' + total.toFixed(2);
            cartCounter.textContent = cart.length;
        }

        // Funzione per mostrare o nascondere il carrello
        function toggleCart() {
            cartSection.style.display = cartSection.style.display === 'block' ? 'none' : 'block';
        }

        // Funzione per rimuovere gli articoli dal carrello
        function removeFromCart(index) {
            cart.splice(index, 1);
            localStorage.setItem("cart", JSON.stringify(cart));
            renderCart();
            updateTotal();
        }

        // Funzione per rendere visibile la notifica
        function showNotification() {
            notification.style.display = 'block';
            setTimeout(() => {
                notification.style.display = 'none';
            }, 2000);
        }

        // Funzione per caricare i prodotti
        function loadProducts() {
            const products = [
                { name: 'Cane', description: 'Un simpatico cane', price: 50, image: 'images/dog.jpg' },
                { name: 'Gatto', description: 'Un dolce gatto', price: 40, image: 'images/cat.jpg' }
            ];

            products.forEach((product, index) => {
                const productElement = document.createElement('div');
                productElement.className = 'product';
                productElement.innerHTML = `
                    <img src="${product.image}" alt="${product.name}">
                    <h3>${product.name}</h3>
                    <p>${product.description}</p>
                    <p>€${product.price.toFixed(2)}</p>
                    <button onclick="addToCart(${index})">Aggiungi al carrello</button>
                `;
                productsContainer.appendChild(productElement);
            });
        }

        // Funzione per aggiungere al carrello
        function addToCart(index) {
            const product = [
                { name: 'Cane', description: 'Un simpatico cane', price: 50, image: 'images/dog.jpg' },
                { name: 'Gatto', description: 'Un dolce gatto', price: 40, image: 'images/cat.jpg' }
            ][index];

            cart.push(product);
            localStorage.setItem("cart", JSON.stringify(cart));
            showNotification();
            updateTotal();
        }

        // Funzione per renderizzare il carrello
        function renderCart() {
            cartItems.innerHTML = '';
            cart.forEach((item, index) => {
                const li = document.createElement('li');
                li.innerHTML = `
                    <span>${item.name} - €${item.price.toFixed(2)}</span>
                    <button onclick="removeFromCart(${index})">Rimuovi</button>
                `;
                cartItems.appendChild(li);
            });
        }

        // Event listener per il pulsante carrello
        cartButton.addEventListener('click', toggleCart);

        // Carica i prodotti e il carrello
        loadProducts();
        updateTotal();
        renderCart();
    </script>
</body>
</html>
