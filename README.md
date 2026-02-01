<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculateur Rando Expert - Premium Edition</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --forest: #1a2e1a;
            --adventure: #f97316;
            --mist: #f8fafc;
        }
        body {
            background-color: #f1f5f9;
            font-family: 'Inter', sans-serif;
            color: #334155;
        }
        
        /* CORRECTIF GITHUB PAGES : Forcer la largeur maximale */
        #main_content, .container-lg, .container { 
            max-width: none !important; 
            padding: 0 !important; 
        }

        .outdoor-card {
            background: white;
            border: 1px solid rgba(226, 232, 240, 0.8);
            border-radius: 1.25rem;
            box-shadow: 0 1px 3px rgba(0,0,0,0.02), 0 1px 2px rgba(0,0,0,0.03);
        }
        .input-field {
            background-color: #f8fafc;
            border: 2px solid transparent;
            border-radius: 0.75rem;
            padding: 0.6rem 1rem;
            transition: all 0.2s;
            font-size: 0.95rem;
        }
        .input-field:focus {
            outline: none;
            border-color: var(--adventure);
            background-color: white;
            box-shadow: 0 0 0 4px rgba(249, 115, 22, 0.1);
        }
        .tab-active {
            color: var(--adventure);
            border-bottom: 2px solid var(--adventure);
        }
        .nav-tab-active {
            background-color: var(--forest) !important;
            color: white !important;
        }
        .nutri-grid {
            display: grid;
            grid-template-columns: 2.5rem 4rem 2fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr auto;
            gap: 0.75rem;
            align-items: center;
        }
        @media (max-width: 1024px) {
            .nutri-grid { grid-template-columns: auto auto 1fr 1fr; }
        }
        .sticky-results {
            position: sticky;
            top: 2rem;
        }
        .custom-progress {
            height: 0.75rem;
            border-radius: 1rem;
            background: #e2e8f0;
            overflow: hidden;
            position: relative;
        }
    </style>
</head>
<body class="antialiased">

    <div class="max-w-[1600px] mx-auto px-4 py-6 md:py-10">
        <header class="mb-10 flex flex-col md:flex-row justify-between items-center bg-[#1a2e1a] p-8 rounded-[2rem] shadow-xl text-white">
            <div class="text-left mb-6 md:mb-0">
                <div class="flex items-center gap-3 mb-1">
                    <div class="bg-orange-500 p-2 rounded-lg">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" class="text-white"><path d="M17 11V7a5 5 0 0 0-10 0v4"/><path d="M5 11h14v7a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2v-7Z"/><circle cx="12" cy="15" r="1"/></svg>
                    </div>
                    <h1 class="text-2xl font-extrabold tracking-tight">Expedition Planner <span class="text-orange-400 font-light">Pro</span></h1>
                </div>
                <div class="flex items-center gap-4 ml-11">
                    <label class="text-green-200/70 text-sm font-semibold uppercase tracking-wider">Durée du trek</label>
                    <div class="flex items-center bg-white/10 rounded-lg p-1">
                        <input type="number" id="total-days" onchange="updateDayCount()" value="5" min="1" max="14" class="bg-transparent w-12 text-center font-bold text-white focus:outline-none">
                        <span class="text-xs text-green-300 pr-2 font-medium">jours</span>
                    </div>
                </div>
            </div>

            <div class="flex gap-3">
                <button onclick="exportData()" class="flex items-center gap-2 px-5 py-3 bg-white/10 hover:bg-white/20 text-white rounded-xl transition-all text-xs font-bold uppercase tracking-widest border border-white/10">
                    Exporter
                </button>
                <button onclick="document.getElementById('importFile').click()" class="flex items-center gap-2 px-5 py-3 bg-orange-500 hover:bg-orange-600 text-white rounded-xl transition-all text-xs font-bold uppercase tracking-widest shadow-lg shadow-orange-900/20">
                    Importer
                </button>
                <input type="file" id="importFile" accept=".json" class="hidden" onchange="importData(event)">
            </div>
        </header>

        <nav class="flex justify-center mb-10">
            <div class="bg-white p-1.5 rounded-2xl shadow-sm inline-flex gap-1 border border-slate-200">
                <button onclick="switchMainView('p0')" id="nav-p0" class="px-8 py-2.5 rounded-xl font-bold transition-all text-sm">Cédrik</button>
                <button onclick="switchMainView('p1')" id="nav-p1" class="px-8 py-2.5 rounded-xl font-bold transition-all text-sm">P-A</button>
                <div class="w-px h-6 bg-slate-200 self-center mx-1"></div>
                <button onclick="switchMainView('summary')" id="nav-summary" class="px-8 py-2.5 rounded-xl font-bold transition-all text-sm flex items-center gap-2">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 3v18h18"/><path d="m19 9-5 5-4-4-3 3"/></svg>
                    Synthèse
                </button>
            </div>
        </nav>

        <div id="individual-view" class="space-y-8">
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                
                <div class="lg:col-span-2 space-y-8">
                    <section class="outdoor-card p-8">
                        <div class="flex items-center gap-2 mb-6">
                            <h2 class="text-lg font-bold text-slate-800 uppercase tracking-tight">Paramètres du randonneur</h2>
                            <div class="h-px flex-1 bg-slate-100 ml-4"></div>
                        </div>
                        <div id="profile-form" class="grid grid-cols-2 md:grid-cols-3 gap-6">
                            <div class="space-y-1.5">
                                <label class="text-[11px] font-bold text-slate-400 uppercase ml-1">Nom d'explorateur</label>
                                <input type="text" id="p-name" oninput="updateProfileData()" class="input-field w-full">
                            </div>
                            <div class="space-y-1.5">
                                <label class="text-[11px] font-bold text-slate-400 uppercase ml-1">Sexe biologique</label>
                                <select id="p-gender" onchange="updateProfileData()" class="input-field w-full appearance-none">
                                    <option value="M">Homme</option>
                                    <option value="F">Femme</option>
                                </select>
                            </div>
                            <div class="space-y-1.5">
                                <label class="text-[11px] font-bold text-slate-400 uppercase ml-1">Âge</label>
                                <input type="number" id="p-age" oninput="updateProfileData()" class="input-field w-full">
                            </div>
                            <div class="space-y-1.5">
                                <label class="text-[11px] font-bold text-slate-400 uppercase ml-1">Taille (cm)</label>
                                <input type="number" id="p-height" oninput="updateProfileData()" class="input-field w-full">
                            </div>
                            <div class="space-y-1.5">
                                <label class="text-[11px] font-bold text-slate-400 uppercase ml-1">Poids corporel (kg)</label>
                                <input type="number" id="p-weight" oninput="updateProfileData()" class="input-field w-full">
                            </div>
                            <div class="space-y-1.5">
                                <label class="text-[11px] font-bold text-slate-400 uppercase ml-1">Niveau physique (1-10)</label>
                                <input type="number" id="p-level" oninput="updateProfileData()" min="1" max="10" class="input-field w-full">
                            </div>
                        </div>
                    </section>

                    <section class="outdoor-card p-8">
                        <div id="day-tabs" class="flex overflow-x-auto gap-6 mb-8 border-b border-slate-100"></div>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-10">
                            <div class="space-y-6">
                                <h3 class="text-sm font-extrabold text-[#1a2e1a] uppercase tracking-widest flex items-center gap-2">
                                    <span class="w-2 h-2 bg-orange-500 rounded-full"></span> Topographie de l'étape
                                </h3>
                                <div class="grid grid-cols-2 gap-4">
                                    <div class="space-y-1">
                                        <label class="text-[10px] font-bold text-slate-500 uppercase">Distance (km)</label>
                                        <input type="number" id="d-dist" oninput="saveCurrentDay(); calculate()" class="input-field w-full font-semibold">
                                    </div>
                                    <div class="space-y-1">
                                        <label class="text-[10px] font-bold text-slate-500 uppercase">Durée marche (h)</label>
                                        <input type="number" id="d-time" oninput="saveCurrentDay(); calculate()" step="0.5" class="input-field w-full font-semibold">
                                    </div>
                                    <div class="space-y-1">
                                        <label class="text-[10px] font-bold text-green-600 uppercase italic">Dénivelé + (m)</label>
                                        <input type="number" id="d-elev-pos" oninput="saveCurrentDay(); calculate()" class="input-field w-full text-green-700 font-bold">
                                    </div>
                                    <div class="space-y-1">
                                        <label class="text-[10px] font-bold text-red-600 uppercase italic">Dénivelé - (m)</label>
                                        <input type="number" id="d-elev-neg" oninput="saveCurrentDay(); calculate()" class="input-field w-full text-red-700 font-bold">
                                    </div>
                                </div>
                                <div class="space-y-1">
                                    <label class="text-[10px] font-bold text-slate-500 uppercase">Coefficient de terrain</label>
                                    <select id="d-terrain" onchange="saveCurrentDay(); calculate()" class="input-field w-full">
                                        <option value="1.0">Macadam / Route (1.0)</option>
                                        <option value="1.2">Sentier balisé (1.2)</option>
                                        <option value="1.5">Hors-piste / Brousse (1.5)</option>
                                        <option value="2.1">Sable mou / Neige (2.1)</option>
                                    </select>
                                </div>
                            </div>
                            <div class="bg-slate-50 rounded-2xl p-6 border border-slate-100 flex flex-col justify-center">
                                <h3 class="text-sm font-extrabold text-[#1a2e1a] uppercase tracking-widest mb-4">Logistique & Charge</h3>
                                <div class="space-y-4">
                                    <label class="block text-sm font-medium text-slate-600" id="label-load-current">Poids du sac de base (kg)</label>
                                    <input type="number" id="d-load-current" oninput="saveCurrentDay(); calculate()" class="input-field w-full bg-white shadow-inner text-lg font-bold">
                                    <div class="flex items-start gap-2 text-blue-600 bg-blue-50 p-3 rounded-lg border border-blue-100">
                                        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="mt-0.5"><circle cx="12" cy="12" r="10"/><line x1="12" x2="12" y1="16" y2="12"/><line x1="12" x2="12" y1="8" y2="8"/></svg>
                                        <p class="text-[11px] leading-snug">Le poids de la nourriture sélectionnée est ajouté dynamiquement au calcul.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </section>
                </div>

                <div class="lg:col-span-1">
                    <div class="sticky-results" id="local-performance-view">
                        </div>
                </div>
            </div>

            <section class="outdoor-card p-8 border-l-[6px] border-[#1a2e1a]">
                <div class="flex justify-between items-end mb-8">
                    <div>
                        <h3 class="text-xl font-black text-slate-800 uppercase tracking-tight">Ravitaillement Personnel</h3>
                        <p class="text-slate-400 text-sm">Gérez votre inventaire alimentaire et optimisez vos apports.</p>
                    </div>
                    <button onclick="addFoodItem(currentProfileIdx)" class="bg-[#1a2e1a] text-white px-6 py-3 rounded-xl text-xs font-bold uppercase tracking-widest hover:bg-black transition-all shadow-lg">
                        + Ajouter un aliment
                    </button>
                </div>
                
                <div class="overflow-x-auto">
                    <div class="min-w-[800px]">
                        <div class="nutri-grid text-[10px] font-black text-slate-400 uppercase mb-4 px-4">
                            <div class="text-center">Choix</div><div class="text-center">Qté</div><div>Aliment</div><div>Poids(g)</div><div>Kcal</div><div>P(g)</div><div>G(g)</div><div>L(g)</div><div>Na(mg)</div><div>Fib</div><div></div>
                        </div>
                        <div id="food-list-current" class="space-y-3"></div>
                    </div>
                </div>
            </section>
        </div>

        <div id="summary-view" class="hidden space-y-10">
            <div id="summary-day-tabs" class="flex overflow-x-auto gap-4 mb-8 border-b border-slate-200"></div>
            
            <div class="flex items-center gap-4 mb-2">
                <h3 class="text-2xl font-black text-slate-800">ANALYSE DE MISSION</h3>
                <div class="h-px flex-1 bg-slate-200"></div>
            </div>
            
            <div id="results-summary" class="grid grid-cols-1 md:grid-cols-2 gap-8"></div>

            <div class="flex items-center gap-4 mb-2 mt-12">
                <h3 class="text-2xl font-black text-slate-800">PLAN DE CHARGE NUTRITIONNEL</h3>
                <div class="h-px flex-1 bg-slate-200"></div>
            </div>
            <div id="food-summary-grid" class="grid grid-cols-1 md:grid-cols-2 gap-8"></div>

            <div class="flex items-center gap-4 mb-2 mt-12">
                <h3 class="text-2xl font-black text-slate-800 uppercase tracking-tight">Liste d'épicerie globale</h3>
                <div class="h-px flex-1 bg-slate-200"></div>
            </div>
            <div id="grocery-summary-grid" class="grid grid-cols-1 md:grid-cols-2 gap-8 pb-20"></div>
        </div>
    </div>

    <script>
        let currentProfileIdx = 0;
        let currentDayIdx = 0;
        let activeMainView = 'p0';

        let profiles = [
            { 
                name: "Cédrik", gender: "M", age: 30, height: 180, weight: 75, level: 7, 
                food: [{id: 'f1', label: "Lyophilisé Poulet", weight: 150, kcal: 650, prot: 35, gluc: 80, lip: 15, sod: 1200, fib: 8}] 
            },
            { 
                name: "P-A", gender: "M", age: 32, height: 175, weight: 70, level: 6, 
                food: [{id: 'f2', label: "Pâtes au Pesto", weight: 180, kcal: 580, prot: 18, gluc: 75, lip: 22, sod: 950, fib: 5}] 
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
            
            const btnBase = "px-8 py-2.5 rounded-xl font-bold transition-all text-sm";
            document.getElementById('nav-p0').className = `${btnBase} ${view === 'p0' ? 'bg-[#1a2e1a] text-white shadow-md' : 'text-slate-500 hover:bg-slate-50'}`;
            document.getElementById('nav-p1').className = `${btnBase} ${view === 'p1' ? 'bg-[#1a2e1a] text-white shadow-md' : 'text-slate-500 hover:bg-slate-50'}`;
            document.getElementById('nav-summary').className = `${btnBase} ${view === 'summary' ? 'bg-[#1a2e1a] text-white shadow-md' : 'text-slate-500 hover:bg-slate-50'}`;

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
                btn.innerText = `Étape ${i + 1}`;
                btn.className = `day-tab px-6 py-3 whitespace-nowrap text-sm font-bold uppercase tracking-tighter transition-all border-b-2 ${currentDayIdx === i ? 'tab-active' : 'text-slate-400 border-transparent hover:text-slate-600'}`;
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
            document.getElementById('label-load-current').innerText = `Poids du sac fixe de ${p.name} (kg)`;
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
            const defaultKcal = 400;
            const defaultWeight = Math.round(defaultKcal / 2.5);
            profiles[pIdx].food.push({ 
                id: newId, 
                label: "Nouvel aliment", 
                weight: defaultWeight,
                kcal: defaultKcal, 
                prot: 0, gluc: 0, lip: 0, sod: 0, fib: 0 
            });
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
                div.className = `nutri-grid bg-white p-3 rounded-xl border-2 transition-all ${isSelected ? 'border-orange-500/30 bg-orange-50/10' : 'border-slate-50'}`;
                div.innerHTML = `
                    <div class="flex justify-center">
                        <input type="checkbox" ${isSelected ? 'checked' : ''} onchange="toggleFoodSelection(${pIdx}, '${item.id}')" 
                               class="w-6 h-6 rounded-md accent-orange-500 cursor-pointer">
                    </div>
                    <div><input type="number" value="${qty}" min="0" oninput="updateFoodQuantity(${pIdx}, '${item.id}', this.value)" class="input-field text-center font-bold px-1"></div>
                    <div class="relative"><input type="text" value="${item.label}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'label', this.value)" class="input-field w-full font-medium"></div>
                    <div><input type="number" value="${item.weight}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'weight', this.value)" class="input-field w-full text-slate-500"></div>
                    <div><input type="number" value="${item.kcal}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'kcal', this.value)" class="input-field w-full text-slate-800 font-bold"></div>
                    <div><input type="number" value="${item.prot}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'prot', this.value)" class="input-field w-full text-slate-500"></div>
                    <div><input type="number" value="${item.gluc}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'gluc', this.value)" class="input-field w-full text-slate-500"></div>
                    <div><input type="number" value="${item.lip}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'lip', this.value)" class="input-field w-full text-slate-500"></div>
                    <div><input type="number" value="${item.sod}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'sod', this.value)" class="input-field w-full text-slate-500"></div>
                    <div><input type="number" value="${item.fib}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'fib', this.value)" class="input-field w-full text-slate-500"></div>
                    <button onclick="profiles[${pIdx}].food.splice(${fIdx},1); renderFoodLists(); calculate();" class="text-slate-300 hover:text-red-500 transition-colors p-2">
                        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M3 6h18"/><path d="M19 6v14c0 1-1 2-2 2H7c-1 0-2-1-2-2V6"/><path d="M8 6V4c0-1 1-2 2-2h4c1 0 2 1 2 2v2"/><line x1="10" x2="10" y1="11" y2="17"/><line x1="14" x2="14" y1="11" y2="17"/></svg>
                    </button>
                `;
                list.appendChild(div);
            });
        }

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
            downloadAnchorNode.setAttribute("download", "expedition_plan.json");
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
                } catch (err) { console.error(err); }
            };
            reader.readAsText(file);
        }

        function calculate() {
            const d = days[currentDayIdx];
            const resultsSummaryDiv = document.getElementById('results-summary');
            const foodSummaryGrid = document.getElementById('food-summary-grid');
            const localPerformanceDiv = document.getElementById('local-performance-view');
            const grocerySummaryGrid = document.getElementById('grocery-summary-grid');
            
            if (resultsSummaryDiv) resultsSummaryDiv.innerHTML = '';
            if (foodSummaryGrid) foodSummaryGrid.innerHTML = '';
            if (localPerformanceDiv) localPerformanceDiv.innerHTML = '';
            if (grocerySummaryGrid) grocerySummaryGrid.innerHTML = '';

            // Consolidation de l'épicerie pour toute la durée
            let profileGroceries = profiles.map(() => ({}));

            days.forEach((day) => {
                profiles.forEach((p, pIdx) => {
                    const daySelections = day.selectedFood[pIdx];
                    for (const foodId in daySelections) {
                        const qty = daySelections[foodId];
                        if (qty > 0) {
                            if (!profileGroceries[pIdx][foodId]) {
                                profileGroceries[pIdx][foodId] = 0;
                            }
                            profileGroceries[pIdx][foodId] += qty;
                        }
                    }
                });
            });

            profiles.forEach((p, i) => {
                const W = p.weight || 1;
                const fixedLoad = d.loads[i] || 0;
                const daySelections = d.selectedFood[i];
                let selectedItemsList = [];
                let foodWeightGrams = 0;
                
                const totals = p.food.reduce((acc, cur) => {
                    const qty = daySelections[cur.id] || 0;
                    if (qty > 0) {
                        acc.kcal += cur.kcal * qty; acc.prot += cur.prot * qty; acc.gluc += cur.gluc * qty; 
                        acc.lip += cur.lip * qty; acc.sod += cur.sod * qty; acc.fib += cur.fib * qty;
                        foodWeightGrams += (cur.weight || 0) * qty;
                        selectedItemsList.push({ label: cur.label, qty: qty, weightTotal: Math.round((cur.weight || 0) * qty), kcalTotal: Math.round(cur.kcal * qty) });
                    }
                    return acc;
                }, { kcal: 0, prot: 0, gluc: 0, lip: 0, sod: 0, fib: 0 });

                const L = fixedLoad + (foodWeightGrams / 1000);

                let bmrDay = (10 * W) + (6.25 * p.height) - (5 * p.age);
                bmrDay += (p.gender === "M") ? 5 : -161;
                
                const timeSec = (d.time || 1) * 3600;
                const distM = (d.dist || 0.001) * 1000;
                const V = distM / timeSec; 
                
                const gradePos = (d.elevPos / distM) * 100;
                const gradeNeg = (d.elevNeg / distM) * 100;

                const loadTerm = 2.0 * (W + L) * Math.pow(L / W, 2);
                const basePower = 1.5 * W + loadTerm;
                const uphillPower = 0.35 * V * gradePos;
                const downhillPower = V * (gradeNeg / 3) * 0.3;

                let metabolicPowerWatts = basePower + d.terrain * (W + L) * (1.5 * Math.pow(V, 2) + uphillPower + downhillPower);
                const kcalPerHourMovement = metabolicPowerWatts * 0.860;
                const totalNeeded = ((bmrDay / 24) * Math.max(0, 24 - d.time)) + (kcalPerHourMovement * d.time);

                const balance = totals.kcal - totalNeeded;
                const intensity = Math.min(100, ((metabolicPowerWatts/W) / (p.level * 1.2)) * 100);

                const resultHTML = `
                    <div class="outdoor-card p-8 border-t-[8px] ${i === 0 ? 'border-green-800' : 'border-blue-800'} shadow-xl">
                        <div class="flex justify-between items-start mb-8">
                            <div>
                                <h4 class="font-black text-2xl text-slate-800 leading-none">${p.name}</h4>
                                <span class="text-[10px] font-bold text-slate-400 uppercase tracking-[0.2em]">Rapport d'activité</span>
                            </div>
                            <div class="bg-slate-100 px-3 py-1 rounded-full text-[10px] font-black text-slate-500 uppercase">
                                Charge: ${L.toFixed(1)}kg
                            </div>
                        </div>

                        <div class="grid grid-cols-1 gap-4 mb-8">
                            <div class="bg-slate-50 rounded-2xl p-5 border border-slate-100">
                                <span class="block text-[10px] uppercase font-black text-slate-400 mb-1 tracking-wider italic">Dépense Estimée</span>
                                <div class="flex items-baseline gap-1">
                                    <span class="text-3xl font-black text-slate-800">${Math.round(totalNeeded)}</span>
                                    <span class="text-sm font-bold text-slate-400">kcal / jour</span>
                                </div>
                            </div>
                            <div class="bg-slate-900 rounded-2xl p-5 shadow-inner">
                                <span class="block text-[10px] uppercase font-black text-slate-500 mb-1 tracking-wider italic">Apport Prévu</span>
                                <div class="flex items-baseline gap-1">
                                    <span class="text-3xl font-black text-orange-400">${Math.round(totals.kcal)}</span>
                                    <span class="text-sm font-bold text-slate-500">kcal / jour</span>
                                </div>
                            </div>
                        </div>

                        <div class="p-4 rounded-xl mb-8 flex items-center justify-between ${balance >= 0 ? 'bg-green-50 text-green-800 border border-green-100' : 'bg-red-50 text-red-800 border border-red-100'}">
                            <span class="text-xs font-bold uppercase tracking-widest">Balance énergétique</span>
                            <span class="text-xl font-black">${balance > 0 ? '+' : ''}${Math.round(balance)}</span>
                        </div>

                        <div class="space-y-4">
                            <div class="flex justify-between text-[11px] mb-1 font-black text-slate-600 uppercase">
                                <span>Fatigue Physiologique</span>
                                <span class="${intensity > 80 ? 'text-red-600' : 'text-slate-400'}">${Math.round(intensity)}%</span>
                            </div>
                            <div class="custom-progress">
                                <div class="h-full transition-all duration-500 ${intensity > 80 ? 'bg-gradient-to-r from-orange-500 to-red-600' : 'bg-gradient-to-r from-green-400 to-orange-500'}" style="width: ${intensity}%"></div>
                            </div>
                        </div>

                        <div class="grid grid-cols-3 gap-2 mt-8">
                            <div class="text-center p-2 rounded-lg bg-slate-50 border border-slate-100">
                                <span class="block text-[8px] font-black text-slate-400 uppercase">Protéines</span>
                                <span class="text-sm font-bold text-slate-700">${Math.round(totals.prot)}g</span>
                            </div>
                            <div class="text-center p-2 rounded-lg bg-slate-50 border border-slate-100">
                                <span class="block text-[8px] font-black text-slate-400 uppercase">Glucides</span>
                                <span class="text-sm font-bold text-slate-700">${Math.round(totals.gluc)}g</span>
                            </div>
                            <div class="text-center p-2 rounded-lg bg-slate-50 border border-slate-100">
                                <span class="block text-[8px] font-black text-slate-400 uppercase">Lipides</span>
                                <span class="text-sm font-bold text-slate-700">${Math.round(totals.lip)}g</span>
                            </div>
                        </div>
                    </div>
                `;

                let foodListHTML = `
                    <div class="outdoor-card p-8 border-l-[8px] ${i === 0 ? 'border-green-800' : 'border-blue-800'} shadow-xl">
                        <div class="flex items-center gap-3 mb-6">
                            <div class="p-2 bg-slate-100 rounded-lg">
                                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="text-slate-600"><path d="M6 13.87A4 4 0 0 1 7.41 6a5.11 5.11 0 0 1 1.05-1.54 5 5 0 0 1 7.08 0A5.11 5.11 0 0 1 16.59 6 4 4 0 0 1 18 13.87V21H6Z"/><line x1="6" x2="18" y1="17" y2="17"/></svg>
                            </div>
                            <h4 class="font-black text-lg text-slate-800 uppercase tracking-tighter">Menu de ${p.name}</h4>
                        </div>
                        ${selectedItemsList.length > 0 ? `
                            <div class="space-y-1">
                                <div class="grid grid-cols-4 text-[9px] font-black text-slate-400 uppercase pb-2 px-2">
                                    <div class="col-span-2">Aliment</div><div class="text-right">Masse</div><div class="text-right">Energie</div>
                                </div>
                                ${selectedItemsList.map(item => `
                                    <div class="grid grid-cols-4 py-3 px-2 border-t border-slate-50 items-center">
                                        <div class="col-span-2">
                                            <span class="block font-bold text-slate-700 text-sm truncate pr-2">${item.label}</span>
                                            <span class="text-[9px] text-slate-400 uppercase">Portion x${item.qty}</span>
                                        </div>
                                        <div class="text-right font-medium text-slate-500 text-sm">${item.weightTotal}g</div>
                                        <div class="text-right font-black text-slate-800 text-sm">${item.kcalTotal} <span class="text-[9px] text-slate-400">kcal</span></div>
                                    </div>
                                `).join('')}
                                <div class="mt-6 p-4 bg-slate-50 rounded-xl flex justify-between items-center border border-slate-100">
                                    <span class="text-[10px] font-black text-slate-400 uppercase tracking-wider">Masse Totale</span>
                                    <span class="text-lg font-black text-slate-800">${foodWeightGrams} <span class="text-xs font-bold text-slate-400 uppercase">g</span></span>
                                </div>
                            </div>
                        ` : '<div class="text-center py-10 bg-slate-50 rounded-2xl border border-dashed border-slate-200"><p class="text-slate-400 font-medium text-sm italic">Aucune ration planifiée</p></div>'}
                    </div>
                `;

                // Rendu de la liste d'épicerie consolidée
                const groceryItems = profileGroceries[i];
                const hasGrocery = Object.keys(groceryItems).length > 0;
                let totalGroceryWeight = 0;

                let groceryListHTML = `
                    <div class="outdoor-card p-8 border-t-[8px] ${i === 0 ? 'border-green-800' : 'border-blue-800'} shadow-xl">
                        <div class="flex items-center justify-between mb-6">
                            <div class="flex items-center gap-3">
                                <div class="p-2 bg-orange-100 rounded-lg">
                                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#f97316" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="8" cy="21" r="1"/><circle cx="19" cy="21" r="1"/><path d="M2.05 2.05h2l2.66 12.42a2 2 0 0 0 2 1.58h9.78a2 2 0 0 0 1.95-1.57l1.65-7.43H5.12"/></svg>
                                </div>
                                <h4 class="font-black text-lg text-slate-800 uppercase tracking-tighter">Épicerie de ${p.name}</h4>
                            </div>
                        </div>
                        <div class="space-y-2">
                            ${hasGrocery ? `
                                <div class="grid grid-cols-4 text-[9px] font-black text-slate-400 uppercase pb-2 px-2">
                                    <div class="col-span-2">Aliment</div><div class="text-right">Quantité</div><div class="text-right">Masse Totale</div>
                                </div>
                                ${Object.keys(groceryItems).map(fid => {
                                    const foodRef = p.food.find(f => f.id === fid) || {label: 'Inconnu', weight: 0};
                                    const q = groceryItems[fid];
                                    const wt = q * foodRef.weight;
                                    totalGroceryWeight += wt;
                                    return `
                                        <div class="grid grid-cols-4 py-3 px-2 border-t border-slate-50 items-center">
                                            <div class="col-span-2 font-bold text-slate-700 text-sm">${foodRef.label}</div>
                                            <div class="text-right font-black text-orange-600 text-sm">x${q}</div>
                                            <div class="text-right font-medium text-slate-500 text-sm">${wt >= 1000 ? (wt/1000).toFixed(2)+'kg' : wt+'g'}</div>
                                        </div>
                                    `;
                                }).join('')}
                                <div class="mt-6 p-4 bg-[#1a2e1a] rounded-xl flex justify-between items-center text-white">
                                    <span class="text-[10px] font-black uppercase tracking-wider opacity-60">Poids Total à acheter</span>
                                    <span class="text-lg font-black">${totalGroceryWeight >= 1000 ? (totalGroceryWeight/1000).toFixed(2) + ' kg' : totalGroceryWeight + ' g'}</span>
                                </div>
                            ` : '<p class="text-center text-slate-400 italic py-4">Aucun aliment sélectionné sur le trek.</p>'}
                        </div>
                    </div>
                `;

                if (activeMainView === 'summary') {
                    resultsSummaryDiv.innerHTML += resultHTML;
                    foodSummaryGrid.innerHTML += foodListHTML;
                    grocerySummaryGrid.innerHTML += groceryListHTML;
                } else if (i === currentProfileIdx) {
                    localPerformanceDiv.innerHTML = resultHTML;
                }
            });
        }

        window.onload = init;
    </script>
</body>
</html>
