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
        /* Pour supprimer les flèches sur les inputs number */
        input::-webkit-outer-spin-button, input::-webkit-inner-spin-button { -webkit-appearance: none; margin: 0; }
    </style>
</head>
<body class="bg-slate-100 min-h-screen pb-12 text-slate-800">

    <header class="bg-emerald-800 text-white py-6 px-4 mb-8 shadow-xl">
        <div class="max-w-[95%] mx-auto flex flex-col md:flex-row justify-between items-center gap-4">
            <div class="text-left">
                <h1 class="text-2xl font-extrabold tracking-tight italic">TrailCalorie Pro <span class="text-emerald-400 font-light">v2.0</span></h1>
                <p class="text-emerald-200 text-xs opacity-90 uppercase font-bold tracking-widest">Analyse Physiologique & Nutritionnelle Large</p>
            </div>
            <div class="flex gap-4">
                <div class="bg-emerald-900/50 px-4 py-2 rounded-lg border border-emerald-700">
                    <p class="text-[9px] uppercase font-black text-emerald-300">Statut Énergétique</p>
                    <p id="headerStatus" class="text-sm font-bold">Initialisation...</p>
                </div>
            </div>
        </div>
    </header>

    <!-- Utilisation de 95% de la largeur pour le mode large -->
    <main class="max-w-[95%] mx-auto space-y-8">
        
        <!-- SECTION SUPÉRIEURE : PARAMÈTRES ET BILAN -->
        <div class="grid grid-cols-1 lg:grid-cols-4 gap-6">
            
            <!-- PARAMÈTRES (3 COLONNES) -->
            <div class="lg:col-span-3 grid grid-cols-1 md:grid-cols-3 gap-6">
                
                <!-- Profil -->
                <section class="card p-5 border-l-4 border-emerald-600">
                    <h2 class="text-sm font-black mb-4 flex items-center gap-2 text-emerald-900 uppercase">
                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"></path></svg>
                        Randonneur
                    </h2>
                    <div class="space-y-3">
                        <div class="flex gap-2">
                            <div class="w-1/2">
                                <label class="text-[9px] font-black text-gray-400 uppercase">Sexe</label>
                                <select id="gender" class="w-full rounded-lg border-gray-200 p-2 bg-slate-50 border font-bold">
                                    <option value="male">Homme</option>
                                    <option value="female">Femme</option>
                                </select>
                            </div>
                            <div class="w-1/2">
                                <label class="text-[9px] font-black text-gray-400 uppercase">Âge</label>
                                <input type="number" id="age" value="30" class="w-full rounded-lg border-gray-200 p-2 bg-slate-50 border font-bold">
                            </div>
                        </div>
                        <div class="flex gap-2">
                            <div class="w-1/2">
                                <label class="text-[9px] font-black text-gray-400 uppercase">Poids Corps (kg)</label>
                                <input type="number" id="weight" value="68" class="w-full rounded-lg border-gray-200 p-2 bg-slate-50 border font-bold">
                            </div>
                            <div class="w-1/2">
                                <label class="text-[9px] font-black text-gray-400 uppercase text-blue-500">Sac à dos (kg)</label>
                                <input type="number" id="packWeight" value="15" class="w-full rounded-lg border-blue-200 p-2 bg-blue-50 border font-bold text-blue-700">
                            </div>
                        </div>
                    </div>
                </section>

                <!-- Terrain -->
                <section class="card p-5 border-l-4 border-emerald-600">
                    <h2 class="text-sm font-black mb-4 flex items-center gap-2 text-emerald-900 uppercase">
                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"></path></svg>
                        Parcours
                    </h2>
                    <div class="space-y-3">
                        <div class="flex gap-2">
                            <div class="w-1/2">
                                <label class="text-[9px] font-black text-gray-400 uppercase">Distance (km)</label>
                                <input type="number" id="distance" value="20" class="w-full rounded-lg border-gray-200 p-2 bg-slate-50 border font-bold">
                            </div>
                            <div class="w-1/2">
                                <label class="text-[9px] font-black text-gray-400 uppercase">Dénivelé + (m)</label>
                                <input type="number" id="elevation" value="800" class="w-full rounded-lg border-gray-200 p-2 bg-slate-50 border font-bold">
                            </div>
                        </div>
                        <div>
                            <label class="text-[9px] font-black text-gray-400 uppercase">Type de Terrain</label>
                            <select id="terrain" class="w-full rounded-lg border-gray-200 p-2 bg-slate-50 border font-bold">
                                <option value="1.0">Bitume / Facile (1.0)</option>
                                <option value="1.2" selected>Sentier Terre (1.2)</option>
                                <option value="1.5">Herbe / Sentier classique (1.5)</option>
                                <option value="1.8">Eboulis / Hors-piste (1.8)</option>
                                <option value="2.5">Neige profonde (2.5)</option>
                            </select>
                        </div>
                    </div>
                </section>

                <!-- Effort -->
                <section class="card p-5 border-l-4 border-emerald-600">
                    <h2 class="text-sm font-black mb-4 flex items-center gap-2 text-emerald-900 uppercase">
                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                        Temps & Météo
                    </h2>
                    <div class="space-y-4">
                        <div>
                            <label class="text-[9px] font-black text-gray-400 uppercase">Heures de marche</label>
                            <input type="number" id="hours" value="4.5" step="0.5" class="w-full rounded-lg border-gray-200 p-2 bg-white border font-black text-lg text-emerald-700">
                            <div class="flex justify-between mt-1 text-[9px] font-bold text-slate-400 italic">
                                <span><span id="speedDisplay">0</span> km/h</span>
                                <span>Pente : <span id="gradeDisplay">0</span>%</span>
                            </div>
                        </div>
                        <div>
                            <div class="flex justify-between items-center mb-1">
                                <label class="text-[9px] font-black text-gray-400 uppercase">Temp. : <span class="text-emerald-700" id="tempVal">18°C</span></label>
                            </div>
                            <input type="range" id="temp" min="-15" max="40" value="18" class="w-full h-1.5 bg-gray-200 rounded-lg cursor-pointer">
                        </div>
                    </div>
                </section>
            </div>

            <!-- BILAN (1 COLONNE) -->
            <section class="card p-5 border-t-4 border-emerald-700 flex flex-col justify-between">
                <div>
                    <p class="text-[10px] text-slate-400 font-black uppercase text-center mb-2">Cible Énergétique</p>
                    <div class="text-center">
                        <span id="totalCalories" class="text-4xl font-black text-slate-800">0</span>
                        <span class="text-sm font-bold text-slate-400 ml-1">kcal</span>
                    </div>
                </div>
                
                <div class="mt-4 space-y-2">
                    <div class="flex justify-between text-[10px] items-center">
                        <span class="font-bold text-slate-500 uppercase">Apport</span>
                        <span class="font-black text-slate-800"><span id="carriedCalories">0</span> kcal</span>
                    </div>
                    <div class="w-full bg-slate-100 rounded-full h-3 overflow-hidden border border-slate-200">
                        <div id="energyProgress" class="progress-bar bg-emerald-500 h-full w-0"></div>
                    </div>
                    <div class="flex justify-between text-[10px] items-center pt-1 border-t border-slate-50">
                        <span class="font-bold text-slate-500 uppercase">Poids nourriture</span>
                        <span id="footerTotalWeight" class="font-black text-blue-600">0 g</span>
                    </div>
                </div>
            </section>
        </div>

        <!-- SECTION INVENTAIRE - LARGEUR MAXIMALE -->
        <section class="card border-l-8 border-blue-600 overflow-hidden">
            <div class="p-6 border-b border-slate-100 flex justify-between items-center bg-slate-50/50">
                <div>
                    <h2 class="text-xl font-black text-blue-900 uppercase tracking-tight">Plan de Ravitaillement</h2>
                    <p class="text-xs text-blue-500 font-medium">Gérez vos aliments et surveillez l'équilibre des macronutriments</p>
                </div>
                <button onclick="addFoodRow()" class="bg-blue-600 hover:bg-blue-700 text-white text-xs font-black py-3 px-6 rounded-xl uppercase tracking-widest transition-all shadow-lg hover:shadow-blue-200 active:scale-95 flex items-center gap-2">
                    <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M12 4v16m8-8H4"></path></svg>
                    Ajouter un aliment
                </button>
            </div>

            <div class="overflow-x-auto">
                <table class="w-full text-left border-collapse table-fixed min-w-[1000px]">
                    <thead>
                        <tr class="text-[10px] font-black text-slate-400 uppercase tracking-widest bg-slate-50">
                            <th class="py-4 px-4 w-16 text-center">Sélec.</th>
                            <th class="py-4 px-2 w-auto text-left">Nom de l'aliment</th>
                            <th class="py-4 px-2 w-24 text-center">Poids (g)</th>
                            <th class="py-4 px-2 w-28 text-center bg-emerald-50 text-emerald-700">Calories</th>
                            <th class="py-4 px-2 w-24 text-center">Glucides</th>
                            <th class="py-4 px-2 w-24 text-center">Lipides</th>
                            <th class="py-4 px-2 w-24 text-center">Prot.</th>
                            <th class="py-4 px-2 w-24 text-center">Sod.(mg)</th>
                            <th class="py-4 px-2 w-24 text-center">Fibres</th>
                            <th class="py-4 px-4 w-16 text-center"></th>
                        </tr>
                    </thead>
                    <tbody id="foodTableBody" class="divide-y divide-slate-100">
                        <!-- Lignes de nourriture injectées -->
                    </tbody>
                </table>
            </div>

            <!-- DASHBOARD DES MACROS ÉLARGI -->
            <div class="p-6 bg-slate-50 border-t border-slate-100">
                <div class="grid grid-cols-2 md:grid-cols-6 gap-4">
                    <div class="p-4 rounded-2xl bg-white shadow-sm border border-slate-200 text-center">
                        <p class="text-[9px] font-black text-slate-400 uppercase mb-1">Poids Total</p>
                        <p id="totalWeightDash" class="text-xl font-black text-slate-800">0<span class="text-xs ml-0.5 opacity-40">g</span></p>
                    </div>
                    <div class="p-4 rounded-2xl bg-white shadow-sm border border-amber-100 text-center">
                        <p class="text-[9px] font-black text-amber-500 uppercase mb-1">Glucides</p>
                        <p id="totalCarbs" class="text-xl font-black text-amber-900">0<span class="text-xs ml-0.5 opacity-40">g</span></p>
                    </div>
                    <div class="p-4 rounded-2xl bg-white shadow-sm border border-rose-100 text-center">
                        <p class="text-[9px] font-black text-rose-500 uppercase mb-1">Lipides</p>
                        <p id="totalFats" class="text-xl font-black text-rose-900">0<span class="text-xs ml-0.5 opacity-40">g</span></p>
                    </div>
                    <div class="p-4 rounded-2xl bg-white shadow-sm border border-indigo-100 text-center">
                        <p class="text-[9px] font-black text-indigo-500 uppercase mb-1">Protéines</p>
                        <p id="totalProts" class="text-xl font-black text-indigo-900">0<span class="text-xs ml-0.5 opacity-40">g</span></p>
                    </div>
                    <div class="p-4 rounded-2xl bg-white shadow-sm border border-slate-200 text-center">
                        <p class="text-[9px] font-black text-slate-500 uppercase mb-1">Sodium</p>
                        <p id="totalSodium" class="text-xl font-black text-slate-900">0<span class="text-xs ml-0.5 opacity-40">mg</span></p>
                    </div>
                    <div class="p-4 rounded-2xl bg-white shadow-sm border border-emerald-100 text-center">
                        <p class="text-[9px] font-black text-emerald-500 uppercase mb-1">Fibres</p>
                        <p id="totalFiber" class="text-xl font-black text-emerald-900">0<span class="text-xs ml-0.5 opacity-40">g</span></p>
                    </div>
                </div>
            </div>
        </section>
    </main>

    <script>
        // --- ÉTAT ---
        let foodItems = [
            { name: "Muesli Bivouac", kcal: 520, weight: 125, carbs: 75, fats: 14, proteins: 12, sodium: 80, fiber: 9, checked: true },
            { name: "Barre Noix & Miel", kcal: 240, weight: 45, carbs: 22, fats: 15, proteins: 5, sodium: 110, fiber: 3, checked: true },
            { name: "Fromage à pâte dure", kcal: 410, weight: 100, carbs: 0, fats: 33, proteins: 25, sodium: 650, fiber: 0, checked: true },
            { name: "Lyophilisé Poulet Curry", kcal: 780, weight: 170, carbs: 95, fats: 28, proteins: 35, sodium: 1400, fiber: 5, checked: true }
        ];

        let targetCalories = 0;

        // --- CALCULS ---
        function calculate() {
            const getVal = (id) => parseFloat(document.getElementById(id).value) || 0;
            const gender = document.getElementById('gender').value;
            const W = getVal('weight'); 
            const age = getVal('age');
            const distanceKM = getVal('distance');
            const elevationM = getVal('elevation');
            const L = getVal('packWeight'); 
            const eta = getVal('terrain'); 
            const hours = getVal('hours') || 1; 
            const temp = getVal('temp');

            let bmrBase = (10 * W) + (6.25 * 175) - (5 * age);
            bmrBase = (gender === 'male') ? bmrBase + 5 : bmrBase - 161;
            
            const velocityKMH = distanceKM / hours;
            const velocityMS = velocityKMH / 3.6; 
            const grade = (elevationM / (distanceKM * 1000)) * 100; 
            
            document.getElementById('speedDisplay').innerText = velocityKMH.toFixed(1);
            document.getElementById('gradeDisplay').innerText = grade.toFixed(1);
            document.getElementById('tempVal').innerText = temp + "°C";

            let metabolicRateWatts = 1.5 * W + 2.0 * (W + L) * Math.pow((L / W), 2) + eta * (W + L) * (1.5 * Math.pow(velocityMS, 2) + 0.35 * velocityMS * grade);
            metabolicRateWatts = metabolicRateWatts * 1.15;

            const totalEffortKcal = (metabolicRateWatts * 0.8604) * hours;
            let adjustments = (totalEffortKcal * 0.10); 

            if (temp < 5) adjustments += bmrBase * (Math.abs(temp - 5) * 0.01); 
            else if (temp > 30) adjustments += totalEffortKcal * ((temp - 30) * 0.03);

            const bmrDuringHike = (bmrBase / 24) * hours;
            const finalTotal = bmrBase + Math.max(0, totalEffortKcal - bmrDuringHike) + adjustments;

            targetCalories = finalTotal;
            document.getElementById('totalCalories').innerText = Math.round(finalTotal).toLocaleString();
            updateFoodAnalysis();
        }

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

            const percentage = Math.min(100, (totals.kcal / targetCalories) * 100);
            const progressBar = document.getElementById('energyProgress');
            progressBar.style.width = percentage + "%";
            progressBar.className = percentage < 85 ? "progress-bar bg-amber-500 h-full" : (percentage > 115 ? "progress-bar bg-rose-500 h-full" : "progress-bar bg-emerald-500 h-full");

            document.getElementById('carriedCalories').innerText = Math.round(totals.kcal).toLocaleString();
            document.getElementById('footerTotalWeight').innerText = Math.round(totals.weight) + " g";
            document.getElementById('totalWeightDash').innerHTML = `${Math.round(totals.weight)}<span class="text-xs ml-0.5 opacity-40">g</span>`;
            
            const diff = totals.kcal - targetCalories;
            const diffEl = document.getElementById('footerDiff');
            diffEl.innerText = (diff > 0 ? "+" : "") + Math.round(diff);
            diffEl.className = diff < -250 ? "text-lg font-black text-amber-600" : (diff > 250 ? "text-lg font-black text-rose-600" : "text-lg font-black text-emerald-600");

            // Dashboard
            document.getElementById('totalCarbs').innerHTML = `${Math.round(totals.carbs)}<span class="text-xs ml-0.5 opacity-40">g</span>`;
            document.getElementById('totalFats').innerHTML = `${Math.round(totals.fats)}<span class="text-xs ml-0.5 opacity-40">g</span>`;
            document.getElementById('totalProts').innerHTML = `${Math.round(totals.proteins)}<span class="text-xs ml-0.5 opacity-40">g</span>`;
            document.getElementById('totalSodium').innerHTML = `${Math.round(totals.sodium)}<span class="text-xs ml-0.5 opacity-40">mg</span>`;
            document.getElementById('totalFiber').innerHTML = `${Math.round(totals.fiber)}<span class="text-xs ml-0.5 opacity-40">g</span>`;

            const status = document.getElementById('headerStatus');
            if (totals.kcal === 0) status.innerText = "Inventaire vide";
            else if (percentage < 90) status.innerText = "Déficit Énergétique";
            else if (percentage > 110) status.innerText = "Surplus (Sac lourd)";
            else status.innerText = "Équilibre Parfait";
        }

        function renderFoodTable() {
            const body = document.getElementById('foodTableBody');
            body.innerHTML = "";

            foodItems.forEach((item, index) => {
                const tr = document.createElement('tr');
                tr.className = item.checked ? "bg-white group" : "bg-slate-50 opacity-50 group";
                tr.innerHTML = `
                    <td class="py-4 px-4 text-center">
                        <input type="checkbox" ${item.checked ? 'checked' : ''} onchange="toggleItem(${index})" class="w-6 h-6 accent-blue-600 cursor-pointer rounded">
                    </td>
                    <td class="py-4 px-2">
                        <input type="text" value="${item.name}" onchange="updateItem(${index}, 'name', this.value)" class="table-input !text-left font-bold text-slate-700">
                    </td>
                    <td class="py-4 px-2">
                        <input type="number" value="${item.weight}" onchange="updateItem(${index}, 'weight', this.value)" class="table-input text-slate-500">
                    </td>
                    <td class="py-4 px-2 bg-emerald-50/30">
                        <input type="number" value="${item.kcal}" onchange="updateItem(${index}, 'kcal', this.value)" class="table-input !text-emerald-700 font-black">
                    </td>
                    <td class="py-4 px-2">
                        <input type="number" value="${item.carbs}" onchange="updateItem(${index}, 'carbs', this.value)" class="table-input !text-amber-700">
                    </td>
                    <td class="py-4 px-2">
                        <input type="number" value="${item.fats}" onchange="updateItem(${index}, 'fats', this.value)" class="table-input !text-rose-700">
                    </td>
                    <td class="py-4 px-2">
                        <input type="number" value="${item.proteins}" onchange="updateItem(${index}, 'proteins', this.value)" class="table-input !text-indigo-700">
                    </td>
                    <td class="py-4 px-2">
                        <input type="number" value="${item.sodium}" onchange="updateItem(${index}, 'sodium', this.value)" class="table-input text-slate-500">
                    </td>
                    <td class="py-4 px-2">
                        <input type="number" value="${item.fiber}" onchange="updateItem(${index}, 'fiber', this.value)" class="table-input !text-emerald-700">
                    </td>
                    <td class="py-4 px-4 text-center">
                        <button onclick="removeFoodRow(${index})" class="text-slate-200 hover:text-rose-500 transition-colors opacity-0 group-hover:opacity-100">
                            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"></path></svg>
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
