<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HEMA CYBER OS - Ultimate Portfolio</title>
    <style>
        /* =========================================
           PART 1: CSS ADVANCED STYLING (HACKER UI)
           ========================================= */
        @import url('https://fonts.googleapis.com/css2?family=Fira+Code:wght@300;400;600;700&family=Orbitron:wght@400;700;900&display=swap');

        :root {
            --primary: #00f2fe;
            --secondary: #4facfe;
            --danger: #ff3b30;
            --warning: #f5a623;
            --success: #00ff7f;
            --bg-base: #05050a;
            --bg-glass: rgba(10, 15, 30, 0.6);
            --border-glow: rgba(0, 242, 254, 0.3);
            --font-mono: 'Fira Code', monospace;
            --font-cyber: 'Orbitron', sans-serif;
            --scanline: rgba(0, 242, 254, 0.05);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
        }

        body {
            background-color: var(--bg-base);
            color: var(--primary);
            font-family: var(--font-mono);
            height: 100vh;
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            perspective: 1000px;
        }

        /* 1.1 Canvas Background Layer */
        #matrix-bg {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            z-index: 1;
            opacity: 0.15;
        }

        /* 1.2 CRT Scanline Overlay */
        .scanlines {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            z-index: 9999;
            pointer-events: none;
            background: linear-gradient(
                to bottom,
                rgba(255,255,255,0),
                rgba(255,255,255,0) 50%,
                var(--scanline) 50%,
                var(--scanline)
            );
            background-size: 100% 4px;
            animation: scan 10s linear infinite;
        }

        @keyframes scan {
            0% { background-position: 0 0; }
            100% { background-position: 0 100vh; }
        }

        /* 1.3 Main OS Container (Glassmorphism) */
        .os-container {
            position: relative;
            z-index: 10;
            width: 95vw;
            height: 90vh;
            background: var(--bg-glass);
            border: 1px solid var(--border-glow);
            border-radius: 20px;
            backdrop-filter: blur(15px);
            -webkit-backdrop-filter: blur(15px);
            box-shadow: 0 0 50px rgba(0, 242, 254, 0.1), inset 0 0 20px rgba(0, 242, 254, 0.05);
            display: grid;
            grid-template-columns: 250px 1fr 300px;
            grid-template-rows: 60px 1fr 40px;
            overflow: hidden;
            animation: bootUp 2s cubic-bezier(0.1, 0.9, 0.2, 1) forwards;
        }

        @keyframes bootUp {
            0% { transform: scale(0.9) rotateX(10deg); opacity: 0; filter: blur(10px); }
            50% { filter: blur(0px); opacity: 1; }
            100% { transform: scale(1) rotateX(0); opacity: 1; filter: blur(0); }
        }

        /* 1.4 Top Navbar */
        .top-nav {
            grid-column: 1 / -1;
            border-bottom: 1px solid var(--border-glow);
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0 20px;
            background: rgba(0, 0, 0, 0.3);
        }

        .nav-brand {
            font-family: var(--font-cyber);
            font-size: 1.5rem;
            font-weight: 900;
            letter-spacing: 2px;
            text-shadow: 0 0 10px var(--primary);
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .nav-status {
            display: flex;
            gap: 20px;
            font-size: 0.9rem;
        }

        .status-item {
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .status-dot {
            width: 8px;
            height: 8px;
            background: var(--success);
            border-radius: 50%;
            box-shadow: 0 0 8px var(--success);
            animation: blink 1.5s infinite;
        }

        @keyframes blink { 0%, 100% { opacity: 1; } 50% { opacity: 0.3; } }

        /* 1.5 Sidebar Navigation */
        .sidebar {
            border-right: 1px solid var(--border-glow);
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 15px;
            background: rgba(0,0,0,0.2);
        }

        .menu-btn {
            background: transparent;
            border: 1px solid transparent;
            color: var(--primary);
            font-family: var(--font-mono);
            text-align: left;
            padding: 12px 15px;
            border-radius: 8px;
            cursor: pointer;
            transition: 0.3s all;
            font-size: 1rem;
            position: relative;
            overflow: hidden;
        }

        .menu-btn:hover, .menu-btn.active {
            background: rgba(0, 242, 254, 0.1);
            border: 1px solid var(--border-glow);
            box-shadow: inset 0 0 10px rgba(0,242,254,0.2);
        }

        .menu-btn::before {
            content: '';
            position: absolute;
            left: -100%;
            top: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(0,242,254,0.2), transparent);
            transition: 0.5s;
        }
        .menu-btn:hover::before { left: 100%; }

        /* 1.6 Main Content Area - Terminal Simulator */
        .main-workspace {
            padding: 20px;
            position: relative;
            overflow-y: auto;
            scrollbar-width: thin;
            scrollbar-color: var(--primary) transparent;
        }

        .terminal-window {
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            border: 1px solid #333;
            border-radius: 10px;
            display: flex;
            flex-direction: column;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        }

        .terminal-header {
            background: #1a1a2e;
            padding: 10px 15px;
            border-bottom: 1px solid #333;
            display: flex;
            align-items: center;
            gap: 8px;
            border-radius: 10px 10px 0 0;
        }

        .term-btn { width: 12px; height: 12px; border-radius: 50%; }
        .term-close { background: var(--danger); }
        .term-min { background: var(--warning); }
        .term-max { background: var(--success); }
        .term-title { margin-left: 15px; font-size: 0.8rem; color: #888; }

        .terminal-body {
            padding: 20px;
            flex-grow: 1;
            overflow-y: auto;
            color: #00ff00;
            font-size: 1rem;
            line-height: 1.5;
            text-shadow: 0 0 5px rgba(0, 255, 0, 0.5);
        }

        .output-line { margin-bottom: 5px; }
        .output-error { color: var(--danger); text-shadow: 0 0 5px var(--danger); }
        .output-sys { color: var(--primary); }

        .input-line {
            display: flex;
            align-items: center;
            margin-top: 10px;
        }

        .prompt { color: var(--secondary); margin-right: 10px; }
        .terminal-input {
            background: transparent;
            border: none;
            color: #00ff00;
            font-family: var(--font-mono);
            font-size: 1rem;
            flex-grow: 1;
            outline: none;
            text-shadow: 0 0 5px rgba(0, 255, 0, 0.5);
        }

        /* 1.7 Right Panel - System Stats HUD */
        .right-panel {
            border-left: 1px solid var(--border-glow);
            padding: 20px;
            background: rgba(0,0,0,0.2);
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .hud-box {
            border: 1px solid rgba(0, 242, 254, 0.2);
            padding: 15px;
            border-radius: 10px;
            background: rgba(0, 242, 254, 0.02);
            position: relative;
        }

        .hud-box::after {
            content: '';
            position: absolute;
            top: 0; left: 0;
            width: 10px; height: 10px;
            border-top: 2px solid var(--primary);
            border-left: 2px solid var(--primary);
        }
        .hud-box::before {
            content: '';
            position: absolute;
            bottom: 0; right: 0;
            width: 10px; height: 10px;
            border-bottom: 2px solid var(--primary);
            border-right: 2px solid var(--primary);
        }

        .hud-title {
            font-family: var(--font-cyber);
            font-size: 0.8rem;
            color: var(--secondary);
            margin-bottom: 10px;
            text-transform: uppercase;
        }

        .progress-bar {
            width: 100%;
            height: 6px;
            background: #111;
            border-radius: 3px;
            margin-top: 5px;
            overflow: hidden;
        }
        .progress-fill {
            height: 100%;
            background: var(--primary);
            box-shadow: 0 0 10px var(--primary);
            width: 0%;
            transition: width 1s ease;
        }

        /* Radar Animation */
        .radar {
            width: 150px;
            height: 150px;
            border-radius: 50%;
            border: 1px solid var(--success);
            margin: 0 auto;
            position: relative;
            background: radial-gradient(circle, rgba(0,255,127,0.1) 0%, transparent 70%);
            overflow: hidden;
        }
        .radar::before {
            content: '';
            position: absolute;
            top: 50%; left: 50%;
            width: 50%; height: 50%;
            background: linear-gradient(45deg, rgba(0,255,127,0.5) 0%, transparent 50%);
            transform-origin: 0% 0%;
            animation: radarSpin 2s linear infinite;
        }
        @keyframes radarSpin { 100% { transform: rotate(360deg); } }

        /* Footer */
        .bottom-bar {
            grid-column: 1 / -1;
            border-top: 1px solid var(--border-glow);
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0 20px;
            font-size: 0.8rem;
            color: #555;
            background: rgba(0,0,0,0.5);
        }

    </style>
</head>
<body>

    <!-- CSS/JS Effect Overlays -->
    <canvas id="matrix-bg"></canvas>
    <div class="scanlines"></div>

    <!-- Main HTML Structure -->
    <div class="os-container">
        
        <!-- Navbar -->
        <header class="top-nav">
            <div class="nav-brand">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"/>
                </svg>
                HEMA_OS v9.9.9
            </div>
            <div class="nav-status">
                <div class="status-item"><div class="status-dot"></div> ENCRYPTED CONNECTION</div>
                <div class="status-item" id="clock">00:00:00</div>
            </div>
        </header>

        <!-- Sidebar -->
        <aside class="sidebar">
            <button class="menu-btn active" onclick="execCommand('whoami')">>_ TERMINAL</button>
            <button class="menu-btn" onclick="execCommand('cat skills.json')">[+] SKILLS_DB</button>
            <button class="menu-btn" onclick="execCommand('run scan_network')">[*] RADAR_SCAN</button>
            <button class="menu-btn" onclick="execCommand('fetch portfolio')">[@] PORTFOLIO</button>
            <button class="menu-btn" style="color: var(--danger); border-color: rgba(255,59,48,0.3);" onclick="execCommand('sudo destroy')">[!] SYSTEM_OVERRIDE</button>
        </aside>

        <!-- Main Workspace (Terminal) -->
        <main class="main-workspace">
            <div class="terminal-window">
                <div class="terminal-header">
                    <div class="term-btn term-close"></div>
                    <div class="term-btn term-min"></div>
                    <div class="term-btn term-max"></div>
                    <div class="term-title">root@hema-server:~</div>
                </div>
                <div class="terminal-body" id="term-body">
                    <div class="output-sys">Welcome to Hema Cyber OS. Type 'help' to see available commands.</div>
                    <div class="output-sys">Establishing secure tunnel... [OK]</div>
                    
                    <div class="input-line">
                        <span class="prompt">root@hema-server:~$</span>
                        <input type="text" class="terminal-input" id="term-input" autocomplete="off" autofocus>
                    </div>
                </div>
            </div>
        </main>

        <!-- Right Panel (HUD) -->
        <aside class="right-panel">
            <div class="hud-box">
                <div class="hud-title">CPU USAGE</div>
                <div style="font-size: 1.5rem; font-family: var(--font-cyber);" id="cpu-val">42%</div>
                <div class="progress-bar"><div class="progress-fill" id="cpu-bar" style="width: 42%;"></div></div>
            </div>
            
            <div class="hud-box">
                <div class="hud-title">MEMORY ALLOCATION</div>
                <div style="font-size: 1.5rem; font-family: var(--font-cyber);" id="mem-val">2.4 / 16 GB</div>
                <div class="progress-bar"><div class="progress-fill" style="width: 25%; background: var(--warning); box-shadow: 0 0 10px var(--warning);"></div></div>
            </div>

            <div class="hud-box" style="flex-grow: 1; display: flex; flex-direction: column; justify-content: center;">
                <div class="hud-title" style="text-align: center;">NETWORK RADAR</div>
                <div class="radar"></div>
                <div style="text-align: center; margin-top: 15px; font-size: 0.8rem; color: var(--success);" id="radar-status">Scanning for targets...</div>
            </div>
        </aside>

        <!-- Bottom Bar -->
        <footer class="bottom-bar">
            <div>UID: 0 (root) | GID: 0 (root)</div>
            <div>[ HEMA ADVANCED CYBERNETICS © 2026 ]</div>
            <div>IP: 192.168.1.xxx (Masked)</div>
        </footer>

    </div>

    <!-- =========================================
         PART 3: ADVANCED JAVASCRIPT LOGIC
         ========================================= -->
    <script>
        // 3.1 Matrix Digital Rain Canvas Animation
        const canvas = document.getElementById('matrix-bg');
        const ctx = canvas.getContext('2d');
        
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
        
        const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789$+-*/=%""\'#&_(),.;:?!\\|{}<>[]^~'.split('');
        const fontSize = 14;
        const columns = canvas.width / fontSize;
        const drops = [];
        
        for (let x = 0; x < columns; x++) drops[x] = 1;
        
        function drawMatrix() {
            ctx.fillStyle = 'rgba(5, 5, 10, 0.05)';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            ctx.fillStyle = '#00f2fe';
            ctx.font = fontSize + 'px monospace';
            
            for (let i = 0; i < drops.length; i++) {
                const text = chars[Math.floor(Math.random() * chars.length)];
                ctx.fillText(text, i * fontSize, drops[i] * fontSize);
                if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) drops[i] = 0;
                drops[i]++;
            }
        }
        setInterval(drawMatrix, 33);

        window.addEventListener('resize', () => {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        });

        // 3.2 Real-time Clock & HUD Updates
        function updateHUD() {
            // Clock
            const now = new Date();
            document.getElementById('clock').textContent = now.toLocaleTimeString('en-US', { hour12: false });
            
            // Randomize CPU
            const cpu = Math.floor(Math.random() * 60) + 20;
            document.getElementById('cpu-val').textContent = cpu + '%';
            document.getElementById('cpu-bar').style.width = cpu + '%';
            
            // Randomize Network Targets
            if(Math.random() > 0.8) {
                const ips = Math.floor(Math.random() * 15) + 1;
                document.getElementById('radar-status').textContent = `Targets Found: ${ips}`;
                setTimeout(() => document.getElementById('radar-status').textContent = "Scanning for targets...", 2000);
            }
        }
        setInterval(updateHUD, 1000);

        // 3.3 Advanced Terminal Engine
        const termInput = document.getElementById('term-input');
        const termBody = document.getElementById('term-body');

        // Command Database
        const commands = {
            'help': 'Available commands: whoami, skills, clear, hack, sudo destroy, date',
            'whoami': `<br/>NAME: Ibrahim Fathy Ibrahim<br/>ROLE: Advanced Cyber Security Specialist & Ethical Hacker<br/>LOCATION: Egypt<br/>PHONE: +20 01202060839<br/>MISSION: Securing the Digital Realm.<br/>`,
            'skills': `<br/>[+] OFFENSIVE SECURITY<br/> - Penetration Testing<br/> - Malware Analysis<br/> - Vulnerability Assessment<br/><br/>[+] LANGUAGES<br/> - Python<br/> - C / C++<br/> - Bash Scripting<br/> - JavaScript / TypeScript<br/>`,
            'date': new Date().toString(),
            'cat skills.json': 'Displaying JSON Payload...<br/>{<br/>  "core": "Ethical Hacking",<br/>  "level": "Expert",<br/>  "tools": ["Kali", "Nmap", "Metasploit", "Burp Suite"]<br/>}',
            'run scan_network': 'Initializing NMAP Stealth Scan...<br/>[==================] 100%<br/>Host is UP. Open ports: 22(ssh), 80(http), 443(https).',
            'fetch portfolio': 'Downloading Projects...<br/>1. Network Sniffer v2.0<br/>2. Zero-Day Exploit POC<br/>3. Cloud Security Architecture<br/>--> Access Granted.',
            'sudo destroy': '<span class="output-error">CRITICAL ERROR: Kernel Panic. Core meltdown initiated... Just kidding. Security blocks active.</span>',
        };

        function printToTerminal(text, isHTML = false, className = '') {
            const inputLine = termBody.querySelector('.input-line');
            const newOutput = document.createElement('div');
            newOutput.className = `output-line ${className}`;
            
            if (isHTML) newOutput.innerHTML = text;
            else newOutput.textContent = text;
            
            termBody.insertBefore(newOutput, inputLine);
            termBody.scrollTop = termBody.scrollHeight;
        }

        // Typing Effect Function for Terminal
        async function typeToTerminal(text, speed = 20) {
            const inputLine = termBody.querySelector('.input-line');
            const newOutput = document.createElement('div');
            newOutput.className = `output-line`;
            termBody.insertBefore(newOutput, inputLine);
            
            for(let i = 0; i < text.length; i++) {
                newOutput.innerHTML += text.charAt(i);
                termBody.scrollTop = termBody.scrollHeight;
                await new Promise(r => setTimeout(r, speed));
            }
        }

        termInput.addEventListener('keydown', async (e) => {
            if (e.key === 'Enter') {
                const cmd = termInput.value.trim().toLowerCase();
                if (!cmd) return;
                
                printToTerminal(`root@hema-server:~$ ${cmd}`, false, 'output-sys');
                termInput.value = '';
                termInput.disabled = true; // Lock input while processing

                if (cmd === 'clear') {
                    const lines = termBody.querySelectorAll('.output-line');
                    lines.forEach(line => line.remove());
                } 
                else if (cmd === 'hack') {
                    await typeToTerminal("INITIALIZING BREACH PROTOCOL...", 50);
                    await typeToTerminal("BYPASSING MAINFRAME FIREWALL... [SUCCESS]", 50);
                    await typeToTerminal("EXTRACTING ROOT HASHES...", 50);
                    printToTerminal("ACCESS GRANTED. Welcome to the Matrix.", true, "output-sys");
                }
                else if (commands[cmd]) {
                    printToTerminal(commands[cmd], true);
                } 
                else {
                    printToTerminal(`bash: ${cmd}: command not found`, false, 'output-error');
                }
                
                termInput.disabled = false;
                termInput.focus();
            }
        });

        // Global Command Execution for Buttons
        window.execCommand = function(cmd) {
            termInput.value = cmd;
            termInput.dispatchEvent(new KeyboardEvent('keydown', { key: 'Enter' }));
            
            // Visual Active State for sidebar
            document.querySelectorAll('.menu-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
        }

        // Ensure focus is always on terminal when clicking inside it
        document.querySelector('.terminal-window').addEventListener('click', () => {
            termInput.focus();
        });
    </script>
</body>
</html>
