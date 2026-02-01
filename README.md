<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculateur Rando Expert - Précision Métabolique</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        :root {
            --primary: #1b4332;
            --secondary: #f8fafc;
            --accent: #2d6a4f;
        }
        body {
            background-color: var(--secondary);
            font-family: 'Inter', system-ui, -apple-system, sans-serif;
            color: #1e293b;
        }
        .card {
            background: white;
            border-radius: 1rem;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05), 0 2px 4px -1px rgba(0, 0, 0, 0.03);
            border: 1px solid #e2e8f0;
        }
        .input-field {
            border: 1px solid #cbd5e1;
            border-radius: 0.5rem;
            padding: 0.5rem;
            width: 100%;
            font-size: 0.875rem;
            transition: border-color 0.2s;
        }
        .input-field:focus {
            outline: none;
            border-color: var(--accent);
            ring: 2px solid var(--accent);
        }
        .tab-active {
            border-bottom: 3px solid var(--primary);
            color: var(--primary);
            font-weight: 700;
        }
        .nav-tab-active {
            background-color: var(--primary) !important;
            color: white !important;
        }
        .nutri-grid {
            display: grid;
            grid-template-columns: 2.5rem 3.5rem 2fr 1fr 1fr 1fr 1fr 1fr 1fr auto;
            gap: 0.5rem;
            align-items: center;
        }
        @media (max-width: 768px) {
            .nutri-grid {
                grid-template-columns: auto auto 1fr 1fr;
            }
        }
        .status-pill {
            padding: 0.25rem 0.75rem;
            border-radius: 9999px;
            font-size: 0.75rem;
            font-weight: 600;
        }
    </style>
</head>
<body class="p-4 md:p-6 lg:p-8">

    <div class="max-w-7xl mx-auto">
        <header class="mb-8 text-center">
            <h1 class="text-4xl font-extrabold text-slate-900 tracking-tight">Calculateur de Trek Expert</h1>
            <p class="text-slate-500 mt-2 italic">Modélisation biomécanique (Pandolf & Santee) & Nutrition</p>
            
            <div class="mt-6 flex flex-wrap items-center justify-center gap-6 bg-white p-4 rounded-xl shadow-sm border border-slate-100">
                <div class="flex items-center gap-3">
                    <label class="text-slate-700 font-semibold">Durée du trek :</label>
                    <input type="number" id="total-days" onchange="updateDayCount()" value="5" min="1" max="21" class="w-16 text-center input-field">
                    <span class="text-slate-400 text-sm font-medium">jours</span>
                </div>
                <div class="h-6 w-px bg-slate-200 hidden md:block"></div>
                <button onclick="exportData()" class="text-sm font-medium text-slate-600 hover:text-slate-900 flex items-center gap-1">
                    💾 Sauvegarder (Session)
                </button>
            </div>
        </header>

        <!-- NAVIGATION PRINCIPALE -->
        <nav class="flex flex-wrap justify-center gap-3 mb-8">
            <button onclick="switchMainView('p0')" id="nav-p0" class="px-8 py-2.5 rounded-full bg-white border font-bold transition-all shadow-sm hover:border-emerald-600">Cédrik</button>
            <button onclick="switchMainView('p1')" id="nav-p1" class="px-8 py-2.5 rounded-full bg-white border font-bold transition-all shadow-sm hover:border-emerald-600">P-A</button>
            <button onclick="switchMainView('summary')" id="nav-summary" class="px-8 py-2.5 rounded-full bg-white border font-bold transition-all shadow-sm flex items-center gap-2 hover:bg-slate-50">
                📊 Synthèse Expédition
            </button>
        </nav>

        <!-- VUE INDIVIDUELLE -->
        <div id="individual-view" class="space-y-6">
            <!-- Paramètres Physiques -->
            <section class="card p-6">
                <h2 class="text-xl font-bold mb-5 flex items-center gap-2 text-slate-800">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"></path></svg>
                    Profil Biométrique
                </h2>
                <div id="profile-form" class="grid grid-cols-2 md:grid-cols-6 gap-4 bg-slate-50 p-5 rounded-xl border border-slate-100">
                    <div>
                        <label class="block text-[10px] font-bold text-slate-500 uppercase tracking-wider mb-1">Nom</label>
                        <input type="text" id="p-name" class="input-field" oninput="updateProfileData()">
                    </div>
                    <div>
                        <label class="block text-[10px] font-bold text-slate-500 uppercase tracking-wider mb-1">Sexe</label>
                        <select id="p-gender" class="input-field" onchange="updateProfileData()">
                            <option value="M">Homme</option>
                            <option value="F">Femme</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-[10px] font-bold text-slate-500 uppercase tracking-wider mb-1">Âge</label>
                        <input type="number" id="p-age" class="input-field" oninput="updateProfileData()">
                    </div>
                    <div>
                        <label class="block text-[10px] font-bold text-slate-500 uppercase tracking-wider mb-1">Taille (cm)</label>
                        <input type="number" id="p-height" class="input-field" oninput="updateProfileData()">
                    </div>
                    <div>
                        <label class="block text-[10px] font-bold text-slate-500 uppercase tracking-wider mb-1">Poids (kg)</label>
                        <input type="number" id="p-weight" class="input-field" oninput="updateProfileData()">
                    </div>
                    <div>
                        <label class="block text-[10px] font-bold text-slate-500 uppercase tracking-wider mb-1">VMA (est. km/h)</label>
                        <input type="number" id="p-level" class="input-field" oninput="updateProfileData()" min="5" max="22" step="0.5">
                    </div>
                </div>
            </section>

            <!-- Étape & Effort -->
            <section class="card p-6">
                <div id="day-tabs" class="flex overflow-x-auto gap-1 mb-6 pb-2 border-b"></div>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-10">
                    <div class="space-y-6">
                        <div class="flex items-center justify-between border-b border-slate-100 pb-2">
                            <h3 class="font-bold text-emerald-900 uppercase text-sm tracking-widest">Données de l'étape</h3>
                            <span id="current-day-label" class="text-xs font-bold bg-emerald-100 text-emerald-800 px-2 py-0.5 rounded">JOUR 1</span>
                        </div>
                        
                        <div class="grid grid-cols-2 gap-6">
                            <div>
                                <label class="block text-xs font-semibold text-slate-600 mb-1">Distance (km)</label>
                                <input type="number" id="d-dist" class="input-field font-bold" oninput="saveCurrentDay(); calculate()">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-slate-600 mb-1">Temps de marche (h)</label>
                                <input type="number" id="d-time" class="input-field font-bold" oninput="saveCurrentDay(); calculate()" step="0.25">
                            </div>
                        </div>
                        
                        <div class="grid grid-cols-2 gap-6">
                            <div>
                                <label class="block text-xs font-semibold text-slate-600 mb-1 text-emerald-700">Dénivelé Positif (m)</label>
                                <input type="number" id="d-elev-pos" class="input-field border-emerald-200" oninput="saveCurrentDay(); calculate()">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-slate-600 mb-1 text-red-700">Dénivelé Négatif (m)</label>
                                <input type="number" id="d-elev-neg" class="input-field border-red-200" oninput="saveCurrentDay(); calculate()">
                            </div>
                        </div>

                        <div>
                            <label class="block text-xs font-semibold text-slate-600 mb-1">Coefficient de Surface ($\eta$)</label>
                            <select id="d-terrain" class="input-field" onchange="saveCurrentDay(); calculate()">
                                <option value="1.0">Bitume / Route ($\eta = 1.0$)</option>
                                <option value="1.1">Piste forestière ($\eta = 1.1$)</option>
                                <option value="1.2">Sentier terre ($\eta = 1.2$)</option>
                                <option value="1.5">Herbe haute / Eboulis ($\eta = 1.5$)</option>
                                <option value="1.8">Marécage / Neige ($\eta = 1.8$)</option>
                                <option value="2.1">Sable mou ($\eta = 2.1$)</option>
                            </select>
                        </div>
                    </div>

                    <div class="bg-emerald-50 p-6 rounded-2xl border border-emerald-100">
                        <h3 class="font-bold text-emerald-900 border-b border-emerald-200 pb-2 mb-4 uppercase text-sm">Logistique de charge</h3>
                        <div class="space-y-4">
                            <div>
                                <label class="block text-sm font-medium text-emerald-800" id="label-load-current">Poids du sac de base (kg)</label>
                                <p class="text-[10px] text-emerald-600 mb-2 italic">Excluant la nourriture du jour sélectionné ci-dessous</p>
                                <input type="number" id="d-load-current" class="input-field border-emerald-300 shadow-inner" oninput="saveCurrentDay(); calculate()">
                            </div>
                            <div id="dynamic-load-info" class="p-3 bg-white rounded-lg border border-emerald-200 text-xs">
                                <!-- Info poids total dynamique -->
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <!-- Garde-manger -->
            <section class="card p-6 border-l-8 border-emerald-800">
                <div class="flex flex-wrap justify-between items-center gap-4 mb-6">
                    <div>
                        <h3 class="text-xl font-bold text-slate-900">🍏 Planification Nutritionnelle</h3>
                        <p class="text-xs text-slate-500">Sélectionnez les quantités pour le jour en cours.</p>
                    </div>
                    <button onclick="addFoodItem(currentProfileIdx)" class="bg-emerald-800 text-white px-4 py-2 rounded-lg text-sm font-bold shadow-md hover:bg-emerald-900 transition-colors">+ Ajouter un aliment</button>
                </div>
                
                <div class="overflow-x-auto">
                    <div class="hidden md:grid nutri-grid text-[10px] font-black text-slate-400 uppercase mb-3 px-2 text-center">
                        <div>Sél.</div><div>Qté</div><div class="text-left">Désignation Aliment</div><div>Kcal/u</div><div>P(g)</div><div>G(g)</div><div>L(g)</div><div>Na(mg)</div><div>Fi(g)</div><div></div>
                    </div>
                    <div id="food-list-current" class="space-y-2"></div>
                </div>
            </section>

            <!-- Résultat Local Direct -->
            <div id="local-performance-view"></div>
        </div>

        <!-- VUE SYNTHÈSE -->
        <div id="summary-view" class="hidden space-y-8">
            <div id="summary-day-tabs" class="flex overflow-x-auto gap-1 mb-6 pb-2 border-b"></div>
            
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                <div class="space-y-6">
                    <h3 class="text-2xl font-black text-slate-800 px-2 flex items-center gap-3">
                        <span class="w-2 h-8 bg-emerald-600 rounded-full"></span>
                        Analyse Comparative
                    </h3>
                    <div id="results-summary" class="grid grid-cols-1 gap-6"></div>
                </div>
                
                <div class="space-y-6">
                    <h3 class="text-2xl font-black text-slate-800 px-2 flex items-center gap-3">
                        <span class="w-2 h-8 bg-blue-600 rounded-full"></span>
                        Logistique Nutritionnelle
                    </h3>
                    <div id="food-summary-grid" class="grid grid-cols-1 gap-6"></div>
                </div>
            </div>
        </div>
    </div>

    <script>
        /**
         * INITIALISATION ET VARIABLES GLOBALES
         */
        let currentProfileIdx = 0;
        let currentDayIdx = 0;
        let activeMainView = 'p0';

        // Profils par défaut avec stockage initial
        let profiles = JSON.parse(localStorage.getItem('trek_profiles')) || [
            { 
                name: "Cédrik", gender: "M", age: 30, height: 180, weight: 78, level: 12, 
                food: [
                    {id: 'f1', label: "Lyophilisé - Petit Déj", kcal: 450, prot: 15, gluc: 65, lip: 12, sod: 450, fib: 6},
                    {id: 'f2', label: "Barre Énergie", kcal: 220, prot: 5, gluc: 35, lip: 8, sod: 110, fib: 2},
                    {id: 'f3', label: "Lyophilisé - Plat Soir", kcal: 680, prot: 32, gluc: 85, lip: 18, sod: 1100, fib: 8}
                ] 
            },
            { 
                name: "P-A", gender: "M", age: 32, height: 175, weight: 72, level: 11, 
                food: [
                    {id: 'f4', label: "Avoine & Noix", kcal: 500, prot: 18, gluc: 70, lip: 15, sod: 300, fib: 10},
                    {id: 'f5', label: "Mix Trail (100g)", kcal: 450, prot: 12, gluc: 40, lip: 30, sod: 50, fib: 6},
                    {id: 'f6', label: "Pâtes Expert", kcal: 620, prot: 22, gluc: 90, lip: 14, sod: 950, fib: 5}
                ] 
            }
        ];

        let days = JSON.parse(localStorage.getItem('trek_days')) || [];

        function init() {
            if (days.length === 0) updateDayCount();
            switchMainView('p0');
        }

        function switchMainView(view) {
            activeMainView = view;
            const indView = document.getElementById('individual-view');
            const sumView = document.getElementById('summary-view');
            
            // UI Update
            ['nav-p0', 'nav-p1', 'nav-summary'].forEach(id => {
                const el = document.getElementById(id);
                el.classList.remove('nav-tab-active');
            });
            document.getElementById(`nav-${view}`).classList.add('nav-tab-active');

            if (view === 'summary') {
                indView.classList.add('hidden');
                sumView.classList.remove('hidden');
                renderDayTabs('summary-day-tabs');
                calculate();
            } else {
                indView.classList.remove('hidden');
                sumView.classList.add('hidden');
                currentProfileIdx = view === 'p0' ? 0 : 1;
                loadProfileUI();
                renderDayTabs('day-tabs');
                loadDayData();
                renderFoodLists();
                calculate();
            }
        }

        function updateDayCount() {
            const count = parseInt(document.getElementById('total-days').value) || 1;
            while (days.length < count) {
                days.push({ 
                    dist: 18, time: 6, elevPos: 900, elevNeg: 900, terrain: 1.2, 
                    loads: [10, 10], // Poids sac de base par profil
                    selectedFood: [{}, {}] // {foodId: qty} pour chaque profil
                });
            }
            if (days.length > count) days = days.slice(0, count);
            renderDayTabs(activeMainView === 'summary' ? 'summary-day-tabs' : 'day-tabs');
            persist();
        }

        function renderDayTabs(containerId) {
            const container = document.getElementById(containerId);
            if (!container) return;
            container.innerHTML = '';
            days.forEach((_, i) => {
                const btn = document.createElement('button');
                btn.innerText = `JOUR ${i + 1}`;
                btn.className = `px-5 py-2 whitespace-nowrap text-xs font-black tracking-tighter transition-all ${currentDayIdx === i ? 'tab-active' : 'text-slate-400 hover:text-slate-600'}`;
                btn.onclick = () => { 
                    saveCurrentDay(); 
                    currentDayIdx = i; 
                    renderDayTabs(containerId); 
                    if (activeMainView !== 'summary') { 
                        loadDayData(); 
                        renderFoodLists(); 
                    }
                    document.getElementById('current-day-label').innerText = `JOUR ${i+1}`;
                    calculate();
                };
                container.appendChild(btn);
            });
        }

        function loadProfileUI() {
            const p = profiles[currentProfileIdx];
            document.getElementById('p-name').value = p.name;
            document.getElementById('p-gender').value = p.gender;
            document.getElementById('p-age').value = p.age;
            document.getElementById('p-height').value = p.height;
            document.getElementById('p-weight').value = p.weight;
            document.getElementById('p-level').value = p.level;
            document.getElementById('label-load-current').innerText = `Sac de base de ${p.name} (kg)`;
            document.getElementById('nav-p0').innerText = profiles[0].name;
            document.getElementById('nav-p1').innerText = profiles[1].name;
        }

        function loadDayData() {
            const d = days[currentDayIdx];
            document.getElementById('d-dist').value = d.dist;
            document.getElementById('d-time').value = d.time;
            document.getElementById('d-elev-pos').value = d.elevPos;
            document.getElementById('d-elev-neg').value = d.elevNeg || d.elevPos; // Fallback
            document.getElementById('d-terrain').value = d.terrain;
            document.getElementById('d-load-current').value = d.loads[currentProfileIdx];
        }

        function saveCurrentDay() {
            if (activeMainView === 'summary') return;
            const d = days[currentDayIdx];
            d.dist = parseFloat(document.getElementById('d-dist').value) || 0;
            d.time = parseFloat(document.getElementById('d-time').value) || 0;
            d.elevPos = parseFloat(document.getElementById('d-elev-pos').value) || 0;
            d.elevNeg = parseFloat(document.getElementById('d-elev-neg').value) || 0;
            d.terrain = parseFloat(document.getElementById('d-terrain').value) || 1;
            d.loads[currentProfileIdx] = parseFloat(document.getElementById('d-load-current').value) || 0;
            persist();
        }

        function updateProfileData() {
            const p = profiles[currentProfileIdx];
            p.name = document.getElementById('p-name').value;
            p.gender = document.getElementById('p-gender').value;
            p.age = parseInt(document.getElementById('p-age').value) || 0;
            p.height = parseInt(document.getElementById('p-height').value) || 0;
            p.weight = parseFloat(document.getElementById('p-weight').value) || 0;
            p.level = parseFloat(document.getElementById('p-level').value) || 10;
            loadProfileUI();
            persist();
            calculate();
        }

        function persist() {
            localStorage.setItem('trek_profiles', JSON.stringify(profiles));
            localStorage.setItem('trek_days', JSON.stringify(days));
        }

        /**
         * LOGIQUE NUTRITIONNELLE
         */
        function addFoodItem(pIdx) {
            const newId = 'f-' + Math.random().toString(36).substr(2, 9);
            profiles[pIdx].food.push({ id: newId, label: "Nouvel aliment", kcal: 0, prot: 0, gluc: 0, lip: 0, sod: 0, fib: 0 });
            renderFoodLists();
            persist();
            calculate();
        }

        function updateFoodItem(pIdx, fIdx, field, value) {
            profiles[pIdx].food[fIdx][field] = (field === 'label') ? value : (parseFloat(value) || 0);
            persist();
            calculate();
        }

        function toggleFoodSelection(pIdx, fId) {
            const selections = days[currentDayIdx].selectedFood[pIdx];
            if (selections[fId] > 0) delete selections[fId];
            else selections[fId] = 1;
            renderFoodLists();
            persist();
            calculate();
        }

        function updateFoodQuantity(pIdx, fId, qty) {
            const selections = days[currentDayIdx].selectedFood[pIdx];
            const val = parseInt(qty);
            if (val > 0) selections[fId] = val;
            else delete selections[fId];
            renderFoodLists();
            persist();
            calculate();
        }

        function renderFoodLists() {
            const list = document.getElementById('food-list-current');
            if (!list) return;
            list.innerHTML = '';
            const pIdx = currentProfileIdx;
            const currentDay = days[currentDayIdx];
            
            profiles[pIdx].food.forEach((item, fIdx) => {
                const qty = currentDay.selectedFood[pIdx][item.id] || 0;
                const isSelected = qty > 0;
                const div = document.createElement('div');
                div.className = `nutri-grid bg-white p-2 rounded-xl border transition-all ${isSelected ? 'border-emerald-600 bg-emerald-50/50 shadow-sm' : 'border-slate-100'}`;
                div.innerHTML = `
                    <div class="flex justify-center"><input type="checkbox" ${isSelected ? 'checked' : ''} onchange="toggleFoodSelection(${pIdx}, '${item.id}')" class="w-5 h-5 accent-emerald-700 cursor-pointer"></div>
                    <div><input type="number" value="${qty}" min="0" oninput="updateFoodQuantity(${pIdx}, '${item.id}', this.value)" class="w-full text-center bg-transparent font-bold"></div>
                    <input type="text" value="${item.label}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'label', this.value)" class="bg-transparent text-sm border-none focus:ring-0">
                    <input type="number" value="${item.kcal}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'kcal', this.value)" class="bg-transparent text-center text-xs">
                    <input type="number" value="${item.prot}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'prot', this.value)" class="bg-transparent text-center text-xs">
                    <input type="number" value="${item.gluc}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'gluc', this.value)" class="bg-transparent text-center text-xs">
                    <input type="number" value="${item.lip}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'lip', this.value)" class="bg-transparent text-center text-xs">
                    <input type="number" value="${item.sod}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'sod', this.value)" class="bg-transparent text-center text-xs">
                    <input type="number" value="${item.fib}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'fib', this.value)" class="bg-transparent text-center text-xs">
                    <button onclick="profiles[${pIdx}].food.splice(${fIdx},1); renderFoodLists(); calculate();" class="text-slate-300 hover:text-red-500 transition-colors">
                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"></path></svg>
                    </button>
                `;
                list.appendChild(div);
            });
        }

        /**
         * MOTEUR DE CALCUL MÉTABOLIQUE (PANDOLF + SANTEE)
         */
        function calculate() {
            const d = days[currentDayIdx];
            const resultsSummaryDiv = document.getElementById('results-summary');
            const foodSummaryGrid = document.getElementById('food-summary-grid');
            const localPerformanceDiv = document.getElementById('local-performance-view');
            const dynamicLoadInfo = document.getElementById('dynamic-load-info');
            
            if (resultsSummaryDiv) resultsSummaryDiv.innerHTML = '';
            if (foodSummaryGrid) foodSummaryGrid.innerHTML = '';
            if (localPerformanceDiv) localPerformanceDiv.innerHTML = '';

            profiles.forEach((p, i) => {
                // 1. Calcul Apports
                const daySelections = d.selectedFood[i];
                let foodWeightG = 0;
                const totals = p.food.reduce((acc, cur) => {
                    const qty = daySelections[cur.id] || 0;
                    if (qty > 0) {
                        acc.kcal += cur.kcal * qty; acc.prot += cur.prot * qty; acc.gluc += cur.gluc * qty; 
                        acc.lip += cur.lip * qty; acc.sod += cur.sod * qty; acc.fib += cur.fib * qty;
                        foodWeightG += (cur.kcal / 4) * qty; // Estimation grossière poids si non saisi, idéalement à ajouter
                    }
                    return acc;
                }, { kcal: 0, prot: 0, gluc: 0, lip: 0, sod: 0, fib: 0 });

                // 2. Calcul Dépense (Biomécanique)
                const W = p.weight;
                const L_base = d.loads[i];
                const L_total = L_base + (foodWeightG / 1000); // Poids sac + nourriture jour
                const V_ms = (d.dist * 1000) / (d.time * 3600 || 1); // Vitesse m/s
                const grade = ((d.elevPos - d.elevNeg) / (d.dist * 1000 || 1)) * 100; // Pente moyenne %
                const eta = d.terrain;

                // Formule de Pandolf modifiée Santee
                // M = 1.5W + 2.0(W+L)(L/W)^2 + eta(W+L)[1.5V^2 + 0.35V*G]
                let metabolicPowerWatts = 0;
                const G = grade;
                const term1 = 1.5 * W;
                const term2 = 2.0 * (W + L_total) * Math.pow(L_total / W, 2);
                
                // Santee correction pour descente
                let gradeFactor = 0;
                if (G >= 0) {
                    gradeFactor = 0.35 * V_ms * G;
                } else {
                    // Descente : M = M_plat * (1 - |G|/L) - correction de Santee
                    // On simplifie ici par une réduction de l'effort jusqu'à -15% puis remontée (freinage excentrique)
                    const absG = Math.abs(G);
                    gradeFactor = absG * 0.1 * V_ms; // Réduction d'effort simplifiée
                    if (absG > 15) gradeFactor = absG * 0.2 * V_ms; // Reprise d'effort pour freinage
                }

                metabolicPowerWatts = term1 + term2 + eta * (W + L_total) * (1.5 * Math.pow(V_ms, 2) + gradeFactor);
                
                // BMR (Harris-Benedict)
                const bmrDay = (p.gender === "M") 
                    ? 88.362 + (13.397 * W) + (4.799 * p.height) - (5.677 * p.age)
                    : 447.593 + (9.247 * W) + (3.098 * p.height) - (4.330 * p.age);
                
                const kcalMoving = (metabolicPowerWatts * 0.860) * d.time; // 1 Watt = 0.86 kcal/h
                const totalNeeded = ((bmrDay / 24) * (24 - d.time)) + kcalMoving;

                // 3. Analyse
                const balance = totals.kcal - totalNeeded;
                const intensity = Math.min(100, ( (metabolicPowerWatts / W) / (p.level * 0.4) ) * 100);

                if (i === currentProfileIdx && dynamicLoadInfo) {
                    dynamicLoadInfo.innerHTML = `<strong>Charge estimée :</strong> ${L_total.toFixed(2)} kg (dont nourriture)`;
                }

                const resultHTML = `
                    <div class="card p-6 border-t-8 ${i === 0 ? 'border-emerald-600' : 'border-blue-600'}">
                        <div class="flex justify-between items-start mb-6">
                            <div>
                                <h4 class="font-black text-2xl text-slate-800">${p.name}</h4>
                                <p class="text-[10px] text-slate-400 font-bold uppercase tracking-widest">Analyse J${currentDayIdx+1}</p>
                            </div>
                            <div class="text-right">
                                <span class="status-pill ${balance >= 0 ? 'bg-emerald-100 text-emerald-800' : 'bg-red-100 text-red-800'}">
                                    ${balance >= 0 ? 'Excédent' : 'Déficit'} ${Math.abs(Math.round(balance))} kcal
                                </span>
                            </div>
                        </div>

                        <div class="grid grid-cols-2 gap-4 mb-6">
                            <div class="bg-slate-50 p-4 rounded-2xl text-center border border-slate-100">
                                <span class="block text-[10px] uppercase font-black text-slate-400 mb-1">Dépense Totale</span>
                                <span class="text-2xl font-black text-slate-800">${Math.round(totalNeeded)}</span>
                                <span class="text-xs font-bold text-slate-400">kcal</span>
                            </div>
                            <div class="bg-slate-50 p-4 rounded-2xl text-center border border-slate-100">
                                <span class="block text-[10px] uppercase font-black text-slate-400 mb-1">Apports Prévus</span>
                                <span class="text-2xl font-black text-emerald-700">${Math.round(totals.kcal)}</span>
                                <span class="text-xs font-bold text-slate-400">kcal</span>
                            </div>
                        </div>

                        <div class="space-y-4">
                            <div>
                                <div class="flex justify-between text-[10px] font-black mb-1.5 text-slate-500 uppercase">
                                    <span>Fatigue Physiologique</span>
                                    <span class="${intensity > 85 ? 'text-red-600' : 'text-slate-700'}">${Math.round(intensity)}%</span>
                                </div>
                                <div class="w-full bg-slate-100 rounded-full h-3 overflow-hidden shadow-inner">
                                    <div class="h-3 rounded-full transition-all duration-500 ${intensity > 85 ? 'bg-red-500' : intensity > 65 ? 'bg-amber-400' : 'bg-emerald-500'}" style="width: ${intensity}%"></div>
                                </div>
                            </div>

                            <div class="grid grid-cols-3 gap-2">
                                <div class="bg-white border border-slate-100 rounded-lg p-2 text-center">
                                    <span class="block text-[8px] font-bold text-slate-400 uppercase">Protéines</span>
                                    <span class="text-sm font-bold text-slate-700">${Math.round(totals.prot)}g</span>
                                </div>
                                <div class="bg-white border border-slate-100 rounded-lg p-2 text-center">
                                    <span class="block text-[8px] font-bold text-slate-400 uppercase">Glucides</span>
                                    <span class="text-sm font-bold text-slate-700">${Math.round(totals.gluc)}g</span>
                                </div>
                                <div class="bg-white border border-slate-100 rounded-lg p-2 text-center">
                                    <span class="block text-[8px] font-bold text-slate-400 uppercase">Lipides</span>
                                    <span class="text-sm font-bold text-slate-700">${Math.round(totals.lip)}g</span>
                                </div>
                            </div>
                        </div>
                    </div>
                `;

                // Vue Synthèse
                const foodListHTML = `
                    <div class="card p-6 border-l-8 ${i === 0 ? 'border-emerald-600' : 'border-blue-600'}">
                        <h4 class="font-black text-slate-800 mb-4 flex items-center justify-between">
                            Menu de ${p.name}
                            <span class="text-xs text-slate-400 uppercase">J${currentDayIdx+1}</span>
                        </h4>
                        <div class="space-y-2">
                            ${p.food.filter(f => daySelections[f.id]).map(f => `
                                <div class="flex justify-between items-center text-sm p-2 bg-slate-50 rounded-lg">
                                    <span class="font-medium text-slate-700">${f.label}</span>
                                    <div class="flex gap-4 items-center">
                                        <span class="text-xs font-bold text-slate-400">x${daySelections[f.id]}</span>
                                        <span class="font-bold text-slate-900 w-16 text-right">${f.kcal * daySelections[f.id]} <small>kcal</small></span>
                                    </div>
                                </div>
                            `).join('') || '<p class="text-center text-slate-400 italic py-4">Aucun aliment prévu</p>'}
                        </div>
                        <div class="mt-4 pt-4 border-t border-dashed border-slate-200 flex justify-between font-black text-lg">
                            <span class="text-slate-500">Total</span>
                            <span class="text-emerald-700">${Math.round(totals.kcal)} kcal</span>
                        </div>
                    </div>
                `;

                if (activeMainView === 'summary') {
                    resultsSummaryDiv.innerHTML += resultHTML;
                    foodSummaryGrid.innerHTML += foodListHTML;
                } else if (i === currentProfileIdx) {
                    localPerformanceDiv.innerHTML = resultHTML;
                }
            });
        }

        function exportData() {
            persist();
            const blob = new Blob([JSON.stringify({profiles, days}, null, 2)], {type: 'application/json'});
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `trek_plan_${new Date().toISOString().split('T')[0]}.json`;
            a.click();
        }

        window.onload = init;
    </script>
</body>
</html>
