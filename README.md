<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>XSVS - PROMAX UI</title>
    <style>
        /* =========================================
           1. TYPOGRAPHY & VARIABLES
        ========================================= */
        @font-face {
            font-family: 'God of War';
            src: url('GodOfWar.ttf') format('truetype');
            font-display: swap;
        }

        @font-face {
            font-family: 'Zionix';
            src: url('Zionix.ttf') format('truetype');
            font-display: swap;
        }

        :root {
            --bg-color: #050505;
            --glass-bg: rgba(255, 255, 255, 0.03);
            --glass-border: rgba(255, 255, 255, 0.08);
            --glass-blur: blur(16px);
            --text-primary: #ffffff;
            --text-secondary: #888888;
            --accent: #ffffff;
            --success: #00ff88;
            --danger: #ff3366;
            --transition-smooth: all 0.6s cubic-bezier(0.16, 1, 0.3, 1);
            --transition-snappy: all 0.4s cubic-bezier(0.25, 1, 0.5, 1);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-primary);
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            overflow: hidden;
            width: 100vw;
            height: 100vh;
        }

        /* =========================================
           2. UTILITIES & GLASSMORPHISM
        ========================================= */
        .glass-panel {
            background: var(--glass-bg);
            backdrop-filter: var(--glass-blur);
            -webkit-backdrop-filter: var(--glass-blur);
            border: 1px solid var(--glass-border);
            border-radius: 16px;
        }

        .gow-text { font-family: 'God of War', sans-serif; }
        .zionix-text { 
            font-family: 'Zionix', sans-serif; 
            letter-spacing: 2px;
            text-transform: uppercase;
        }

        .page {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.8s cubic-bezier(0.16, 1, 0.3, 1);
            z-index: 10;
        }

        .page.active {
            opacity: 1;
            pointer-events: auto;
            z-index: 20;
        }

        input {
            background: rgba(255, 255, 255, 0.02);
            border: 1px solid rgba(255, 255, 255, 0.1);
            color: white;
            padding: 14px 20px;
            border-radius: 8px;
            font-size: 14px;
            width: 100%;
            outline: none;
            transition: var(--transition-snappy);
        }
        input:focus { border-color: rgba(255,255,255,0.5); }

        button {
            background: rgba(255,255,255,0.05);
            color: white;
            border: 1px solid rgba(255,255,255,0.2);
            padding: 16px;
            border-radius: 8px;
            font-size: 16px;
            cursor: pointer;
            transition: var(--transition-snappy);
        }
        button:active { transform: scale(0.95); background: rgba(255,255,255,0.1); }

        /* =========================================
           3. PAGE 1: ENTRY SCREEN (FIXED LAYOUT)
        ========================================= */
        #page-entry {
            display: flex;
            flex-direction: column;
            justify-content: space-evenly;
            align-items: center;
            padding: 40px 20px;
        }
        
        .team-avatars {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
        }
        .team-avatars span { font-size: 12px; letter-spacing: 3px; color: var(--text-secondary); }
        .avatar-group { display: flex; gap: -10px; }
        .avatar-group img {
            width: 50px; height: 50px; border-radius: 50%;
            border: 2px solid var(--bg-color); object-fit: cover;
            box-shadow: 0 4px 10px rgba(0,0,0,0.5);
        }

        .center-viewbox {
            position: relative;
            width: 300px;
            height: 180px;
            border-radius: 20px;
            overflow: hidden;
            margin: 20px 0;
        }
        .center-viewbox video { width: 100%; height: 100%; object-fit: cover; }
        .center-viewbox .floating-title {
            position: absolute;
            top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            font-size: 42px;
            color: white;
            text-shadow: 0 10px 30px rgba(0,0,0,0.8);
            z-index: 2;
        }

        .social-footer {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 15px;
        }
        .social-icons { display: flex; gap: 25px; }
        .social-icons a { color: white; display: block; width: 24px; height: 24px; opacity: 0.6; transition: var(--transition-snappy); }
        .social-icons a:hover { opacity: 1; transform: translateY(-3px); }
        .social-icons svg { width: 100%; height: 100%; fill: currentColor; }

        /* =========================================
           4. BACKGROUND VIDEO & PAGE 2: LOGIN
        ========================================= */
        .bg-video { position: absolute; top: 0; left: 0; width: 100%; height: 100%; object-fit: cover; z-index: -1; opacity: 0.4; }
        
        .login-box {
            width: 320px;
            padding: 30px 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
            margin: auto;
        }
        .login-box img { width: 80px; height: 80px; border-radius: 16px; object-fit: cover; }
        .login-box p { font-size: 12px; color: var(--text-secondary); text-align: center; }
        
        /* =========================================
           5. PAGE 3: LOADING SCREEN
        ========================================= */
        .loading-container {
            margin: auto;
            width: 200px;
            height: 2px;
            background: rgba(255,255,255,0.1);
            position: relative;
            overflow: hidden;
        }
        .loading-bar {
            position: absolute;
            top: 0; left: 0; height: 100%; width: 0%;
            background: white;
            transition: width linear;
        }

        /* =========================================
           6. MAIN DASHBOARD & TABS
        ========================================= */
        .dashboard-layout {
            display: flex; flex-direction: column; height: 100%; width: 100%;
            position: relative; z-index: 1;
        }
        .header {
            display: flex; justify-content: space-between; align-items: center;
            padding: 20px;
        }
        .header .user-pic {
            width: 40px; height: 40px; border-radius: 50%; object-fit: cover;
            border: 1px solid var(--glass-border);
        }
        
        .tab-content {
            flex: 1; overflow-y: auto; overflow-x: hidden;
            padding: 0 20px 100px 20px; 
            display: none;
            opacity: 0;
            transform: translateY(10px);
            transition: var(--transition-smooth);
        }
        .tab-content.active {
            display: flex; flex-direction: column; opacity: 1; transform: translateY(0);
        }

        /* WIDGETS */
        .widget { margin-bottom: 15px; position: relative; overflow: hidden; }
        
        .slider-widget { height: 150px; border-radius: 12px; }
        .slider-track { display: flex; width: 300%; height: 100%; transition: transform 0.5s ease; }
        .slider-track img { width: 33.33%; height: 100%; object-fit: cover; }
        .slider-text {
            position: absolute; bottom: 10px; left: 10px;
            font-size: 10px; width: 70%;
            color: rgba(255,255,255,0.8);
            text-shadow: 0 2px 4px rgba(0,0,0,0.8);
        }

        .clock-widget {
            padding: 25px; text-align: center; font-size: 36px; letter-spacing: 4px; font-weight: 300;
        }

        .marquee-widget {
            padding: 10px; white-space: nowrap; overflow: hidden;
        }
        .marquee-content {
            display: inline-block; padding-left: 100%;
            animation: marquee 15s linear infinite;
            font-size: 12px; color: var(--text-secondary);
        }
        @keyframes marquee { 0% { transform: translate(0, 0); } 100% { transform: translate(-100%, 0); } }

        /* =========================================
           7. BOTTOM NAVIGATION
        ========================================= */
        .bottom-nav {
            position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%);
            width: 90%; max-width: 400px;
            height: 65px;
            display: flex; justify-content: space-around; align-items: center;
            border-radius: 30px;
            z-index: 100;
        }
        .nav-item {
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            width: 50px; height: 50px; color: var(--text-secondary);
            cursor: pointer; transition: var(--transition-snappy);
            position: relative;
        }
        .nav-item svg { width: 22px; height: 22px; fill: currentColor; margin-bottom: 4px; transition: var(--transition-snappy); }
        .nav-item span { font-size: 9px; }
        .nav-item.active { color: white; }
        .nav-item.active svg { transform: translateY(-3px); }
        
        .fab {
            position: fixed; right: 20px; bottom: 100px;
            width: 50px; height: 50px; border-radius: 25px;
            display: flex; justify-content: center; align-items: center;
            background: rgba(255,255,255,0.1); border: 1px solid rgba(255,255,255,0.2);
            backdrop-filter: blur(10px); color: white; z-index: 99;
            box-shadow: 0 10px 20px rgba(0,0,0,0.5); cursor: pointer;
        }

        /* =========================================
           8. BUG WA PAGE & TOOLS PAGE (FIXED HORIZONTAL SLIDER)
        ========================================= */
        .page-header-video {
            width: 100%; height: 120px; border-radius: 12px; overflow: hidden; position: relative; margin-bottom: 20px;
        }
        .page-header-video video { width: 100%; height: 100%; object-fit: cover; }
        .page-header-video h2 { position: absolute; top: 50%; left: 50%; transform: translate(-50%,-50%); font-size: 28px; }

        .sender-options { display: flex; gap: 10px; margin-bottom: 20px; }
        .sender-card {
            flex: 1; padding: 15px 10px; text-align: center; font-size: 12px;
            border-radius: 10px; cursor: pointer; position: relative;
            transition: var(--transition-snappy); border: 1px solid var(--glass-border);
        }
        .sender-card.active, .bug-card.active { border-color: rgba(255,255,255,0.6); background: rgba(255,255,255,0.2); }
        .tick-icon {
            position: absolute; top: 5px; right: 5px; width: 14px; height: 14px;
            background: var(--success); border-radius: 50%; opacity: 0; transform: scale(0);
            transition: var(--transition-snappy);
        }
        .active .tick-icon { opacity: 1; transform: scale(1); }

        /* HORIZONTAL SLIDER FIX */
        .bug-slider { 
            display: flex; 
            overflow-x: auto; 
            gap: 12px; 
            margin-bottom: 20px; 
            padding-bottom: 10px;
            scroll-snap-type: x mandatory;
            -ms-overflow-style: none; /* IE and Edge */
            scrollbar-width: none; /* Firefox */
        }
        .bug-slider::-webkit-scrollbar { display: none; /* Safari and Chrome */ }
        
        .bug-card {
            flex: 0 0 130px; /* Fixed width for card to slide horizontally */
            scroll-snap-align: center;
            padding: 15px 10px; text-align: center; font-size: 11px; font-weight: bold;
            border-radius: 8px; cursor: pointer; position: relative;
            background: linear-gradient(135deg, rgba(255,255,255,0.02) 0%, rgba(255,255,255,0.05) 100%);
            border: 1px solid var(--glass-border); transition: var(--transition-snappy);
            white-space: nowrap;
        }

        .tools-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
        .tool-card {
            padding: 20px 10px; display: flex; flex-direction: column; align-items: center;
            text-align: center; border-radius: 12px; cursor: pointer;
            border: 1px solid var(--glass-border); transition: var(--transition-snappy);
        }
        .tool-card svg { width: 30px; height: 30px; fill: white; margin-bottom: 10px; }
        .tool-card span { font-size: 11px; }
        .tool-card:active { transform: scale(0.95); }

        .profile-container { display: flex; flex-direction: column; gap: 15px; align-items: center; padding-top: 20px; }
        .profile-upload {
            width: 100px; height: 100px; border-radius: 50%; border: 1px dashed rgba(255,255,255,0.3);
            display: flex; justify-content: center; align-items: center; position: relative; overflow: hidden;
        }
        .profile-upload img { width: 100%; height: 100%; object-fit: cover; display: none; }
        .profile-upload input[type="file"] { position: absolute; width: 100%; height: 100%; opacity: 0; cursor: pointer; }

        /* =========================================
           9. MODALS & ALERTS & 3D SHATTER
        ========================================= */
        .modal-overlay {
            position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
            background: rgba(0,0,0,0.8); backdrop-filter: blur(5px);
            display: flex; justify-content: center; align-items: center;
            opacity: 0; pointer-events: none; transition: var(--transition-snappy); z-index: 200;
        }
        .modal-overlay.active { opacity: 1; pointer-events: auto; }
        .modal-box {
            width: 90%; max-width: 320px; padding: 20px; display: flex; flex-direction: column; gap: 15px;
            transform: translateY(20px) scale(0.95); transition: var(--transition-snappy);
        }
        .modal-overlay.active .modal-box { transform: translateY(0) scale(1); }
        .modal-close { position: absolute; top: 10px; right: 15px; cursor: pointer; font-size: 20px; }

        .toast {
            position: fixed; top: -50px; left: 50%; transform: translateX(-50%);
            background: rgba(255,255,255,0.1); backdrop-filter: blur(10px);
            border: 1px solid rgba(255,255,255,0.2); padding: 12px 24px; border-radius: 30px;
            font-size: 12px; z-index: 999; transition: var(--transition-smooth);
        }
        .toast.show { top: 40px; }

        #shatter-container {
            position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
            pointer-events: none; z-index: 1000; overflow: hidden;
        }
        .shard {
            position: absolute; background: var(--glass-bg); backdrop-filter: var(--glass-blur);
            border: 1px solid rgba(255,255,255,0.2);
            transition: transform 1.2s cubic-bezier(0.19, 1, 0.22, 1), opacity 1s ease-out;
        }
        
        .x-error {
            position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%);
            font-size: 150px; font-weight: bold; color: var(--danger); font-family: sans-serif;
            z-index: 900; pointer-events: none; opacity: 0; text-shadow: 0 0 20px rgba(255,51,102,0.5);
        }
    </style>
</head>
<body>

    <div id="shatter-container"></div>
    <div id="x-error" class="x-error">X</div>
    <div id="toast" class="toast">Notifikasi</div>

    <!-- PAGE 1: ENTRY SCREEN (FIXED) -->
    <div id="page-entry" class="page active">
        <div class="team-avatars">
            <span class="gow-text">TQ TO TEAM</span>
            <div class="avatar-group">
                <img src="Sf.jpg" alt="Team 1" onerror="this.src='data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI1MCIgaGVpZ2h0PSI1MCI+PHJlY3Qgd2lkdGg9IjUwIiBoZWlnaHQ9IjUwIiBmaWxsPSIjMzMzIi8+PC9zdmc+'">
                <img src="gh.jpg" alt="Team 2" onerror="this.src='data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI1MCIgaGVpZ2h0PSI1MCI+PHJlY3Qgd2lkdGg9IjUwIiBoZWlnaHQ9IjUwIiBmaWxsPSIjNDQ0Ii8+PC9zdmc+'">
                <img src="hg.jpg" alt="Team 3" onerror="this.src='data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI1MCIgaGVpZ2h0PSI1MCI+PHJlY3Qgd2lkdGg9IjUwIiBoZWlnaHQ9IjUwIiBmaWxsPSIjNTU1Ii8+PC9zdmc+'">
            </div>
        </div>

        <div class="glass-panel center-viewbox" id="entry-viewbox">
            <video src="1video.mp4" autoplay loop muted playsinline onerror="this.style.display='none'"></video>
            <div class="floating-title gow-text">SXVS</div>
        </div>

        <button id="btn-enter" class="gow-text glass-panel" style="width: 250px; font-size: 20px;" onclick="triggerEntryShatter()">PRESS TO ENTER</button>

        <div class="social-footer">
            <div class="social-icons">
                <a href="https://tiktok.com/@kyz.n.e.m.e.s.i.s" target="_blank">
                    <svg viewBox="0 0 24 24"><path d="M19.59 6.69a4.83 4.83 0 0 1-3.77-4.25V2h-3.45v13.67a2.89 2.89 0 0 1-5.2 1.74 2.89 2.89 0 0 1 2.31-4.64 2.93 2.93 0 0 1 .88.13V9.4a6.84 6.84 0 0 0-1-.05A6.33 6.33 0 0 0 5 20.1a6.34 6.34 0 0 0 10.86-4.43v-7a8.16 8.16 0 0 0 4.77 1.52v-3.4a4.85 4.85 0 0 1-1-.1z"/></svg>
                </a>
                <a href="https://t.me/kyzz" target="_blank">
                    <svg viewBox="0 0 24 24"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm4.64 6.8c-.15 1.58-.8 5.42-1.13 7.19-.14.75-.42 1-.68 1.03-.58.05-1.02-.38-1.58-.75-.88-.58-1.38-.94-2.23-1.5-.99-.65-.35-1.01.22-1.59.15-.15 2.71-2.48 2.76-2.69.01-.03.01-.14-.07-.19-.08-.05-.19-.02-.27.01-.12.04-1.96 1.25-5.54 3.69-.52.36-1 .53-1.42.52-.47-.01-1.37-.26-2.03-.48-.82-.27-1.47-.42-1.42-.88.03-.24.29-.48.79-.74 3.08-1.34 5.15-2.23 6.19-2.66 2.95-1.23 3.56-1.44 3.97-1.45.09 0 .28.02.41.11.11.08.15.19.16.27-.01.04-.01.12-.02.16z"/></svg>
                </a>
                <a href="https://wa.me/6283184214499" target="_blank">
                    <svg viewBox="0 0 24 24"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51a12.8 12.8 0 0 0-.57-.01c-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 0 1-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 0 1-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 0 1 2.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0 0 12.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 0 0 5.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 0 0-3.48-8.413Z"/></svg>
                </a>
            </div>
            <span style="font-size: 10px; opacity: 0.5; letter-spacing: 2px;">XSVS</span>
        </div>
    </div>

    <!-- PAGE 2: LOGIN SCREEN -->
    <div id="page-login" class="page">
        <video class="bg-video" src="webview.mp4" autoplay loop muted playsinline></video>
        <div class="glass-panel login-box">
            <img src="icon.png" alt="XSVS Icon" onerror="this.src='data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI4MCIgaGVpZ2h0PSI4MCI+PHJlY3Qgd2lkdGg9IjgwIiBoZWlnaHQ9IjgwIiBmaWxsPSIjMjIyIi8+PC9zdmc+'">
            <p>Silahkan masukan usn dan pw untuk login</p>
            <input type="text" id="login-usn" placeholder="Username">
            <input type="password" id="login-pw" placeholder="Password">
            <button class="gow-text" style="width: 100%;" onclick="attemptLogin()">LOGIN</button>
        </div>
    </div>

    <!-- PAGE 3: LOADING SCREEN -->
    <div id="page-loading" class="page">
        <video class="bg-video" src="2video.mp4" autoplay loop muted playsinline id="loading-video"></video>
        <div style="position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); text-align: center;">
            <div class="gow-text" style="font-size: 24px; margin-bottom: 15px; letter-spacing: 5px;">INITIALIZING</div>
            <div class="loading-container glass-panel">
                <div class="loading-bar" id="loading-bar"></div>
            </div>
        </div>
    </div>

    <!-- MAIN APP & TABS (FIXED BG DASHBOARD) -->
    <div id="page-main" class="page">
        
        <!-- DASHBOARD VIDEO BACKGROUND -->
        <video class="bg-video" src="bgvideo.mp4" autoplay loop muted playsinline style="opacity: 0.4;"></video>

        <div class="dashboard-layout">
            <!-- HEADER -->
            <div class="header">
                <div class="gow-text" style="font-size: 24px;">XSVS</div>
                <img src="" alt="User" class="user-pic" id="header-user-pic" onerror="this.src='data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PGNpcmNsZSBjeD0iMTIiIGN5PSI4IiByPSI0IiBmaWxsPSIjZmZmIi8+PHBhdGggZD0iTTEyIDE0Yy02LjEgMC04IDQtOCA0djJoMTZ2LTJzLTEuOS00LTgtNHoiIGZpbGw9IiNmZmYiLz48L3N2Zz4='">
            </div>

            <!-- TAB: DASHBOARD -->
            <div id="tab-dashboard" class="tab-content active">
                
                <div class="widget slider-widget glass-panel">
                    <div class="slider-track" id="anime-slider">
                        <img src="anime1.png" onerror="this.src='data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0MDAiIGhlaWdodD0iMjAwIj48cmVjdCB3aWR0aD0iNDAwIiBoZWlnaHQ9IjIwMCIgZmlsbD0iIzIyNCIvPjwvc3ZnPg=='">
                        <img src="anime2.png" onerror="this.src='data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0MDAiIGhlaWdodD0iMjAwIj48cmVjdCB3aWR0aD0iNDAwIiBoZWlnaHQ9IjIwMCIgZmlsbD0iIzQyMiIvPjwvc3ZnPg=='">
                        <img src="anime3.png" onerror="this.src='data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0MDAiIGhlaWdodD0iMjAwIj48cmVjdCB3aWR0aD0iNDAwIiBoZWlnaHQ9IjIwMCIgZmlsbD0iIzI0MiIvPjwvc3ZnPg=='">
                    </div>
                    <div class="slider-text zionix-text">XSVS APPS DENGAN TAMPILAN UI SUPER MODERN DAN FITUR YANG TERSEDIA BANYAK!</div>
                </div>

                <div class="widget clock-widget glass-panel" id="digital-clock">00:00:00</div>

                <div class="widget slider-widget glass-panel">
                    <div class="slider-track" id="ad-slider" style="width: 500%;">
                        <img src="1.png" onerror="this.src='data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0MDAiIGhlaWdodD0iMjAwIj48cmVjdCB3aWR0aD0iNDAwIiBoZWlnaHQ9IjIwMCIgZmlsbD0iIzMzMyIvPjx0ZXh0IHg9IjUwJSIgeT0iNTAlIiBmaWxsPSJ3aGl0ZSIgZG9taW5hbnQtYmFzZWxpbmU9Im1pZGRsZSIgdGV4dC1hbmNob3I9Im1pZGRsZSI+QWQgMTwvdGV4dD48L3N2Zz4='">
                        <img src="2.png" onerror="this.src='data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0MDAiIGhlaWdodD0iMjAwIj48cmVjdCB3aWR0aD0iNDAwIiBoZWlnaHQ9IjIwMCIgZmlsbD0iIzQ0NCIvPjx0ZXh0IHg9IjUwJSIgeT0iNTAlIiBmaWxsPSJ3aGl0ZSIgZG9taW5hbnQtYmFzZWxpbmU9Im1pZGRsZSIgdGV4dC1hbmNob3I9Im1pZGRsZSI+QWQgMjwvdGV4dD48L3N2Zz4='">
                        <img src="3.png" onerror="this.src='data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0MDAiIGhlaWdodD0iMjAwIj48cmVjdCB3aWR0aD0iNDAwIiBoZWlnaHQ9IjIwMCIgZmlsbD0iIzU1NSIvPjx0ZXh0IHg9IjUwJSIgeT0iNTAlIiBmaWxsPSJ3aGl0ZSIgZG9taW5hbnQtYmFzZWxpbmU9Im1pZGRsZSIgdGV4dC1hbmNob3I9Im1pZGRsZSI+QWQgMzwvdGV4dD48L3N2Zz4='">
                        <img src="4.png" onerror="this.src='data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0MDAiIGhlaWdodD0iMjAwIj48cmVjdCB3aWR0aD0iNDAwIiBoZWlnaHQ9IjIwMCIgZmlsbD0iIzY2NiIvPjx0ZXh0IHg9IjUwJSIgeT0iNTAlIiBmaWxsPSJ3aGl0ZSIgZG9taW5hbnQtYmFzZWxpbmU9Im1pZGRsZSIgdGV4dC1hbmNob3I9Im1pZGRsZSI+QWQgNDwvdGV4dD48L3N2Zz4='">
                        <img src="5.png" onerror="this.src='data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0MDAiIGhlaWdodD0iMjAwIj48cmVjdCB3aWR0aD0iNDAwIiBoZWlnaHQ9IjIwMCIgZmlsbD0iIzc3NyIvPjx0ZXh0IHg9IjUwJSIgeT0iNTAlIiBmaWxsPSJ3aGl0ZSIgZG9taW5hbnQtYmFzZWxpbmU9Im1pZGRsZSIgdGV4dC1hbmNob3I9Im1pZGRsZSI+QWQgNTwvdGV4dD48L3N2Zz4='">
                    </div>
                </div>

                <div class="widget marquee-widget glass-panel">
                    <div class="marquee-content gow-text">SELAMAT DATANG DI XSVS - SYSTEM ONLINE - ALL SYSTEMS NOMINAL - MAINTAINING CONNECTION...</div>
                </div>

            </div>

            <!-- TAB: BUG WHATSAPP (FIXED HORIZONTAL SLIDER) -->
            <div id="tab-bug" class="tab-content">
                <div class="page-header-video glass-panel">
                    <video src="fvideo.mp4" autoplay loop muted playsinline></video>
                    <h2 class="zionix-text">XSVS</h2>
                </div>

                <div class="widget">
                    <input type="tel" id="target-number" placeholder="Target Number (ex: +628...)">
                </div>

                <div class="sender-options">
                    <div class="sender-card glass-panel" onclick="selectSender(this, 'privat')">
                        SENDER PRIVAT<div class="tick-icon"></div>
                    </div>
                    <div class="sender-card glass-panel" onclick="selectSender(this, 'public')">
                        SENDER PUBLIC<div class="tick-icon"></div>
                    </div>
                </div>

                <!-- MENU SLIDER HORIZONTAL -->
                <div class="bug-slider" id="bug-options">
                    <div class="bug-card" onclick="selectBug(this, 'DELAY HARD')">DELAY HARD<div class="tick-icon"></div></div>
                    <div class="bug-card" onclick="selectBug(this, 'BLANK OS')">BLANK OS<div class="tick-icon"></div></div>
                    <div class="bug-card" onclick="selectBug(this, 'BLANK ANDRO')">BLANK ANDRO<div class="tick-icon"></div></div>
                    <div class="bug-card" onclick="selectBug(this, 'CRASH BULDOZER')">CRASH BULDOZER<div class="tick-icon"></div></div>
                    <div class="bug-card" onclick="selectBug(this, 'FC BLANK')">FC BLANK<div class="tick-icon"></div></div>
                    <div class="bug-card" onclick="selectBug(this, 'CRASH ANDRO')">CRASH ANDRO<div class="tick-icon"></div></div>
                    <div class="bug-card" onclick="selectBug(this, 'DELAY MS')">DELAY MS<div class="tick-icon"></div></div>
                </div>

                <button class="gow-text" style="width: 100%;" onclick="executeBug()">EXECUTE</button>
            </div>

            <!-- TAB: TOOLS -->
            <div id="tab-tools" class="tab-content">
                <h3 class="gow-text" style="margin-bottom: 20px; text-align: center;">TOOLS MODULE</h3>
                <div class="tools-grid">
                    <div class="tool-card glass-panel" onclick="openToolModal('Spam NGL', 'Username NGL')">
                        <svg viewBox="0 0 24 24"><path d="M20 4H4c-1.1 0-1.99.9-1.99 2L2 18c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zm0 4l-8 5-8-5V6l8 5 8-5v2z"/></svg>
                        <span class="zionix-text">Spam NGL</span>
                    </div>
                    <div class="tool-card glass-panel" onclick="openToolModal('Search OSINT', 'Query / IP')">
                        <svg viewBox="0 0 24 24"><path d="M15.5 14h-.79l-.28-.27A6.471 6.471 0 0 0 16 9.5 6.5 6.5 0 1 0 9.5 16c1.61 0 3.09-.59 4.23-1.57l.27.28v.79l5 4.99L20.49 19l-4.99-5zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5 14 7.01 14 9.5 11.99 14 9.5 14z"/></svg>
                        <span class="zionix-text">Search OSINT</span>
                    </div>
                    <div class="tool-card glass-panel" onclick="openToolModal('NIK to Phone', 'Input NIK')">
                        <svg viewBox="0 0 24 24"><path d="M14 2H6c-1.1 0-1.99.9-1.99 2L4 20c0 1.1.89 2 1.99 2H18c1.1 0 2-.9 2-2V8l-6-6zm2 16H8v-2h8v2zm0-4H8v-2h8v2zm-3-5V3.5L18.5 9H13z"/></svg>
                        <span class="zionix-text">NIK to Phone</span>
                    </div>
                    <div class="tool-card glass-panel" onclick="openToolModal('Phone to NIK', 'Input Phone')">
                        <svg viewBox="0 0 24 24"><path d="M20.01 15.38c-1.23 0-2.42-.2-3.53-.56a.977.977 0 0 0-1.01.24l-1.57 1.97c-2.83-1.35-5.48-3.9-6.89-6.83l1.95-1.66c.27-.28.35-.67.24-1.02-.37-1.11-.56-2.3-.56-3.53 0-.54-.45-.99-.99-.99H4.19C3.65 3 3 3.24 3 3.99 3 13.28 10.73 21 20.01 21c.71 0 .99-.63.99-1.18v-3.45c0-.54-.45-.99-.99-.99z"/></svg>
                        <span class="zionix-text">Phone to NIK</span>
                    </div>
                    <div class="tool-card glass-panel" onclick="openToolModal('Downloader Video', 'Video URL')">
                        <svg viewBox="0 0 24 24"><path d="M19 9h-4V3H9v6H5l7 7 7-7zM5 18v2h14v-2H5z"/></svg>
                        <span class="zionix-text">Downloader</span>
                    </div>
                    <div class="tool-card glass-panel" onclick="openToolModal('Image to URL', 'Select Image')">
                        <svg viewBox="0 0 24 24"><path d="M19.35 10.04C18.67 6.59 15.64 4 12 4 9.11 4 6.6 5.64 5.35 8.04 2.34 8.36 0 10.91 0 14c0 3.31 2.69 6 6 6h13c2.76 0 5-2.24 5-5 0-2.64-2.05-4.78-4.65-4.96zM14 13v4h-4v-4H7l5-5 5 5h-3z"/></svg>
                        <span class="zionix-text">Img to URL</span>
                    </div>
                    <div class="tool-card glass-panel" style="grid-column: span 2;" onclick="openToolModal('BLE Spam', 'Target Device Mac')">
                        <svg viewBox="0 0 24 24"><path d="M17.71 7.71L12 2h-1v7.59L6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 11 14.41V22h1l5.71-5.71-4.3-4.29 4.3-4.29zM13 5.83l1.88 1.88L13 9.59V5.83zm1.88 10.46L13 18.17v-3.76l1.88 1.88z"/></svg>
                        <span class="zionix-text">BLE Spam</span>
                    </div>
                </div>
            </div>

            <!-- TAB: PROFILE -->
            <div id="tab-profile" class="tab-content">
                <div class="profile-container glass-panel" style="padding-bottom: 20px;">
                    <div class="profile-upload">
                        <input type="file" id="profile-input" accept="image/*" onchange="previewProfile(this)">
                        <span style="font-size: 10px; color: var(--text-secondary); pointer-events: none; text-align: center; padding: 10px;">TAP TO UPLOAD</span>
                        <img id="profile-preview" src="">
                    </div>
                    <div style="width: 100%; padding: 0 20px;">
                        <input type="text" id="prof-name" placeholder="Name" style="margin-bottom: 10px;">
                        <input type="text" id="prof-id" placeholder="User ID" style="margin-bottom: 20px;">
                        <button class="gow-text" style="width: 100%;" onclick="saveProfile()">SAVE PROFILE</button>
                    </div>
                </div>
            </div>

            <!-- BOTTOM NAV -->
            <nav class="bottom-nav glass-panel">
                <div class="nav-item active" onclick="switchTab('dashboard', this)">
                    <svg viewBox="0 0 24 24"><path d="M3 13h8V3H3v10zm0 8h8v-6H3v6zm10 0h8V11h-8v10zm0-18v6h8V3h-8z"/></svg>
                    <span>DASHBOARD</span>
                </div>
                <div class="nav-item" onclick="switchTab('bug', this)">
                    <svg viewBox="0 0 24 24"><path d="M20 8h-2.81c-.45-.78-1.07-1.45-1.82-1.96L17 4.41 15.59 3l-2.17 2.17C12.96 5.06 12.49 5 12 5c-.49 0-.96.06-1.41.17L8.41 3 7 4.41l1.62 1.63C7.88 6.55 7.26 7.22 6.81 8H4v2h2.09c-.05.33-.09.66-.09 1v1H4v2h2v1c0 .34.04.67.09 1H4v2h2.81c1.04 1.79 2.97 3 5.19 3s4.15-1.21 5.19-3H20v-2h-2.09c.05-.33.09-.66.09-1v-1h2v-2h-2v-1c0-.34-.04-.67-.09-1H20V8zm-6 8h-4v-2h4v2zm0-4h-4v-2h4v2z"/></svg>
                    <span>BUG WA</span>
                </div>
                <div class="nav-item" onclick="switchTab('tools', this)">
                    <svg viewBox="0 0 24 24"><path d="M22.7 19l-9.1-9.1c.9-2.3.4-5-1.5-6.9-2-2-5-2.4-7.4-1.3L9 6 6 9 1.6 4.7C.5 7.1.9 10.1 2.9 12.1c1.9 1.9 4.6 2.4 6.9 1.5l9.1 9.1c.4.4 1 .4 1.4 0l2.3-2.3c.5-.4.5-1.1.1-1.4z"/></svg>
                    <span>TOOLS</span>
                </div>
                <div class="nav-item" onclick="switchTab('profile', this)">
                    <svg viewBox="0 0 24 24"><path d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z"/></svg>
                    <span>PROFILE</span>
                </div>
            </nav>

            <div class="fab" onclick="document.getElementById('modal-link').classList.add('active')">
                <svg viewBox="0 0 24 24" width="24" height="24" fill="white"><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"/></svg>
            </div>
        </div>
    </div>

    <!-- MODAL: LINK DEVICE -->
    <div id="modal-link" class="modal-overlay">
        <div class="modal-box glass-panel">
            <div class="modal-close" onclick="this.parentElement.parentElement.classList.remove('active')">&times;</div>
            <h3 class="gow-text" style="font-size: 16px; text-align: center;">TAUTKAN PERANGKAT</h3>
            <input type="tel" id="link-phone" placeholder="No. WA (+62...)">
            <button class="gow-text" onclick="showToast('Menghubungkan ke Perangkat...', true); setTimeout(()=>{document.getElementById('modal-link').classList.remove('active'); showToast('Perangkat Ditautkan!');}, 2000);">LINK DEVICE</button>
        </div>
    </div>

    <!-- MODAL: TOOLS -->
    <div id="modal-tools" class="modal-overlay">
        <div class="modal-box glass-panel">
            <div class="modal-close" onclick="this.parentElement.parentElement.classList.remove('active')">&times;</div>
            <h3 class="gow-text" id="tool-title" style="font-size: 16px; text-align: center;">TOOL</h3>
            <input type="text" id="tool-input" placeholder="Input">
            <button class="gow-text" onclick="executeTool()">EXECUTE</button>
        </div>
    </div>

    <!-- JAVASCRIPT -->
    <script>
        function showPage(pageId) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(pageId).classList.add('active');
        }

        function triggerEntryShatter() {
            const btn = document.getElementById('btn-enter');
            const viewbox = document.getElementById('entry-viewbox');
            
            createShards(viewbox.getBoundingClientRect(), 12);
            createShards(btn.getBoundingClientRect(), 6);
            
            document.getElementById('page-entry').style.opacity = '0';
            document.getElementById('page-entry').style.transform = 'scale(1.5)';
            
            setTimeout(() => {
                showPage('page-login');
                document.getElementById('page-entry').style.display = 'none';
                clearShards();
            }, 800);
        }

        function createShards(rect, count) {
            const container = document.getElementById('shatter-container');
            for (let i = 0; i < count; i++) {
                const shard = document.createElement('div');
                shard.className = 'shard';
                
                const w = Math.random() * 60 + 20;
                const h = Math.random() * 60 + 20;
                shard.style.width = w + 'px';
                shard.style.height = h + 'px';
                shard.style.top = (rect.top + rect.height/2 - h/2) + 'px';
                shard.style.left = (rect.left + rect.width/2 - w/2) + 'px';
                
                const points = [
                    `${Math.random()*100}% 0%`,
                    `100% ${Math.random()*100}%`,
                    `0% 100%`
                ];
                shard.style.clipPath = `polygon(${points.join(', ')})`;
                
                container.appendChild(shard);

                setTimeout(() => {
                    const tx = (Math.random() - 0.5) * window.innerWidth;
                    const ty = (Math.random() - 0.5) * window.innerHeight;
                    const rot = (Math.random() - 0.5) * 720;
                    shard.style.transform = `translate3d(${tx}px, ${ty}px, 200px) rotate(${rot}deg)`;
                    shard.style.opacity = '0';
                }, 50);
            }
        }
        function clearShards() { document.getElementById('shatter-container').innerHTML = ''; }

        function attemptLogin() {
            const usn = document.getElementById('login-usn').value;
            const pw = document.getElementById('login-pw').value;

            if(usn === 'code' && pw === '123') {
                showPage('page-loading');
                startLoadingSequence();
            } else {
                const xEl = document.getElementById('x-error');
                xEl.style.opacity = '1';
                xEl.style.transform = 'translate(-50%, -50%) scale(1.5)';
                
                setTimeout(() => {
                    xEl.style.opacity = '0';
                    xEl.style.transform = 'translate(-50%, -50%) scale(1)';
                    const rect = xEl.getBoundingClientRect();
                    createShards(rect, 15);
                    setTimeout(clearShards, 1000);
                }, 300);
            }
        }

        function startLoadingSequence() {
            const vid = document.getElementById('loading-video');
            const bar = document.getElementById('loading-bar');
            bar.style.width = '0%';
            
            let duration = vid.duration && !isNaN(vid.duration) ? vid.duration * 1000 : 3000;
            
            bar.style.transition = `width ${duration}ms linear`;
            
            setTimeout(() => { bar.style.width = '100%'; }, 50);

            setTimeout(() => {
                showPage('page-main');
                initDashboard();
            }, duration);
        }

        function initDashboard() {
            startClock();
            startSliders();
        }

        function startClock() {
            const clockEl = document.getElementById('digital-clock');
            setInterval(() => {
                const d = new Date();
                clockEl.innerText = d.toLocaleTimeString('id-ID', { hour12: false });
            }, 1000);
        }

        function startSliders() {
            let animeIndex = 0;
            let adIndex = 0;
            const animeTrack = document.getElementById('anime-slider');
            const adTrack = document.getElementById('ad-slider');

            setInterval(() => {
                animeIndex = (animeIndex + 1) % 3;
                animeTrack.style.transform = `translateX(-${animeIndex * 33.33}%)`;
            }, 3000);

            setInterval(() => {
                adIndex = (adIndex + 1) % 5;
                adTrack.style.transform = `translateX(-${adIndex * 20}%)`;
            }, 4000);
        }

        function switchTab(tabId, navItem) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            
            document.getElementById(`tab-${tabId}`).classList.add('active');
            navItem.classList.add('active');
        }

        let selectedSender = null;
        let selectedBug = null;

        function selectSender(el, type) {
            document.querySelectorAll('.sender-card').forEach(c => c.classList.remove('active'));
            el.classList.add('active');
            selectedSender = type;
        }

        function selectBug(el, type) {
            document.querySelectorAll('.bug-card').forEach(c => c.classList.remove('active'));
            el.classList.add('active');
            selectedBug = type;
        }

        function executeBug() {
            const target = document.getElementById('target-number').value;
            const regex = /^\+62\d{10,}$/;

            if(!regex.test(target)) {
                showToast("Target wajib awalan +62 & min 10 digit angka!");
                return;
            }
            if(!selectedSender || !selectedBug) {
                showToast("Silahkan pilih opsi dan sender!");
                return;
            }

            showToast("Memproses Bug " + selectedBug + "...", true);
            setTimeout(() => {
                showToast("✅ Bug Terkirim (Target: " + target + ")");
            }, 2500);
        }

        function openToolModal(title, placeholder) {
            document.getElementById('tool-title').innerText = title;
            const input = document.getElementById('tool-input');
            input.placeholder = placeholder;
            input.value = '';
            document.getElementById('modal-tools').classList.add('active');
        }

        function executeTool() {
            const title = document.getElementById('tool-title').innerText;
            const val = document.getElementById('tool-input').value;
            if(!val) { showToast("Input tidak boleh kosong!"); return; }
            
            showToast("Executing " + title + "...", true);
            setTimeout(() => {
                document.getElementById('modal-tools').classList.remove('active');
                showToast("Command Executed Successfully!");
            }, 1500);
        }

        function previewProfile(input) {
            if (input.files && input.files[0]) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    const preview = document.getElementById('profile-preview');
                    preview.src = e.target.result;
                    preview.style.display = 'block';
                    input.nextElementSibling.style.display = 'none'; 
                }
                reader.readAsDataURL(input.files[0]);
            }
        }

        function saveProfile() {
            const name = document.getElementById('prof-name').value;
            const img = document.getElementById('profile-preview').src;
            
            if(img && img !== window.location.href) {
                document.getElementById('header-user-pic').src = img;
            }
            showToast("Profile Saved!");
        }

        function showToast(msg, isPersistent = false) {
            const t = document.getElementById('toast');
            t.innerText = msg;
            t.classList.add('show');
            if(!isPersistent) {
                setTimeout(() => { t.classList.remove('show'); }, 3000);
            }
        }
    </script>
</body>
</html>
