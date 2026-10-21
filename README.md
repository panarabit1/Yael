<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>⚡ NEON BRAWL ⚡</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Press+Start+2P&display=swap" rel="stylesheet">
    <style>
        * {
            user-select: none;
            -webkit-user-select: none;
            -webkit-touch-callout: none;
            touch-action: manipulation;
        }

        body {
            font-family: 'Orbitron', sans-serif;
            background-color: #030308;
            color: #fff;
            overflow: hidden;
        }

        .font-pixel {
            font-family: 'Press Start 2P', monospace;
        }

        /* Neon Text & Box Styling */
        .neon-text-cyan {
            text-shadow: 0 0 5px #00f3ff, 0 0 10px #00f3ff, 0 0 20px #00f3ff;
        }
        .neon-text-magenta {
            text-shadow: 0 0 5px #ff007f, 0 0 10px #ff007f, 0 0 20px #ff007f;
        }
        .neon-text-yellow {
            text-shadow: 0 0 5px #ffe600, 0 0 10px #ffe600;
        }

        /* Scanline CRT Overlay */
        .scanlines {
            position: relative;
        }
        .scanlines::before {
            content: " ";
            display: block;
            position: absolute;
            top: 0; left: 0; bottom: 0; right: 0;
            background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.35) 50%);
            background-size: 100% 4px;
            z-index: 20;
            pointer-events: none;
            opacity: 0.7;
        }

        /* Arcade Touch Controls Styling */
        .btn-arcade {
            position: relative;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            font-weight: 800;
            user-select: none;
            touch-action: none;
            cursor: pointer;
            transition: transform 0.05s ease, background-color 0.1s ease, box-shadow 0.1s ease;
            box-shadow: 0 5px 0 rgba(0,0,0,0.6), 0 0 12px currentColor;
        }

        .btn-arcade:active, .btn-arcade.is-active {
            transform: translateY(4px);
            box-shadow: 0 1px 0 rgba(0,0,0,0.6), 0 0 22px currentColor;
            filter: brightness(1.3);
        }

        .bar-fill {
            transition: width 0.15s ease-out;
        }

        .vignette {
            box-shadow: inset 0 0 80px rgba(0,0,0,0.85);
        }
    </style>
</head>
<body class="w-screen h-screen flex flex-col justify-between items-center p-2 sm:p-4 bg-slate-950 scanlines select-none overflow-hidden">

    <!-- TOP HEADER / HUD -->
    <header class="w-full max-w-5xl z-30 flex flex-col gap-2 bg-slate-900/90 backdrop-blur-md p-3 rounded-xl border border-cyan-500/30 shadow-2xl">
        <div class="flex justify-between items-center font-pixel text-xs sm:text-sm">
            <div class="flex items-center gap-2">
                <span class="text-yellow-400 animate-pulse text-base sm:text-lg">⚡</span>
                <h1 class="neon-text-cyan font-black tracking-wider text-cyan-300">NEON BRAWL</h1>
            </div>
            
            <div class="bg-black/70 px-3 py-1 rounded-lg border border-purple-500/50 flex gap-3 text-center">
                <span id="round-display" class="neon-text-yellow text-yellow-300">RONDA 1</span>
                <span class="text-slate-600">·</span>
                <span id="score-display" class="text-emerald-400">PUNTOS 0</span>
            </div>
        </div>

        <!-- HEALTH & SPECIAL BARS -->
        <div class="grid grid-cols-2 gap-3 sm:gap-6 mt-1">
            <!-- PLAYER -->
            <div class="flex flex-col gap-1">
                <div class="flex justify-between items-center text-xs font-bold">
                    <span class="text-emerald-400 flex items-center gap-1">🟢 TU VIDA</span>
                    <span id="player-hp-text" class="text-emerald-300">100%</span>
                </div>
                <div class="w-full h-4 sm:h-5 bg-slate-950 rounded-full p-0.5 border border-emerald-500/40 relative overflow-hidden shadow-[0_0_10px_rgba(16,185,129,0.2)]">
                    <div id="player-hp-bar" class="h-full bg-gradient-to-r from-emerald-600 to-teal-400 rounded-full bar-fill shadow-[0_0_12px_#10b981]" style="width: 100%;"></div>
                </div>
                <div class="w-full h-2 bg-slate-950 rounded-full p-0.5 border border-cyan-500/30 mt-0.5 relative overflow-hidden">
                    <div id="player-sp-bar" class="h-full bg-gradient-to-r from-cyan-500 to-blue-500 rounded-full bar-fill shadow-[0_0_8px_#06b6d4]" style="width: 0%;"></div>
                </div>
            </div>

            <!-- ENEMY -->
            <div class="flex flex-col gap-1">
                <div class="flex justify-between items-center text-xs font-bold">
                    <span id="enemy-hp-text" class="text-rose-400">100%</span>
                    <span class="text-rose-400 flex items-center gap-1">ENEMIGO 🔴</span>
                </div>
                <div class="w-full h-4 sm:h-5 bg-slate-950 rounded-full p-0.5 border border-rose-500/40 relative overflow-hidden shadow-[0_0_10px_rgba(244,63,94,0.2)]">
                    <div id="enemy-hp-bar" class="h-full bg-gradient-to-l from-rose-600 to-pink-500 rounded-full bar-fill shadow-[0_0_12px_#f43f5e] float-right" style="width: 100%;"></div>
                </div>
                <div class="text-right text-[10px] text-rose-300/70 font-semibold tracking-wider">CYBER THUG</div>
            </div>
        </div>
    </header>

    <!-- GAME CANVAS AREA -->
    <main class="relative w-full max-w-5xl flex-1 my-2 rounded-2xl overflow-hidden border-2 border-slate-800 shadow-[0_0_30px_rgba(0,243,255,0.15)] flex justify-center items-center bg-black">
        <canvas id="gameCanvas" class="w-full h-full object-contain block"></canvas>
        
        <div class="absolute inset-0 pointer-events-none vignette border border-cyan-500/20 rounded-2xl"></div>

        <!-- OVERLAY MODAL -->
        <div id="overlay-screen" class="absolute inset-0 bg-slate-950/90 backdrop-blur-md flex flex-col justify-center items-center p-6 text-center z-40 transition-opacity duration-300">
            <h2 id="overlay-title" class="text-3xl sm:text-5xl font-black italic neon-text-magenta mb-3 tracking-widest">⚡ NEON BRAWL ⚡</h2>
            <p id="overlay-subtitle" class="text-cyan-300 font-pixel text-xs sm:text-sm max-w-md mb-6 leading-relaxed">
                ¡Demuestra quién manda en el barrio cibernético!
            </p>
            
            <div id="overlay-stats" class="hidden mb-6 bg-slate-900/90 p-4 rounded-xl border border-purple-500/40 text-center font-pixel text-xs gap-2 flex-col">
                <div class="text-yellow-400" id="stat-round">Rondas Alcanzadas: 1</div>
                <div class="text-emerald-400" id="stat-score">Puntuación Total: 0</div>
                <div class="text-cyan-400" id="stat-combo">Máximo Combo: 0x</div>
            </div>

            <button id="start-btn" class="btn-arcade bg-gradient-to-r from-cyan-500 to-fuchsia-600 text-white text-base sm:text-xl px-8 py-4 border-2 border-cyan-300 rounded-xl hover:scale-105 active:scale-95">
                <i class="fa-solid fa-play mr-2"></i> INICIAR PELEA
            </button>
            
            <div class="mt-8 text-slate-400 text-xs flex flex-wrap justify-center gap-4 max-w-lg border-t border-slate-800 pt-4">
                <div><span class="bg-slate-800 px-2 py-1 rounded text-cyan-400">A / D / Flechas</span> Moverse</div>
                <div><span class="bg-slate-800 px-2 py-1 rounded text-cyan-400">W / Arriba</span> Saltar</div>
                <div><span class="bg-slate-800 px-2 py-1 rounded text-amber-400">J</span> Puño</div>
                <div><span class="bg-slate-800 px-2 py-1 rounded text-rose-400">K</span> Patada</div>
                <div><span class="bg-slate-800 px-2 py-1 rounded text-fuchsia-400">L</span> Especial</div>
            </div>
        </div>
    </main>

    <!-- TOUCH CONTROLS -->
    <footer class="w-full max-w-5xl z-30 flex justify-between items-center gap-2 p-1 sm:p-2">
        <!-- DIRECTIONAL CONTROLS -->
        <div class="flex gap-2">
            <button id="btn-left" class="btn-arcade w-14 h-14 sm:w-16 sm:h-16 bg-slate-800 text-cyan-400 text-xl border border-cyan-500/50 rounded-xl active:bg-cyan-500 active:text-black">
                <i class="fa-solid fa-arrow-left pointer-events-none"></i>
            </button>
            <button id="btn-right" class="btn-arcade w-14 h-14 sm:w-16 sm:h-16 bg-slate-800 text-cyan-400 text-xl border border-cyan-500/50 rounded-xl active:bg-cyan-500 active:text-black">
                <i class="fa-solid fa-arrow-right pointer-events-none"></i>
            </button>
            <button id="btn-jump" class="btn-arcade w-14 h-14 sm:w-16 sm:h-16 bg-slate-800 text-cyan-400 text-xl border border-cyan-500/50 rounded-xl active:bg-cyan-500 active:text-black ml-1">
                <i class="fa-solid fa-arrow-up pointer-events-none"></i>
            </button>
        </div>

        <!-- ACTION CONTROLS -->
        <div class="flex gap-2">
            <button id="btn-punch" class="btn-arcade w-14 h-14 sm:w-16 sm:h-16 bg-amber-950/90 text-amber-400 text-sm border border-amber-500/60 rounded-xl active:bg-amber-400 active:text-black flex flex-col pointer-events-auto">
                <span class="text-[10px] sm:text-xs font-bold pointer-events-none">PUÑO</span>
                <span class="font-pixel text-[9px] opacity-70 pointer-events-none">J</span>
            </button>
            <button id="btn-kick" class="btn-arcade w-14 h-14 sm:w-16 sm:h-16 bg-rose-950/90 text-rose-400 text-sm border border-rose-500/60 rounded-xl active:bg-rose-500 active:text-black flex flex-col pointer-events-auto">
                <span class="text-[10px] sm:text-xs font-bold pointer-events-none">PATADA</span>
                <span class="font-pixel text-[9px] opacity-70 pointer-events-none">K</span>
            </button>
            <button id="btn-special" class="btn-arcade w-14 h-14 sm:w-16 sm:h-16 bg-fuchsia-950/90 text-fuchsia-300 text-sm border border-fuchsia-500/80 rounded-xl active:bg-fuchsia-500 active:text-white flex flex-col pointer-events-auto relative overflow-hidden">
                <span class="text-[9px] sm:text-xs font-bold pointer-events-none">ESPECIAL</span>
                <span class="font-pixel text-[9px] opacity-70 pointer-events-none">L</span>
                <div id="btn-special-glow" class="absolute inset-0 bg-fuchsia-500/30 pointer-events-none hidden animate-pulse"></div>
            </button>
        </div>
    </footer>

    <!-- ENGINE CODE -->
    <script>
        /**
         * Sound FX Synthesizer
         */
        class SoundFX {
            constructor() {
                this.ctx = null;
            }

            init() {
                if (!this.ctx) {
                    const AudioContext = window.AudioContext || window.webkitAudioContext;
                    this.ctx = new AudioContext();
                }
                if (this.ctx && this.ctx.state === 'suspended') {
                    this.ctx.resume();
                }
            }

            playPunch() {
                if (!this.ctx) return;
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'triangle';
                osc.frequency.setValueAtTime(160, this.ctx.currentTime);
                osc.frequency.exponentialRampToValueAtTime(30, this.ctx.currentTime + 0.12);
                
                gain.gain.setValueAtTime(0.35, this.ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.12);

                osc.connect(gain);
                gain.connect(this.ctx.destination);

                osc.start();
                osc.stop(this.ctx.currentTime + 0.12);
            }

            playKick() {
                if (!this.ctx) return;
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'square';
                osc.frequency.setValueAtTime(230, this.ctx.currentTime);
                osc.frequency.exponentialRampToValueAtTime(40, this.ctx.currentTime + 0.18);

                gain.gain.setValueAtTime(0.4, this.ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.18);

                osc.connect(gain);
                gain.connect(this.ctx.destination);

                osc.start();
                osc.stop(this.ctx.currentTime + 0.18);
            }

            playSpecial() {
                if (!this.ctx) return;
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'sawtooth';
                osc.frequency.setValueAtTime(110, this.ctx.currentTime);
                osc.frequency.linearRampToValueAtTime(650, this.ctx.currentTime + 0.3);
                osc.frequency.exponentialRampToValueAtTime(80, this.ctx.currentTime + 0.5);

                gain.gain.setValueAtTime(0.5, this.ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.5);

                osc.connect(gain);
                gain.connect(this.ctx.destination);

                osc.start();
                osc.stop(this.ctx.currentTime + 0.5);
            }

            playHit() {
                if (!this.ctx) return;
                const bufferSize = this.ctx.sampleRate * 0.08;
                const buffer = this.ctx.createBuffer(1, bufferSize, this.ctx.sampleRate);
                const data = buffer.getChannelData(0);
                for (let i = 0; i < bufferSize; i++) {
                    data[i] = Math.random() * 2 - 1;
                }

                const noise = this.ctx.createBufferSource();
                noise.buffer = buffer;

                const filter = this.ctx.createBiquadFilter();
                filter.type = 'bandpass';
                filter.frequency.value = 900;

                const gain = this.ctx.createGain();
                gain.gain.setValueAtTime(0.4, this.ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.08);

                noise.connect(filter);
                filter.connect(gain);
                gain.connect(this.ctx.destination);

                noise.start();
            }

            playWin() {
                if (!this.ctx) return;
                const notes = [261.63, 329.63, 392.00, 523.25];
                notes.forEach((freq, idx) => {
                    const osc = this.ctx.createOscillator();
                    const gain = this.ctx.createGain();
                    osc.type = 'sine';
                    osc.frequency.value = freq;
                    gain.gain.setValueAtTime(0.25, this.ctx.currentTime + idx * 0.12);
                    gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + idx * 0.12 + 0.3);
                    osc.connect(gain);
                    gain.connect(this.ctx.destination);
                    osc.start(this.ctx.currentTime + idx * 0.12);
                    osc.stop(this.ctx.currentTime + idx * 0.12 + 0.3);
                });
            }

            playLose() {
                if (!this.ctx) return;
                const notes = [280, 230, 180, 130];
                notes.forEach((freq, idx) => {
                    const osc = this.ctx.createOscillator();
                    const gain = this.ctx.createGain();
                    osc.type = 'sawtooth';
                    osc.frequency.value = freq;
                    gain.gain.setValueAtTime(0.25, this.ctx.currentTime + idx * 0.15);
                    gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + idx * 0.15 + 0.25);
                    osc.connect(gain);
                    gain.connect(this.ctx.destination);
                    osc.start(this.ctx.currentTime + idx * 0.15);
                    osc.stop(this.ctx.currentTime + idx * 0.15 + 0.25);
                });
            }
        }

        const audio = new SoundFX();

        /**
         * Canvas & Global Setup
         */
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        const GAME_WIDTH = 960;
        const GAME_HEIGHT = 540;
        const GROUND_Y = 460;

        function resizeCanvas() {
            canvas.width = GAME_WIDTH;
            canvas.height = GAME_HEIGHT;
        }
        resizeCanvas();

        let gameState = 'MENU'; // 'MENU', 'PLAYING', 'ROUND_CLEAR', 'GAME_OVER'
        let round = 1;
        let score = 0;
        let comboCount = 0;
        let maxCombo = 0;
        let comboTimer = 0;

        let particles = [];
        let floatingTexts = [];

        // Global Key States
        const keys = {
            left: false,
            right: false,
            up: false,
            punch: false,
            kick: false,
            special: false
        };

        // Keyboard Controls
        window.addEventListener('keydown', (e) => {
            if (e.repeat) return;
            const key = e.key.toLowerCase();
            
            if (['arrowleft', 'arrowright', 'arrowup', 'arrowdown', ' '].includes(key)) {
                e.preventDefault();
            }

            if (key === 'a' || key === 'arrowleft') keys.left = true;
            if (key === 'd' || key === 'arrowright') keys.right = true;
            if (key === 'w' || key === 'arrowup') keys.up = true;
            if (key === 'j') triggerAttack('punch');
            if (key === 'k') triggerAttack('kick');
            if (key === 'l') triggerAttack('special');
        });

        window.addEventListener('keyup', (e) => {
            const key = e.key.toLowerCase();
            if (key === 'a' || key === 'arrowleft') keys.left = false;
            if (key === 'd' || key === 'arrowright') keys.right = false;
            if (key === 'w' || key === 'arrowup') keys.up = false;
        });

        // Trigger Attack Action Function
        function triggerAttack(type) {
            audio.init();
            if (gameState !== 'PLAYING') return;

            if (type === 'punch') player.punch();
            else if (type === 'kick') player.kick();
            else if (type === 'special') player.performSpecial();
        }

        /**
         * Robust Multi-Touch & Pointer Event Listener Setup
         */
        function bindTouchButton(id, keyProp, attackAction = null) {
            const btn = document.getElementById(id);
            if (!btn) return;

            const press = (e) => {
                if (e) e.preventDefault();
                audio.init();
                btn.classList.add('is-active');
                if (keyProp) keys[keyProp] = true;
                if (attackAction) triggerAttack(attackAction);
            };

            const release = (e) => {
                if (e) e.preventDefault();
                btn.classList.remove('is-active');
                if (keyProp) keys[keyProp] = false;
            };

            // Touch Events
            btn.addEventListener('touchstart', press, { passive: false });
            btn.addEventListener('touchend', release, { passive: false });
            btn.addEventListener('touchcancel', release, { passive: false });

            // Mouse Events for Desktop Testing
            btn.addEventListener('mousedown', press);
            btn.addEventLis
