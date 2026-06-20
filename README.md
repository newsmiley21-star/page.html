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
        .page-section { display: none; }
        .page-section.active { display: block; }
    </style>
</head>
<body class="bg-slate-50 text-slate-900 font-sans">

    <!-- En-tête -->
    <header class="bg-white border-b border-slate-200">
        <nav class="max-w-7xl mx-auto px-4 py-4 flex justify-between items-center">
            <div class="flex items-center space-x-2">
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
    <section class="bg-white py-6 text-center">
        <img src="https://i.ibb.co/Xk4h5wFJ/Capture-d-cran-2026-06-09-231624.png" alt="Bannière 1" class="mx-auto max-h-48 mb-2">
        <img src="https://i.ibb.co/sdbMDY94/Capture-d-cran-2026-06-09-231508.png" alt="Bannière 2" class="mx-auto max-h-48 mb-4">
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

        <!-- Services (Mis à jour avec vos liens) -->
        <section id="services" class="page-section bg-white p-8 rounded shadow-sm">
            <h2 class="text-3xl font-bold mb-6 text-center">Nos Services</h2>
            <div class="grid md:grid-cols-3 gap-6">
                <a href="https://consultation-html.pages.dev/" target="_blank" class="block p-6 bg-blue-50 hover:bg-blue-100 rounded-lg border border-blue-200 transition text-center">
                    <i class="fas fa-search text-3xl text-blue-600 mb-3"></i>
                    <h3 class="font-bold text-blue-900 text-lg mb-2">Suivi des livraisons</h3>
                    <p class="text-sm text-blue-700">Consultez l'état de votre colis en temps réel.</p>
                </a>
                <a href="https://1-2-html.pages.dev/" target="_blank" class="block p-6 bg-emerald-50 hover:bg-emerald-100 rounded-lg border border-emerald-200 transition text-center">
                    <i class="fas fa-truck text-3xl text-emerald-600 mb-3"></i>
                    <h3 class="font-bold text-emerald-900 text-lg mb-2">Nouvelle commande</h3>
                    <p class="text-sm text-emerald-700">Effectuez une demande de livraison rapidement.</p>
                </a>
                <a href="https://index-html-2qm.pages.dev/" target="_blank" class="block p-6 bg-orange-50 hover:bg-orange-100 rounded-lg border border-orange-200 transition text-center">
                    <i class="fas fa-user-tie text-3xl text-orange-600 mb-3"></i>
                    <h3 class="font-bold text-orange-900 text-lg mb-2">Espace livreurs</h3>
                    <p class="text-sm text-orange-700">Accès réservé aux partenaires logistiques.</p>
                </a>
            </div>
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

        <!-- Section Commander -->
        <section id="commander" class="page-section bg-white p-8 rounded shadow-sm">
            <h2 class="text-3xl font-bold mb-4">Nouvelle Commande</h2>
            <p>Formulaire de commande à remplir ici...</p>
        </section>

        <!-- Section Suivi -->
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
            document.querySelectorAll('.page-section').forEach(section => {
                section.classList.remove('active');
            });
            document.getElementById(pageId).classList.add('active');
        }
    </script>
</body>
</html>
