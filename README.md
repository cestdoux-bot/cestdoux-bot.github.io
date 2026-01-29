<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Analyseur Nutritionnel Rando Pro</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .card { background: white; border-radius: 1rem; shadow-sm border border-slate-200; }
        input[type="range"] { accent-color: #059669; }
        .progress-bar { transition: width 0.5s cubic-bezier(0.4, 0, 0.2, 1), background-color 0.3s; }
        .table-input { @apply w-full bg-transparent border-b border-transparent hover:border-slate-200 focus:border-emerald-500 outline-none transition-colors text-center font-medium py-1; }
        /* Scrollbar subtile pour le tableau mobile */
        .custom-scrollbar::-webkit-scrollbar { height: 4px; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
    </style>
</head>
<body class="bg-slate-50 min-h-screen pb-12 text-slate-800">

    <header class="bg-emerald-900 text-white py-10 px-4 mb-8 shadow-2xl relative overflow-hidden">
        <div class="absolute inset-0 opacity-10">
            <svg viewBox="0 0 100 100" preserveAspectRatio="none" class="h-full w-full"><path d="M0 100 L30 40 L50 70 L80 20 L100 100 Z" fill="white"></path></svg>
        </div>
        <div class="max-w-6xl mx-auto text-center relative z-10">
            <h1 class="text-4xl font-black mb-2 tracking-tight">Analyseur Nutritionnel Rando</h1>
            <p class="text-emerald-200 text-sm uppercase tracking-[0.2em] font-bold">Physiologie & Logistique de l'effort</p>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 space-y-8">
        
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
            <div class="lg:col-span-2 space-y-6">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <section class="card p-6 border-t-4 border-emerald-600">
                        <h2 class="text-sm font-black mb-4 flex items-center gap-2 text-emerald-800 uppercase tracking-widest">
                            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" stroke-width="2.5"></path></svg>
                            Le Randonneur
                        </h2>
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label class="block text-[10px] font-black text-slate-400 uppercase">Poids de corps (kg)</label>
                                <input type="number" id="weight" value="70" class="mt-1 block w-full rounded-lg border-slate-200 p-2 bg-slate-50 border font-bold focus:ring-2 focus:ring-emerald-500 outline-none">
                            </div>
                            <div>
                                <label class="block text-[10px] font-black text-slate-400 uppercase">Poids du sac (kg)</label>
                                <input type="number" id="packWeight" value="12" class="mt-1 block w-full rounded-lg border-emerald-100 p-2 bg-emerald-50 border font-bold text-emerald-700">
                            </div>
                            <div class="col-span-2 flex gap-3">
                                <div class="w-1/2">
                                    <label class="block text-[10px] font-black text-slate-400 uppercase">Sexe</label>
                                    <select id="gender" class="mt-1 block w-full rounded-lg border-slate-200 p-2 bg-slate-50 border font-bold">
                                        <option value="male">Homme</option>
                                        <option value="female">Femme</option>
                                    </select>
                                </div>
                                <div class="w-1/2">
                                    <label class="block text-[10px] font-black text-slate-400 uppercase">Âge</label>
                                    <input type="number" id="age" value="30" class="mt-1 block w-full rounded-lg border-slate-200 p-2 bg-slate-50 border font-bold">
                                </div>
                            </div>
                        </div>
                    </section>

                    <section class="card p-6 border-t-4 border-emerald-600">
                        <h2 class="text-sm font-black mb-4 flex items-center gap-2 text-emerald-800 uppercase tracking-widest">
                            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6" stroke-width="2.5"></path></svg>
                            L'Étape
                        </h2>
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label class="block text-[10px] font-black text-slate-400 uppercase">Distance (km)</label>
                                <input type="number" id="distance" value="15" class="mt-1 block w-full rounded-lg border-slate-200 p-2 bg-slate-50 border font-bold">
                            </div>
                            <div>
                                <label class="block text-[10px] font-black text-slate-400 uppercase">D+ Cumulé (m)</label>
                                <input type="number" id="elevation" value="800" class="mt-1 block w-full rounded-lg border-slate-200 p-2 bg-slate-50 border font-bold">
                            </div>
                            <div class="col-span-2">
                                <label class="block text-[10px] font-black text-slate-400 uppercase">Nature du Terrain</label>
                                <select id="terrain" class="mt-1 block w-full rounded-lg border-slate-200 p-2 bg-slate-50 border font-bold">
                                    <option value="1.0">Route / Bitume (Facile)</option>
                                    <option value="1.2" selected>Sentier Terre / Forêt (Standard)</option>
                                    <option value="1.5">Herbe haute / Sentier meuble</option>
                                    <option value="1.8">Éboulis / Sable / Hors-piste</option>
                                    <option value="2.5">Neige profonde (Difficile)</option>
                                </select>
                            </div>
                        </div>
                    </section>
                </div>

                <section class="card p-6 border-l-8 border-emerald-600">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-8 items-center">
                        <div>
                            <label class="block text-[10px] font-black text-slate-400 uppercase mb-1">Temps de marche effectif (h)</label>
                            <input type="number" id="hours" value="5" step="0.5" class="mt-1 block w-full rounded-xl border-slate-200 p-3 bg-white border font-black text-2xl text-emerald-800 focus:ring-2 focus:ring-emerald-500">
                            <div class="flex justify-between mt-3 text-[10px] font-bold text-slate-400 uppercase italic">
                                <span>Vitesse : <span id="speedDisplay" class="text-emerald-600">0</span> km/h</span>
                                <span>Pente moy. : <span id="gradeDisplay" class="text-emerald-600">0</span> %</span>
                            </div>
                        </div>
                        <div>
                            <label class="block text-[10px] font-black text-slate-400 uppercase italic mb-3 text-center md:text-left">Météo : <span class="text-emerald-700 font-black text-sm" id="tempVal">18°C</span></label>
                            <input type="range" id="temp" min="-15" max="40" value="18" class="w-full h-2 bg-slate-200 rounded-lg cursor-pointer">
                            <div class="flex justify-between text-[9px] font-black text-slate-300 mt-2 uppercase tracking-tighter">
                                <span>Grand Froid (-15°)</span>
                                <span>Tempéré</span>
                                <span>Canicule (40°)</span>
                            </div>
                        </div>
                    </div>
                </section>
            </div>

            <div class="lg:col-span-1">
                <div class="card p-6 sticky top-8 border-b-8 border-emerald-800 shadow-xl bg-white/80 backdrop-blur">
                    <h2 class="text-lg font-black mb-6 text-slate-800 text-center uppercase tracking-tighter">Bilan Énergétique 24h</h2>
                    
                    <div class="space-y-4">
                        <div class="bg-slate-900 p-6 rounded-3xl text-center shadow-lg transform transition-transform hover:scale-[1.02]">
                            <p class="text-[10px] text-emerald-400 font-black uppercase tracking-widest mb-1">Cible Calorique Estimée</p>
                            <p class="text-5xl font-black text-white" id="totalCalories">0</p>
                            <p class="text-xs text-slate-400 font-bold mt-1 uppercase">Kilocalories</p>
                        </div>

                        <div class="p-5 bg-white rounded-2xl border border-slate-100 shadow-inner space-y-4">
                            <div class="flex justify-between items-end">
                                <p class="text-[10px] font-black text-slate-400 uppercase text-left">Apport actuel<br><span class="text-xs normal-case font-bold text-slate-300">Dans le sac à dos</span></p>
                                <p class="text-2xl font-black text-slate-800"><span id="carriedCalories">0</span> <span class="text-xs text-slate-400">kcal</span></p>
                            </div>
                            <div class="w-full bg-slate-100 rounded-full h-5 overflow-hidden p-1 shadow-inner">
                                <div id="energyProgress" class="progress-bar rounded-full h-full w-0 bg-emerald-500"></div>
                            </div>
                            <div id="energyStatus" class="text-center py-1 rounded-lg text-[10px] font-black uppercase tracking-widest bg-slate-50 text-slate-500">Calcul en cours...</div>
                        </div>

                        <div class="grid grid-cols-2 gap-3 text-center">
                            <div class="bg-blue-50/50 p-3 rounded-2xl border border-blue-100">
                                <p class="text-[9px] font-black text-blue-400 uppercase mb-1">Poids Vivres</p>
                                <p id="footerTotalWeight" class="text-xl font-black text-blue-900">0g</p>
                            </div>
                            <div class="bg-slate-50 p-3 rounded-2xl border border-slate-200">
                                <p class="text-[9px] font-black text-slate-400 uppercase mb-1">Balance</p>
                                <p id="footerDiff" class="text-xl font-black">0</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <section class="card p-6 border-l-8 border-blue-600">
            <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-8 gap-4">
                <div>
                    <h2 class="text-2xl font-black flex items-center gap-3 text-slate-900">
                        <span class="bg-blue-600 text-white p-2 rounded-lg"><svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10" stroke-width="2"></path></svg></span>
                        Inventaire de la Ration
                    </h2>
                    <p class="text-slate-400 text-xs font-bold uppercase tracking-widest mt-1">Personnalisez vos apports pour l'étape</p>
                </div>
                <button onclick="addFoodRow()" class="bg-blue-600 hover:bg-blue-700 text-white text-xs font-black py-3 px-6 rounded-xl uppercase tracking-widest transition-all shadow-lg active:scale-95 flex items-center gap-2">
                    <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M12 4v16m8-8H4" stroke-width="3"></path></svg>
                    Ajouter
                </button>
            </div>

            <div class="overflow-x-auto custom-scrollbar">
                <table class="w-full text-left border-collapse min-w-[900px]">
                    <thead>
                        <tr class="text-[10px] font-black text-slate-400 uppercase tracking-widest border-b-2 border-slate-100">
                            <th class="py-4 px-2 text-center w-12">Sac</th>
                            <th class="py-4 px-2 w-1/4">Nom de l'aliment</th>
                            <th class="py-4 px-2 text-center">Masse (g)</th>
                            <th class="py-4 px-2 text-center bg-emerald-50 text-emerald-700">Kcal</th>
                            <th class="py-4 px-2 text-center">Carbs (g)</th>
                            <th class="py-4 px-2 text-center">Fats (g)</th>
                            <th class="py-4 px-2 text-center">Prot (g)</th>
                            <th class="py-4 px-2 text-center">Sodium (mg)</th>
                            <th class="py-4 px-1 text-center w-12"></th>
                        </tr>
                    </thead>
                    <tbody id="foodTableBody" class="divide-y divide-slate-100"></tbody>
                </table>
            </div>

            <div class="mt-8 grid grid-cols-2 md:grid-cols-4 lg:grid-cols-6 gap-4">
                <div class="p-4 rounded-2xl bg-amber-50 border border-amber-100">
                    <p class="text-[10px] font-black text-amber-500 uppercase mb-1">Glucides</p>
                    <p id="totalCarbs" class="text-2xl font-black text-amber-900">0g</p>
                </div>
                <div class="p-4 rounded-2xl bg-rose-50 border border-rose-100">
                    <p class="text-[10px] font-black text-rose-500 uppercase mb-1">Lipides</p>
                    <p id="totalFats" class="text-2xl font-black text-rose-900">0g</p>
                </div>
                <div class="p-4 rounded-2xl bg-indigo-50 border border-indigo-100">
                    <p class="text-[10px] font-black text-indigo-500 uppercase mb-1">Protéines</p>
                    <p id="totalProts" class="text-2xl font-black text-indigo-900">0g</p>
                </div>
                <div class="p-4 rounded-2xl bg-slate-100 border border-slate-200">
                    <p class="text-[10px] font-black text-slate-500 uppercase mb-1">Sodium</p>
                    <p id="totalSodium" class="text-2xl font-black text-slate-900">0mg</p>
                </div>
                <div class="p-4 rounded-2xl bg-emerald-50 border border-emerald-100 lg:col-span-2">
                    <p class="text-[10px] font-black text-emerald-500 uppercase mb-1">Densité Énergétique</p>
                    <p id="energyDensity" class="text-2xl font-black text-emerald-900">0 <span class="text-xs">kcal/100g</span></p>
                </div>
            </div>
        </section>
    </main>

    <script>
        // --- ÉTAT ET PERSISTANCE ---
        let foodItems = JSON.parse(localStorage.getItem('hiking_macros')) || [
            { name: "Muesli au lait", kcal: 500, weight: 120, carbs: 75, fats: 12, proteins: 15, sodium: 150, checked: true },
            { name: "Mélange Noix de cajou", kcal: 580, weight: 100, carbs: 30, fats: 45, proteins: 18, sodium: 300, checked: true },
            { name: "Lyophilisé Pâtes Bolonaises", kcal: 650, weight: 150, carbs: 85, fats: 22, proteins: 25, sodium: 1100, checked: true }
        ];

        let targetCalories = 0;

        function saveData() {
            localStorage.setItem('hiking_macros', JSON.stringify(foodItems));
        }

        // --- CALCULS PHYSIOLOGIQUES AVANCÉS ---
        function calculate() {
            const getVal = (id) => parseFloat(document.getElementById(id).value) || 0;
            const gender = document.getElementById('gender').value;
            const weight = getVal('weight'); 
            const age = getVal('age');
            const distance = getVal('distance');
            const elevation = getVal('elevation');
            const pack = getVal('packWeight'); 
            const eta = getVal('terrain'); 
            const hours = getVal('hours') || 1; 
            const temp = getVal('temp');

            // 1. Métabolisme de base (Mifflin-St Jeor)
            let bmr = (10 * weight) + (6.25 * 175) - (5 * age);
            bmr = (gender === 'male') ? bmr + 5 : bmr - 161;

            // 2. Coût de l'effort (Vitesse et Pente)
            const velocityKMH = distance / hours;
            const velocityMS = velocityKMH / 3.6; 
            const grade = (elevation / (distance * 1000)) * 100; 

            // Formule de Pandolf modifiée (Watts)
            // Inclut le coût du sac et le terrain
            let watts = 1.5 * weight + 2.0 * (weight + pack) * Math.pow(pack / weight, 2) + 
                        eta * (weight + pack) * (1.5 * Math.pow(velocityMS, 2) + 0.35 * velocityMS * grade);
            
            // Si la pente est descendante (approximation 1/3 de l'effort de montée pour le freinage)
            // On considère ici que elevation est le D+ cumulé, donc on ajoute un coût forfaitaire pour le retour
            const energyKcalEffort = (watts * 0.8604) * hours; 

            // 3. Ajustements Thermiques
            let thermoAdjustment = 0;
            if (temp < 10) thermoAdjustment = bmr * (Math.abs(temp - 10) * 0.015); // Surcoût froid
            if (temp > 28) thermoAdjustment = energyKcalEffort * ((temp - 28) * 0.04); // Surcoût chaleur

            // 4. Total 24h : BMR + Effort (moins le bmr déjà compté pendant l'effort) + Afterburn (5%)
            const total = (bmr + Math.max(0, energyKcalEffort) + thermoAdjustment) * 1.05;

            targetCalories = total;
            
            // Update UI
            document.getElementById('totalCalories').innerText = Math.round(total).toLocaleString();
            document.getElementById('speedDisplay').innerText = velocityKMH.toFixed(1);
            document.getElementById('gradeDisplay').innerText = grade.toFixed(1);
            document.getElementById('tempVal').innerText = temp + "°C";

            updateFoodAnalysis();
        }

        // --- ANALYSE NUTRITIONNELLE ---
        function updateFoodAnalysis() {
            let totals = { kcal: 0, weight: 0, carbs: 0, fats: 0, proteins: 0, sodium: 0 };

            foodItems.forEach(item => {
                if (item.checked) {
                    totals.kcal += item.kcal;
                    totals.weight += item.weight;
                    totals.carbs += item.carbs;
                    totals.fats += item.fats;
                    totals.proteins += item.proteins;
                    totals.sodium += item.sodium;
                }
            });

            // Barre de progression intelligente
            const percentage = Math.min(100, (totals.kcal / targetCalories) * 100);
            const pb = document.getElementById('energyProgress');
            pb.style.width = percentage + "%";
            
            // Changement de couleur dynamique
            pb.classList.remove('bg-emerald-500', 'bg-amber-500', 'bg-rose-500');
            if (percentage < 80) pb.classList.add('bg-amber-500');
            else if (percentage > 110) pb.classList.add('bg-rose-500');
            else pb.classList.add('bg-emerald-500');

            const status = document.getElementById('energyStatus');
            status.className = "text-center py-1 rounded-lg text-[10px] font-black uppercase tracking-widest";
            if (percentage < 85) { status.innerText = "Déficit Énergétique"; status.classList.add('bg-amber-100', 'text-amber-700'); }
            else if (percentage > 110) { status.innerText = "Excédent : Sac trop lourd ?"; status.classList.add('bg-rose-100', 'text-rose-700'); }
            else { status.innerText = "Ration Optimale"; status.classList.add('bg-emerald-100', 'text-emerald-700'); }

            document.getElementById('carriedCalories').innerText = Math.round(totals.kcal).toLocaleString();
            document.getElementById('footerTotalWeight').innerText = Math.round(totals.weight) + "g";
            
            const diff = Math.round(totals.kcal - targetCalories);
            const diffEl = document.getElementById('footerDiff');
            diffEl.innerText = (diff > 0 ? "+" : "") + diff;
            diffEl.className = `text-xl font-black ${diff < -200 ? 'text-rose-600' : 'text-emerald-600'}`;

            // Macros
            document.getElementById('totalCarbs').innerText = Math.round(totals.carbs) + "g";
            document.getElementById('totalFats').innerText = Math.round(totals.fats) + "g";
            document.getElementById('totalProts').innerText = Math.round(totals.proteins) + "g";
            document.getElementById('totalSodium').innerText = Math.round(totals.sodium) + "mg";
            
            const density = totals.weight > 0 ? (totals.kcal / (totals.weight / 100)).toFixed(0) : 0;
            document.getElementById('energyDensity').innerText = density;

            saveData();
        }

        function renderFoodTable() {
            const body = document.getElementById('foodTableBody');
            body.innerHTML = "";
            foodItems.forEach((item, index) => {
                const tr = document.createElement('tr');
                tr.className = item.checked ? "bg-white hover:bg-slate-50 transition-colors" : "bg-slate-50/50 opacity-60";
                tr.innerHTML = `
                    <td class="py-4 px-2 text-center"><input type="checkbox" ${item.checked ? 'checked' : ''} onchange="toggleItem(${index})" class="w-6 h-6 accent-emerald-600 cursor-pointer"></td>
                    <td class="py-4 px-2"><input type="text" value="${item.name}" onchange="updateItem(${index}, 'name', this.value)" class="table-input !text-left font-bold text-slate-700"></td>
                    <td class="py-4 px-2"><input type="number" value="${item.weight}" onchange="updateItem(${index}, 'weight', this.value)" class="table-input"></td>
                    <td class="py-4 px-2 bg-emerald-50/30"><input type="number" value="${item.kcal}" onchange="updateItem(${index}, 'kcal', this.value)" class="table-input !text-emerald-700 font-black"></td>
                    <td class="py-4 px-2"><input type="number" value="${item.carbs}" onchange="updateItem(${index}, 'carbs', this.value)" class="table-input !text-amber-700"></td>
                    <td class="py-4 px-2"><input type="number" value="${item.fats}" onchange="updateItem(${index}, 'fats', this.value)" class="table-input !text-rose-700"></td>
                    <td class="py-4 px-2"><input type="number" value="${item.proteins}" onchange="updateItem(${index}, 'proteins', this.value)" class="table-input !text-indigo-700"></td>
                    <td class="py-4 px-2"><input type="number" value="${item.sodium}" onchange="updateItem(${index}, 'sodium', this.value)" class="table-input !text-slate-500"></td>
                    <td class="py-4 px-2">
                        <button onclick="removeFoodRow(${index})" class="text-slate-300 hover:text-rose-600 transition-colors">
                            <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20"><path d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z"></path></svg>
                        </button>
                    </td>
                `;
                body.appendChild(tr);
            });
            updateFoodAnalysis();
        }

        // --- HANDLERS ---
        function addFoodRow() {
            foodItems.push({ name: "Nouvel aliment", kcal: 0, weight: 0, carbs: 0, fats: 0, proteins: 0, sodium: 0, checked: true });
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
            foodItems[index][key] = key === 'name' ? value : (parseFloat(value) || 0);
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
