# gaelmariano
juego de peleas
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>El Infierno Eterno - Arena de Combate</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@700;900&family=Rajdhani:wght@600;700&display=swap" rel="stylesheet">
    <style>
        * {
            box-sizing: border-box;
            user-select: none;
            -webkit-user-select: none;
        }
        body {
            margin: 0;
            padding: 0;
            background-color: #090303;
            color: #f8fafc;
            font-family: 'Rajdhani', sans-serif;
            overflow: hidden;
            height: 100vh;
            width: 100vw;
            display: flex;
            flex-direction: column;
        }
        .font-epic {
            font-family: 'Cinzel', serif;
        }
        /* Efectos de brillo infernal */
        .lava-glow {
            box-shadow: 0 0 15px rgba(239, 68, 68, 0.6), inset 0 0 10px rgba(249, 115, 22, 0.4);
        }
        .vs-text {
            background: linear-gradient(180deg, #fef08a 0%, #f97316 45%, #dc2626 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            filter: drop-shadow(0 0 8px rgba(220, 38, 38, 0.9));
        }
        .skill-btn {
            transition: transform 0.1s ease, filter 0.15s ease;
            background: radial-gradient(circle at center, #2a0a0a 0%, #0f0404 100%);
            border: 2px solid #7f1d1d;
        }
        .skill-btn:active {
            transform: scale(0.92);
        }
        .skill-ready {
            border-color: #f97316;
            box-shadow: 0 0 12px rgba(249, 115, 22, 0.5);
        }
        canvas {
            display: block;
            width: 100%;
            height: 100%;
        }
    </style>
</head>
<body class="relative bg-black">

    <!-- HUD SUPERIOR (Estilo Mockup) -->
    <header class="w-full z-20 px-2 sm:px-6 pt-2 sm:pt-3 pb-1 flex items-center justify-between gap-2 bg-gradient-to-b from-black/90 via-black/60 to-transparent pointer-events-none">
        
        <!-- Jugador 1: Diablo Serpiente -->
        <div class="flex items-center gap-2 sm:gap-3 flex-1 max-w-[42%]">
            <!-- Retrato P1 -->
            <div class="w-12 h-12 sm:w-16 sm:h-16 rounded-lg border-2 border-red-600 bg-gradient-to-br from-red-950 to-black flex-shrink-0 overflow-hidden relative lava-glow flex items-center justify-center">
                <svg viewBox="0 0 64 64" class="w-full h-full">
                    <circle cx="32" cy="34" r="18" fill="#450a0a"/>
                    <!-- Cuernos -->
                    <path d="M18 24 Q10 10 22 6 Q20 18 25 22 Z" fill="#dc2626"/>
                    <path d="M46 24 Q54 10 42 6 Q44 18 39 22 Z" fill="#dc2626"/>
                    <!-- Ojos brillando -->
                    <polygon points="23,30 29,33 23,35" fill="#facc15"/>
                    <polygon points="41,30 35,33 41,35" fill="#facc15"/>
                    <!-- Serpientes laterales -->
                    <path d="M12 48 Q6 36 14 28" stroke="#ef4444" stroke-width="3" fill="none"/>
                    <path d="M52 48 Q58 36 50 28" stroke="#ef4444" stroke-width="3" fill="none"/>
                </svg>
            </div>
            <div class="flex-1 min-w-0">
                <div class="flex justify-between items-baseline mb-1">
                    <span class="font-epic text-xs sm:text-base md:text-lg font-bold text-red-100 truncate drop-shadow">Diablo Serpiente</span>
                    <span id="p1-hp-text" class="text-[10px] sm:text-xs font-bold text-amber-300 ml-1">1000 / 1000</span>
                </div>
                <!-- Barra de Vida P1 -->
                <div class="w-full h-3 sm:h-4 bg-zinc-950/90 border border-red-900/80 rounded-sm overflow-hidden p-[1px] shadow-inner">
                    <div id="p1-hp-bar" class="h-full bg-gradient-to-r from-red-800 via-red-600 to-orange-500 transition-all duration-150 rounded-sm" style="width: 100%;"></div>
                </div>
                <!-- Barra de Energía / Furia -->
                <div class="w-3/4 h-1.5 bg-zinc-950 border border-amber-900/60 rounded-sm overflow-hidden mt-1">
                    <div id="p1-energy-bar" class="h-full bg-gradient-to-r from-amber-600 to-yellow-400 transition-all duration-100" style="width: 100%;"></div>
                </div>
            </div>
        </div>

        <!-- Centro: Emblema VS y Temporizador -->
        <div class="flex flex-col items-center justify-center px-1 sm:px-3 flex-shrink-0">
            <div class="font-epic text-2xl sm:text-4xl font-black italic vs-text leading-none tracking-tighter">VS</div>
            <div id="timer-display" class="mt-0.5 px-2 py-0.5 bg-black/80 border border-red-800/70 rounded text-xs sm:text-sm font-bold text-amber-200 tracking-widest">
                5:00
    
            </div>
        </div>

        <!-- Jugador 2: El Jinete Infernal -->
        <div class="flex items-center flex-row-reverse gap-2 sm:gap-3 flex-1 max-w-[42%]">
            <!-- Retrato P2 -->
            <div class="w-12 h-12 sm:w-16 sm:h-16 rounded-lg border-2 border-orange-500 bg-gradient-to-bl from-orange-950 to-black flex-shrink-0 overflow-hidden relative lava-glow flex items-center justify-center">
                <svg viewBox="0 0 64 64" class="w-full h-full">
                    <rect width="64" height="64" fill="#1c0a05"/>
                    <!-- Espada ígnea detrás -->
                    <line x1="12" y1="52" x2="52" y2="12" stroke="#f97316" stroke-width="5" stroke-linecap="round"/>
                    <!-- Cabeza del Jinete -->
                    <circle cx="32" cy="28" r="13" fill="#27272a"/>
                    <path d="M19 24 L32 12 L45 24 Z" fill="#991b1b"/>
                    <circle cx="27" cy="29" r="2" fill="#ef4444"/>
                    <circle cx="37" cy="29" r="2" fill="#ef4444"/>
                    <!-- Tres hocicos de Cerbero abajo -->
                    <circle cx="20" cy="48" r="7" fill="#18181b" stroke="#ea580c" stroke-width="1.5"/>
                    <circle cx="32" cy="50" r="8" fill="#18181b" stroke="#ea580c" stroke-width="1.5"/>
                    <circle cx="44" cy="48" r="7" fill="#18181b" stroke="#ea580c" stroke-width="1.5"/>
                </svg>
            </div>
            <div class="flex-1 min-w-0 text-right">
                <div class="flex flex-row-reverse justify-between items-baseline mb-1">
                    <span class="font-epic text-xs sm:text-base md:text-lg font-bold text-orange-100 truncate drop-shadow">El jinete Infernal</span>
                    <span id="p2-hp-text" class="text-[10px] sm:text-xs font-bold text-amber-300 mr-1">1000 / 1000</span>
                </div>
                <!-- Barra de Vida P2 -->
                <div class="w-full h-3 sm:h-4 bg-zinc-950/90 border border-orange-900/80 rounded-sm overflow-hidden p-[1px] shadow-inner flex justify-end">
                    <div id="p2-hp-bar" class="h-full bg-gradient-to-l from-red-800 via-orange-600 to-amber-400 transition-all duration-150 rounded-sm" style="width: 100%;"></div>
                </div>
                <!-- Barra de Energía P2 -->
                <div class="w-3/4 h-1.5 bg-zinc-950 border border-amber-900/60 rounded-sm overflow-hidden mt-1 ml-auto flex justify-end">
                    <div id="p2-energy-bar" class="h-full bg-gradient-to-l from-amber-600 to-yellow-400 transition-all duration-100" style="width: 100%;"></div>
                </div>
            </div>
        </div>
    </header>

    <!-- ÁREA CENTRAL DE COMBATE (CANVAS) -->
    <main class="relative flex-1 w-full h-full overflow-hidden">
        <canvas id="arenaCanvas"></canvas>

        <!-- Mensaje de Anuncio Central (¡PELEA!, ¡K.O.!, etc.) -->
        <div id="announcer-overlay" class="pointer-events-none absolute inset-0 flex items-center justify-center z-10">
            <h2 id="announcer-text" class="font-epic text-4xl sm:text-6xl md:text-7xl font-black text-amber-400 tracking-widest uppercase drop-shadow-[0_4px_20px_rgba(220,38,38,1)] opacity-0 transition-all duration-300 scale-75">
                ¡A PELEAR!
            </h2>
        </div>

        <!-- PANTALLA DE INICIO / SELECCIÓN DE MODO -->
        <div id="menu-modal" class="absolute inset-0 z-30 bg-black/85 backdrop-blur-sm flex items-center justify-center p-4">
            <div class="max-w-lg w-full bg-gradient-to-b from-zinc-900 via-red-950/90 to-black border-2 border-red-700/80 rounded-2xl p-6 sm:p-8 text-center shadow-[0_0_50px_rgba(220,38,38,0.4)]">
                <p class="text-xs uppercase tracking-[0.3em] text-orange-400 font-bold mb-1">Arena Volcánica</p>
                <h1 class="font-epic text-3xl sm:text-4xl font-black text-white mb-2 tracking-wide drop-shadow-[0_2px_10px_rgba(239,68,68,0.8)]">
                    EL INFIERNO ETERNO
                </h1>
                <p class="text-sm text-zinc-300 mb-6">
                    Elige tu modo de combate en las grietas de lava.
                </p>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 mb-6">
                    <button onclick="startGame('1P')" class="group relative overflow-hidden rounded-xl bg-gradient-to-r from-red-700 to-orange-600 p-4 font-bold text-white shadow-lg hover:from-red-600 hover:to-orange-500 transition-all border border-amber-400/40">
                        <div class="font-epic text-lg">1 JUGADOR</div>
                        <div class="text-xs text-amber-100/90 font-normal mt-0.5">busca rival</div>
                    </button>
                    <button onclick="startGame('2P')" class="group relative overflow-hidden rounded-xl bg-gradient-to-r from-zinc-800 to-red-950 p-4 font-bold text-white shadow-lg hover:from-zinc-700 hover:to-red-900 transition-all border border-red-500/50">
                        <div class="font-epic text-lg">2 JUGADORES</div>
                        <div class="text-xs text-red-200/90 font-normal mt-0.5">2 jugadores</div>
                    </button>
                </div>

                <!-- Guía rápida de controles -->
                <div class="bg-black/60 rounded-xl p-3.5 border border-red-900/50 text-left text-xs text-zinc-300 space-y-2">
                    <div class="flex justify-between items-center border-b border-zinc-800 pb-1.5">
                        <span class="font-bold text-red-400">J1 (Diablo Serpiente):</span>
                        <span>Mover: <kbd class="px-1.5 py-0.5 bg-zinc-800 rounded text-amber-300">A</kbd> <kbd class="px-1.5 py-0.5 bg-zinc-800 rounded text-amber-300">D</kbd> | Saltar: <kbd class="px-1.5 py-0.5 bg-zinc-800 rounded text-amber-300">W </kbd></span>
                    </div>
                    <div class="flex justify-between items-center border-b border-zinc-800 pb-1.5">
                        <span class="font-bold text-orange-400">Habilidades J1 / Clic:</span>
                        <span><kbd class="px-1.5 py-0.5 bg-zinc-800 rounded text-amber-300">Q</kbd> Garras | <kbd class="px-1.5 py-0.5 bg-zinc-800 rounded text-amber-300">E</kbd> Serpiente Fuego | <kbd class="px-1.5 py-0.5 bg-zinc-800 rounded text-amber-300">R</kbd> Furia</span>
                    </div>
                    <div class="flex justify-between items-center">
                        <span class="font-bold text-amber-400">J2 (En modo 2P):</span>
                        <span>Flechas <kbd class="px-1.5 py-0.5 bg-zinc-800 rounded text-amber-300">←↑→</kbd> + <kbd class="px-1.5 py-0.5 bg-zinc-800 rounded text-amber-300">1</kbd> <kbd class="px-1.5 py-0.5 bg-zinc-800 rounded text-amber-300">2</kbd> <kbd class="px-1.5 py-0.5 bg-zinc-800 rounded text-amber-300">3</kbd></span>
                    </div>
                </div>
            </div>
        </div>

        <!-- PANTALLA DE FIN DE PARTIDA -->
        <div id="gameover-modal" class="hidden absolute inset-0 z-30 bg-black/80 backdrop-blur-sm flex items-center justify-center p-4">
            <div class="max-w-md w-full bg-gradient-to-b from-red-950 to-black border-2 border-amber-500/80 rounded-2xl p-6 text-center shadow-[0_0_40px_rgba(245,158,11,0.4)]">
                <p class="text-xs uppercase tracking-[0.25em] text-amber-400 font-bold">Combate Finalizado</p>
                <h2 id="winner-title" class="font-epic text-3xl sm:text-4xl font-black text-white my-2">¡VICTORIA!</h2>
                <p id="winner-subtitle" class="text-sm text-zinc-300 mb-6">El Diablo Serpiente domina el inframundo.</p>
                <div class="flex gap-3 justify-center">
                    <button onclick="restartSameMode()" class="px-6 py-3 rounded-xl bg-gradient-to-r from-red-600 to-orange-600 font-epic font-bold text-white hover:from-red-500 hover:to-orange-500 shadow-lg border border-amber-300/40">
                        Revancha
                    </button>
                    <button onclick="showMenu()" class="px-5 py-3 rounded-xl bg-zinc-800 font-bold text-zinc-200 hover:bg-zinc-700 border border-zinc-600">
                        Menú
                    </button>
                </div>
            </div>
        </div>
    </main>

    <!-- HUD INFERIOR: HABILIDADES Y CONTROLES TÁCTILES (Inspirado en los botones Q, E, R, Atacar, Saltar, Defender del mockup) -->
    <footer class="w-full z-20 px-3 sm:px-8 pb-3 pt-2 bg-gradient-to-t from-black via-black/85 to-transparent flex items-center justify-between gap-2">
        
        <!-- Controles de movimiento izquierdo (Táctil / Ratón) -->
        <div class="flex items-center gap-2">
            <button id="btn-left" class="skill-btn w-12 h-12 sm:w-14 sm:h-14 rounded-xl flex items-center justify-center text-amber-200 active:bg-red-900/60" aria-label="Izquierda">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" stroke-width="2.5" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M15 19l-7-7 7-7"/></svg>
            </button>
            <button id="btn-right" class="skill-btn w-12 h-12 sm:w-14 sm:h-14 rounded-xl flex items-center justify-center text-amber-200 active:bg-red-900/60" aria-label="Derecha">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" stroke-width="2.5" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M9 5l7 7-7 7"/></svg>
            </button>
        </div>

        <!-- Habilidades Centrales (Q, E, R) -->
        <div class="flex items-center gap-2.5 sm:gap-4">
            <!-- Q: Zarpazo / Corte -->
            <button id="btn-skill-q" onclick="triggerPlayerAction('attack')" class="skill-btn skill-ready relative w-12 h-12 sm:w-14 sm:h-14 rounded-lg flex flex-col items-center justify-center group">
                <svg class="w-6 h-6 text-red-400" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M19 3L5 17l2 2L21 5l-2-2zM3 19l2 2 3-3-2-2-3 3z"/>
                </svg>
                <span class="absolute -bottom-2 px-1.5 bg-black border border-red-700 rounded text-[10px] font-bold text-amber-300">Q</span>
            </button>

            <!-- E: Fuego de Serpiente / Embestida -->
            <button id="btn-skill-e" onclick="triggerPlayerAction('projectile')" class="skill-btn skill-ready relative w-12 h-12 sm:w-14 sm:h-14 rounded-lg flex flex-col items-center justify-center">
                <svg class="w-6 h-6 text-orange-400" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M12 2C8 6 6 9.5 6 13a6 6 0 0012 0c0-3.5-2-7-6-11zm0 15a2.5 2.5 0 01-2.5-2.5c0-1.5 1-3 2.5-4.5 1.5 1.5 2.5 3 2.5 4.5A2.5 2.5 0 0112 17z"/>
                </svg>
                <span id="cd-e-overlay" class="absolute inset-0 bg-black/75 rounded-lg hidden items-center justify-center text-xs font-bold text-white"></span>
                <span class="absolute -bottom-2 px-1.5 bg-black border border-red-700 rounded text-[10px] font-bold text-amber-300">E</span>
            </button>

            <!-- R: Furia Infernal (Especial) -->
            <button id="btn-skill-r" onclick="triggerPlayerAction('special')" class="skill-btn skill-ready relative w-12 h-12 sm:w-14 sm:h-14 rounded-lg flex flex-col items-center justify-center">
                <svg class="w-6 h-6 text-yellow-400" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M12 2L9 9l-7 3 7 3 3 7 3-7 7-3-7-3-3-7z"/>
                </svg>
                <span id="cd-r-overlay" class="absolute inset-0 bg-black/75 rounded-lg hidden items-center justify-center text-xs font-bold text-white"></span>
                <span class="absolute -bottom-2 px-1.5 bg-black border border-red-700 rounded text-[10px] font-bold text-amber-300">R</span>
            </button>
        </div>

        <!-- Botones de Acción Derecha (Atacar, Saltar, Defender) -->
        <div class="flex items-center gap-2">
            <button id="btn-jump" onclick="triggerPlayerAction('jump')" class="skill-btn w-12 h-12 sm:w-14 sm:h-14 rounded-full flex flex-col items-center justify-center text-amber-100">
                <svg class="w-5 h-5 text-orange-300" fill="none" stroke="currentColor" stroke-width="2.5" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M5 15l7-7 7 7"/></svg>
                <span class="text-[9px] uppercase tracking-wider text-zinc-300">Saltar</span>
            </button>
            <button id="btn-block" class="skill-btn w-12 h-12 sm:w-14 sm:h-14 rounded-full flex flex-col items-center justify-center text-amber-100">
                <svg class="w-5 h-5 text-red-400" fill="none" stroke="currentColor" stroke-width="2.2" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M12 3l8 4v5c0 5.25-3.5 9.74-8 11-4.5-1.26-8-5.75-8-11V7l8-4z"/></svg>
                <span class="text6-[9px] uppercase tracking-wider text-zinc-300">Escudo</span>
            </button>
        </div>
    </footer>

    <script>
        /**
         * EL INFIERNO ETERNO - Motor de Combate 2D
         * Arte procedural en Canvas, sistema de partículas de lava, física de combate e IA.
         */

        const canvas = document.getElementById('arenaCanvas');
        const ctx = canvas.getContext('2d');

        // Estado global del juego
        let gameState = 'MENU'; // MENU, PLAYING, GAMEOVER
        let gameMode = '1P';    // 1P o 2P
        let matchTime = 167;    // 02:47 como en el mockup
        let lastTime = 0;
        let timerAccumulator = 0;
        let screenShake = 0;

        // Dimensiones dinámicas
        let width = 0;
        let height = 0;
        let groundY = 0;

        // Efectos visuales en pantalla
        let embers = [];
        let lavaCracks = [];
        let projectiles = [];
        let particles = [];
        let floatingTexts = [];
        let castleTowers = [];

        // Control de teclado y botones táctiles
        const keys = {};
        const touchState = { left: false, right: false, block: false };

        // Sistema de audio sintetizado ligero (Web Audio API sin archivos externos)
        let audioCtx = null;
        function playSound(type) {
            try {
                if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                if (audioCtx.state === 'suspended') audioCtx.resume();
                
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                const now = audioCtx.currentTime;

                if (type === 'slash') {
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(320, now);
                    osc.frequency.exponentialRampToValueAtTime(70, now + 0.14);
                    gain.gain.setValueAtTime(0.18, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.14);
                    osc.start(now);
                    osc.stop(now + 0.14);
                } else if (type === 'fireball') {
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(520, now);
                    osc.frequency.exponentialRampToValueAtTime(140, now + 0.25);
                    gain.gain.setValueAtTime(0.2, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.25);
                    osc.start(now);
                    osc.stop(now + 0.25);
                } else if (type === 'hit') {
                    osc.type = 'square';
                    osc.frequency.setValueAtTime(130, now);
                    osc.frequency.exponentialRampToValueAtTime(35, now + 0.18);
                    gain.gain.setValueAtTime(0.25, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.18);
                    osc.start(now);
                    osc.stop(now + 0.18);
                } else if (type === 'special') {
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(180, now);
                    osc.frequency.linearRampToValueAtTime(440, now + 0.15);
                    osc.frequency.exponentialRampToValueAtTime(60, now + 0.45);
                    gain.gain.setValueAtTime(0.28, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.45);
                    osc.start(now);
                    osc.stop(now + 0.45);
                }
            } catch (e) {
                // Ignorar si el navegador bloquea audio antes de interacción
            }
        }

        // Clase Luchador
        class Fighter {
            constructor({ id, name, x, facing, type }) {
                this.id = id;
                this.name = name;
                this.type = type; // 'serpent' o 'cerberus'
                this.x = x;
                this.y = 0;
                this.vx = 0;
                this.vy = 0;
                this.width = 90;
                this.height = 140;
                this.facing = facing; // 1 = derecha, -1 = izquierda
                
                this.maxHp = 1000;
                this.hp = 1000;
                this.maxEnergy = 100;
                this.energy = 100;

                this.isGrounded = true;
                this.isBlocking = false;
                this.attackState = null; // 'melee', 'special'
                this.attackTimer = 0;
                this.hitRegistered = false;

                // Enfriamientos (Cooldowns)
                this.cdProjectile = 0;
                this.cdSpecial = 0;
                this.hurtFlash = 0;
                this.animTime = Math.random() * 10;
            }

            reset(x, facing) {
                this.x = x;
                this.y = groundY;
                this.vx = 0;
                this.vy = 0;
                this.facing = facing;
                this.hp = this.maxHp;
                this.energy = this.maxEnergy;
                this.isGrounded = true;
                this.isBlocking = false;
                this.attackState = null;
                this.attackTimer = 0;
                this.cdProjectile = 0;
                this.cdSpecial = 0;
                this.hurtFlash = 0;
            }

            jump() {
                if (this.isGrounded && !this.attackState) {
                    this.vy = -15.5;
                    this.isGrounded = false;
                    spawnParticles(this.x, groundY, '#f97316', 8);
                }
            }

            meleeAttack() {
                if (this.attackState || this.isBlocking) return;
                this.attackState = 'melee';
                this.attackTimer = 0.28;
                this.hitRegistered = false;
                playSound('slash');
            }

            fireProjectile() {
                if (this.cdProjectile > 0 || this.attackState || this.isBlocking) return;
                this.cdProjectile = 1.6;
                this.attackState = 'cast';
                this.attackTimer = 0.22;
                playSound('fireball');

                const startX = this.x + this.facing * 55;
                const startY = this.y - 75;
                projectiles.push({
                    owner: this.id,
                    type: this.type,
                    x: startX,
                    y: startY,
                    vx: this.facing * 11,
                    radius: 16,
                    damage: 85,
                    life: 2.0
                });
            }

            specialAttack() {
                if (this.cdSpecial > 0 || this.energy < 40 || this.attackState) return;
                this.cdSpecial = 3.5;
                this.energy = Math.max(0, this.energy - 40);
                this.attackState = 'special';
                this.attackTimer = 0.42;
                this.hitRegistered = false;
                this.vx = this.facing * 14; // Embestida rápida
                screenShake = 8;
                playSound('special');
                spawnParticles(this.x, this.y - 60, '#ef4444', 18);
            }

            update(dt, opponent) {
                this.animTime += dt * 5;

                // Reducir cooldowns y regenerar energía
                if (this.cdProjectile > 0) this.cdProjectile = Math.max(0, this.cdProjectile - dt);
                if (this.cdSpecial > 0) this.cdSpecial = Math.max(0, this.cdSpecial - dt);
                if (this.hurtFlash > 0) this.hurtFlash = Math.max(0, this.hurtFlash - dt);
                this.energy = Math.min(this.maxEnergy, this.energy + dt * 12);

                // Orientarse hacia el oponente si no está atacando
                if (!this.attackState && opponent) {
                    this.facing = opponent.x >= this.x ? 1 : -1;
                }

                // Aplicar gravedad y movimiento
                this.x += this.vx;
                this.y += this.vy;

                if (this.y < groundY) {
                    this.vy += 0.72; // Gravedad
                    this.isGrounded = false;
                } else {
                    this.y = groundY;
                    this.vy = 0;
                    this.isGrounded = true;
                }

                // Fricción horizontal
                this.vx *= 0.82;

                // Límites de la arena
                const margin = 65;
                if (this.x < margin) this.x = margin;
                if (this.x > width - margin) this.x = width - margin;

                // Lógica de ventanas de ataque
                if (this.attackState) {
                    this.attackTimer -= dt;

                    // Detección de impacto cuerpo a cuerpo
                    if ((this.attackState === 'melee' || this.attackState === 'special') && !this.hitRegistered && opponent) {
                        const reach = this.attackState === 'special' ? 135 : 110;
                        const distX = (opponent.x - this.x) * this.facing;
                        const distY = Math.abs(opponent.y - this.y);

                        if (distX > -20 && distX < reach && distY < 95) {
                            this.hitRegistered = true;
                            const dmg = this.attackState === 'special' ? 165 : 75;
                            opponent.takeDamage(dmg, this.facing, this.attackState === 'special');
                        }
                    }

                    if (this.attackTimer <= 0) {
                        this.attackState = null;
                    }
                }
            }

            takeDamage(amount, hitDir, isHeavy = false) {
                let finalDmg = amount;
                if (this.isBlocking) {
                    finalDmg = Math.round(amount * 0.25);
                    spawnFloatingText(this.x, this.y - 120, `BLOQUEO -${finalDmg}`, '#93c5fd');
                    spawnParticles(this.x + this.facing * 25, this.y - 70, '#60a5fa', 10);
                } else {
                    this.hurtFlash = 0.2;
                    this.vx = hitDir * (isHeavy ? 15 : 7);
                    if (isHeavy) {
                        this.vy = -7;
                        screenShake = 14;
                    } else {
                        screenShake = 5;
                    }
                    spawnFloatingText(this.x, this.y - 125, `-${finalDmg}`, isHeavy ? '#facc15' : '#ef4444');
                    spawnParticles(this.x, this.y - 70, '#f97316', isHeavy ? 22 : 12);
                }

                playSound('hit');
                this.hp = Math.max(0, this.hp - finalDmg);
            }

            draw(ctx) {
                ctx.save();
                ctx.translate(this.x, this.y);

                // Aura circular de lava bajo los pies (como en la imagen)
                const auraGrad = ctx.createRadialGradient(0, 0, 10, 0, 0, 75);
                auraGrad.addColorStop(0, 'rgba(239, 68, 68, 0.45)');
                auraGrad.addColorStop(0.7, 'rgba(234, 88, 12, 0.15)');
                auraGrad.addColorStop(1, 'rgba(0, 0, 0, 0)');
                ctx.fillStyle = auraGrad;
                ctx.beginPath();
                ctx.ellipse(0, 2, 75, 18, 0, 0, Math.PI * 2);
                ctx.fill();

                // Anillo rúnico brillante en el suelo
                ctx.strokeStyle = this.type === 'serpent' ? 'rgba(239, 68, 68, 0.7)' : 'rgba(249, 115, 22, 0.75)';
                ctx.lineWidth = 2;
                ctx.beginPath();
                ctx.ellipse(0, 2, 58, 13, 0, 0, Math.PI * 2);
                ctx.stroke();

                // Voltear según dirección
                ctx.scale(this.facing, 1);

                // Efecto de parpadeo al recibir daño
                if (this.hurtFlash > 0) {
                    ctx.filter = 'brightness(2.2) saturate(2)';
                }

                if (this.type === 'serpent') {
                    this.drawDiabloSerpiente(ctx);
                } else {
                    this.drawJineteInfernal(ctx);
                }

                // Escudo visual si está defendiendo
                if (this.isBlocking) {
                    ctx.strokeStyle = '#38bdf8';
                    ctx.lineWidth = 3;
                    ctx.fillStyle = 'rgba(56, 189, 248, 0.18)';
                    ctx.beginPath();
                    ctx.arc(28, -70, 52, -Math.PI * 0.45, Math.PI * 0.45);
                    ctx.stroke();
                    ctx.fill();
                }

                ctx.restore();
            }

            // Arte procedural del Jugador 1: Diablo alado con cola y cabezas de serpiente
            drawDiabloSerpiente(ctx) {
                const breathe = Math.sin(this.animTime) * 4;
                const attackSwing = this.attackState ? Math.sin((1 - this.attackTimer / 0.3) * Math.PI) : 0;

                // 1. Alas demoníacas grandes detrás
                ctx.save();
                ctx.translate(-10, -95 + breathe);
                ctx.rotate(-0.2 - attackSwing * 0.25);
                ctx.fillStyle = '#180505';
                ctx.strokeStyle = '#991b1b';
                ctx.lineWidth = 2.5;
                ctx.beginPath();
                ctx.moveTo(0, 0);
                ctx.quadraticCurveTo(-55, -65, -85, -20);
                ctx.quadraticCurveTo(-60, -10, -55, 15);
                ctx.quadraticCurveTo(-35, 5, -25, 25);
                ctx.closePath();
                ctx.fill();
                ctx.stroke();
                ctx.restore();

                // 2. Cola gruesa de serpiente enroscada en el suelo
                ctx.strokeStyle = '#1c0707';
                ctx.lineWidth = 18;
                ctx.lineCap = 'round';
                ctx.beginPath();
                ctx.moveTo(-35, -8);
                ctx.bezierCurveTo(-15, 8, 25, 6, 15, -18);
                ctx.bezierCurveTo(5, -35, -8, -48, 0, -68);
                ctx.stroke();

                // Escamas brillantes en la cola
                ctx.strokeStyle = '#dc2626';
                ctx.lineWidth = 3;
                ctx.stroke();

                // 3. Serpientes secundarias emergiendo de la espalda
                for (let i = 0; i < 2; i++) {
                    const sWave = Math.sin(this.animTime * 1.4 + i * 2) * 8;
                    ctx.strokeStyle = '#290808';
                    ctx.lineWidth = 8;
                    ctx.beginPath();
                    ctx.moveTo(5, -85 + i * 12);
                    ctx.quadraticCurveTo(35 + attackSwing * 25, -115 + sWave + i * 15, 52 + attackSwing * 35, -98 + sWave + i * 18);
                    ctx.stroke();

                    // Cabeza de serpiente con ojos amarillos
                    const hx = 52 + attackSwing * 35;
                    const hy = -98 + sWave + i * 18;
                    ctx.fillStyle = '#450a0a';
                    ctx.beginPath();
                    ctx.ellipse(hx, hy, 9, 5, 0.2, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.fillStyle = '#facc15';
                    ctx.fillRect(hx + 2, hy - 2, 3, 2);
                }

                // 4. Torso demoníaco acorazado
                ctx.fillStyle = '#140404';
                ctx.strokeStyle = '#ef4444';
                ctx.lineWidth = 1.8;
                ctx.beginPath();
                ctx.moveTo(-16, -65);
                ctx.lineTo(18, -65);
                ctx.lineTo(24, -105 + breathe);
                ctx.lineTo(-20, -105 + breathe);
                ctx.closePath();
                ctx.fill();
                ctx.stroke();

                // Grietas de lava en el pecho
                ctx.strokeStyle = '#f97316';
                ctx.lineWidth = 2;
                ctx.beginPath();
                ctx.moveTo(0, -98 + breathe);
                ctx.lineTo(6, -85 + breathe);
                ctx.lineTo(-4, -75 + breathe);
                ctx.stroke();

                // 5. Cabeza con cuernos curvos
                ctx.save();
                ctx.translate(4, -118 + breathe);
                ctx.fillStyle = '#1c0505';
                ctx.beginPath();
                ctx.arc(0, 0, 14, 0, Math.PI * 2);
                ctx.fill();
                // Cuernos
                ctx.fillStyle = '#b91c1c';
                ctx.beginPath();
                ctx.moveTo(-4, -10);
                ctx.quadraticCurveTo(-18, -32, 4, -34);
                ctx.quadraticCurveTo(-6, -22, 2, -10);
                ctx.fill();
                // Ojo brillante
                ctx.fillStyle = '#fef08a';
                ctx.fillRect(5, -3, 5, 3);
                ctx.restore();

                // 6. Brazo con garras infernales (animado al atacar)
                ctx.save();
                ctx.translate(12, -96 + breathe);
                ctx.rotate(-0.4 + attackSwing * 1.5);
                ctx.strokeStyle = '#dc2626';
                ctx.lineWidth = 9;
                ctx.beginPath();
                ctx.moveTo(0, 0);
                ctx.lineTo(36, 12);
                ctx.stroke();

                // Estela de corte rojo si ataca
                if (this.attackState) {
                    ctx.strokeStyle = 'rgba(239, 68, 68, 0.85)';
                    ctx.lineWidth = 6;
                    ctx.beginPath();
                    ctx.arc(10, 5, 58, -0.7, 0.8);
                    ctx.stroke();
                }
                ctx.restore();
            }

            // Arte procedural del Jugador 2: Jinete con espada roja montado en Cerbero de 3 cabezas
            drawJineteInfernal(ctx) {
                const gallop = Math.abs(Math.sin(this.animTime * 1.3)) * 5;
                const swordSwing = this.attackState ? Math.sin((1 - this.attackTimer / 0.3) * Math.PI) : 0;

                // 1. Cuerpo bestial de Cerbero (Perro infernal negro con brillo naranja)
                ctx.fillStyle = '#121215';
                ctx.strokeStyle = '#ea580c';
                ctx.lineWidth = 1.8;

                // Patas traseras y delanteras
                ctx.lineWidth = 9;
                ctx.strokeStyle = '#18181b';
                ctx.beginPath();
                ctx.moveTo(-28, -35 - gallop);
                ctx.lineTo(-36, -4);
                ctx.moveTo(24, -35 - gallop);
                ctx.lineTo(34 - gallop, -4);
                ctx.stroke();

                // Torso de Cerbero
                ctx.fillStyle = '#0f0f12';
                ctx.strokeStyle = '#c2410c';
                ctx.lineWidth = 2;
                ctx.beginPath();
                ctx.ellipse(0, -42 - gallop, 42, 22, -0.05, 0, Math.PI * 2);
                ctx.fill();
                ctx.stroke();

                // 2. Tres cabezas de Cerbero gruñendo hacia adelante
                const headOffsets = [
                    { dx: 36, dy: -62, scale: 0.9 },
                    { dx: 46, dy: -48, scale: 1.05 },
                    { dx: 34, dy: -34, scale: 0.9 }
                ];
                headOffsets.forEach((h, idx) => {
                    const bite = swordSwing * 10;
                    ctx.save();
                    ctx.translate(h.dx + bite, h.dy - gallop + Math.sin(this.animTime + idx) * 2);
                    ctx.scale(h.scale, h.scale);
                    ctx.fillStyle = '#18181b';
                    ctx.strokeStyle = '#f97316';
                    ctx.lineWidth = 1.5;
                    ctx.beginPath();
                    ctx.ellipse(0, 0, 14, 10, 0.1, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.stroke();
                    // Ojos de fuego
                    ctx.fillStyle = '#facc15';
                    ctx.fillRect(3, -4, 4, 2.5);
                    // Colmillos
                    ctx.fillStyle = '#fff';
                    ctx.fillRect(8, 3, 2, 4);
                    ctx.restore();
                });

                // 3. El Jinete Infernal montado sobre el lomo
                ctx.save();
                ctx.translate(-4, -64 - gallop);

                // Capa oscura ondeando
                ctx.fillStyle = '#450a0a';
                ctx.beginPath();
                ctx.moveTo(-4, -34);
                ctx.quadraticCurveTo(-38, -20 + Math.sin(this.animTime) * 6, -48, 4);
                ctx.lineTo(-10, -5);
                ctx.closePath();
                ctx.fill();

                // Armadura del Jinete
                ctx.fillStyle = '#1f1f23';
                ctx.strokeStyle = '#ef4444';
                ctx.lineWidth = 1.5;
                ctx.fillRect(-10, -36, 20, 28);
                ctx.strokeRect(-10, -36, 20, 28);

                // Cabeza del Jinete
                ctx.fillStyle = '#18181b';
                ctx.beginPath();
                ctx.arc(2, -46, 10, 0, Math.PI * 2);
                ctx.fill();
                ctx.fillStyle = '#ef4444';
                ctx.fillRect(5, -48, 4, 2.5);

                // 4. Gran Espada de Lava Incandescente
                ctx.save();
                ctx.translate(6, -28);
                ctx.rotate(-0.7 + swordSwing * 1.9);

                // Empuñadura
                ctx.fillStyle = '#78350f';
                ctx.fillRect(-3, -4, 6, 14);
                ctx.fillStyle = '#f59e0b';
                ctx.fillRect(-10, -6, 20, 4);

                // Hoja de la espada brillante
                const swordGrad = ctx.createLinearGradient(0, -6, 0, -78);
                swordGrad.addColorStop(0, '#fef08a');
                swordGrad.addColorStop(0.4, '#f97316');
                swordGrad.addColorStop(1, '#dc2626');
                ctx.fillStyle = swordGrad;
                ctx.shadowColor = '#ef4444';
                ctx.shadowBlur = 14;
                ctx.beginPath();
                ctx.moveTo(-5, -6);
                ctx.lineTo(5, -6);
                ctx.lineTo(0, -82);
                ctx.closePath();
                ctx.fill();
                ctx.shadowBlur = 0;

                // Arco de fuego al golpear
                if (this.attackState) {
                    ctx.strokeStyle = 'rgba(251, 146, 60, 0.9)';
                    ctx.lineWidth = 8;
                    ctx.beginPath();
                    ctx.arc(0, -10, 75, -1.2, 0.4);
                    ctx.stroke();
                }

                ctx.restore();
                ctx.restore();
            }
        }

        // Instancias de los 2 personajes
        const player1 = new Fighter({ id: 1, name: 'Diablo Serpiente', x: 220, facing: 1, type: 'serpent' });
        const player2 = new Fighter({ id: 2, name: 'El Jinete Infernal', x: 650, facing: -1, type: 'cerberus' });

        // Inicializar entorno volcánico (grietas de lava, castillos, brasas)
        function initEnvironment() {
            width = canvas.width = canvas.parentElement.clientWidth;
            height = canvas.height = canvas.parentElement.clientHeight;
            groundY = height * 0.83;

            // Generar siluetas de torres góticas en el fondo
            castleTowers = [
                { x: width * 0.12, w: 45, h: height * 0.36 },
                { x: width * 0.25, w: 35, h: height * 0.28 },
                { x: width * 0.50, w: 70, h: height * 0.44, main: true },
                { x: width * 0.74, w: 38, h: height * 0.30 },
                { x: width * 0.88, w: 50, h: height * 0.38 }
            ];

            // Generar red de grietas de lava en el suelo rocoso
            lavaCracks = [];
            const numCracks = Math.max(10, Math.floor(width / 85));
            for (let i = 0; i < numCracks; i++) {
                const startX = (width / numCracks) * i + Math.random() * 40;
                const points = [{ x: startX, y: groundY - 12 }];
                let currX = startX;
                let currY = groundY - 12;
                while (currY < height) {
                    currX += (Math.random() - 0.5) * 55;
                    currY += 18 + Math.random() * 22;
                    points.push({ x: currX, y: currY });
                }
                lavaCracks.push({
                    points,
                    phase: Math.random() * Math.PI * 2,
                    width: 2 + Math.random() * 3.5
                });
            }

            // Generar partículas de brasas flotando
            if (embers.length === 0) {
                for (let i = 0; i < 55; i++) {
                    embers.push({
                        x: Math.random() * width,
                        y: Math.random() * height,
                        r: 1 + Math.random() * 2.5,
                        vy: -0.4 - Math.random() * 1.2,
                        vx: (Math.random() - 0.5) * 0.8,
                        alpha: 0.3 + Math.random() * 0.7
                    });
                }
            }
        }

        function spawnParticles(x, y, color, count) {
            for (let i = 0; i < count; i++) {
                const angle = Math.random() * Math.PI * 2;
                const speed = 2 + Math.random() * 7;
                particles.push({
                    x,
                    y,
                    vx: Math.cos(angle) * speed,
                    vy: Math.sin(angle) * speed - 2,
                    size: 2 + Math.random() * 4,
                    color,
                    life: 0.35 + Math.random() * 0.35,
                    maxLife: 0.7
                });
            }
        }

        function spawnFloatingText(x, y, text, color) {
            floatingTexts.push({
                x,
                y,
                text,
                color,
                life: 0.85
            });
        }

        // Dibujar el fondo de El Infierno Eterno
        function drawBackground(time) {
            // Cielo rojo apocalíptico
            const skyGrad = ctx.createLinearGradient(0, 0, 0, groundY);
            skyGrad.addColorStop(0, '#140303');
            skyGrad.addColorStop(0.45, '#5c1108');
            skyGrad.addColorStop(0.85, '#b91c1c');
            skyGrad.addColorStop(1, '#ea580c');
            ctx.fillStyle = skyGrad;
            ctx.fillRect(0, 0, width, groundY);

            // Gran Luna Roja Carmesí al centro
            const moonX = width * 0.5;
            const moonY = height * 0.25;
            const moonR = Math.min(width, height) * 0.16;

            const moonGlow = ctx.createRadialGradient(moonX, moonY, moonR * 0.4, moonX, moonY, moonR * 2.1);
            moonGlow.addColorStop(0, 'rgba(254, 202, 202, 0.95)');
            moonGlow.addColorStop(0.35, 'rgba(239, 68, 68, 0.55)');
            moonGlow.addColorStop(1, 'rgba(153, 27, 27, 0)');
            ctx.fillStyle = moonGlow;
            ctx.beginPath();
            ctx.arc(moonX, moonY, moonR * 2.1, 0, Math.PI * 2);
            ctx.fill();

            ctx.fillStyle = '#fca5a5';
            ctx.beginPath();
            ctx.arc(moonX, moonY, moonR, 0, Math.PI * 2);
            ctx.fill();

            // Cráteres sutiles en la luna
            ctx.fillStyle = 'rgba(185, 28, 28, 0.28)';
            ctx.beginPath();
            ctx.arc(moonX - moonR * 0.3, moonY - moonR * 0.15, moonR * 0.25, 0, Math.PI * 2);
            ctx.arc(moonX + moonR * 0.25, moonY + moonR * 0.2, moonR * 0.18, 0, Math.PI * 2);
            ctx.fill();

            // Montañas volcánicas oscuras
            ctx.fillStyle = '#1f0705';
            ctx.beginPath();
            ctx.moveTo(0, groundY);
            ctx.lineTo(0, groundY - height * 0.22);
            ctx.lineTo(width * 0.22, groundY - height * 0.34);
            ctx.lineTo(width * 0.48, groundY - height * 0.18);
            ctx.lineTo(width * 0.78, groundY - height * 0.36);
            ctx.lineTo(width, groundY - height * 0.20);
            ctx.lineTo(width, groundY);
            ctx.closePath();
            ctx.fill();

            // Siluetas de Torres del Castillo Infernal
            ctx.fillStyle = '#120303';
            castleTowers.forEach(t => {
                const topY = groundY - t.h;
                ctx.fillRect(t.x - t.w / 2, topY, t.w, t.h);
                // Aguja gótica
                ctx.beginPath();
                ctx.moveTo(t.x - t.w * 0.6, topY);
                ctx.lineTo(t.x, topY - t.w * 0.95);
                ctx.lineTo(t.x + t.w * 0.6, topY);
                ctx.closePath();
                ctx.fill();

                // Ventanas con luz de fuego
                ctx.fillStyle = '#f97316';
                ctx.fillRect(t.x - 3, topY + 20, 6, 12);
                if (t.main) {
                    ctx.fillRect(t.x - 14, topY + 45, 5, 10);
                    ctx.fillRect(t.x + 9, topY + 45, 5, 10);
                }
                ctx.fillStyle = '#120303';
            });

            // Río de lava lejano en el horizonte
            const horizonGlow = ctx.createLinearGradient(0, groundY - 25, 0, groundY + 5);
            horizonGlow.addColorStop(0, 'rgba(249, 115, 22, 0)');
            horizonGlow.addColorStop(0.7, 'rgba(249, 115, 22, 0.85)');
            horizonGlow.addColorStop(1, '#fef08a');
            ctx.fillStyle = horizonGlow;
            ctx.fillRect(0, groundY - 25, width, 30);

            // Plataforma de Roca Volcánica Oscura
            const floorGrad = ctx.createLinearGradient(0, groundY - 12, 0, height);
            floorGrad.addColorStop(0, '#1c1917');
            floorGrad.addColorStop(0.25, '#0c0a09');
            floorGrad.addColorStop(1, '#050202');
            ctx.fillStyle = floorGrad;
            ctx.fillRect(0, groundY - 12, width, height - groundY + 12);

            // Grietas brillantes de lava en el suelo
            lavaCracks.forEach(crack => {
                const pulse = 0.7 + Math.sin(time * 3 + crack.phase) * 0.3;
                ctx.save();
                ctx.strokeStyle = `rgba(234, 88, 12, ${0.45 * pulse})`;
                ctx.lineWidth = crack.width * 2.8;
                ctx.lineCap = 'round';
                ctx.beginPath();
                crack.points.forEach((pt, idx) => {
                    if (idx === 0) ctx.moveTo(pt.x, pt.y);
                    else ctx.lineTo(pt.x, pt.y);
                });
                ctx.stroke();

                // Núcleo incandescente amarillo-naranja de la grieta
                ctx.strokeStyle = `rgba(254, 240, 138, ${0.9 * pulse})`;
                ctx.lineWidth = crack.width * 0.9;
                ctx.stroke();
                ctx.restore();
            });
        }

        // Actualizar y dibujar proyectiles de fuego/veneno
        function updateAndDrawProjectiles(dt) {
            for (let i = projectiles.length - 1; i >= 0; i--) {
                const p = projectiles[i];
                p.x += p.vx;
                p.life -= dt;

                // Estela de fuego
                if (Math.random() < 0.7) {
                    spawnParticles(p.x, p.y, p.type === 'serpent' ? '#ef4444' : '#f97316', 1);
                }

                // Dibujar proyectil (Cabeza de fuego / Esfera demoníaca)
                ctx.save();
                ctx.translate(p.x, p.y);
                const grad = ctx.createRadialGradient(0, 0, 2, 0, 0, p.radius * 1.5);
                grad.addColorStop(0, '#fef08a');
                grad.addColorStop(0.5, p.type === 'serpent' ? '#dc2626' : '#f97316');
                grad.addColorStop(1, 'rgba(0,0,0,0)');
                ctx.fillStyle = grad;
                ctx.beginPath();
                ctx.arc(0, 0, p.radius * 1.5, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();

                // Comprobar colisión con el rival
                const target = p.owner === 1 ? player2 : player1;
                if (Math.abs(p.x - target.x) < 48 && Math.abs(p.y - (target.y - 65)) < 65) {
                    target.takeDamage(p.damage, Math.sign(p.vx), false);
                    projectiles.splice(i, 1);
                    continue;
                }

                if (p.life <= 0 || p.x < 0 || p.x > width) {
                    projectiles.splice(i, 1);
                }
            }
        }

        // Inteligencia Artificial básica para modo 1 Jugador
        let aiTimer = 0;
        let aiDecision = 'idle';
        function updateAI(dt) {
            if (gameMode !== '1P' || gameState !== 'PLAYING') return;

            aiTimer -= dt;
            const dist = player1.x - player2.x;
            const absDist = Math.abs(dist);

            if (aiTimer <= 0) {
                aiTimer = 0.25 + Math.random() * 0.35;
                const rand = Math.random();

                if (absDist > 260) {
                    aiDecision = rand < 0.35 ? 'projectile' : 'approach';
                } else if (absDist > 115) {
                    if (rand < 0.25 && player2.cdSpecial <= 0) aiDecision = 'special';
                    else if (rand < 0.45) aiDecision = 'projectile';
                    else aiDecision = 'approach';
                } else {
                    if (rand < 0.45) aiDecision = 'melee';
                    else if (rand < 0.65) aiDecision = 'block';
                    else if (rand < 0.8) aiDecision = 'jump';
                    else aiDecision = 'retreat';
                }
            }

            player2.isBlocking = (aiDecision === 'block');

            if (aiDecision === 'approach') {
                player2.vx = Math.sign(dist) * 4.8;
            } else if (aiDecision === 'retreat') {
                player2.vx = -Math.sign(dist) * 4.5;
            } else if (aiDecision === 'melee') {
                player2.meleeAttack();
                aiDecision = 'idle';
            } else if (aiDecision === 'projectile') {
                player2.fireProjectile();
                aiDecision = 'approach';
            } else if (aiDecision === 'special') {
                player2.specialAttack();
                aiDecision = 'idle';
            } else if (aiDecision === 'jump') {
                player2.jump();
                aiDecision = 'approach';
            }
        }

        // Procesar entradas de teclado y táctiles
        function handleInputs() {
            if (gameState !== 'PLAYING') return;

            // --- JUGADOR 1 ---
            const moveLeft1 = keys['KeyA'] || touchState.left || (gameMode === '1P' && keys['ArrowLeft']);
            const moveRight1 = keys['KeyD'] || touchState.right || (gameMode === '1P' && keys['ArrowRight']);
            player1.isBlocking = !!(keys['KeyS'] || touchState.block || (gameMode === '1P' && keys['ArrowDown']));

            if (!player1.isBlocking && !player1.attackState) {
                if (moveLeft1) player1.vx = -5.5;
                if (moveRight1) player1.vx = 5.5;
            }
            if (keys['KeyW'] || (gameMode === '1P' && keys['ArrowUp'])) {
                player1.jump();
            }

            // --- JUGADOR 2 (Solo en modo 2P) ---
            if (gameMode === '2P') {
                player2.isBlocking = !!keys['ArrowDown'];
                if (!player2.isBlocking && !player2.attackState) {
                    if (keys['ArrowLeft']) player2.vx = -5.5;
                    if (keys['ArrowRight']) player2.vx = 5.5;
                }
                if (keys['ArrowUp']) player2.jump();
            }
        }

        // Actualizar barras de vida y enfriamientos en el HUD HTML
        function updateHUD() {
            document.getElementById('p1-hp-text').textContent = `${player1.hp} / ${player1.maxHp}`;
            document.getElementById('p1-hp-bar').style.width = `${(player1.hp / player1.maxHp) * 100}%`;
            document.getElementById('p1-energy-bar').style.width = `${(player1.energy / player1.maxEnergy) * 100}%`;

            document.getElementById('p2-hp-text').textContent = `${player2.hp} / ${player2.maxHp}`;
            document.getElementById('p2-hp-bar').style.width = `${(player2.hp / player2.maxHp) * 100}%`;
            document.getElementById('p2-energy-bar').style.width = `${(player2.energy / player2.maxEnergy) * 100}%`;

            // Cooldown visual en botones E y R
            const cdE = document.getElementById('cd-e-overlay');
            if (player1.cdProjectile > 0) {
                cdE.classList.remove('hidden');
                cdE.classList.add('flex');
                cdE.textContent = player1.cdProjectile.toFixed(1);
            } else {
                cdE.classList.add('hidden');
                cdE.classList.remove('flex');
            }

            const cdR = document.getElementById('cd-r-overlay');
            if (player1.cdSpecial > 0 || player1.energy < 40) {
                cdR.classList.remove('hidden');
                cdR.classList.add('flex');
                cdR.textContent = player1.cdSpecial > 0 ? player1.cdSpecial.toFixed(1) : '⚡40';
            } else {
                cdR.classList.add('hidden');
                cdR.classList.remove('flex');
            }
        }

        function showAnnouncement(text) {
            const el = document.getElementById('announcer-text');
            el.textContent = text;
            el.classList.remove('opacity-0', 'scale-75');
            el.classList.add('opacity-100', 'scale-100');
            setTimeout(() => {
                el.classList.remove('opacity-100', 'scale-100');
                el.classList.add('opacity-0', 'scale-75');
            }, 1200);
        }

        function endGame(winnerText, subtitleText) {
            gameState = 'GAMEOVER';
            showAnnouncement('¡K.O.!');
            setTimeout(() => {
                document.getElementById('winner-title').textContent = winnerText;
                document.getElementById('winner-subtitle').textContent = subtitleText;
                document.getElementById('gameover-modal').classList.remove('hidden');
            }, 1000);
        }

        // Bucle principal de animación
        function gameLoop(timestamp) {
            const dt = Math.min(0.05, (timestamp - lastTime) / 1000 || 0.016);
            lastTime = timestamp;

            ctx.save();
            // Aplicar vibración de cámara en golpes pesados
            if (screenShake > 0.2) {
                const sx = (Math.random() - 0.5) * screenShake;
                const sy = (Math.random() - 0.5) * screenShake;
                ctx.translate(sx, sy);
                screenShake *= 0.85;
            }

            drawBackground(timestamp / 1000);

            // Actualizar brasas flotantes
            embers.forEach(e => {
                e.x += e.vx;
                e.y += e.vy;
                if (e.y < 0) {
                    e.y = height;
                    e.x = Math.random() * width;
                }
                ctx.fillStyle = `rgba(251, 146, 60, ${e.alpha})`;
                ctx.beginPath();
                ctx.arc(e.x, e.y, e.r, 0, Math.PI * 2);
                ctx.fill();
            });

            if (gameState === 'PLAYING') {
                // Temporizador de combate
                timerAccumulator += dt;
                if (timerAccumulator >= 1) {
                    timerAccumulator -= 1;
                    matchTime = Math.max(0, matchTime - 1);
                    const mins = String(Math.floor(matchTime / 60)).padStart(2, '0');
                    const secs = String(matchTime % 60).padStart(2, '0');
                    document.getElementById('timer-display').textContent = `${mins}:${secs}`;

                    if (matchTime === 0) {
                        if (player1.hp >= player2.hp) {
                            endGame('¡DIABLO SERPIENTE GANA!', 'Tiempo agotado: mayor vitalidad infernal.');
                        } else {
                            endGame('¡EL JINETE INFERNAL GANA!', 'Tiempo agotado: Cerbero reclama la arena.');
                        }
                    }
                }

                handleInputs();
                updateAI(dt);
                player1.update(dt, player2);
                player2.update(dt, player1);
                updateHUD();

                // Verificar derrota
                if (player1.hp <= 0) {
                    endGame('¡EL JINETE INFERNAL GANA!', 'La espada de lava y Cerbero han triunfado.');
                } else if (player2.hp <= 0) {
                    endGame('¡DIABLO SERPIENTE GANA!', 'El señor de las serpientes reina en el infierno.');
                }
            }

            // Dibujar combatientes
            player1.draw(ctx);
            player2.draw(ctx);

            // Proyectiles y partículas
            updateAndDrawProjectiles(dt);

            for (let i = particles.length - 1; i >= 0; i--) {
                const pt = particles[i];
                pt.x += pt.vx;
                pt.y += pt.vy;
                pt.life -= dt;
                ctx.fillStyle = pt.color;
                ctx.globalAlpha = Math.max(0, pt.life / pt.maxLife);
                ctx.beginPath();
                ctx.arc(pt.x, pt.y, pt.size, 0, Math.PI * 2);
                ctx.fill();
                ctx.globalAlpha = 1;
                if (pt.life <= 0) particles.splice(i, 1);
            }

            // Textos flotantes de daño
            for (let i = floatingTexts.length - 1; i >= 0; i--) {
                const ft = floatingTexts[i];
                ft.y -= 45 * dt;
                ft.life -= dt;
                ctx.font = 'bold 20px Cinzel, serif';
                ctx.textAlign = 'center';
                ctx.fillStyle = ft.color;
                ctx.strokeStyle = '#000';
                ctx.lineWidth = 3;
                ctx.strokeText(ft.text, ft.x, ft.y);
                ctx.fillText(ft.text, ft.x, ft.y);
                if (ft.life <= 0) floatingTexts.splice(i, 1);
            }

            ctx.restore();
            requestAnimationFrame(gameLoop);
        }

        // Funciones públicas para botones UI
        function startGame(mode) {
            gameMode = mode;
            document.getElementById('menu-modal').classList.add('hidden');
            document.getElementById('gameover-modal').classList.add('hidden');
            initEnvironment();
            player1.reset(width * 0.25, 1);
            player2.reset(width * 0.75, -1);
            projectiles = [];
            particles = [];
            matchTime = 167;
            document.getElementById('timer-display').textContent = '02:47';
            updateHUD();
            gameState = 'PLAYING';
            showAnnouncement('¡A PELEAR!');
        }

        function restartSameMode() {
            startGame(gameMode);
        }

        function showMenu() {
            gameState = 'MENU';
            document.getElementById('gameover-modal').classList.add('hidden');
            document.getElementById('menu-modal').classList.remove('hidden');
        }

        function triggerPlayerAction(action) {
            if (gameState !== 'PLAYING') return;
            if (action === 'attack') player1.meleeAttack();
            if (action === 'projectile') player1.fireProjectile();
            if (action === 'special') player1.specialAttack();
            if (action === 'jump') player1.jump();
        }

        // Eventos de teclado
        window.addEventListener('keydown', (e) => {
            keys[e.code] = true;
            if (gameState !== 'PLAYING') return;

            // Habilidades J1
            if (e.code === 'KeyQ' || e.code === 'Space') player1.meleeAttack();
            if (e.code === 'KeyE') player1.fireProjectile();
            if (e.code === 'KeyR') player1.specialAttack();

            // Habilidades J2 (en modo 2P)
            if (gameMode === '2P') {
                if (e.code === 'Digit1' || e.code === 'Numpad1') player2.meleeAttack();
                if (e.code === 'Digit2' || e.code === 'Numpad2') player2.fireProjectile();
                if (e.code === 'Digit3' || e.code === 'Numpad3') player2.specialAttack();
            }
        });

        window.addEventListener('keyup', (e) => {
            keys[e.code] = false;
        });

        // Configurar controles táctiles continuos (mantener presionado)
        function bindHoldButton(id, prop) {
            const btn = document.getElementById(id);
            const start = (e) => { e.preventDefault(); touchState[prop] = true; };
            const end = (e) => { e.preventDefault(); touchState[prop] = false; };
            btn.addEventListener('mousedown', start);
            btn.addEventListener('mouseup', end);
            btn.addEventListener('mouseleave', end);
            btn.addEventListener('touchstart', start, { passive: false });
            btn.addEventListener('touchend', end, { passive: false });
        }

        bindHoldButton('btn-left', 'left');
        bindHoldButton('btn-right', 'right');
        bindHoldButton('btn-block', 'block');

        window.addEventListener('resize', () => {
            const prevGround = groundY;
            initEnvironment();
            if (prevGround > 0) {
                player1.y = groundY;
                player2.y = groundY;
            }
        });

        // Arranque inicial
        window.onload = function () {
            initEnvironment();
            player1.reset(width * 0.25, 1);
            player2.reset(width * 0.75, -1);
            requestAnimationFrame(gameLoop);
        };
    </script>
</body>
</html>
