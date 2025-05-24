
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>0.10.23.3</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&display=swap');
        
        body {
            font-family: 'Share Tech Mono', monospace;
            background-color: #0a0a0a;
            color: #33ff33;
        }
        
        .terminal {
            border: 1px solid #33ff33;
            box-shadow: 0 0 10px #33ff33, 0 0 20px rgba(51, 255, 51, 0.2);
            animation: pulse 2s infinite alternate;
        }
        
        .terminal-header {
            border-bottom: 1px solid #33ff33;
        }
        
        .data-row {
            border-bottom: 1px solid rgba(51, 255, 51, 0.2);
        }
        
        .data-row:last-child {
            border-bottom: none;
        }
        
        .blink {
            animation: blink 1s step-end infinite;
        }
        
        .typing {
            overflow: hidden;
            white-space: nowrap;
            animation: typing 3s steps(40, end);
        }
        
        .grid-bg {
            background-image: radial-gradient(#33ff3320 1px, transparent 1px);
            background-size: 20px 20px;
        }
        
        @keyframes pulse {
            from { box-shadow: 0 0 10px #33ff33, 0 0 20px rgba(51, 255, 51, 0.2); }
            to { box-shadow: 0 0 15px #33ff33, 0 0 30px rgba(51, 255, 51, 0.4); }
        }
        
        @keyframes blink {
            0%, 100% { opacity: 1; }
            50% { opacity: 0; }
        }
        
        @keyframes typing {
            from { width: 0 }
            to { width: 100% }
        }
        
        .hexagon {
            clip-path: polygon(25% 0%, 75% 0%, 100% 50%, 75% 100%, 25% 100%, 0% 50%);
        }
        
        .matrix-bg {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: -1;
            opacity: 0.15;
        }

        .loader {
            width: 20px;
            height: 20px;
            border: 3px solid rgba(51, 255, 51, 0.3);
            border-radius: 50%;
            border-top-color: #33ff33;
            animation: spin 1s linear infinite;
            display: inline-block;
            vertical-align: middle;
            margin-right: 8px;
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        .status-indicator {
            height: 10px;
            width: 10px;
            border-radius: 50%;
            display: inline-block;
            margin-right: 8px;
        }

        .status-secure {
            background-color: #33ff33;
            box-shadow: 0 0 5px #33ff33;
        }

        .status-warning {
            background-color: #ffcc00;
            box-shadow: 0 0 5px #ffcc00;
        }

        .status-danger {
            background-color: #ff3333;
            box-shadow: 0 0 5px #ff3333;
        }

        .progress-bar {
            height: 4px;
            background-color: rgba(51, 255, 51, 0.2);
            border-radius: 2px;
            overflow: hidden;
            margin-top: 4px;
        }

        .progress-fill {
            height: 100%;
            background-color: #33ff33;
            box-shadow: 0 0 5px #33ff33;
            width: 0%;
            transition: width 0.5s ease;
        }
    </style>
</head>
<body class="grid-bg min-h-screen">
    <div class="matrix-bg" id="matrix"></div>
    
    <div class="container mx-auto px-4 py-12">
        <header class="mb-8 text-center">
            <div class="flex justify-center items-center mb-4">
                <div class="hexagon bg-green-500 w-12 h-12 flex items-center justify-center mr-3">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-black" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z" />
                    </svg>
                </div>
                <h1 class="text-4xl md:text-5xl font-bold tracking-tighter">NET<span class="text-green-500">TRACKER</span></h1>
            </div>
            <p class="text-green-400 typing">// Initializing real-time system reconnaissance...</p>
        </header>
        
        <div class="terminal bg-black bg-opacity-80 rounded-lg p-4 mb-8 max-w-3xl mx-auto">
            <div class="terminal-header flex justify-between items-center pb-2 mb-4">
                <div class="flex items-center">
                    <span class="h-3 w-3 rounded-full bg-red-500 inline-block mr-2"></span>
                    <span class="h-3 w-3 rounded-full bg-yellow-500 inline-block mr-2"></span>
                    <span class="h-3 w-3 rounded-full bg-green-500 inline-block"></span>
                </div>
                <div class="text-xs text-green-400">user@system:~</div>
                <div class="text-xs text-green-400" id="current-time">00:00:00</div>
            </div>
            
            <div class="mb-4">
                <p class="text-green-400 mb-2">$ <span class="typing">execute_system_scan.sh --deep --no-cache</span></p>
                <p class="text-yellow-400 mb-2" id="scan-status">Scanning network parameters...</p>
                <div class="progress-bar">
                    <div class="progress-fill" id="scan-progress"></div>
                </div>
                <p class="text-green-400 mb-2 mt-2" id="access-message" style="display: none;">Access granted. Displaying real system information:</p>
            </div>
            
            <div class="space-y-1" id="info-container">
                <div class="data-row flex py-2">
                    <div class="w-1/3 text-green-500">IP Address:</div>
                    <div class="w-2/3 text-white" id="ip-address"><div class="loader"></div>Detecting...</div>
                </div>
                <div class="data-row flex py-2">
                    <div class="w-1/3 text-green-500">Location:</div>
                    <div class="w-2/3 text-white" id="location"><div class="loader"></div>Detecting...</div>
                </div>
                <div class="data-row flex py-2">
                    <div class="w-1/3 text-green-500">ISP:</div>
                    <div class="w-2/3 text-white" id="isp"><div class="loader"></div>Detecting...</div>
                </div>
                <div class="data-row flex py-2">
                    <div class="w-1/3 text-green-500">Browser:</div>
                    <div class="w-2/3 text-white" id="browser"><div class="loader"></div>Detecting...</div>
                </div>
                <div class="data-row flex py-2">
                    <div class="w-1/3 text-green-500">Operating System:</div>
                    <div class="w-2/3 text-white" id="os"><div class="loader"></div>Detecting...</div>
                </div>
                <div class="data-row flex py-2">
                    <div class="w-1/3 text-green-500">Screen Resolution:</div>
                    <div class="w-2/3 text-white" id="resolution"><div class="loader"></div>Detecting...</div>
                </div>
                <div class="data-row flex py-2">
                    <div class="w-1/3 text-green-500">Device Memory:</div>
                    <div class="w-2/3 text-white" id="memory"><div class="loader"></div>Detecting...</div>
                </div>
                <div class="data-row flex py-2">
                    <div class="w-1/3 text-green-500">CPU Cores:</div>
                    <div class="w-2/3 text-white" id="cpu-cores"><div class="loader"></div>Detecting...</div>
                </div>
                <div class="data-row flex py-2">
                    <div class="w-1/3 text-green-500">Language:</div>
                    <div class="w-2/3 text-white" id="language"><div class="loader"></div>Detecting...</div>
                </div>
                <div class="data-row flex py-2">
                    <div class="w-1/3 text-green-500">Time Zone:</div>
                    <div class="w-2/3 text-white" id="timezone"><div class="loader"></div>Detecting...</div>
                </div>
                <div class="data-row flex py-2">
                    <div class="w-1/3 text-green-500">Connection:</div>
                    <div class="w-2/3 text-white" id="connection"><div class="loader"></div>Detecting...</div>
                </div>
                <div class="data-row flex py-2">
                    <div class="w-1/3 text-green-500">Battery Status:</div>
                    <div class="w-2/3 text-white" id="battery"><div class="loader"></div>Detecting...</div>
                </div>
                <div class="data-row flex py-2">
                    <div class="w-1/3 text-green-500">User Agent:</div>
                    <div class="w-2/3 text-white text-xs" id="user-agent"><div class="loader"></div>Detecting...</div>
                </div>
                <div class="data-row flex py-2">
                    <div class="w-1/3 text-green-500">Cookies Enabled:</div>
                    <div class="w-2/3 text-white" id="cookies"><div class="loader"></div>Detecting...</div>
                </div>
                <div class="data-row flex py-2">
                    <div class="w-1/3 text-green-500">Do Not Track:</div>
                    <div class="w-2/3 text-white" id="do-not-track"><div class="loader"></div>Detecting...</div>
                </div>
            </div>
            
            <div class="mt-4 pt-2 border-t border-green-500 border-opacity-30">
                <p class="text-green-400">$ <span class="blink">_</span></p>
            </div>
        </div>
        
        <div class="text-center">
            <button id="refresh-btn" class="bg-green-500 hover:bg-green-600 text-black font-bold py-2 px-6 rounded-md transition duration-300 flex items-center mx-auto">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
                </svg>
                Refresh Scan
            </button>
            <p class="text-xs text-green-500 mt-4">Note: All data is collected from your browser and public IP information only.</p>
        </div>
    </div>

    <script>
        // Matrix background effect
        const canvas = document.createElement('canvas');
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
        document.getElementById('matrix').appendChild(canvas);
        
        const ctx = canvas.getContext('2d');
        const characters = "01アイウエオカキクケコサシスセソタチツテトナニヌネノハヒフヘホマミムメモヤユヨラリルレロワヲン";
        const columns = Math.floor(canvas.width / 20);
        const drops = [];
        
        for (let i = 0; i < columns; i++) {
            drops[i] = Math.random() * -100;
        }
        
        function drawMatrix() {
            ctx.fillStyle = "rgba(0, 0, 0, 0.05)";
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            ctx.fillStyle = "#33ff33";
            ctx.font = "15px 'Share Tech Mono'";
            
            for (let i = 0; i < drops.length; i++) {
                const text = characters.charAt(Math.floor(Math.random() * characters.length));
                ctx.fillText(text, i * 20, drops[i] * 20);
                
                if (drops[i] * 20 > canvas.height && Math.random() > 0.975) {
                    drops[i] = 0;
                }
                
                drops[i]++;
            }
        }
        
        // Update time
        function updateTime() {
            const now = new Date();
            const timeString = now.toTimeString().split(' ')[0];
            document.getElementById('current-time').textContent = timeString;
        }
        
        // Simulate progress bar
        function runProgressBar() {
            const progressBar = document.getElementById('scan-progress');
            let width = 0;
            const interval = setInterval(() => {
                if (width >= 100) {
                    clearInterval(interval);
                    document.getElementById('scan-status').textContent = 'Scan complete.';
                    document.getElementById('access-message').style.display = 'block';
                } else {
                    width += Math.random() * 5;
                    if (width > 100) width = 100;
                    progressBar.style.width = width + '%';
                }
            }, 100);
        }
        
        // Get real user information
        async function getUserInfo() {
            runProgressBar();
            
            try {
                // Get IP and location info from ipinfo.io
                const ipResponse = await fetch('https://ipinfo.io/json');
                const ipData = await ipResponse.json();
                
                // IP Address
                document.getElementById('ip-address').innerHTML = `<span class="status-indicator status-secure"></span>${ipData.ip || 'Unknown'}`;
                
                // Location
                if (ipData.city && ipData.region && ipData.country) {
                    document.getElementById('location').innerHTML = `<span class="status-indicator status-secure"></span>${ipData.city}, ${ipData.region}, ${ipData.country}`;
                } else {
                    document.getElementById('location').innerHTML = '<span class="status-indicator status-warning"></span>Location data unavailable';
                }
                
                // ISP
                document.getElementById('isp').innerHTML = `<span class="status-indicator status-secure"></span>${ipData.org || 'Unknown'}`;
                
                // Timezone
                document.getElementById('timezone').innerHTML = `<span class="status-indicator status-secure"></span>${ipData.timezone || Intl.DateTimeFormat().resolvedOptions().timeZone || 'Unknown'}`;
                
                // Browser detection
                const userAgent = navigator.userAgent;
                let browser = 'Unknown';
                if (userAgent.indexOf("Firefox") > -1) {
                    browser = "Mozilla Firefox";
                } else if (userAgent.indexOf("SamsungBrowser") > -1) {
                    browser = "Samsung Browser";
                } else if (userAgent.indexOf("Opera") > -1 || userAgent.indexOf("OPR") > -1) {
                    browser = "Opera";
                } else if (userAgent.indexOf("Trident") > -1) {
                    browser = "Internet Explorer";
                } else if (userAgent.indexOf("Edge") > -1) {
                    browser = "Microsoft Edge";
                } else if (userAgent.indexOf("Chrome") > -1) {
                    browser = "Google Chrome";
                } else if (userAgent.indexOf("Safari") > -1) {
                    browser = "Safari";
                }
                document.getElementById('browser').innerHTML = `<span class="status-indicator status-secure"></span>${browser}`;
                
                // OS detection
                let os = 'Unknown';
                if (userAgent.indexOf("Win") > -1) {
                    os = "Windows";
                    if (userAgent.indexOf("Windows NT 10.0") > -1) os = "Windows 10/11";
                    else if (userAgent.indexOf("Windows NT 6.3") > -1) os = "Windows 8.1";
                    else if (userAgent.indexOf("Windows NT 6.2") > -1) os = "Windows 8";
                    else if (userAgent.indexOf("Windows NT 6.1") > -1) os = "Windows 7";
                } else if (userAgent.indexOf("Mac") > -1) {
                    os = "MacOS";
                } else if (userAgent.indexOf("Linux") > -1) {
                    os = "Linux";
                } else if (userAgent.indexOf("Android") > -1) {
                    os = "Android";
                } else if (userAgent.indexOf("like Mac") > -1) {
                    os = "iOS";
                }
                document.getElementById('os').innerHTML = `<span class="status-indicator status-secure"></span>${os}`;
                
                // Screen resolution
                const width = window.screen.width * window.devicePixelRatio;
                const height = window.screen.height * window.devicePixelRatio;
                document.getElementById('resolution').innerHTML = `<span class="status-indicator status-secure"></span>${width} x ${height} (${window.devicePixelRatio}x pixel ratio)`;
                
                // Device memory
                if (navigator.deviceMemory) {
                    document.getElementById('memory').innerHTML = `<span class="status-indicator status-secure"></span>${navigator.deviceMemory} GB`;
                } else {
                    document.getElementById('memory').innerHTML = '<span class="status-indicator status-warning"></span>Not available';
                }
                
                // CPU cores
                if (navigator.hardwareConcurrency) {
                    document.getElementById('cpu-cores').innerHTML = `<span class="status-indicator status-secure"></span>${navigator.hardwareConcurrency} cores`;
                } else {
                    document.getElementById('cpu-cores').innerHTML = '<span class="status-indicator status-warning"></span>Not available';
                }
                
                // Language
                document.getElementById('language').innerHTML = `<span class="status-indicator status-secure"></span>${navigator.language} (${navigator.languages.join(', ')})`;
                
                // Connection type
                if (navigator.connection) {
                    const conn = navigator.connection;
                    let connType = conn.effectiveType || 'Unknown';
                    connType = connType.toUpperCase();
                    
                    let connDetails = [];
                    if (conn.downlink) connDetails.push(`${conn.downlink} Mbps`);
                    if (conn.rtt) connDetails.push(`${conn.rtt} ms RTT`);
                    if (conn.saveData) connDetails.push('Data Saver ON');
                    
                    const connInfo = connDetails.length > 0 ? 
                        `${connType} (${connDetails.join(', ')})` : connType;
                    
                    document.getElementById('connection').innerHTML = `<span class="status-indicator status-secure"></span>${connInfo}`;
                } else {
                    document.getElementById('connection').innerHTML = '<span class="status-indicator status-warning"></span>Connection info not available';
                }
                
                // Battery status
                if (navigator.getBattery) {
                    try {
                        const battery = await navigator.getBattery();
                        const level = Math.floor(battery.level * 100);
                        const charging = battery.charging ? 'Charging' : 'Not charging';
                        document.getElementById('battery').innerHTML = `<span class="status-indicator ${level < 20 && !battery.charging ? 'status-warning' : 'status-secure'}"></span>${level}% (${charging})`;
                    } catch (e) {
                        document.getElementById('battery').innerHTML = '<span class="status-indicator status-warning"></span>Battery info not available';
                    }
                } else {
                    document.getElementById('battery').innerHTML = '<span class="status-indicator status-warning"></span>Battery API not supported';
                }
                
                // User agent
                document.getElementById('user-agent').innerHTML = `<span class="status-indicator status-secure"></span>${navigator.userAgent}`;
                
                // Cookies enabled
                document.getElementById('cookies').innerHTML = `<span class="status-indicator status-secure"></span>${navigator.cookieEnabled ? 'Enabled' : 'Disabled'}`;
                
                // Do Not Track
                const dnt = navigator.doNotTrack || window.doNotTrack || navigator.msDoNotTrack;
                let dntStatus;
                if (dnt == "1" || dnt == "yes") {
                    dntStatus = '<span class="status-indicator status-secure"></span>Enabled';
                } else if (dnt == "0" || dnt == "no") {
                    dntStatus = '<span class="status-indicator status-warning"></span>Disabled';
                } else {
                    dntStatus = '<span class="status-indicator status-warning"></span>Not specified';
                }
                document.getElementById('do-not-track').innerHTML = dntStatus;
                
            } catch (error) {
                console.error('Error fetching user info:', error);
                document.getElementById('ip-address').innerHTML = '<span class="status-indicator status-danger"></span>Error loading data';
                document.getElementById('location').innerHTML = '<span class="status-indicator status-danger"></span>Error loading data';
            }
        }
        
        // Initialize
        window.addEventListener('load', () => {
            getUserInfo();
            updateTime();
            setInterval(updateTime, 1000);
            setInterval(drawMatrix, 50);
            
            document.getElementById('refresh-btn').addEventListener('click', () => {
                // Reset all fields to loading state
                const infoElements = document.querySelectorAll('#info-container > div > div:nth-child(2)');
                infoElements.forEach(el => {
                    el.innerHTML = '<div class="loader"></div>Detecting...';
                });
                
                // Reset progress bar
                document.getElementById('scan-progress').style.width = '0%';
                document.getElementById('scan-status').textContent = 'Scanning network parameters...';
                document.getElementById('access-message').style.display = 'none';
                
                // Get info again
                setTimeout(getUserInfo, 500);
            });
        });
        
        // Handle window resize for matrix effect
        window.addEventListener('resize', () => {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        });
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'94493db62515bfac',t:'MTc0ODA1MjQ2My4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
