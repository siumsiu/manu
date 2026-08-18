<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Gli Avanzamenti di Manu</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');

        :root {
            --mc-bg: #c6c6c6;
            --mc-dark: #373737;
            --mc-border-light: #ffffff;
            --mc-border-dark: #555555;
            --mc-text: #3c3c3c;
            --mc-text-light: #ffffff;
            --mc-text-yellow: #ffff55;
            --mc-text-green: #55ff55;
        }

        * {
            box-sizing: border-box;
            user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            margin: 0;
            padding: 0;
            font-family: 'Press Start 2P', monospace;
            background-color: #1d1d1d;
            background-image: repeating-linear-gradient(45deg, #222 25%, transparent 25%, transparent 75%, #222 75%, #222), repeating-linear-gradient(45deg, #222 25%, #1d1d1d 25%, #1d1d1d 75%, #222 75%, #222);
            background-position: 0 0, 10px 10px;
            background-size: 20px 20px;
            color: var(--mc-text-light);
            height: 100vh;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        /* UI Box in stile Minecraft */
        .mc-window {
            background-color: var(--mc-bg);
            border: 4px solid;
            border-color: var(--mc-border-light) var(--mc-border-dark) var(--mc-border-dark) var(--mc-border-light);
            padding: 10px;
            color: var(--mc-text);
        }

        /* Header e Barra XP */
        header {
            margin: 10px;
            padding: 15px 10px;
            text-align: center;
        }

        h1 {
            font-size: 14px;
            margin: 0 0 15px 0;
            color: var(--mc-dark);
            text-shadow: 2px 2px 0px #888;
        }

        .xp-container {
            width: 100%;
            margin-top: 10px;
            position: relative;
        }

        .xp-bar-bg {
            height: 14px;
            background-color: #000;
            border: 2px solid #333;
            width: 100%;
            position: relative;
        }

        .xp-bar-fill {
            height: 100%;
            background-color: #38b117; /* Verde XP */
            width: 0%;
            transition: width 0.5s ease;
            border-top: 2px solid #5cff2b;
        }

        .xp-labels {
            display: flex;
            justify-content: space-between;
            font-size: 8px;
            margin-top: 8px;
            color: var(--mc-dark);
            font-weight: bold;
        }

        .xp-level {
            position: absolute;
            top: -20px;
            left: 50%;
            transform: translateX(-50%);
            font-size: 16px;
            color: var(--mc-text-green);
            text-shadow: 2px 2px 0px #000;
        }

        /* Tabs Categorie */
        .tabs {
            display: flex;
            overflow-x: auto;
            margin: 0 10px;
            padding-bottom: 5px;
            scrollbar-width: none;
        }
        .tabs::-webkit-scrollbar { display: none; }

        .tab {
            background-color: #8b8b8b;
            border: 4px solid;
            border-color: var(--mc-border-light) var(--mc-border-dark) var(--mc-border-dark) var(--mc-border-light);
            padding: 10px;
            margin-right: 5px;
            font-size: 10px;
            cursor: pointer;
            white-space: nowrap;
            color: var(--mc-text-light);
            text-shadow: 1px 1px 0px #000;
        }
        .tab.active {
            background-color: var(--mc-bg);
            color: var(--mc-text);
            text-shadow: none;
            border-bottom: 4px solid var(--mc-bg);
        }

        /* Area Progressi */
        .advancements-area {
            flex: 1;
            margin: 0 10px 10px 10px;
            background-color: #555;
            background-image: url('data:image/svg+xml;utf8,<svg width="32" height="32" viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg"><rect width="32" height="32" fill="%236e6e6e"/><rect width="16" height="16" fill="%235a5a5a"/><rect x="16" y="16" width="16" height="16" fill="%235a5a5a"/></svg>');
            border: 4px solid;
            border-color: #333 #777 #777 #333;
            overflow-y: auto;
            position: relative;
            padding: 20px 10px;
        }

        .tree {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
            position: relative;
        }

        /* Collegamenti verticali */
        .tree::before {
            content: '';
            position: absolute;
            top: 0;
            bottom: 0;
            left: 50%;
            width: 4px;
            background-color: #000;
            transform: translateX(-50%);
            z-index: 1;
        }

        .advancement {
            position: relative;
            z-index: 2;
            display: flex;
            align-items: center;
            background-color: var(--mc-bg);
            border: 2px solid #000;
            padding: 5px;
            width: 90%;
            cursor: pointer;
            box-shadow: inset -2px -2px 0px #888, inset 2px 2px 0px #fff;
        }

        .advancement.completed {
            background-color: #f6e076;
            box-shadow: inset -2px -2px 0px #c5a027, inset 2px 2px 0px #fffdbb;
        }
        
        .advancement.completed .adv-title {
            color: #7b5900;
        }

        .adv-icon {
            width: 40px;
            height: 40px;
            background-color: rgba(0,0,0,0.1);
            border: 2px solid #333;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            margin-right: 10px;
        }

        .adv-info {
            flex: 1;
        }

        .adv-title {
            font-size: 10px;
            line-height: 1.4;
            color: var(--mc-text);
        }

        .adv-pts {
            font-size: 8px;
            color: #555;
            margin-top: 5px;
        }

        /* Finestra Modale */
        .modal-overlay {
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0,0,0,0.7);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 100;
        }

        .modal {
            width: 90%;
            max-width: 350px;
            position: relative;
        }

        .modal-content {
            text-align: center;
            padding: 20px;
        }

        .modal-icon {
            font-size: 40px;
            margin-bottom: 10px;
        }

        .modal-title {
            color: var(--mc-text-yellow);
            text-shadow: 2px 2px 0px #000;
            font-size: 12px;
            margin-bottom: 15px;
            line-height: 1.5;
        }

        .modal-desc {
            font-size: 9px;
            line-height: 1.6;
            color: #fff;
            text-shadow: 1px 1px 0px #000;
            margin-bottom: 20px;
            background: rgba(0,0,0,0.5);
            padding: 10px;
            border: 2px solid #555;
        }

        .btn {
            font-family: 'Press Start 2P', monospace;
            background-color: var(--mc-bg);
            border: 4px solid;
            border-color: var(--mc-border-light) var(--mc-border-dark) var(--mc-border-dark) var(--mc-border-light);
            padding: 15px;
            font-size: 10px;
            width: 100%;
            cursor: pointer;
            margin-bottom: 10px;
        }
        
        .btn:active {
            border-color: var(--mc-border-dark) var(--mc-border-light) var(--mc-border-light) var(--mc-border-dark);
        }

        .btn-close {
            background-color: #ff5555;
            color: white;
            border-color: #ffaaaa #aa0000 #aa0000 #ffaaaa;
        }

        /* Notifica Toast */
        .toast {
            position: fixed;
            top: -100px;
            left: 50%;
            transform: translateX(-50%);
            background-color: #212121;
            border: 2px solid #55ff55;
            color: #fff;
            padding: 15px;
            display: flex;
            align-items: center;
            gap: 15px;
            z-index: 200;
            transition: top 0.5s ease;
            box-shadow: 0px 4px 10px rgba(0,0,0,0.5);
            width: 90%;
            max-width: 400px;
        }
        .toast.show { top: 20px; }
        .toast-icon { font-size: 24px; }
        .toast-text div:first-child { color: var(--mc-text-yellow); font-size: 10px; margin-bottom: 5px;}
        .toast-text div:last-child { font-size: 8px; }

    </style>
</head>
<body>

    <!-- Header & Progression -->
    <header class="mc-window">
        <h1>Missioni Vacanza</h1>
        <div class="xp-container">
            <div class="xp-level" id="xpLevel">0 PUNTI</div>
            <div class="xp-bar-bg">
                <div class="xp-bar-fill" id="xpFill"></div>
            </div>
            <div class="xp-labels">
                <span>67loser</span>
                <span>Dybala mega super aura</span>
            </div>
        </div>
    </header>

    <!-- Tabs (Categorie) -->
    <div class="tabs" id="tabsContainer">
        <!-- Generati da JS -->
    </div>

    <!-- Area delle missioni -->
    <div class="advancements-area" id="advancementsArea">
        <div class="tree" id="treeContainer">
            <!-- Generati da JS -->
        </div>
    </div>

    <!-- Modale Dettagli -->
    <div class="modal-overlay" id="modalOverlay">
        <div class="mc-window modal">
            <div class="modal-content">
                <div class="modal-icon" id="mIcon"></div>
                <div class="modal-title" id="mTitle">Titolo</div>
                <div class="modal-desc" id="mDesc">Descrizione della missione...</div>
                <button class="btn" id="btnComplete">Segna come COMPLETATO</button>
                <button class="btn btn-close" onclick="closeModal()">Chiudi</button>
            </div>
        </div>
    </div>

    <!-- Notifica Avanzamento -->
    <div class="toast" id="toast">
        <div class="toast-icon">🏆</div>
        <div class="toast-text">
            <div>Obbiettivo Completato!</div>
            <div id="toastDesc">Nome Missione</div>
        </div>
    </div>

    <script>
        const missions = [
            // PARLATO & COMPORTAMENTO (Categoria 1)
            { id: 1, cat: 0, pts: 2, icon: '🗣️', title: 'Assolutismo', desc: 'Abolisci la parola "molto" e usa solo i superlativi assoluti (bravissimo, malissimo, fortunatissimo, bravobravo, malemale).' },
            { id: 2, cat: 0, pts: 3, icon: '🦈', title: 'Dizionario Bizzarro', desc: 'Fai entrare prepotentemente nel tuo parlato: "lardo"/"poderoso", "squalo martello" e parole buffe in spagnolo.' },
            { id: 12, cat: 0, pts: 4, icon: '🚫', title: 'Il Dittatore', desc: 'Decidi che una parola è bannata e arrabbiati finché non smettono di usarla. Poi ricomincia tu a spammarla.' },
            { id: 16, cat: 0, pts: 7, icon: '✅', title: 'Yes Man', desc: 'Per un intero giorno non puoi MAI dire di "no".' },
            { id: 17, cat: 0, pts: 4, icon: '🇪🇸', title: 'El Conquistador', desc: 'Fai la prima mezza giornata, appena arrivi, con l\'accento più spagnolo che sai fare. Usa anche termini che credi siano spagnoli.' },
            
            // SOCIAL & INFLUENCER (Categoria 2)
            { id: 3, cat: 1, pts: 6, icon: '📹', title: 'Clickbait Real', desc: 'Fai un video di uno scherzo che se pubblicassi su YouTube chiameresti "Scherzo EPICO finito MALE".' },
            { id: 7, cat: 1, pts: 8, icon: '📱', title: 'Photobombing', desc: 'Finisci in un TikTok di gente a caso, trovalo e mandalo in giro al gruppo.' },
            { id: 8, cat: 1, pts: 10, icon: '🎤', title: 'Aura Intervista', desc: 'Fai un\'intervista ad un telegiornale, giornale, tiktoker ecc. e cita esplicitamente: monzambano, aura, fabio, dybala, fortnite.' },
            { id: 11, cat: 1, pts: 5, icon: '📈', title: 'Social Manager', desc: 'Convinci tutti ad aprire un profilo TikTok di gruppo per le cose più random. Il tuo TikTok deve essere il più virale.' },
            { id: 13, cat: 1, pts: 5, icon: '💸', title: 'Finanza Spicciola', desc: 'Replica in almeno 3/4 posti i classici TikTok "what can I buy at ... with one EURO".' },
            { id: 24, cat: 1, pts: 7, icon: '🃏', title: 'Joker di Valencia', desc: 'Replica tutto il video di Joker "la mia prima volta in Sardegna". Sostituisci il meno possibile, a parte "la mia prima volta a Valencia".' },
            { id: 27, cat: 1, pts: 4, icon: '🖼️', title: 'Meme Creator', desc: 'Crea uno sticker WhatsApp con soggetto uno del team per OGNI singola emozione che riesci a catturare.' },

            // SCHERZI & CAOS (Categoria 3)
            { id: 4, cat: 2, pts: 4, icon: '🧠', title: 'Il Manipolatore', desc: 'Convinci 2/3 del gruppo di una cosa totalmente falsa.' },
            { id: 5, cat: 2, pts: 5, icon: '💦', title: 'Battesimo Forzato', desc: 'Convinci uno del gruppo a farsi il bagno completamente vestito.' },
            { id: 10, cat: 2, pts: 6, icon: '✍️', title: 'VIP per Caso', desc: 'Aspetta che uno sconosciuto sia da solo e corri lì chiedendo un autografo. Obiettivo: farlo agitare e fargli chiedere foto/autografo.' },
            { id: 14, cat: 2, pts: 5, icon: '🔮', title: 'Nostradamus', desc: 'Durante la notte, scrivi su un foglio sigillato una previsione assurda su cosa farà uno del gruppo il giorno dopo (ripetibile ogni giorno).' },
            { id: 18, cat: 2, pts: 6, icon: '⏰', title: 'Hacker del Sonno', desc: 'Scopri la password del telefono di qualcuno e mettigli la sveglia dopo due ore che si è addormentato.' },
            { id: 19, cat: 2, pts: 8, icon: '🕰️', title: 'Viaggiatore del Tempo', desc: 'Cambia tutti gli orari dei telefoni e convinci tutti che sono le 12 quando in realtà sono le 6.' },
            { id: 25, cat: 2, pts: 5, icon: '👃', title: 'Il Rituale', desc: 'Fai "tocca naso tocca verde" ogni ora per un giorno intero (usa sveglie). Agitati un casino e arrabbiati se qualcuno non lo fa.' },
            { id: 26, cat: 2, pts: 6, icon: '🚨', title: 'Responsabile Sicurezza', desc: 'Una notte, organizza e fai eseguire un test di evacuazione antincendio generale.' },

            // IMPRESE ASSURDE (Categoria 4)
            { id: 6, cat: 3, pts: 7, icon: '🏆', title: 'Il Collezionista', desc: 'Torna a casa a fine vacanza con un "oggetto trofeo" totalmente assurdo e inaspettato.' },
            { id: 9, cat: 3, pts: 5, icon: '🎰', title: 'All-In!', desc: 'Se c\'è un casinò entra con 5 euro, vai tutto fiero alla roulette e punta "tutto sul negro" (citando la tipica battuta spagnola sulle puntate sul nero).' },
            { id: 15, cat: 3, pts: 9, icon: '🌅', title: 'Fotografo Paesaggista', desc: 'Convinci la parte maschile a fare un Nutscapes (foto delle palle con il tramonto o l\'alba come sfondo). Ripetibile per ogni maschio.' },
            { id: 20, cat: 3, pts: 10, icon: '🐉', title: 'Speedrunner', desc: 'Starta una vanilla di Minecraft in vacanza e batti il drago.' },
            { id: 21, cat: 3, pts: 2, icon: '🎮', title: 'Curatore AppStore', desc: 'Trova il miglior gioco mobile di sempre e costringi tutti a scaricarlo.' },
            { id: 22, cat: 3, pts: 3, icon: '🔥', title: 'Cacciatore di Fuoco', desc: 'Portami a casa un buffo accendino Clipper come souvenir.' },
            { id: 23, cat: 3, pts: 5, icon: '⚽', title: 'Joga Bonito', desc: 'Gioca a calcio con un gruppo di spagnoli e tieni come sottofondo dal telefono le canzoni brasiliane.' },
            { id: 28, cat: 3, pts: 9, icon: '👑', title: 'Capo del 67', desc: 'Crea il contest del 67: scegli le regole e dopo un’attenta selezione eleggi ufficialmente il capo del 67.' }
        ];

        const categories = [
            "🗣️ Parlato", 
            "📱 Social", 
            "🤡 Scherzi", 
            "🔥 Imprese"
        ];

        let state = JSON.parse(localStorage.getItem('manu_advancements')) || {};
        let currentTab = 0;
        let selectedMission = null;

        const maxPoints = missions.reduce((sum, m) => sum + m.pts, 0);

        function init() {
            renderTabs();
            renderCategory(currentTab);
            updateProgress();
        }

        function renderTabs() {
            const container = document.getElementById('tabsContainer');
            container.innerHTML = '';
            categories.forEach((cat, index) => {
                const div = document.createElement('div');
                div.className = `tab ${index === currentTab ? 'active' : ''}`;
                div.innerText = cat;
                div.onclick = () => {
                    currentTab = index;
                    renderTabs();
                    renderCategory(currentTab);
                };
                container.appendChild(div);
            });
        }

        function renderCategory(catIndex) {
            const container = document.getElementById('treeContainer');
            container.innerHTML = '';
            
            const filteredMissions = missions.filter(m => m.cat === catIndex);
            
            filteredMissions.forEach(m => {
                const isCompleted = state[m.id];
                const div = document.createElement('div');
                div.className = `advancement ${isCompleted ? 'completed' : ''}`;
                div.onclick = () => openModal(m);

                div.innerHTML = `
                    <div class="adv-icon">${m.icon}</div>
                    <div class="adv-info">
                        <div class="adv-title">${m.title}</div>
                        <div class="adv-pts">${m.pts} Pt ${isCompleted ? '✓' : ''}</div>
                    </div>
                `;
                container.appendChild(div);
            });
        }

        function openModal(mission) {
            selectedMission = mission;
            const isCompleted = state[mission.id];

            document.getElementById('mIcon').innerText = mission.icon;
            document.getElementById('mTitle').innerText = mission.title;
            document.getElementById('mDesc').innerText = mission.desc;
            
            const btn = document.getElementById('btnComplete');
            if (isCompleted) {
                btn.innerText = "ANNULLA COMPLETAMENTO";
                btn.style.color = "#ff5555";
            } else {
                btn.innerText = "SEGNA COME COMPLETATO";
                btn.style.color = "var(--mc-text-green)";
            }
            
            btn.onclick = () => toggleComplete(mission.id);
            document.getElementById('modalOverlay').style.display = 'flex';
        }

        function closeModal() {
            document.getElementById('modalOverlay').style.display = 'none';
            selectedMission = null;
        }

        function toggleComplete(id) {
            const wasCompleted = state[id];
            
            if (wasCompleted) {
                delete state[id];
            } else {
                state[id] = true;
                showToast(selectedMission.title);
            }
            
            localStorage.setItem('manu_advancements', JSON.stringify(state));
            updateProgress();
            renderCategory(currentTab);
            closeModal();
        }

        function updateProgress() {
            let currentPoints = 0;
            missions.forEach(m => {
                if (state[m.id]) currentPoints += m.pts;
            });

            const percentage = Math.min(100, Math.max(0, (currentPoints / maxPoints) * 100));
            
            document.getElementById('xpFill').style.width = percentage + '%';
            document.getElementById('xpLevel').innerText = `${currentPoints} PUNTI`;
        }

        function showToast(title) {
            const toast = document.getElementById('toast');
            document.getElementById('toastDesc').innerText = title;
            toast.classList.add('show');
            setTimeout(() => {
                toast.classList.remove('show');
            }, 3000);
        }

        // Init
        init();
    </script>
</body>
</html>
