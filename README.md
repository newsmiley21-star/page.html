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
        import { getFirestore, collection, addDoc, query, onSnapshot, serverTimestamp, doc, deleteDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
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
        window.currentTicketData = null;

        const initSession = async () => {
            try {
                const userCredential = await signInAnonymously(auth);
                currentUser = userCredential.user;
            } catch (error) {
                console.error("Erreur Auth:", error);
            }
        };

        window.deleteBordereau = async (id) => {
            const pin = prompt("Entrez le code PIN pour supprimer :");
            if (pin !== "2410") return;
            try {
                await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'bordereaux', id));
            } catch (e) { console.error(e); }
        };

        window.loadAdminHistory = () => {
            if (unsubscribeHistory) unsubscribeHistory();
            const q = query(collection(db, 'artifacts', appId, 'public', 'data', 'bordereaux'));
            unsubscribeHistory = onSnapshot(q, (snapshot) => {
                const historyList = document.getElementById('historyList');
                historyList.innerHTML = '';
                const docs = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }))
                    .sort((a, b) => (b.createdAt?.seconds || 0) - (a.createdAt?.seconds || 0));

                if (docs.length === 0) {
                    historyList.innerHTML = '<p class="text-[10px] text-gray-400 text-center py-4 col-span-2">Aucun historique.</p>';
                    return;
                }

                docs.forEach(data => {
                    const item = document.createElement('div');
                    item.className = "group relative p-3 bg-white border rounded-lg shadow-sm hover:border-[#007d40] cursor-pointer";
                    item.innerHTML = `
                        <div onclick="reprintBordereau('${btoa(JSON.stringify(data))}')">
                            <p class="text-[10px] font-black text-[#007d40]">${data.ticketId}</p>
                            <p class="text-xs font-bold text-gray-800">${data.vendeur}</p>
                            <p class="text-[9px] text-gray-400">${data.client} • ${data.total} FCFA</p>
                        </div>
                        <button onclick="event.stopPropagation(); deleteBordereau('${data.id}')" class="absolute top-2 right-2 opacity-0 group-hover:opacity-100 p-1 text-red-500 bg-red-50 rounded">
                            <svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 6h18M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"></path></svg>
                        </button>
                    `;
                    historyList.appendChild(item);
                });
            }, (error) => console.error("Erreur Sync:", error));
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
                console.error("Erreur Sauvegarde Cloud:", e);
                throw e; 
            }
        };

        initSession();
    </script>

    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;900&display=swap');
        body { font-family: 'Inter', sans-serif; background-color: #f8fafc; color: #1e293b; }
        .receipt-card { width: 450px; background: white; border-radius: 16px; overflow: hidden; box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1); }
        .modal-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.6); backdrop-filter: blur(4px); z-index: 50; align-items: center; justify-content: center; }
        .modal-active { display: flex; }
        input, select { transition: all 0.2s; border: 1px solid #e2e8f0; }
        input:focus, select:focus { border-color: #007d40; ring: 2px; ring-color: #007d40; outline: none; background: white; }
        @media (max-width: 480px) { .receipt-card { width: 100%; } }
    </style>
</head>
<body class="p-4 md:p-8">

    <!-- MODAL AUTH ADMIN -->
    <div id="adminModal" class="modal-overlay">
        <div class="bg-white p-6 rounded-2xl shadow-2xl w-full max-w-sm mx-4">
            <h3 class="text-lg font-black text-gray-800 mb-2 text-center">Accès Admin</h3>
            <input type="password" id="adminPin" maxlength="4" placeholder="••••" class="w-full text-center text-2xl tracking-[1em] p-4 rounded-xl border-2 mb-4 focus:border-[#007d40] outline-none">
            <div class="flex space-x-3">
                <button onclick="toggleAdminModal(false)" class="flex-1 py-3 text-xs font-bold text-gray-400">ANNULER</button>
                <button onclick="verifyAdminCode()" class="flex-1 py-3 bg-[#007d40] text-white rounded-xl font-black text-xs">VÉRIFIER</button>
            </div>
            <p id="authError" class="hidden text-red-500 text-[10px] mt-4 font-bold text-center italic">Code incorrect.</p>
        </div>
    </div>

    <div class="max-w-7xl mx-auto grid grid-cols-1 lg:grid-cols-3 gap-8">
        
        <!-- FORMULAIRE -->
        <div class="lg:col-span-2 space-y-6">
            <div class="bg-white p-6 rounded-2xl shadow-xl border border-gray-100">
                <div class="flex items-center space-x-3 mb-8">
                    <img src="https://i.ibb.co/xKY76DgR/Gemini-Generated-Image-1pvtp31pvtp31pvt-1.png" class="h-12 rounded-lg" alt="Logo">
                    <div>
                        <h2 class="text-xl font-black text-gray-800 uppercase tracking-tighter">CT241 Logistique</h2>
                        <p class="text-[10px] font-bold text-[#007d40] tracking-widest">PORTAIL D'EXPÉDITION</p>
                    </div>
                </div>
                
                <form id="bordereauForm" class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <!-- Expéditeur -->
                    <div class="space-y-4 border-l-4 border-[#007d40] pl-4">
                        <h3 class="text-xs font-black text-[#007d40] uppercase">1. Expéditeur</h3>
                        <input type="text" id="vendeur" placeholder="Nom de la Boutique" class="w-full p-3 rounded-lg bg-gray-50">
                        <input type="tel" id="vendeurTel" placeholder="Numéro WhatsApp Expéditeur" class="w-full p-3 rounded-lg bg-gray-50">
                        <div class="grid grid-cols-2 gap-2">
                            <input type="text" id="produit" placeholder="Article(s)" class="w-full p-3 rounded-lg bg-gray-50">
                            <input type="number" id="nbColis" value="1" min="1" class="w-full p-3 rounded-lg bg-gray-50">
                        </div>
                    </div>

                    <!-- Destinataire -->
                    <div class="space-y-4 border-l-4 border-gray-300 pl-4">
                        <h3 class="text-xs font-black text-gray-500 uppercase">2. Destinataire</h3>
                        <input type="text" id="client" placeholder="Nom complet du Client" class="w-full p-3 rounded-lg bg-gray-50">
                        <input type="tel" id="telephone" placeholder="Numéro de Téléphone" class="w-full p-3 rounded-lg bg-gray-50">
                        <div class="grid grid-cols-2 gap-2">
                            <select id="quartier" class="w-full p-3 rounded-lg bg-gray-50 text-xs font-semibold">
                                <optgroup label="Libreville">
                                    <option value="Centre Ville">Centre Ville</option>
                                    <option value="Louis">Louis</option>
                                    <option value="Glass">Glass</option>
                                    <option value="AIAI">AIAI</option>
                                    <option value="Nzeng-Ayong">Nzeng-Ayong</option>
                                    <option value="PK5-PK12">PK5-PK12</option>
                                    <option value="Akebé">Akebé</option>
                                </optgroup>
                                <optgroup label="Akanda">
                                    <option value="Angondjé">Angondjé</option>
                                    <option value="Avorbam">Avorbam</option>
                                    <option value="Marseille">Marseille</option>
                                </optgroup>
                                <optgroup label="Owendo">
                                    <option value="Port Owendo">Port Owendo</option>
                                    <option value="Acae">Acae</option>
                                </optgroup>
                            </select>
                            <input type="text" id="precision" placeholder="Point de repère" class="w-full p-3 rounded-lg bg-gray-50">
                        </div>
                    </div>

                    <!-- Paiement -->
                    <div class="space-y-4 border-l-4 border-[#eab308] pl-4 md:col-span-2">
                        <h3 class="text-xs font-black text-[#eab308] uppercase">3. Détails financiers</h3>
                        <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
                            <div>
                                <label class="text-[9px] font-bold text-gray-400 ml-1">Prix Article (FCFA)</label>
                                <input type="number" id="prix" value="0" class="w-full p-3 rounded-lg bg-gray-50 font-bold">
                            </div>
                            <div>
                                <label class="text-[9px] font-bold text-gray-400 ml-1">Livraison (Fixe)</label>
                                <input type="number" id="livraison" value="2000" readonly class="w-full p-3 rounded-lg bg-gray-200 font-black text-[#007d40] cursor-not-allowed">
                            </div>
                            <div>
                                <label class="text-[9px] font-bold text-gray-400 ml-1">Mode de règlement</label>
                                <select id="modePay" class="w-full p-3 rounded-lg bg-gray-50 text-xs font-bold">
                                    <option value="CASH">CASH À LIVRAISON</option>
                                    <option value="AIRTEL">AIRTEL MONEY</option>
                                    <option value="MOOV">MOOV MONEY</option>
                                    <option value="PAID">DÉJÀ PAYÉ (0 FCFA)</option>
                                </select>
                            </div>
                            <div class="flex items-center justify-center bg-gray-50 p-3 rounded-lg border mt-5">
                                <input type="checkbox" id="urgent" class="w-5 h-5 accent-red-600">
                                <label for="urgent" class="ml-2 text-[10px] font-black text-red-600 uppercase italic">Urgent ⚡</label>
                            </div>
                        </div>
                    </div>

                    <button type="button" onclick="preparePreview()" class="md:col-span-2 w-full bg-[#007d40] text-white font-black py-5 rounded-2xl shadow-lg hover:shadow-[#007d4044] active:scale-[0.98] transition-all text-sm tracking-widest">
                        GÉNÉRER L'APERÇU DU BORDEREAU
                    </button>
                </form>
            </div>

            <!-- Historique -->
            <div id="adminSection" class="bg-white p-6 rounded-2xl border border-gray-200 shadow-sm">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-[10px] font-black text-gray-400 uppercase tracking-[0.2em] flex items-center">
                        <span id="adminDot" class="w-2 h-2 bg-gray-300 rounded-full mr-2"></span>
                        Base de données des envois
                    </h3>
                    <button id="lockBtn" onclick="location.reload()" class="hidden text-[9px] font-black text-red-500 hover:underline uppercase">Se déconnecter</button>
                </div>
                <div id="lockedState" class="text-center py-8 bg-gray-50 rounded-xl border border-dashed">
                    <button onclick="toggleAdminModal(true)" class="bg-gray-800 text-white px-8 py-3 rounded-xl font-black text-[11px] uppercase tracking-wider shadow-md active:scale-95 transition">Accéder à l'historique</button>
                </div>
                <div id="historyList" class="hidden grid grid-cols-1 md:grid-cols-2 gap-4"></div>
            </div>
        </div>

        <!-- ZONE APERÇU -->
        <div class="flex flex-col items-center">
            <div id="captureArea" class="receipt-card border border-gray-100">
                <!-- Header Ticket -->
                <div class="bg-slate-900 p-6 text-white flex justify-between items-start relative">
                    <div class="z-10">
                        <div class="bg-white p-2 rounded-lg inline-block mb-3">
                            <img src="https://i.ibb.co/xKY76DgR/Gemini-Generated-Image-1pvtp31pvtp31pvt-1.png" class="h-10" alt="Logo">
                        </div>
                        <h1 class="text-lg font-black tracking-tighter leading-none">CT241 <span class="text-[#007d40]">LOG</span></h1>
                        <p class="text-[9px] font-bold text-blue-400 mt-1 uppercase" id="display-id">#TICKET-EN-ATTENTE</p>
                    </div>
                    <div id="badge-urgent-ui" class="hidden px-3 py-1.5 bg-red-600 text-white rounded-full text-[9px] font-black uppercase italic animate-pulse">Livraison Express ⚡</div>
                    <div class="absolute bottom-0 left-0 w-full h-1.5 bg-[#007d40]"></div>
                </div>
                
                <div class="p-8 space-y-6">
                    <!-- Section Expéditeur -->
                    <div class="flex justify-between border-b border-gray-100 pb-5">
                        <div>
                            <p class="text-[8px] text-gray-400 font-black uppercase tracking-widest mb-1">Point de départ</p>
                            <p class="font-black text-gray-800 text-xl leading-none" id="display-vendeur">---</p>
                            <p class="text-[11px] text-gray-500 font-bold mt-2" id="display-vendeurTel">---</p>
                        </div>
                        <div class="text-right">
                            <p class="text-[8px] text-gray-400 font-black uppercase mb-1 tracking-widest">Date émission</p>
                            <p class="text-xs font-black text-gray-800" id="display-date">--/--/--</p>
                        </div>
                    </div>

                    <!-- Section Destinataire -->
                    <div class="grid grid-cols-2 gap-6 py-2">
                        <div>
                            <p class="text-[8px] text-gray-400 font-black uppercase tracking-widest mb-1">Livrer à</p>
                            <p class="text-sm font-black text-gray-800" id="display-client">---</p>
                            <p class="text-xs font-bold text-gray-600 mt-1" id="display-tel">---</p>
                        </div>
                        <div class="text-right">
                            <p class="text-[8px] text-gray-400 font-black uppercase tracking-widest mb-1">Destination</p>
                            <p class="text-sm font-black text-gray-800" id="display-destination">---</p>
                            <p class="text-[10px] text-gray-500 font-medium italic mt-1 leading-tight" id="display-precision">---</p>
                        </div>
                    </div>

                    <!-- Section Financière -->
                    <div class="bg-slate-50 p-5 rounded-2xl border border-gray-100 flex justify-between items-center shadow-inner">
                        <div>
                            <p class="text-[9px] font-black text-gray-400 uppercase mb-1">Montant à percevoir</p>
                            <p class="text-2xl font-black text-[#007d40]" id="display-total">0 FCFA</p>
                        </div>
                        <div class="text-right">
                            <div id="display-mode" class="text-[10px] font-black bg-gray-800 text-white px-3 py-1.5 rounded-lg inline-block">CASH</div>
                            <p class="text-[8px] font-bold text-gray-400 mt-2 italic uppercase">Hors frais de dépôt</p>
                        </div>
                    </div>

                    <!-- Footer Ticket -->
                    <div class="flex items-center justify-between pt-4 border-t border-dashed">
                        <div class="w-3/4 pr-4">
                            <p class="text-[9px] font-black text-gray-800 uppercase flex items-center">
                                <svg class="w-3 h-3 mr-1 text-[#007d40]" fill="currentColor" viewBox="0 0 20 20"><path d="M7 3a1 1 0 000 2h6a1 1 0 100-2H7zM4 7a1 1 0 011-1h10a1 1 0 110 2H5a1 1 0 01-1-1zM2 11a2 2 0 012-2h12a2 2 0 012 2v4a2 2 0 01-2 2H4a2 2 0 01-2-2v-4z"></path></svg>
                                Description Colis
                            </p>
                            <p class="text-[10px] text-gray-500 font-medium mt-1 leading-relaxed" id="display-colis-full">---</p>
                        </div>
                        <div id="qrcode" class="p-1.5 bg-white border border-gray-100 rounded-lg"></div>
                    </div>
                </div>
                <div class="bg-slate-900 p-3 text-center">
                    <p class="text-[8px] font-black text-white tracking-[0.4em] uppercase opacity-50">Sécurité • Rapidité • CT241</p>
                </div>
            </div>

            <!-- Boutons d'Action Finales -->
            <div id="actionButtons" class="hidden w-full space-y-3 mt-6">
                <button onclick="executeFinalActions()" id="finalBtn" class="w-full bg-[#eab308] text-black py-5 rounded-2xl font-black uppercase text-xs shadow-xl flex flex-col items-center justify-center space-y-1 hover:brightness-105 active:scale-95 transition-all">
                    <span class="flex items-center text-sm">
                        <svg class="w-5 h-5 mr-2" fill="currentColor" viewBox="0 0 24 24"><path d="M12.01 2.01c-5.52 0-10 4.48-10 10s4.48 10 10 10 10-4.48 10-10-4.48-10-10-10zM18 13h-5v5h-2v-5H6v-2h5V6h2v5h5v2z"/></svg>
                        CONFIRMER & EXPÉDIER
                    </span>
                    <span class="text-[9px] opacity-70 font-bold tracking-widest">(SAVE + IMAGE + WHATSAPP)</span>
                </button>
                <p class="text-[10px] text-center text-gray-400 font-bold italic">En cliquant, le ticket sera enregistré et envoyé au +241 77 73 60 65</p>
            </div>
            
            <div id="successMessage" class="hidden mt-6 w-full text-center p-4 bg-green-50 text-green-700 border border-green-200 rounded-2xl font-black text-xs uppercase tracking-tighter animate-bounce">
                ✅ Opération traitée avec succès !
            </div>
        </div>
    </div>

    <script>
        const ADMIN_PIN = "2410";
        const LOGISTICS_WA = "24177736065"; // Le numéro WhatsApp de destination

        function toggleAdminModal(show) {
            const modal = document.getElementById('adminModal');
            show ? modal.classList.add('modal-active') : modal.classList.remove('modal-active');
            document.getElementById('adminPin').value = '';
            document.getElementById('authError').classList.add('hidden');
        }

        function verifyAdminCode() {
            if(document.getElementById('adminPin').value === ADMIN_PIN) {
                document.getElementById('lockedState').classList.add('hidden');
                document.getElementById('historyList').classList.remove('hidden');
                document.getElementById('lockBtn').classList.remove('hidden');
                document.getElementById('adminDot').classList.replace('bg-gray-300', 'bg-green-500');
                if (window.loadAdminHistory) window.loadAdminHistory();
                toggleAdminModal(false);
            } else {
                document.getElementById('authError').classList.remove('hidden');
            }
        }

        function preparePreview() {
            const vendeur = document.getElementById('vendeur').value.trim();
            const client = document.getElementById('client').value.trim();
            
            if(!vendeur || !client) {
                alert("Veuillez remplir au moins les noms de la boutique et du client.");
                return;
            }

            const data = {
                vendeur: vendeur || 'Boutique Inconnue',
                vendeurTel: document.getElementById('vendeurTel').value || 'N/A',
                produit: document.getElementById('produit').value || 'Colis',
                nbColis: document.getElementById('nbColis').value || '1',
                client: client || '---',
                telephone: document.getElementById('telephone').value || 'N/A',
                quartier: document.getElementById('quartier').value,
                precision: document.getElementById('precision').value || 'Aucune précision',
                prix: parseInt(document.getElementById('prix').value) || 0,
                livraison: 2000,
                mode: document.getElementById('modePay').value,
                service: document.getElementById('urgent').checked ? 'Express' : 'Standard',
                ticketId: `CT-${Math.floor(1000 + Math.random() * 9000)}`
            };

            // Gestion du mode "DÉJÀ PAYÉ"
            if(data.mode === "PAID") {
                data.total = 0;
            } else {
                data.total = data.prix + data.livraison;
            }

            window.currentTicketData = data;

            // Mise à jour de l'interface
            document.getElementById('display-vendeur').innerText = data.vendeur.toUpperCase();
            document.getElementById('display-vendeurTel').innerText = "📞 " + data.vendeurTel;
            document.getElementById('display-client').innerText = data.client;
            document.getElementById('display-tel').innerText = data.telephone;
            document.getElementById('display-destination').innerText = data.quartier;
            document.getElementById('display-precision').innerText = "📍 " + data.precision;
            document.getElementById('display-total').innerText = data.total.toLocaleString() + " FCFA";
            document.getElementById('display-id').innerText = "#" + data.ticketId;
            document.getElementById('display-colis-full').innerText = `${data.produit} (${data.nbColis} pièce(s)). Vérification et encaissement requis.`;
            document.getElementById('display-mode').innerText = data.mode === "PAID" ? "DÉJÀ PAYÉ" : data.mode;
            document.getElementById('badge-urgent-ui').style.display = data.service === 'Express' ? 'block' : 'none';
            document.getElementById('display-date').innerText = new Date().toLocaleDateString('fr-FR');

            const qrBox = document.getElementById("qrcode");
            qrBox.innerHTML = "";
            new QRCode(qrBox, { text: data.ticketId, width: 64, height: 64, colorDark : "#0f172a" });

            document.getElementById('actionButtons').classList.remove('hidden');
            document.getElementById('successMessage').classList.add('hidden');
            
            // Scroll fluide vers l'aperçu sur mobile
            if(window.innerWidth < 1024) {
                document.getElementById('captureArea').scrollIntoView({ behavior: 'smooth' });
            }
        }

        async function executeFinalActions() {
            const data = window.currentTicketData;
            if(!data) return;

            const btn = document.getElementById('finalBtn');
            const originalContent = btn.innerHTML;
            
            btn.disabled = true;
            btn.innerHTML = `
                <svg class="animate-spin h-5 w-5 text-black mb-1" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                    <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                    <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                </svg>
                <span class="text-[10px] font-black uppercase">Finalisation en cours...</span>
            `;

            try {
                // 1. Sauvegarde Cloud Firestore
                await window.saveBordereauToCloud(data);

                // 2. Capture et Téléchargement de l'Image
                const canvas = await html2canvas(document.getElementById('captureArea'), { 
                    scale: 3,
                    useCORS: true,
                    backgroundColor: "#ffffff"
                });
                const link = document.createElement('a');
                link.download = `Bordereau_${data.ticketId}_${data.client.replace(/\s+/g, '_')}.png`;
                link.href = canvas.toDataURL("image/png");
                link.click();

                // 3. Construction du message WhatsApp pour le numéro logistique
                const msg = `*NOUVELLE DEMANDE DE LIVRAISON*\n` +
                            `-------------------------------\n` +
                            `🆔 ID Ticket: *${data.ticketId}*\n` +
                            `🚚 Service: *${data.service.toUpperCase()}*\n\n` +
                            `🏪 Boutique: *${data.vendeur}*\n` +
                            `📞 Contact Exp: ${data.vendeurTel}\n\n` +
                            `👤 Client: *${data.client}*\n` +
                            `📞 Contact Dest: ${data.telephone}\n` +
                            `📍 Quartier: *${data.quartier}*\n` +
                            `🚩 Repère: ${data.precision}\n\n` +
                            `📦 Contenu: ${data.produit} (x${data.nbColis})\n` +
                            `💰 Total à percevoir: *${data.total} FCFA*\n` +
                            `💳 Règlement: ${data.mode}\n` +
                            `-------------------------------\n` +
                            `_Généré via Portail CT241 Logistique_`;

                const waUrl = `https://wa.me/${LOGISTICS_WA}?text=${encodeURIComponent(msg)}`;
                
                // On laisse un petit délai pour que le téléchargement se lance
                setTimeout(() => {
                    window.open(waUrl, '_blank');
                    document.getElementById('successMessage').classList.remove('hidden');
                    btn.innerHTML = originalContent;
                    btn.disabled = false;
                }, 800);

            } catch (err) {
                console.error(err);
                alert("Une erreur est survenue lors de l'enregistrement. Vérifiez votre connexion.");
                btn.innerHTML = originalContent;
                btn.disabled = false;
            }
        }

        window.reprintBordereau = (encoded) => {
            const data = JSON.parse(atob(encoded));
            
            // On remplit le formulaire avec les anciennes données
            document.getElementById('vendeur').value = data.vendeur;
            document.getElementById('vendeurTel').value = data.vendeurTel || '';
            document.getElementById('client').value = data.client;
            document.getElementById('telephone').value = data.telephone || '';
            document.getElementById('produit').value = data.produit;
            document.getElementById('nbColis').value = data.nbColis;
            document.getElementById('quartier').value = data.quartier;
            document.getElementById('precision').value = data.precision;
            document.getElementById('prix').value = data.prix;
            document.getElementById('modePay').value = data.mode;
            document.getElementById('urgent').checked = (data.service === 'Express');

            // On régénère l'aperçu
            preparePreview();
            window.scrollTo({ top: 0, behavior: 'smooth' });
        };
    </script>
</body>
</html>
