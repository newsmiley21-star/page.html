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
        .product-page { display: none; }
        .product-page.active { display: grid; }
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
                <li><button onclick="showPage('cart')" class="hover:text-green-700">Fret Maritime / Aérien</button></li>
                <li><button onclick="showPage('faq')" class="hover:text-green-700">FAQ</button></li>
            </ul>
        </nav>
    </header>

    <!-- Bannière Enseigne -->
    <section class="bg-white py-6 text-center">
        <div class="flex flex-wrap justify-center items-center gap-4">
            <img src="https://i.ibb.co/Xk4h5wFJ/Capture-d-cran-2026-06-09-231624.png" alt="Bannière 1" class="max-h-48">
            <img src="https://i.ibb.co/sdbMDY94/Capture-d-cran-2026-06-09-231508.png" alt="Bannière 2" class="max-h-48">
        </div>
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
            <h2 class="text-3xl font-bold mb-6 text-center">Nos Services</h2>
            <div class="grid md:grid-cols-3 gap-6">
                <a href="https://consultation-html.pages.dev/" target="_blank" class="block p-6 bg-blue-50 hover:bg-blue-100 rounded-lg border border-blue-200 transition text-center">
                    <i class="fas fa-search text-3xl text-blue-600 mb-3"></i>
                    <h3 class="font-bold text-blue-900 text-lg mb-2">Suivi des livraisons</h3>
                </a>
                <a href="https://1-2-html.pages.dev/" target="_blank" class="block p-6 bg-emerald-50 hover:bg-emerald-100 rounded-lg border border-emerald-200 transition text-center">
                    <i class="fas fa-truck text-3xl text-emerald-600 mb-3"></i>
                    <h3 class="font-bold text-emerald-900 text-lg mb-2">Nouvelle commande</h3>
                </a>
                <a href="https://index-html-2qm.pages.dev/" target="_blank" class="block p-6 bg-orange-50 hover:bg-orange-100 rounded-lg border border-orange-200 transition text-center">
                    <i class="fas fa-user-tie text-3xl text-orange-600 mb-3"></i>
                    <h3 class="font-bold text-orange-900 text-lg mb-2">Espace livreurs</h3>
                </a>
            </div>
        </section>

        <!-- Qui sommes-nous -->
        <section id="about" class="page-section bg-white p-8 rounded shadow-sm">
            <h2 class="text-3xl font-bold mb-6 text-green-700">Qui sommes-nous ?</h2>
            <div class="space-y-6 text-slate-700">
                <p>Bienvenue chez <strong>X-PRESS LIVRAISON</strong>, votre solution privilégiée pour la livraison au Gabon. Comme l'illustre notre identité visuelle, nous avons placé la rapidité et l'efficacité au cœur de notre ADN.</p>
                
                <div class="bg-slate-50 p-6 rounded-lg border border-slate-200">
                    <h3 class="text-xl font-bold mb-4 text-blue-900">Contactez-nous</h3>
                    <p><i class="fas fa-phone mr-2 text-green-600"></i> <strong>077 73 60 65</strong> / <strong>066 13 98 46</strong></p>
                    <p><i class="fas fa-envelope mr-2 text-blue-600"></i> X-presslivraisoncommercial@gmail.com</p>
                </div>

                <h3 class="text-xl font-bold text-blue-900">Pourquoi choisir X-PRESS LIVRAISON ?</h3>
                <ul class="list-disc ml-5 space-y-2">
                    <li><strong>Expertise locale :</strong> Maîtrise de la géographie du Grand Libreville.</li>
                    <li><strong>Polyvalence :</strong> Tous types de colis.</li>
                    <li><strong>Technologie :</strong> Système de suivi par identifiant.</li>
                    <li><strong>Sécurité :</strong> Protection des données sensibles.</li>
                </ul>
            </div>
        </section>

        <!-- Produits -->
        <section id="products" class="page-section bg-white p-8 rounded shadow-sm">
            <h2 class="text-3xl font-bold mb-8 text-center">Notre Vitrine</h2>
            <div id="page-products-1" class="product-page active grid grid-cols-2 md:grid-cols-4 gap-6">
                <div class="border rounded-lg p-4 hover:shadow-md transition">
                    <div class="bg-slate-100 h-32 flex items-center justify-center rounded mb-4"><span class="text-4xl text-slate-300 font-light">+</span></div>
                    <button onclick="showPage('commander')" class="w-full bg-blue-900 text-white text-xs py-2 rounded">Commander</button>
                </div>
            </div>
            <div id="pagination-container" class="mt-10 flex justify-center items-center gap-4 text-sm text-blue-900 font-semibold">
                <button onclick="changeProductPage(1)" class="page-btn border px-3 py-1 rounded bg-blue-900 text-white">1</button>
            </div>
        </section>

        <!-- FAQ -->
        <section id="faq" class="page-section bg-white p-8 rounded shadow-sm">
            <h2 class="text-3xl font-bold mb-6 text-green-700">FAQ - Questions Fréquentes</h2>
            <div class="space-y-4">
                <details class="bg-slate-50 p-4 rounded-lg">
                    <summary class="font-bold cursor-pointer">Comment suivre mon colis ?</summary>
                    <p class="mt-2 text-slate-600">Utilisez votre code CT241 dans l'onglet "Suivi des livraisons" pour voir le statut en temps réel.</p>
                </details>
                <details class="bg-slate-50 p-4 rounded-lg">
                    <summary class="font-bold cursor-pointer">Quels sont les délais de livraison ?</summary>
                    <p class="mt-2 text-slate-600">Nos trajets sont optimisés pour assurer la livraison la plus rapide possible dans le Grand Libreville.</p>
                </details>
                <details class="bg-slate-50 p-4 rounded-lg">
                    <summary class="font-bold cursor-pointer">Que faire en cas d'erreur de paiement ?</summary>
                    <p class="mt-2 text-slate-600">Notre service client sera averti automatiquement et prendra contact avec vous pour régulariser la situation.</p>
                </details>
            </div>
        </section>

        <!-- Fret Maritime / Aérien -->
        <section id="cart" class="page-section bg-white p-8 rounded shadow-sm">
            <h2 class="text-3xl font-bold mb-4">Fret Maritime & Aérien</h2>
            <p>Solutions de transport international sur mesure.</p>
        </section>

        <!-- Section Commander -->
        <section id="commander" class="page-section bg-white p-8 rounded shadow-sm">
            <h2 class="text-3xl font-bold mb-4">Nouvelle Commande</h2>
            <p>Formulaire...</p>
        </section>

        <section id="suivi" class="page-section bg-white p-8 rounded shadow-sm">
            <h2 class="text-3xl font-bold mb-4">Suivi de Colis</h2>
            <p>Veuillez entrer votre numéro de suivi (CT241).</p>
        </section>

        <!-- WhatsApp -->
        <section class="mt-12 text-center py-10 bg-orange-100 rounded-lg">
            <p class="mb-4 font-bold">CLIQUEZ ICI POUR REJOINDRE NOTRE COMMUNAUTÉ</p>
            <a href="https://wa.me/77736065" target="_blank" class="bg-green-500 text-white px-8 py-3 rounded-full font-bold inline-flex items-center hover:bg-green-600 transition">
                <i class="fab fa-whatsapp mr-2 text-xl"></i> WhatsApp
            </a>
        </section>
    </main>

    <footer class="text-center py-10 text-slate-500 text-sm">
        &copy; 2026 X-press livraison gabon - Tous droits réservés.
        <br> Contact : 077 73 60 65 - X-presslivraisoncommercial@gmail.com
    </footer>

    <script>
        function showPage(pageId) {
            document.querySelectorAll('.page-section').forEach(section => section.classList.remove('active'));
            document.getElementById(pageId).classList.add('active');
        }

        function changeProductPage(pageNumber) {
            document.querySelectorAll('.product-page').forEach(page => {
                page.classList.remove('active');
                page.classList.add('hidden');
            });
            document.getElementById('page-products-' + pageNumber).classList.remove('hidden');
            document.getElementById('page-products-' + pageNumber).classList.add('active');
            
            document.querySelectorAll('.page-btn').forEach(btn => {
                btn.classList.remove('bg-blue-900', 'text-white');
                btn.classList.add('hover:bg-blue-50');
            });
            event.currentTarget.classList.add('bg-blue-900', 'text-white');
        }
    </script>
</body>
</html>
