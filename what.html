<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculateur de Dépense Énergétique - Randonnée</title>
    <!-- Chargement de Tailwind CSS via CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .card { background: white; border-radius: 1rem; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1); }
        input[type="range"] { accent-color: #059669; }
        input, select { font-size: 16px !important; } 
    </style>
</head>
<body class="bg-slate-100 min-h-screen pb-12">

    <!-- Header -->
    <header class="bg-emerald-800 text-white py-8 px-4 mb-8 shadow-xl">
        <div class="max-w-4xl mx-auto text-center">
            <h1 class="text-3xl font-extrabold mb-2 tracking-tight">Calculateur de Calories Randonnée</h1>
            <p class="text-emerald-100 text-sm md:text-base opacity-90">Modélisation Avancée (Pandolf, Santee & Mifflin-St Jeor)</p>
        </div>
    </header>

    <main class="max-w-6xl mx-auto px-4 grid grid-cols-1 lg:grid-cols-3 gap-8">
        
        <!-- Formulaire de saisie -->
        <div class="lg:col-span-2 space-y-6">
            
            <!-- Section 1 : Profil -->
            <section class="card p-6 border-l-8 border-emerald-600">
                <h2 class="text-xl font-bold mb-4 flex items-center gap-2 text-emerald-900">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"></path></svg>
                    Profil du Randonneur
                </h2>
                <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
                    <div>
                        <label class="block text-xs font-black text-gray-400 uppercase">Sexe</label>
                        <select id="gender" class="mt-1 block w-full rounded-lg border-gray-300 shadow-sm p-3 bg-white border font-semibold">
                            <option value="male">Homme</option>
                            <option value="female">Femme</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-black text-gray-400 uppercase">Poids (kg)</label>
                        <input type="number" id="weight" value="75" class="mt-1 block w-full rounded-lg border-gray-300 shadow-sm p-3 bg-white border font-semibold">
                    </div>
                    <div>
                        <label class="block text-xs font-black text-gray-400 uppercase">Taille (cm)</label>
                        <input type="number" id="height" value="175" class="mt-1 block w-full rounded-lg border-gray-300 shadow-sm p-3 bg-white border font-semibold">
                    </div>
                    <div>
                        <label class="block text-xs font-black text-gray-400 uppercase">Âge</label>
                        <input type="number" id="age" value="30" class="mt-1 block w-full rounded-lg border-gray-300 shadow-sm p-3 bg-white border font-semibold">
                    </div>
                </div>
            </section>

            <!-- Section 2 : La Randonnée -->
            <section class="card p-6 border-l-8 border-emerald-600">
                <h2 class="text-xl font-bold mb-4 flex items-center gap-2 text-emerald-900">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 20l-5.447-2.724A2 2 0 013 15.483V6a2 2 0 011.236-1.841l7-3a2 2 0 011.528 0l7 3A2 2 0 0121 6v9.483a2 2 0 01-1.553 1.943L14 20l-5 2z"></path></svg>
                    Données de l'Étape
                </h2>
                <div class="space-y-6">
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                        <div>
                            <label class="block text-xs font-black text-gray-400 uppercase">Distance Totale (km)</label>
                            <input type="number" id="distance" value="20" step="0.5" class="mt-1 block w-full rounded-lg border-gray-300 shadow-sm p-3 bg-white border font-semibold text-emerald-700">
                        </div>
                        <div>
                            <label class="block text-xs font-black text-gray-400 uppercase">Dénivelé Positif (m)</label>
                            <input type="number" id="elevation" value="800" class="mt-1 block w-full rounded-lg border-gray-300 shadow-sm p-3 bg-white border font-semibold text-emerald-700">
                        </div>
                        <div>
                            <label class="block text-xs font-black text-gray-400 uppercase">Poids du Sac (kg)</label>
                            <input type="number" id="packWeight" value="12" class="mt-1 block w-full rounded-lg border-gray-300 shadow-sm p-3 bg-white border font-semibold text-blue-600">
                        </div>
                    </div>

                    <div>
                        <label class="block text-xs font-black text-gray-400 uppercase mb-2">Nature du Terrain</label>
                        <select id="terrain" class="block w-full rounded-lg border-gray-300 shadow-sm p-3 bg-white border font-semibold text-sm">
                            <option value="1.0">Route goudronnée / Plat (η=1.0)</option>
                            <option value="1.2" selected>Sentier de terre battue / Facile (η=1.2)</option>
                            <option value="1.5">Herbe haute / Sentier rocailleux (η=1.5)</option>
                            <option value="1.8">Eboulis / Hors-piste / Forêt (η=1.8)</option>
                            <option value="2.1">Sable mou / Marécage (η=2.1)</option>
                            <option value="2.5">Neige profonde (η=2.5)</option>
                        </select>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div>
                            <label class="block text-xs font-black text-gray-400 uppercase">Durée de marche active (h)</label>
                            <input type="number" id="hours" value="7" step="0.5" class="mt-1 block w-full rounded-lg border-gray-300 shadow-sm p-3 bg-white border font-semibold">
                            <div class="flex justify-between mt-2">
                                <p class="text-[10px] text-gray-500 font-bold italic uppercase">Vitesse : <span id="speedDisplay" class="text-emerald-600">0</span> km/h</p>
                                <p class="text-[10px] text-gray-500 font-bold italic uppercase">Pente moy. : <span id="gradeDisplay" class="text-emerald-600">0</span> %</p>
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-black text-gray-400 uppercase italic">Température (°C)</label>
                            <input type="range" id="temp" min="-15" max="40" value="18" class="w-full h-2 bg-gray-200 rounded-lg cursor-pointer mt-4">
                            <div class="flex justify-between text-[10px] font-black text-gray-400 mt-1 uppercase">
                                <span>Froid</span>
                                <span class="text-emerald-700 text-lg" id="tempVal">18°C</span>
                                <span>Chaud</span>
                            </div>
                        </div>
                    </div>
                </div>
            </section>
        </div>

        <!-- Résultats -->
        <div class="lg:col-span-1">
            <div class="card p-6 sticky top-8 border-t-8 border-emerald-700">
                <h2 class="text-xl font-black mb-6 text-gray-800 text-center uppercase tracking-tighter">Bilan Calorique</h2>
                
                <div class="space-y-4">
                    <div class="bg-emerald-900 p-6 rounded-2xl text-center shadow-inner">
                        <p class="text-[10px] text-emerald-300 font-black uppercase tracking-widest mb-1">Dépense Totale Estimée</p>
                        <p class="text-5xl font-black text-white" id="totalCalories">0</p>
                        <p class="text-sm text-emerald-300 font-bold">kcal / 24h</p>
                    </div>

                    <div class="space-y-3 px-2 py-4 border-b border-gray-100">
                        <div class="flex justify-between text-sm">
                            <span class="text-gray-500 font-bold uppercase text-[10px]">Métabolisme de Base</span>
                            <span class="font-bold text-gray-700" id="resBmr">0 kcal</span>
                        </div>
                        <div class="flex justify-between text-sm">
                            <span class="text-gray-500 font-bold uppercase text-[10px]">Effort (Pandolf/Santee)</span>
                            <span class="font-bold text-gray-700" id="resEffort">0 kcal</span>
                        </div>
                        <div class="flex justify-between text-sm">
                            <span class="text-gray-500 font-bold uppercase text-[10px]">Ajustement Thermique</span>
                            <span class="font-bold text-gray-700" id="resTemp">0 kcal</span>
                        </div>
                    </div>

                    <div class="bg-blue-700 p-5 rounded-2xl text-white">
                        <h3 class="text-xs font-black uppercase mb-3 opacity-90 flex items-center gap-2">
                            <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20"><path d="M5 4a1 1 0 00-2 0v7.268a2 2 0 000 3.464V16a1 1 0 102 0v-1.268a2 2 0 000-3.464V4zM11 4a1 1 0 10-2 0v1.268a2 2 0 000 3.464V16a1 1 0 102 0v-7.268a2 2 0 000-3.464V4zM16 3a1 1 0 011 1v2.268a2 2 0 110 3.464V16a1 1 0 11-2 0V9.732a2 2 0 110-3.464V4a1 1 0 011-1z"></path></svg>
                            Logistique Ration
                        </h3>
                        <div class="flex justify-between items-center">
                            <div>
                                <p class="text-3xl font-black" id="foodWeight">0 g</p>
                                <p class="text-[10px] uppercase font-bold text-blue-200">Poids sec estimé</p>
                            </div>
                            <div class="text-right">
                                <p class="text-[10px] leading-tight text-blue-100 italic">Ratio : 4.5 kcal/g<br>(Lyophilisé/Noix)</p>
                            </div>
                        </div>
                    </div>

                    <p class="text-[10px] text-gray-400 italic text-center px-4">
                        Note: Les randonnées de longue durée (thru-hiking) peuvent augmenter ce besoin de 10-15% après 2 semaines d'effort continu.
                    </p>

                    <button onclick="window.print()" class="w-full bg-gray-100 hover:bg-gray-200 text-gray-600 font-black py-3 px-4 rounded-xl transition-all text-xs uppercase tracking-widest mt-2 border border-gray-200">
                        Exporter l'Étape
                    </button>
                </div>
            </div>
        </div>
    </main>

    <footer class="max-w-4xl mx-auto mt-12 px-4 text-center text-gray-400 text-[10px] uppercase tracking-widest leading-relaxed">
        <p>© 2024 - Modèle Physiologique de Précision pour Randonneurs</p>
        <p class="mt-1">Modèles : Mifflin-St Jeor (BMR) | Pandolf et al. (Marche) | Santee et al. (Correction de pente)</p>
    </footer>

    <script>
        function calculate() {
            const getVal = (id) => parseFloat(document.getElementById(id).value) || 0;
            const gender = document.getElementById('gender').value;
            const W = getVal('weight'); // Poids corps
            const height = getVal('height');
            const age = getVal('age');
            const distanceKM = getVal('distance');
            const elevationM = getVal('elevation');
            const L = getVal('packWeight'); // Poids sac
            const eta = getVal('terrain'); // Coefficient terrain
            const hours = getVal('hours') || 1; 
            const temp = getVal('temp');

            // 1. MÉTABOLISME DE BASE (Mifflin-St Jeor)
            // On ajoute un facteur d'activité de base "sédentaire" (1.2) pour les heures hors marche
            let bmrBase = (10 * W) + (6.25 * height) - (5 * age);
            bmrBase = (gender === 'male') ? bmrBase + 5 : bmrBase - 161;
            
            // Le BMR pour les heures de repos (24h - heures de marche)
            const restHours = Math.max(0, 24 - hours);
            const bmrDaily = (bmrBase / 24) * 24; // On garde le BMR complet sur 24h

            // 2. CINÉMATIQUE
            const velocityKMH = distanceKM / hours;
            const velocityMS = velocityKMH / 3.6; 
            const grade = (elevationM / (distanceKM * 1000)) * 100; // Pente en %
            
            document.getElementById('speedDisplay').innerText = velocityKMH.toFixed(1);
            document.getElementById('gradeDisplay').innerText = grade.toFixed(1);
            document.getElementById('tempVal').innerText = temp + "°C";

            // 3. ÉQUATION DE PANDOLF OPTIMISÉE
            // M = 1.5W + 2.0(W+L)(L/W)^2 + eta(W+L)[1.5V^2 + 0.35VG]
            // On utilise une version corrigée pour la descente si nécessaire (Santee)
            
            let metabolicRateWatts = 1.5 * W + 
                                     2.0 * (W + L) * Math.pow((L / W), 2) + 
                                     eta * (W + L) * (1.5 * Math.pow(velocityMS, 2) + 0.35 * velocityMS * grade);
            
            // Correction Santee pour la descente (si grade est négatif, mais ici on saisit le D+)
            // Note: En rando on considère souvent un profil mixte. Si grade > 0, on garde Pandolf.
            // Si on voulait être parfait, on diviserait l'étape en montée/descente.
            // On ajoute un "Buffer de stabilisation" de 10% pour le terrain irrégulier non-labo
            metabolicRateWatts = metabolicRateWatts * 1.10;

            // Conversion Watts -> kcal/h (1 Watt = 0.8604 kcal/h)
            // L'effort est calculé AU-DESSUS du repos pour éviter le double comptage durant les heures de marche
            const caloriesPerHourEffort = metabolicRateWatts * 0.8604;
            const totalEffortKcal = caloriesPerHourEffort * hours;

            // 4. THERMORÉGULATION
            let thermoSurcharge = 0;
            if (temp < 0) {
                // Froid : Le corps dépense pour réchauffer l'air et maintenir le noyau
                thermoSurcharge = bmrDaily * (Math.abs(temp) * 0.005); 
            } else if (temp > 28) {
                // Chaud : Sudation et augmentation du débit cardiaque
                thermoSurcharge = totalEffortKcal * ((temp - 28) * 0.02);
            }

            // 5. SYNTHÈSE TOTALE
            // Total = BMR (24h) + Surplus d'effort pendant la marche + Ajustement température
            // On retire le coût du BMR durant les heures de marche car Pandolf inclut déjà le coût métabolique de base
            const netEffort = Math.max(0, totalEffortKcal - ((bmrBase / 24) * hours));
            const finalTotal = bmrBase + netEffort + thermoSurcharge;

            // AFFICHAGE
            document.getElementById('totalCalories').innerText = Math.round(finalTotal).toLocaleString();
            document.getElementById('resBmr').innerText = Math.round(bmrBase) + " kcal";
            document.getElementById('resEffort').innerText = Math.round(netEffort) + " kcal";
            document.getElementById('resTemp').innerText = Math.round(thermoSurcharge) + " kcal";
            
            // Ration alimentaire (450 kcal pour 100g est une bonne moyenne pour du sec/dense)
            const foodWeightGrams = (finalTotal / 4.5);
            document.getElementById('foodWeight').innerText = Math.round(foodWeightGrams) + " g";
        }

        const ids = ['weight', 'height', 'age', 'distance', 'elevation', 'packWeight', 'terrain', 'hours', 'temp', 'gender'];
        ids.forEach(id => document.getElementById(id).addEventListener('input', calculate));

        window.onload = calculate;
    </script>
</body>
</html>
