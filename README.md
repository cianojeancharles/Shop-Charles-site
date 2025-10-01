<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Page d'accueil - Shop Charles</title>
    <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
    <style>
        /* Styles pour la page d'accueil */
        body {
            margin: 0;
            font-family: sans-serif;
            color: white;
            background: linear-gradient(to right, #66BB6A, #42A5F5);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }
        
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 20px;
            z-index: 10;
        }
        
        .logo {
            font-size: 2.5em;
            font-weight: bold;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }
        
        .header-buttons {
            display: flex;
            align-items: center;
        }

        .menu-btn {
            background-color: transparent;
            border: 2px solid white;
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            cursor: pointer;
            border-radius: 5px;
            text-transform: uppercase;
            margin-left: 10px;
            width: 70px;
        }

        .menu-btn:hover {
            background-color: rgba(255, 255, 255, 0.2);
        }

        /* Boutons de langue */
        .language-buttons {
            position: fixed;
            top: 20px;
            left: 20px;
            display: flex;
            gap: 10px;
            z-index: 1000;
        }

        .lang-btn {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            border: 2px solid white;
            background: rgba(255, 255, 255, 0.2);
            color: white;
            font-weight: bold;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;
        }

        .lang-btn:hover {
            background: rgba(255, 255, 255, 0.3);
            transform: scale(1.1);
        }

        .lang-btn.active {
            background: #FFEB3B;
            color: #333;
        }

        .main-content {
            flex-grow: 1;
            display: flex;
            flex-direction: column;
            justify-content: flex-start;
            align-items: center;
            text-align: center;
            padding: 20px;
            padding-top: 80px;
            min-height: 120vh;
        }

        .slogan {
            font-size: 1.2em;
            margin-top: 20px;
            font-style: italic;
        }

        /* Section partenaires */
        .partners-section {
            margin-top: 60px;
            padding: 30px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            max-width: 70%;
            backdrop-filter: blur(5px);
            -webkit-backdrop-filter: blur(5px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .partners-section h3 {
            margin-top: 0;
            font-size: 1.4em;
            margin-bottom: 15px;
            color: white;
        }

        .partners-section p {
            font-size: 1.1em;
            color: rgba(255, 255, 255, 0.8);
            margin: 0;
        }

        /* Espace pour la réponse */
        .answer-space {
            margin-top: 40px;
            padding: 30px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            max-width: 70%;
            min-height: 150px;
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }

        .answer-space h3 {
            margin-top: 0;
            font-size: 1.3em;
            margin-bottom: 20px;
        }

        .answer-space p {
            font-size: 1.1em;
            line-height: 1.6;
            margin: 0;
            color: rgba(255, 255, 255, 0.9);
        }

        /* Styles de la fenêtre modale (menu) */
        .modal {
            display: none;
            position: fixed;
            z-index: 100;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0,0,0,0.6);
            backdrop-filter: blur(5px);
            -webkit-backdrop-filter: blur(5px);
            justify-content: center;
            align-items: center;
        }

        .modal-content {
            background: linear-gradient(to bottom right, #3F51B5, #00BCD4);
            margin: auto;
            padding: 30px;
            border-radius: 15px;
            width: 80%;
            max-width: 400px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.4);
            text-align: center;
            position: relative;
        }

        .close-btn {
            color: white;
            position: absolute;
            top: 10px;
            right: 20px;
            font-size: 35px;
            font-weight: bold;
            cursor: pointer;
            transition: color 0.3s ease;
        }

        .close-btn:hover {
            color: #FFEB3B;
        }

        .modal-content h2 {
            margin-bottom: 25px;
            font-size: 2em;
            text-shadow: 1px 1px 3px rgba(0,0,0,0.2);
        }

        .modal-content button {
            display: block;
            width: calc(100% - 40px);
            padding: 15px;
            margin: 15px auto;
            background-color: #FFEB3B;
            color: #333;
            border: none;
            border-radius: 8px;
            font-size: 1.1em;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.2s ease;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
            font-weight: bold;
        }

        .modal-content button:hover {
            background-color: #FDD835;
            transform: translateY(-2px);
        }

        /* Pages */
        .support-page, .don-page, .boutique-page, .products-page, .cart-page, .account-page, .purchases-page {
            display: none;
            min-height: 100vh;
        }

        .support-page {
            background-color: black;
            color: white;
            padding: 50px 20px;
            text-align: center;
            font-family: sans-serif;
        }

        .support-page h1 {
            font-size: 2.5em;
            margin-bottom: 50px;
        }

        .contact-info {
            font-size: 1.5em;
            line-height: 2;
            max-width: 600px;
            margin: 0 auto;
        }

        .contact-info p {
            margin: 30px 0;
        }

        .back-btn {
            background-color: white;
            color: black;
            padding: 15px 30px;
            border: none;
            border-radius: 8px;
            font-size: 1.1em;
            cursor: pointer;
            margin-top: 50px;
            font-weight: bold;
        }

        .back-btn:hover {
            background-color: #f0f0f0;
        }

        /* Bouton Boutique sur la page d'accueil */
        .boutique-btn {
            background-color: #FFEB3B;
            color: #333;
            padding: 15px 30px;
            border: none;
            border-radius: 8px;
            font-size: 1.2em;
            cursor: pointer;
            margin-top: 30px;
            font-weight: bold;
            transition: background-color 0.3s ease, transform 0.2s ease;
            box-shadow: 0 3px 8px rgba(0,0,0,0.2);
        }

        .boutique-btn:hover {
            background-color: #FDD835;
            transform: translateY(-2px);
        }

        /* Formulaire de création de compte */
        .form-group {
            margin-bottom: 20px;
            text-align: left;
        }

        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
            color: white;
        }

        .form-group input {
            width: 100%;
            padding: 12px;
            border: 2px solid rgba(255,255,255,0.3);
            border-radius: 8px;
            background: rgba(255,255,255,0.1);
            color: white;
            font-size: 1em;
            box-sizing: border-box;
        }

        .form-group input::placeholder {
            color: rgba(255,255,255,0.7);
        }

        .form-group input:focus {
            outline: none;
            border-color: #FFEB3B;
            background: rgba(255,255,255,0.2);
        }

        /* Page de don */
        .don-page {
            background-color: black;
            color: white;
            padding: 50px 20px;
            text-align: center;
            font-family: sans-serif;
        }

        .don-page h1 {
            font-size: 2.5em;
            margin-bottom: 50px;
        }

        .don-info {
            font-size: 1.3em;
            line-height: 1.8;
            max-width: 600px;
            margin: 0 auto;
        }

        .don-info p {
            margin: 30px 0;
        }

        .thank-message {
            margin-top: 50px;
            font-size: 1.1em;
            color: #FFEB3B;
            font-style: italic;
        }

        /* Page boutique */
        .boutique-page {
            background-color: white;
            color: black;
            font-family: sans-serif;
            overflow-y: auto;
        }

        .boutique-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 20px;
            background-color: white;
            position: sticky;
            top: 0;
            z-index: 10;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        .boutique-logo {
            font-size: 2.5em;
            font-weight: bold;
            color: black;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
        }

        .boutique-main {
            padding: 40px 20px;
            text-align: center;
        }

        .boutique-main h1 {
            margin-bottom: 50px;
            font-size: 2.2em;
        }

        .categories-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            max-width: 1000px;
            margin: 0 auto 100px auto;
        }

        .category-btn {
            background-color: #f5f5f5;
            color: #333;
            padding: 30px 20px;
            border: 2px solid #ddd;
            border-radius: 12px;
            font-size: 1.2em;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s ease;
            box-shadow: 0 3px 8px rgba(0,0,0,0.1);
        }

        .category-btn:hover {
            background-color: #e0e0e0;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        }

        .limited-offer {
            margin-top: 80px;
            padding: 40px 20px;
            background-color: #f8f8f8;
            border-top: 1px solid #ddd;
            text-align: center;
        }

        .limited-offer h2 {
            color: #e53e3e;
            font-size: 1.8em;
            margin-bottom: 30px;
        }

        .offer-photos-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 15px;
            max-width: 1200px;
            margin: 0 auto 40px auto;
        }

        .offer-product-card {
            background: white;
            border-radius: 12px;
            box-shadow: 0 3px 12px rgba(0,0,0,0.1);
            padding: 20px;
            text-align: center;
            transition: transform 0.3s ease;
            border: 2px solid #e53e3e;
        }

        .offer-product-card:hover {
            transform: translateY(-5px);
        }

        .product-image-placeholder {
            width: 100%;
            height: 150px;
            background: #f0f0f0;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2em;
            margin-bottom: 15px;
        }

        .product-name {
            font-size: 1em;
            font-weight: bold;
            margin-bottom: 8px;
            color: #333;
        }

        .product-price {
            font-size: 1.2em;
            color: #e53e3e;
            font-weight: bold;
        }

        .back-to-home {
            display: inline-flex;
            align-items: center;
            background-color: #4CAF50;
            color: white;
            padding: 15px 25px;
            border: none;
            border-radius: 8px;
            font-size: 1.1em;
            cursor: pointer;
            margin-top: 30px;
            font-weight: bold;
            transition: background-color 0.3s ease;
        }

        .back-to-home:hover {
            background-color: #45a049;
        }

        .back-to-home::after {
            content: " ←";
            margin-left: 10px;
        }

        /* Page produits */
        .products-page {
            background-color: white;
            color: black;
            font-family: sans-serif;
            text-align: center;
        }

        .products-page h1 {
            font-size: 2em;
            color: #333;
            margin-bottom: 40px;
        }

        .products-header-buttons {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-bottom: 40px;
        }

        .panier-btn {
            background-color: #FF6B35;
            color: white;
            padding: 12px 24px;
            border: none;
            border-radius: 8px;
            font-size: 1.1em;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s ease;
        }

        .panier-btn:hover {
            background-color: #E55A2B;
            transform: translateY(-2px);
        }

        .product-photos-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            max-width: 1200px;
            margin: 0 auto 40px auto;
            padding: 0 20px;
        }

        .product-card {
            background: white;
            border-radius: 12px;
            box-shadow: 0 3px 12px rgba(0,0,0,0.1);
            padding: 20px;
            text-align: center;
            transition: transform 0.3s ease;
        }

        .product-card:hover {
            transform: translateY(-5px);
        }

        /* Page panier */
        .cart-page {
            background-color: white;
            color: black;
            padding: 50px 20px;
            text-align: center;
            font-family: sans-serif;
        }

        .cart-page h1 {
            font-size: 2.5em;
            margin-bottom: 50px;
            color: #333;
        }

        /* Page compte */
        .account-page {
            background-color: white;
            color: black;
            padding: 50px 20px;
            text-align: center;
            font-family: sans-serif;
        }

        .account-page h1 {
            font-size: 2.5em;
            margin-bottom: 50px;
            color: #333;
        }

        .account-info {
            background: #f8f9fa;
            padding: 30px;
            border-radius: 15px;
            max-width: 500px;
            margin: 0 auto;
            box-shadow: 0 3px 15px rgba(0,0,0,0.1);
        }

        .info-item {
            margin: 20px 0;
            padding: 15px;
            background: white;
            border-radius: 8px;
            border-left: 4px solid #667eea;
        }

        .info-label {
            font-weight: bold;
            color: #333;
            font-size: 0.9em;
            text-transform: uppercase;
        }

        .info-value {
            font-size: 1.2em;
            color: #555;
            margin-top: 5px;
        }

        /* Page achats */
        .purchases-page {
            background-color: white;
            color: black;
            padding: 50px 20px;
            text-align: center;
            font-family: sans-serif;
        }

        .purchases-page h1 {
            font-size: 2.5em;
            margin-bottom: 50px;
            color: #333;
        }

        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 25px;
            border-radius: 5px;
            color: white;
            font-weight: 500;
            z-index: 9999;
            animation: slideIn 0.3s ease;
        }

        .notification.success {
            background: #48bb78;
        }

        .notification.error {
            background: #f56565;
        }

        @keyframes slideIn {
            from { transform: translateX(100%); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }
    </style>
</head>
<body>

    <!-- Boutons de langue -->
    <div class="language-buttons">
        <button class="lang-btn active" id="frBtn" data-lang="fr">FR</button>
        <button class="lang-btn" id="crBtn" data-lang="cr">CR</button>
    </div>
        <div id="mainPage">
        <header class="header">
            <div class="logo" data-translate="site-title">Shop Charles</div>
            <div class="header-buttons">
                <button class="menu-btn" id="openMenuBtn" data-translate="menu-btn">Menu</button>
                <button class="menu-btn" id="openEntrerBtn" data-translate="enter-btn">Entrer</button>
            </div>
        </header>

        <main class="main-content">
            <h1 data-translate="welcome">Bienvenue !</h1>
            
            <div class="answer-space">
                <h3 data-translate="discover-title">Découvrez Shop Charles</h3>
                <p data-translate="discover-text">Ici, vous pourrez bientôt découvrir tout ce que Shop Charles a à vous offrir. Un espace dédié à vos besoins et à vos envies.</p>
                <button class="boutique-btn" id="boutiqueBtnAccueil" style="margin-top: 20px;" data-translate="shop-btn">Boutique</button>
            </div>
            <p class="slogan" data-translate="slogan">"Faites-vous plaisir, c'est l'heure de se régaler !"</p>
            
            <div class="partners-section">
                <h3 data-translate="partners-title">Nos partenaires</h3>
                <p data-translate="no-partners">Pas encore de partenaires</p>
            </div>
        </main>
    </div>

    <div id="supportPage" class="support-page">
        <h1 data-translate="support-title">Support</h1>
        <div class="contact-info">
            <p data-translate="phone-label">Numéro: +509 38390831</p>
            <p data-translate="email-label">Email: Shopcharles@gmail.com</p>
        </div>
        <button class="back-btn" id="backToMainBtn" data-translate="back-btn">Retour</button>
    </div>

    <div id="donPage" class="don-page">
        <h1 data-translate="donation-title">Faire un don</h1>
        <div class="don-info">
            <p data-translate="moncash-number">Numéro de moncash: +50931102597/+50934125103</p>
            <div class="thank-message">
                <p data-translate="thanks-message">Merci infiniment pour votre générosité et votre soutien à Shop Charles !</p>
            </div>
        </div>
        <button class="back-btn" id="backFromDonBtn" data-translate="back-btn">Retour</button>
    </div>

    <div id="boutiquePage" class="boutique-page">
        <header class="boutique-header">
            <div class="boutique-logo" data-translate="site-title">Shop Charles</div>
            <div class="header-buttons">
                <button class="menu-btn" id="boutiqueMenuBtn" data-translate="menu-btn">Menu</button>
                <button class="menu-btn" id="boutiqueEntrerBtn" data-translate="enter-btn">Entrer</button>
            </div>
        </header>

        <main class="boutique-main">
            <h1 data-translate="shop-welcome">Bienvenue dans notre boutique !</h1>
            
            <div class="categories-grid">
                <button class="category-btn" data-category="bijoux" data-translate="jewelry">Bijoux</button>
                <button class="category-btn" data-category="vetement" data-translate="clothing">Vêtement</button>
                <button class="category-btn" data-category="voiture" data-translate="car">Voiture</button>
                <button class="category-btn" data-category="maison" data-translate="house">Maison</button>
                <button class="category-btn" data-category="cosmetiques" data-translate="cosmetics">Produits cosmétiques</button>
                <button class="category-btn" data-category="nourriture" data-translate="food">Nourriture en gros</button>
                <button class="category-btn" data-category="decoration" data-translate="decoration">Décoration</button>
                <button class="category-btn" data-category="meuble" data-translate="furniture">Meuble</button>
                <button class="category-btn" data-category="autres" data-translate="others">Autres</button>
            </div>

            <div class="limited-offer">
                <h2 data-translate="limited-offer">Offre limitée !</h2>
                <div class="offer-photos-grid" id="offerPhotosGrid">
                    <p>Chargement des offres spéciales...</p>
                </div>
                <button class="back-to-home" id="backToHomeFromBoutique" data-translate="back-home">Retour à l'accueil</button>
            </div>
        </main>
    </div>

    <div id="productsPage" class="products-page">
        <header class="boutique-header">
            <div class="boutique-logo" data-translate="site-title">Shop Charles</div>
            <div class="header-buttons">
                <button class="menu-btn" id="productsMenuBtn" data-translate="menu-btn">Menu</button>
                <button class="menu-btn" id="productsEntrerBtn" data-translate="enter-btn">Entrer</button>
            </div>
        </header>

        <main style="padding: 40px 20px; text-align: center;">
            <h1 id="categoryTitle" data-translate="products">Produits</h1>
            
            <div class="products-header-buttons">
                <button class="panier-btn" id="panierBtn" data-translate="cart">Panier</button>
            </div>
            
            <div id="productsContainer" class="product-photos-grid">
                <p>Chargement des produits...</p>
            </div>
            
            <button class="back-btn" id="backFromProductsBtn" data-translate="back-shop">Retour à la boutique</button>
        </main>
    </div>

    <div id="cartPage" class="cart-page">
        <h1 data-translate="select-purchases">Sélectionnez vos achats</h1>
        <p data-translate="cart-empty">Votre panier est vide pour le moment.</p>
        <button class="back-btn" id="backFromCartBtn" data-translate="back-shop">Retour à la boutique</button>
    </div>

    <div id="accountPage" class="account-page">
        <h1 data-translate="my-account">Mon compte</h1>
        <div class="account-info" id="accountInfo">
            <div class="info-item">
                <div class="info-label" data-translate="email-label">Email</div>
                <div class="info-value" id="userEmail" data-translate="not-connected">Non connecté</div>
            </div>
            <div class="info-item">
                <div class="info-label" data-translate="phone-label">Téléphone</div>
                <div class="info-value" id="userPhone" data-translate="not-provided">Non renseigné</div>
            </div>
        </div>
        <button class="back-btn" id="backFromAccountBtn" data-translate="back-btn">Retour</button>
    </div>

    <div id="purchasesPage" class="purchases-page">
        <h1 data-translate="my-purchases">Mes achats</h1>
        <p data-translate="no-purchases">Aucun achat pour le moment.</p>
        <button class="back-btn" id="backFromPurchasesBtn" data-translate="back-btn">Retour</button>
    </div>

    <div id="myModal" class="modal">
        <div class="modal-content">
            <span class="close-btn" id="closeMenuBtn">&times;</span>
            <h2 data-translate="menu-title">Menu</h2>
            <button id="supportBtn" data-translate="support-btn">Support</button>
            <button id="createAccountBtn" data-translate="create-account-btn">Créer son compte</button>
            <button id="loginBtn" data-translate="login-btn">Se connecter à son compte</button>
            <button id="donBtn" data-translate="donation-btn">Don pour Shop Charles</button>
        </div>
    </div>

    <div id="createAccountModal" class="modal">
        <div class="modal-content">
            <span class="close-btn" id="closeCreateAccountBtn">&times;</span>
            <h2 data-translate="create-account-title">Créer son compte</h2>
            <form id="createAccountForm">
                <div class="form-group">
                    <label for="email" data-translate="email-label">Email:</label>
                    <input type="email" id="email" name="email" placeholder="Votre email" required>
                </div>
                <div class="form-group">
                    <label for="phone" data-translate="phone-input-label">Numéro de téléphone:</label>
                    <input type="tel" id="phone" name="phone" placeholder="Votre numéro" required>
                </div>
                <div class="form-group">
                    <label for="password" data-translate="password-label">Mot de passe:</label>
                    <input type="password" id="password" name="password" placeholder="Votre mot de passe" required>
                </div>
                <button type="submit" style="margin-top: 20px;" data-translate="create-btn">Créer le compte</button>
            </form>
        </div>
    </div>

    <div id="loginModal" class="modal">
        <div class="modal-content">
            <span class="close-btn" id="closeLoginBtn">&times;</span>
            <h2 data-translate="login-title">Se connecter</h2>
            <form id="loginForm">
                <div class="form-group">
                    <label for="loginEmail" data-translate="email-label">Email:</label>
                    <input type="email" id="loginEmail" name="loginEmail" placeholder="Votre email" required>
                </div>
                <div class="form-group">
                    <label for="loginPassword" data-translate="password-label">Mot de passe:</label>
                    <input type="password" id="loginPassword" name="loginPassword" placeholder="Votre mot de passe" required>
                </div>
                <button type="submit" style="margin-top: 20px;" data-translate="connect-btn">Se connecter</button>
            </form>
        </div>
    </div>

    <div id="entrerModal" class="modal">
        <div class="modal-content">
            <span class="close-btn" id="closeEntrerBtn">&times;</span>
            <h2 data-translate="enter-title">Entrer</h2>
            <button id="boutiqueBtnEntrer" data-translate="shop-btn">Boutique</button>
            <button id="accountBtn" data-translate="my-account-btn">Mon compte</button>
            <button id="purchasesBtn" data-translate="my-purchases-btn">Mes achats</button>
        </div>
    </div>

    <script>
        const supabaseUrl = 'https://jesrnoejlqfzktxydoca.supabase.co';
        const supabaseKey = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Implc3Jub2VqbHFmemt0eHlkb2NhIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NTgzMTMzMzIsImV4cCI6MjA3Mzg4OTMzMn0.7dYktgAQdx6j_JoFEv8iT6VJQ476q8KKjhlbhPicnIE';
        const supabase = window.supabase.createClient(supabaseUrl, supabaseKey);

        let currentUser = null;
        let currentLanguage = 'fr';
        let currentCategory = '';

        const translations = {
            fr: {
                'site-title': 'Shop Charles', 'menu-btn': 'Menu', 'enter-btn': 'Entrer', 'welcome': 'Bienvenue !',
                'discover-title': 'Découvrez Shop Charles', 'shop-btn': 'Boutique',
                'discover-text': 'Ici, vous pourrez bientôt découvrir tout ce que Shop Charles a à vous offrir.',
                'slogan': '"Faites-vous plaisir, c\'est l\'heure de se régaler !"',
                'partners-title': 'Nos partenaires', 'no-partners': 'Pas encore de partenaires',
                'support-title': 'Support', 'back-btn': 'Retour', 'donation-title': 'Faire un don',
                'shop-welcome': 'Bienvenue dans notre boutique !', 'jewelry': 'Bijoux', 'clothing': 'Vêtement',
                'car': 'Voiture', 'house': 'Maison', 'cosmetics': 'Produits cosmétiques',
                'food': 'Nourriture en gros', 'decoration': 'Décoration', 'furniture': 'Meuble',
                'others': 'Autres', 'limited-offer': 'Offre limitée !', 'products': 'Produits',
                'cart': 'Panier', 'select-purchases': 'Sélectionnez vos achats',
                'cart-empty': 'Votre panier est vide pour le moment.', 'my-account': 'Mon compte',
                'not-connected': 'Non connecté', 'not-provided': 'Non renseigné',
                'my-purchases': 'Mes achats', 'no-purchases': 'Aucun achat pour le moment.'
            },
            cr: {
                'site-title': 'Shop Charles', 'menu-btn': 'Meni', 'enter-btn': 'Antre', 'welcome': 'Byenvini !',
                'discover-title': 'Dekouvri Shop Charles', 'shop-btn': 'Boutik',
                'discover-text': 'Isit la, nou pral jwenn tou sa Shop Charles gen pou nou an.',
                'slogan': '"Fè kè nou kontan, se lè pou nou goute !"',
                'partners-title': 'Patnè nou yo', 'no-partners': 'Poko gen patnè',
                'support-title': 'Sipò', 'back-btn': 'Tounen', 'donation-title': 'Fè yon don',
                'shop-welcome': 'Byenvini nan boutik nou an !', 'jewelry': 'Bijou', 'clothing': 'Rad',
                'car': 'Machin', 'house': 'Kay', 'cosmetics': 'Pwodwi bèlte',
                'food': 'Manje nan gwo', 'decoration': 'Dekorasyon', 'furniture': 'Mèb',
                'others': 'Lòt bagay', 'limited-offer': 'Òf limite !', 'products': 'Pwodwi',
                'cart': 'Panye', 'select-purchases': 'Chwazi acha nou yo',
                'cart-empty': 'Panye nou an vid kounye a.', 'my-account': 'Kont mwen',
                'not-connected': 'Pa konekte', 'not-provided': 'Pa bay',
                'my-purchases': 'Acha mwen yo', 'no-purchases': 'Poko gen acha.'
            }
        };

        function changeLanguage(lang) {
            currentLanguage = lang;
            document.querySelectorAll('.lang-btn').forEach(btn => {
                btn.classList.toggle('active', btn.dataset.lang === lang);
            });
            document.querySelectorAll('[data-translate]').forEach(el => {
                const key = el.getAttribute('data-translate');
                if (translations[lang][key]) {
                    if (el.tagName === 'INPUT') el.placeholder = translations[lang][key];
                    else el.textContent = translations[lang][key];
                }
            });
        }

        function showPage(pageId) {
            ['mainPage', 'supportPage', 'donPage', 'boutiquePage', 'productsPage', 'cartPage', 'accountPage', 'purchasesPage'].forEach(id => {
                const page = document.getElementById(id);
                if (page) page.style.display = id === pageId ? "block" : "none";
            });
        }

        function closeModal(modalElement) {
            modalElement.style.display = "none";
        }

        async function loadSpecialOffers() {
            const container = document.getElementById('offerPhotosGrid');
            try {
                const { data: products, error } = await supabase
                    .from('products')
                    .select('*')
                    .eq('category', 'offre-speciale')
                    .eq('status', 'active')
                    .limit(20);

                if (error) throw error;

                if (products && products.length > 0) {
                    container.innerHTML = products.map(p => `
                        <div class="offer-product-card">
                            <div class="product-image-placeholder">${getProductIcon(p.category)}</div>
                            <div class="product-name">${p.name}</div>
                            <div class="product-price">${p.price}€</div>
                        </div>
                    `).join('');
                } else {
                    container.innerHTML = '<p style="color: #666;">Aucune offre spéciale pour le moment.</p>';
                }
            } catch (error) {
                console.error('Erreur:', error);
                container.innerHTML = '<p style="color: #f56565;">Erreur de chargement</p>';
            }
        }

        async function loadProducts(category) {
            const container = document.getElementById('productsContainer');
            const title = document.getElementById('categoryTitle');
            
            container.innerHTML = '<p>Chargement des produits...</p>';
            title.textContent = getCategoryName(category);

            try {
                const { data: products, error } = await supabase
                    .from('products')
                    .select('*')
                    .eq('category', category)
                    .eq('status', 'active');

                if (error) throw error;

                if (products && products.length > 0) {
                    container.innerHTML = products.map(p => `
                        <div class="product-card">
                            <div class="product-image-placeholder">${getProductIcon(p.category)}</div>
                            <div class="product-name">${p.name}</div>
                            <div class="product-price">${p.price}€</div>
                            <p style="color: #666; font-size: 0.9em;">${p.description || ''}</p>
                        </div>
                    `).join('');
                } else {
                    container.innerHTML = '<p style="color: #666;">Aucun produit dans cette catégorie.</p>';
                }
            } catch (error) {
                console.error('Erreur:', error);
                container.innerHTML = '<p style="color: #f56565;">Erreur de chargement</p>';
            }
        }

        function getProductIcon(category) {
            const icons = {
                'bijoux': '💍', 'vetement': '👕', 'voiture': '🚗', 'maison': '🏠',
                'cosmetiques': '💄', 'nourriture': '🍽️', 'decoration': '🏺',
                'meuble': '🪑', 'autres': '📦', 'offre-speciale': '🎁'
            };
            return icons[category] || '📦';
        }

        function getCategoryName(category) {
            const names = {
                'bijoux': 'Bijoux', 'vetement': 'Vêtement', 'voiture': 'Voiture',
                'maison': 'Maison', 'cosmetiques': 'Produits cosmétiques',
                'nourriture': 'Nourriture en gros', 'decoration': 'Décoration',
                'meuble': 'Meuble', 'autres': 'Autres'
            };
            return names[category] || category;
        }

        function updateAccountInfo() {
            document.getElementById('userEmail').textContent = currentUser ? currentUser.email : 'Non connecté';
            document.getElementById('userPhone').textContent = currentUser ? (currentUser.phone || 'Non renseigné') : 'Non renseigné';
        }

        function showNotification(message, type) {
            const notif = document.createElement('div');
            notif.className = `notification ${type}`;
            notif.textContent = message;
            document.body.appendChild(notif);
            setTimeout(() => notif.remove(), 3000);
        }

        document.addEventListener('DOMContentLoaded', function() {
            document.getElementById('frBtn').onclick = () => changeLanguage('fr');
            document.getElementById('crBtn').onclick = () => changeLanguage('cr');

            document.getElementById("openMenuBtn").onclick = () => {
                closeModal(document.getElementById("entrerModal"));
                document.getElementById("myModal").style.display = "flex";
            };
            document.getElementById("openEntrerBtn").onclick = () => {
                closeModal(document.getElementById("myModal"));
                document.getElementById("entrerModal").style.display = "flex";
            };
            document.getElementById("boutiqueMenuBtn").onclick = () => {
                closeModal(document.getElementById("entrerModal"));
                document.getElementById("myModal").style.display = "flex";
            };
            document.getElementById("boutiqueEntrerBtn").onclick = () => {
                closeModal(document.getElementById("myModal"));
                document.getElementById("entrerModal").style.display = "flex";
            };
            document.getElementById("productsMenuBtn").onclick = () => {
                closeModal(document.getElementById("entrerModal"));
                document.getElementById("myModal").style.display = "flex";
            };
            document.getElementById("productsEntrerBtn").onclick = () => {
                closeModal(document.getElementById("myModal"));
                document.getElementById("entrerModal").style.display = "flex";
            };

            document.getElementById("closeMenuBtn").onclick = () => closeModal(document.getElementById("myModal"));
            document.getElementById("closeEntrerBtn").onclick = () => closeModal(document.getElementById("entrerModal"));
            document.getElementById("closeCreateAccountBtn").onclick = () => closeModal(document.getElementById("createAccountModal"));
            document.getElementById("closeLoginBtn").onclick = () => closeModal(document.getElementById("loginModal"));

            document.getElementById("supportBtn").onclick = () => { closeModal(document.getElementById("myModal")); showPage("supportPage"); };
            document.getElementById("createAccountBtn").onclick = () => { closeModal(document.getElementById("myModal")); document.getElementById("createAccountModal").style.display = "flex"; };
            document.getElementById("loginBtn").onclick = () => { closeModal(document.getElementById("myModal")); document.getElementById("loginModal").style.display = "flex"; };
            document.getElementById("donBtn").onclick = () => { closeModal(document.getElementById("myModal")); showPage("donPage"); };
            document.getElementById("boutiqueBtnAccueil").onclick = () => { showPage("boutiquePage"); loadSpecialOffers(); };
            document.getElementById("boutiqueBtnEntrer").onclick = () => { closeModal(document.getElementById("entrerModal")); showPage("boutiquePage"); loadSpecialOffers(); };
            document.getElementById("accountBtn").onclick = () => { closeModal(document.getElementById("entrerModal")); showPage("accountPage"); updateAccountInfo(); };
            document.getElementById("purchasesBtn").onclick = () => { closeModal(document.getElementById("entrerModal")); showPage("purchasesPage"); };

            document.getElementById("backToMainBtn").onclick = () => showPage("mainPage");
            document.getElementById("backFromDonBtn").onclick = () => showPage("mainPage");
            document.getElementById("backToHomeFromBoutique").onclick = () => showPage("mainPage");
            document.getElementById("backFromProductsBtn").onclick = () => showPage("boutiquePage");
            document.getElementById("backFromCartBtn").onclick = () => showPage("productsPage");
            document.getElementById("backFromAccountBtn").onclick = () => showPage("mainPage");
            document.getElementById("backFromPurchasesBtn").onclick = () => showPage("mainPage");

            document.querySelectorAll(".category-btn").forEach(button => {
                button.onclick = () => {
                    currentCategory = button.getAttribute('data-category');
                    loadProducts(currentCategory);
                    showPage('productsPage');
                };
            });

            document.getElementById("panierBtn").onclick = () => showPage("cartPage");

            document.getElementById('createAccountForm').addEventListener('submit', async function(e) {
                e.preventDefault();
                try {
                    const { error } = await supabase.from('users').insert([{
                        email: document.getElementById('email').value,
                        phone: document.getElementById('phone').value,
                        password_hash: btoa(document.getElementById('password').value)
                    }]);
                    if (error) throw error;
                    showNotification('Compte créé avec succès !', 'success');
                    closeModal(document.getElementById("createAccountModal"));
                    this.reset();
                } catch (error) {
                    showNotification('Erreur lors de la création', 'error');
                }
            });

            document.getElementById('loginForm').addEventListener('submit', async function(e) {
                e.preventDefault();
                try {
                    const { data: users, error } = await supabase.from('users').select('*')
                        .eq('email', document.getElementById('loginEmail').value)
                        .eq('password_hash', btoa(document.getElementById('loginPassword').value));
                    if (error || !users.length) throw new Error('Identifiants incorrects');
                    currentUser = users[0];
                    showNotification(`Bienvenue ${currentUser.email} !`, 'success');
                    closeModal(document.getElementById("loginModal"));
                    this.reset();
                } catch (error) {
                    showNotification('Email ou mot de passe incorrect', 'error');
                }
            });

            window.onclick = function(event) {
                [document.getElementById("myModal"), document.getElementById("entrerModal"),
                 document.getElementById("createAccountModal"), document.getElementById("loginModal")].forEach(modal => {
                    if (event.target === modal) closeModal(modal);
                });
            }
        });
    </script>
</body>
</html>
