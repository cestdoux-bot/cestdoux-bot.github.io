<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculateur de Dépense Énergétique - Randonnée</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .card { background: white; border-radius: 1rem; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1); }
        input[type="range"] { accent-color: #059669; }
        input, select { font-size: 14px !important; }
        .progress-bar { transition: width 0.5s ease-in-out; }
        .table-input { @apply w-full bg-transparent border-b border-transparent hover:border-slate-200 focus:border-blue-500 outline-none transition-colors text-center font-medium py-1; }
    </style>
</head>
<body class="bg-slate-100 min-h-screen pb-12 text-slate-800">

    <header class="bg-emerald-800 text-white py-8 px-4 mb-8 shadow-xl">
        <div class="max-w-6xl mx-auto text-center">
            <h1 class="text-3xl font-extrabold mb-2 tracking-tight">Analyseur Nutritionnel Randonnée</h1>
            <p class="text-emerald-100 text-sm md:text-base opacity-90">Physiologie de l'effort & Optimisation des Macros</p>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 space-y-8">
        
        <!-- GRID PRINCIPALE : CALCULS PHYSIOLOGIQUES -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
            <div class="lg:col-span-2 space-y-6">
                <!-- Section 1 : Profil & Terrain -->
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <section class="card p-6 border-l-8 border-emerald-600">
                        <h2 class="text-lg font-bold mb-4 flex items-center gap-2 text-emerald-900 uppercase tracking-tight">
                            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"></path></svg>
                            Le Randonneur
                        </h2>
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label class="block text-[10px] font-black text-gray-400 uppercase">Poids (kg)</label>
                                <input type="number" id="weight" value="68" class="mt-1 block w-full rounded-lg border-gray-200 shadow-sm p-2 bg-slate-50 border font-bold">
                            </div>
                            <div>
                                <label class="block text-[10px] font-black text-gray-400 uppercase">Sac (kg)</label>
                                <input type="number" id="packWeight" value="15" class="mt-1 block w-full rounded-lg border-gray-200 shadow-sm p-2 bg-blue-50 border border-blue-100 font-bold text-blue-700">
                            </div>
                            <div class="col-span-2">
                                <label class="block text-[10px] font-black text-gray-400 uppercase">Sexe & Âge</label>
                                <div class="flex gap-2">
                                    <select id="gender" class="mt-1 block w-1/2 rounded-lg border-gray-200 p-2 bg-slate-50 border font-bold">
                                        <option value="male">Homme</option>
                                        <option value="female">Femme</option>
                                    </select>
                                    <input type="number" id="age" value="30" class="mt-1 block w-1/2 rounded-lg border-gray-200 p-2 bg-slate-50 border font-bold">
                                </div>
                            </div>
                        </div>
                    </section>

                    <section class="card p-6 border-l-8 border-emerald-600">
                        <h2 class="text-lg font-bold mb-4 flex items-center gap-2 text-emerald-900 uppercase tracking-tight">
                            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"></path></svg>
                            L'Étape
                        </h2>
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label class="block text-[10px] font-black text-gray-400 uppercase">Distance (km)</label>
                                <input type="number" id="distance" value="20" class="mt-1 block w-full rounded-lg border-gray-200 p-2 bg-slate-50 border font-bold">
                            </div>
                            <div>
                                <label class="block text-[10px] font-black text-gray-400 uppercase">Dénivelé + (m)</label>
                                <input type="number" id="elevation" value="800" class="mt-1 block w-full rounded-lg border-gray-200 p-2 bg-slate-50 border font-bold">
                            </div>
                            <div class="col-span-2">
                                <label class="block text-[10px] font-black text-gray-400 uppercase">Terrain</label>
                                <select id="terrain" class="mt-1 block w-full rounded-lg border-gray-200 p-2 bg-slate-50 border font-bold">
                                    <option value="1.0">Bitume (1.0)</option>
                                    <option value="1.2" selected>Sentier Terre (1.2)</option>
                                    <option value="1.5">Herbe / Sentier classique (1.5)</option>
                                    <option value="1.8">Eboulis / Hors-piste (1.8)</option>
                                    <option value="2.5">Neige profonde (2.5)</option>
                                </select>
                            </div>
                        </div>
                    </section>
                </div>

                <section class="card p-6 border-l-8 border-emerald-600">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-8 items-center">
                        <div>
                            <label class="block text-[10px] font-black text-gray-400 uppercase">Temps de marche (h)</label>
                            <input type="number" id="hours" value="4.5" step="0.5" class="mt-1 block w-full rounded-lg border-gray-200 p-3 bg-white border font-black text-xl text-emerald-700">
                            <div class="flex justify-between mt-2 text-[10px] font-bold text-slate-400 italic">
                                <span>Vitesse : <span id="speedDisplay" class="text-emerald-600">0</span> km/h</span>
                                <span>Pente : <span id="gradeDisplay" class="text-emerald-600">0</span> %</span>
                            </div>
                        </div>
                        <div>
                            <label class="block text-[10px] font-black text-gray-400 uppercase italic">Température Extérieure : <span class="text-emerald-700" id="tempVal">18°C</span></label>
                            <input type="range" id="temp" min="-15" max="40" value="18" class="w-full h-2 bg-gray-200 rounded-lg cursor-pointer mt-4">
                            <div class="flex justify-between text-[9px] font-black text-gray-300 mt-1 uppercase">
                                <span>Grand Froid</span>
                                <span>Canicule</span>
                            </div>
                        </div>
                    </div>
                </section>
            </div>

            <!-- BILAN CALORIQUE STICKY -->
            <div class="lg:col-span-1">
                <div class="card p-6 sticky top-8 border-t-8 border-emerald-700">
                    <h2 class="text-xl font-black mb-6 text-gray-800 text-center uppercase tracking-tighter">Bilan de l'Étape</h2>
                    
                    <div class="space-y-4">
                        <div class="bg-emerald-900 p-6 rounded-2xl text-center shadow-inner">
                            <p class="text-[10px] text-emerald-300 font-black uppercase tracking-widest mb-1">Cible Calorique (24h)</p>
                            <p class="text-5xl font-black text-white" id="totalCalories">0</p>
                            <p class="text-sm text-emerald-300 font-bold">kcal</p>
                        </div>

                        <div class="p-4 bg-slate-50 rounded-xl border border-slate-200 space-y-3">
                            <div class="flex justify-between items-end">
                                <p class="text-[10px] font-black text-slate-400 uppercase">Apport Sélectionné</p>
                                <p class="text-xl font-black text-slate-700"><span id="carriedCalories">0</span> <span class="text-xs text-slate-400 font-bold">kcal</span></p>
                            </div>
                            <div class="w-full bg-slate-200 rounded-full h-4 overflow-hidden shadow-inner">
                                <div id="energyProgress" class="progress-bar bg-emerald-500 h-full w-0"></div>
                            </div>
                            <p id="energyStatus" class="text-[10px] text-center font-bold text-slate-500 uppercase italic tracking-tighter">Analyse en cours...</p>
                        </div>

                        <div class="grid grid-cols-2 gap-2">
                            <div class="bg-blue-50 p-3 rounded-xl border border-blue-100 text-center">
                                <p class="text-[9px] font-black text-blue-400 uppercase">Poids nourriture</p>
                                <p id="footerTotalWeight" class="text-lg font-black text-blue-800">0 g</p>
                            </div>
                            <div class="bg-blue-50 p-3 rounded-xl border border-blue-100 text-center">
                                <p class="text-[9px] font-black text-blue-400 uppercase">Différence</p>
                                <p id="footerDiff" class="text-lg font-black text-emerald-600">0</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- SECTION NOURRITURE & MACROS -->
        <section class="card p-6 border-l-8 border-blue-600 overflow-hidden">
            <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-6 gap-4">
                <h2 class="text-xl font-bold flex items-center gap-2 text-blue-900">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10"></path></svg>
                    Détail Nutritionnel de la Ration
                </h2>
                <button onclick="addFoodRow()" class="bg-blue-600 hover:bg-blue-700 text-white text-xs font-black py-2.5 px-5 rounded-xl uppercase tracking-widest transition-all shadow-lg active:scale-95">
                    + Ajouter un aliment
                </button>
            </div>

            <div class="overflow-x-auto -mx-6 px-6">
                <table class="w-full text-left border-collapse min-w-[800px]">
                    <thead>
                        <tr class="text-[9px] font-black text-gray-400 uppercase tracking-widest border-b-2 border-slate-100">
                            <th class="py-3 px-1 text-center w-12">On</th>
                            <th class="py-3 px-2 w-1/4">Aliment</th>
                            <th class="py-3 px-2 text-center">Poids (g)</th>
                            <th class="py-3 px-2 text-center bg-emerald-50 text-emerald-700">Kcal</th>
                            <th class="py-3 px-2 text-center">Glucides</th>
                            <th class="py-3 px-2 text-center">Lipides</th>
                            <th class="py-3 px-2 text-center">Prot.</th>
                            <th class="py-3 px-2 text-center">Sod.(mg)</th>
                            <th class="py-3 px-2 text-center">Fibres</th>
                            <th class="py-3 px-1 text-center w-10"></th>
                        </tr>
                    </thead>
                    <tbody id="foodTableBody" class="divide-y divide-slate-50">
                        <!-- Lignes injectées -->
                    </tbody>
                </table>
            </div>

            <!-- ANALYSE DES MACROS -->
            <div class="mt-8 grid grid-cols-2 md:grid-cols-5 gap-4">
                <div class="p-4 rounded-2xl bg-amber-50 border border-amber-100">
                    <p class="text-[10px] font-black text-amber-500 uppercase mb-1">Glucides (Énergie)</p>
                    <p id="totalCarbs" class="text-2xl font-black text-amber-900">0<span class="text-xs ml-1">g</span></p>
                </div>
                <div class="p-4 rounded-2xl bg-rose-50 border border-rose-100">
                    <p class="text-[10px] font-black text-rose-500 uppercase mb-1">Lipides (Fond)</p>
                    <p id="totalFats" class="text-2xl font-black text-rose-900">0<span class="text-xs ml-1">g</span></p>
                </div>
                <div class="p-4 rounded-2xl bg-indigo-50 border border-indigo-100">
                    <p class="text-[10px] font-black text-indigo-500 uppercase mb-1">Protéines (Récup)</p>
                    <p id="totalProts" class="text-2xl font-black text-indigo-900">0<span class="text-xs ml-1">g</span></p>
                </div>
                <div class="p-4 rounded-2xl bg-slate-100 border border-slate-200">
                    <p class="text-[10px] font-black text-slate-500 uppercase mb-1">Sodium (Sels)</p>
                    <p id="totalSodium" class="text-2xl font-black text-slate-900">0<span class="text-xs ml-1">mg</span></p>
                </div>
                <div class="p-4 rounded-2xl bg-emerald-50 border border-emerald-100">
                    <p class="text-[10px] font-black text-emerald-500 uppercase mb-1">Fibres (Digest.)</p>
                    <p id="totalFiber" class="text-2xl font-black text-emerald-900">0<span class="text-xs ml-1">g</span></p>
                </div>
            </div>
        </section>
    </main>

    <script>
        // --- ÉTAT INITIAL ---
        let foodItems = [
            { name: "Petit Déjeuner Muesli", kcal: 550, weight: 120, carbs: 80, fats: 15, proteins: 12, sodium: 100, fiber: 10, checked: true },
            { name: "Mélange Noix & Raisins", kcal: 620, weight: 100, carbs: 35, fats: 48, proteins: 15, sodium: 250, fiber: 8, checked: true },
            { name: "Saucisson (50g)", kcal: 210, weight: 50, carbs: 1, fats: 18, proteins: 11, sodium: 900, fiber: 0, checked: true },
            { name: "Lyophilisé Soir", kcal: 850, weight: 180, carbs: 110, fats: 32, proteins: 28, sodium: 1200, fiber: 6, checked: true }
        ];

        let targetCalories = 0;

        // --- CALCULS PHYSIOLOGIQUES ---
        function calculate() {
            const getVal = (id) => parseFloat(document.getElementById(id).value) || 0;
            const gender = document.getElementById('gender').value;
            const W = getVal('weight'); 
            const height = 175; // Fixe pour démo
            const age = getVal('age');
            const distanceKM = getVal('distance');
            const elevationM = getVal('elevation');
            const L = getVal('packWeight'); 
            const eta = getVal('terrain'); 
            const hours = getVal('hours') || 1; 
            const temp = getVal('temp');

            let bmrBase = (10 * W) + (6.25 * height) - (5 * age);
            bmrBase = (gender === 'male') ? bmrBase + 5 : bmrBase - 161;
            
            const velocityKMH = distanceKM / hours;
            const velocityMS = velocityKMH / 3.6; 
            const grade = (elevationM / (distanceKM * 1000)) * 100; 
            
            document.getElementById('speedDisplay').innerText = velocityKMH.toFixed(1);
            document.getElementById('gradeDisplay').innerText = grade.toFixed(1);
            document.getElementById('tempVal').innerText = temp + "°C";

            let metabolicRateWatts = 1.5 * W + 
                                     2.0 * (W + L) * Math.pow((L / W), 2) + 
                                     eta * (W + L) * (1.5 * Math.pow(velocityMS, 2) + 0.35 * velocityMS * grade);
            
            metabolicRateWatts = metabolicRateWatts * 1.15; // Facteur stabilisation terrain

            const caloriesPerHourEffort = metabolicRateWatts * 0.8604;
            const totalEffortKcal = caloriesPerHourEffort * hours;
            let adjustments = (totalEffortKcal * 0.10); // Afterburn

            if (temp < 5) adjustments += bmrBase * (Math.abs(temp - 5) * 0.01); 
            else if (temp > 30) adjustments += totalEffortKcal * ((temp - 30) * 0.03);

            const bmrDuringHike = (bmrBase / 24) * hours;
            const finalTotal = bmrBase + Math.max(0, totalEffortKcal - bmrDuringHike) + adjustments;

            targetCalories = finalTotal;
            document.getElementById('totalCalories').innerText = Math.round(finalTotal).toLocaleString();
            updateFoodAnalysis();
        }

        // --- GESTION NUTRITION ---
        function updateFoodAnalysis() {
            let totals = { kcal: 0, weight: 0, carbs: 0, fats: 0, proteins: 0, sodium: 0, fiber: 0 };

            foodItems.forEach(item => {
                if (item.checked) {
                    totals.kcal += item.kcal;
                    totals.weight += item.weight;
                    totals.carbs += item.carbs;
                    totals.fats += item.fats;
                    totals.proteins += item.proteins;
                    totals.sodium += item.sodium;
                    totals.fiber += item.fiber;
                }
            });

            // UI Principale
            const percentage = Math.min(100, (totals.kcal / targetCalories) * 100);
            const progressBar = document.getElementById('energyProgress');
            progressBar.style.width = percentage + "%";
            progressBar.className = percentage < 85 ? "progress-bar bg-amber-500 h-full w-0" : 
                                  (percentage > 115 ? "progress-bar bg-rose-500 h-full w-0" : "progress-bar bg-emerald-500 h-full w-0");

            document.getElementById('carriedCalories').innerText = Math.round(totals.kcal).toLocaleString();
            document.getElementById('footerTotalWeight').innerText = Math.round(totals.weight) + " g";
            
            const diff = totals.kcal - targetCalories;
            document.getElementById('footerDiff').innerText = (diff > 0 ? "+" : "") + Math.round(diff);
            
            // Dashboard Macros
            document.getElementById('totalCarbs').innerHTML = `${Math.round(totals.carbs)}<span class="text-xs ml-1 font-bold opacity-40">g</span>`;
            document.getElementById('totalFats').innerHTML = `${Math.round(totals.fats)}<span class="text-xs ml-1 font-bold opacity-40">g</span>`;
            document.getElementById('totalProts').innerHTML = `${Math.round(totals.proteins)}<span class="text-xs ml-1 font-bold opacity-40">g</span>`;
            document.getElementById('totalSodium').innerHTML = `${Math.round(totals.sodium)}<span class="text-xs ml-1 font-bold opacity-40">mg</span>`;
            document.getElementById('totalFiber').innerHTML = `${Math.round(totals.fiber)}<span class="text-xs ml-1 font-bold opacity-40">g</span>`;

            const status = document.getElementById('energyStatus');
            if (totals.kcal === 0) status.innerText = "Ajoutez des aliments";
            else if (percentage < 90) status.innerText = "Déficit important";
            else if (percentage > 110) status.innerText = "Excédent : Sac optimisable";
            else status.innerText = "Équilibre optimal";
        }

        function renderFoodTable() {
            const body = document.getElementById('foodTableBody');
            body.innerHTML = "";

            foodItems.forEach((item, index) => {
                const tr = document.createElement('tr');
                tr.className = item.checked ? "bg-white" : "bg-slate-50 opacity-60";
                tr.innerHTML = `
                    <td class="py-3 px-1 text-center"><input type="checkbox" ${item.checked ? 'checked' : ''} onchange="toggleItem(${index})" class="w-5 h-5 accent-emerald-600 cursor-pointer"></td>
                    <td class="py-3 px-2"><input type="text" value="${item.name}" onchange="updateItem(${index}, 'name', this.value)" class="table-input !text-left font-bold text-slate-700"></td>
                    <td class="py-3 px-2"><input type="number" value="${item.weight}" onchange="updateItem(${index}, 'weight', this.value)" class="table-input"></td>
                    <td class="py-3 px-2 bg-emerald-50/50"><input type="number" value="${item.kcal}" onchange="updateItem(${index}, 'kcal', this.value)" class="table-input !text-emerald-700 font-black"></td>
                    <td class="py-3 px-2"><input type="number" value="${item.carbs}" onchange="updateItem(${index}, 'carbs', this.value)" class="table-input !text-amber-700"></td>
                    <td class="py-3 px-2"><input type="number" value="${item.fats}" onchange="updateItem(${index}, 'fats', this.value)" class="table-input !text-rose-700"></td>
                    <td class="py-3 px-2"><input type="number" value="${item.proteins}" onchange="updateItem(${index}, 'proteins', this.value)" class="table-input !text-indigo-700"></td>
                    <td class="py-3 px-2"><input type="number" value="${item.sodium}" onchange="updateItem(${index}, 'sodium', this.value)" class="table-input !text-slate-700"></td>
                    <td class="py-3 px-2"><input type="number" value="${item.fiber}" onchange="updateItem(${index}, 'fiber', this.value)" class="table-input !text-emerald-700"></td>
                    <td class="py-3 px-1">
                        <button onclick="removeFoodRow(${index})" class="text-slate-300 hover:text-red-500 transition-colors">
                            <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd"></path></svg>
                        </button>
                    </td>
                `;
                body.appendChild(tr);
            });
            updateFoodAnalysis();
        }

        function addFoodRow() {
            foodItems.push({ name: "Nouvel aliment", kcal: 0, weight: 0, carbs: 0, fats: 0, proteins: 0, sodium: 0, fiber: 0, checked: true });
            renderFoodTable();
        }

        function removeFoodRow(index) {
            foodItems.splice(index, 1);
            renderFoodTable();
        }

        function toggleItem(index) {
            foodItems[index].checked = !foodItems[index].checked;
            renderFoodTable();
        }

        function updateItem(index, key, value) {
            if (key !== 'name') foodItems[index][key] = parseFloat(value) || 0;
            else foodItems[index][key] = value;
            updateFoodAnalysis();
        }

        const ids = ['weight', 'age', 'distance', 'elevation', 'packWeight', 'terrain', 'hours', 'temp', 'gender'];
        ids.forEach(id => document.getElementById(id).addEventListener('input', calculate));
        
        window.onload = () => {
            calculate();
            renderFoodTable();
        };
    </script>
</body>
</html>
