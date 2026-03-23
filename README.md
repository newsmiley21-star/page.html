<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CT241 - Portail Logistique</title>
    
    <!-- PWA & Meta -->
    <meta name="theme-color" content="#007d40">
    <link rel="apple-touch-icon" href="https://i.ibb.co/xKY76DgR/Gemini-Generated-Image-1pvtp31pvtp31pvt-1.png">
    
    <!-- Scripts Externes -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    
    <!-- Firebase SDKs -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getFirestore, collection, addDoc, query, onSnapshot, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        import { getAuth, signInAnonymously } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";

        const firebaseConfig = {
            apiKey: "AIzaSyBnFK2ATbYsRZ3RX8bsOYkNEMtk6tsCGsc",
            authDomain: "portail-de-commandes.firebaseapp.com",
            projectId: "portail-de-commandes",
            storageBucket: "portail-de-commandes.firebasestorage.app",
            messagingSenderId: "961030938629",
            appId: "1:961030938629:web:0f3d43445b8b358014bc6c",
            measurementId: "G-7ZKSRQS4T3"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const auth = getAuth(app);
        const appId = "ct241-logistics-v1";

        let currentUser = null;
        let unsubscribeHistory = null;

        const initSession = async () => {
            try {
                const userCredential = await signInAnonymously(auth);
                currentUser = userCredential.user;
                console.log("Session active:", currentUser.uid);
            } catch (error) {
                console.error("Erreur Auth:", error);
            }
        };

        // Fonction pour charger l'historique une fois authentifié admin
        window.loadAdminHistory = () => {
            if (unsubscribeHistory) unsubscribeHistory(); // Nettoyer l'ancien listener si présent

            const q = query(collection(db, 'artifacts', appId, 'public', 'data', 'bordereaux'));
            
            unsubscribeHistory = onSnapshot(q, (snapshot) => {
                const historyList = document.getElementById('historyList');
                historyList.innerHTML = '';
                
                const docs = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }))
                    .sort((a, b) => (b.createdAt?.seconds || 0) - (a.createdAt?.seconds || 0))
                    .slice(0, 15);

                if (docs.length === 0) {
                    historyList.innerHTML = '<p class="text-[10px] text-gray-400 italic col-span-2 text-center py-4">Aucun bordereau trouvé.</p>';
                    return;
                }

                docs.forEach(data => {
                    const item = document.createElement('div');
                    item.className = "flex justify-between items-center p-3 bg-white border border-gray-100 rounded-lg shadow-sm mb-2 hover:border-[#007d40] transition-colors cursor-pointer";
                    item.innerHTML = `
                        <div>
                            <p class="text-[10px] font-black text-[#007d40]">${data.ticketId}</p>
                            <p class="text-xs font-bold text-gray-800">${data.vendeur}</p>
                            <p class="text-[9px] text-gray-400">${data.client} • ${data.total} FCFA</p>
                        </div>
                        <div class="text-right">
                             <span class="text-[8px] px-2 py-1 ${data.service === 'Express' ? 'bg-red-50 text-red-600' : 'bg-gray-100 text-gray-600'} rounded font-bold uppercase">${data.service}</span>
                        </div>
                    `;
                    historyList.appendChild(item);
                });
            }, (error) => {
                console.error("Erreur Firestore:", error);
                document.getElementById('historyList').innerHTML = '<p class="text-red-500 text-[10px]">Erreur de permission Cloud.</p>';
            });
        };

        window.saveBordereauToCloud = async (bordereauData) => {
            if (!currentUser) return;
            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'bordereaux'), {
                    ...bordereauData,
                    userId: currentUser.uid,
                    createdAt: serverTimestamp()
                });
            } catch (e) {
                console.error("Erreur Sauvegarde:", e);
            }
        };

        initSession();
    </script>

    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;900&display=swap');
        body { font-family: 'Inter', sans-serif; background-color: #f8fafc; }
        .receipt-card { width: 450px; background: white; border-radius: 16px; overflow: hidden; box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1); }
        input, select { background-color: #f1f5f9 !important; border: 1px solid #e2e8f0 !important; }
        .logo-img { max-height: 52px; width: auto; object-fit: contain; }
        .ticket-logo-box { background: white; padding: 4px; border-radius: 8px; display: flex; align-items: center; justify-content: center; box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
        
        /* Modal Style */
        .modal-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.6); backdrop-filter: blur(4px); z-index: 50; align-items: center; justify-content: center; }
        .modal-active { display: flex; }

        @media (max-width: 480px) { .receipt-card { width: 100%; } }
    </style>
</head>
<body class="p-4 md:p-8">

    <!-- MODAL AUTH ADMIN -->
    <div id="adminModal" class="modal-overlay">
        <div class="bg-white p-6 rounded-2xl shadow-2xl w-full max-w-sm mx-4">
            <h3 class="text-lg font-black text-gray-800 mb-2">Accès Administrateur</h3>
            <p class="text-xs text-gray-500 mb-6">Veuillez saisir le code PIN pour accéder à l'activité globale.</p>
            <input type="password" id="adminPin" maxlength="4" placeholder="••••" class="w-full text-center text-2xl tracking-[1em] p-4 rounded-xl border-2 border-gray-100 outline-none mb-4 focus:border-[#007d40]">
            <div class="flex space-x-3">
                <button onclick="toggleAdminModal(false)" class="flex-1 py-3 text-xs font-bold text-gray-400 uppercase">Annuler</button>
                <button onclick="verifyAdminCode()" class="flex-1 py-3 bg-[#007d40] text-white rounded-xl font-black uppercase text-xs">Vérifier</button>
            </div>
            <p id="authError" class="hidden text-red-500 text-[10px] mt-4 font-bold text-center">Code incorrect. Accès refusé.</p>
        </div>
    </div>

    <div class="max-w-7xl mx-auto grid grid-cols-1 lg:grid-cols-3 gap-8">
        
        <!-- FORMULAIRE -->
        <div class="lg:col-span-2 space-y-6">
            <div class="bg-white p-6 md:p-8 rounded-2xl shadow-xl border border-gray-100">
                <div class="flex items-center space-x-3 mb-8">
                    <img src="https://i.ibb.co/xKY76DgR/Gemini-Generated-Image-1pvtp31pvtp31pvt-1.png" class="logo-img" alt="CT241 Logo">
                    <div>
                        <h2 class="text-xl font-black text-gray-800 tracking-tighter uppercase leading-none">CT241 Logistique</h2>
                        <p class="text-[10px] font-bold text-[#007d40]">INTERFACE LIVRAISON</p>
                    </div>
                </div>
                
                <form id="bordereauForm" class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div class="space-y-4 border-l-4 border-[#007d40] pl-4">
                        <h3 class="text-xs font-black text-[#007d40] uppercase tracking-widest">Expéditeur</h3>
                        <input type="text" id="vendeur" placeholder="Boutique / Vendeur" class="w-full p-2.5 rounded-lg outline-none">
                        <input type="tel" id="vendeurTel" placeholder="Contact Expéditeur" class="w-full p-2.5 rounded-lg outline-none">
                        <div class="grid grid-cols-2 gap-2">
                            <input type="text" id="produit" placeholder="Contenu" class="w-full p-2.5 rounded-lg outline-none">
                            <input type="number" id="nbColis" value="1" class="w-full p-2.5 rounded-lg outline-none">
                        </div>
                    </div>

                    <div class="space-y-4 border-l-4 border-gray-300 pl-4">
                        <h3 class="text-xs font-black text-gray-500 uppercase tracking-widest">Destinataire</h3>
                        <input type="text" id="client" placeholder="Nom du Client" class="w-full p-2.5 rounded-lg outline-none">
                        <input type="tel" id="telephone" placeholder="Téléphone Client" class="w-full p-2.5 rounded-lg outline-none">
                        <div class="grid grid-cols-2 gap-2">
                            <select id="quartier" class="w-full p-2.5 rounded-lg outline-none text-xs">
                                <option value="Libreville">Libreville</option>
                                <option value="Akanda">Akanda</option>
                                <option value="Owendo">Owendo</option>
                                <option value="PK5-PK12">PK5-PK12</option>
                            </select>
                            <input type="text" id="precision" placeholder="Repère" class="w-full p-2.5 rounded-lg outline-none">
                        </div>
                    </div>

                    <div class="space-y-4 border-l-4 border-[#eab308] pl-4 md:col-span-2">
                        <h3 class="text-xs font-black text-[#eab308] uppercase tracking-widest">Paiement & Service</h3>
                        <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
                            <input type="number" id="prix" placeholder="Prix Colis" class="w-full p-2.5 rounded-lg outline-none">
                            <input type="number" id="livraison" value="2000" class="w-full p-2.5 rounded-lg outline-none">
                            <select id="modePay" class="w-full p-2.5 rounded-lg outline-none">
                                <option value="COD">CASH</option>
                                <option value="PREPAID">PAYÉ</option>
                                <option value="AIRTEL">AIRTEL</option>
                            </select>
                            <div class="flex items-center space-x-2 bg-gray-50 p-2 rounded-lg border">
                                <input type="checkbox" id="urgent" class="w-4 h-4">
                                <label for="urgent" class="text-[10px] font-black text-red-600 uppercase">Urgent⚡</label>
                            </div>
                        </div>
                    </div>

                    <button type="button" onclick="handleGenerate()" class="md:col-span-2 w-full bg-[#007d40] text-white font-black py-4 rounded-xl shadow-lg active:scale-[0.98] transition">
                        GÉNÉRER LE BORDEREAU
                    </button>
                </form>
            </div>

            <!-- Historique Cloud Protégé -->
            <div id="adminSection" class="bg-slate-50 p-6 rounded-2xl border border-dashed border-gray-300 relative overflow-hidden">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-xs font-black text-gray-400 uppercase tracking-widest flex items-center">
                        <span id="adminDot" class="w-2 h-2 bg-gray-300 rounded-full mr-2"></span>
                        Journal d'activité global
                    </h3>
                    <button id="lockBtn" onclick="lockAdmin()" class="hidden text-[9px] font-black text-red-500 uppercase flex items-center">
                        🔒 Verrouiller
                    </button>
                </div>

                <!-- État Verrouillé -->
                <div id="lockedState" class="text-center py-8">
                    <div class="inline-flex items-center justify-center w-12 h-12 bg-gray-200 rounded-full mb-4">
                        <span class="text-xl">🔒</span>
                    </div>
                    <p class="text-sm font-bold text-gray-800 mb-4">Accès restreint à l'administration</p>
                    <button onclick="toggleAdminModal(true)" class="bg-slate-800 text-white px-6 py-2 rounded-lg font-black text-[10px] uppercase shadow-md">
                        Déverrouiller le journal
                    </button>
                </div>

                <!-- État Déverrouillé -->
                <div id="historyList" class="hidden grid grid-cols-1 md:grid-cols-2 gap-3">
                    <!-- Rempli dynamiquement -->
                </div>
            </div>
        </div>

        <!-- APERÇU TICKET -->
        <div class="flex flex-col items-center">
            <div id="captureArea" class="receipt-card border border-gray-200">
                <div class="bg-slate-900 p-5 text-white flex justify-between items-center relative">
                    <div class="z-10 flex flex-col space-y-2">
                        <div class="ticket-logo-box">
                            <img src="https://i.ibb.co/xKY76DgR/Gemini-Generated-Image-1pvtp31pvtp31pvt-1.png" class="h-10 w-auto" alt="Logo">
                        </div>
                        <p class="text-[10px] font-bold text-blue-400" id="display-id">#ID-PENDING</p>
                    </div>
                    <div id="badge-urgent-ui" class="hidden px-2 py-1 bg-red-100 text-red-600 rounded text-[10px] font-black uppercase z-10 italic">Express ⚡</div>
                    <div class="absolute bottom-0 left-0 w-full h-1 bg-[#007d40]"></div>
                </div>
                
                <div class="p-6 space-y-4">
                    <div class="flex justify-between items-start border-b border-dashed pb-4">
                        <div>
                            <p class="text-[9px] text-gray-400 font-bold uppercase tracking-tighter">Expéditeur</p>
                            <p class="font-black text-gray-800 text-lg leading-tight" id="display-vendeur">---</p>
                            <p class="text-[10px] text-gray-500 font-bold" id="display-vendeurTel">---</p>
                        </div>
                        <div class="text-right text-[9px] text-gray-400 font-bold" id="display-date">--/--/--</div>
                    </div>

                    <div class="grid grid-cols-2 gap-4">
                        <div class="space-y-1">
                            <p class="text-[9px] text-gray-400 font-bold uppercase italic">Destinataire</p>
                            <p class="text-sm font-black text-gray-800" id="display-client">---</p>
                            <p class="text-xs font-bold text-gray-600" id="display-tel">---</p>
                        </div>
                        <div class="space-y-1 text-right">
                            <p class="text-[9px] text-gray-400 font-bold uppercase italic">Destination</p>
                            <p class="text-sm font-black text-gray-800" id="display-destination">---</p>
                            <p class="text-[10px] text-gray-500 italic" id="display-precision">---</p>
                        </div>
                    </div>

                    <div class="bg-green-50 p-4 rounded-xl border border-green-100 flex justify-between items-center">
                        <div>
                            <p class="text-[9px] font-bold text-green-700 uppercase">Somme à percevoir</p>
                            <p class="text-xl font-black text-[#007d40]" id="display-total">0 FCFA</p>
                        </div>
                        <div id="display-mode" class="text-[10px] font-black bg-[#007d40] text-white px-2 py-1 rounded">COD</div>
                    </div>

                    <div class="flex items-center justify-between pt-2">
                        <div class="w-2/3">
                            <p class="text-[9px] font-black text-gray-800 uppercase underline">Instruction</p>
                            <p class="text-[9px] text-gray-500 leading-tight italic mt-1" id="display-colis-full">Colis: ---</p>
                        </div>
                        <div id="qrcode" class="p-1 border rounded bg-white"></div>
                    </div>
                </div>
                <div class="bg-slate-100 p-2 text-center text-[8px] font-bold text-gray-400 tracking-widest uppercase">CT241 • LOGISTIQUE CONNECTÉE</div>
            </div>

            <button onclick="downloadImage()" id="downloadBtn" class="hidden w-full mt-4 bg-[#eab308] text-black py-3 rounded-xl font-black uppercase text-sm shadow-lg active:scale-95">
                📥 Enregistrer le ticket
            </button>
        </div>
    </div>

    <script>
        const ADMIN_PIN = "2410"; // Code PIN Admin par défaut

        function toggleAdminModal(show) {
            const modal = document.getElementById('adminModal');
            if(show) {
                modal.classList.add('modal-active');
                document.getElementById('adminPin').value = '';
                document.getElementById('authError').classList.add('hidden');
                setTimeout(() => document.getElementById('adminPin').focus(), 100);
            } else {
                modal.classList.remove('modal-active');
            }
        }

        function verifyAdminCode() {
            const pin = document.getElementById('adminPin').value;
            if(pin === ADMIN_PIN) {
                unlockAdmin();
                toggleAdminModal(false);
            } else {
                document.getElementById('authError').classList.remove('hidden');
            }
        }

        function unlockAdmin() {
            document.getElementById('lockedState').classList.add('hidden');
            document.getElementById('historyList').classList.remove('hidden');
            document.getElementById('lockBtn').classList.remove('hidden');
            document.getElementById('adminDot').classList.replace('bg-gray-300', 'bg-green-500');
            document.getElementById('adminDot').classList.add('animate-pulse');
            
            // Appelle la fonction Firebase exportée dans le module
            if (window.loadAdminHistory) window.loadAdminHistory();
        }

        function lockAdmin() {
            document.getElementById('lockedState').classList.remove('hidden');
            document.getElementById('historyList').classList.add('hidden');
            document.getElementById('lockBtn').classList.add('hidden');
            document.getElementById('adminDot').classList.replace('bg-green-500', 'bg-gray-300');
            document.getElementById('adminDot').classList.remove('animate-pulse');
            
            // On pourrait arrêter le listener Firebase ici si nécessaire
            location.reload(); // Solution simple pour réinitialiser l'état
        }

        function handleGenerate() {
            const data = {
                vendeur: document.getElementById('vendeur').value || 'INCONNU',
                vendeurTel: document.getElementById('vendeurTel').value || '---',
                produit: document.getElementById('produit').value || 'Marchandise',
                nbColis: document.getElementById('nbColis').value || '1',
                client: document.getElementById('client').value || '---',
                telephone: document.getElementById('telephone').value || '---',
                quartier: document.getElementById('quartier').value,
                precision: document.getElementById('precision').value || '---',
                prix: parseInt(document.getElementById('prix').value) || 0,
                livraison: parseInt(document.getElementById('livraison').value) || 2000,
                mode: document.getElementById('modePay').value,
                service: document.getElementById('urgent').checked ? 'Express' : 'Standard',
                ticketId: `CT-${Math.floor(1000 + Math.random() * 9000)}`
            };
            data.total = data.prix + data.livraison;

            // Update UI
            document.getElementById('display-vendeur').innerText = data.vendeur.toUpperCase();
            document.getElementById('display-vendeurTel').innerText = data.vendeurTel;
            document.getElementById('display-client').innerText = data.client;
            document.getElementById('display-tel').innerText = data.telephone;
            document.getElementById('display-destination').innerText = data.quartier;
            document.getElementById('display-precision').innerText = "📍 " + data.precision;
            document.getElementById('display-total').innerText = data.total.toLocaleString() + " FCFA";
            document.getElementById('display-id').innerText = "#" + data.ticketId;
            document.getElementById('display-colis-full').innerText = `Colis: ${data.produit} (x${data.nbColis}). Encaissement avant remise.`;
            document.getElementById('display-mode').innerText = data.mode;
            document.getElementById('badge-urgent-ui').style.display = data.service === 'Express' ? 'block' : 'none';
            document.getElementById('display-date').innerText = new Date().toLocaleDateString('fr-FR');

            const qrBox = document.getElementById("qrcode");
            qrBox.innerHTML = "";
            new QRCode(qrBox, { text: data.ticketId, width: 60, height: 60 });

            document.getElementById('downloadBtn').classList.remove('hidden');
            
            if (window.saveBordereauToCloud) window.saveBordereauToCloud(data);

            const msg = `*BORDEREAU CT241*\nID: *${data.ticketId}*\n\nExp: ${data.vendeur}\nClient: ${data.client}\nLieu: ${data.quartier}\nTotal: *${data.total} FCFA*\nMode: ${data.mode}`;
            window.open(`https://wa.me/24177736065?text=${encodeURIComponent(msg)}`, '_blank');
        }

        function downloadImage() {
            html2canvas(document.getElementById('captureArea'), { scale: 3 }).then(canvas => {
                const link = document.createElement('a');
                link.download = `ct241-${Date.now()}.png`;
                link.href = canvas.toDataURL();
                link.click();
            });
        }
    </script>
</body>
</html>
