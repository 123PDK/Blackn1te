<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bl@ckn1te | SNI Host Analyser</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary-color: #00ff41;
            --secondary-color: #008f11;
            --bg-color: #0a0e14;
            --terminal-bg: rgba(0, 0, 0, 0.85);
            --text-color: #ffffff;
            --danger-color: #ff5555;
            --warning-color: #ffb86c;
            --info-color: #8be9fd;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Courier New', monospace;
        }
        
        body {
            background-color: var(--bg-color);
            color: var(--text-color);
            background-image: 
                radial-gradient(circle at 10% 20%, rgba(0, 255, 65, 0.05) 0%, transparent 20%),
                radial-gradient(circle at 90% 80%, rgba(0, 143, 17, 0.05) 0%, transparent 20%);
            min-height: 100vh;
            overflow-x: hidden;
        }
        
        .scan-line {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 2px;
            background: linear-gradient(to right, transparent, var(--primary-color), transparent);
            animation: scan 3s linear infinite;
            z-index: 1000;
        }
        
        @keyframes scan {
            0% { top: 0; }
            100% { top: 100%; }
        }
        
        .terminal-container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        .terminal-header {
            background-color: var(--terminal-bg);
            border-radius: 8px 8px 0 0;
            padding: 15px 20px;
            border-bottom: 1px solid var(--secondary-color);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .logo {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .logo h1 {
            color: var(--primary-color);
            font-size: 1.8rem;
            text-shadow: 0 0 10px rgba(0, 255, 65, 0.5);
        }
        
        .logo-icon {
            color: var(--primary-color);
            font-size: 1.5rem;
        }
        
        .country-selector {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .country-selector select {
            background-color: #111;
            color: var(--primary-color);
            border: 1px solid var(--secondary-color);
            padding: 5px 10px;
            border-radius: 4px;
            font-size: 0.9rem;
        }
        
        .flag {
            font-size: 1.5rem;
        }
        
        .terminal-body {
            background-color: var(--terminal-bg);
            border-radius: 0 0 8px 8px;
            padding: 25px;
            min-height: 70vh;
            box-shadow: 0 0 30px rgba(0, 255, 65, 0.1);
            position: relative;
            overflow: hidden;
        }
        
        .terminal-body::before {
            content: "";
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: 
                repeating-linear-gradient(
                    0deg,
                    rgba(0, 255, 65, 0.03) 0px,
                    rgba(0, 255, 65, 0.03) 1px,
                    transparent 1px,
                    transparent 2px
                );
            pointer-events: none;
            z-index: 0;
        }
        
        .terminal-content {
            position: relative;
            z-index: 1;
        }
        
        .command-line {
            display: flex;
            align-items: center;
            margin-bottom: 20px;
            flex-wrap: wrap;
            gap: 10px;
        }
        
        .prompt {
            color: var(--primary-color);
            font-weight: bold;
            margin-right: 10px;
            white-space: nowrap;
        }
        
        .blinking-cursor {
            display: inline-block;
            width: 8px;
            height: 16px;
            background-color: var(--primary-color);
            animation: blink 1s infinite;
            margin-left: 5px;
            vertical-align: middle;
        }
        
        @keyframes blink {
            0%, 50% { opacity: 1; }
            51%, 100% { opacity: 0; }
        }
        
        .input-group {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            width: 100%;
            margin-bottom: 20px;
        }
        
        .url-input {
            flex-grow: 1;
            background-color: #111;
            border: 1px solid var(--secondary-color);
            color: var(--text-color);
            padding: 12px 15px;
            border-radius: 4px;
            font-size: 1rem;
        }
        
        .url-input:focus {
            outline: none;
            border-color: var(--primary-color);
            box-shadow: 0 0 10px rgba(0, 255, 65, 0.3);
        }
        
        .action-buttons {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-bottom: 30px;
        }
        
        .btn {
            padding: 12px 20px;
            border: none;
            border-radius: 4px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .btn-primary {
            background-color: var(--primary-color);
            color: #000;
        }
        
        .btn-primary:hover {
            background-color: #00cc33;
            box-shadow: 0 0 15px rgba(0, 255, 65, 0.5);
        }
        
        .btn-secondary {
            background-color: #333;
            color: var(--text-color);
            border: 1px solid var(--secondary-color);
        }
        
        .btn-secondary:hover {
            background-color: #444;
            border-color: var(--primary-color);
        }
        
        .btn-danger {
            background-color: var(--danger-color);
            color: white;
        }
        
        .btn-danger:hover {
            background-color: #ff3333;
            box-shadow: 0 0 15px rgba(255, 85, 85, 0.5);
        }
        
        .results-container {
            margin-top: 30px;
        }
        
        .results-header {
            color: var(--primary-color);
            border-bottom: 1px solid var(--secondary-color);
            padding-bottom: 10px;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .status-indicator {
            display: inline-block;
            width: 12px;
            height: 12px;
            border-radius: 50%;
            margin-right: 8px;
        }
        
        .status-online {
            background-color: var(--primary-color);
            box-shadow: 0 0 10px var(--primary-color);
        }
        
        .status-offline {
            background-color: var(--danger-color);
            box-shadow: 0 0 10px var(--danger-color);
        }
        
        .status-unknown {
            background-color: var(--warning-color);
            box-shadow: 0 0 10px var(--warning-color);
        }
        
        .results-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .result-card {
            background-color: rgba(0, 0, 0, 0.5);
            border: 1px solid #333;
            border-radius: 6px;
            padding: 15px;
            transition: all 0.3s;
        }
        
        .result-card:hover {
            border-color: var(--secondary-color);
            box-shadow: 0 0 15px rgba(0, 255, 65, 0.1);
        }
        
        .result-title {
            color: var(--info-color);
            margin-bottom: 10px;
            font-size: 1.1rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .result-data {
            font-family: monospace;
            font-size: 0.9rem;
            color: #ccc;
            overflow-wrap: break-word;
        }
        
        .terminal-output {
            background-color: rgba(0, 0, 0, 0.7);
            border: 1px solid #333;
            border-radius: 6px;
            padding: 15px;
            min-height: 150px;
            max-height: 300px;
            overflow-y: auto;
            margin-bottom: 30px;
            font-family: monospace;
            font-size: 0.9rem;
            color: #ccc;
        }
        
        .terminal-output-line {
            margin-bottom: 5px;
            display: flex;
            align-items: flex-start;
        }
        
        .terminal-output-line .prompt {
            min-width: 100px;
        }
        
        .about-section {
            margin-top: 40px;
            padding-top: 20px;
            border-top: 1px solid #333;
        }
        
        .about-section h2 {
            color: var(--primary-color);
            margin-bottom: 15px;
        }
        
        .about-section p {
            line-height: 1.6;
            margin-bottom: 15px;
            color: #ccc;
        }
        
        .contact-section {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            margin-top: 20px;
        }
        
        .contact-item {
            display: flex;
            align-items: center;
            gap: 10px;
            color: #ccc;
        }
        
        .contact-icon {
            color: var(--primary-color);
        }
        
        .footer {
            text-align: center;
            margin-top: 40px;
            padding-top: 20px;
            border-top: 1px solid #333;
            color: #666;
            font-size: 0.9rem;
        }
        
        @media (max-width: 768px) {
            .terminal-header {
                flex-direction: column;
                gap: 15px;
            }
            
            .action-buttons {
                justify-content: center;
            }
            
            .btn {
                padding: 10px 15px;
                font-size: 0.9rem;
            }
            
            .results-grid {
                grid-template-columns: 1fr;
            }
        }
        
        .glitch {
            position: relative;
            animation: glitch 5s infinite;
        }
        
        @keyframes glitch {
            0% { text-shadow: 0 0 10px var(--primary-color); }
            97% { text-shadow: 0 0 10px var(--primary-color); }
            98% { text-shadow: -2px 0 5px var(--danger-color), 2px 0 5px var(--info-color); }
            99% { text-shadow: 2px 0 5px var(--danger-color), -2px 0 5px var(--info-color); }
            100% { text-shadow: 0 0 10px var(--primary-color); }
        }
    </style>
</head>
<body>
    <div class="scan-line"></div>
    
    <div class="terminal-container">
        <div class="terminal-header">
            <div class="logo">
                <i class="fas fa-terminal logo-icon"></i>
                <h1 class="glitch">Bl@ckn1te | SNI Host Analyser</h1>
            </div>
            <div class="country-selector">
                <span class="flag">🌍</span>
                <select id="countrySelect">
                    <option value="ZW">Zimbabwe 🇿🇼</option>
                    <option value="US">United States 🇺🇸</option>
                    <option value="GB">United Kingdom 🇬🇧</option>
                    <option value="ZA">South Africa 🇿🇦</option>
                    <option value="KE">Kenya 🇰🇪</option>
                    <option value="NG">Nigeria 🇳🇬</option>
                    <option value="IN">India 🇮🇳</option>
                    <option value="CN">China 🇨🇳</option>
                    <option value="RU">Russia 🇷🇺</option>
                    <option value="BR">Brazil 🇧🇷</option>
                </select>
            </div>
        </div>
        
        <div class="terminal-body">
            <div class="terminal-content">
                <div class="command-line">
                    <span class="prompt">root@bl@ckn1te:~#</span>
                    <span id="commandText">SNI Host Analyser v2.0 Initialized</span>
                    <span class="blinking-cursor"></span>
                </div>
                
                <div class="input-group">
                    <input type="text" class="url-input" id="hostInput" placeholder="Enter host URL (e.g., https://example.com)" value="https://api.example.com">
                </div>
                
                <div class="action-buttons">
                    <button class="btn btn-primary" id="checkBtn">
                        <i class="fas fa-satellite-dish"></i> Check API Status
                    </button>
                    <button class="btn btn-secondary" id="pingBtn">
                        <i class="fas fa-network-wired"></i> Ping Host
                    </button>
                    <button class="btn btn-secondary" id="tracerouteBtn">
                        <i class="fas fa-route"></i> Traceroute
                    </button>
                    <button class="btn btn-secondary" id="dnsBtn">
                        <i class="fas fa-server"></i> DNS Lookup
                    </button>
                    <button class="btn btn-danger" id="clearBtn">
                        <i class="fas fa-broom"></i> Clear
                    </button>
                </div>
                
                <div class="results-container">
                    <h2 class="results-header">
                        <i class="fas fa-poll"></i> Analysis Results
                        <span class="status-indicator status-unknown" id="statusIndicator"></span>
                    </h2>
                    
                    <div class="results-grid">
                        <div class="result-card">
                            <div class="result-title">
                                <span>Host Status</span>
                            </div>
                            <div class="result-data" id="hostStatus">Waiting for analysis...</div>
                        </div>
                        
                        <div class="result-card">
                            <div class="result-title">
                                <span>Response Time</span>
                            </div>
                            <div class="result-data" id="responseTime">-- ms</div>
                        </div>
                        
                        <div class="result-card">
                            <div class="result-title">
                                <span>Server Location</span>
                            </div>
                            <div class="result-data" id="serverLocation">Unknown</div>
                        </div>
                        
                        <div class="result-card">
                            <div class="result-title">
                                <span>SSL Certificate</span>
                            </div>
                            <div class="result-data" id="sslStatus">Not checked</div>
                        </div>
                    </div>
                    
                    <div class="terminal-output" id="terminalOutput">
                        <div class="terminal-output-line">
                            <span class="prompt">system:</span>
                            <span>SNI Host Analyser Terminal Ready</span>
                        </div>
                        <div class="terminal-output-line">
                            <span class="prompt">system:</span>
                            <span>Enter a host URL and select an analysis option to begin</span>
                        </div>
                    </div>
                </div>
                
                <div class="about-section">
                    <h2><i class="fas fa-user-secret"></i> About Bl@ckn1te</h2>
                    <p>I'm Bl@ckn1te, a passionate programmer and ethical hacker with expertise in cybersecurity, network analysis, and web development. My mission is to create tools that help analyze and secure digital infrastructure.</p>
                    <p>Specializing in crafting configuration files, developing security plugins, and building analytical tools like this SNI Host Analyser, I focus on creating solutions that are both powerful and user-friendly.</p>
                    <p>This tool allows you to analyze SNI hosts across multiple countries, check API status, perform ping tests, traceroute analysis, and DNS lookups to ensure your services are running optimally.</p>
                    
                    <div class="contact-section">
                        <div class="contact-item">
                            <i class="fas fa-phone contact-icon"></i>
                            <span>+263 780 540 231</span>
                        </div>
                        <div class="contact-item">
                            <i class="fas fa-phone contact-icon"></i>
                            <span>+263 714 799 483</span>
                        </div>
                        <div class="contact-item">
                            <i class="fas fa-envelope contact-icon"></i>
                            <span>pardonk2005@gmail.com</span>
                        </div>
                        <div class="contact-item">
                            <i class="fas fa-globe contact-icon"></i>
                            <span>Support this project: Contact for custom solutions</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="footer">
            <p>Bl@ckn1te SNI Host Analyser v2.0 | Terminal Interface | &copy; 2026 | All analysis is simulated for demonstration purposes</p>
        </div>
    </div>

    <script>
        // DOM Elements
        const hostInput = document.getElementById('hostInput');
        const checkBtn = document.getElementById('checkBtn');
        const pingBtn = document.getElementById('pingBtn');
        const tracerouteBtn = document.getElementById('tracerouteBtn');
        const dnsBtn = document.getElementById('dnsBtn');
        const clearBtn = document.getElementById('clearBtn');
        const countrySelect = document.getElementById('countrySelect');
        const terminalOutput = document.getElementById('terminalOutput');
        const commandText = document.getElementById('commandText');
        const statusIndicator = document.getElementById('statusIndicator');
        const hostStatus = document.getElementById('hostStatus');
        const responseTime = document.getElementById('responseTime');
        const serverLocation = document.getElementById('serverLocation');
        const sslStatus = document.getElementById('sslStatus');
        
        // Country data for simulation
        const countryData = {
            'ZW': { name: 'Zimbabwe', city: 'Harare', flag: '🇿🇼' },
            'US': { name: 'United States', city: 'New York', flag: '🇺🇸' },
            'GB': { name: 'United Kingdom', city: 'London', flag: '🇬🇧' },
            'ZA': { name: 'South Africa', city: 'Johannesburg', flag: '🇿🇦' },
            'KE': { name: 'Kenya', city: 'Nairobi', flag: '🇰🇪' },
            'NG': { name: 'Nigeria', city: 'Lagos', flag: '🇳🇬' },
            'IN': { name: 'India', city: 'Mumbai', flag: '🇮🇳' },
            'CN': { name: 'China', city: 'Beijing', flag: '🇨🇳' },
            'RU': { name: 'Russia', city: 'Moscow', flag: '🇷🇺' },
            'BR': { name: 'Brazil', city: 'São Paulo', flag: '🇧🇷' }
        };
        
        // Command simulation
        const commands = [
            "SNI Host Analyser v2.0 Initialized",
            "Loading security protocols...",
            "Establishing secure connection...",
            "Ready for host analysis"
        ];
        
        let commandIndex = 0;
        
        // Rotate through commands
        function rotateCommand() {
            commandText.textContent = commands[commandIndex];
            commandIndex = (commandIndex + 1) % commands.length;
        }
        
        // Set initial command rotation
        setInterval(rotateCommand, 3000);
        
        // Add output line to terminal
        function addOutputLine(prompt, message, color = '#ccc') {
            const line = document.createElement('div');
            line.className = 'terminal-output-line';
            line.innerHTML = `<span class="prompt">${prompt}:</span> <span style="color: ${color}">${message}</span>`;
            terminalOutput.appendChild(line);
            terminalOutput.scrollTop = terminalOutput.scrollHeight;
        }
        
        // Simulate API check
        function checkAPIStatus() {
            const host = hostInput.value || 'https://api.example.com';
            const country = countrySelect.value;
            const countryInfo = countryData[country];
            
            // Update UI
            commandText.textContent = `check_api_status --host=${host} --country=${country}`;
            statusIndicator.className = 'status-indicator status-unknown';
            hostStatus.textContent = 'Checking...';
            responseTime.textContent = '-- ms';
            serverLocation.textContent = `${countryInfo.city}, ${countryInfo.name} ${countryInfo.flag}`;
            sslStatus.textContent = 'Verifying...';
            
            // Simulate API check with random result
            addOutputLine('system', `Initiating API status check for ${host} from ${countryInfo.name}...`, '#00ff41');
            
            setTimeout(() => {
                const isOnline = Math.random() > 0.3; // 70% chance of being online
                const pingTime = Math.floor(Math.random() * 300) + 20;
                const sslValid = Math.random() > 0.2; // 80% chance of valid SSL
                
                if (isOnline) {
                    statusIndicator.className = 'status-indicator status-online';
                    hostStatus.textContent = 'ONLINE';
                    hostStatus.style.color = '#00ff41';
                    addOutputLine('api-check', `SUCCESS: Host is online (${pingTime}ms response)`, '#00ff41');
                } else {
                    statusIndicator.className = 'status-indicator status-offline';
                    hostStatus.textContent = 'OFFLINE';
                    hostStatus.style.color = '#ff5555';
                    addOutputLine('api-check', `ERROR: Host is offline or unreachable`, '#ff5555');
                }
                
                responseTime.textContent = `${pingTime} ms`;
                sslStatus.textContent = sslValid ? 'Valid SSL Certificate' : 'SSL Certificate Issue';
                sslStatus.style.color = sslValid ? '#00ff41' : '#ffb86c';
                
                addOutputLine('system', `API status check completed for ${host}`, '#8be9fd');
            }, 1500);
        }
        
        // Simulate ping test
        function pingHost() {
            const host = hostInput.value || 'https://api.example.com';
            commandText.textContent = `ping --host=${host} --count=4`;
            addOutputLine('system', `Initiating ping test to ${host}...`, '#00ff41');
            
            setTimeout(() => {
                addOutputLine('ping', `PING ${host} (192.168.1.1): 56 data bytes`, '#8be9fd');
                
                for (let i = 0; i < 4; i++) {
                    const pingTime = Math.floor(Math.random() * 100) + 20;
                    setTimeout(() => {
                        addOutputLine('ping', `64 bytes from ${host}: icmp_seq=${i} ttl=57 time=${pingTime}ms`, '#ccc');
                        
                        if (i === 3) {
                            const avgPing = Math.floor(Math.random() * 80) + 30;
                            addOutputLine('ping', `--- ${host} ping statistics ---`, '#8be9fd');
                            addOutputLine('ping', `4 packets transmitted, 4 received, 0% packet loss, time 3001ms`, '#ccc');
                            addOutputLine('ping', `rtt min/avg/max/mdev = 20/${avgPing}/100/25 ms`, '#00ff41');
                            addOutputLine('system', `Ping test completed`, '#8be9fd');
                        }
                    }, i * 600);
                }
            }, 1000);
        }
        
        // Simulate traceroute
        function tracerouteHost() {
            const host = hostInput.value || 'https://api.example.com';
            commandText.textContent = `traceroute --host=${host} --max-hops=10`;
            addOutputLine('system', `Initiating traceroute to ${host}...`, '#00ff41');
            
            setTimeout(() => {
                addOutputLine('traceroute', `traceroute to ${host} (93.184.216.34), 10 hops max, 60 byte packets`, '#8be9fd');
                
                const hops = [
                    { ip: '192.168.1.1', time: '1.234 ms' },
                    { ip: '10.10.0.1', time: '10.123 ms' },
                    { ip: '154.16.25.1', time: '15.456 ms' },
                    { ip: '41.74.201.1', time: '25.678 ms' },
                    { ip: '154.16.25.254', time: '30.123 ms' },
                    { ip: '41.74.201.254', time: '45.789 ms' },
                    { ip: '93.184.216.1', time: '80.456 ms' },
                    { ip: '93.184.216.34', time: '95.123 ms' }
                ];
                
                hops.forEach((hop, i) => {
                    setTimeout(() => {
                        const times = `${hop.time} ${hop.time} ${hop.time}`;
                        addOutputLine('traceroute', `${i+1}  ${hop.ip}  ${times}`, '#ccc');
                        
                        if (i === hops.length - 1) {
                            addOutputLine('system', `Traceroute completed`, '#8be9fd');
                        }
                    }, i * 800);
                });
            }, 1000);
        }
        
        // Simulate DNS lookup
        function dnsLookup() {
            const host = hostInput.value || 'https://api.example.com';
            commandText.textContent = `nslookup --host=${host} --type=ANY`;
            addOutputLine('system', `Performing DNS lookup for ${host}...`, '#00ff41');
            
            setTimeout(() => {
                addOutputLine('dns', `Server:         8.8.8.8`, '#8be9fd');
                addOutputLine('dns', `Address:        8.8.8.8#53`, '#8be9fd');
                addOutputLine('dns', ``, '#8be9fd');
                addOutputLine('dns', `Non-authoritative answer:`, '#ccc');
                addOutputLine('dns', `Name:   ${host.replace('https://', '')}`, '#00ff41');
                addOutputLine('dns', `Address: 93.184.216.34`, '#00ff41');
                addOutputLine('dns', `Name:   ${host.replace('https://', '')}`, '#00ff41');
                addOutputLine('dns', `Address: 2606:2800:220:1:248:1893:25c8:1946`, '#00ff41');
                addOutputLine('dns', ``, '#8be9fd');
                
                // Simulate additional records
                setTimeout(() => {
                    addOutputLine('dns', `Authoritative answers can be found from:`, '#ccc');
                    addOutputLine('dns', `ns1.example.com  internet address = 192.0.2.1`, '#ccc');
                    addOutputLine('dns', `ns2.example.com  internet address = 192.0.2.2`, '#ccc');
                    addOutputLine('system', `DNS lookup completed`, '#8be9fd');
                }, 800);
            }, 1000);
        }
        
        // Clear terminal
        function clearTerminal() {
            terminalOutput.innerHTML = '';
            commandText.textContent = 'Terminal cleared';
            statusIndicator.className = 'status-indicator status-unknown';
            hostStatus.textContent = 'Waiting for analysis...';
            responseTime.textContent = '-- ms';
            serverLocation.textContent = 'Unknown';
            sslStatus.textContent = 'Not checked';
            hostStatus.style.color = '';
            sslStatus.style.color = '';
            
            addOutputLine('system', 'Terminal cleared. Ready for new analysis.', '#00ff41');
        }
        
        // Event listeners
        checkBtn.addEventListener('click', checkAPIStatus);
        pingBtn.addEventListener('click', pingHost);
        tracerouteBtn.addEventListener('click', tracerouteHost);
        dnsBtn.addEventListener('click', dnsLookup);
        clearBtn.addEventListener('click', clearTerminal);
        
        // Enter key to check API status
        hostInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                checkAPIStatus();
            }
        });
        
        // Country change updates server location
        countrySelect.addEventListener('change', () => {
            const country = countrySelect.value;
            const countryInfo = countryData[country];
            serverLocation.textContent = `${countryInfo.city}, ${countryInfo.name} ${countryInfo.flag}`;
            addOutputLine('system', `Analysis location changed to ${countryInfo.name} ${countryInfo.flag}`, '#8be9fd');
        });
        
        // Initialize with a simulated check
        setTimeout(() => {
            addOutputLine('system', 'System initialized. Type a host URL or use the sample above.', '#00ff41');
        }, 1000);
    </script>
</body>
</html>