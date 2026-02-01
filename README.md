<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Expedition Planner Pro | Dashboard</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --forest: #1a2e1a;
            --adventure: #f97316;
            --sidebar-width: 260px;
        }
        body {
            background-color: #f8fafc;
            font-family: 'Plus Jakarta Sans', sans-serif;
            color: #1e293b;
            margin: 0;
        }

        .bento-card {
            background: white;
            border: 1px solid #e2e8f0;
            border-radius: 1rem;
            transition: all 0.2s ease;
        }
        .bento-card:hover { border-color: #cbd5e1; box-shadow: 0 4px 12px rgba(0,0,0,0.03); }

        .sidebar {
            width: var(--sidebar-width);
            background: var(--forest);
            height: 100vh;
            position: fixed;
            left: 0;
            top: 0;
            z-index: 50;
        }

        .main-wrapper { 
            margin-left: var(--sidebar-width); 
            min-height: 100vh;
            width: calc(100% - var(--sidebar-width));
        }

        .input-compact {
            background-color: #f1f5f9;
            border: 1px solid #e2e8f0;
            border-radius: 0.5rem;
            padding: 0.4rem 0.6rem;
            font-size: 0.813rem;
            width: 100%;
        }

        .table-header {
            display: grid;
            background: #f8fafc;
            padding: 0.75rem 1rem;
            font-weight: 700;
            font-size: 0.7rem;
            text-transform: uppercase;
            color: #64748b;
        }

        .nutri-row {
            display: grid;
            grid-template-columns: 40px 60px 2fr 80px 80px 60px 60px 60px 60px 60px 40px;
            gap: 0.5rem;
            align-items: center;
            padding: 0.5rem 1rem;
            border-bottom: 1px solid #f1f5f9;
        }

        .equip-row {
            display: grid;
            grid-template-columns: 40px 60px 3fr 100px 40px;
            gap: 1rem;
            align-items: center;
            padding: 0.5rem 1rem;
            border-bottom: 1px solid #f1f5f9;
        }

        .nav-link {
            display: flex;
            align-items: center;
            gap: 0.75rem;
            padding: 0.875rem 1.25rem;
            margin: 0.25rem 1rem;
            border-radius: 0.75rem;
            color: #94a3b8;
            font-weight: 600;
            font-size: 0.875rem;
        }
        .nav-link.active { background: rgba(255,255,255,0.1); color: white; }

        .tab-step {
            padding: 0.5rem 1.25rem;
            border-radius: 0.5rem;
            font-weight: 700;
            font-size: 0.75rem;
            white-space: nowrap;
        }
        .tab-step.active { background: var(--adventure); color: white; }
    </style>
</head>
<body class="antialiased">

    <aside class="sidebar flex flex-col">
        <div class="p-8">
            <div class="flex items-center gap-3 mb-10">
                <div class="bg-orange-500 p-2 rounded-xl">
                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round" class="text-white"><path d="M17 11V7a5 5 0 0 0-10 0v4"/><path d="M5 11h14v7a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2v-7Z"/><circle cx="12" cy="15" r="1"/></svg>
                </div>
                <h1 class="text-lg font-extrabold text-white tracking-tight">Expedition <span class="text-orange-500">Pro</span></h1>
            </div>
            
            <nav class="space-y-1">
                <div class="flex justify-between items-center px-5 mb-3">
                    <p class="text-[10px] font-bold text-slate-500 uppercase tracking-widest">Membres</p>
                    <button onclick="addNewProfile()" class="text-white bg-white/10 hover:bg-white/20 p-1 rounded-md">
                        <svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12h14"/><path d="M12 5v14"/></svg>
                    </button>
                </div>
                <div id="nav-profiles-list"></div>
                <div class="pt-6">
                    <p class="text-[10px] font-bold text-slate-500 uppercase tracking-widest ml-5 mb-3">Global</p>
                    <button onclick="switchMainView('summary')" id="nav-summary" class="nav-link w-full text-left">
                        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 3v18h18"/><path d="m19 9-5 5-4-4-3 3"/></svg>
                        Synthèse Mission
                    </button>
                </div>
            </nav>
        </div>
        <div class="mt-auto p-6 bg-black/20">
            <button onclick="exportData()" class="w-full mb-2 py-2.5 rounded-lg bg-white/5 text-white text-xs font-bold border border-white/10">Exporter (.json)</button>
            <button onclick="document.getElementById('importFile').click()" class="w-full py-2.5 rounded-lg bg-orange-600 text-white text-xs font-bold shadow-lg">Importer Plan</button>
            <input type="file" id="importFile" accept=".json" class="hidden" onchange="importData(event)">
        </div>
    </aside>

    <main class="main-wrapper">
        <header class="bg-white border-b border-slate-200 px-8 py-4 flex justify-between items-center sticky top-0 z-40">
            <div class="flex items-center gap-6">
                <div class="flex items-center gap-3">
                    <span class="text-xs font-bold text-slate-400 uppercase">Durée</span>
                    <div class="flex items-center bg-slate-100 rounded-lg p-1 border border-slate-200">
                        <input type="number" id="total-days" onchange="updateDayCount()" value="5" min="1" max="14" class="bg-transparent w-10 text-center font-bold text-sm">
                        <span class="text-[10px] font-bold text-slate-400 pr-2">j</span>
                    </div>
                </div>
                <div id="day-tabs" class="flex gap-2 overflow-x-auto"></div>
            </div>
            <div id="status-badge" class="px-3 py-1 bg-green-100 text-green-700 rounded-full text-[10px] font-black uppercase">Système Prêt</div>
        </header>

        <div class="p-8">
            <div id="individual-view" class="grid grid-cols-12 gap-8">
                <div class="col-span-12 lg:col-span-8 space-y-8">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <section class="bento-card p-6">
                            <h3 class="text-xs font-black text-slate-400 uppercase mb-4 flex justify-between">
                                Paramètres Randonneur
                                <button onclick="deleteCurrentProfile()" class="text-red-400 hover:text-red-600">Supprimer</button>
                            </h3>
                            <div class="grid grid-cols-2 gap-4">
                                <div class="col-span-2">
                                    <label class="text-[10px] font-bold text-slate-400 uppercase">Nom</label>
                                    <input type="text" id="p-name" oninput="updateProfileData()" class="input-compact font-bold">
                                </div>
                                <div><label class="text-[10px] font-bold text-slate-400 uppercase">Sexe</label>
                                    <select id="p-gender" onchange="updateProfileData()" class="input-compact"><option value="M">Homme</option><option value="F">Femme</option></select>
                                </div>
                                <div><label class="text-[10px] font-bold text-slate-400 uppercase">Âge</label><input type="number" id="p-age" oninput="updateProfileData()" class="input-compact"></div>
                                <div><label class="text-[10px] font-bold text-slate-400 uppercase">Taille (cm)</label><input type="number" id="p-height" oninput="updateProfileData()" class="input-compact"></div>
                                <div><label class="text-[10px] font-bold text-slate-400 uppercase">Poids (kg)</label><input type="number" id="p-weight" oninput="updateProfileData()" class="input-compact"></div>
                            </div>
                        </section>

                        <section class="bento-card p-6">
                            <h3 class="text-xs font-black text-slate-400 uppercase mb-4 tracking-widest">Topographie Étape</h3>
                            <div class="grid grid-cols-2 gap-4">
                                <div><label class="text-[10px] font-bold text-slate-400">Dist (km)</label><input type="number" id="d-dist" oninput="saveCurrentDay(); calculate()" class="input-compact text-orange-600 font-bold"></div>
                                <div><label class="text-[10px] font-bold text-slate-400">Durée (h)</label><input type="number" id="d-time" oninput="saveCurrentDay(); calculate()" step="0.5" class="input-compact"></div>
                                <div><label class="text-[10px] font-bold text-green-600">D+ (m)</label><input type="number" id="d-elev-pos" oninput="saveCurrentDay(); calculate()" class="input-compact"></div>
                                <div><label class="text-[10px] font-bold text-red-600">D- (m)</label><input type="number" id="d-elev-neg" oninput="saveCurrentDay(); calculate()" class="input-compact"></div>
                            </div>
                        </section>
                    </div>

                    <section class="bento-card p-6">
                        <div class="flex justify-between items-center mb-6">
                            <h3 class="text-xs font-black text-slate-400 uppercase">Configuration Charge</h3>
                            <div class="flex bg-slate-100 p-1 rounded-lg">
                                <button id="mode-manual" onclick="setLoadMode('manual')" class="px-4 py-1.5 text-[10px] font-black uppercase rounded-md">Manuel</button>
                                <button id="mode-auto" onclick="setLoadMode('auto')" class="px-4 py-1.5 text-[10px] font-black uppercase rounded-md">Auto</button>
                            </div>
                        </div>
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                            <div id="manual-load-box">
                                <label class="block text-[10px] font-bold text-slate-400 uppercase mb-1" id="label-load-current">Sac (kg)</label>
                                <input type="number" id="d-load-current" oninput="saveCurrentDay(); calculate()" class="input-compact text-lg font-black">
                            </div>
                            <div id="auto-load-box" class="hidden">
                                <label class="block text-[10px] font-bold text-slate-400 uppercase mb-1">Matériel</label>
                                <div id="auto-load-val" class="text-2xl font-black text-orange-600">0.00 kg</div>
                            </div>
                            <div class="md:col-span-2 bg-blue-50 p-4 rounded-xl text-[11px] text-blue-700">Le poids dégressif est calculé sur la nourriture restante.</div>
                        </div>
                    </section>

                    <section class="bento-card overflow-hidden">
                        <div class="p-4 border-b flex justify-between items-center">
                            <h3 class="text-xs font-black text-slate-400 uppercase">Alimentation</h3>
                            <button onclick="addFoodToDatabase()" class="px-3 py-1.5 bg-forest text-white rounded text-[10px] font-black">+ Ajouter</button>
                        </div>
                        <div class="overflow-x-auto"><div id="food-list-current"></div></div>
                    </section>
                </div>

                <div class="col-span-12 lg:col-span-4">
                    <div class="sticky top-[100px]" id="local-performance-view"></div>
                </div>
            </div>

            <div id="summary-view" class="hidden space-y-12">
                <div id="summary-day-tabs" class="flex gap-2 overflow-x-auto border-b pb-4"></div>
                <div id="results-summary" class="grid grid-cols-1 md:grid-cols-2 gap-8"></div>
            </div>
        </div>
    </main>

    <script>
        let currentProfileIdx = 0;
        let currentDayIdx = 0;
        let activeMainView = 'p0';
        let FOOD_DATABASE = [{id: 'f1', label: "Lyophilisé Poulet", weight: 150, kcal: 650, prot: 35, gluc: 80, lip: 15, sod: 1200, fib: 8}];
        let EQUIPMENT_DATABASE = [{id: 'e1', label: "Sac à dos 60L", weight: 1800}];
        let profiles = [{ name: "Cédrik", gender: "M", age: 30, height: 180, weight: 75, loadMode: 'manual', globalEquipment: {} }];
        let days = [];

        function init() {
            updateDayCount();
            renderProfilesNav();
            switchMainView('p0');
        }

        function renderProfilesNav() {
            const container = document.getElementById('nav-profiles-list');
            container.innerHTML = '';
            profiles.forEach((p, idx) => {
                const btn = document.createElement('button');
                btn.onclick = () => switchMainView(`p${idx}`);
                btn.className = `nav-link w-full ${activeMainView === `p${idx}` ? 'active' : ''}`;
                btn.innerHTML = `<div class="w-2 h-2 rounded-full bg-blue-500"></div><span class="truncate">${p.name || 'Sans nom'}</span>`;
                container.appendChild(btn);
            });
        }

        function updateDayCount() {
            const count = parseInt(document.getElementById('total-days').value) || 1;
            while (days.length < count) {
                days.push({ dist: 15, time: 5, elevPos: 800, elevNeg: 800, terrain: 1.2, loads: profiles.map(() => 10), selectedFood: profiles.map(() => ({})) });
            }
            renderDayTabs(activeMainView === 'summary' ? 'summary-day-tabs' : 'day-tabs');
        }

        function renderDayTabs(containerId) {
            const container = document.getElementById(containerId);
            if (!container) return;
            container.innerHTML = '';
            days.forEach((_, i) => {
                const btn = document.createElement('button');
                btn.innerText = `Étape ${i + 1}`;
                btn.className = `tab-step ${currentDayIdx === i ? 'active' : 'bg-white border'}`;
                btn.onclick = () => { currentDayIdx = i; renderDayTabs(containerId); loadDayData(); calculate(); };
                container.appendChild(btn);
            });
        }

        function switchMainView(view) {
            activeMainView = view;
            document.getElementById('individual-view').classList.toggle('hidden', view === 'summary');
            document.getElementById('summary-view').classList.toggle('hidden', view !== 'summary');
            if (view !== 'summary') {
                currentProfileIdx = parseInt(view.replace('p', ''));
                loadProfileUI();
                loadDayData();
            }
            calculate();
        }

        function loadProfileUI() {
            const p = profiles[currentProfileIdx];
            document.getElementById('p-name').value = p.name;
            document.getElementById('p-gender').value = p.gender;
            document.getElementById('p-age').value = p.age;
            document.getElementById('p-height').value = p.height;
            document.getElementById('p-weight').value = p.weight;
            updateLoadModeUI();
        }

        function loadDayData() {
            const d = days[currentDayIdx];
            document.getElementById('d-dist').value = d.dist;
            document.getElementById('d-time').value = d.time;
            document.getElementById('d-elev-pos').value = d.elevPos;
            document.getElementById('d-elev-neg').value = d.elevNeg;
            document.getElementById('d-load-current').value = d.loads[currentProfileIdx] || 10;
            renderFoodLists();
        }

        function saveCurrentDay() {
            const d = days[currentDayIdx];
            d.dist = parseFloat(document.getElementById('d-dist').value) || 0;
            d.time = parseFloat(document.getElementById('d-time').value) || 0.1;
            d.elevPos = parseFloat(document.getElementById('d-elev-pos').value) || 0;
            d.elevNeg = parseFloat(document.getElementById('d-elev-neg').value) || 0;
            d.loads[currentProfileIdx] = parseFloat(document.getElementById('d-load-current').value) || 0;
        }

        function updateProfileData() {
            const p = profiles[currentProfileIdx];
            p.name = document.getElementById('p-name').value;
            p.gender = document.getElementById('p-gender').value;
            p.age = parseInt(document.getElementById('p-age').value) || 30;
            p.height = parseInt(document.getElementById('p-height').value) || 170;
            p.weight = parseFloat(document.getElementById('p-weight').value) || 70;
            calculate();
        }

        function calculate() {
            const d = days[currentDayIdx];
            const targetDiv = (activeMainView === 'summary') ? document.getElementById('results-summary') : document.getElementById('local-performance-view');
            if (!targetDiv) return;
            targetDiv.innerHTML = '';

            profiles.forEach((p, i) => {
                if (activeMainView !== 'summary' && i !== currentProfileIdx) return;
                
                const W = p.weight || 70;
                const L = d.loads[i] || 10;
                const kcalApport = Object.keys(d.selectedFood[i]).reduce((acc, fid) => {
                    const item = FOOD_DATABASE.find(f => f.id === fid);
                    return acc + (item ? item.kcal * d.selectedFood[i][fid] : 0);
                }, 0);

                let bmr = (10 * W) + (6.25 * p.height) - (5 * p.age) + (p.gender === "M" ? 5 : -161);
                const distM = d.dist * 1000;
                const timeSec = (d.time || 1) * 3600;
                const V = distM / timeSec;
                const totalNeeded = (bmr * 1.2) + (V * 0.5 * W); // Simplifié pour stabilité

                const balance = kcalApport - totalNeeded;

                targetDiv.innerHTML += `
                    <div class="bento-card p-6 border-t-4 border-orange-500 mb-4">
                        <h4 class="font-bold mb-4">${p.name} (J${currentDayIdx+1})</h4>
                        <div class="grid grid-cols-2 gap-4 text-center">
                            <div class="bg-slate-50 p-3 rounded">
                                <span class="text-[9px] uppercase block">Besoin</span>
                                <span class="font-bold">${Math.round(totalNeeded)} kcal</span>
                            </div>
                            <div class="bg-slate-900 text-white p-3 rounded">
                                <span class="text-[9px] uppercase block">Apport</span>
                                <span class="font-bold text-orange-400">${Math.round(kcalApport)} kcal</span>
                            </div>
                        </div>
                        <div class="mt-4 p-3 rounded text-center font-bold ${balance >= 0 ? 'bg-green-100' : 'bg-red-100'}">
                            Balance: ${Math.round(balance)} kcal
                        </div>
                    </div>
                `;
            });
        }

        function renderFoodLists() {
            const list = document.getElementById('food-list-current');
            if(!list) return;
            list.innerHTML = '<div class="table-header nutri-row"><div></div><div>Qté</div><div class="col-span-2">Aliment</div><div>Kcal</div></div>';
            FOOD_DATABASE.forEach(item => {
                const qty = days[currentDayIdx].selectedFood[currentProfileIdx][item.id] || 0;
                const div = document.createElement('div');
                div.className = "nutri-row hover:bg-slate-50";
                div.innerHTML = `
                    <input type="checkbox" ${qty > 0 ? 'checked' : ''} onchange="toggleFood('${item.id}')">
                    <input type="number" value="${qty}" class="w-12 text-center bg-transparent" oninput="updateFoodQty('${item.id}', this.value)">
                    <div class="col-span-2 text-xs font-bold">${item.label}</div>
                    <div class="text-xs">${item.kcal}</div>
                `;
                list.appendChild(div);
            });
        }

        function toggleFood(id) {
            const sel = days[currentDayIdx].selectedFood[currentProfileIdx];
            sel[id] = sel[id] > 0 ? 0 : 1;
            renderFoodLists(); calculate();
        }

        function updateFoodQty(id, qty) {
            days[currentDayIdx].selectedFood[currentProfileIdx][id] = parseInt(qty) || 0;
            calculate();
        }

        function setLoadMode(mode) { 
            profiles[currentProfileIdx].loadMode = mode; 
            updateLoadModeUI(); 
        }

        function updateLoadModeUI() {
            const isManual = profiles[currentProfileIdx].loadMode === 'manual';
            document.getElementById('manual-load-box').classList.toggle('hidden', !isManual);
            document.getElementById('auto-load-box').classList.toggle('hidden', isManual);
            document.getElementById('mode-manual').className = isManual ? 'px-4 py-1.5 bg-forest text-white rounded-md' : 'px-4 py-1.5 text-slate-400';
            document.getElementById('mode-auto').className = !isManual ? 'px-4 py-1.5 bg-orange-500 text-white rounded-md' : 'px-4 py-1.5 text-slate-400';
        }

        window.onload = init;
    </script>
</body>
</html>
