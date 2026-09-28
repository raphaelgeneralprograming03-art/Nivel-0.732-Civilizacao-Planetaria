
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulador de Civilização Tipo 0.732 - Status Atual da Humanidade</title>
    <style>
        :root {
            --bg-dark: #030712;
            --panel-bg: rgba(15, 23, 42, 0.88);
            --border-glow: rgba(245, 158, 11, 0.35);
            --fossil-amber: #f59e0b;
            --fossil-red: #ef4444;
            --clean-cyan: #06b6d4;
            --clean-green: #10b981;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
            user-select: none;
        }

        body {
            background-color: var(--bg-dark);
            color: var(--text-main);
            height: 100vh;
            overflow: hidden;
            display: flex;
            flex-direction: column;
        }

        /* HEADER */
        header {
            height: 60px;
            background: linear-gradient(180deg, rgba(15, 23, 42, 0.98) 0%, rgba(3, 7, 18, 0.85) 100%);
            border-bottom: 1px solid var(--border-glow);
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 25px;
            z-index: 10;
        }

        header h1 {
            font-size: 1.05rem;
            letter-spacing: 2px;
            color: var(--fossil-amber);
            text-transform: uppercase;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        header h1 span {
            font-size: 0.75rem;
            background: rgba(245, 158, 11, 0.15);
            border: 1px solid var(--fossil-amber);
            padding: 2px 8px;
            border-radius: 4px;
            color: var(--fossil-amber);
        }

        .status-badge {
            font-size: 0.75rem;
            padding: 4px 12px;
            border-radius: 12px;
            background: rgba(239, 68, 68, 0.15);
            border: 1px solid var(--fossil-red);
            color: var(--fossil-red);
            letter-spacing: 1px;
            animation: pulse-border 2s infinite;
        }

        @keyframes pulse-border {
            0%, 100% { border-color: var(--fossil-red); box-shadow: 0 0 5px rgba(239, 68, 68, 0.2); }
            50% { border-color: var(--fossil-amber); box-shadow: 0 0 12px rgba(245, 158, 11, 0.4); }
        }

        /* MAIN CONTAINER */
        .main-container {
            display: grid;
            grid-template-columns: 380px 1fr;
            height: calc(100vh - 60px);
            position: relative;
        }

        /* PAINEL LATERAL DE CONTROLE */
        .control-panel {
            background: var(--panel-bg);
            border-right: 1px solid var(--border-glow);
            backdrop-filter: blur(12px);
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 16px;
            overflow-y: auto;
            z-index: 5;
        }

        .kardashev-box {
            background: radial-gradient(circle, rgba(245, 158, 11, 0.2) 0%, rgba(3, 7, 18, 0.8) 100%);
            border: 1px solid var(--fossil-amber);
            border-radius: 10px;
            padding: 16px;
            text-align: center;
            box-shadow: 0 0 25px rgba(245, 158, 11, 0.15);
            position: relative;
        }

        .kardashev-score {
            font-size: 2.4rem;
            font-family: monospace;
            font-weight: bold;
            color: #fff;
            text-shadow: 0 0 15px var(--fossil-amber);
            margin: 2px 0;
        }

        .power-consumption {
            font-size: 0.85rem;
            font-family: monospace;
            color: var(--clean-cyan);
        }

        .section-title {
            font-size: 0.78rem;
            text-transform: uppercase;
            letter-spacing: 1.5px;
            color: var(--fossil-amber);
            border-bottom: 1px solid rgba(245, 158, 11, 0.2);
            padding-bottom: 6px;
            margin-top: 5px;
        }

        .metric-group {
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        .metric-header {
            display: flex;
            justify-content: space-between;
            font-size: 0.8rem;
        }

        .metric-value {
            font-family: monospace;
            font-weight: bold;
        }

        input[type="range"] {
            width: 100%;
            height: 6px;
            border-radius: 3px;
            background: rgba(255, 255, 255, 0.1);
            outline: none;
            cursor: pointer;
        }

        #slider-fossil { accent-color: var(--fossil-amber); }
        #slider-renewable { accent-color: var(--clean-cyan); }
        #slider-nuclear { accent-color: #a855f7; }
        #slider-power { accent-color: #3b82f6; }

        .toggle-box {
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: rgba(255, 255, 255, 0.03);
            border: 1px solid rgba(255, 255, 255, 0.08);
            border-radius: 6px;
            padding: 8px 12px;
            font-size: 0.78rem;
        }

        .switch {
            position: relative;
            display: inline-block;
            width: 36px;
            height: 18px;
        }

        .switch input { opacity: 0; width: 0; height: 0; }

        .slider {
            position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0;
            background-color: rgba(255,255,255,0.2); transition: .3s; border-radius: 18px;
        }

        .slider:before {
            position: absolute; content: ""; height: 12px; width: 12px; left: 3px; bottom: 3px;
            background-color: white; transition: .3s; border-radius: 50%;
        }

        input:checked + .slider { background-color: var(--clean-green); }
        input:checked + .slider:before { transform: translateX(18px); }

        .info-card {
            background: rgba(0, 0, 0, 0.45);
            border: 1px solid rgba(255, 255, 255, 0.08);
            border-radius: 8px;
            padding: 12px;
            font-size: 0.78rem;
            line-height: 1.45;
            color: var(--text-muted);
        }

        .info-card strong {
            color: var(--text-main);
        }

        /* VIEWPORT CANVAS */
        .viewport {
            position: relative;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at center, #0B132B 0%, #030712 100%);
            overflow: hidden;
        }

        canvas {
            width: 100%;
            height: 100%;
            display: block;
        }

        /* HUD TELEMETRIA OVERLAY */
        .hud-overlay {
            position: absolute;
            bottom: 20px;
            right: 20px;
            background: var(--panel-bg);
            border: 1px solid var(--border-glow);
            border-radius: 10px;
            padding: 16px;
            font-family: monospace;
            font-size: 0.75rem;
            pointer-events: none;
            display: flex;
            flex-direction: column;
            gap: 8px;
            backdrop-filter: blur(10px);
            box-shadow: 0 8px 32px rgba(0,0,0,0.5);
            min-width: 280px;
        }

        .hud-line {
            display: flex;
            justify-content: space-between;
            gap: 15px;
            align-items: center;
        }

        .progress-bar-bg {
            width: 100%;
            height: 8px;
            background: rgba(255,255,255,0.1);
            border-radius: 4px;
            overflow: hidden;
            margin-top: 4px;
            border: 1px solid rgba(255,255,255,0.08);
        }

        .progress-bar-fill {
            height: 100%;
            width: 73.2%;
            background: linear-gradient(90deg, var(--fossil-amber) 0%, var(--clean-cyan) 100%);
            border-radius: 4px;
            transition: width 0.3s ease;
        }

        ::-webkit-scrollbar { width: 5px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-glow); border-radius: 3px; }
    </style>
</head>
<body>

    <header>
        <h1>SIMULADOR KARDASHEV <span>TIPO 0.732</span></h1>
        <div class="status-badge" id="status-indicator">STATUS: TRANSIÇÃO CRÍTICA</div>
    </header>

    <div class="main-container">
        <aside class="control-panel">
            <div class="kardashev-box">
                <div style="font-size: 0.7rem; color: var(--text-muted); text-transform: uppercase;">Nível Atual na Escala Kardashev</div>
                <div class="kardashev-score" id="k-score">K 0.732</div>
                <div class="power-consumption" id="power-display">20.00 TW (2.00 × 10¹³ W)</div>
            </div>

            <div class="section-title">1. Matriz Energética Global</div>
            
            <div class="metric-group">
                <div class="metric-header">
                    <span>Combustíveis Fósseis (Petróleo/Carvão/Gás)</span>
                    <span class="metric-value" id="val-fossil" style="color: var(--fossil-amber);">78%</span>
                </div>
                <input type="range" id="slider-fossil" min="0" max="100" value="78">
            </div>

            <div class="metric-group">
                <div class="metric-header">
                    <span>Renováveis (Solar/Eólica/Hídrica)</span>
                    <span class="metric-value" id="val-renewable" style="color: var(--clean-cyan);">14%</span>
                </div>
                <input type="range" id="slider-renewable" min="0" max="100" value="14">
            </div>

            <div class="metric-group">
                <div class="metric-header">
                    <span>Energia Nuclear (Fissão)</span>
                    <span class="metric-value" id="val-nuclear" style="color: #a855f7;">8%</span>
                </div>
                <input type="range" id="slider-nuclear" min="0" max="100" value="8">
            </div>

            <div class="section-title">2. Consumo & Tecnologia</div>

            <div class="metric-group">
                <div class="metric-header">
                    <span>Consumo Energético Total (TW)</span>
                    <span class="metric-value" id="val-power" style="color: #3b82f6;">20.0 TW</span>
                </div>
                <input type="range" id="slider-power" min="10" max="100" step="0.5" value="20.0">
            </div>

            <div class="toggle-box">
                <span>Captura de Carbono Atmosférico (DAC)</span>
                <label class="switch"><input type="checkbox" id="sw-ccs"><span class="slider"></span></label>
            </div>

            <div class="toggle-box">
                <span>Fusão Nuclear Experimental (Ativa)</span>
                <label class="switch"><input type="checkbox" id="sw-fusion"><span class="slider"></span></label>
            </div>

            <div class="section-title">3. Diagnóstico Planetário</div>
            <div class="info-card">
                <strong>Análise de Transição:</strong><br>
                A humanidade consome ~20 Terawatts, predominantemente extraídos de biomassa fóssil antiga. Para alcançar o Nível 1.0 (10¹⁶ W), precisamos multiplicar nosso consumo por ~500x e capturar 100% do insolamento solar e energia natural do planeta.
            </div>
        </aside>

        <main class="viewport">
            <canvas id="simCanvas"></canvas>

            <div class="hud-overlay">
                <div class="hud-line">
                    <span>DENSIDADE CO₂:</span>
                    <span id="hud-co2" style="color: var(--fossil-amber)">424 ppm</span>
                </div>
                <div class="hud-line">
                    <span>ANOMALIA TÉRMICA:</span>
                    <span id="hud-temp" style="color: var(--fossil-red)">+1.32 °C</span>
                </div>
                <div class="hud-line">
                    <span>QUALIDADE ATMOSFÉRICA:</span>
                    <span id="hud-atmosphere" style="color: var(--fossil-amber)">DEGRADADA</span>
                </div>
                <div class="hud-line">
                    <span>PROGRESSO PARA TIPO I (1.0):</span>
                    <span id="hud-progress-pct" style="color: var(--clean-cyan)">73.2%</span>
                </div>
                <div class="progress-bar-bg">
                    <div class="progress-bar-fill" id="progress-bar-fill"></div>
                </div>
            </div>
        </main>
    </div>

    <script>
        const canvas = document.getElementById('simCanvas');
        const ctx = canvas.getContext('2d');

        // Estado da Simulação
        const state = {
            fossilPct: 78,
            renewablePct: 14,
            nuclearPct: 8,
            powerTW: 20.0,
            powerWatts: 2.0e13,
            kardashevScore: 0.7301,
            ccsActive: false,
            fusionActive: false,
            rotationAngle: 0,
            cloudsAngle: 0
        };

        function resizeCanvas() {
            canvas.width = canvas.parentElement.clientWidth;
            canvas.height = canvas.parentElement.clientHeight;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        // Controles HTML
        const sliderFossil = document.getElementById('slider-fossil');
        const sliderRenewable = document.getElementById('slider-renewable');
        const sliderNuclear = document.getElementById('slider-nuclear');
        const sliderPower = document.getElementById('slider-power');
        const swCCS = document.getElementById('sw-ccs');
        const swFusion = document.getElementById('sw-fusion');

        // Continentes simplificados gerados via coordenadas polares no globo
        const continentNodes = [];
        const NUM_NODES = 250;
        for (let i = 0; i < NUM_NODES; i++) {
            // Agrupamento simulação de massas continentais
            const lat = (Math.random() - 0.5) * Math.PI * 0.85;
            const lon = Math.random() * Math.PI * 2;
            continentNodes.push({ lat, lon, isCity: Math.random() < 0.65, isRenewable: Math.random() < 0.35 });
        }

        // Normalização das porcentagens da matriz energética
        function balanceSliders(changed) {
            let f = parseInt(sliderFossil.value);
            let r = parseInt(sliderRenewable.value);
            let n = parseInt(sliderNuclear.value);

            let total = f + r + n;

            if (total === 0) { f = 100; total = 100; }

            // Ajusta proporcionalmente os outros
            if (changed === 'fossil') {
                let rem = 100 - f;
                let subTotal = r + n;
                r = subTotal > 0 ? Math.round((r / subTotal) * rem) : Math.round(rem / 2);
                n = rem - r;
            } else if (changed === 'renewable') {
                let rem = 100 - r;
                let subTotal = f + n;
                f = subTotal > 0 ? Math.round((f / subTotal) * rem) : Math.round(rem / 2);
                n = rem - f;
            } else if (changed === 'nuclear') {
                let rem = 100 - n;
                let subTotal = f + r;
                f = subTotal > 0 ? Math.round((f / subTotal) * rem) : Math.round(rem / 2);
                r = rem - f;
            }

            sliderFossil.value = f;
            sliderRenewable.value = r;
            sliderNuclear.value = n;

            state.fossilPct = f;
            state.renewablePct = r;
            state.nuclearPct = n;

            document.getElementById('val-fossil').innerText = `${f}%`;
            document.getElementById('val-renewable').innerText = `${r}%`;
            document.getElementById('val-nuclear').innerText = `${n}%`;

            updateMetrics();
        }

        function updateMetrics() {
            state.powerTW = parseFloat(sliderPower.value);
            state.powerWatts = state.powerTW * 1e12; // 1 TW = 10^12 W

            // Fórmula de Sagan para Escala Kardashev: K = (log10(P) - 6) / 10
            state.kardashevScore = (Math.log10(state.powerWatts) - 6) / 10;

            state.ccsActive = swCCS.checked;
            state.fusionActive = swFusion.checked;

            // Atualização do HUD & Dashboard
            document.getElementById('k-score').innerText = `K ${state.kardashevScore.toFixed(3)}`;
            document.getElementById('val-power').innerText = `${state.powerTW.toFixed(1)} TW`;
            
            const expWatts = (state.powerWatts / 1e13).toFixed(2);
            document.getElementById('power-display').innerText = `${state.powerTW.toFixed(2)} TW (${expWatts} × 10¹³ W)`;

            // Cálculo do CO2 e Temperatura Anômala
            let baseCO2 = 280 + (state.fossilPct * 1.85) * (state.powerTW / 20.0);
            if (state.ccsActive) baseCO2 -= 40;
            baseCO2 = Math.max(280, Math.min(800, baseCO2));

            let tempAnomaly = 0.2 + ((baseCO2 - 280) / 100) * 0.75;
            if (state.fusionActive) tempAnomaly -= 0.15;
            tempAnomaly = Math.max(0.1, tempAnomaly);

            document.getElementById('hud-co2').innerText = `${Math.round(baseCO2)} ppm`;
            const tempHUD = document.getElementById('hud-temp');
            tempHUD.innerText = `+${tempAnomaly.toFixed(2)} °C`;

            const statusBadge = document.getElementById('status-indicator');
            const hudAtmos = document.getElementById('hud-atmosphere');

            if (tempAnomaly > 2.0) {
                tempHUD.style.color = 'var(--fossil-red)';
                hudAtmos.innerText = 'RISCO CRÍTICO';
                hudAtmos.style.color = 'var(--fossil-red)';
                statusBadge.innerText = 'STATUS: COLAPSO CLIMÁTICO';
                statusBadge.style.borderColor = 'var(--fossil-red)';
                statusBadge.style.color = 'var(--fossil-red)';
            } else if (tempAnomaly > 1.2) {
                tempHUD.style.color = 'var(--fossil-amber)';
                hudAtmos.innerText = 'DEGRADADA';
                hudAtmos.style.color = 'var(--fossil-amber)';
                statusBadge.innerText = 'STATUS: TRANSIÇÃO CRÍTICA';
                statusBadge.style.borderColor = 'var(--fossil-amber)';
                statusBadge.style.color = 'var(--fossil-amber)';
            } else {
                tempHUD.style.color = 'var(--clean-green)';
                hudAtmos.innerText = 'ESTABILIZADA';
                hudAtmos.style.color = 'var(--clean-green)';
                statusBadge.innerText = 'STATUS: EQUILÍBRIO SUSTENTÁVEL';
                statusBadge.style.borderColor = 'var(--clean-green)';
                statusBadge.style.color = 'var(--clean-green)';
            }

            // Progresso na Barra
            const pctProg = (state.kardashevScore * 100).toFixed(1);
            document.getElementById('hud-progress-pct').innerText = `${pctProg}%`;
            document.getElementById('progress-bar-fill').style.width = `${pctProg}%`;
        }

        // Event Listeners
        sliderFossil.addEventListener('input', () => balanceSliders('fossil'));
        sliderRenewable.addEventListener('input', () => balanceSliders('renewable'));
        sliderNuclear.addEventListener('input', () => balanceSliders('nuclear'));
        sliderPower.addEventListener('input', updateMetrics);
        swCCS.addEventListener('change', updateMetrics);
        swFusion.addEventListener('change', updateMetrics);

        function render() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            const cx = canvas.width / 2;
            const cy = canvas.height / 2;
            const earthRadius = Math.min(canvas.width, canvas.height) * 0.28;

            state.rotationAngle += 0.002;
            state.cloudsAngle += 0.0015;

            // 1. Grade de Escala Sci-Fi no Fundo
            ctx.strokeStyle = 'rgba(245, 158, 11, 0.03)';
            ctx.lineWidth = 1;
            const step = 50;
            for (let x = 0; x < canvas.width; x += step) {
                ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, canvas.height); ctx.stroke();
            }
            for (let y = 0; y < canvas.height; y += step) {
                ctx.beginPath(); ctx.moveTo(0, y); ctx.lineTo(canvas.width, y); ctx.stroke();
            }

            // 2. Brilho Atmosférico Primário
            const atmosGlow = ctx.createRadialGradient(cx, cy, earthRadius - 5, cx, cy, earthRadius + 30);
            const pollutionAlpha = (state.fossilPct / 100) * 0.5;
            atmosGlow.addColorStop(0, 'rgba(56, 189, 248, 0.2)');
            atmosGlow.addColorStop(0.6, `rgba(245, 158, 11, ${pollutionAlpha})`);
            atmosGlow.addColorStop(1, 'rgba(0,0,0,0)');

            ctx.beginPath();
            ctx.arc(cx, cy, earthRadius + 30, 0, Math.PI * 2);
            ctx.fillStyle = atmosGlow;
            ctx.fill();

            // 3. Oceano / Corpo do Planeta
            const oceanGrad = ctx.createRadialGradient(cx - earthRadius * 0.3, cy - earthRadius * 0.3, 10, cx, cy, earthRadius);
            oceanGrad.addColorStop(0, '#1e3a8a');
            oceanGrad.addColorStop(0.7, '#1e293b');
            oceanGrad.addColorStop(1, '#0f172a');

            ctx.beginPath();
            ctx.arc(cx, cy, earthRadius, 0, Math.PI * 2);
            ctx.fillStyle = oceanGrad;
            ctx.fill();

            // 4. Mapeamento de Continentes e Cidades (Rotativos)
            ctx.save();
            ctx.beginPath();
            ctx.arc(cx, cy, earthRadius, 0, Math.PI * 2);
            ctx.clip(); // Limita o desenho à esfera terrestre

            // Iluminação Diurna/Noturna Gradiente
            const sunGrad = ctx.createLinearGradient(cx - earthRadius, cy, cx + earthRadius, cy);
            sunGrad.addColorStop(0, 'rgba(255, 255, 255, 0.15)');
            sunGrad.addColorStop(0.5, 'rgba(0, 0, 0, 0.2)');
            sunGrad.addColorStop(1, 'rgba(0, 0, 0, 0.85)');

            // Renderizar Pontos de Continente / Luzes Urbanas
            continentNodes.forEach(node => {
                const currentLon = node.lon + state.rotationAngle;
                const x = cx + Math.cos(currentLon) * Math.cos(node.lat) * earthRadius;
                const y = cy + Math.sin(node.lat) * earthRadius;

                // Somente pontos na parte frontal do globo
                if (Math.sin(currentLon) > -0.2) {
                    ctx.beginPath();
                    ctx.arc(x, y, 3, 0, Math.PI * 2);
                    ctx.fillStyle = '#15803d'; // Cor de massa de terra verde
                    ctx.fill();

                    // Se for um nó populacional (Luzes de Cidades ou Redes Limpas)
                    if (node.isCity) {
                        ctx.beginPath();
                        ctx.arc(x, y, 1.8, 0, Math.PI * 2);
                        
                        // Alterna entre luz fóssil (âmbar/cinza) e renovável (ciano)
                        if (node.isRenewable && state.renewablePct > 30) {
                            ctx.fillStyle = '#06b6d4';
                            ctx.shadowColor = '#06b6d4';
                            ctx.shadowBlur = 6;
                        } else {
                            ctx.fillStyle = state.fossilPct > 50 ? '#f59e0b' : '#fef08a';
                            ctx.shadowColor = '#f59e0b';
                            ctx.shadowBlur = 4;
                        }
                        ctx.fill();
                        ctx.shadowBlur = 0;
                    }
                }
            });

            // Aplica sombra da noite
            ctx.fillStyle = sunGrad;
            ctx.fillRect(cx - earthRadius, cy - earthRadius, earthRadius * 2, earthRadius * 2);

            // 5. Camada Dinâmica de Poluição e Nuvens de Smog por Carbono
            const smogDensity = (state.fossilPct / 100) * 0.65;
            if (smogDensity > 0.05) {
                ctx.fillStyle = `rgba(120, 85, 40, ${smogDensity})`;
                for (let c = 0; c < 12; c++) {
                    const angle = state.cloudsAngle + (c * Math.PI / 6);
                    const cloudX = cx + Math.cos(angle) * (earthRadius * 0.6);
                    const cloudY = cy + Math.sin(angle * 1.5) * (earthRadius * 0.5);

                    ctx.beginPath();
                    ctx.arc(cloudX, cloudY, 35 + (c * 4), 0, Math.PI * 2);
                    ctx.fill();
                }
            }

            ctx.restore();

            // 6. Anéis de Órbita de Monitoramento
            ctx.beginPath();
            ctx.arc(cx, cy, earthRadius + 45, 0, Math.PI * 2);
            ctx.strokeStyle = 'rgba(245, 158, 11, 0.15)';
            ctx.setLineDash([4, 10]);
            ctx.stroke();
            ctx.setLineDash([]);

            requestAnimationFrame(render);
        }

        // Inicialização
        balanceSliders('fossil');
        render();
    </script>
</body>
</html>
