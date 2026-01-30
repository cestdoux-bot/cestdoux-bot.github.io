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
        input[type="number"], input[type="text"], select {
            border: 1px solid #e2e8f0;
            border-radius: 0.5rem;
            padding: 0.4rem;
            width: 100%;
            font-size: 0.875rem;
        }
        .profile-badge {
            cursor: pointer;
            transition: all 0.2s;
        }
        .profile-badge.active {
            background-color: #2d5a27;
            color: white;
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
        <header class="mb-8 text-center">
            <h1 class="text-3xl font-bold text-green-900">Calculateur de Randonnée Expert</h1>
            <p class="text-sm text-gray-600 mt-2 italic">Moteur bioénergétique basé sur les équations de Pandolf & Harris-Benedict</p>
            <div class="mt-4 flex items-center justify-center gap-4">
                <label class="text-gray-700 font-medium">Nombre de jours :</label>
                <input type="number" id="total-days" onchange="updateDayCount()" value="5" min="1" max="14" class="w-20 text-center">
            </div>
        </header>

        <!-- SECTION PROFILS -->
        <section class="card p-6 mb-8">
            <h2 class="text-xl font-semibold mb-4 flex items-center gap-2">👤 Profils des Randonneurs</h2>
            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="switchProfile(0)" id="btn-p0" class="profile-badge active p-3 rounded-lg border text-center font-bold">Cédrik</button>
                <button onclick="switchProfile(1)" id="btn-p1" class="profile-badge p-3 rounded-lg border text-center font-bold">P-A</button>
            </div>

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
                    <input type="number" id="p-age" oninput="updateProfileData()" placeholder="ans">
                </div>
                <div>
                    <label class="block text-[10px] font-bold text-gray-500 uppercase">Taille (cm)</label>
                    <input type="number" id="p-height" oninput="updateProfileData()" placeholder="cm">
                </div>
                <div>
                    <label class="block text-[10px] font-bold text-gray-500 uppercase">Poids (kg)</label>
                    <input type="number" id="p-weight" oninput="updateProfileData()" placeholder="kg">
                </div>
                <div>
                    <label class="block text-[10px] font-bold text-gray-500 uppercase">Niveau (1-10)</label>
                    <input type="number" id="p-level" oninput="updateProfileData()" min="1" max="10">
                </div>
            </div>
        </section>

        <!-- SECTION ÉTAPE DU JOUR -->
        <section class="card p-6 mb-8">
            <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-4">
                <h2 class="text-xl font-semibold flex items-center gap-2">🏔️ Itinéraire & Étapes</h2>
                <div class="text-xs bg-yellow-100 text-yellow-800 px-3 py-1 rounded-full border border-yellow-200">
                    Sélectionnez les aliments et ajustez les quantités pour chaque jour.
                </div>
            </div>
            
            <div id="day-tabs" class="flex overflow-x-auto gap-2 mb-6 pb-2 border-b">
                <!-- Généré par JS -->
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <div class="space-y-4">
                    <h3 class="font-bold text-green-800 border-b pb-1">Détails de l'étape</h3>
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label class="block text-xs font-medium">Distance (km)</label>
                            <input type="number" id="d-dist" oninput="saveCurrentDay(); calculate()" value="15">
                        </div>
                        <div>
                            <label class="block text-xs font-medium">Temps de marche (h)</label>
                            <input type="number" id="d-time" oninput="saveCurrentDay(); calculate()" step="0.5" value="5">
                            <span class="text-[9px] text-gray-400 italic">Durée réelle de l'effort</span>
                        </div>
                    </div>
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label class="block text-xs text-green-700 font-medium">Dénivelé Positif (m)</label>
                            <input type="number" id="d-elev-pos" oninput="saveCurrentDay(); calculate()" value="800">
                        </div>
                        <div>
                            <label class="block text-xs text-blue-700 font-medium">Dénivelé Négatif (m)</label>
                            <input type="number" id="d-elev-neg" oninput="saveCurrentDay(); calculate()" value="800">
                        </div>
                    </div>
                    <div>
                        <label class="block text-xs font-medium">Coefficient de terrain ($\eta$)</label>
                        <select id="d-terrain" onchange="saveCurrentDay(); calculate()">
                            <option value="1.0">Route / Sentier lisse (1.0)</option>
                            <option value="1.2" selected>Sentier de terre / Mixte (1.2)</option>
                            <option value="1.5">Herbe haute / Broussailles (1.5)</option>
                            <option value="2.1">Sable mou / Éboulis (2.1)</option>
                        </select>
                    </div>
                </div>

                <div class="space-y-4 bg-green-50 p-4 rounded-lg">
                    <h3 class="font-bold text-green-800 border-b pb-1">Charges du jour (Sac hors nourriture)</h3>
                    <div>
                        <label class="block text-sm" id="label-load-0">Sac Cédrik (kg)</label>
                        <input type="number" id="d-load-0" oninput="saveCurrentDay(); calculate()" value="10">
                    </div>
                    <div>
                        <label class="block text-sm" id="label-load-1">Sac P-A (kg)</label>
                        <input type="number" id="d-load-1" oninput="saveCurrentDay(); calculate()" value="8">
                    </div>
                </div>
            </div>
        </section>

        <!-- NOURRITURE INDIVIDUELLE -->
        <section class="space-y-8 mb-8">
            <div class="card p-6 border-l-4 border-green-700 overflow-x-auto">
                <div class="flex justify-between items-center mb-4">
                    <h3 class="text-lg font-bold text-green-900">🍎 Garde-manger : <span id="food-title-0">Cédrik</span></h3>
                    <button onclick="addFoodItem(0)" class="bg-green-700 text-white px-3 py-1 rounded text-sm hover:bg-green-800">+ Ajouter</button>
                </div>
                <div class="hidden md:grid nutri-grid text-[10px] font-bold text-gray-500 uppercase mb-2 px-1 text-center">
                    <div>Prévu</div>
                    <div>Qté</div>
                    <div class="text-left">Aliment</div>
                    <div>Kcal/u</div>
                    <div>Prot/u</div>
                    <div>Gluc/u</div>
                    <div>Lip/u</div>
                    <div>Sod/u</div>
                    <div>Fib/u</div>
                    <div></div>
                </div>
                <div id="food-list-0" class="space-y-2"></div>
            </div>

            <div class="card p-6 border-l-4 border-blue-600 overflow-x-auto">
                <div class="flex justify-between items-center mb-4">
                    <h3 class="text-lg font-bold text-blue-900">🍎 Garde-manger : <span id="food-title-1">P-A</span></h3>
                    <button onclick="addFoodItem(1)" class="bg-blue-600 text-white px-3 py-1 rounded text-sm hover:bg-blue-800">+ Ajouter</button>
                </div>
                <div class="hidden md:grid nutri-grid text-[10px] font-bold text-gray-500 uppercase mb-2 px-1 text-center">
                    <div>Prévu</div>
                    <div>Qté</div>
                    <div class="text-left">Aliment</div>
                    <div>Kcal/u</div>
                    <div>Prot/u</div>
                    <div>Gluc/u</div>
                    <div>Lip/u</div>
                    <div>Sod/u</div>
                    <div>Fib/u</div>
                    <div></div>
                </div>
                <div id="food-list-1" class="space-y-2"></div>
            </div>
        </section>

        <!-- RÉSULTATS -->
        <section id="results" class="grid grid-cols-1 md:grid-cols-2 gap-6 pb-10"></section>

    </div>

    <script>
        let currentProfileIdx = 0;
        let currentDayIdx = 0;

        const profiles = [
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
            loadProfileUI();
            renderFoodLists();
            calculate();
        }

        function updateDayCount() {
            const count = parseInt(document.getElementById('total-days').value) || 1;
            const currentLen = days.length;
            if (count > currentLen) {
                for (let i = currentLen; i < count; i++) {
                    // Chaque jour contient un objet "selectedFood" : mapping foodId -> quantité (0 si non sélectionné)
                    days.push({ 
                        dist: 15, time: 5, elevPos: 800, elevNeg: 800, terrain: 1.2, 
                        loads: [10, 8],
                        selectedFood: [{}, {}] 
                    });
                }
            } else if (count < currentLen) {
                days = days.slice(0, count);
                if (currentDayIdx >= count) currentDayIdx = count - 1;
            }
            renderDayTabs();
            loadDayData();
        }

        function renderDayTabs() {
            const container = document.getElementById('day-tabs');
            container.innerHTML = '';
            days.forEach((_, i) => {
                const btn = document.createElement('button');
                btn.innerText = `Jour ${i + 1}`;
                btn.className = `day-tab px-4 py-2 whitespace-nowrap ${currentDayIdx === i ? 'tab-active' : ''}`;
                btn.onclick = () => { saveCurrentDay(); currentDayIdx = i; renderDayTabs(); loadDayData(); renderFoodLists(); };
                container.appendChild(btn);
            });
        }

        function loadDayData() {
            const d = days[currentDayIdx];
            document.getElementById('d-dist').value = d.dist;
            document.getElementById('d-time').value = d.time;
            document.getElementById('d-elev-pos').value = d.elevPos;
            document.getElementById('d-elev-neg').value = d.elevNeg;
            document.getElementById('d-terrain').value = d.terrain;
            document.getElementById('d-load-0').value = d.loads[0];
            document.getElementById('d-load-1').value = d.loads[1];
            calculate();
        }

        function saveCurrentDay() {
            days[currentDayIdx].dist = parseFloat(document.getElementById('d-dist').value) || 0;
            days[currentDayIdx].time = parseFloat(document.getElementById('d-time').value) || 0;
            days[currentDayIdx].elevPos = parseFloat(document.getElementById('d-elev-pos').value) || 0;
            days[currentDayIdx].elevNeg = parseFloat(document.getElementById('d-elev-neg').value) || 0;
            days[currentDayIdx].terrain = parseFloat(document.getElementById('d-terrain').value) || 1;
            days[currentDayIdx].loads[0] = parseFloat(document.getElementById('d-load-0').value) || 0;
            days[currentDayIdx].loads[1] = parseFloat(document.getElementById('d-load-1').value) || 0;
        }

        function switchProfile(idx) {
            currentProfileIdx = idx;
            document.getElementById('btn-p0').classList.toggle('active', idx === 0);
            document.getElementById('btn-p1').classList.toggle('active', idx === 1);
            loadProfileUI();
        }

        function loadProfileUI() {
            const p = profiles[currentProfileIdx];
            document.getElementById('p-name').value = p.name;
            document.getElementById('p-gender').value = p.gender;
            document.getElementById('p-age').value = p.age;
            document.getElementById('p-height').value = p.height;
            document.getElementById('p-weight').value = p.weight;
            document.getElementById('p-level').value = p.level;
        }

        function updateProfileData() {
            const p = profiles[currentProfileIdx];
            p.name = document.getElementById('p-name').value;
            p.gender = document.getElementById('p-gender').value;
            p.age = parseInt(document.getElementById('p-age').value) || 30;
            p.height = parseInt(document.getElementById('p-height').value) || 170;
            p.weight = parseFloat(document.getElementById('p-weight').value) || 70;
            p.level = parseInt(document.getElementById('p-level').value) || 5;
            
            document.getElementById('btn-p0').innerText = profiles[0].name;
            document.getElementById('btn-p1').innerText = profiles[1].name;
            document.getElementById('label-load-0').innerText = `Sac ${profiles[0].name} (kg)`;
            document.getElementById('label-load-1').innerText = `Sac ${profiles[1].name} (kg)`;
            document.getElementById('food-title-0').innerText = profiles[0].name;
            document.getElementById('food-title-1').innerText = profiles[1].name;
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
            const currentDay = days[currentDayIdx];
            const selections = currentDay.selectedFood[pIdx];
            
            if (selections[fId] > 0) {
                delete selections[fId];
            } else {
                selections[fId] = 1;
            }
            renderFoodLists();
            calculate();
        }

        function updateFoodQuantity(pIdx, fId, qty) {
            const selections = days[currentDayIdx].selectedFood[pIdx];
            const val = parseInt(qty);
            if (val > 0) {
                selections[fId] = val;
            } else {
                delete selections[fId];
            }
            renderFoodLists();
            calculate();
        }

        function removeFoodItem(pIdx, fIdx) {
            const fId = profiles[pIdx].food[fIdx].id;
            profiles[pIdx].food.splice(fIdx, 1);
            days.forEach(day => {
                delete day.selectedFood[pIdx][fId];
            });
            renderFoodLists();
            calculate();
        }

        function renderFoodLists() {
            const currentDay = days[currentDayIdx];
            [0, 1].forEach(pIdx => {
                const list = document.getElementById(`food-list-${pIdx}`);
                list.innerHTML = '';
                profiles[pIdx].food.forEach((item, fIdx) => {
                    const selections = currentDay.selectedFood[pIdx];
                    const qty = selections[item.id] || 0;
                    const isSelected = qty > 0;
                    
                    const div = document.createElement('div');
                    div.className = `nutri-grid bg-white p-1 rounded border border-gray-100 hover:border-gray-300 transition-colors ${isSelected ? 'item-selected' : ''}`;
                    div.innerHTML = `
                        <div class="flex justify-center">
                            <input type="checkbox" ${isSelected ? 'checked' : ''} onchange="toggleFoodSelection(${pIdx}, '${item.id}')" class="w-5 h-5 accent-green-700 cursor-pointer">
                        </div>
                        <div>
                            <input type="number" value="${qty}" min="0" oninput="updateFoodQuantity(${pIdx}, '${item.id}', this.value)" class="text-center bg-transparent" placeholder="0">
                        </div>
                        <input type="text" value="${item.label}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'label', this.value)" placeholder="Nom">
                        <input type="number" value="${item.kcal}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'kcal', this.value)" placeholder="kcal">
                        <input type="number" value="${item.prot}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'prot', this.value)" placeholder="P">
                        <input type="number" value="${item.gluc}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'gluc', this.value)" placeholder="G">
                        <input type="number" value="${item.lip}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'lip', this.value)" placeholder="L">
                        <input type="number" value="${item.sod}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'sod', this.value)" placeholder="Na">
                        <input type="number" value="${item.fib}" oninput="updateFoodItem(${pIdx}, ${fIdx}, 'fib', this.value)" placeholder="Fi">
                        <button onclick="removeFoodItem(${pIdx}, ${fIdx})" class="text-red-500 font-bold px-2 hover:bg-red-50 rounded">×</button>
                    `;
                    list.appendChild(div);
                });
            });
        }

        function calculate() {
            const d = days[currentDayIdx];
            const resultsDiv = document.getElementById('results');
            resultsDiv.innerHTML = '';

            profiles.forEach((p, i) => {
                const load = d.loads[i];
                const W = p.weight;
                const L = load;
                
                // --- 1. MÉTABOLISME DE BASE (BMR) ---
                let bmrDay = 0;
                if (p.gender === "M") {
                    bmrDay = 88.362 + (13.397 * W) + (4.799 * p.height) - (5.677 * p.age);
                } else {
                    bmrDay = 447.593 + (9.247 * W) + (3.098 * p.height) - (4.330 * p.age);
                }
                
                // --- 2. COÛT DE LA MARCHE (PANDOLF MODIFIÉ) ---
                const timeSec = (d.time || 1) * 3600;
                const distMeters = d.dist * 1000;
                const V = distMeters / timeSec; 
                const grade = (d.elevPos / distMeters) * 100; 
                const eta = d.terrain;

                const loadTerm = 2.0 * (W + L) * Math.pow(L / W, 2);
                let metabolicPowerWatts = 1.5 * W + loadTerm + eta * (W + L) * (1.5 * Math.pow(V, 2) + 0.35 * V * grade);
                const kcalPerHourMovement = metabolicPowerWatts * 0.860;
                
                // --- 3. DÉPENSE TOTALE ---
                const restHours = 24 - (d.time || 1);
                const bmrPerHour = bmrDay / 24;
                const totalNeeded = (bmrPerHour * restHours) + (kcalPerHourMovement * (d.time || 1));

                // --- 4. NUTRITION (Somme des items * quantités pour le jour) ---
                const daySelections = d.selectedFood[i];
                const totals = p.food.reduce((acc, cur) => {
                    const qty = daySelections[cur.id] || 0;
                    if (qty > 0) {
                        acc.kcal += cur.kcal * qty; 
                        acc.prot += cur.prot * qty; 
                        acc.gluc += cur.gluc * qty; 
                        acc.lip += cur.lip * qty; 
                        acc.sod += cur.sod * qty; 
                        acc.fib += cur.fib * qty;
                    }
                    return acc;
                }, { kcal: 0, prot: 0, gluc: 0, lip: 0, sod: 0, fib: 0 });

                const balance = totals.kcal - totalNeeded;
                const wattsPerKg = metabolicPowerWatts / W;
                const intensity = Math.min(100, (wattsPerKg / (p.level * 1.2)) * 100);

                resultsDiv.innerHTML += `
                    <div class="card p-6 border-t-4 ${i === 0 ? 'border-green-600' : 'border-blue-500'}">
                        <div class="flex justify-between items-start mb-4">
                            <div>
                                <h3 class="text-xl font-bold">${p.name} <span class="text-xs font-normal text-gray-400">(${p.gender})</span></h3>
                                <p class="text-[10px] text-gray-500 uppercase font-bold tracking-tight">Moteur Pandolf + H.B.</p>
                            </div>
                            <span class="text-xs px-2 py-1 bg-gray-100 rounded">Jour ${currentDayIdx + 1}</span>
                        </div>
                        
                        <div class="grid grid-cols-2 gap-4 mb-4 text-center">
                            <div class="bg-gray-50 p-2 rounded">
                                <span class="block text-[10px] uppercase text-gray-500 font-bold">Besoins (24h)</span>
                                <span class="font-bold text-lg">${Math.round(totalNeeded)}</span> <span class="text-xs">kcal</span>
                            </div>
                            <div class="bg-gray-50 p-2 rounded">
                                <span class="block text-[10px] uppercase text-gray-500 font-bold">Apports (${Object.values(daySelections).reduce((a, b) => a + b, 0)} items)</span>
                                <span class="font-bold text-lg text-blue-600">${Math.round(totals.kcal)}</span> <span class="text-xs">kcal</span>
                            </div>
                        </div>

                        <div class="grid grid-cols-3 gap-2 mb-4 text-[10px] text-center">
                            <div class="border rounded p-1"><strong>PROT:</strong> ${Math.round(totals.prot)}g</div>
                            <div class="border rounded p-1"><strong>GLUC:</strong> ${Math.round(totals.gluc)}g</div>
                            <div class="border rounded p-1"><strong>LIP:</strong> ${Math.round(totals.lip)}g</div>
                            <div class="border rounded p-1"><strong>SOD:</strong> ${Math.round(totals.sod)}mg</div>
                            <div class="border rounded p-1"><strong>FIB:</strong> ${Math.round(totals.fib)}g</div>
                            <div class="border rounded p-1 ${balance >= 0 ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800'} font-bold">
                                BAL: ${Math.round(balance)}
                            </div>
                        </div>

                        <div>
                            <div class="flex justify-between text-[10px] mb-1">
                                <span>Indice de charge physiologique</span>
                                <span>${Math.round(intensity)}%</span>
                            </div>
                            <div class="w-full bg-gray-200 rounded-full h-1.5">
                                <div class="h-1.5 rounded-full ${intensity > 85 ? 'bg-red-500' : intensity > 60 ? 'bg-orange-400' : 'bg-green-600'}" style="width: ${intensity}%"></div>
                            </div>
                            <p class="text-[9px] text-gray-400 mt-2 italic text-center">
                                Vitesse: ${(V*3.6).toFixed(1)} km/h | Pente: ${grade.toFixed(1)}% | Puissance: ${Math.round(metabolicPowerWatts)}W
                            </p>
                        </div>
                    </div>
                `;
            });
        }

        window.onload = init;
    </script>
</body>
</html>
