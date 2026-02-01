<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculateur de Randonnée Expert</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        :root {
            --primary: #2d5a27;
            --secondary: #f4f1ea;
        }
        body {
            background-color: var(--secondary);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        .card {
            background: white;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.05);
            padding: 1.5rem;
            margin-bottom: 1.5rem;
        }
        .input-group label {
            display: block;
            font-size: 0.875rem;
            font-weight: 600;
            color: #4a5568;
            margin-bottom: 0.25rem;
        }
        input, select {
            width: 100%;
            padding: 0.5rem;
            border: 1px solid #e2e8f0;
            border-radius: 6px;
            margin-bottom: 1rem;
        }
        .grid-nutrition {
            display: grid;
            grid-template-columns: 2fr 1fr 1fr 1fr 1fr 0.5fr;
            gap: 0.5rem;
            align-items: center;
        }
        .header-nutrition {
            font-weight: bold;
            border-bottom: 2px solid #2d5a27;
            padding-bottom: 0.5rem;
            margin-bottom: 0.5rem;
        }
    </style>
</head>
<body class="p-4 md:p-8">
    <div class="max-w-5xl mx-auto">
        <header class="mb-8 text-center">
            <h1 class="text-3xl font-bold text-green-900">Planificateur de Randonnée Itinérante</h1>
            <p class="text-gray-600">Optimisation métabolique et logistique</p>
        </header>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <!-- Section 1: Paramètres Physiques -->
            <div class="card">
                <h2 class="text-xl font-bold mb-4 flex items-center">
                    <span class="mr-2">👤</span> Profil du Randonneur
                </h2>
                <div class="grid grid-cols-2 gap-4">
                    <div class="input-group">
                        <label>Poids du corps (kg)</label>
                        <input type="number" id="poidsCorps" value="75" oninput="calculerTout()">
                    </div>
                    <div class="input-group">
                        <label>Poids sac (hors nour.) (kg)</label>
                        <input type="number" id="poidsSacBase" value="10" oninput="calculerTout()">
                    </div>
                </div>
                <div class="bg-green-50 p-3 rounded-lg border border-green-200">
                    <p class="text-sm text-green-800">
                        <strong>Poids total estimé :</strong> <span id="poidsTotalSac">0</span> kg
                    </p>
                </div>
            </div>

            <!-- Section 2: Paramètres du Terrain -->
            <div class="card">
                <h2 class="text-xl font-bold mb-4 flex items-center">
                    <span class="mr-2">🏔️</span> Terrain & Effort
                </h2>
                <div class="grid grid-cols-2 gap-4">
                    <div class="input-group">
                        <label>Vitesse (km/h)</label>
                        <input type="number" id="vitesse" value="4" step="0.1" oninput="calculerTout()">
                    </div>
                    <div class="input-group">
                        <label>Pente (%)</label>
                        <input type="number" id="pente" value="0" oninput="calculerTout()">
                    </div>
                </div>
                <div class="input-group">
                    <label>Type de terrain (Facteur η)</label>
                    <select id="terrain" onchange="calculerTout()">
                        <option value="1.0">Route goudronnée (1.0)</option>
                        <option value="1.1">Chemin de terre (1.1)</option>
                        <option value="1.2" selected>Sentier de randonnée (1.2)</option>
                        <option value="1.5">Sable ou neige (1.5)</option>
                        <option value="2.1">Marécage / Terrain meuble (2.1)</option>
                    </select>
                </div>
            </div>
        </div>

        <!-- Section 3: Planification de la Nourriture -->
        <div class="card">
            <h2 class="text-xl font-bold mb-4 flex items-center">
                <span class="mr-2">🍎</span> Planification Nutritionnelle
            </h2>
            
            <div class="grid-nutrition header-nutrition hidden md:grid">
                <div>Aliment</div>
                <div class="text-center">Grammes (g)</div>
                <div class="text-center">Calories/100g</div>
                <div class="text-center">Prot. (g)</div>
                <div class="text-center">Gluc. (g)</div>
                <div></div>
            </div>

            <div id="listeNourriture" class="space-y-2 mb-4">
                <!-- Les lignes d'aliments seront insérées ici -->
            </div>

            <button onclick="ajouterLigneAliment()" class="bg-green-700 text-white px-4 py-2 rounded-lg hover:bg-green-800 transition">
                + Ajouter un aliment
            </button>

            <div class="mt-6 p-4 bg-gray-50 rounded-xl grid grid-cols-2 md:grid-cols-4 gap-4 text-center">
                <div>
                    <p class="text-xs text-gray-500 uppercase">Poids Nourriture Total</p>
                    <p class="text-xl font-bold" id="totalPoidsNourriture">0 g</p>
                </div>
                <div>
                    <p class="text-xs text-gray-500 uppercase">Énergie Totale</p>
                    <p class="text-xl font-bold text-orange-600" id="totalCalories">0 kcal</p>
                </div>
                <div>
                    <p class="text-xs text-gray-500 uppercase">Protéines</p>
                    <p class="text-xl font-bold" id="totalProteines">0 g</p>
                </div>
                <div>
                    <p class="text-xs text-gray-500 uppercase">Glucides</p>
                    <p class="text-xl font-bold" id="totalGlucides">0 g</p>
                </div>
            </div>
        </div>

        <!-- Résultats Finaux -->
        <div class="card bg-green-900 text-white">
            <h2 class="text-xl font-bold mb-4 text-green-200 underline">Bilan Énergétique (Equation de Pandolf & Santee)</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <div>
                    <p class="text-sm opacity-80 italic mb-2">Dépense horaire estimée pour l'effort :</p>
                    <p class="text-4xl font-black"><span id="depenseHoraire">0</span> kcal/h</p>
                </div>
                <div class="flex flex-col justify-center">
                    <p class="text-sm"><span class="font-bold">Ratio Énergie/Poids :</span> <span id="ratioEnergie">0</span> kcal/kg</p>
                    <p class="text-sm mt-1 text-green-300">Un ratio élevé (>450 kcal/100g) est préférable pour l'itinérance.</p>
                </div>
            </div>
        </div>
    </div>

    <script>
        // État initial de la nourriture
        let inventaire = [
            { nom: "Muesli / Avoine", grammes: 100, kcal100: 370, prot100: 10, gluc100: 60 },
            { nom: "Lyo : Pâtes Bolo", grammes: 120, kcal100: 420, prot100: 15, gluc100: 65 },
            { nom: "Mélange Noix/Fruits", grammes: 50, kcal100: 550, prot100: 12, gluc100: 30 }
        ];

        function ajouterLigneAliment() {
            inventaire.push({ nom: "", grammes: 0, kcal100: 0, prot100: 0, gluc100: 0 });
            afficherInventaire();
            calculerTout();
        }

        function supprimerAliment(index) {
            inventaire.splice(index, 1);
            afficherInventaire();
            calculerTout();
        }

        function mettreAJourAliment(index, champ, valeur) {
            inventaire[index][champ] = champ === 'nom' ? valeur : parseFloat(valeur) || 0;
            calculerTout();
        }

        function afficherInventaire() {
            const container = document.getElementById('listeNourriture');
            container.innerHTML = '';
            
            inventaire.forEach((item, index) => {
                const div = document.createElement('div');
                div.className = 'grid-nutrition bg-white border border-gray-100 p-1 rounded hover:bg-gray-50';
                div.innerHTML = `
                    <input type="text" placeholder="Nom" value="${item.nom}" oninput="mettreAJourAliment(${index}, 'nom', this.value)" class="mb-0">
                    <input type="number" placeholder="Grammes" value="${item.grammes}" oninput="mettreAJourAliment(${index}, 'grammes', this.value)" class="mb-0 text-center">
                    <input type="number" placeholder="Kcal/100g" value="${item.kcal100}" oninput="mettreAJourAliment(${index}, 'kcal100', this.value)" class="mb-0 text-center">
                    <input type="number" placeholder="Prot/100g" value="${item.prot100}" oninput="mettreAJourAliment(${index}, 'prot100', this.value)" class="mb-0 text-center">
                    <input type="number" placeholder="Gluc/100g" value="${item.gluc100}" oninput="mettreAJourAliment(${index}, 'gluc100', this.value)" class="mb-0 text-center">
                    <button onclick="supprimerAliment(${index})" class="text-red-500 font-bold hover:text-red-700">✕</button>
                `;
                container.appendChild(div);
            });
        }

        function calculerTout() {
            // 1. Calcul des totaux nutritionnels
            let totalG = 0;
            let totalKcal = 0;
            let totalProt = 0;
            let totalGluc = 0;

            inventaire.forEach(item => {
                const ratio = item.grammes / 100;
                totalG += item.grammes;
                totalKcal += item.kcal100 * ratio;
                totalProt += item.prot100 * ratio;
                totalGluc += item.gluc100 * ratio;
            });

            document.getElementById('totalPoidsNourriture').innerText = `${Math.round(totalG)} g`;
            document.getElementById('totalCalories').innerText = `${Math.round(totalKcal)} kcal`;
            document.getElementById('totalProteines').innerText = `${Math.round(totalProt)} g`;
            document.getElementById('totalGlucides').innerText = `${Math.round(totalGluc)} g`;

            // 2. Calcul du poids total du sac
            const poidsBaseSac = parseFloat(document.getElementById('poidsSacBase').value) || 0;
            const poidsNourritureKg = totalG / 1000;
            const poidsTotalSac = poidsBaseSac + poidsNourritureKg;
            document.getElementById('poidsTotalSac').innerText = poidsTotalSac.toFixed(2);

            // 3. Calcul métabolique (Pandolf)
            const W = parseFloat(document.getElementById('poidsCorps').value) || 75;
            const L = poidsTotalSac;
            const V = (parseFloat(document.getElementById('vitesse').value) || 4) / 3.6; // conversion en m/s
            const G = parseFloat(document.getElementById('pente').value) || 0;
            const eta = parseFloat(document.getElementById('terrain').value) || 1.2;

            // Formule de Pandolf : M = 1.5W + 2.0(W+L)(L/W)^2 + eta(W+L)[1.5V^2 + 0.35VG]
            // Note: C'est une simplification pour l'UI, Santee traite les pentes négatives.
            let M = 1.5 * W + 2.0 * (W + L) * Math.pow(L / W, 2) + eta * (W + L) * (1.5 * Math.pow(V, 2) + 0.35 * V * G);
            
            // Conversion Watts en kcal/h (1 Watt = 0.860 kcal/h)
            const kcalH = M * 0.860;
            document.getElementById('depenseHoraire').innerText = Math.round(kcalH);

            // Ratio énergie
            const ratio = totalG > 0 ? (totalKcal / (totalG / 100)) : 0;
            document.getElementById('ratioEnergie').innerText = Math.round(ratio);
        }

        // Initialisation
        window.onload = () => {
            afficherInventaire();
            calculerTout();
        };
    </script>
</body>
</html>
