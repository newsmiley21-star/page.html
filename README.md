<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>X-press livraison Gabon</title>
    <!-- Logo/Favicon -->
    <link rel="icon" href="https://i.ibb.co/TMtfZB1Q/Gemini-Generated-Image-cac734cac734cac7.png" type="image/x-icon">
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    
    <style>
        /* CSS pour gérer le basculement des sections */
        .page-section { display: none; }
        .page-section.active { display: block; }
    </style>
</head>
<body class="bg-slate-50 text-slate-900 font-sans">

    <!-- En-tête -->
    <header class="bg-white border-b border-slate-200">
        <nav class="max-w-7xl mx-auto px-4 py-4 flex justify-between items-center">
            <div class="flex items-center space-x-2">
                <!-- Logo mis à jour -->
                <img src="https://i.ibb.co/TMtfZB1Q/Gemini-Generated-Image-cac734cac734cac7.png" alt="Logo X-Press" class="h-10">
            </div>
            <ul class="flex space-x-6 text-sm font-semibold text-slate-600">
                <li><button onclick="showPage('accueil')" class="hover:text-green-700">Accueil</button></li>
                <li><button onclick="showPage('services')" class="hover:text-green-700">Nos services</button></li>
                <li><button onclick="showPage('about')" class="hover:text-green-700">Qui sommes-nous?</button></li>
                <li><button onclick="showPage('products')" class="hover:text-green-700">Nos produits</button></li>
                <li><button onclick="showPage('cart')" class="hover:text-green-700">Mon panier</button></li>
            </ul>
        </nav>
    </header>

    <!-- Bannière Enseigne -->
    <section class="bg-white py-10 text-center">
        <!-- Enseigne mise à jour -->
        <img src="https://i.ibb.co/Xk4h5wFJ/Capture-d-cran-2026-06-09-231624.png" alt="X-Press Livraison" class="mx-90 max-h-58 mb-4">
        <img src="https://i.ibb.co/sdbMDY94/Capture-d-cran-2026-06-09-231508.png" alt="X-Press Livraison" class="mx-auto max-h-58 mb-4">
    </section>

    <!-- Boutons d'action rapides -->
    <div class="flex justify-center gap-4 py-4 border-b border-slate-200">
        <button onclick="showPage('commander')" class="bg-blue-900 text-white px-6 py-2 rounded shadow hover:bg-blue-800 transition">Demander une livraison</button>
        <button onclick="showPage('suivi')" class="bg-white border border-slate-300 px-6 py-2 rounded shadow hover:bg-slate-100 transition">Suivre un colis 📦</button>
    </div>

    <!-- CONTENU DYNAMIQUE -->
    <main class="max-w-6xl mx-auto py-12 px-6">
        
        <!-- Accueil -->
        <section id="accueil" class="page-section active bg-white p-8 rounded shadow-sm">
            <h2 class="text-3xl font-bold mb-4">Bienvenue chez X-PRESS</h2>
            <p>La rapidité au service de vos envois. Sélectionnez une option dans le menu ou utilisez les boutons ci-dessus.</p>
        </section>

        <!-- Services -->
        <section id="services" class="page-section bg-white p-8 rounded shadow-sm">
            <h2 class="text-3xl font-bold mb-4">Nos Services</h2>
            <p>Découvrez nos solutions de transport express, livraison sécurisée et logistique pour entreprises.</p>
        </section>

        <!-- Qui sommes-nous -->
        <section id="about" class="page-section bg-white p-8 rounded shadow-sm">
            <h2 class="text-3xl font-bold mb-4">Qui sommes-nous ?</h2>
            <p>X-PRESS LIVRAISON est votre partenaire de confiance au Gabon.</p>
        </section>

        <!-- Produits -->
        <section id="products" class="page-section bg-white p-8 rounded shadow-sm">
            <h2 class="text-3xl font-bold mb-4">Nos Produits</h2>
            <p>Découvrez nos emballages, nos formules d'abonnement et nos solutions de transport.</p>
        </section>

        <!-- Panier -->
        <section id="cart" class="page-section bg-white p-8 rounded shadow-sm">
            <h2 class="text-3xl font-bold mb-4">Mon Panier</h2>
            <p>Gérez vos commandes en attente de paiement ici.</p>
        </section>

        <!-- Section Commander (Cachée par défaut) -->
        <section id="commander" class="page-section bg-white p-8 rounded shadow-sm">
            <h2 class="text-3xl font-bold mb-4">Nouvelle Commande</h2>
            <p>Formulaire de commande à remplir ici...</p>
        </section>

        <!-- Section Suivi (Cachée par défaut) -->
        <section id="suivi" class="page-section bg-white p-8 rounded shadow-sm">
            <h2 class="text-3xl font-bold mb-4">Suivi de Colis</h2>
            <p>Entrez votre numéro de suivi pour localiser votre colis.</p>
        </section>

        <!-- Section WhatsApp -->
        <section class="mt-12 text-center py-10 bg-orange-100 rounded-lg">
            <p class="mb-4 font-bold">CLIQUEZ ICI POUR REJOINDRE NOTRE COMMUNAUTÉ</p>
            <a href="https://wa.me/77736065" target="_blank" class="bg-green-500 text-white px-8 py-3 rounded-full font-bold inline-flex items-center hover:bg-green-600 transition">
                <i class="fab fa-whatsapp mr-2 text-xl"></i> WhatsApp
            </a>
        </section>
    </main>

    <!-- Pied de page -->
    <footer class="text-center py-10 text-slate-500 text-sm">
        &copy; 2026 X-press livraison gabon - Tous droits réservés.
    </footer>

    <!-- Script pour la navigation -->
    <script>
        function showPage(pageId) {
            // Cacher toutes les sections
            document.querySelectorAll('.page-section').forEach(section => {
                section.classList.remove('active');
            });
            // Afficher la section demandée
            document.getElementById(pageId).classList.add('active');
        }
    </script>
</body>
</html>
