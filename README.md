<!DOCTYPE html>
<html lang="fr" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="mobile-web-app-capable" content="yes">
    
    <title>Trek Companion</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = { darkMode: 'class' }
    </script>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        #map { height: calc(100vh - 340px); min-height: 220px; width: 100%; z-index: 10; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        #elevationChart { max-height: 140px; width: 100% !important; }
        /* Empêcher la sélection de texte et les gestes parasites en mode app */
        body { user-select: none; -webkit-tap-highlight-color: transparent; }
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
                <h3 class="text-lg font-semibold text-green-400 mt-6 mb-2">Nourriture Personnelle</h3>
                <ul id="prep-food-list" class="space-y-2 bg-gray-800 p-3 rounded-lg"></ul>
            </div>
        </section>

        <section id="tab-courses" class="tab-content">
            <h2 class="text-2xl font-bold mb-4 border-b border-gray-700 pb-2">Liste de Courses</h2>
            <div class="flex space-x-2 mb-4">
                <button onclick="setCourseMode('group')" id="btn-course-group" class="flex-1 bg-green-600 text-white py-2 rounded font-semibold transition">Groupe</button>
                <button onclick="setCourseMode('indiv')" id="btn-course-indiv" class="flex-1 bg-gray-700 text-gray-300 py-2 rounded font-semibold transition">Individuel</button>
            </div>
            <select id="course-profile-select" class="w-full bg-gray-800 border border-gray-600 text-white rounded p-2 mb-4 hidden" onchange="renderCourses()"></select>
            <ul id="courses-list" class="space-y-2 bg-gray-800 p-3 rounded-lg mt-2"></ul>
        </section>

        <section id="tab-gps" class="tab-content">
            <h2 class="text-2xl font-bold mb-2 border-b border-gray-700 pb-2">Navigation</h2>
            <div class="flex space-x-2 mb-3">
                <select id="gps-day-select" class="flex-1 bg-gray-800 border border-gray-600 text-white rounded p-2" onchange="loadMapDay()"></select>
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
            <div id="map" class="rounded-lg border border-gray-600 bg-gray-800"></div>
            <p id="gps-status" class="text-xs text-gray-400 mt-2 text-center"></p>
            <div class="bg-gray-800 rounded-lg border border-gray-700 mt-3 p-3">
                <div class="text-xs text-gray-400 uppercase font-semibold mb-2">Profil d'altitude</div>
                <div style="position:relative; height:140px;"><canvas id="elevationChart"></canvas></div>
            </div>
        </section>
    </main>

    <nav class="bg-gray-800 fixed bottom-0 w-full border-t border-gray-700 flex justify-around p-2 pb-safe z-50">
        <button onclick="switchTab('import')" class="flex flex-col items-center p-2 text-gray-400">
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12"></path></svg>
            <span class="text-xs mt-1">Données</span>
        </button>
        <button onclick="switchTab('prep')" class="flex flex-col items-center p-2 text-gray-400">
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-6 9l2 2 4-4"></path></svg>
            <span class="text-xs mt-1">Prép</span>
        </button>
        <button onclick="switchTab('courses')" class="flex flex-col items-center p-2 text-gray-400">
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"></path></svg>
            <span class="text-xs mt-1">Courses</span>
        </button>
        <button onclick="switchTab('gps')" class="flex flex-col items-center p-2 text-gray-400">
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.242-4.243a8 8 0 1111.314 0z"></path></svg>
            <span class="text-xs mt-1">GPS</span>
        </button>
    </nav>

    <script>
        // --- LOGIQUE JAVASCRIPT ---
        let appData = null;
        let map = null;
        let polyline = null;
        let positionMarker = null;
        let watchId = null;
        let courseMode = 'group';
        let elevationChart = null;

        document.addEventListener('DOMContentLoaded', () => {
            const storedData = localStorage.getItem('trekData');
            if (storedData) {
                appData = JSON.parse(storedData);
                updateUIWithData();
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
                    showStatus('Fichier importé !', 'text-green-400');
                } catch (err) { showStatus('Erreur JSON', 'text-red-400'); }
            };
            reader.readAsText(file);
        });

        function clearData() {
            localStorage.removeItem('trekData');
            location.reload();
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
                setTimeout(() => { if (map) map.invalidateSize(); else initMap(); }, 100);
            }
        }

        function updateUIWithData() {
            if(!appData) return;
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

        // --- GPS LOGIQUE ---
        function initMap() {
            if (map) return;
            map = L.map('map').setView([48, -70], 12);
            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', { maxZoom: 18 }).addTo(map);
            loadMapDay();
        }

        function loadMapDay() {
            if(!appData || !map) return;
            const dayIdx = document.getElementById('gps-day-select').value;
            const day = appData.days[dayIdx];
            if (polyline) map.removeLayer(polyline);
            polyline = L.polyline(day.gpxPoints, {color: '#3b82f6', weight: 4}).addTo(map);
            map.fitBounds(polyline.getBounds());
            document.getElementById('gps-dist').innerText = `${day.dist.toFixed(1)} km`;
            document.getElementById('gps-elev').innerText = `${day.elevPos} m`;
            renderElevationChart(day.chartData);
        }

        function toggleTracking() {
            const btn = document.getElementById('btn-track');
            const statusEl = document.getElementById('gps-status');

            if (watchId) {
                navigator.geolocation.clearWatch(watchId);
                watchId = null;
                btn.innerText = "Démarrer";
                btn.className = "bg-blue-600 px-4 py-2 rounded font-bold text-white shadow";
                statusEl.innerText = "GPS arrêté.";
                return;
            }

            // Sécurité contextuelle
            if (window.location.protocol === 'file:') {
                statusEl.innerText = "GPS bloqué en 'file://'. Hébergez le fichier (ex: GitHub Pages).";
                statusEl.className = "text-xs text-red-400 mt-2 font-bold";
                return;
            }

            btn.innerText = "Arrêter";
            btn.className = "bg-red-600 px-4 py-2 rounded font-bold text-white shadow";
            statusEl.innerText = "Recherche satellite...";

            watchId = navigator.geolocation.watchPosition(
                (pos) => updatePosition(pos.coords.latitude, pos.coords.longitude),
                (err) => {
                    statusEl.innerText = "Erreur : " + (err.code === 1 ? "Permission refusée" : "Signal faible");
                    statusEl.className = "text-xs text-red-400 mt-2 font-bold";
                },
                { enableHighAccuracy: true, timeout: 15000, maximumAge: 0 }
            );
        }

        function updatePosition(lat, lon) {
            document.getElementById('gps-status').innerText = "Signal actif";
            if (!positionMarker) {
                positionMarker = L.circleMarker([lat, lon], { radius: 8, fillColor: "#3b82f6", color: "#fff", fillOpacity: 1 }).addTo(map);
            } else {
                positionMarker.setLatLng([lat, lon]);
            }
            map.panTo([lat, lon]);
            calculateProgress(lat, lon);
        }

        // Fonctions utilitaires identiques (Courses, Elevation, Progress...)
        function getDistanceFromLatLonInKm(lat1, lon1, lat2, lon2) {
            const R = 6371;
            const dLat = (lat2-lat1) * Math.PI/180;
            const dLon = (lon2-lon1) * Math.PI/180;
            const a = Math.sin(dLat/2) * Math.sin(dLat/2) + Math.cos(lat1*Math.PI/180) * Math.cos(lat2*Math.PI/180) * Math.sin(dLon/2) * Math.sin(dLon/2);
            return R * 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));
        }

        function calculateProgress(curLat, curLon) {
            const dayIdx = document.getElementById('gps-day-select').value;
            const day = appData.days[dayIdx];
            const pts = day.gpxPoints;
            let minDist = Infinity; let closestIdx = 0;
            pts.forEach((pt, idx) => {
                let d = getDistanceFromLatLonInKm(curLat, curLon, pt[0], pt[1]);
                if (d < minDist) { minDist = d; closestIdx = idx; }
            });
            let distRemaining = 0;
            for(let i = closestIdx; i < pts.length - 1; i++) {
                distRemaining += getDistanceFromLatLonInKm(pts[i][0], pts[i][1], pts[i+1][0], pts[i+1][1]);
            }
            document.getElementById('gps-dist').innerText = `${distRemaining.toFixed(2)} km`;
            
            // Update Chart point
            if (elevationChart && minDist < 1) {
                const chartX = day.chartData[Math.floor((closestIdx / pts.length) * day.chartData.length)].x;
                const chartY = day.chartData[Math.floor((closestIdx / pts.length) * day.chartData.length)].y;
                elevationChart.data.datasets[1].data = [{x: chartX, y: chartY}];
                elevationChart.update('none');
            }
        }

        function renderElevationChart(data) {
            const ctx = document.getElementById('elevationChart').getContext('2d');
            if (elevationChart) elevationChart.destroy();
            elevationChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: data.map(d => d.x),
                    datasets: [{
                        data: data.map(d => d.y),
                        borderColor: '#10b981', fill: true, backgroundColor: 'rgba(16, 185, 129, 0.2)', pointRadius: 0
                    }, {
                        data: [], pointRadius: 6, pointBackgroundColor: '#3b82f6', showLine: false
                    }]
                },
                options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } }, scales: { x: { display: false }, y: { ticks: { color: '#9ca3af', font: { size: 9 } } } } }
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

        function renderPrep() {
            const idx = document.getElementById('prep-profile-select').value;
            if(idx === "") return;
            document.getElementById('prep-content').classList.remove('hidden');
            const profile = appData.profiles[idx];
            const equipList = document.getElementById('prep-equip-list');
            equipList.innerHTML = '';
            appData.EQUIPMENT_DATABASE.forEach(eq => {
                if (eq.assignedTo == idx || (profile.globalEquipment && profile.globalEquipment[eq.id])) {
                    equipList.innerHTML += `<li class="flex items-center text-sm"><input type="checkbox" class="mr-2"> ${eq.label}</li>`;
                }
            });
        }

        function setCourseMode(mode) {
            courseMode = mode;
            document.getElementById('course-profile-select').classList.toggle('hidden', mode === 'group');
            renderCourses();
        }

        function renderCourses() {
            if(!appData) return;
            const idx = courseMode === 'indiv' ? parseInt(document.getElementById('course-profile-select').value) : null;
            const totals = getFoodTotals(idx);
            const listEl = document.getElementById('courses-list');
            listEl.innerHTML = '';
            Object.entries(totals).forEach(([id, q]) => {
                const f = appData.FOOD_DATABASE.find(x => x.id === id);
                listEl.innerHTML += `<li class="flex justify-between text-sm border-b border-gray-700 py-1"><span>${f ? f.label : id}</span><span class="font-bold">x${q}</span></li>`;
            });
        }
    </script>
</body>
</html>
