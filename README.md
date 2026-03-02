<!DOCTYPE html>
<html lang="fr" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Trek Companion</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = { darkMode: 'class' }
    </script>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        /* S'assurer que la carte prenne tout l'espace disponible */
        #map { height: calc(100vh - 340px); min-height: 220px; width: 100%; z-index: 10; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        #elevationChart { max-height: 140px; width: 100% !important; }
    </style>
</head>
<body class="bg-gray-900 text-gray-100 font-sans antialiased pb-20">

    <header class="bg-gray-800 p-4 shadow-md sticky top-0 z-50">
        <h1 class="text-xl font-bold text-green-400 text-center">Trek Companion</h1>
    </header>

    <main class="p-4">
        
        <section id="tab-import" class="tab-content active">
            <h2 class="text-2xl font-bold mb-4 border-b border-gray-700 pb-2">Données</h2>
            <div class="bg-gray-800 p-4 rounded-lg shadow">
                <p class="mb-4 text-sm text-gray-300">Importez le fichier JSON exporté par le Planner.</p>
                <input type="file" id="fileInput" accept=".json" class="block w-full text-sm text-gray-400 file:mr-4 file:py-2 file:px-4 file:rounded file:border-0 file:text-sm file:font-semibold file:bg-green-600 file:text-white hover:file:bg-green-700 cursor-pointer"/>
                <div id="import-status" class="mt-4 text-sm hidden"></div>
            </div>
            <button onclick="clearData()" class="mt-6 w-full bg-red-600/20 text-red-400 py-2 rounded-lg border border-red-600/50 hover:bg-red-600/40">Supprimer les données locales</button>
        </section>

        <section id="tab-prep" class="tab-content">
            <h2 class="text-2xl font-bold mb-4 border-b border-gray-700 pb-2">Préparation</h2>
            <select id="prep-profile-select" class="w-full bg-gray-800 border border-gray-600 text-white rounded p-2 mb-4" onchange="renderPrep()">
                <option value="">Sélectionner un profil...</option>
            </select>
            
            <div id="prep-content" class="hidden">
                <h3 class="text-lg font-semibold text-blue-400 mt-4 mb-2">Équipement Assigné</h3>
                <ul id="prep-equip-list" class="space-y-2 bg-gray-800 p-3 rounded-lg"></ul>

                <h3 class="text-lg font-semibold text-green-400 mt-6 mb-2">Nourriture Personnelle (Total)</h3>
                <ul id="prep-food-list" class="space-y-2 bg-gray-800 p-3 rounded-lg"></ul>
            </div>
        </section>

        <section id="tab-courses" class="tab-content">
            <h2 class="text-2xl font-bold mb-4 border-b border-gray-700 pb-2">Liste de Courses</h2>
            <div class="flex space-x-2 mb-4">
                <button onclick="setCourseMode('group')" id="btn-course-group" class="flex-1 bg-green-600 text-white py-2 rounded font-semibold transition">Groupe</button>
                <button onclick="setCourseMode('indiv')" id="btn-course-indiv" class="flex-1 bg-gray-700 text-gray-300 py-2 rounded font-semibold transition">Individuel</button>
            </div>
            
            <select id="course-profile-select" class="w-full bg-gray-800 border border-gray-600 text-white rounded p-2 mb-4 hidden" onchange="renderCourses()">
            </select>

            <ul id="courses-list" class="space-y-2 bg-gray-800 p-3 rounded-lg mt-2"></ul>
        </section>

        <section id="tab-gps" class="tab-content">
            <h2 class="text-2xl font-bold mb-2 border-b border-gray-700 pb-2">Navigation</h2>
            
            <div class="flex space-x-2 mb-3">
                <select id="gps-day-select" class="flex-1 bg-gray-800 border border-gray-600 text-white rounded p-2" onchange="loadMapDay()">
                </select>
                <button id="btn-track" onclick="toggleTracking()" class="bg-blue-600 px-4 py-2 rounded font-bold text-white shadow">Démarrer</button>
            </div>

            <div class="grid grid-cols-2 gap-2 mb-3 text-center">
                <div class="bg-gray-800 p-2 rounded border border-gray-700">
                    <div class="text-xs text-gray-400 uppercase">Dist. Restante</div>
                    <div id="gps-dist" class="text-xl font-bold text-white">-- km</div>
                </div>
                <div class="bg-gray-800 p-2 rounded border border-gray-700">
                    <div class="text-xs text-gray-400 uppercase">D+ Restant</div>
                    <div id="gps-elev" class="text-xl font-bold text-green-400">-- m</div>
                </div>
            </div>

            <div id="map" class="rounded-lg border border-gray-600"></div>
            <p id="gps-status" class="text-xs text-gray-400 mt-2 text-center"></p>

            <div class="bg-gray-800 rounded-lg border border-gray-700 mt-3 p-3">
                <div class="text-xs text-gray-400 uppercase font-semibold mb-2 tracking-wide">Profil d'altitude</div>
                <div style="position:relative; height:140px;">
                    <canvas id="elevationChart"></canvas>
                </div>
            </div>
        </section>

    </main>

    <nav class="bg-gray-800 fixed bottom-0 w-full border-t border-gray-700 flex justify-around p-2 pb-safe z-50">
        <button onclick="switchTab('import')" class="flex flex-col items-center p-2 text-gray-400 hover:text-green-400">
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12"></path></svg>
            <span class="text-xs mt-1">Données</span>
        </button>
        <button onclick="switchTab('prep')" class="flex flex-col items-center p-2 text-gray-400 hover:text-green-400">
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-6 9l2 2 4-4"></path></svg>
            <span class="text-xs mt-1">Prép</span>
        </button>
        <button onclick="switchTab('courses')" class="flex flex-col items-center p-2 text-gray-400 hover:text-green-400">
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"></path></svg>
            <span class="text-xs mt-1">Courses</span>
        </button>
        <button onclick="switchTab('gps')" class="flex flex-col items-center p-2 text-gray-400 hover:text-green-400">
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.242-4.243a8 8 0 1111.314 0z"></path><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"></path></svg>
            <span class="text-xs mt-1">GPS</span>
        </button>
    </nav>

    <script>
        let appData = null;
        let map = null;
        let polyline = null;
        let positionMarker = null;
        let watchId = null;
        let courseMode = 'group'; // 'group' or 'indiv'
        let elevationChart = null; // Instance Chart.js

        // --- CORE & LOCALSTORAGE ---
        document.addEventListener('DOMContentLoaded', () => {
            const storedData = localStorage.getItem('trekData');
            if (storedData) {
                appData = JSON.parse(storedData);
                updateUIWithData();
                showStatus('Données chargées depuis le stockage local.', 'text-green-400');
            }
        });

        document.getElementById('fileInput').addEventListener('change', function(e) {
            const file = e.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = function(e) {
                try {
                    appData = JSON.parse(e.target.result);
                    localStorage.setItem('trekData', JSON.stringify(appData));
                    updateUIWithData();
                    showStatus('Fichier importé avec succès !', 'text-green-400');
                } catch (err) {
                    showStatus('Erreur de lecture du JSON.', 'text-red-400');
                }
            };
            reader.readAsText(file);
        });

        function clearData() {
            localStorage.removeItem('trekData');
            appData = null;
            showStatus('Données supprimées.', 'text-yellow-400');
            // reset UI
        }

        function showStatus(msg, colorClass) {
            const el = document.getElementById('import-status');
            el.className = `mt-4 text-sm ${colorClass} block`;
            el.innerText = msg;
        }

        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.getElementById('tab-' + tabId).classList.add('active');
            if (tabId === 'gps' && appData) {
                setTimeout(() => {
                    if (map) {
                        map.invalidateSize();
                    } else {
                        initMap();
                    }
                    // Redessiner le graphique si nécessaire
                    if (elevationChart) elevationChart.update();
                }, 100);
            }
        }

        function updateUIWithData() {
            if(!appData) return;
            // Populate selects
            const prepSel = document.getElementById('prep-profile-select');
            const courseSel = document.getElementById('course-profile-select');
            prepSel.innerHTML = '<option value="">Sélectionner un profil...</option>';
            courseSel.innerHTML = '';
            
            appData.profiles.forEach((p, idx) => {
                prepSel.innerHTML += `<option value="${idx}">${p.name}</option>`;
                courseSel.innerHTML += `<option value="${idx}">${p.name}</option>`;
            });

            const daySel = document.getElementById('gps-day-select');
            daySel.innerHTML = '';
            appData.days.forEach((d, idx) => {
                daySel.innerHTML += `<option value="${idx}">Étape ${idx + 1} (${d.dist}km)</option>`;
            });

            renderCourses();
        }

        // --- PREPARATION (Checklists) ---
        function renderPrep() {
            const idx = document.getElementById('prep-profile-select').value;
            const content = document.getElementById('prep-content');
            if(idx === "") { content.classList.add('hidden'); return; }
            content.classList.remove('hidden');

            const profile = appData.profiles[idx];
            
            // Équipement
            const equipList = document.getElementById('prep-equip-list');
            equipList.innerHTML = '';
            // On cherche l'équipement assigné à ce profil ET/OU présent dans son globalEquipment
            appData.EQUIPMENT_DATABASE.forEach(eq => {
                if (eq.assignedTo == idx || (profile.globalEquipment && profile.globalEquipment[eq.id])) {
                    let qty = profile.globalEquipment ? (profile.globalEquipment[eq.id] || eq.qty) : eq.qty;
                    equipList.innerHTML += `
                        <li class="flex items-center">
                            <input type="checkbox" class="w-5 h-5 mr-3 accent-green-500 rounded bg-gray-700 border-gray-600">
                            <span class="text-gray-200">${eq.label} <span class="text-gray-500 text-sm">x${qty} (${eq.weight * qty}g)</span></span>
                        </li>`;
                }
            });

            // Nourriture (Somme de tous les jours pour ce profil)
            const foodTotals = getFoodTotals(parseInt(idx));
            const foodList = document.getElementById('prep-food-list');
            foodList.innerHTML = '';
            
            Object.entries(foodTotals).forEach(([foodId, qty]) => {
                const fDb = appData.FOOD_DATABASE.find(f => f.id === foodId);
                const label = fDb ? fDb.label : foodId;
                const weight = fDb ? (fDb.weight * qty) : 0;
                foodList.innerHTML += `
                    <li class="flex items-center">
                        <input type="checkbox" class="w-5 h-5 mr-3 accent-green-500 rounded bg-gray-700 border-gray-600">
                        <span class="text-gray-200">${label} <span class="text-gray-500 text-sm">x${qty} (${weight}g)</span></span>
                    </li>`;
            });
        }

        // --- COURSES ---
        function setCourseMode(mode) {
            courseMode = mode;
            document.getElementById('btn-course-group').className = mode === 'group' ? 'flex-1 bg-green-600 text-white py-2 rounded font-semibold transition' : 'flex-1 bg-gray-700 text-gray-300 py-2 rounded font-semibold transition';
            document.getElementById('btn-course-indiv').className = mode === 'indiv' ? 'flex-1 bg-green-600 text-white py-2 rounded font-semibold transition' : 'flex-1 bg-gray-700 text-gray-300 py-2 rounded font-semibold transition';
            
            document.getElementById('course-profile-select').style.display = mode === 'indiv' ? 'block' : 'none';
            renderCourses();
        }

        function renderCourses() {
            if(!appData) return;
            let targetIdx = null;
            if (courseMode === 'indiv') {
                targetIdx = parseInt(document.getElementById('course-profile-select').value);
                if(isNaN(targetIdx)) return;
            }

            const totals = getFoodTotals(targetIdx);
            const listEl = document.getElementById('courses-list');
            listEl.innerHTML = '';

            // Map IDs to Database info and sort alphabetically
            let displayList = [];
            for (const [foodId, qty] of Object.entries(totals)) {
                const fDb = appData.FOOD_DATABASE.find(f => f.id === foodId);
                displayList.push({
                    label: fDb ? fDb.label : foodId,
                    qty: qty,
                    weight: fDb ? (fDb.weight * qty) : 0
                });
            }
            displayList.sort((a, b) => a.label.localeCompare(b.label));

            displayList.forEach(item => {
                listEl.innerHTML += `
                    <li class="flex justify-between items-center border-b border-gray-700 py-2 last:border-0">
                        <span class="text-gray-200">${item.label}</span>
                        <span class="text-green-400 font-bold">x${item.qty} <span class="text-gray-500 text-xs font-normal">(${item.weight/1000} kg)</span></span>
                    </li>`;
            });
        }

        function getFoodTotals(forProfileIdx = null) {
            const totals = {};
            appData.days.forEach(day => {
                if(!day.selectedFood) return;
                day.selectedFood.forEach((profFood, idx) => {
                    if (forProfileIdx !== null && idx !== forProfileIdx) return;
                    for (const [foodId, qty] of Object.entries(profFood)) {
                        totals[foodId] = (totals[foodId] || 0) + qty;
                    }
                });
            });
            return totals;
        }

        // --- GPS & CARTOGRAPHIE ---
        function initMap() {
            if (map) return;
            map = L.map('map').setView([48.26, -69.94], 12);
            
            // Tuiles OpenStreetMap standards (Nécessite connexion au 1er chargement)
            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                maxZoom: 19,
                attribution: '© OpenStreetMap'
            }).addTo(map);

            loadMapDay();
        }

        function loadMapDay() {
            if(!appData || !map) return;
            const dayIdx = document.getElementById('gps-day-select').value;
            const day = appData.days[dayIdx];
            
            if (polyline) map.removeLayer(polyline);
            
            // Inverser [lat, lon] du JSON si nécessaire, Leaflet attend [lat, lon]
            const pts = day.gpxPoints; 
            polyline = L.polyline(pts, {color: '#3b82f6', weight: 4, opacity: 0.8}).addTo(map);
            map.fitBounds(polyline.getBounds());

            // Reset dashboard
            document.getElementById('gps-dist').innerText = `${day.dist.toFixed(1)} km`;
            document.getElementById('gps-elev').innerText = `${day.elevPos} m`;

            // Rendu du graphique d'élévation
            renderElevationChart(day.chartData);
        }

        // --- GRAPHIQUE D'ÉLÉVATION ---
        function renderElevationChart(data) {
            const canvas = document.getElementById('elevationChart');
            if (!canvas) return;
            const ctx = canvas.getContext('2d');

            // Détruire l'ancien graphique s'il existe
            if (elevationChart) {
                elevationChart.destroy();
                elevationChart = null;
            }

            if (!data || data.length === 0) return;

            // Dégradé de remplissage vert (style identique au fichier de référence)
            const gradient = ctx.createLinearGradient(0, 0, 0, 140);
            gradient.addColorStop(0, 'rgba(16, 185, 129, 0.4)');
            gradient.addColorStop(1, 'rgba(16, 185, 129, 0)');

            elevationChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: data.map(d => d.x),
                    datasets: [
                        {
                            // Dataset 1 : profil d'altitude
                            label: 'Altitude (m)',
                            data: data.map(d => d.y),
                            borderColor: '#10b981',
                            backgroundColor: gradient,
                            fill: true,
                            tension: 0.2,
                            pointRadius: 0,
                            pointHoverRadius: 4,
                            borderWidth: 2,
                            order: 2
                        },
                        {
                            // Dataset 2 : position GPS actuelle (point bleu unique)
                            label: 'Position',
                            data: [],           // Rempli dynamiquement par updatePosition()
                            borderColor: '#3b82f6',
                            backgroundColor: '#3b82f6',
                            pointRadius: 8,
                            pointHoverRadius: 10,
                            pointStyle: 'circle',
                            showLine: false,    // On ne relie pas les points
                            order: 1
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    animation: { duration: 300 },
                    plugins: {
                        legend: { display: false },
                        tooltip: {
                            mode: 'index',
                            intersect: false,
                            callbacks: {
                                title: (items) => `${items[0].label} km`
                            }
                        }
                    },
                    scales: {
                        x: {
                            type: 'linear',
                            display: true,
                            grid: { display: false },
                            ticks: {
                                color: '#9ca3af',
                                font: { size: 9 },
                                maxRotation: 0,
                                callback: (v) => `${v} km`
                            }
                        },
                        y: {
                            display: true,
                            grid: { color: 'rgba(75, 85, 99, 0.3)' },
                            ticks: {
                                color: '#9ca3af',
                                font: { size: 9 },
                                callback: (v) => `${v} m`
                            }
                        }
                    }
                }
            });
        }

        /**
         * Met à jour le point bleu de position sur le graphique d'élévation.
         * Le point n'est affiché que si la distance au tracé est < 2 km.
         * @param {number} curLat - Latitude GPS actuelle
         * @param {number} curLon - Longitude GPS actuelle
         * @param {number} minDist - Distance au point le plus proche du tracé (km)
         * @param {number} closestChartIdx - Index dans chartData le plus proche de la position
         */
        function updateElevationChartPosition(curLat, curLon, minDist, closestChartIdx) {
            if (!elevationChart) return;

            const positionDataset = elevationChart.data.datasets[1];
            const chartData = elevationChart.data.labels; // tableau des x

            if (minDist < 2 && closestChartIdx !== null) {
                // On place un unique point bleu à la position correspondante sur la courbe
                const xVal = chartData[closestChartIdx];
                const yVal = elevationChart.data.datasets[0].data[closestChartIdx];
                positionDataset.data = [{ x: xVal, y: yVal }];
            } else {
                // On masque le point si on est trop loin du tracé
                positionDataset.data = [];
            }

            elevationChart.update('none'); // 'none' = pas d'animation pour fluidité
        }

        // Formule de Haversine pour distance GPS (en km)
        function getDistanceFromLatLonInKm(lat1, lon1, lat2, lon2) {
            const R = 6371; 
            const dLat = (lat2 - lat1) * (Math.PI/180);
            const dLon = (lon2 - lon1) * (Math.PI/180); 
            const a = 
                Math.sin(dLat/2) * Math.sin(dLat/2) +
                Math.cos(lat1 * (Math.PI/180)) * Math.cos(lat2 * (Math.PI/180)) * Math.sin(dLon/2) * Math.sin(dLon/2); 
            const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a)); 
            return R * c;
        }

        function toggleTracking() {
            const btn = document.getElementById('btn-track');
            const statusEl = document.getElementById('gps-status');

            if (watchId) {
                navigator.geolocation.clearWatch(watchId);
                watchId = null;
                btn.innerText = "Démarrer";
                btn.className = "bg-blue-600 px-4 py-2 rounded font-bold text-white shadow";
                statusEl.innerText = "Tracking arrêté.";
                statusEl.className = "text-xs text-gray-400 mt-2 text-center";
                return;
            }

            // 1. Détection explicite du contexte sécurisé (HTTPS obligatoire)
            if (!window.isSecureContext) {
                statusEl.innerText = "Erreur : La géolocalisation nécessite une connexion sécurisée (HTTPS).";
                statusEl.className = "text-xs text-red-400 mt-2 text-center font-bold";
                return;
            }

            // 2. Détection du support global du navigateur
            if (!navigator.geolocation) {
                statusEl.innerText = "Géolocalisation non supportée par votre navigateur.";
                statusEl.className = "text-xs text-red-400 mt-2 text-center font-bold";
                return;
            }

            btn.innerText = "Arrêter";
            btn.className = "bg-red-600 px-4 py-2 rounded font-bold text-white shadow";
            statusEl.innerText = "Recherche du signal GPS...";
            statusEl.className = "text-xs text-green-400 mt-2 text-center";

            watchId = navigator.geolocation.watchPosition(
                (pos) => updatePosition(pos.coords.latitude, pos.coords.longitude),
                (err) => { 
                    // Gestion des erreurs de permission refusée par l'utilisateur
                    if (err.code === err.PERMISSION_DENIED) {
                        statusEl.innerText = "Erreur : Accès au GPS refusé. Vérifiez vos permissions.";
                    } else {
                        statusEl.innerText = "Erreur GPS: " + err.message; 
                    }
                    statusEl.className = "text-xs text-red-400 mt-2 text-center font-bold";
                    
                    // Réinitialisation de l'interface en cas d'erreur
                    if (watchId) navigator.geolocation.clearWatch(watchId);
                    watchId = null;
                    btn.innerText = "Démarrer";
                    btn.className = "bg-blue-600 px-4 py-2 rounded font-bold text-white shadow";
                },
                { enableHighAccuracy: true, maximumAge: 10000, timeout: 5000 }
            );
        }

        function updatePosition(lat, lon) {
            document.getElementById('gps-status').innerText = "Signal GPS actif.";
            document.getElementById('gps-status').className = "text-xs text-green-400 mt-2 text-center";
            
            if (!positionMarker) {
                positionMarker = L.circleMarker([lat, lon], {
                    radius: 8, fillColor: "#3b82f6", color: "#ffffff", weight: 2, fillOpacity: 1
                }).addTo(map);
            } else {
                positionMarker.setLatLng([lat, lon]);
            }
            map.panTo([lat, lon]);

            calculateProgress(lat, lon);
        }

        function calculateProgress(curLat, curLon) {
            const dayIdx = document.getElementById('gps-day-select').value;
            const day = appData.days[dayIdx];
            const pts = day.gpxPoints;
            const chartData = day.chartData;

            // 1. Trouver le point le plus proche du tracé (gpxPoints)
            let minDist = Infinity;
            let closestIdx = 0;
            pts.forEach((pt, idx) => {
                let d = getDistanceFromLatLonInKm(curLat, curLon, pt[0], pt[1]);
                if (d < minDist) { minDist = d; closestIdx = idx; }
            });

            // 2. Calcul de la distance restante le long du tracé
            let distRemaining = 0;
            for(let i = closestIdx; i < pts.length - 1; i++) {
                distRemaining += getDistanceFromLatLonInKm(pts[i][0], pts[i][1], pts[i+1][0], pts[i+1][1]);
            }

            // 3. Calcul du D+ restant via chartData (Approximation par proximité)
            // On cherche l'index chartData correspondant au plus près de notre position
            let minChartDist = Infinity;
            let closestChartIdx = 0;
            chartData.forEach((cd, idx) => {
                let d = getDistanceFromLatLonInKm(curLat, curLon, cd.lat, cd.lon);
                if(d < minChartDist) { minChartDist = d; closestChartIdx = idx; }
            });

            let elevRemaining = 0;
            for(let i = closestChartIdx + 1; i < chartData.length; i++) {
                let diff = chartData[i].y - chartData[i-1].y;
                if(diff > 0) elevRemaining += diff; // On additionne seulement les montées
            }

            // Mise à jour de l'UI
            document.getElementById('gps-dist').innerText = `${distRemaining.toFixed(2)} km`;
            document.getElementById('gps-elev').innerText = `${Math.round(elevRemaining)} m`;

            // 4. Mettre à jour le point bleu sur le graphique d'élévation
            //    minDist correspond à la distance au tracé en gpxPoints ;
            //    on utilise minChartDist (distance au point chartData le plus proche)
            //    pour décider d'afficher ou non le point (seuil : 2 km).
            updateElevationChartPosition(curLat, curLon, minChartDist, closestChartIdx);
        }
    </script>
</body>
</html>
