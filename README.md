<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Sombra Eterna: GD Cyber Madness</title>
    <style>
        * {
            box-sizing: border-box;
            user-select: none;
            -webkit-user-select: none;
            margin: 0;
            padding: 0;
            touch-action: none;
        }

        body {
            background-color: #020005;
            color: #e2e8f0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            overflow: hidden;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            touch-action: none;
            width: 100vw;
            height: 100vh;
        }

        .overlay {
            position: fixed;
            top: 0; left: 0;
            width: 100vw; height: 100vh;
            background: rgba(2, 0, 8, 0.96);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 100;
            padding: 20px;
            text-align: center;
        }

        .modal-card {
            background: rgba(10, 2, 20, 0.95);
            border: 2px solid #00f0ff;
            box-shadow: 0 0 50px #00f0ff, inset 0 0 25px #ff0055;
            border-radius: 16px;
            padding: 25px;
            max-width: 650px;
            width: 90%;
            max-height: 90vh;
            overflow-y: auto;
            backdrop-filter: blur(10px);
        }

        .modal-title {
            font-size: 26px;
            color: #00f0ff;
            text-transform: uppercase;
            letter-spacing: 3px;
            margin-bottom: 15px;
            text-shadow: 0 0 15px #00f0ff, 0 0 30px #ff0055;
        }

        .modal-story {
            font-size: 13px;
            line-height: 1.6;
            color: #cbd5e1;
            margin-bottom: 20px;
            text-align: justify;
            border-left: 4px solid #ff0055;
            padding-left: 14px;
            background: rgba(255, 0, 85, 0.08);
        }

        .btn-mode {
            background: linear-gradient(135deg, #ff0055 0%, #6a00ff 100%);
            border: 2px solid #00f0ff;
            color: #fff;
            padding: 14px 24px;
            font-size: 15px;
            font-weight: 800;
            border-radius: 30px;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 0 20px rgba(0, 240, 255, 0.4);
            margin: 6px;
            touch-action: manipulation;
        }

        .btn-mode:hover {
            transform: scale(1.05);
            box-shadow: 0 0 35px #00f0ff, 0 0 15px #ff0055;
        }

        #game-wrapper {
            position: relative;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            width: 100vw;
            height: 100vh;
            overflow: hidden;
            touch-action: none;
        }

        #game-container {
            position: relative;
            width: 900px;
            height: 450px;
            border: 3px solid #00f0ff;
            box-shadow: 0 0 50px rgba(0, 240, 255, 0.4), inset 0 0 30px rgba(255, 0, 85, 0.4);
            overflow: hidden;
            background: #010003;
            border-radius: 8px;
            touch-action: none;
        }

        body.mobile-mode #game-container {
            width: 100vw;
            height: 100vh;
            max-height: 100vh;
            border: none;
            border-radius: 0;
            box-shadow: none;
        }

        canvas {
            display: block;
            width: 100%;
            height: 100%;
            touch-action: none;
        }

        .hud {
            position: absolute;
            top: 12px; left: 15px; right: 15px;
            display: flex;
            justify-content: space-between;
            color: #fff;
            font-size: 13px;
            font-weight: 900;
            letter-spacing: 1.2px;
            text-shadow: 0 0 10px #00f0ff, 0 0 20px #ff0055;
            z-index: 10;
            pointer-events: none;
        }

        .power-hud {
            color: #ffe600;
            text-shadow: 0 0 10px #ffe600, 0 0 20px #ff0055;
            display: none;
        }

        .controls {
            display: none;
            width: 100vw;
            height: 15vh;
            background: #020005;
            border-top: 3px solid #ff0055;
            padding: 8px;
            justify-content: center;
            align-items: center;
            touch-action: none;
        }

        body.mobile-mode .controls {
            display: flex;
        }

        .btn-jump-mobile {
            background: linear-gradient(135deg, #00f0ff, #ff0055);
            color: #fff;
            border: 2px solid #fff;
            border-radius: 20px;
            width: 95%; height: 85%;
            font-size: 20px;
            font-weight: 900;
            box-shadow: 0 0 25px #ff0055;
            touch-action: manipulation;
        }
    </style>
</head>
<body>

    <div id="modal-start" class="overlay">
        <div class="modal-card">
            <h1 class="modal-title">⚡ CYBER PSYCHO GD ⚡</h1>
            <div class="modal-story">
                <strong>¡NUEVAS MECÁNICAS Y EFECTOS PSICODÉLICOS!</strong><br>
                • <strong>Doble Salto:</strong> Tienes 2 saltos en el aire para volar sin límites.<br>
                • <strong>Sin techo inicial:</strong> ¡Sal a volar con los Boosters sin chocar arriba!<br>
                • <strong>Pisotón:</strong> Destruye a los monstruos saltando sobre ellos.<br>
                • <strong>Invencibilidad:</strong> Junta 3 monedas para destruir todo lo que toques por 30s.<br>
                • <strong>Efectos de Luces y Nebulosas:</strong> Luces, destellos y explosiones psicodélicas.
            </div>
            <div>
                <button class="btn-mode" onclick="startGame('laptop')">💻 TECLADO (ESPACIO / W)</button>
                <button class="btn-mode" onclick="startGame('mobile')">📱 TÁCTIL / CELULAR</button>
            </div>
        </div>
    </div>

    <div id="game-wrapper">
        <div id="game-container">
            <div class="hud">
                <div>DISTANCIA: <span id="dist-display">0m</span></div>
                <div>MONEDAS: <span id="coins-display">0/3</span></div>
                <div id="power-timer" class="power-hud">PODER: <span id="power-sec">30</span>s</div>
                <div>CHECKPOINT: <span id="cp-display">0</span></div>
            </div>
            <canvas id="gameCanvas" width="900" height="450"></canvas>
        </div>

        <div class="controls">
            <button class="btn-jump-mobile" id="btn-jump">¡TOCA CUALQUIER LUGAR PARA SALTAR! ▲▲</button>
        </div>
    </div>

<script>
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

let gameDistance = 0;
let checkpointIndex = 0;
let lastCheckpointX = 50;
let speedMultiplier = 1.0;
let cameraX = 0;
let screenShake = 0;
let isReversed = false; 
let gravityDir = 1;     

let jumpCount = 0;
const maxJumps = 2;

let levelCoinsCollected = 0;
let isInvincible = false;
let invincibilityTimeLeft = 0;
let invincibilityTimerInterval = null;

let strobeColor = '#00f0ff';
let strobeTimer = 0;

let audioCtx = null;
let musicInterval = null;

function initAudio() {
    if (!audioCtx) {
        audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    }
    if (audioCtx.state === 'suspended') audioCtx.resume();
    startAudioTheme();
}

function speakNeymar() {
    if ('speechSynthesis' in window) {
        window.speechSynthesis.cancel();
        const utterance = new SpeechSynthesisUtterance("¡Neymar!");
        utterance.lang = 'es-ES';
        utterance.pitch = 1.5;
        utterance.rate = 1.2;
        window.speechSynthesis.speak(utterance);
    }
}

function startAudioTheme() {
    if (musicInterval) clearInterval(musicInterval);
    const baseTempo = Math.max(100, 280 - checkpointIndex * 15);
    const darkScale = [55, 65, 69, 82, 87, 98, 110, 123, 130];

    musicInterval = setInterval(() => {
        if (!audioCtx) return;
        const note = darkScale[Math.floor(Math.random() * darkScale.length)];
        
        try {
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            const filter = audioCtx.createBiquadFilter();

            osc.type = isInvincible ? 'square' : 'sawtooth';
            osc.frequency.setValueAtTime(note * (isInvincible ? 1.5 : 1), audioCtx.currentTime);

            filter.type = 'lowpass';
            filter.frequency.setValueAtTime(350 + checkpointIndex * 120, audioCtx.currentTime);

            gain.gain.setValueAtTime(0.08, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + 0.3);

            osc.connect(filter);
            filter.connect(gain);
            gain.connect(audioCtx.destination);

            osc.start();
            osc.stop(audioCtx.currentTime + 0.3);
        } catch(e){}
    }, baseTempo);
}

function playSound(type) {
    if (!audioCtx) return;
    try {
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.connect(gain);
        gain.connect(audioCtx.destination);

        if (type === 'jump') {
            osc.type = 'sine';
            osc.frequency.setValueAtTime(200, audioCtx.currentTime);
            osc.frequency.exponentialRampToValueAtTime(600, audioCtx.currentTime + 0.12);
            gain.gain.setValueAtTime(0.08, audioCtx.currentTime);
            gain.gain.linearRampToValueAtTime(0.01, audioCtx.currentTime + 0.12);
            osc.start(); osc.stop(audioCtx.currentTime + 0.12);
        } else if (type === 'coin') {
            osc.type = 'triangle';
            osc.frequency.setValueAtTime(587.33, audioCtx.currentTime);
            osc.frequency.exponentialRampToValueAtTime(880, audioCtx.currentTime + 0.2);
            gain.gain.setValueAtTime(0.12, audioCtx.currentTime);
            gain.gain.linearRampToValueAtTime(0.01, audioCtx.currentTime + 0.2);
            osc.start(); osc.stop(audioCtx.currentTime + 0.2);
        } else if (type === 'boost') {
            osc.type = 'sawtooth';
            osc.frequency.setValueAtTime(200, audioCtx.currentTime);
            osc.frequency.linearRampToValueAtTime(1200, audioCtx.currentTime + 0.4);
            gain.gain.setValueAtTime(0.15, audioCtx.currentTime);
            gain.gain.linearRampToValueAtTime(0.01, audioCtx.currentTime + 0.4);
            osc.start(); osc.stop(audioCtx.currentTime + 0.4);
        } else if (type === 'destroy') {
            osc.type = 'square';
            osc.frequency.setValueAtTime(350, audioCtx.currentTime);
            osc.frequency.linearRampToValueAtTime(80, audioCtx.currentTime + 0.18);
            gain.gain.setValueAtTime(0.18, audioCtx.currentTime);
            gain.gain.linearRampToValueAtTime(0.01, audioCtx.currentTime + 0.18);
            osc.start(); osc.stop(audioCtx.currentTime + 0.18);
        } else if (type === 'death') {
            osc.type = 'sawtooth';
            osc.frequency.setValueAtTime(120, audioCtx.currentTime);
            osc.frequency.linearRampToValueAtTime(20, audioCtx.currentTime + 0.3);
            gain.gain.setValueAtTime(0.15, audioCtx.currentTime);
            gain.gain.linearRampToValueAtTime(0.01, audioCtx.currentTime + 0.3);
            osc.start(); osc.stop(audioCtx.currentTime + 0.3);
        }
    } catch(e){}
}

const player = {
    x: 50,
    y: 200,
    width: 32,
    height: 32,
    baseSpeed: 5.0,
    vy: 0,
    jumpForce: 11.5,
    gravity: 0.58,
    grounded: false,
    rotation: 0,
    trail: []
};

let platforms = [];
let spikes = [];
let enemies = [];
let portals = [];
let tunnels = [];
let coins = [];
let checkpoints = [];
let particles = [];
let nebulae = [];

for (let i = 0; i < 6; i++) {
    nebulae.push({
        x: Math.random() * 3000,
        y: Math.random() * 400,
        radius: Math.random() * 120 + 80,
        color: ['rgba(0,240,255,0.08)', 'rgba(255,0,85,0.08)', 'rgba(106,0,255,0.08)'][i % 3],
        vx: (Math.random() - 0.5) * 0.8
    });
}

function activateInvincibility() {
    isInvincible = true;
    invincibilityTimeLeft = 30;
    document.getElementById('power-timer').style.display = 'block';
    document.getElementById('power-sec').innerText = invincibilityTimeLeft;

    if (invincibilityTimerInterval) clearInterval(invincibilityTimerInterval);

    invincibilityTimerInterval = setInterval(() => {
        invincibilityTimeLeft--;
        document.getElementById('power-sec').innerText = invincibilityTimeLeft;

        if (invincibilityTimeLeft <= 0) {
            clearInterval(invincibilityTimerInterval);
            isInvincible = false;
            document.getElementById('power-timer').style.display = 'none';
        }
    }, 1000);
}

function generateWorldChunk(startX) {
    let currentX = startX;
    const diffFactor = Math.min(checkpointIndex, 10);

    if (startX < 300) {
        platforms.push({ x: 0, y: 350, width: 900, height: 100 });
        currentX = 900;
    }

    let chunkCoinsCount = 0;

    for (let i = 0; i < 10; i++) {
        let platformWidth = 550 + Math.random() * 300 - diffFactor * 10;
        let groundY = 360 - Math.random() * 40;

        platforms.push({ x: currentX, y: groundY, width: platformWidth, height: 120 });

        if (checkpointIndex >= 3) {
            platforms.push({ x: currentX, y: 0, width: platformWidth, height: 35 });
        }

        if (chunkCoinsCount < 3 && Math.random() < 0.5) {
            let coinX = currentX + 160 + chunkCoinsCount * 110;
            let coinY = groundY - 60 - (diffFactor * 8);
            coins.push({ x: coinX, y: coinY, radius: 12, collected: false });
            chunkCoinsCount++;
        }

        let spikeSpacing = Math.max(160, 260 - diffFactor * 10);
        for (let sj = 180; sj < platformWidth - 140; sj += spikeSpacing) {
            if (Math.random() < 0.25 + diffFactor * 0.03) {
                spikes.push({ x: currentX + sj, y: groundY - 28, width: 28, height: 28, dir: 1 });
            }
        }

        if (Math.random() < 0.3 + diffFactor * 0.04) {
            enemies.push({
                x: currentX + platformWidth / 2,
                y: groundY - 80,
                width: 32, height: 32,
                baseY: groundY - 80,
                angle: Math.random() * Math.PI,
                speed: 0.04 + diffFactor * 0.005
            });
        }

        if (i === 3 || i === 7) {
            tunnels.push({ x: currentX + 280, y: groundY - 80, width: 60, height: 80 });
        }

        if (i === 9) {
            checkpoints.push({ x: currentX + platformWidth - 70, y: groundY - 95, width: 36, height: 95, passed: false });
        }

        let gapWidth = Math.min(100 + diffFactor * 5, 140);
        currentX += platformWidth + gapWidth;
    }
}

function triggerJump() {
    if (player.grounded || jumpCount < maxJumps) {
        player.vy = -player.jumpForce * gravityDir;
        player.grounded = false;
        jumpCount++;
        playSound('jump');
        spawnParticles(player.x + player.width/2, player.y + (gravityDir === 1 ? player.height : 0), 12, jumpCount === 2 ? '#ff0055' : '#00f0ff');
    }
}

function respawnAtCheckpoint() {
    playSound('death');
    screenShake = 22;
    spawnParticles(player.x, player.y, 40, '#ff0055');

    player.x = lastCheckpointX;
    player.y = 180;
    player.vy = 0;
    jumpCount = 0;
    gravityDir = 1;
    isReversed = false;
}

function spawnParticles(x, y, count = 12, color = '#00f0ff') {
    for (let i = 0; i < count; i++) {
        particles.push({
            x: x, y: y,
            vx: (Math.random() - 0.5) * 9,
            vy: (Math.random() - 0.5) * 9,
            size: Math.random() * 6 + 2,
            color: color,
            life: 30
        });
    }
}

function update() {
    if (screenShake > 0) screenShake--;

    // Aceleración continua y progresiva con cada frame recorrido
    speedMultiplier += 0.0002;

    strobeTimer++;
    if (strobeTimer % 60 === 0) {
        strobeColor = ['#00f0ff', '#ff0055', '#6a00ff', '#ffe600'][Math.floor(Math.random() * 4)];
    }

    const actualSpeed = player.baseSpeed * speedMultiplier * (isReversed ? -1 : 1);
    player.x += actualSpeed;

    player.vy += player.gravity * gravityDir;
    player.y += player.vy;

    if (!player.grounded) {
        player.rotation += 0.16 * gravityDir;
    } else {
        player.rotation = 0;
    }

    player.trail.push({ x: player.x, y: player.y, rotation: player.rotation });
    if (player.trail.length > 8) player.trail.shift();

    // Colisión Plataformas
    player.grounded = false;
    for (let p of platforms) {
        if (player.x < p.x + p.width &&
            player.x + player.width > p.x &&
            player.y < p.y + p.height &&
            player.y + player.height > p.y) {
            
            if (gravityDir === 1 && player.vy > 0 && player.y + player.height - player.vy <= p.y + 14) {
                player.y = p.y - player.height;
                player.vy = 0;
                player.grounded = true;
                jumpCount = 0;
            } else if (gravityDir === -1 && player.vy < 0 && player.y - player.vy >= p.y + p.height - 14) {
                player.y = p.y + p.height;
                player.vy = 0;
                player.grounded = true;
                jumpCount = 0;
            } else {
                if (!isInvincible) {
                    respawnAtCheckpoint();
                    return;
                }
            }
        }
    }

    // Colisión Picos
    for (let i = spikes.length - 1; i >= 0; i--) {
        let s = spikes[i];
        if (player.x < s.x + s.width - 5 &&
            player.x + player.width > s.x + 5 &&
            player.y < s.y + s.height &&
            player.y + player.height > s.y) {
            
            if (isInvincible) {
                playSound('destroy');
                spawnParticles(s.x, s.y, 25, '#ffe600');
                spikes.splice(i, 1);
            } else {
                respawnAtCheckpoint();
                return;
            }
        }
    }

    // Monstruos: Pisotón o Destrucción
    for (let i = enemies.length - 1; i >= 0; i--) {
        let e = enemies[i];
        if (player.x < e.x + e.width &&
            player.x + player.width > e.x &&
            player.y < e.y + e.height &&
            player.y + player.height > e.y) {
            
            if (isInvincible) {
                playSound('destroy');
                spawnParticles(e.x, e.y, 30, '#ffe600');
                enemies.splice(i, 1);
            } 
            else if (player.vy > 0 && player.y + player.height - player.vy <= e.y + 14) {
                playSound('destroy');
                spawnParticles(e.x, e.y, 25, '#00f0ff');
                player.vy = -player.jumpForce * 0.85;
                jumpCount = 1; 
                enemies.splice(i, 1);
            } 
            else {
                respawnAtCheckpoint();
                return;
            }
        }
    }

    // Recolección Monedas
    for (let c of coins) {
        if (!c.collected && Math.hypot((player.x + player.width/2) - c.x, (player.y + player.height/2) - c.y) < c.radius + 16) {
            c.collected = true;
            levelCoinsCollected++;
            playSound('coin');
            spawnParticles(c.x, c.y, 20, '#ffe600');

            document.getElementById('coins-display').innerText = levelCoinsCollected + '/3';

            if (levelCoinsCollected >= 3) {
                activateInvincibility();
            }
        }
    }

    // Túneles Booster
    for (let t of tunnels) {
        if (player.x < t.x + t.width &&
            player.x + player.width > t.x &&
            player.y < t.y + t.height &&
            player.y + player.height > t.y) {
            
            playSound('boost');
            screenShake = 18;
            player.vy = -16 * gravityDir; 
            player.x += 140; 
            jumpCount = 1;
            spawnParticles(player.x, player.y, 35, '#ff0055');
        }
    }

    // Checkpoints
    for (let cp of checkpoints) {
        if (!cp.passed && player.x > cp.x) {
            cp.passed = true;
            checkpointIndex++;
            lastCheckpointX = cp.x;
            speedMultiplier += 0.08;
            levelCoinsCollected = 0;
            document.getElementById('coins-display').innerText = '0/3';

            speakNeymar();
            startAudioTheme();
            spawnParticles(cp.x, cp.y + 45, 50, '#00f0ff');

            document.getElementById('cp-display').innerText = checkpointIndex;
        }
    }

    // Generar más terreno
    if (player.x > platforms[platforms.length - 1].x - 900) {
        generateWorldChunk(platforms[platforms.length - 1].x + 300);
    }

    // Caída libre
    if (player.y > 600 || player.y < -300) {
        respawnAtCheckpoint();
    }

    cameraX = player.x - 200;
    gameDistance = Math.max(gameDistance, Math.floor(player.x / 10));
    document.getElementById('dist-display').innerText = gameDistance + 'm';

    // Actualizar partículas y nebulosas
    for (let i = particles.length - 1; i >= 0; i--) {
        let p = particles[i];
        p.x += p.vx; p.y += p.vy;
        p.life--;
        if (p.life <= 0) particles.splice(i, 1);
    }

    nebulae.forEach(n => {
        n.x += n.vx;
        if (n.x < cameraX - 300) n.x = cameraX + 1200;
    });
}

function drawBackground() {
    ctx.fillStyle = '#010005';
    ctx.fillRect(cameraX, 0, 900, 450);

    nebulae.forEach(n => {
        let grad = ctx.createRadialGradient(n.x, n.y, 10, n.x, n.y, n.radius);
        grad.addColorStop(0, n.color);
        grad.addColorStop(1, 'rgba(0,0,0,0)');
        ctx.fillStyle = grad;
        ctx.beginPath();
        ctx.arc(n.x, n.y, n.radius, 0, Math.PI * 2);
        ctx.fill();
    });

    ctx.strokeStyle = 'rgba(0, 240, 255, 0.08)';
    ctx.lineWidth = 1;
    for (let x = Math.floor(cameraX / 50) * 50; x < cameraX + 950; x += 50) {
        ctx.beginPath();
        ctx.moveTo(x, 0);
        ctx.lineTo(x, 450);
        ctx.stroke();
    }
}

function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    ctx.save();

    if (screenShake > 0) {
        ctx.translate((Math.random() - 0.5) * screenShake, (Math.random() - 0.5) * screenShake);
    }

    ctx.translate(-cameraX, 0);

    drawBackground();

    platforms.forEach(p => {
        ctx.fillStyle = '#060010';
        ctx.fillRect(p.x, p.y, p.width, p.height);

        ctx.strokeStyle = strobeColor;
        ctx.shadowColor = strobeColor;
        ctx.shadowBlur = 10;
        ctx.lineWidth = 2;
        ctx.strokeRect(p.x, p.y, p.width, p.height);
        ctx.shadowBlur = 0;
    });

    spikes.forEach(s => {
        ctx.save();
        ctx.fillStyle = '#ff0055';
        ctx.shadowColor = '#ff0055';
        ctx.shadowBlur = 12;
        ctx.beginPath();
        ctx.moveTo(s.x, s.y + s.height);
        ctx.lineTo(s.x + s.width / 2, s.y);
        ctx.lineTo(s.x + s.width, s.y + s.height);
        ctx.closePath();
        ctx.fill();
        ctx.restore();
    });

    coins.forEach(c => {
        if (!c.collected) {
            ctx.save();
            ctx.fillStyle = '#ffe600';
            ctx.shadowColor = '#ffe600';
            ctx.shadowBlur = 18;
            ctx.beginPath();
            ctx.arc(c.x, c.y, c.radius + Math.sin(Date.now() * 0.01) * 3, 0, Math.PI * 2);
            ctx.fill();
            ctx.restore();
        }
    });

    tunnels.forEach(t => {
        ctx.save();
        ctx.fillStyle = 'rgba(255, 0, 85, 0.35)';
        ctx.strokeStyle = '#ff0055';
        ctx.lineWidth = 3;
        ctx.shadowColor = '#ff0055';
        ctx.shadowBlur = 20;
        ctx.fillRect(t.x, t.y, t.width, t.height);
        ctx.strokeRect(t.x, t.y, t.width, t.height);
        ctx.restore();
    });

    checkpoints.forEach(cp => {
        ctx.save();
        ctx.fillStyle = cp.passed ? '#00ffaa' : '#00f0ff';
        ctx.shadowColor = ctx.fillStyle;
        ctx.shadowBlur = 25;
        ctx.fillRect(cp.x, cp.y, cp.width, cp.height);
        ctx.restore();
    });

    player.trail.forEach((t, idx) => {
        ctx.save();
        ctx.translate(t.x + player.width/2, t.y + player.height/2);
        ctx.rotate(t.rotation);
        ctx.fillStyle = isInvincible ? `rgba(255, 230, 0, ${idx * 0.12})` : `rgba(0, 240, 255, ${idx * 0.09})`;
        ctx.fillRect(-player.width/2, -player.height/2, player.width, player.height);
        ctx.restore();
    });

    ctx.save();
    ctx.translate(player.x + player.width/2, player.y + player.height/2);
    ctx.rotate(player.rotation);

    ctx.fillStyle = '#000';
    ctx.fillRect(-player.width/2, -player.height/2, player.width, player.height);

    ctx.strokeStyle = isInvincible ? (Math.sin(Date.now() * 0.03) > 0 ? '#ffe600' : '#ff0055') : (jumpCount === 2 ? '#ff0055' : '#00f0ff');
    ctx.shadowColor = ctx.strokeStyle;
    ctx.shadowBlur = isInvincible ? 30 : 18;
    ctx.lineWidth = 4;
    ctx.strokeRect(-player.width/2, -player.height/2, player.width, player.height);

    ctx.fillStyle = isInvincible ? '#ffffff' : '#ff0055';
    ctx.fillRect(-5, -5, 10, 10);
    ctx.restore();

    enemies.forEach(e => {
        ctx.save();
        ctx.fillStyle = '#ff0055';
        ctx.shadowColor = '#ff0055';
        ctx.shadowBlur = 18;
        ctx.beginPath();
        ctx.arc(e.x + e.width/2, e.y + e.height/2, e.width/2, 0, Math.PI * 2);
        ctx.fill();

        ctx.fillStyle = '#ffffff';
        ctx.fillRect(e.x + 8, e.y + 10, 5, 5);
        ctx.fillRect(e.x + 18, e.y + 10, 5, 5);
        ctx.restore();
    });

    particles.forEach(p => {
        ctx.fillStyle = p.color;
        ctx.fillRect(p.x, p.y, p.size, p.size);
    });

    ctx.restore();
}

function startGame(mode) {
    initAudio();
    document.getElementById('modal-start').style.display = 'none';
    if (mode === 'mobile') document.body.classList.add('mobile-mode');

    platforms = []; spikes = []; enemies = []; portals = []; tunnels = []; coins = []; checkpoints = [];
    generateWorldChunk(0);

    gameLoop();
}

function gameLoop() {
    update();
    draw();
    requestAnimationFrame(gameLoop);
}

// Teclado
window.addEventListener('keydown', (e) => {
    if (e.key === ' ' || e.key === 'ArrowUp' || e.key.toLowerCase() === 'w') triggerJump();
});

// Manejador multitáctil para la pantalla completa
const gameWrapper = document.getElementById('game-wrapper');
const mobileBtn = document.getElementById('btn-jump');

function handleTouchJump(e) {
    // Si el modal inicial está visible, no activar el salto
    if (document.getElementById('modal-start').style.display !== 'none') return;
    if (e && e.cancelable) e.preventDefault();
    triggerJump();
}

// Permitir tocar en cualquier lugar del área de juego
gameWrapper.addEventListener('touchstart', handleTouchJump, { passive: false });
gameWrapper.addEventListener('mousedown', (e) => {
    if (document.getElementById('modal-start').style.display === 'none') {
        triggerJump();
    }
});

// Bloquear pinch-to-zoom y gestos táctiles por defecto
document.addEventListener('touchmove', (e) => {
    if (e.scale !== 1) { e.preventDefault(); }
}, { passive: false });

let lastTouchEnd = 0;
document.addEventListener('touchend', (e) => {
    const now = (new Date()).getTime();
    if (now - lastTouchEnd <= 300) {
        e.preventDefault();
    }
    lastTouchEnd = now;
}, false);
</script>

</body>
</html>