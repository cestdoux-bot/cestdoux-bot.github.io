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
        /* Style pour les navigateurs mobiles */
        input, select { font-size: 16px !important; } 
    </style>
</head>
<body class="bg-slate-50 min-h-screen pb-12">
    <!-- Header -->
    <header class="bg-emerald-700 text-white py-8 px-4 mb-8 shadow-lg">
        <div class="max-w-4xl mx-auto text-center">
            <h1 class="text-3xl font-bold mb-2">Calculateur de Calories Randonnée</h1>
            <p class="text-emerald-100 text-sm md:text-base">Optimisation du ravitaillement (Modèles Pandolf & Mifflin-St Jeor)</p>
        </div>
    </header>
    <main class="max-w-5xl mx-auto px-4 grid grid-cols-1 lg:grid-cols-3 gap-8">
        <!-- Formulaire de saisie -->
        <div class="lg:col-span-2 space-y-6">
            <!-- Section 1 : Profil -->
            <section class="card p-6 border-l-4 border-emerald-500">
                <h2 class="text-xl font-semibold mb-4 flex items-center gap-2 text-emerald-800">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"></path></svg>
                    Profil du Randonneur
                </h2>
                <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
                    <div>
                        <label class="block text-xs font-bold text-gray-500 uppercase italic">Sexe</label>
                        <select id="gender" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-2 bg-gray-50 border">
                            <option value="male">Homme</option>
                            <option value="female">Femme</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-gray-500 uppercase italic">Poids (kg)</label>
                        <input type="number" id="weight" value="75" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-2 bg-gray-50 border">
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-gray-500 uppercase italic">Taille (cm)</label>
                        <input type="number" id="height" value="175" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-2 bg-gray-50 border">
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-gray-500 uppercase italic">Âge</label>
                        <input type="number" id="age" value="30" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-2 bg-gray-50 border">
                    </div>
                </div>
            </section>
            <!-- Section 2 : La Randonnée -->
            <section class="card p-6 border-l-4 border-emerald-500">
                <h2 class="text-xl font-semibold mb-4 flex items-center gap-2 text-emerald-800">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 20l-5.447-2.724A2 2 0 013 15.483V6a2 2 0 011.236-1.841l7-3a2 2 0 011.528 0l7 3A2 2 0 0121 6v9.483a2 2 0 01-1.553 1.943L14 20l-5 2z"></path></svg>
                    Données de l'Étape
                </h2>
                <div class="space-y-6">
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                        <div>
                            <label class="block text-xs font-bold text-gray-500 uppercase italic">Distance (km)</label>
                            <input type="number" id="distance" value="20" step="0.5" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-2 bg-gray-50 border">
                        </div>
                        <div>
                            <label class="block text-xs font-bold text-gray-500 uppercase italic">Dénivelé Positif (m)</label>
                            <input type="number" id="elevation" value="800" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-2 bg-gray-50 border">
                        </div>
                        <div>
                            <label class="block text-xs font-bold text-gray-500 uppercase italic">Poids du Sac (kg)</label>
                            <input type="number" id="packWeight" value="12" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-2 bg-gray-50 border">
                        </div>
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-gray-500 uppercase mb-2 italic">Type de Terrain (η)</label>
                        <select id="terrain" class="block w-full rounded-md border-gray-300 shadow-sm p-2 bg-gray-50 border text-sm">
                            <option value="1.0">Route goudronnée / Plat (1.0)</option>
                            <option value="1.1" selected>Sentier facile / Terre battue (1.1)</option>
                            <option value="1.2">Herbe / Sentier classique (1.2)</option>
                            <option value="1.5">Forêt dense / Hors-piste / Éboulis (1.5)</option>
                            <option value="1.8">Sable mou / Marécage (1.8)</option>
                            <option value="2.1">Neige profonde (2.1)</option>
                        </select>
                    </div>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div>
                            <label class="block text-xs font-bold text-gray-500 uppercase italic">Durée de marche active (h)</label>
                            <input type="number" id="hours" value="7" step="0.5" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-2 bg-gray-50 border">
                            <p class="text-[10px] text-gray-400 mt-1 italic">Vitesse calculée : <span id="speedDisplay">0</span> km/h</p>
                        </div>
                        <div>
                            <label class="block text-xs font-bold text-gray-500 uppercase italic">Température Moyenne (°C)</label>
                            <input type="range" id="temp" min="-10" max="40" value="18" class="w-full h-2 bg-gray-200 rounded-lg cursor-pointer mt-4">
                            <div class="flex justify-between text-[10px] font-bold text-gray-400 mt-1 uppercase italic">
                                <span>Froid</span>
                                <span class="text-emerald-700" id="tempVal">18°C</span>
                                <span>Chaud</span>
                            </div>
                        </div>
                    </div>
                </div>
            </section>
        </div>
        <!-- Résultats -->
        <div class="lg:col-span-1">
            <div class="card p-6 sticky top-8 border-t-8 border-emerald-600">
                <h2 class="text-xl font-bold mb-6 text-gray-800 text-center uppercase tracking-tighter">Bilan Journalier</h2>
                <div class="space-y-4">
                    <div class="bg-emerald-50 p-6 rounded-xl text-center border border-emerald-100">
                        <p class="text-xs text-emerald-800 font-bold uppercase tracking-widest mb-1">Total Énergie</p>
                        <p class="text-5xl font-black text-emerald-700" id="totalCalories">0</p>
                        <p class="text-sm text-emerald-600 font-medium">kcal par jour</p>
                    </div>
                    <div class="space-y-3 px-2">
                        <div class="flex justify-between text-sm">
                            <span class="text-gray-500 font-medium">Repos (BMR)</span>
                            <span class="font-bold text-gray-700" id="resBmr">0 kcal</span>
                        </div>
                        <div class="flex justify-between text-sm">
                            <span class="text-gray-500 font-medium">Effort Physique</span>
                            <span class="font-bold text-gray-700" id="resEffort">0 kcal</span>
                        </div>
                        <div class="flex justify-between text-sm">
                            <span class="text-gray-500 font-medium">Thermorégulation</span>
                            <span class="font-bold text-gray-700" id="resTemp">0 kcal</span>
                        </div>
                    </div>
                    <div class="bg-blue-600 p-5 rounded-xl mt-6 text-white shadow-inner">
                        <h3 class="text-xs font-black uppercase mb-3 opacity-80 flex items-center gap-2">
                            <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20"><path d="M5 4a1 1 0 00-2 0v7.268a2 2 0 000 3.464V16a1 1 0 102 0v-1.268a2 2 0 000-3.464V4zM11 4a1 1 0 10-2 0v1.268a2 2 0 000 3.464V16a1 1 0 102 0v-7.268a2 2 0 000-3.464V4zM16 3a1 1 0 011 1v2.268a2 2 0 110 3.464V16a1 1 0 11-2 0V9.732a2 2 0 110-3.464V4a1 1 0 011-1z"></path></svg>
                            Logistique Sac à Dos
                        </h3>
                        <div class="flex justify-between items-center">
                            <div>
                                <p class="text-2xl font-black" id="foodWeight">0 g</p>
                                <p class="text-[10px] uppercase font-bold opacity-75">Poids nourriture</p>
                            </div>
                            <div class="text-right">
                                <p class="text-[10px] leading-tight opacity-70 italic">Basé sur<br>450 kcal / 100g</p>
                            </div>
                        </div>
                    </div>
                    <button onclick="window.print()" class="w-full border-2 border-gray-200 hover:border-emerald-500 hover:text-emerald-600 text-gray-400 font-bold py-2 px-4 rounded-lg transition-all text-sm mt-4">
                        Imprimer le bilan
                    </button>
                </div>
            </div>
        </div>
    </main>
    <footer class="max-w-4xl mx-auto mt-12 px-4 text-center text-gray-400 text-[10px] uppercase tracking-widest leading-relaxed">
        <p>Modèle basé sur l'équation de Pandolf et Mifflin-St Jeor.</p>
        <p class="mt-1">Ce calculateur est une aide à la décision, adaptez selon votre expérience.</p>
    </footer>
    <script>
        function calculate() {
            const getVal = (id) => parseFloat(document.getElementById(id).value) || 0;
            const gender = document.getElementById('gender').value;
            const weight = getVal('weight');
            const height = getVal('height');
            const age = getVal('age');
            const distance = getVal('distance');
            const elevation = getVal('elevation');
            const packWeight = getVal('packWeight');
            const terrainEta = getVal('terrain');
            const hours = getVal('hours') || 1; 
            const temp = getVal('temp');
            // 1. Métabolisme de Base
            let bmr = (10 * weight) + (6.25 * height) - (5 * age);
            bmr = (gender === 'male') ? bmr + 5 : bmr - 161;
            // 2. Cinématique
            const velocityMS = (distance * 1000) / (hours * 3600); 
            const grade = (elevation / (distance * 1000)) * 100; 
            document.getElementById('speedDisplay').innerText = (distance / hours).toFixed(1);
            document.getElementById('tempVal').innerText = temp + "°C";
            // 3. Équation de Pandolf
            const W = weight;
            const L = packWeight;
            const V = velocityMS;
            const G = grade;
            const eta = terrainEta;
            let watts = 1.5 * W + 
                        2.0 * (W + L) * Math.pow((L / W), 2) + 
                        eta * (W + L) * (1.5 * Math.pow(V, 2) + 0.35 * V * G);
            const effortKcal = watts * 0.8604 * hours;
            // 4. Thermorégulation
            let tempAdjustment = 0;
            if (temp < 5) tempAdjustment = (bmr + effortKcal) * 0.08; 
            if (temp > 32) tempAdjustment = (bmr + effortKcal) * 0.05; 
            // 5. Total
            const total = bmr + effortKcal + tempAdjustment;
            // Affichage
            document.getElementById('totalCalories').innerText = Math.round(total).toLocaleString();
            document.getElementById('resBmr').innerText = Math.round(bmr) + " kcal";
            document.getElementById('resEffort').innerText = Math.round(effortKcal) + " kcal";
            document.getElementById('resTemp').innerText = Math.round(tempAdjustment) + " kcal";
            const foodWeightGrams = (total / 450) * 100;
            document.getElementById('foodWeight').innerText = Math.round(foodWeightGrams) + " g";
        }
        const ids = ['weight', 'height', 'age', 'distance', 'elevation', 'packWeight', 'terrain', 'hours', 'temp', 'gender'];
        ids.forEach(id => document.getElementById(id).addEventListener('input', calculate));
        window.onload = calculate;
    </script>
</body>
</html>
