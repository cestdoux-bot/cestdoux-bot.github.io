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
            overflow-x: hidden;
        }

        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: #f1f5f9; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
        ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }

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

        .main-wrapper { margin-left: var(--sidebar-width); width: calc(100% - var(--sidebar-width)); }

        .input-compact {
            background-color: #f1f5f9;
            border: 1px solid #e2e8f0;
            border-radius: 0.5rem;
            padding: 0.4rem 0.6rem;
            font-size: 0.813rem;
            width: 100%;
            transition: all 0.2s;
        }
        .input-compact:focus {
            outline: none;
            border-color: var(--adventure);
            background-color: white;
            ring: 2px solid rgba(249, 115, 22, 0.1);
        }

        .table-header {
            display: grid;
            background: #f8fafc;
            padding: 0.75rem 1rem;
            border-radius: 0.75rem 0.75rem 0 0;
            border-bottom: 1px solid #e2e8f0;
            font-weight: 700;
            font-size: 0.7rem;
            text-transform: uppercase;
            letter-spacing: 0.05em;
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
            transition: all 0.2s;
        }
        .nav-link.active { background: rgba(255,255,255,0.1); color: white; }
        .nav-link:hover:not(.active) { background: rgba(255,255,255,0.05); color: #cbd5e1; }

        .terrain-slider {
            -webkit-appearance: none;
            height: 4px;
            background: #e2e8f0;
            border-radius: 2px;
        }
        .terrain-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 16px;
            height: 16px;
            background: var(--adventure);
            border-radius: 50%;
            cursor: pointer;
            border: 2px solid white;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        .tab-step {
            padding: 0.5rem 1.25rem;
            border-radius: 0.5rem;
            font-weight: 700;
            font-size: 0.75rem;
            white-space: nowrap;
            transition: all 0.2s;
        }
        .tab-step.active { background: var(--adventure); color: white; }
        .tab-step:not(.active) { background: white; color: #64748b; border: 1px solid #e2e8f0; }
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
                    <button onclick="addNewProfile()" class="text-white bg-white/10 hover:bg-white/20 p-1 rounded-md transition-colors">
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
            <div class="flex flex-col gap-2">
                <button onclick="exportData()" class="w-full py-2.5 rounded-lg bg-white/5 hover:bg-white/10 text-white text-xs font-bold transition-all border border-white/10">Exporter (.json)</button>
                <button onclick="document.getElementById('importFile').click()" class="w-full py-2.5 rounded-lg bg-orange-600 hover:bg-orange-700 text-white text-xs font-bold transition-all shadow-lg shadow-orange-900/40">Importer Plan</button>
                <input type="file" id="importFile" accept=".json" class="hidden" onchange="importData(event)">
            </div>
        </div>
    </aside>

    <main class="main-wrapper min-h-screen">
        <header class="bg-white border-b border-slate-200 px-8 py-4 flex justify-between items-center sticky top-0 z-40">
            <div class="flex items-center gap-6">
                <div class="flex items-center gap-3">
                    <span class="text-xs font-bold text-slate-400 uppercase tracking-widest">Durée du trek</span>
                    <div class="flex items-center bg-slate-100 rounded-lg p-1 border border-slate-200">
                        <input type="number" id="total-days" onchange="updateDayCount()" value="5" min="1" max="14" class="bg-transparent w-10 text-center font-bold text-slate-700 focus:outline-none text-sm">
                        <span class="text-[10px] font-bold text-slate-400 pr-2">jours</span>
                    </div>
                </div>
                <div id="day-tabs" class="flex gap-2 overflow-x-auto max-w-md pb-1"></div>
            </div>
            <div id="status-badge" class="px-3 py-1 bg-green-100 text-green-700 rounded-full text-[10px] font-black uppercase">Système Prêt</div>
        </header>

        <div class="p-8">
            <div id="individual-view" class="grid grid-cols-12 gap-8">
                <div class="col-span-12 lg:col-span-8 space-y-8">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <section class="bento-card p-6">
                            <div class="flex justify-between items-start mb-4">
                                <h3 class="text-xs font-black text-slate-400 uppercase tracking-widest flex items-center gap-2">
                                    <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></svg>
                                    Paramètres Randonneur
                                </h3>
                                <button onclick="deleteCurrentProfile()" class="text-red-400 hover:text-red-600 transition-colors">
                                    <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M3 6h18"/><path d="M19 6v14c0 1-1 2-2 2H7c-1 0-2-1-2-2V6"/><path d="M8 6V4c0-1 1-2 2-2h4c1 0 2 1 2 2v2"/></svg>
                                </button>
                            </div>
                            <div class="grid grid-cols-2 gap-4">
                                <div class="col-span-2">
                                    <label class="text-[10px] font-bold text-slate-400 uppercase ml-1">Nom</label>
                                    <input type="text" id="p-name" oninput="updateProfileData()" class="input-compact font-bold">
                                </div>
                                <div>
                                    <label class="text-[10px] font-bold text-slate-400 uppercase ml-1">Sexe</label>
                                    <select id="p-gender" onchange="updateProfileData()" class="input-compact appearance-none">
                                        <option value="M">Homme</option>
                                        <option value="F">Femme</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="text-[10px] font-bold text-slate-400 uppercase ml-1">Âge</label>
                                    <input type="number" id="p-age" oninput="updateProfileData()" class="input-compact">
                                </div>
                                <div>
                                    <label class="text-[10px] font-bold text-slate-400 uppercase ml-1">Taille (cm)</label>
                                    <input type="number" id="p-height" oninput="updateProfileData()" class="input-compact">
                                </div>
                                <div>
                                    <label class="text-[10px] font-bold text-slate-400 uppercase ml-1">Poids (kg)</label>
                                    <input type="number" id="p-weight" oninput="updateProfileData()" class="input-compact">
                                </div>
                            </div>
                        </section>

                        <section class="bento-card p-6">
                            <h3 class="text-xs font-black text-slate-400 uppercase tracking-widest mb-4 flex items-center gap-2">
                                <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="m4.7 14 5.5-5.5 4.1 4.1 4.5-4.5"/><path d="M11 5H4v7"/><path d="M20 19v-7h-7"/></svg>
                                Topographie de l'Étape
                            </h3>
                            <div class="grid grid-cols-2 gap-4 mb-4">
                                <div>
                                    <label class="text-[10px] font-bold text-slate-400 uppercase ml-1">Distance (km)</label>
                                    <input type="number" id="d-dist" oninput="saveCurrentDay(); calculate()" class="input-compact font-bold text-orange-600">
                                </div>
                                <div>
                                    <label class="text-[10px] font-bold text-slate-400 uppercase ml-1">Durée (h)</label>
                                    <input type="number" id="d-time" oninput="saveCurrentDay(); calculate()" step="0.5" class="input-compact font-bold">
                                </div>
                                <div>
                                    <label class="text-[10px] font-bold text-green-600 uppercase ml-1">D+ (m)</label>
                                    <input type="number" id="d-elev-pos" oninput="saveCurrentDay(); calculate()" class="input-compact text-green-700">
                                </div>
                                <div>
                                    <label class="text-[10px] font-bold text-red-600 uppercase ml-1">D- (m)</label>
                                    <input type="number" id="d-elev-neg" oninput="saveCurrentDay(); calculate()" class="input-compact text-red-700">
                                </div>
                            </div>
                            <div class="bg-slate-50 p-3 rounded-lg border border-slate-100">
                                <div class="flex justify-between items-center mb-1">
                                    <label class="text-[10px] font-bold text-slate-400 uppercase tracking-tight">Terrain : <span id="terrain-val-display" class="text-orange-600">1.2</span></label>
                                    <span id="terrain-desc" class="text-[9px] font-bold text-slate-500 italic">Sentier battu</span>
                                </div>
                                <input type="range" id="d-terrain" min="1.0" max="2.5" step="0.1" value="1.2" 
                                       oninput="updateTerrainLabel(this.value); saveCurrentDay(); calculate()" 
                                       class="terrain-slider w-full">
                            </div>
                        </section>
                    </div>

                    <section class="bento-card p-6">
                        <div class="flex justify-between items-center mb-6">
                            <h3 class="text-xs font-black text-slate-400 uppercase tracking-widest flex items-center gap-2">
                                <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M12 21a9 9 0 1 0 0-18 9 9 0 0 0 0 18Z"/><path d="M12 8v4"/><path d="M12 16h.01"/></svg>
                                Configuration de la Charge
                            </h3>
                            <div class="flex bg-slate-100 p-1 rounded-lg border border-slate-200">
                                <button id="mode-manual" onclick="setLoadMode('manual')" class="px-4 py-1.5 text-[10px] font-black uppercase rounded-md transition-all">Manuel</button>
                                <button id="mode-auto" onclick="setLoadMode('auto')" class="px-4 py-1.5 text-[10px] font-black uppercase rounded-md transition-all">Auto</button>
                            </div>
                        </div>
                        
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 items-center">
                            <div id="manual-load-box" class="md:col-span-1">
                                <label class="block text-[10px] font-bold text-slate-400 uppercase mb-1" id="label-load-current">Poids fixe (kg)</label>
                                <input type="number" id="d-load-current" oninput="saveCurrentDay(); calculate()" class="input-compact text-lg font-black bg-white">
                            </div>
                            <div id="auto-load-box" class="hidden md:col-span-1">
                                <label class="block text-[10px] font-bold text-slate-400 uppercase mb-1">Inventaire matériel (Constant)</label>
                                <div id="auto-load-val" class="text-2xl font-black text-orange-600">0.00 kg</div>
                            </div>
                            <div class="md:col-span-2 flex items-center gap-4 bg-blue-50/50 p-4 rounded-xl border border-blue-100">
                                <div class="bg-blue-500 p-2 rounded-lg text-white">
                                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="M12 16v-4"/><path d="M12 8h.01"/></svg>
                                </div>
                                <p class="text-[11px] text-blue-700 leading-tight font-medium">L'équipement est fixe pour tout le trek. Le poids dégressif est calculé sur la nourriture restante à partir du jour sélectionné.</p>
                            </div>
                        </div>
                    </section>

                    <section class="bento-card overflow-hidden">
                        <div class="p-6 border-b border-slate-100 flex justify-between items-center bg-white">
                            <h3 class="text-xs font-black text-slate-400 uppercase tracking-widest">Alimentation (Journalier)</h3>
                            <button onclick="addFoodToDatabase()" class="px-4 py-2 bg-orange-700 text-white rounded-lg text-[10px] font-black uppercase tracking-widest hover:bg-black transition-all">
                                + Ajouter aliment global
                            </button>
                        </div>
                        <div class="overflow-x-auto">
                            <div class="min-w-[1000px]">
                                <div class="nutri-row table-header">
                                    <div class="text-center">Choix</div>
                                    <div class="text-center">Qté</div>
                                    <div>Aliment</div>
                                    <div>Poids (g)</div>
                                    <div>Kcal</div>
                                    <div>Prot (g)</div>
                                    <div>Gluc (g)</div>
                                    <div>Lip (g)</div>
                                    <div>Na (mg)</div>
                                    <div>Fib</div>
                                    <div class="text-center">X</div>
                                </div>
                                <div id="food-list-current" class="max-h-[500px] overflow-y-auto"></div>
                            </div>
                        </div>
                    </section>

                    <section class="bento-card overflow-hidden">
                        <div class="p-6 border-b border-slate-100 flex justify-between items-center bg-white">
                            <h3 class="text-xs font-black text-slate-400 uppercase tracking-widest">Inventaire Matériel (Permanent)</h3>
                            <button onclick="addEquipToDatabase()" class="px-4 py-2 bg-orange-500 text-white rounded-lg text-[10px] font-black uppercase tracking-widest hover:bg-orange-600 transition-all">
                                + Ajouter matériel global
                            </button>
                        </div>
                        <div class="overflow-x-auto">
                            <div class="min-w-[800px]">
                                <div class="equip-row table-header">
                                    <div class="text-center">Porté</div>
                                    <div class="text-center">Qté</div>
                                    <div>Objet / Description</div>
                                    <div>Poids (g)</div>
                                    <div class="text-center">X</div>
                                </div>
                                <div id="equip-list-current" class="max-h-[400px] overflow-y-auto"></div>
                            </div>
                        </div>
                    </section>
                </div>

                <div class="col-span-12 lg:col-span-4">
                    <div class="sticky top-[100px]" id="local-performance-view"></div>
                </div>
            </div>

            <div id="summary-view" class="hidden space-y-12 pb-20">
                <div id="summary-day-tabs" class="flex gap-2 overflow-x-auto border-b border-slate-200 pb-4"></div>
                <div class="space-y-4">
                    <div class="flex items-center gap-4">
                        <h2 class="text-2xl font-black text-slate-800 tracking-tight">ANALYSE DE MISSION</h2>
                        <div class="h-px flex-1 bg-slate-200"></div>
                    </div>
                    <div id="results-summary" class="grid grid-cols-1 md:grid-cols-2 gap-8"></div>
                </div>
                <div class="space-y-4">
                    <div class="flex items-center gap-4">
                        <h2 class="text-2xl font-black text-slate-800 tracking-tight uppercase">Plan Nutritionnel</h2>
                        <div class="h-px flex-1 bg-slate-200"></div>
                    </div>
                    <div id="food-summary-grid" class="grid grid-cols-1 md:grid-cols-2 gap-8"></div>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-12">
                    <div class="space-y-4">
                        <h3 class="text-xl font-black text-slate-800 uppercase">Épicerie Globale</h3>
                        <div id="grocery-summary-grid" class="space-y-4"></div>
                    </div>
                    <div class="space-y-4">
                        <h3 class="text-xl font-black text-slate-800 uppercase">Inventaire Matériel</h3>
                        <div id="equipment-summary-grid" class="space-y-4"></div>
                    </div>
                </div>
            </div>
        </div>
    </main>

    <script>
        let currentProfileIdx = 0;
        let currentDayIdx = 0;
        let activeMainView = 'p0';

        let FOOD_DATABASE = [
            {id: 'f1', label: "Lyophilisé Poulet", weight: 150, kcal: 650, prot: 35, gluc: 80, lip: 15, sod: 1200, fib: 8},
            {id: 'f2', label: "Pâtes au Pesto", weight: 180, kcal: 580, prot: 18, gluc: 75, lip: 22, sod: 950, fib: 5}
        ];
        
        let EQUIPMENT_DATABASE = [
            {id: 'e1', label: "Sac à dos 60L", weight: 1800},
            {id: 'e2', label: "Tente 2P", weight: 2200}
        ];

        let profiles = [
            { name: "Cédrik", gender: "M", age: 30, height: 180, weight: 75, loadMode: 'manual', globalEquipment: {} },
            { name: "P-A", gender: "M", age: 32, height: 175, weight: 70, loadMode: 'manual', globalEquipment: {} }
        ];

        let days = [];

        function saveToLocalStorage() {
            const dataToSave = { profiles, days, FOOD_DATABASE, EQUIPMENT_DATABASE, totalDays: document.getElementById('total-days').value };
            localStorage.setItem('rando_expert_v3_shared', JSON.stringify(dataToSave));
        }

        function init() {
            const savedData = localStorage.getItem('rando_expert_v3_shared');
            if (savedData) {
                try {
                    const parsed = JSON.parse(savedData);
                    if (parsed.profiles) {
                        profiles = parsed.profiles;
                        profiles.forEach(p => { if(!p.globalEquipment) p.globalEquipment = {}; });
                    }
                    if (parsed.days) days = parsed.days;
                    if (parsed.FOOD_DATABASE) FOOD_DATABASE = parsed.FOOD_DATABASE;
                    if (parsed.EQUIPMENT_DATABASE) EQUIPMENT_DATABASE = parsed.EQUIPMENT_DATABASE;
                    if (parsed.totalDays) document.getElementById('total-days').value = parsed.totalDays;
                } catch (e) { console.error(e); }
            }
            updateDayCount();
            renderProfilesNav();
            switchMainView('p0');
        }

        function renderProfilesNav() {
            const container = document.getElementById('nav-profiles-list');
            container.innerHTML = '';
            profiles.forEach((p, idx) => {
                const colors = ['bg-green-500', 'bg-blue-500', 'bg-purple-500', 'bg-yellow-500', 'bg-pink-500'];
                const color = colors[idx % colors.length];
                const btn = document.createElement('button');
                btn.id = `nav-p${idx}`;
                btn.onclick = () => switchMainView(`p${idx}`);
                btn.className = `nav-link w-full text-left ${activeMainView === `p${idx}` ? 'active' : ''}`;
                btn.innerHTML = `<div class="w-2 h-2 rounded-full ${color}"></div><span class="truncate">${p.name || 'Sans nom'}</span>`;
                container.appendChild(btn);
            });
        }

        function addNewProfile() {
            const newIdx = profiles.length;
            profiles.push({ name: "Nouveau", gender: "M", age: 30, height: 175, weight: 70, loadMode: 'manual', globalEquipment: {} });
            days.forEach(day => {
                day.loads.push(10);
                day.selectedFood.push({});
            });
            renderProfilesNav();
            switchMainView(`p${newIdx}`);
            saveToLocalStorage();
        }

        function deleteCurrentProfile() {
            if (profiles.length <= 1) return alert("Vous devez avoir au moins un profil.");
            if (!confirm(`Supprimer le profil de ${profiles[currentProfileIdx].name} ?`)) return;
            const idxToRemove = currentProfileIdx;
            days.forEach(day => {
                day.loads.splice(idxToRemove, 1);
                day.selectedFood.splice(idxToRemove, 1);
            });
            profiles.splice(idxToRemove, 1);
            currentProfileIdx = 0;
            renderProfilesNav();
            switchMainView('p0');
            saveToLocalStorage();
        }

        function getTerrainLabel(val) {
            const v = parseFloat(val);
            if (v <= 1.0) return "Route / Bitume";
            if (v <= 1.2) return "Sentier / Prairie";
            if (v <= 1.5) return "Forêt / Herbes";
            if (v <= 1.8) return "Eboulis / Rochers";
            if (v <= 2.1) return "Sable / Neige";
            return "Marécage / Hostile";
        }

        function updateTerrainLabel(val) {
            document.getElementById('terrain-val-display').innerText = parseFloat(val).toFixed(1);
            document.getElementById('terrain-desc').innerText = getTerrainLabel(val);
        }

        function switchMainView(view) {
            activeMainView = view;
            const indView = document.getElementById('individual-view');
            const sumView = document.getElementById('summary-view');
            document.querySelectorAll('.nav-link').forEach(el => el.classList.remove('active'));
            const activeNav = document.getElementById(`nav-${view}`);
            if (activeNav) activeNav.classList.add('active');

            if (view === 'summary') {
                indView.classList.add('hidden');
                sumView.classList.remove('hidden');
                renderDayTabs('summary-day-tabs');
                calculate();
            } else {
                indView.classList.remove('hidden');
                sumView.classList.add('hidden');
                currentProfileIdx = parseInt(view.replace('p', ''));
                loadProfileUI();
                renderDayTabs('day-tabs');
                loadDayData();
                renderFoodLists();
                renderEquipLists();
                calculate();
            }
        }

        function setLoadMode(mode) {
            profiles[currentProfileIdx].loadMode = mode;
            updateLoadModeUI();
            calculate();
            saveToLocalStorage();
        }

        function updateLoadModeUI() {
            const mode = profiles[currentProfileIdx].loadMode;
            const isManual = mode === 'manual';
            document.getElementById('mode-manual').className = `px-4 py-1.5 text-[10px] font-black uppercase rounded-md transition-all ${isManual ? 'bg-forest text-white shadow-md' : 'text-slate-400'}`;
            document.getElementById('mode-auto').className = `px-4 py-1.5 text-[10px] font-black uppercase rounded-md transition-all ${!isManual ? 'bg-orange-500 text-white shadow-md' : 'text-slate-400'}`;
            document.getElementById('manual-load-box').classList.toggle('hidden', !isManual);
            document.getElementById('auto-load-box').classList.toggle('hidden', isManual);
        }

        function updateDayCount() {
            const count = parseInt(document.getElementById('total-days').value) || 1;
            while (days.length < count) {
                days.push({ 
                    dist: 15, time: 5, elevPos: 800, elevNeg: 800, terrain: 1.2, 
                    loads: profiles.map(() => 10),
                    selectedFood: profiles.map(() => ({}))
                });
            }
            if (days.length > count) days = days.slice(0, count);
            renderDayTabs(activeMainView === 'summary' ? 'summary-day-tabs' : 'day-tabs');
            saveToLocalStorage();
        }

        function renderDayTabs(containerId) {
            const container = document.getElementById(containerId);
            if (!container) return;
            container.innerHTML = '';
            days.forEach((_, i) => {
                const btn = document.createElement('button');
                btn.innerText = `Étape ${i + 1}`;
                btn.className = `tab-step ${currentDayIdx === i ? 'active' : ''}`;
                btn.onclick = () => { 
                    saveCurrentDay(); 
                    currentDayIdx = i; 
                    renderDayTabs(containerId); 
                    calculate();
                    if (activeMainView !== 'summary') { loadDayData(); renderFoodLists(); renderEquipLists(); }
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
            document.getElementById('label-load-current').innerText = `Sac de ${p.name} (kg)`;
            renderProfilesNav();
            updateLoadModeUI();
        }

        function loadDayData() {
            const d = days[currentDayIdx];
            document.getElementById('d-dist').value = d.dist;
            document.getElementById('d-time').value = d.time;
            document.getElementById('d-elev-pos').value = d.elevPos;
            document.getElementById('d-elev-neg').value = d.elevNeg;
            document.getElementById('d-terrain').value = d.terrain;
            document.getElementById('d-load-current').value = d.loads[currentProfileIdx];
            updateTerrainLabel(d.terrain);
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
            saveToLocalStorage();
        }

        function updateProfileData() {
            const p = profiles[currentProfileIdx];
            p.name = document.getElementById('p-name').value;
            p.gender = document.getElementById('p-gender').value;
            p.age = parseInt(document.getElementById('p-age').value) || 0;
            p.height = parseInt(document.getElementById('p-height').value) || 0;
            p.weight = parseFloat(document.getElementById('p-weight').value) || 0;
            renderProfilesNav();
            calculate();
            saveToLocalStorage();
        }

        function addEquipToDatabase() {
            const newId = 'e-' + Math.random().toString(36).substr(2, 9);
            EQUIPMENT_DATABASE.push({ id: newId, label: "Nouvel objet", weight: 500 });
            renderEquipLists();
            saveToLocalStorage();
        }

        function renderEquipLists() {
            const list = document.getElementById('equip-list-current');
            list.innerHTML = '';
            const p = profiles[currentProfileIdx];
            
            EQUIPMENT_DATABASE.forEach((item, dbIdx) => {
                const qty = p.globalEquipment[item.id] || 0;
                const div = document.createElement('div');
                div.className = `equip-row ${qty > 0 ? 'bg-orange-50/30' : ''}`;
                div.innerHTML = `
                    <div class="flex justify-center"><input type="checkbox" ${qty > 0 ? 'checked' : ''} onchange="toggleEquipSelection('${item.id}')" class="w-4 h-4 rounded accent-orange-500"></div>
                    <div><input type="number" value="${qty}" min="0" oninput="updateEquipQuantity('${item.id}', this.value)" class="input-compact text-center"></div>
                    <div><input type="text" value="${item.label}" oninput="EQUIPMENT_DATABASE[${dbIdx}].label = this.value; saveToLocalStorage();" class="input-compact"></div>
                    <div><input type="number" value="${item.weight}" oninput="EQUIPMENT_DATABASE[${dbIdx}].weight = parseFloat(this.value)||0; calculate(); saveToLocalStorage();" class="input-compact"></div>
                    <button onclick="EQUIPMENT_DATABASE.splice(${dbIdx},1); renderEquipLists(); calculate(); saveToLocalStorage();" class="text-slate-300 hover:text-red-500 transition-colors mx-auto">
                        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 6h18"/><path d="M19 6v14c0 1-1 2-2 2H7c-1 0-2-1-2-2V6"/></svg>
                    </button>
                `;
                list.appendChild(div);
            });
        }

        function toggleEquipSelection(id) {
            const sel = profiles[currentProfileIdx].globalEquipment;
            sel[id] = sel[id] > 0 ? 0 : 1;
            renderEquipLists();
            calculate();
            saveToLocalStorage();
        }

        function updateEquipQuantity(id, qty) {
            profiles[currentProfileIdx].globalEquipment[id] = parseInt(qty) || 0;
            renderEquipLists();
            calculate();
            saveToLocalStorage();
        }

        function addFoodToDatabase() {
            const newId = 'f-' + Math.random().toString(36).substr(2, 9);
            FOOD_DATABASE.push({ id: newId, label: "Nouvel aliment", weight: 200, kcal: 400, prot: 0, gluc: 0, lip: 0, sod: 0, fib: 0 });
            renderFoodLists();
            saveToLocalStorage();
        }

        function renderFoodLists() {
            const list = document.getElementById('food-list-current');
            list.innerHTML = '';
            const pIdx = currentProfileIdx;
            const dIdx = currentDayIdx;
            
            FOOD_DATABASE.forEach((item, dbIdx) => {
                const qty = days[dIdx].selectedFood[pIdx][item.id] || 0;
                const div = document.createElement('div');
                div.className = `nutri-row ${qty > 0 ? 'bg-orange-50/40' : ''}`;
                div.innerHTML = `
                    <div class="flex justify-center"><input type="checkbox" ${qty > 0 ? 'checked' : ''} onchange="toggleFoodSelection('${item.id}')" class="w-4 h-4 rounded accent-orange-500"></div>
                    <div><input type="number" value="${qty}" min="0" oninput="updateFoodQuantity('${item.id}', this.value)" class="input-compact text-center"></div>
                    <div><input type="text" value="${item.label}" oninput="FOOD_DATABASE[${dbIdx}].label = this.value; saveToLocalStorage();" class="input-compact font-medium"></div>
                    <div><input type="number" value="${item.weight}" oninput="FOOD_DATABASE[${dbIdx}].weight = parseFloat(this.value)||0; calculate(); saveToLocalStorage();" class="input-compact"></div>
                    <div><input type="number" value="${item.kcal}" oninput="FOOD_DATABASE[${dbIdx}].kcal = parseFloat(this.value)||0; calculate(); saveToLocalStorage();" class="input-compact font-bold"></div>
                    <div><input type="number" value="${item.prot}" oninput="FOOD_DATABASE[${dbIdx}].prot = parseFloat(this.value)||0; calculate();" class="input-compact"></div>
                    <div><input type="number" value="${item.gluc}" oninput="FOOD_DATABASE[${dbIdx}].gluc = parseFloat(this.value)||0; calculate();" class="input-compact"></div>
                    <div><input type="number" value="${item.lip}" oninput="FOOD_DATABASE[${dbIdx}].lip = parseFloat(this.value)||0; calculate();" class="input-compact"></div>
                    <div><input type="number" value="${item.sod}" oninput="FOOD_DATABASE[${dbIdx}].sod = parseFloat(this.value)||0; calculate();" class="input-compact"></div>
                    <div><input type="number" value="${item.fib}" oninput="FOOD_DATABASE[${dbIdx}].fib = parseFloat(this.value)||0; calculate();" class="input-compact"></div>
                    <button onclick="FOOD_DATABASE.splice(${dbIdx},1); renderFoodLists(); calculate(); saveToLocalStorage();" class="text-slate-300 hover:text-red-500 transition-colors mx-auto">
                        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 6h18"/><path d="M19 6v14c0 1-1 2-2 2H7c-1 0-2-1-2-2V6"/></svg>
                    </button>
                `;
                list.appendChild(div);
            });
        }

        function toggleFoodSelection(id) {
            const sel = days[currentDayIdx].selectedFood[currentProfileIdx];
            sel[id] = sel[id] > 0 ? 0 : 1;
            renderFoodLists();
            calculate();
            saveToLocalStorage();
        }

        function updateFoodQuantity(id, qty) {
            days[currentDayIdx].selectedFood[currentProfileIdx][id] = parseInt(qty) || 0;
            renderFoodLists();
            calculate();
            saveToLocalStorage();
        }

        function calculate() {
            const d = days[currentDayIdx];
            const resultsSummaryDiv = document.getElementById('results-summary');
            const foodSummaryGrid = document.getElementById('food-summary-grid');
            const localPerformanceDiv = document.getElementById('local-performance-view');
            const grocerySummaryGrid = document.getElementById('grocery-summary-grid');
            const equipmentSummaryGrid = document.getElementById('equipment-summary-grid');
            
            if (resultsSummaryDiv) resultsSummaryDiv.innerHTML = '';
            if (foodSummaryGrid) foodSummaryGrid.innerHTML = '';
            if (localPerformanceDiv) localPerformanceDiv.innerHTML = '';
            if (grocerySummaryGrid) grocerySummaryGrid.innerHTML = '';
            if (equipmentSummaryGrid) equipmentSummaryGrid.innerHTML = '';

            profiles.forEach((p, i) => {
                const W = p.weight || 1;
                
                // Calcul poids matos (GLOBAL AU PROFIL)
                const globalEquip = p.globalEquipment || {};
                let equipWeightKg = 0;
                for(const eid in globalEquip) {
                    const item = EQUIPMENT_DATABASE.find(e => e.id === eid);
                    if(item) equipWeightKg += (item.weight * (globalEquip[eid] || 0));
                }
                equipWeightKg /= 1000;
                
                let totalFoodGrams = 0;
                let consumedWeightUntilToday = 0;
                days.forEach((day, idx) => {
                    const selections = day.selectedFood[i];
                    for (const fId in selections) {
                        const foodItem = FOOD_DATABASE.find(f => f.id === fId);
                        if (foodItem) {
                            const weight = (foodItem.weight * (selections[fId] || 0));
                            totalFoodGrams += weight;
                            if (idx < currentDayIdx) consumedWeightUntilToday += weight;
                        }
                    }
                });

                const foodWeightRemainingKg = (totalFoodGrams - consumedWeightUntilToday) / 1000;
                const baseSacPoids = (p.loadMode === 'auto') ? equipWeightKg : (d.loads[i] || 0);
                const L = baseSacPoids + foodWeightRemainingKg;

                if (i === currentProfileIdx && activeMainView !== 'summary') {
                    document.getElementById('auto-load-val').innerText = `${equipWeightKg.toFixed(2)} kg`;
                }

                const daySelections = d.selectedFood[i];
                let selectedItemsList = [];
                const totals = FOOD_DATABASE.reduce((acc, cur) => {
                    const qty = daySelections[cur.id] || 0;
                    if (qty > 0) {
                        acc.kcal += cur.kcal * qty; acc.prot += cur.prot * qty; acc.gluc += cur.gluc * qty; 
                        acc.lip += cur.lip * qty; acc.sod += cur.sod * qty; acc.fib += cur.fib * qty;
                        selectedItemsList.push({ label: cur.label, qty: qty, weightTotal: Math.round((cur.weight || 0) * qty), kcalTotal: Math.round(cur.kcal * qty) });
                    }
                    return acc;
                }, { kcal: 0, prot: 0, gluc: 0, lip: 0, sod: 0, fib: 0 });

                let bmrDay = (10 * W) + (6.25 * p.height) - (5 * p.age) + (p.gender === "M" ? 5 : -161);
                const timeSec = (d.time || 1) * 3600;
                const distM = (d.dist || 0.001) * 1000;
                const V = distM / timeSec; 
                const gradePos = (d.elevPos / distM) * 100;
                const gradeNeg = (d.elevNeg / distM) * 100;
                const loadTerm = 2.0 * (W + L) * Math.pow(L / W, 2);
                const basePower = 1.5 * W + loadTerm;
                const metabolicPowerWatts = basePower + d.terrain * (W + L) * (1.5 * Math.pow(V, 2) + (0.35 * V * gradePos) + (V * (gradeNeg / 3) * 0.3));
                const totalNeeded = ((bmrDay / 24) * Math.max(0, 24 - d.time)) + (metabolicPowerWatts * 0.860 * d.time);
                const balance = totals.kcal - totalNeeded;
                const intensity = Math.min(100, ((metabolicPowerWatts/W) / (7 * 1.2)) * 100);

                const resultHTML = `
                    <div class="bento-card p-6 border-t-4 border-orange-500">
                        <div class="flex justify-between items-center mb-6">
                            <h4 class="font-extrabold text-xl text-slate-800">${p.name}</h4>
                            <div class="px-2 py-1 bg-slate-100 rounded text-[9px] font-black uppercase text-slate-500">Charge Totale (J${currentDayIdx+1}): ${L.toFixed(2)}kg</div>
                        </div>
                        <div class="space-y-4">
                            <div class="grid grid-cols-2 gap-4">
                                <div class="bg-slate-50 p-4 rounded-xl border border-slate-100">
                                    <span class="text-[9px] font-bold text-slate-400 uppercase block mb-1">Dépense</span>
                                    <div class="text-xl font-black text-slate-800">${Math.round(totalNeeded)} <span class="text-[10px] text-slate-400">kcal</span></div>
                                </div>
                                <div class="bg-slate-900 p-4 rounded-xl">
                                    <span class="text-[9px] font-bold text-slate-500 uppercase block mb-1">Apport</span>
                                    <div class="text-xl font-black text-orange-400">${Math.round(totals.kcal)} <span class="text-[10px] text-slate-500">kcal</span></div>
                                </div>
                            </div>
                            <div class="p-4 rounded-xl flex justify-between items-center ${balance >= 0 ? 'bg-green-50 text-green-700' : 'bg-red-50 text-red-700'}">
                                <span class="text-[10px] font-black uppercase">Balance</span>
                                <span class="text-lg font-black">${balance > 0 ? '+' : ''}${Math.round(balance)} kcal</span>
                            </div>
                            <div class="space-y-2">
                                <div class="flex justify-between text-[10px] font-bold uppercase">
                                    <span class="text-slate-400">Intensité Relative</span>
                                    <span class="${intensity > 80 ? 'text-red-600' : 'text-slate-600'}">${Math.round(intensity)}%</span>
                                </div>
                                <div class="h-2 bg-slate-100 rounded-full overflow-hidden">
                                    <div class="h-full ${intensity > 80 ? 'bg-red-500' : 'bg-green-500'}" style="width: ${intensity}%"></div>
                                </div>
                            </div>
                        </div>
                    </div>
                `;

                if (activeMainView === 'summary') {
                    resultsSummaryDiv.innerHTML += resultHTML;
                    foodSummaryGrid.innerHTML += `
                        <div class="bento-card p-6 border-l-4 border-slate-200">
                            <h4 class="font-black text-sm mb-4 uppercase">Menu ${p.name} - Étape ${currentDayIdx+1}</h4>
                            <div class="space-y-2">
                                ${selectedItemsList.map(item => `<div class="flex justify-between text-xs py-1 border-b border-slate-50"><span>${item.label} (x${item.qty})</span><span class="font-bold">${item.kcalTotal} kcal</span></div>`).join('')}
                            </div>
                        </div>`;
                    
                    let groceryItems = {};
                    days.forEach(day => {
                        const sel = day.selectedFood[i];
                        for (const fid in sel) groceryItems[fid] = (groceryItems[fid] || 0) + sel[fid];
                    });
                    
                    grocerySummaryGrid.innerHTML += `
                        <div class="bento-card p-6">
                            <h4 class="font-black text-sm mb-4 uppercase">Épicerie : ${p.name}</h4>
                            <div class="space-y-1">
                                ${Object.keys(groceryItems).map(fid => {
                                    const fr = FOOD_DATABASE.find(f => f.id === fid);
                                    return (fr && groceryItems[fid] > 0) ? `<div class="flex justify-between text-xs"><span>${fr.label}</span><span class="font-bold">x${groceryItems[fid]}</span></div>` : '';
                                }).join('')}
                                <div class="pt-2 mt-2 border-t font-black text-xs flex justify-between"><span>TOTAL NOURRITURE</span><span>${(totalFoodGrams/1000).toFixed(2)} kg</span></div>
                            </div>
                        </div>`;

                    equipmentSummaryGrid.innerHTML += `
                        <div class="bento-card p-6">
                            <h4 class="font-black text-sm mb-4 uppercase">Matériel : ${p.name} (Fixe)</h4>
                            <div class="space-y-1">
                                ${Object.keys(globalEquip).map(eid => {
                                    const e = EQUIPMENT_DATABASE.find(item => item.id === eid);
                                    return (e && (globalEquip[eid] || 0) > 0) ? `<div class="flex justify-between text-xs"><span>${e.label} (x${globalEquip[eid]})</span><span class="font-bold text-slate-400">${((e.weight*globalEquip[eid])/1000).toFixed(2)}kg</span></div>` : '';
                                }).join('')}
                                <div class="pt-2 mt-2 border-t font-black text-xs flex justify-between text-orange-600"><span>BASE SAC (Matériel)</span><span>${equipWeightKg.toFixed(2)} kg</span></div>
                            </div>
                        </div>`;
                } else if (i === currentProfileIdx) {
                    localPerformanceDiv.innerHTML = resultHTML;
                }
            });
        }

        function exportData() {
            const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify({profiles, days, FOOD_DATABASE, EQUIPMENT_DATABASE, totalDays: document.getElementById('total-days').value}));
            const dl = document.createElement('a');
            dl.setAttribute("href", dataStr);
            dl.setAttribute("download", "expedition_plan_shared.json");
            dl.click();
        }

        function importData(event) {
            const reader = new FileReader();
            reader.onload = (e) => {
                const imported = JSON.parse(e.target.result);
                profiles = imported.profiles; 
                profiles.forEach(p => { if(!p.globalEquipment) p.globalEquipment = {}; });
                days = imported.days;
                FOOD_DATABASE = imported.FOOD_DATABASE || [];
                EQUIPMENT_DATABASE = imported.EQUIPMENT_DATABASE || [];
                document.getElementById('total-days').value = imported.totalDays;
                init();
            };
            reader.readAsText(event.target.files[0]);
        }

        window.onload = init;
    </script>
</body>
</html>
