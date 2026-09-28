
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulador Kardashev: Nível 0.73 - A Transição Crítica</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --m3c5: #0f172a;
            --m3c6: #1e293b;
            --m3c7: #334155;
            --m3c9: #f8fafc;
            --m3c10: #94a3b8;
            --m3c18: #38bdf8;
            --m3c23: #60a5fa;
        }

        body {
            background-color: var(--m3c5);
            color: var(--m3c9);
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
        }

        .card {
            background-color: var(--m3c6);
            border: 1px solid var(--m3c7);
        }

        .custom-slider {
            accent-color: var(--m3c18);
        }

        .pulse-critical {
            animation: pulse-red 2s infinite;
        }

        @keyframes pulse-red {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.4; }
        }
    </style>
</head>
<body class="min-h-screen flex flex-col p-4 md:p-6">

    <!-- Top Bar Header -->
    <header class="max-w-7xl mx-auto w-full mb-6 flex flex-col md:flex-row justify-between items-start md:items-center border-b border-slate-700 pb-4 gap-4">
        <div>
            <span class="text-xs font-mono uppercase tracking-widest text-sky-400">Simulador de Evolução de Civilização</span>
            <h1 class="text-2xl md:text-3xl font-bold flex items-center gap-3">
                Escala Kardashev: <span id="kardashevDisplay" class="text-sky-400 font-mono">0.730</span>
                <span class="text-xs px-2 py-1 rounded bg-amber-500/20 text-amber-300 border border-amber-500/30">Nível 0 - A Transição Crítica</span>
            </h1>
        </div>
        <div class="flex items-center gap-4">
            <button id="btnAdvance" onclick="advanceYear()" class="bg-sky-500 hover:bg-sky-600 text-slate-950 font-bold px-5 py-2.5 rounded-lg shadow-lg shadow-sky-500/20 transition cursor-pointer flex items-center gap-2">
                <span>Avançar 1 Ano</span>
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 5l7 7-7 7M5 5l7 7-7 7"></path></svg>
            </button>
            <button id="btnReset" onclick="resetSimulation()" class="bg-slate-700 hover:bg-slate-600 text-slate-200 px-3 py-2.5 rounded-lg text-sm transition cursor-pointer">
                Reiniciar
            </button>
        </div>
    </header>

    <!-- Main Dashboard Grid -->
    <main class="max-w-7xl mx-auto w-full grid grid-cols-1 lg:grid-cols-3 gap-6 flex-1">
        
        <!-- Left Column: Core Controls & Needs -->
        <section class="space-y-6">
            <div class="card p-5 rounded-xl">
                <h2 class="text-lg font-bold text-sky-400 mb-4 flex items-center gap-2">
                    <span>🛠️ Necessidades Críticas</span>
                </h2>
                
                <!-- Fusion Power -->
                <div class="mb-5">
                    <div class="flex justify-between items-center text-sm mb-1">
                        <span class="font-medium">Fusão Nuclear Comercial</span>
                        <span id="fusionVal" class="font-mono text-sky-400">15%</span>
                    </div>
                    <input type="range" id="fusionSlider" min="0" max="100" value="15" class="w-full custom-slider" oninput="updateControls()">
                    <p class="text-xs text-slate-400 mt-1">Substituição de termelétricas e fissão tradicional por fusão limpa.</p>
                </div>

                <!-- Global Grid -->
                <div class="mb-5">
                    <div class="flex justify-between items-center text-sm mb-1">
                        <span class="font-medium">Rede Elétrica Global Inteligente</span>
                        <span id="gridVal" class="font-mono text-sky-400">20%</span>
                    </div>
                    <input type="range" id="gridSlider" min="0" max="100" value="20" class="w-full custom-slider" oninput="updateControls()">
                    <p class="text-xs text-slate-400 mt-1">Transmissão intercontinental de energia limpa sem perdas.</p>
                </div>

                <!-- Geopolitical Unity -->
                <div class="mb-5">
                    <div class="flex justify-between items-center text-sm mb-1">
                        <span class="font-medium">Superação do "Grande Filtro"</span>
                        <span id="unityVal" class="font-mono text-sky-400">35%</span>
                    </div>
                    <input type="range" id="unitySlider" min="0" max="100" value="35" class="w-full custom-slider" oninput="updateControls()">
                    <p class="text-xs text-slate-400 mt-1">Cooperação geopolítica contra guerra nuclear, pandemias e colapso.</p>
                </div>

                <!-- Carbon Capture -->
                <div>
                    <div class="flex justify-between items-center text-sm mb-1">
                        <span class="font-medium">Captura Atmosférica de Carbono</span>
                        <span id="carbonVal" class="font-mono text-sky-400">10%</span>
                    </div>
                    <input type="range" id="carbonSlider" min="0" max="100" value="10" class="w-full custom-slider" oninput="updateControls()">
                    <p class="text-xs text-slate-400 mt-1">Remoção direta de CO₂ da atmosfera para reversão do aquecimento.</p>
                </div>
            </div>

            <!-- Global Status Overview -->
            <div class="card p-5 rounded-xl">
                <h3 class="text-md font-bold mb-3 text-slate-200">Métricas Globais do Planeta</h3>
                <div class="space-y-3 text-sm">
                    <div>
                        <div class="flex justify-between mb-1">
                            <span>CO₂ Atmosférico (ppm)</span>
                            <span id="co2Display" class="font-mono font-bold text-rose-400">425 ppm</span>
                        </div>
                        <div class="w-full bg-slate-700 h-2 rounded-full overflow-hidden">
                            <div id="co2Bar" class="bg-rose-500 h-full transition-all" style="width: 70%"></div>
                        </div>
                    </div>

                    <div>
                        <div class="flex justify-between mb-1">
                            <span>Matriz Energética Limpa</span>
                            <span id="cleanEnergyDisplay" class="font-mono font-bold text-emerald-400">22%</span>
                        </div>
                        <div class="w-full bg-slate-700 h-2 rounded-full overflow-hidden">
                            <div id="cleanEnergyBar" class="bg-emerald-500 h-full transition-all" style="width: 22%"></div>
                        </div>
                    </div>

                    <div>
                        <div class="flex justify-between mb-1">
                            <span>Risco de Colapso Planetário</span>
                            <span id="collapseRiskDisplay" class="font-mono font-bold text-amber-400">68%</span>
                        </div>
                        <div class="w-full bg-slate-700 h-2 rounded-full overflow-hidden">
                            <div id="riskBar" class="bg-amber-500 h-full transition-all" style="width: 68%"></div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Center Column: Domain Impact -->
        <section class="space-y-6">
            <div class="card p-5 rounded-xl">
                <h2 class="text-lg font-bold text-sky-400 mb-4">🌍 Transformação dos Ambientes</h2>
                
                <div class="space-y-4 text-sm">
                    <!-- Sea Domain -->
                    <div class="p-3 bg-slate-800/60 rounded-lg border border-slate-700">
                        <div class="flex justify-between items-center mb-1">
                            <span class="font-bold text-cyan-300">🌊 No Mar</span>
                            <span id="seaBadge" class="text-xs px-2 py-0.5 rounded bg-amber-500/20 text-amber-300 border border-amber-500/30">Inicial</span>
                        </div>
                        <p id="seaDesc" class="text-xs text-slate-300">Início da mineração robótica submarina sustentável e protótipos de fazendas de algas artificiais.</p>
                    </div>

                    <!-- Land Domain -->
                    <div class="p-3 bg-slate-800/60 rounded-lg border border-slate-700">
                        <div class="flex justify-between items-center mb-1">
                            <span class="font-bold text-emerald-300">🌱 Na Terra</span>
                            <span id="landBadge" class="text-xs px-2 py-0.5 rounded bg-amber-500/20 text-amber-300 border border-amber-500/30">Dependência Fóssil</span>
                        </div>
                        <p id="landDesc" class="text-xs text-slate-300">Uso massivo de combustíveis fósseis e restos orgânicos. Desmatamento ainda ativo.</p>
                    </div>

                    <!-- Air Domain -->
                    <div class="p-3 bg-slate-800/60 rounded-lg border border-slate-700">
                        <div class="flex justify-between items-center mb-1">
                            <span class="font-bold text-indigo-300">✈️ No Ar</span>
                            <span id="airBadge" class="text-xs px-2 py-0.5 rounded bg-amber-500/20 text-amber-300 border border-amber-500/30">Emissão Alta</span>
                        </div>
                        <p id="airDesc" class="text-xs text-slate-300">Frotas de aviação baseadas em querosene. Acúmulo constante de gases do efeito estufa.</p>
                    </div>

                    <!-- Off-Planet Domain -->
                    <div class="p-3 bg-slate-800/60 rounded-lg border border-slate-700">
                        <div class="flex justify-between items-center mb-1">
                            <span class="font-bold text-purple-300">🚀 No Espaço / Off-Planet</span>
                            <span id="spaceBadge" class="text-xs px-2 py-0.5 rounded bg-slate-700 text-slate-300">Postos Avançados</span>
                        </div>
                        <p id="spaceDesc" class="text-xs text-slate-300">Primeiras bases de exploração científica na Lua e Marte (dependência total da Terra).</p>
                    </div>
                </div>
            </div>

            <!-- Simulation Log -->
            <div class="card p-5 rounded-xl">
                <h3 class="text-md font-bold mb-2 text-slate-200">Diário do Progresso Planetário</h3>
                <div id="simLog" class="h-40 overflow-y-auto space-y-2 text-xs font-mono bg-slate-950/60 p-3 rounded border border-slate-800">
                    <p class="text-sky-400">[Ano 2026] Início da simulação no Nível Kardashev 0.730. A civilização depende de energia fóssil.</p>
                </div>
            </div>
        </section>

        <!-- Right Column: Graphs & Analysis -->
        <section class="space-y-6">
            <div class="card p-5 rounded-xl flex flex-col justify-between">
                <div>
                    <h2 class="text-lg font-bold text-sky-400 mb-2">📊 Trajetória Evolutiva</h2>
                    <p class="text-xs text-slate-400 mb-4">Projeção do índice Kardashev e nível de CO₂ ao longo dos anos.</p>
                </div>
                <div class="h-64">
                    <canvas id="kardashevChart"></canvas>
                </div>
            </div>

            <div id="outcomeBox" class="card p-5 rounded-xl border-sky-500/30">
                <h3 id="outcomeTitle" class="font-bold text-md text-sky-400 mb-1">Estado da Civilização</h3>
                <p id="outcomeText" class="text-xs text-slate-300 leading-relaxed">
                    Você está no Nível 0.73. Para transitar para o Tipo 1, é essencial zerar a dependência de fósseis, controlar as emissões de CO₂ e garantir estabilidade geopolítica.
                </p>
            </div>
        </section>

    </main>

    <script>
        // State Variables
        let year = 2026;
        let kardashevLevel = 0.73;
        let co2Ppm = 425;
        let cleanEnergy = 22;
        let collapseRisk = 68;

        // Chart initialized
        let ctx = document.getElementById('kardashevChart').getContext('2d');
        let chart = new Chart(ctx, {
            type: 'line',
            data: {
                labels: [2026],
                datasets: [
                    {
                        label: 'Nível Kardashev',
                        data: [0.73],
                        borderColor: '#38bdf8',
                        backgroundColor: 'rgba(56, 189, 248, 0.1)',
                        yAxisID: 'y',
                        tension: 0.3,
                        fill: true
                    },
                    {
                        label: 'CO2 (ppm)',
                        data: [425],
                        borderColor: '#f43f5e',
                        borderDash: [5, 5],
                        yAxisID: 'y1',
                        tension: 0.3
                    }
                ]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    x: {
                        ticks: { color: '#94a3b8' },
                        grid: { color: '#334155' }
                    },
                    y: {
                        type: 'linear',
                        display: true,
                        position: 'left',
                        min: 0.7,
                        max: 1.0,
                        ticks: { color: '#38bdf8' },
                        grid: { color: '#334155' }
                    },
                    y1: {
                        type: 'linear',
                        display: true,
                        position: 'right',
                        min: 250,
                        max: 600,
                        ticks: { color: '#f43f5e' },
                        grid: { drawOnChartArea: false }
                    }
                },
                plugins: {
                    legend: { labels: { color: '#f8fafc', boxWidth: 12 } }
                }
            }
        });

        function updateControls() {
            let fusion = parseInt(document.getElementById('fusionSlider').value);
            let grid = parseInt(document.getElementById('gridSlider').value);
            let unity = parseInt(document.getElementById('unitySlider').value);
            let carbon = parseInt(document.getElementById('carbonSlider').value);

            document.getElementById('fusionVal').innerText = fusion + '%';
            document.getElementById('gridVal').innerText = grid + '%';
            document.getElementById('unityVal').innerText = unity + '%';
            document.getElementById('carbonVal').innerText = carbon + '%';

            // Calculate metrics derived from current sliders
            cleanEnergy = Math.min(100, Math.round((fusion * 0.4) + (grid * 0.4) + 20));
            collapseRisk = Math.max(0, Math.round(100 - (unity * 0.6) - (cleanEnergy * 0.3)));

            // Update UI elements
            document.getElementById('cleanEnergyDisplay').innerText = cleanEnergy + '%';
            document.getElementById('cleanEnergyBar').style.width = cleanEnergy + '%';

            document.getElementById('collapseRiskDisplay').innerText = collapseRisk + '%';
            document.getElementById('riskBar').style.width = collapseRisk + '%';

            updateDomainStates(fusion, grid, unity, carbon);
        }

        function updateDomainStates(fusion, grid, unity, carbon) {
            // Sea Domain
            let seaBadge = document.getElementById('seaBadge');
            let seaDesc = document.getElementById('seaDesc');
            if (fusion > 50 && grid > 40) {
                seaBadge.innerText = "Avançado";
                seaBadge.className = "text-xs px-2 py-0.5 rounded bg-emerald-500/20 text-emerald-300 border border-emerald-500/30";
                seaDesc.innerText = "Dessalinização massiva ativa com energia limpa; mineração submarina autônoma sustentável e megafazendas de algas.";
            } else {
                seaBadge.innerText = "Inicial";
                seaBadge.className = "text-xs px-2 py-0.5 rounded bg-amber-500/20 text-amber-300 border border-amber-500/30";
                seaDesc.innerText = "Início da mineração robótica submarina sustentável e protótipos de fazendas de algas artificiais.";
            }

            // Land Domain
            let landBadge = document.getElementById('landBadge');
            let landDesc = document.getElementById('landDesc');
            if (fusion > 70 && cleanEnergy > 80) {
                landBadge.innerText = "Sustentável";
                landBadge.className = "text-xs px-2 py-0.5 rounded bg-emerald-500/20 text-emerald-300 border border-emerald-500/30";
                landDesc.innerText = "Fim total de termelétricas. Cidades verticais megasustentáveis e reflorestamento global concluído.";
            } else {
                landBadge.innerText = "Transição";
                landBadge.className = "text-xs px-2 py-0.5 rounded bg-amber-500/20 text-amber-300 border border-amber-500/30";
                landDesc.innerText = "Substituição gradual de termelétricas. Desmatamento em queda, mas infraestrutura fóssil ainda presente.";
            }

            // Air Domain
            let airBadge = document.getElementById('airBadge');
            let airDesc = document.getElementById('airDesc');
            if (carbon > 60) {
                airBadge.innerText = "Reversão Ativa";
                airBadge.className = "text-xs px-2 py-0.5 rounded bg-emerald-500/20 text-emerald-300 border border-emerald-500/30";
                airDesc.innerText = "Captura de carbono massiva reduzindo ppm atmosférico. Frota aérea 100% elétrica/hidrogênio.";
            } else {
                airBadge.innerText = "Emissão Alta";
                airBadge.className = "text-xs px-2 py-0.5 rounded bg-amber-500/20 text-amber-300 border border-amber-500/30";
                airDesc.innerText = "Início da captura de carbono. Frotas elétricas em expansão no espaço aéreo.";
            }

            // Space Domain
            let spaceBadge = document.getElementById('spaceBadge');
            let spaceDesc = document.getElementById('spaceDesc');
            if (fusion > 80 && unity > 70) {
                spaceBadge.innerText = "Colônias Autossuficientes";
                spaceBadge.className = "text-xs px-2 py-0.5 rounded bg-purple-500/20 text-purple-300 border border-purple-500/30";
                spaceDesc.innerText = "Bases autossuficientes operacionais em Marte e na Lua atuando como postos avançados da humanidade.";
            } else {
                spaceBadge.innerText = "Postos Avançados";
                spaceBadge.className = "text-xs px-2 py-0.5 rounded bg-slate-700 text-slate-300";
                spaceDesc.innerText = "Primeiras bases de exploração científica na Lua e Marte (dependência total da Terra).";
            }
        }

        function advanceYear() {
            year += 1;

            let fusion = parseInt(document.getElementById('fusionSlider').value);
            let grid = parseInt(document.getElementById('gridSlider').value);
            let unity = parseInt(document.getElementById('unitySlider').value);
            let carbon = parseInt(document.getElementById('carbonSlider').value);

            // Calculate annual CO2 change based on clean energy and carbon capture
            let co2Delta = 3 - (cleanEnergy * 0.05) - (carbon * 0.04);
            co2Ppm = Math.max(280, Math.round((co2Ppm + co2Delta) * 10) / 10);

            // Calculate Kardashev growth
            let growth = (fusion * 0.0008) + (grid * 0.0005) + (unity * 0.0004) + (cleanEnergy * 0.0003);
            kardashevLevel = Math.min(1.0, Math.round((kardashevLevel + growth) * 1000) / 1000);

            // Update CO2 Display
            document.getElementById('co2Display').innerText = co2Ppm + ' ppm';
            document.getElementById('co2Bar').style.width = Math.min(100, (co2Ppm / 500) * 100) + '%';
            document.getElementById('kardashevDisplay').innerText = kardashevLevel.toFixed(3);

            // Update Log
            let log = document.getElementById('simLog');
            let newLog = document.createElement('p');

            if (collapseRisk > 80 && Math.random() < 0.4) {
                newLog.className = "text-rose-400";
                newLog.innerText = `[Ano ${year}] CRISE: Instabilidade geopolítica e eventos climáticos desaceleram o progresso!`;
            } else if (kardashevLevel >= 1.0) {
                newLog.className = "text-emerald-400 font-bold";
                newLog.innerText = `[Ano ${year}] MARCO ALCANÇADO! A civilização atingiu o TIPO 1 na Escala Kardashev!`;
            } else {
                newLog.className = "text-slate-300";
                newLog.innerText = `[Ano ${year}] Kardashev: ${kardashevLevel.toFixed(3)} | CO2: ${co2Ppm} ppm | Risco: ${collapseRisk}%`;
            }
            log.prepend(newLog);

            // Update Chart
            chart.data.labels.push(year);
            chart.data.datasets[0].data.push(kardashevLevel);
            chart.data.datasets[1].data.push(co2Ppm);
            chart.update();

            // Evaluate simulation state
            checkStatus();
        }

        function checkStatus() {
            let title = document.getElementById('outcomeTitle');
            let text = document.getElementById('outcomeText');
            let box = document.getElementById('outcomeBox');

            if (co2Ppm > 480 || collapseRisk > 85) {
                title.innerText = "⚠️ Risco Severo de Colapso";
                title.className = "font-bold text-md text-rose-400 mb-1";
                box.className = "card p-5 rounded-xl border-rose-500/50 bg-rose-950/10";
                text.innerText = "A civilização enfrentará o 'Grande Filtro' com altas chances de colapso devido ao descontrole climático e instabilidade geopolítica.";
            } else if (kardashevLevel >= 1.0) {
                title.innerText = "🌟 Civilização de Tipo I Atingida!";
                title.className = "font-bold text-md text-emerald-400 mb-1";
                box.className = "card p-5 rounded-xl border-emerald-500/50 bg-emerald-950/10";
                text.innerText = "Parabéns! A humanidade domina completamente a energia do planeta Terra, superou a dependência fóssil e estabeleceu estabilidade planetária.";
            } else {
                title.innerText = "Estado da Civilização: Transição Ativa";
                title.className = "font-bold text-md text-sky-400 mb-1";
                box.className = "card p-5 rounded-xl border-sky-500/30";
                text.innerText = `Progresso contínuo no Nível ${kardashevLevel.toFixed(3)}. Mantenha o equilíbrio entre investimento tecnológico e coesão geopolítica.`;
            }
        }

        function resetSimulation() {
            year = 2026;
            kardashevLevel = 0.73;
            co2Ppm = 425;

            document.getElementById('fusionSlider').value = 15;
            document.getElementById('gridSlider').value = 20;
            document.getElementById('unitySlider').value = 35;
            document.getElementById('carbonSlider').value = 10;

            document.getElementById('simLog').innerHTML = '<p class="text-sky-400">[Ano 2026] Simulação reiniciada. Nível Kardashev 0.730.</p>';
            
            chart.data.labels = [2026];
            chart.data.datasets[0].data = [0.73];
            chart.data.datasets[1].data = [425];
            chart.update();

            updateControls();
            document.getElementById('co2Display').innerText = '425 ppm';
            document.getElementById('kardashevDisplay').innerText = '0.730';
        }

        // Initialize on load
        updateControls();
    </script>
</body>
</html>
