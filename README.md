<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CT241 - Gestionnaire de Bordereaux</title>
    
    <!-- PWA Meta Tags -->
    <meta name="theme-color" content="#007d40">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <link rel="apple-touch-icon" href="https://i.ibb.co/xKY76DgR/Gemini-Generated-Image-1pvtp31pvtp31pvt-1.png">
    
    <!-- Manifeste Web -->
    <link rel="manifest" href="data:application/json;base64,ewogICJuYW1lIjogIkNUMjQxIExvZ2lzdGljcyIsCiAgInNob3J0X25hbWUiOiAiQ1QyNDEiLAogICJzdGFydF91cmwiOiAiLiIsCiAgImRpc3BsYXkiOiAic3RhbmRhbG9uZSIsCiAgImJhY2tncm91bmRfY29sb3IiOiAiI2Y4ZmFmYyIsCiAgInRoZW1lX2NvbG9yIjogIiMwMDdkNDAiLAogICJpY29ucyI6IFsKICAgIHsKICAgICAgInNyYyI6ICJodHRwczovL2kuaWJiLmNvL3hLWTc2RGdSL0dlbWluaS1HZW5lcmF0ZWQtSW1hZ2UtMXB2dHAzMXB2dHAzMXB2dC0xLnBuZyIsCiAgICAgICJzaXplcyI6ICI1MTJ4NTEyIiwKICAgICAgInR5cGUiOiAiaW1hZ2UvcG5nIgogICAgfQogIF0KfQ==">

    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;900&display=swap');
        body { font-family: 'Inter', sans-serif; background-color: #f8fafc; }
        .receipt-card { width: 450px; background: white; border-radius: 16px; overflow: hidden; box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1); }
        input, select, textarea { background-color: #f1f5f9 !important; border: 1px solid #e2e8f0 !important; }
        .urgent-badge { background: #fee2e2; color: #ef4444; border: 1px solid #fca5a5; }
        @media (max-width: 480px) { .receipt-card { width: 100%; } }
        
        .logo-img { max-height: 52px; width: auto; object-fit: contain; }
        .ticket-logo-box {
            background: white; padding: 4px; border-radius: 8px;
            display: flex; align-items: center; justify-content: center;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
    </style>
</head>
<body class="p-4 md:p-8">

    <div id="installBanner" class="hidden mb-6 bg-[#007d40] p-4 rounded-xl text-white flex justify-between items-center shadow-lg">
        <div class="flex items-center space-x-3">
            <img src="https://i.ibb.co/xKY76DgR/Gemini-Generated-Image-1pvtp31pvtp31pvt-1.png" class="w-10 h-10 rounded-lg object-cover border border-white/20" alt="Logo CT241">
            <div>
                <p class="text-sm font-bold">Installer CT241 ?</p>
                <p class="text-xs opacity-80">Gérez vos livraisons plus vite</p>
            </div>
        </div>
        <button id="installBtn" class="bg-white text-[#007d40] px-4 py-2 rounded-lg font-black text-xs uppercase">Installer</button>
    </div>

    <div class="max-w-6xl mx-auto grid grid-cols-1 lg:grid-cols-2 gap-10">
        
        <!-- FORMULAIRE DE SAISIE -->
        <div class="bg-white p-6 md:p-8 rounded-2xl shadow-xl border border-gray-100">
            <div class="flex items-center justify-between mb-8">
                <div class="flex items-center space-x-3">
                    <img src="https://i.ibb.co/xKY76DgR/Gemini-Generated-Image-1pvtp31pvtp31pvt-1.png" class="logo-img" alt="CT241 Logo">
                    <div>
                        <h2 class="text-xl font-black text-gray-800 tracking-tighter uppercase leading-none">Logistique</h2>
                        <p class="text-[10px] font-bold text-[#007d40]">GESTIONNAIRE DE BORDEREAUX</p>
                    </div>
                </div>
            </div>
            
            <form id="bordereauForm" class="space-y-6">
                <!-- Expédition Enrichie -->
                <div class="space-y-4 border-l-4 border-[#007d40] pl-4">
                    <h3 class="text-xs font-black text-[#007d40] uppercase tracking-widest">Informations Expéditeur</h3>
                    <div class="grid grid-cols-2 gap-4">
                        <input type="text" id="vendeur" placeholder="Boutique / Vendeur" class="w-full p-2.5 rounded-lg outline-none">
                        <input type="tel" id="vendeurTel" placeholder="Contact Expéditeur" class="w-full p-2.5 rounded-lg outline-none">
                    </div>
                    <div class="grid grid-cols-2 gap-4">
                        <input type="text" id="produit" placeholder="Désignation (ex: Robe, iPhone...)" class="w-full p-2.5 rounded-lg outline-none">
                        <div class="flex items-center bg-[#f1f5f9] rounded-lg px-2 border border-[#e2e8f0]">
                            <span class="text-[10px] font-bold text-gray-400 mr-2 uppercase">Nbr:</span>
                            <input type="number" id="nbColis" value="1" min="1" class="w-full p-2.5 bg-transparent outline-none">
                        </div>
                    </div>
                </div>

                <!-- Destinataire -->
                <div class="space-y-4 border-l-4 border-gray-300 pl-4">
                    <h3 class="text-xs font-black text-gray-500 uppercase tracking-widest">Destinataire</h3>
                    <div class="grid grid-cols-2 gap-4">
                        <input type="text" id="client" placeholder="Nom du Client" class="w-full p-2.5 rounded-lg outline-none">
                        <input type="tel" id="telephone" placeholder="Téléphone Client" class="w-full p-2.5 rounded-lg outline-none">
                    </div>
                    <div class="grid grid-cols-2 gap-4">
                        <select id="quartier" class="w-full p-2.5 rounded-lg outline-none">
                            <option value="Libreville">Libreville (Centre)</option>
                            <option value="Akanda">Akanda</option>
                            <option value="Nzeng-ayong">Nzeng-ayong</option>
                            <option value="Owendo">Owendo</option>
                            <option value="Charbonnages">Charbonnages</option>
                            <option value="Awendjé">Awendjé</option>
                            <option value="Ntoum">Ntoum</option>
                            <option value="PK5 à PK12">PK5 à PK12</option>
                        </select>
                        <input type="text" id="precision" placeholder="Point de repère" class="w-full p-2.5 rounded-lg outline-none">
                    </div>
                </div>

                <!-- Paiement -->
                <div class="space-y-4 border-l-4 border-[#eab308] pl-4">
                    <h3 class="text-xs font-black text-[#eab308] uppercase tracking-widest">Paiement</h3>
                    <div class="grid grid-cols-3 gap-4">
                        <input type="number" id="prix" placeholder="Prix Colis" value="0" class="w-full p-2.5 rounded-lg outline-none">
                        <input type="number" id="livraison" placeholder="Frais Liv." value="2000" class="w-full p-2.5 rounded-lg outline-none">
                        <select id="modePay" class="w-full p-2.5 rounded-lg outline-none text-xs">
                            <option value="COD">À encaisser</option>
                            <option value="PREPAID">Payé d'avance</option>
                            <option value="MOBILE">Mobile Money</option>
                            <option value="AIRTEL">Airtel Money</option>
                        </select>
                    </div>
                </div>

                <!-- Options -->
                <div class="bg-gray-50 p-4 rounded-xl space-y-3">
                    <p class="text-[10px] font-black text-gray-400 uppercase">Service & Sécurité</p>
                    <div class="flex flex-wrap gap-4">
                        <div class="flex items-center">
                            <input type="radio" name="service" id="urgent" value="Express" class="w-4 h-4">
                            <label for="urgent" class="ml-2 text-xs font-bold text-red-600">EXPRESS ⚡</label>
                        </div>
                        <div class="flex items-center">
                            <input type="radio" name="service" id="standard" value="Standard" checked class="w-4 h-4 text-[#007d40]">
                            <label for="standard" class="ml-2 text-xs font-bold">STANDARD</label>
                        </div>
                        <div class="flex items-center">
                            <input type="checkbox" id="fragile" class="w-4 h-4 text-orange-600">
                            <label for="fragile" class="ml-2 text-xs font-bold text-orange-600">FRAGILE 🍷</label>
                        </div>
                    </div>
                </div>

                <button type="button" onclick="generateBordereau()" class="w-full bg-[#007d40] text-white font-black py-4 rounded-xl hover:bg-[#005c2f] shadow-xl transition active:scale-95">
                    GÉNÉRER & ENVOYER WHATSAPP
                </button>
            </form>
        </div>

        <!-- APERÇU TICKET -->
        <div class="flex flex-col items-center">
            <div id="captureArea" class="receipt-card border border-gray-200">
                <!-- Header -->
                <div class="bg-slate-900 p-5 text-white flex justify-between items-center relative overflow-hidden">
                    <div class="z-10 flex flex-col space-y-2">
                        <div class="ticket-logo-box">
                            <img src="https://i.ibb.co/xKY76DgR/Gemini-Generated-Image-1pvtp31pvtp31pvt-1.png" class="h-10 w-auto" alt="Logo CT241">
                        </div>
                        <p class="text-[10px] font-bold text-blue-400 uppercase tracking-tighter" id="display-id">#ID-PROVISOIRE</p>
                    </div>
                    <div id="badge-urgent" class="hidden px-2 py-1 urgent-badge rounded text-[10px] font-black uppercase tracking-tighter z-10">Express ⚡</div>
                    <div class="absolute bottom-0 left-0 w-full h-1 bg-[#007d40]"></div>
                </div>
                
                <div class="p-6 space-y-5">
                    <!-- Expéditeur Enrichi sur le Ticket -->
                    <div class="flex justify-between items-start border-b border-dashed pb-4">
                        <div class="max-w-[70%]">
                            <p class="text-[9px] text-gray-400 font-bold uppercase tracking-widest">Expéditeur</p>
                            <p class="font-black text-gray-800 text-lg leading-tight" id="display-vendeur">---</p>
                            <p class="text-[10px] text-gray-500 font-bold" id="display-vendeurTel">Tél: ---</p>
                        </div>
                        <div class="text-right">
                            <p class="text-[9px] text-gray-400 font-bold uppercase">Date</p>
                            <p class="text-[10px] font-bold text-gray-800" id="display-date">--/--/--</p>
                        </div>
                    </div>

                    <!-- Détails Colis -->
                    <div class="bg-slate-50 p-3 rounded-lg flex justify-between items-center">
                        <div>
                            <p class="text-[9px] text-gray-400 font-bold uppercase italic leading-none mb-1">Désignation Colis</p>
                            <p class="text-xs font-black text-gray-700 uppercase" id="display-produit">---</p>
                        </div>
                        <div class="text-center bg-white px-3 py-1 rounded border border-gray-200 shadow-sm">
                            <p class="text-[8px] text-gray-400 font-bold uppercase">Nombre</p>
                            <p class="text-sm font-black text-[#007d40]" id="display-nbColis">1</p>
                        </div>
                    </div>

                    <!-- Client & Destination -->
                    <div class="grid grid-cols-2 gap-4">
                        <div class="space-y-1">
                            <p class="text-[9px] text-gray-400 font-bold uppercase italic">Destinataire</p>
                            <p class="text-sm font-black text-gray-800" id="display-client">---</p>
                            <p class="text-xs font-bold text-gray-600" id="display-tel">---</p>
                        </div>
                        <div class="space-y-1">
                            <p class="text-[9px] text-gray-400 font-bold uppercase italic">Destination</p>
                            <p class="text-sm font-black text-gray-800" id="display-destination">---</p>
                            <p class="text-[10px] leading-tight text-gray-500 font-medium italic" id="display-precision">---</p>
                        </div>
                    </div>

                    <!-- Finance -->
                    <div class="bg-green-50 p-5 rounded-2xl border border-green-100 flex justify-between items-center relative overflow-hidden">
                        <div class="absolute top-0 right-0 w-1.5 h-full bg-[#eab308]"></div>
                        <div>
                            <p class="text-[10px] font-bold text-green-700 uppercase">Somme à percevoir</p>
                            <p class="text-2xl font-black text-[#007d40]" id="display-total">0 FCFA</p>
                        </div>
                        <div class="text-right">
                            <p class="text-[9px] font-black text-white bg-[#007d40] px-2 py-0.5 rounded uppercase" id="display-mode">COD</p>
                            <p class="text-[8px] text-gray-400 font-bold mt-1" id="badge-fragile">Fragile: NON</p>
                        </div>
                    </div>

                    <!-- Footer QR & Consignes -->
                    <div class="flex items-center justify-between pt-2">
                        <div class="w-2/3">
                            <p class="text-[9px] font-black text-gray-800 uppercase underline decoration-[#007d40]">Consignes Livreur</p>
                            <p class="text-[9px] text-gray-500 leading-tight mt-1 italic">Appeler avant départ. Vérifier l'état du colis avec le client. Encaissement avant remise.</p>
                        </div>
                        <div class="flex flex-col items-center">
                            <div id="qrcode" class="p-1 bg-white border border-gray-200 rounded"></div>
                            <p class="text-[8px] font-black text-gray-400 mt-1 uppercase">ID LIVRAISON</p>
                        </div>
                    </div>
                </div>
                
                <div class="bg-slate-100 p-2 text-center border-t border-dashed border-gray-300">
                    <p class="text-[8px] text-gray-400 font-bold uppercase tracking-[0.2em]">CT241 • LOGISTIQUE GABON</p>
                </div>
            </div>

            <button id="downloadBtn" onclick="downloadImage()" class="hidden w-full mt-6 bg-[#eab308] text-black py-3 rounded-xl font-black shadow-lg uppercase text-sm border-b-4 border-[#ca8a04] active:border-b-0 active:translate-y-1 transition-all">
                📥 Enregistrer l'image du Bordereau
            </button>
        </div>
    </div>

    <script>
        // --- LOGIQUE PWA ---
        let deferredPrompt;
        window.addEventListener('beforeinstallprompt', (e) => {
            e.preventDefault();
            deferredPrompt = e;
            document.getElementById('installBanner').classList.remove('hidden');
        });

        document.getElementById('installBtn').addEventListener('click', async () => {
            if (deferredPrompt) {
                deferredPrompt.prompt();
                const { outcome } = await deferredPrompt.userChoice;
                if (outcome === 'accepted') document.getElementById('installBanner').classList.add('hidden');
                deferredPrompt = null;
            }
        });

        // --- LOGIQUE CORE ---
        function generateShortId() {
            const date = new Date();
            const year = date.getFullYear().toString().substr(-2);
            const random = Math.random().toString(36).substr(2, 4).toUpperCase();
            return `CT-${year}${random}`;
        }

        function generateBordereau() {
            // Saisie
            const vendeur = document.getElementById('vendeur').value || '---';
            const vendeurTel = document.getElementById('vendeurTel').value || '---';
            const produit = document.getElementById('produit').value || 'COLIS STANDARD';
            const nbColis = document.getElementById('nbColis').value || '1';
            const client = document.getElementById('client').value || '---';
            const tel = document.getElementById('telephone').value || '---';
            const destination = document.getElementById('quartier').value;
            const precision = document.getElementById('precision').value || '---';
            const prix = parseInt(document.getElementById('prix').value) || 0;
            const livraison = parseInt(document.getElementById('livraison').value) || 0;
            const mode = document.getElementById('modePay').value;
            const isFragile = document.getElementById('fragile').checked;
            const service = document.querySelector('input[name="service"]:checked').value;

            const total = prix + livraison;
            const uniqueId = generateShortId();
            const dateStr = new Date().toLocaleDateString('fr-FR', { day: '2-digit', month: '2-digit', year: 'numeric', hour: '2-digit', minute: '2-digit' });

            // Affichage Ticket
            document.getElementById('display-vendeur').innerText = vendeur.toUpperCase();
            document.getElementById('display-vendeurTel').innerText = "Tél: " + vendeurTel;
            document.getElementById('display-produit').innerText = produit;
            document.getElementById('display-nbColis').innerText = nbColis;
            document.getElementById('display-client').innerText = client;
            document.getElementById('display-tel').innerText = tel;
            document.getElementById('display-destination').innerText = destination;
            document.getElementById('display-precision').innerText = "📍 " + precision;
            document.getElementById('display-total').innerText = total.toLocaleString() + " FCFA";
            document.getElementById('display-id').innerText = "#" + uniqueId;
            document.getElementById('display-date').innerText = dateStr;
            
            let modeText = "CASH";
            if(mode === 'PREPAID') modeText = "PAYÉ";
            if(mode === 'MOBILE') modeText = "M-MONEY";
            if(mode === 'AIRTEL') modeText = "AIRTEL";
            document.getElementById('display-mode').innerText = modeText;
            
            document.getElementById('badge-urgent').style.display = service === 'Express' ? 'block' : 'none';
            document.getElementById('badge-fragile').innerText = "Fragile: " + (isFragile ? 'OUI 🍷' : 'NON');
            document.getElementById('badge-fragile').style.color = isFragile ? '#ea580c' : '#94a3b8';

            // QR Code
            const qrcodeContainer = document.getElementById("qrcode");
            qrcodeContainer.innerHTML = "";
            new QRCode(qrcodeContainer, {
                text: uniqueId,
                width: 65,
                height: 65,
                colorDark : "#0f172a",
                correctLevel : QRCode.CorrectLevel.M
            });

            document.getElementById('downloadBtn').classList.remove('hidden');
            
            // WhatsApp
            const message = `*BORDEREAU CT241 - LOGISTIQUE*\n` +
                          `🆔 ID: *${uniqueId}*\n\n` +
                          `*EXPÉDITION*\n` +
                          `🏪 Boutique: ${vendeur.toUpperCase()}\n` +
                          `📞 Contact Exp: ${vendeurTel}\n` +
                          `📦 Colis: ${produit} (x${nbColis})\n\n` +
                          `*DESTINATAIRE*\n` +
                          `👤 Client: ${client}\n` +
                          `📞 Tél: ${tel}\n` +
                          `📍 Lieu: ${destination}\n` +
                          `🏠 Rép: ${precision}\n\n` +
                          `*FINANCE*\n` +
                          `💰 À ENCAISSER: *${total.toLocaleString()} FCFA*\n` +
                          `💳 Mode: ${modeText}\n` +
                          `🚚 Service: ${service}\n` +
                          `⚠️ Fragile: ${isFragile ? 'OUI' : 'NON'}\n\n` +
                          `_Généré le ${dateStr}_`;
            
            window.open(`https://wa.me/24177736065?text=${encodeURIComponent(message)}`, '_blank');
        }

        function downloadImage() {
            const captureArea = document.getElementById('captureArea');
            html2canvas(captureArea, { 
                scale: 3,
                useCORS: true,
                allowTaint: false,
                backgroundColor: "#f8fafc"
            }).then(canvas => {
                const link = document.createElement('a');
                const id = document.getElementById('display-id').innerText.replace('#', '');
                link.download = `bordereau-${id}.png`;
                link.href = canvas.toDataURL("image/png");
                link.click();
            });
        }
    </script>
</body>
</html>
