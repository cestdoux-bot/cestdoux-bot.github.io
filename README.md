<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculateur Rando Expert - Itinérance & Nutrition</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        :root {
            --primary: #2d5a27;
            --secondary: #f4f7f1;
        }
        body {
            background-color: var(--secondary);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        .card {
            background: white;
            border-radius: 1rem;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
        }
        .tab-active {
            border-bottom: 3px solid var(--primary);
            color: var(--primary);
            font-weight: bold;
        }
        .nav-tab-active {
            background-color: var(--primary) !important;
            color: white !important;
        }
        input[type="number"], input[type="text"], select {
            border: 1px solid #e2e8f0;
            border-radius: 0.5rem;
            padding: 0.4rem;
            width: 100%;
            font-size: 0.875rem;
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
        .item-selected {
            background-color: #f0fdf4;
            border-color: #bbf7d0;
        }
    </style>
</head>
<body class="p-4 md:p-8">

    <div class="max-w-6xl mx-auto">
        <header class="mb-6 flex flex-col md:flex-row justify-between items-center bg-white p-6 rounded-2xl shadow-sm border border-gray-100">
            <div class="text-left">
                <h1 class="text-2xl font-bold text-green-900 leading-tight">Calculateur de Randonnée Expert</h1>
                <div class="mt-2 flex items-center gap-3">
                    <label class="text-gray-600 text-sm font-medium">Durée du trek :</label>
                    <input type="number" id="total-days" onchange="updateDayCount()" value="5" min="1" max="14" class="w-16 text-center h-8">
                    <span class="text-gray-400 text-xs">jours</span>
                </div>
            </div>

            <!-- BOUTONS DE SAUVEGARDE / IMPORT -->
            <div class="flex gap-2 mt-4 md:mt-0">
                <button onclick="exportData()" class="flex items-center gap-2 px-3 py-2 bg-gray-100 hover:bg-gray-200 text-gray-700 rounded-lg transition-all text-xs font-bold uppercase tracking-wider">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" x2="12" y1="15" y2="3"/></svg>
                    Exporter JSON
                </button>
                <button onclick="document.getElementById('importFile').click()" class="flex items-center gap-2 px-3 py-2 bg-green-800 hover:bg-green-900 text-white rounded-lg transition-all text-xs font-bold uppercase tracking-wider">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="17 8 12 3 7 8"/><line x1="12" x2="12" y1="3" y2="15"/></svg>
                    Importer JSON
                </button>
                <input type="file" id="importFile" accept=".json" class="hidden" onchange="importData(event)">
            </div>
        </header>

        <!-- NAVIGATION PRINCIPALE -->
        <nav class="flex justify-center gap-2 mb-8">
            <button onclick="switchMainView('p0')" id="nav-p0" class="px-6 py-2 rounded-full bg-white border font-bold transition-all shadow-sm">Cédrik</button>
            <button onclick="switchMainView('p1')" id="nav-p1" class="px-6 py-2 rounded-full bg-white border font-bold transition-all shadow-sm">P-A</button>
            <button onclick="switchMainView('summary')" id="nav-summary" class="px-6 py-2 rounded-full bg-white border font-bold transition-all shadow-sm flex items-center gap-2">
                📊 Synthèse
            </button>
        </nav>

        <!-- VUE INDIVIDUELLE (PROFIL + ALIMENTS) -->
        <div id="individual-view" class="space-y-6">
            <!-- Paramètres Physiques -->
            <section class="card p-6">
                <h2 class="text-xl font-semibold mb-4 flex items-center gap-2">👤 Paramètres Physiques</h2>
                <div id="profile-form" class="grid grid-cols-2 md:grid-cols-6 gap-3 bg-gray-50 p-4 rounded-lg">
                    <div>
                        <label class="block text-[10px] font-bold text-gray-500 uppercase">Nom</label>
                        <input type="text" id="p-name" oninput="updateProfileData()">
                    </div>
                    <div>
                        <label class="block text-[10px] font-bold text-gray-500 uppercase">Sexe</label>
                        <select id="p-gender" onchange="updateProfileData()">
                            <option value="M">Homme</option>
                            <option value="F">Femme</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-[10px] font-bold text-gray-500 uppercase">Âge</label>
                        <input type="number" id="p-age" oninput="updateProfileData()">
                    </div>
                    <div>
                        <label class="block text-[10px] font-bold text-gray-500 uppercase">Taille (cm)</label>
                        <input type="number" id="p-height" oninput="updateProfileData()">
                    </div>
                    <div>
                        <label class="block text-[10px] font-bold text-gray-500 uppercase">Poids (kg)</label>
                        <input type="number" id="p-weight" oninput="updateProfileData()">
                    </div>
                    <div>
                        <label class="block text-[10px] font-bold text-gray-500 uppercase">Niveau (1-10)</label>
                        <input type="number" id="p-level" oninput="updateProfileData()" min="1" max="10">
                    </div>
                </div>
            </section>

            <!-- Étape & Sac -->
            <section class="card p-6">
                <div id="day-tabs" class="flex overflow-x-auto gap-2 mb-6 pb-2 border-b"></div>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                    <div class="space-y-4">
                        <h3 class="font-bold text-green-800 border-b pb-1">L'étape du jour</h3>
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-medium">Distance (km)</label>
                                <input type="number" id="d-dist" oninput="saveCurrentDay(); calculate()">
                            </div>
                            <div>
                                <label class="block text-xs font-medium">Temps de marche (h)</label>
                                <input type="number" id="d-time" oninput="saveCurrentDay(); calculate()" step="0.5">
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs text-green-700 font-medium">Dénivelé + (m)</label>
                                <input type="number" id="d-elev-pos" oninput="saveCurrentDay(); calculate()">
                            </div>
                            <div>
                                <label class="block text-xs text-red-700 font-medium">Dénivelé - (m)</label>
                                <input type="number" id="d-elev-neg" oninput="saveCurrentDay(); calculate()">
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <div class="col-span-2">
                                <label class="block text-xs text-blue-700 font-medium">Terrain</label>
                                <select id="d-terrain" onchange="saveCurrentDay(); calculate()">
                                    <option value="1.0">Route (1.0)</option>
                                    <option value="1.2">Sentier (1.2)</option>
                                    <option value="1.5">Brousse (1.5)</option>
                                    <option value="2.1">Sable (2.1)</option>
                                </select>
                            </div>
                        </div>
                    </div>
                    <div class="bg-green-50 p-4 rounded-lg">
                        <h3 class="font-bold text-green-800 border-b pb-1">Charge du sac</h3>
                        <div class="mt-4">
                            <label class="block text-sm" id="label-load-current">Poids du sac (hors nourriture)</label>
                            <input type="number" id="d-load-current" oninput="saveCurrentDay(); calculate()">
                        </div>
                    </div>
                </div>
            </section>

            <!-- Garde-manger -->
            <section class="card p-6 border-l-4 border-green-700 overflow-x-auto">
                <div class="flex justify-between items-center mb-4">
                    <h3 class="text-lg font-bold text-green-900">🍎 Garde-manger personnel</h3>
                    <button onclick="addFoodItem(currentProfileIdx)" class="bg-green-700 text-white px-3 py-1 rounded text-sm hover:bg-green-800">+ Ajouter un aliment</button>
                </div>
                <div class="hidden md:grid nutri-grid text-[10px] font-bold text-gray-500 uppercase mb-2 px-1 text-center">
                    <div>Prévu</div><div>Qté</div><div class="text-left">Aliment</div><div>Kcal/u</div><div>P/u</div><div>G/u</div><div>L/u</div><div>Na/u</div><div>Fi/u</div><div></div>
                </div>
                <div id="food-list-current" class="space-y-2"></div>
            </section>

            <!-- Résumé Local (Affiché dans le profil) -->
            <div id="local-performance-view"></div>
        </div>

        <!-- VUE SYNTHÈSE -->
        <div id="summary-view" class="hidden space-y-6">
            <div id="summary-day-tabs" class="flex overflow-x-auto gap-2 mb-6 pb-2 border-b"></div>
            
            <!-- Section Résultats Performance -->
            <h3 class="text-xl font-bold text-gray-800 px-2">📊 Performance Comparative</h3>
            <div id="results-summary" class="grid grid-cols-1 md:grid-cols-2 gap-6"></div>

            <!-- Section Détail Nourriture Synthèse -->
            <h3 class="text-xl font-bold text-gray-800 px-2 mt-8">🍱 Détail des menus (Jour actuel)</h3>
            <div id="food-summary-grid" class="grid grid-cols-1 md:grid-cols-2 gap-6 pb-12"></div>
        </div>

    </div>

    <script>
        let currentProfileIdx = 0;
        let currentDayIdx = 0;
        let activeMainView = 'p0';

        let profiles = [
            { 
                name: "Cédrik", gender: "M", age: 30, height: 180, weight: 75, level: 7, 
                food: [{id: 'f1', label: "Lyophilisé Poulet", kcal: 650, prot: 35, gluc: 80, lip: 15, sod: 1200, fib: 8}] 
            },
            { 
                name: "P-A", gender: "M", age: 32, height: 175, weight: 70, level: 6, 
                food: [{id: 'f2', label: "Pâtes au Pesto", kcal: 580, prot: 18, gluc: 75, lip: 22, sod: 950, fib: 5}] 
            }
        ];

        let days = [];

        function init() {
            updateDayCount();
            switchMainView('p0');
        }

        function switchMainView(view) {
            activeMainView = view;
            const indView = document.getElementById('individual-view');
            const sumView = document.getElementById('summary-view');
            
            document.getElementById('nav-p0').className = `px-6 py-2 rounded-full border font-bold transition-all shadow-sm ${view === 'p0' ? 'nav-tab-active' : 'bg-white'}`;
            document.getElementById('nav-p1').className = `px-6 py-2 rounded-full border font-bold transition-all shadow-sm ${view === 'p1' ? 'nav-tab-active' : 'bg-white'}`;
            document.getElementById('nav-summary').className = `px-6 py-2 rounded-full border font-bold transition-all shadow-sm ${view === 'summary' ? 'nav-tab-active' : 'bg-white'}`;

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
                    dist: 15, time: 5, elevPos: 800, elevNeg: 800, terrain: 1.2, 
                    loads: [10, 8],
                    selectedFood: [{}, {}] 
                });
            }
            if (days.length > count) days = days.slice(0, count);
            renderDayTabs(activeMainView === 'summary' ? 'summary-day-tabs' : 'day-tabs');
        }

        function renderDayTabs(containerId) {
            const container = document.getElementById(containerId);
            if (!container) return;
            container.innerHTML = '';
            days.forEach((_, i) => {
                const btn = document.createElement('button');
                btn.innerText = `Jour ${i + 1}`;
                btn.className = `day-tab px-4 py-2 whitespace-nowrap ${currentDayIdx === i ? 'tab-active' : ''}`;
                btn.onclick = () => { 
                    saveCurrentDay(); 
                    currentDayIdx = i; 
                    renderDayTabs(containerId); 
                    calculate();
                    if (activeMainView !== 'summary') { loadDayData(); renderFoodLists(); }
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
            document.getElementById('label-load-current').innerText = `Poids du sac de ${p.name} (kg)`;
            document.getElementById('nav-p0').innerText = profiles[0].name;
            document.getElementById('nav-p1').innerText = profiles[1].name;
        }

        function loadDayData() {
            const d = days[currentDayIdx];
            document.getElementById('d-dist').value = d.dist;
            document.getElementById('d-time').value = d.time;
            document.getElementById('d-elev-pos').value = d.elevPos;
            document.getElementById('d-elev-neg').value = d.elevNeg;
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
        }

        function updateProfileData() {
            const p = profiles[currentProfileIdx];
            p.name = document.getElementById('p-name').value;
            p.gender = document.getElementById('p-gender').value;
            p.age = parseInt(document.getElementById('p-age').value) || 0;
            p.height = parseInt(document.getElementById('p-height').value) || 0;
            p.weight = parseFloat(document.getElementById('p-weight').value) || 0;
            p.level = parseInt(document.getElementById('p-level').value) || 5;
            loadProfileUI();
            calculate();
        }

        function addFoodItem(pIdx) {
            const newId = 'f-' + Math.random().toString(36).substr(2, 9);
            profiles[pIdx].food.push({ id: newId, label: "Nouvel aliment", kcal: 0, prot: 0, gluc: 0, lip: 0, sod: 0, fib: 0 });
            renderFoodLists();
            calculate();
        }

        function updateFoodItem(pIdx, fIdx, field, value) {
            profiles[pIdx].food[fIdx][field] = (field === 'label') ? value : (parseFloat(value) || 0);
            calculate();
        }

        function toggleFoodSelection(pIdx, fId) {
            const selections = days[currentDayIdx].selectedFood[pIdx];
            if (selections[fId] > 0) delete selections[fId];
            else selections[fId] = 1;
            renderFoodLists();
            calculate();
        }

        function updateFoodQuantity(pIdx, fId, qty) {
            const selections = days[currentDayIdx].selectedFood[pIdx];
            const val = parseInt(qty);
            if (val > 0) selections[fId] = val;
            else delete selections[fId];
            renderFoodLists();
            calculate();
        }

        function renderFoodLists() {
            const list = document.getElementById('food-list-current');
            list.innerHTML = '';
            const pIdx = currentProfileIdx;
            const currentDay = days[currentDayIdx];
            
            profiles[pIdx].food.forEach((item, fIdx) => {
                const qty = currentDay.selectedFood[pIdx][item.id] || 0;
                const isSelected = qty > 0;
                const div = document.createElement('div');
                div.className = `nutri-grid bg-white p-1 rounded border border-gray-100 transition-colors ${isSelected ? 'item-selected' : ''}`;
                div.innerHTML = `
                    <div class="flex justify-center"><input type="checkbox" ${isSelected ? 'checked' : ''} onchange="toggleFoodSelection(${pIdx}, '${item.id}')" class="w-5 h-5 accent-green-700 cursor-pointer"></div>
                    <div><input type="number" value="${qty}" min="0" oninput="updateFoodQuantity(${pIdx}, '${item.id}', this.value)" class="text-center bg-transparent"></div>
                    <input type="text" value="${item.label}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'label', this.value)">
                    <input type="number" value="${item.kcal}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'kcal', this.value)">
                    <input type="number" value="${item.prot}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'prot', this.value)">
                    <input type="number" value="${item.gluc}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'gluc', this.value)">
                    <input type="number" value="${item.lip}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'lip', this.value)">
                    <input type="number" value="${item.sod}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'sod', this.value)">
                    <input type="number" value="${item.fib}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'fib', this.value)">
                    <button onclick="profiles[${pIdx}].food.splice(${fIdx},1); renderFoodLists(); calculate();" class="text-red-400 font-bold hover:text-red-600">×</button>
                `;
                list.appendChild(div);
            });
        }

        // --- GESTION DE LA SAUVEGARDE ---
        function exportData() {
            saveCurrentDay();
            const fullState = {
                profiles: profiles,
                days: days,
                totalDays: document.getElementById('total-days').value,
                timestamp: new Date().toISOString()
            };
            const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(fullState));
            const downloadAnchorNode = document.createElement('a');
            downloadAnchorNode.setAttribute("href", dataStr);
            downloadAnchorNode.setAttribute("download", "rando_expert_session.json");
            document.body.appendChild(downloadAnchorNode);
            downloadAnchorNode.click();
            downloadAnchorNode.remove();
        }

        function importData(event) {
            const file = event.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = function(e) {
                try {
                    const imported = JSON.parse(e.target.result);
                    if (imported.profiles && imported.days) {
                        profiles = imported.profiles;
                        days = imported.days;
                        document.getElementById('total-days').value = imported.totalDays;
                        currentDayIdx = 0;
                        switchMainView('p0');
                    }
                } catch (err) {
                    console.error("Erreur d'importation : ", err);
                }
            };
            reader.readAsText(file);
        }

        /**
         * CALCULS MÉTABOLIQUES AVANCÉS
         */
        function calculate() {
            const d = days[currentDayIdx];
            const resultsSummaryDiv = document.getElementById('results-summary');
            const foodSummaryGrid = document.getElementById('food-summary-grid');
            const localPerformanceDiv = document.getElementById('local-performance-view');
            
            if (resultsSummaryDiv) resultsSummaryDiv.innerHTML = '';
            if (foodSummaryGrid) foodSummaryGrid.innerHTML = '';
            if (localPerformanceDiv) localPerformanceDiv.innerHTML = '';

            profiles.forEach((p, i) => {
                const W = p.weight || 1;
                const L = d.loads[i] || 0;
                
                // 1. Formule de Mifflin-St Jeor pour le BMR (plus précis que Harris-Benedict)
                let bmrDay = (10 * W) + (6.25 * p.height) - (5 * p.age);
                bmrDay += (p.gender === "M") ? 5 : -161;
                
                // Données de l'étape
                const timeSec = (d.time || 1) * 3600;
                const distM = (d.dist || 0.001) * 1000;
                const V = distM / timeSec; // Vitesse en m/s
                
                // 2. Calcul des pentes séparées (Positives et Négatives)
                const gradePos = (d.elevPos / distM) * 100;
                const gradeNeg = (d.elevNeg / distM) * 100;

                // 3. Équation de Santee découplée (Poids total W+L pour l'effort de charge)
                const loadTerm = 2.0 * (W + L) * Math.pow(L / W, 2);
                
                // Effort de base sur plat + impact du terrain
                const basePower = 1.5 * W + loadTerm;
                
                // Effort de montée (travail contre la gravité)
                const uphillPower = 0.35 * V * gradePos;
                
                // Effort de descente (travail excentrique / freinage)
                const downhillPower = V * (gradeNeg / 3) * 0.3;

                // Puissance métabolique totale (Watts)
                // Formule de Santee : Base + Terrain * (Vitesse^2 + Pentes)
                let metabolicPowerWatts = basePower + d.terrain * (W + L) * (1.5 * Math.pow(V, 2) + uphillPower + downhillPower);
                
                // Conversion Watts -> Kcal/h
                const kcalPerHourMovement = metabolicPowerWatts * 0.860;
                
                // Total journalier : BMR prorata temps repos + Effort prorata temps marche
                const totalNeeded = ((bmrDay / 24) * Math.max(0, 24 - d.time)) + (kcalPerHourMovement * d.time);

                // Analyse de la nourriture
                const daySelections = d.selectedFood[i];
                let selectedItemsList = [];
                const totals = p.food.reduce((acc, cur) => {
                    const qty = daySelections[cur.id] || 0;
                    if (qty > 0) {
                        acc.kcal += cur.kcal * qty; acc.prot += cur.prot * qty; acc.gluc += cur.gluc * qty; 
                        acc.lip += cur.lip * qty; acc.sod += cur.sod * qty; acc.fib += cur.fib * qty;
                        selectedItemsList.push({ label: cur.label, qty: qty, kcalTotal: Math.round(cur.kcal * qty) });
                    }
                    return acc;
                }, { kcal: 0, prot: 0, gluc: 0, lip: 0, sod: 0, fib: 0 });

                const balance = totals.kcal - totalNeeded;
                const intensity = Math.min(100, ((metabolicPowerWatts/W) / (p.level * 1.2)) * 100);

                const resultHTML = `
                    <div class="card p-5 border-t-4 ${i === 0 ? 'border-green-600' : 'border-blue-500'}">
                        <div class="flex justify-between items-center mb-4">
                            <h4 class="font-bold text-lg">${p.name}</h4>
                            <span class="text-xs font-bold text-gray-400 uppercase">Résultats J${currentDayIdx+1}</span>
                        </div>
                        <div class="space-y-3">
                            <div class="grid grid-cols-2 gap-2 text-center mb-4">
                                <div class="bg-gray-50 rounded p-2">
                                    <span class="block text-[9px] uppercase font-bold text-gray-500">Dépense (Mifflin)</span>
                                    <span class="text-lg font-bold">${Math.round(totalNeeded)}</span> <span class="text-xs">kcal</span>
                                </div>
                                <div class="bg-gray-50 rounded p-2">
                                    <span class="block text-[9px] uppercase font-bold text-gray-500">Apports (prévus)</span>
                                    <span class="text-lg font-bold text-blue-600">${Math.round(totals.kcal)}</span> <span class="text-xs">kcal</span>
                                </div>
                            </div>
                            
                            <div class="flex justify-between items-center px-3 py-2 border rounded ${balance >= 0 ? 'bg-green-50 border-green-200' : 'bg-red-50 border-red-200'}">
                                <span class="text-sm font-medium">Balance</span>
                                <span class="font-bold ${balance >= 0 ? 'text-green-700' : 'text-red-600'}">${Math.round(balance)} kcal</span>
                            </div>

                            <div class="grid grid-cols-3 gap-1 text-[10px] text-center pt-2">
                                <div class="bg-white border rounded py-1"><strong>P:</strong> ${Math.round(totals.prot)}g</div>
                                <div class="bg-white border rounded py-1"><strong>G:</strong> ${Math.round(totals.gluc)}g</div>
                                <div class="bg-white border rounded py-1"><strong>L:</strong> ${Math.round(totals.lip)}g</div>
                                <div class="bg-white border rounded py-1"><strong>Na:</strong> ${Math.round(totals.sod)}mg</div>
                                <div class="bg-white border rounded py-1"><strong>Fi:</strong> ${Math.round(totals.fib)}g</div>
                                <div class="bg-white border rounded py-1"><strong>Sac:</strong> ${L}kg</div>
                            </div>

                            <div class="mt-4">
                                <div class="flex justify-between text-[10px] mb-1 font-bold">
                                    <span>INTENSITÉ PHYSIOLOGIQUE</span>
                                    <span>${Math.round(intensity)}%</span>
                                </div>
                                <div class="w-full bg-gray-100 rounded-full h-2 overflow-hidden">
                                    <div class="h-2 ${intensity > 80 ? 'bg-red-500' : intensity > 60 ? 'bg-orange-400' : 'bg-green-500'}" style="width: ${intensity}%"></div>
                                </div>
                            </div>
                        </div>
                    </div>
                `;

                let foodListHTML = `
                    <div class="card p-5 border-l-4 ${i === 0 ? 'border-green-600' : 'border-blue-500'}">
                        <h4 class="font-bold mb-3 border-b pb-1">Menu de ${p.name}</h4>
                        ${selectedItemsList.length > 0 ? `
                            <table class="w-full text-sm">
                                <thead class="text-left text-[10px] text-gray-500 uppercase">
                                    <tr><th class="pb-2">Aliment</th><th class="pb-2 text-center">Qté</th><th class="pb-2 text-right">Kcal</th></tr>
                                </thead>
                                <tbody>
                                    ${selectedItemsList.map(item => `
                                        <tr class="border-t border-gray-50">
                                            <td class="py-2">${item.label}</td>
                                            <td class="py-2 text-center">${item.qty}</td>
                                            <td class="py-2 text-right font-medium">${item.kcalTotal}</td>
                                        </tr>
                                    `).join('')}
                                </tbody>
                            </table>
                        ` : '<p class="text-gray-400 italic text-sm text-center py-4">Aucun aliment sélectionné</p>'}
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

        window.onload = init;
    </script>
</body>
</html>
