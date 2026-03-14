<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="theme-color" content="#0a0a0a">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Victor Records">
    <meta name="description" content="Sistema de Segurança Victor Records - Acesso Restrito">
    <meta name="mobile-web-app-capable" content="yes">
    
    <title>Victor Records | Sistema de Segurança</title>
    
    <!-- Ícones para PWA -->
    <link rel="manifest" href="data:application/json;base64,eyJuYW1lIjoiVmljdG9yIFJlY29yZHMiLCJzaG9ydF9uYW1lIjoiVklDVE9SIiwic3RhcnRfdXJsIjoiLiIsImRpc3BsYXkiOiJzdGFuZGFsb25lIiwiYmFja2dyb3VuZF9jb2xvciI6IiMwYTBhMGEiLCJ0aGVtZV9jb2xvciI6IiMwYTBhMGEiLCJpY29ucyI6W3sic3JjIjoiZGF0YTppbWFnZS9zdmcreG1sLCUzQ3N2ZyB4bWxucz0naHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnIHZpZXdCb3g9JzAgMCAxOTIgMTkyJyUzRSUzQ3JlY3QgZmlsbD0nIzBhMGEwYScgd2lkdGg9JzE5MicgaGVpZ2h0PScxOTInLzUzRTNDdGV4dCBmaWxsPScjMDBmZjQxJyBmb250LWZhbWlseT0nbW9ub3NwYWNlJyBmb250LXNpemU9JzgwJyB4PSc1MCUnIHk9JzUwJScgZG9taW5hbnQtYmFzZWxpbmU9J21pZGRsZScgdGV4dC1hbmNob3I9J21pZGRsZSclM0VWJTNDL3RleHQlM0UlM0Mvc3ZnJTNFIiwic2l6ZXMiOiIxOTJ4MTkyIiwidHlwZSI6ImltYWdlL3N2Zyt4bWwifV19">
    
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
            user-select: none;
            -webkit-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none;
        }

        :root {
            --primary: #00ff41;
            --danger: #ff0000;
            --dark: #0a0a0a;
            --darker: #050505;
            --glass: rgba(0, 255, 65, 0.05);
        }

        body {
            font-family: 'Courier New', monospace;
            background: var(--dark);
            color: var(--primary);
            min-height: 100vh;
            overflow-x: hidden;
            position: fixed;
            width: 100%;
            height: 100%;
            touch-action: manipulation;
        }

        /* Prevenir scroll bounce no iOS */
        .app-container {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            overflow-y: auto;
            overflow-x: hidden;
            -webkit-overflow-scrolling: touch;
        }

        /* Scanlines */
        .scanlines {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(
                transparent 50%, 
                rgba(0, 255, 65, 0.03) 50%
            );
            background-size: 100% 4px;
            pointer-events: none;
            z-index: 1000;
            animation: scanline 8s linear infinite;
        }

        @keyframes scanline {
            0% { transform: translateY(0); }
            100% { transform: translateY(10px); }
        }

        /* Glitch effect */
        .glitch {
            position: relative;
            animation: glitch 2s infinite;
        }

        @keyframes glitch {
            0%, 90%, 100% { transform: translate(0); }
            92% { transform: translate(-2px, 2px); }
            94% { transform: translate(2px, -2px); }
            96% { transform: translate(-2px, -2px); }
            98% { transform: translate(2px, 2px); }
        }

        /* Splash Screen */
        .splash-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--dark);
            z-index: 10000;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            transition: opacity 0.5s, visibility 0.5s;
        }

        .splash-screen.hidden {
            opacity: 0;
            visibility: hidden;
        }

        .splash-logo {
            font-size: 3rem;
            font-weight: bold;
            text-transform: uppercase;
            letter-spacing: 8px;
            text-shadow: 0 0 20px var(--primary);
            margin-bottom: 30px;
        }

        .splash-loader {
            width: 200px;
            height: 3px;
            background: rgba(0, 255, 65, 0.2);
            position: relative;
            overflow: hidden;
        }

        .splash-progress {
            height: 100%;
            background: var(--primary);
            width: 0%;
            animation: loadProgress 2s ease-out forwards;
            box-shadow: 0 0 10px var(--primary);
        }

        @keyframes loadProgress {
            0% { width: 0%; }
            100% { width: 100%; }
        }

        /* Terminal */
        .terminal {
            background: rgba(10, 10, 10, 0.98);
            border: 1px solid var(--primary);
            border-radius: 12px;
            padding: 25px;
            margin: 20px;
            box-shadow: 
                0 0 30px rgba(0, 255, 65, 0.2),
                inset 0 0 30px rgba(0, 255, 65, 0.05);
            position: relative;
            overflow: hidden;
            backdrop-filter: blur(10px);
        }

        .terminal::before {
            content: '';
            position: absolute;
            top: -2px;
            left: -2px;
            right: -2px;
            bottom: -2px;
            background: linear-gradient(45deg, var(--primary), transparent, var(--primary));
            z-index: -1;
            opacity: 0.3;
            animation: borderRotate 4s linear infinite;
        }

        @keyframes borderRotate {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* Header */
        .header {
            text-align: center;
            margin-bottom: 25px;
            border-bottom: 1px solid var(--primary);
            padding-bottom: 15px;
        }

        .logo {
            font-size: 1.8rem;
            font-weight: bold;
            text-transform: uppercase;
            letter-spacing: 4px;
            text-shadow: 0 0 10px var(--primary);
            margin-bottom: 5px;
        }

        .subtitle {
            font-size: 0.7rem;
            opacity: 0.7;
            letter-spacing: 2px;
        }

        /* Status bar */
        .status-bar {
            display: flex;
            justify-content: space-between;
            font-size: 0.65rem;
            margin-bottom: 15px;
            padding: 8px 10px;
            background: var(--glass);
            border: 1px solid rgba(0, 255, 65, 0.3);
            border-radius: 4px;
        }

        .status-indicator {
            display: flex;
            align-items: center;
            gap: 5px;
        }

        .status-dot {
            width: 6px;
            height: 6px;
            background: var(--danger);
            border-radius: 50%;
            animation: pulse 1s infinite;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; box-shadow: 0 0 5px var(--danger); }
            50% { opacity: 0.3; box-shadow: none; }
        }

        /* Tabs */
        .tabs {
            display: flex;
            margin-bottom: 20px;
            border-bottom: 1px solid rgba(0, 255, 65, 0.3);
        }

        .tab {
            flex: 1;
            padding: 12px;
            background: transparent;
            border: none;
            color: rgba(0, 255, 65, 0.5);
            font-family: 'Courier New', monospace;
            font-size: 0.8rem;
            text-transform: uppercase;
            letter-spacing: 1px;
            cursor: pointer;
            transition: all 0.3s;
            position: relative;
            -webkit-touch-callout: none;
        }

        .tab.active {
            color: var(--primary);
            background: var(--glass);
        }

        .tab.active::after {
            content: '';
            position: absolute;
            bottom: -1px;
            left: 0;
            width: 100%;
            height: 2px;
            background: var(--primary);
            box-shadow: 0 0 10px var(--primary);
        }

        /* Formulário */
        .form-container {
            display: none;
        }

        .form-container.active {
            display: block;
            animation: fadeIn 0.4s;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-label {
            display: block;
            font-size: 0.7rem;
            margin-bottom: 6px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .form-input {
            width: 100%;
            padding: 12px;
            background: rgba(0, 0, 0, 0.8);
            border: 1px solid var(--primary);
            color: var(--primary);
            font-family: 'Courier New', monospace;
            font-size: 1rem;
            outline: none;
            transition: all 0.3s;
            border-radius: 4px;
            -webkit-appearance: none;
        }

        .form-input:focus {
            box-shadow: 0 0 15px rgba(0, 255, 65, 0.4);
            background: var(--glass);
        }

        /* Botão */
        .btn {
            width: 100%;
            padding: 14px;
            background: transparent;
            border: 2px solid var(--primary);
            color: var(--primary);
            font-family: 'Courier New', monospace;
            font-size: 0.9rem;
            text-transform: uppercase;
            letter-spacing: 2px;
            cursor: pointer;
            position: relative;
            overflow: hidden;
            transition: all 0.3s;
            margin-top: 10px;
            border-radius: 4px;
            -webkit-touch-callout: none;
        }

        .btn:active {
            transform: scale(0.98);
        }

        .btn:hover, .btn.active {
            background: var(--primary);
            color: var(--dark);
            box-shadow: 0 0 20px rgba(0, 255, 65, 0.5);
        }

        /* Mensagens */
        .security-msg {
            margin-top: 15px;
            padding: 12px;
            border: 1px solid rgba(255, 0, 0, 0.5);
            background: rgba(255, 0, 0, 0.1);
            font-size: 0.7rem;
            text-align: center;
            color: #ff4444;
            border-radius: 4px;
        }

        .success-msg {
            margin-top: 15px;
            padding: 12px;
            border: 1px solid rgba(0, 255, 65, 0.5);
            background: var(--glass);
            font-size: 0.7rem;
            text-align: center;
            color: var(--primary);
            display: none;
            border-radius: 4px;
        }

        /* Footer */
        .footer {
            margin-top: 20px;
            text-align: center;
            font-size: 0.6rem;
            opacity: 0.5;
            border-top: 1px solid rgba(0, 255, 65, 0.3);
            padding-top: 12px;
        }

        /* Dashboard */
        .dashboard {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--dark);
            z-index: 100;
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
        }

        .dashboard-header {
            position: sticky;
            top: 0;
            background: rgba(10, 10, 10, 0.95);
            backdrop-filter: blur(10px);
            border-bottom: 2px solid var(--primary);
            padding: 15px 20px;
            z-index: 50;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .welcome-text {
            font-size: 1.1rem;
            text-shadow: 0 0 10px var(--primary);
        }

        .logout-btn {
            padding: 8px 15px;
            background: transparent;
            border: 1px solid var(--danger);
            color: var(--danger);
            font-family: 'Courier New', monospace;
            font-size: 0.7rem;
            cursor: pointer;
            border-radius: 4px;
        }

        .dashboard-content {
            padding: 20px;
            padding-bottom: 100px;
        }

        /* Info Cards */
        .info-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 25px;
        }

        .info-card {
            border: 1px solid var(--primary);
            padding: 15px;
            background: var(--glass);
            border-radius: 8px;
            text-align: center;
        }

        .info-card h3 {
            font-size: 0.65rem;
            opacity: 0.8;
            margin-bottom: 5px;
            text-transform: uppercase;
        }

        .info-card p {
            font-size: 1.1rem;
            font-weight: bold;
        }

        /* Vídeos */
        .section-title {
            font-size: 1rem;
            margin-bottom: 15px;
            border-left: 3px solid var(--primary);
            padding-left: 10px;
            text-transform: uppercase;
        }

        .videos-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 15px;
        }

        .video-card {
            border: 1px solid var(--primary);
            background: var(--glass);
            border-radius: 8px;
            overflow: hidden;
            position: relative;
        }

        .video-thumbnail {
            width: 100%;
            height: 180px;
            background: linear-gradient(135deg, var(--dark) 0%, #1a1a1a 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            cursor: pointer;
        }

        .video-thumbnail::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: repeating-linear-gradient(
                0deg,
                transparent,
                transparent 2px,
                rgba(0, 255, 65, 0.03) 2px,
                rgba(0, 255, 65, 0.03) 4px
            );
        }

        .play-icon {
            width: 50px;
            height: 50px;
            border: 2px solid var(--primary);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            color: var(--primary);
            transition: all 0.3s;
            z-index: 10;
        }

        .video-card:active .play-icon {
            background: var(--primary);
            color: var(--dark);
            transform: scale(0.9);
        }

        .video-info {
            padding: 15px;
        }

        .video-title {
            font-size: 0.9rem;
            margin-bottom: 5px;
            text-transform: uppercase;
        }

        .video-meta {
            font-size: 0.7rem;
            opacity: 0.6;
            display: flex;
            gap: 10px;
        }

        .video-badge {
            position: absolute;
            top: 10px;
            right: 10px;
            background: rgba(255, 0, 0, 0.8);
            color: #fff;
            padding: 3px 6px;
            font-size: 0.6rem;
            text-transform: uppercase;
            border-radius: 3px;
        }

        /* Player Modal */
        .video-player-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.98);
            z-index: 5000;
            flex-direction: column;
            justify-content: center;
        }

        .player-container {
            width: 100%;
            height: 100%;
            position: relative;
            background: #000;
        }

        .video-element {
            width: 100%;
            height: 100%;
            object-fit: contain;
        }

        .player-controls {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            background: linear-gradient(transparent, rgba(0,0,0,0.9));
            padding: 20px;
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .control-btn {
            background: transparent;
            border: 1px solid var(--primary);
            color: var(--primary);
            padding: 10px 15px;
            cursor: pointer;
            font-family: 'Courier New', monospace;
            font-size: 0.8rem;
            border-radius: 4px;
        }

        .progress-container {
            flex: 1;
            height: 4px;
            background: rgba(0, 255, 65, 0.2);
            cursor: pointer;
            border-radius: 2px;
        }

        .progress-fill {
            height: 100%;
            background: var(--primary);
            width: 0%;
            border-radius: 2px;
            box-shadow: 0 0 5px var(--primary);
        }

        .close-player {
            position: absolute;
            top: 20px;
            right: 20px;
            background: rgba(255, 0, 0, 0.2);
            border: 1px solid var(--danger);
            color: var(--danger);
            padding: 10px 15px;
            cursor: pointer;
            font-family: 'Courier New', monospace;
            z-index: 10;
            border-radius: 4px;
        }

        .watermark {
            position: absolute;
            top: 20px;
            left: 20px;
            font-size: 0.7rem;
            opacity: 0.5;
            color: var(--primary);
            text-transform: uppercase;
            pointer-events: none;
            z-index: 10;
        }

        /* Logs */
        .logs-container {
            margin-top: 25px;
            border: 1px solid var(--primary);
            padding: 15px;
            background: rgba(0, 0, 0, 0.5);
            border-radius: 8px;
        }

        .logs-content {
            font-size: 0.7rem;
            opacity: 0.8;
            line-height: 1.6;
            font-family: monospace;
            max-height: 150px;
            overflow-y: auto;
        }

        /* Modal */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.95);
            z-index: 6000;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .modal-content {
            background: var(--dark);
            border: 2px solid var(--danger);
            padding: 30px;
            text-align: center;
            max-width: 350px;
            width: 100%;
            border-radius: 8px;
            animation: shake 0.5s;
        }

        .modal-content.success {
            border-color: var(--primary);
            animation: fadeIn 0.3s;
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-10px); }
            75% { transform: translateX(10px); }
        }

        .modal h2 {
            color: var(--danger);
            margin-bottom: 15px;
            font-size: 1.2rem;
        }

        .modal.success h2 {
            color: var(--primary);
        }

        /* Bottom Nav (App style) */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: rgba(10, 10, 10, 0.98);
            border-top: 1px solid var(--primary);
            display: flex;
            justify-content: space-around;
            padding: 10px 0;
            z-index: 200;
            backdrop-filter: blur(10px);
        }

        .nav-item {
            flex: 1;
            text-align: center;
            padding: 8px;
            color: rgba(0, 255, 65, 0.5);
            font-size: 0.7rem;
            cursor: pointer;
            transition: all 0.3s;
            text-transform: uppercase;
        }

        .nav-item.active {
            color: var(--primary);
        }

        .nav-icon {
            font-size: 1.5rem;
            margin-bottom: 3px;
            display: block;
        }

        /* Seções do app */
        .app-section {
            display: none;
            animation: fadeIn 0.3s;
        }

        .app-section.active {
            display: block;
        }

        /* Biometria simulada */
        .biometric-btn {
            width: 80px;
            height: 80px;
            border: 2px solid var(--primary);
            border-radius: 50%;
            margin: 20px auto;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
            cursor: pointer;
            transition: all 0.3s;
            position: relative;
        }

        .biometric-btn:active {
            transform: scale(0.95);
            background: var(--glass);
        }

        .biometric-btn.scanning::after {
            content: '';
            position: absolute;
            top: -5px;
            left: -5px;
            right: -5px;
            bottom: -5px;
            border: 2px solid var(--primary);
            border-radius: 50%;
            animation: scan 1s infinite;
        }

        @keyframes scan {
            0% { transform: scale(1); opacity: 1; }
            100% { transform: scale(1.2); opacity: 0; }
        }

        /* Notificações */
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            background: var(--dark);
            border: 1px solid var(--primary);
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 0 20px rgba(0, 255, 65, 0.3);
            transform: translateX(400px);
            transition: transform 0.3s;
            z-index: 7000;
            max-width: 300px;
        }

        .notification.show {
            transform: translateX(0);
        }

        .notification.danger {
            border-color: var(--danger);
            box-shadow: 0 0 20px rgba(255, 0, 0, 0.3);
        }

        /* Offline indicator */
        .offline-banner {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            background: var(--danger);
            color: #fff;
            text-align: center;
            padding: 8px;
            font-size: 0.75rem;
            transform: translateY(-100%);
            transition: transform 0.3s;
            z-index: 8000;
        }

        .offline-banner.show {
            transform: translateY(0);
        }

        /* Responsive */
        @media (min-width: 768px) {
            .videos-grid {
                grid-template-columns: repeat(2, 1fr);
            }
            .info-grid {
                grid-template-columns: repeat(4, 1fr);
            }
        }

        /* Hide scrollbar */
        ::-webkit-scrollbar {
            width: 4px;
        }
        ::-webkit-scrollbar-track {
            background: var(--dark);
        }
        ::-webkit-scrollbar-thumb {
            background: var(--primary);
            border-radius: 2px;
        }
    </style>
</head>
<body oncontextmenu="return false;">
    <div class="scanlines"></div>
    
    <!-- Offline Banner -->
    <div class="offline-banner" id="offlineBanner">
        ⚠ MODO OFFLINE - Sincronização pendente
    </div>

    <!-- Splash Screen -->
    <div class="splash-screen" id="splashScreen">
        <div class="splash-logo glitch">V</div>
        <div class="splash-loader">
            <div class="splash-progress"></div>
        </div>
        <div style="margin-top: 15px; font-size: 0.8rem; opacity: 0.7;">Inicializando...</div>
    </div>

    <!-- Notificação -->
    <div class="notification" id="notification">
        <div style="font-weight: bold; margin-bottom: 5px;" id="notifTitle">ALERTA</div>
        <div style="font-size: 0.8rem;" id="notifText">Mensagem</div>
    </div>

    <!-- Player de Vídeo -->
    <div class="video-player-modal" id="videoPlayer">
        <div class="player-container">
            <div class="watermark">VICTOR RECORDS - CONFIDENCIAL</div>
            <button class="close-player" onclick="closeVideo()">✕ FECHAR</button>
            <video class="video-element" id="videoElement" playsinline webkit-playsinline controlsList="nodownload noplaybackrate" disablePictureInPicture>
                <source src="" type="video/mp4">
            </video>
            <div class="player-controls">
                <button class="control-btn" onclick="togglePlay()">⏵/⏸</button>
                <div class="progress-container" onclick="seekVideo(event)">
                    <div class="progress-fill" id="progressFill"></div>
                </div>
                <button class="control-btn" onclick="toggleMute()">🔊</button>
            </div>
        </div>
    </div>

    <!-- Modal -->
    <div class="modal" id="modal">
        <div class="modal-content" id="modalContent">
            <h2 id="modalTitle">⚠ ACESSO NEGADO</h2>
            <p id="modalText" style="font-size: 0.85rem; margin-bottom: 20px;">Tentativa de acesso não autorizado.</p>
            <button class="btn" onclick="closeModal()">RETORNAR</button>
        </div>
    </div>

    <!-- App Container -->
    <div class="app-container" id="appContainer">
        
        <!-- Tela de Login -->
        <div id="loginScreen">
            <div class="terminal" style="margin-top: 50px;">
                <div class="header">
                    <div class="logo glitch">Victor Records</div>
                    <div class="subtitle">Sistema de Segurança Mobile v5.0</div>
                </div>

                <div class="status-bar">
                    <div class="status-indicator">
                        <div class="status-dot"></div>
                        <span>SEGURANÇA: OFFLINE</span>
                    </div>
                    <div id="clock">00:00</div>
                </div>

                <!-- Biometria -->
                <div style="text-align: center; margin: 20px 0;">
                    <div style="font-size: 0.75rem; margin-bottom: 10px;">OU USE BIOMETRIA</div>
                    <div class="biometric-btn" id="bioBtn" onclick="scanBiometric()">
                        👆
                    </div>
                </div>

                <div class="tabs">
                    <button class="tab active" onclick="switchTab('login')">Acesso</button>
                    <button class="tab" onclick="switchTab('register')">Novo ID</button>
                </div>

                <!-- Login Form -->
                <div class="form-container active" id="loginForm">
                    <form onsubmit="return handleLogin(event);">
                        <div class="form-group">
                            <label class="form-label">Operador ID:</label>
                            <input type="text" class="form-input" id="loginUsername" placeholder="Digite ID..." autocomplete="off" required>
                        </div>
                        <div class="form-group">
                            <label class="form-label">Chave:</label>
                            <input type="password" class="form-input" id="loginPassword" placeholder="••••••" autocomplete="off" required>
                        </div>
                        <button type="submit" class="btn">[ AUTENTICAR ]</button>
                    </form>
                </div>

                <!-- Register Form -->
                <div class="form-container" id="registerForm">
                    <form onsubmit="return handleRegister(event);">
                        <div class="form-group">
                            <label class="form-label">Novo ID:</label>
                            <input type="text" class="form-input" id="regUsername" placeholder="Crie ID..." required minlength="3">
                        </div>
                        <div class="form-group">
                            <label class="form-label">Chave (6+):</label>
                            <input type="password" class="form-input" id="regPassword" placeholder="••••••" required minlength="6">
                        </div>
                        <div class="form-group">
                            <label class="form-label">Confirmar:</label>
                            <input type="password" class="form-input" id="regConfirmPassword" placeholder="••••••" required>
                        </div>
                        <button type="submit" class="btn">[ CRIAR ACESSO ]</button>
                    </form>
                    <div class="success-msg" id="registerSuccess">
                        ✓ Cadastro ativado!
                    </div>
                </div>

                <div class="security-msg">
                    ÁREA RESTRITA<br>Monitoramento Ativo 24/7
                </div>

                <div class="footer">
                    <div>© 2024 Victor Records</div>
                    <div style="margin-top: 5px;">AES-256 | Firewall | IDS</div>
                </div>
            </div>
        </div>

        <!-- Dashboard -->
        <div class="dashboard" id="dashboard">
            <div class="dashboard-header">
                <div>
                    <div class="welcome-text" id="welcomeText">OP: DESCONHECIDO</div>
                    <div style="font-size: 0.65rem; opacity: 0.6;" id="sessionInfo">--</div>
                </div>
                <button class="logout-btn" onclick="logout()">SAIR</button>
            </div>

            <div class="dashboard-content">
                
                <!-- Seção Início -->
                <div class="app-section active" id="sectionHome">
                    <div class="info-grid">
                        <div class="info-card">
                            <h3>Status</h3>
                            <p style="color: var(--primary);">● ONLINE</p>
                        </div>
                        <div class="info-card">
                            <h3>Arquivos</h3>
                            <p id="totalVideos">4</p>
                        </div>
                        <div class="info-card">
                            <h3>Acesso</h3>
                            <p>ALPHA</p>
                        </div>
                        <div class="info-card">
                            <h3>Cripto</h3>
                            <p>AES-256</p>
                        </div>
                    </div>

                    <div class="section-title">Arquivos de Vigilância</div>
                    <div style="font-size: 0.75rem; opacity: 0.6; margin-bottom: 15px;">
                        > Toque para reproduzir. Monitoramento ativo.
                    </div>

                    <div class="videos-grid" id="videosGrid">
                        <!-- Vídeos gerados via JS -->
                    </div>
                </div>

                <!-- Seção Logs -->
                <div class="app-section" id="sectionLogs">
                    <div class="section-title">Registro de Eventos</div>
                    <div class="logs-container">
                        <div class="logs-content" id="logsContent">
                            <div>> Sistema inicializado...</div>
                            <div>> Aguardando autenticação...</div>
                        </div>
                    </div>
                </div>

                <!-- Seção Perfil -->
                <div class="app-section" id="sectionProfile">
                    <div class="section-title">Perfil do Operador</div>
                    <div class="terminal" style="margin: 0;">
                        <div style="text-align: center; margin-bottom: 20px;">
                            <div style="width: 80px; height: 80px; border: 2px solid var(--primary); border-radius: 50%; margin: 0 auto 15px; display: flex; align-items: center; justify-content: center; font-size: 2rem;">
                                👤
                            </div>
                            <div style="font-size: 1.2rem;" id="profileName">--</div>
                            <div style="font-size: 0.75rem; opacity: 0.6;" id="profileLevel">Nível: Administrador</div>
                        </div>
                        
                        <div style="border-top: 1px solid var(--primary); padding-top: 15px;">
                            <div style="display: flex; justify-content: space-between; margin-bottom: 10px; font-size: 0.8rem;">
                                <span>ID Único:</span>
                                <span id="profileId">--</span>
                            </div>
                            <div style="display: flex; justify-content: space-between; margin-bottom: 10px; font-size: 0.8rem;">
                                <span>Último Acesso:</span>
                                <span id="profileLast">--</span>
                            </div>
                            <div style="display: flex; justify-content: space-between; font-size: 0.8rem;">
                                <span>Dispositivo:</span>
                                <span id="profileDevice">Mobile</span>
                            </div>
                        </div>

                        <button class="btn" style="margin-top: 20px;" onclick="clearCache()">
                            [ LIMPAR CACHE LOCAL ]
                        </button>
                    </div>
                </div>

            </div>

            <!-- Bottom Navigation -->
            <div class="bottom-nav">
                <div class="nav-item active" onclick="switchSection('home')">
                    <span class="nav-icon">📁</span>
                    <span>Arquivos</span>
                </div>
                <div class="nav-item" onclick="switchSection('logs')">
                    <span class="nav-icon">📋</span>
                    <span>Logs</span>
                </div>
                <div class="nav-item" onclick="switchSection('profile')">
                    <span class="nav-icon">👤</span>
                    <span>Perfil</span>
                </div>
            </div>
        </div>

    </div>

    <script>
        // Dados dos vídeos
        const videosData = [
            {
                id: 1,
                title: "OP. MIDNIGHT - SETOR 7",
                duration: "12:34",
                date: "14/03/24",
                classification: "ULTRA",
                url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4"
            },
            {
                id: 2,
                title: "PROTOCOLO SOMBRA",
                duration: "08:22",
                date: "13/03/24",
                classification: "SIGILO",
                url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ElephantsDream.mp4"
            },
            {
                id: 3,
                title: "VIGILÂNCIA NOITE",
                duration: "45:18",
                date: "12/03/24",
                classification: "RESTRITO",
                url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerBlazes.mp4"
            },
            {
                id: 4,
                title: "ENTREVISTA S-47",
                duration: "23:45",
                date: "11/03/24",
                classification: "ULTRA",
                url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/TearsOfSteel.mp4"
            }
        ];

        // Estado do app
        let currentUser = null;
        let currentVideo = null;
        let isBiometricScanning = false;

        // Inicialização
        document.addEventListener('DOMContentLoaded', () => {
            initApp();
        });

        function initApp() {
            // Splash screen
            setTimeout(() => {
                document.getElementById('splashScreen').classList.add('hidden');
            }, 2500);

            // Clock
            setInterval(updateClock, 1000);
            updateClock();

            // Verificar sessão
            const savedUser = localStorage.getItem('victorCurrentUser');
            if(savedUser) {
                currentUser = JSON.parse(savedUser);
                setTimeout(() => showDashboard(), 3000);
            }

            // Online/Offline detection
            window.addEventListener('online', () => {
                document.getElementById('offlineBanner').classList.remove('show');
                showNotification('Sistema', 'Conexão restaurada', false);
            });
            
            window.addEventListener('offline', () => {
                document.getElementById('offlineBanner').classList.add('show');
                showNotification('Alerta', 'Modo offline ativado', true);
            });

            // Proteções
            setupProtections();
        }

        function setupProtections() {
            document.addEventListener('contextmenu', e => e.preventDefault());
            document.addEventListener('selectstart', e => {
                if(e.target.tagName !== 'INPUT') e.preventDefault();
            });
            
            // Prevenir zoom
            document.addEventListener('gesturestart', e => e.preventDefault());
            document.addEventListener('gesturechange', e => e.preventDefault());
            document.addEventListener('gestureend', e => e.preventDefault());
        }

        function updateClock() {
            const now = new Date();
            document.getElementById('clock').textContent = 
                now.getHours().toString().padStart(2,'0') + ':' + 
                now.getMinutes().toString().padStart(2,'0');
        }

        // Biometria simulada
        function scanBiometric() {
            if(isBiometricScanning) return;
            
            const btn = document.getElementById('bioBtn');
            isBiometricScanning = true;
            btn.classList.add('scanning');
            
            showNotification('Biometria', 'Escaneando impressão digital...', false);
            
            setTimeout(() => {
                btn.classList.remove('scanning');
                isBiometricScanning = false;
                
                // Simular sucesso se tiver usuário salvo
                const users = JSON.parse(localStorage.getItem('victorUsers') || '[]');
                if(users.length > 0 && currentUser) {
                    showNotification('Sucesso', 'Biometria confirmada', false);
                    handleQuickLogin();
                } else {
                    showNotification('Erro', 'Biometria não cadastrada', true);
                }
            }, 2000);
        }

        function handleQuickLogin() {
            document.getElementById('loginScreen').style.display = 'none';
            showDashboard();
        }

        // Tabs
        function switchTab(tab) {
            document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
            document.querySelectorAll('.form-container').forEach(f => f.classList.remove('active'));
            
            if(tab === 'login') {
                document.querySelectorAll('.tab')[0].classList.add('active');
                document.getElementById('loginForm').classList.add('active');
            } else {
                document.querySelectorAll('.tab')[1].classList.add('active');
                document.getElementById('registerForm').classList.add('active');
            }
        }

        // Registro
        function handleRegister(e) {
            e.preventDefault();
            
            const username = document.getElementById('regUsername').value.trim();
            const password = document.getElementById('regPassword').value;
            const confirm = document.getElementById('regConfirmPassword').value;

            if(password !== confirm) {
                showModal('Erro', 'Chaves não coincidem', false);
                return false;
            }

            let users = JSON.parse(localStorage.getItem('victorUsers') || '[]');
            
            if(users.find(u => u.username === username)) {
                showModal('Erro', 'ID já existe', false);
                return false;
            }

            const newUser = {
                id: Date.now().toString(36),
                username,
                password: btoa(password),
                created: new Date().toISOString(),
                biometric: false
            };

            users.push(newUser);
            localStorage.setItem('victorUsers', JSON.stringify(users));

            document.getElementById('registerSuccess').style.display = 'block';
            addLog(`Novo operador cadastrado: ${username}`);
            
            setTimeout(() => {
                document.getElementById('registerSuccess').style.display = 'none';
                switchTab('login');
                document.getElementById('loginUsername').value = username;
            }, 1500);

            return false;
        }

        // Login
        function handleLogin(e) {
            e.preventDefault();
            
            const username = document.getElementById('loginUsername').value.trim();
            const password = document.getElementById('loginPassword').value;

            const users = JSON.parse(localStorage.getItem('victorUsers') || '[]');
            const user = users.find(u => u.username === username && u.password === btoa(password));

            if(user) {
                currentUser = user;
                user.lastLogin = new Date().toISOString();
                localStorage.setItem('victorUsers', JSON.stringify(users));
                localStorage.setItem('victorCurrentUser', JSON.stringify(user));
                
                document.getElementById('loginScreen').style.display = 'none';
                showDashboard();
                addLog(`Login: ${username}`);
            } else {
                showModal('Acesso Negado', 'Credenciais inválidas', false);
                addLog(`Tentativa falha: ${username}`);
            }

            return false;
        }

        // Dashboard
        function showDashboard() {
            document.getElementById('dashboard').style.display = 'block';
            
            document.getElementById('welcomeText').textContent = `OP: ${currentUser.username.toUpperCase()}`;
            document.getElementById('sessionInfo').textContent = new Date().toLocaleString('pt-BR');
            
            // Perfil
            document.getElementById('profileName').textContent = currentUser.username;
            document.getElementById('profileId').textContent = currentUser.id.substr(-6).toUpperCase();
            document.getElementById('profileLast').textContent = new Date(currentUser.lastLogin || currentUser.created).toLocaleString('pt-BR');
            
            renderVideos();
            showNotification('Bem-vindo', `Operador ${currentUser.username} autenticado`, false);
        }

        function renderVideos() {
            const grid = document.getElementById('videosGrid');
            grid.innerHTML = '';
            
            videosData.forEach((video, idx) => {
                const card = document.createElement('div');
                card.className = 'video-card';
                card.innerHTML = `
                    <div class="video-thumbnail" onclick="playVideo(${idx})">
                        <span class="video-badge">${video.classification}</span>
                        <div class="play-icon">▶</div>
                    </div>
                    <div class="video-info">
                        <div class="video-title">${video.title}</div>
                        <div class="video-meta">
                            <span>⏱ ${video.duration}</span>
                            <span>📅 ${video.date}</span>
                        </div>
                    </div>
                `;
                grid.appendChild(card);
            });
        }

        // Player
        function playVideo(idx) {
            const video = videosData[idx];
            currentVideo = video;
            
            const player = document.getElementById('videoPlayer');
            const videoEl = document.getElementById('videoElement');
            
            player.style.display = 'flex';
            videoEl.src = video.url;
            
            // Forçar fullscreen no mobile
            if(videoEl.requestFullscreen) videoEl.requestFullscreen();
            else if(videoEl.webkitRequestFullscreen) videoEl.webkitRequestFullscreen();
            
            videoEl.play();
            
            // Progresso
            videoEl.ontimeupdate = () => {
                const pct = (videoEl.currentTime / videoEl.duration) * 100;
                document.getElementById('progressFill').style.width = pct + '%';
            };
            
            addLog(`Reprodução: ${video.title}`);
        }

        function closeVideo() {
            const player = document.getElementById('videoPlayer');
            const videoEl = document.getElementById('videoElement');
            
            videoEl.pause();
            videoEl.src = '';
            player.style.display = 'none';
            
            if(currentVideo) {
                addLog(`Encerrado: ${currentVideo.title}`);
                currentVideo = null;
            }
        }

        function togglePlay() {
            const video = document.getElementById('videoElement');
            video.paused ? video.play() : video.pause();
        }

        function toggleMute() {
            const video = document.getElementById('videoElement');
            video.muted = !video.muted;
        }

        function seekVideo(e) {
            const video = document.getElementById('videoElement');
            const rect = e.currentTarget.getBoundingClientRect();
            const pct = (e.clientX - rect.left) / rect.width;
            video.currentTime = pct * video.duration;
        }

        // Navegação
        function switchSection(section) {
            document.querySelectorAll('.app-section').forEach(s => s.classList.remove('active'));
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            
            if(section === 'home') {
                document.getElementById('sectionHome').classList.add('active');
                document.querySelectorAll('.nav-item')[0].classList.add('active');
            } else if(section === 'logs') {
                document.getElementById('sectionLogs').classList.add('active');
                document.querySelectorAll('.nav-item')[1].classList.add('active');
            } else {
                document.getElementById('sectionProfile').classList.add('active');
                document.querySelectorAll('.nav-item')[2].classList.add('active');
            }
        }

        // Logout
        function logout() {
            localStorage.removeItem('victorCurrentUser');
            currentUser = null;
            
            document.getElementById('dashboard').style.display = 'none';
            document.getElementById('loginScreen').style.display = 'block';
            
            document.getElementById('loginUsername').value = '';
            document.getElementById('loginPassword').value = '';
            
            switchSection('home');
            addLog('Logout realizado');
        }

        // Logs
        function addLog(msg) {
            const logs = document.getElementById('logsContent');
            const time = new Date().toLocaleTimeString('pt-BR');
            const entry = document.createElement('div');
            entry.innerHTML = `> [${time}] ${msg}`;
            logs.insertBefore(entry, logs.firstChild);
            
            while(logs.children.length > 50) {
                logs.removeChild(logs.lastChild);
            }
        }

        function clearCache() {
            if(confirm('Limpar todos os dados locais?')) {
                localStorage.clear();
                showNotification('Sistema', 'Cache limpo. Reiniciando...', false);
                setTimeout(() => location.reload(), 1500);
            }
        }

        // Modal
        function showModal(title, text, isSuccess) {
            const modal = document.getElementById('modal');
            const content = document.getElementById('modalContent');
            
            document.getElementById('modalTitle').textContent = title;
            document.getElementById('modalText').textContent = text;
            
            if(isSuccess) content.classList.add('success');
            else content.classList.remove('success');
            
            modal.style.display = 'flex';
        }

        function closeModal() {
            document.getElementById('modal').style.display = 'none';
        }

        // Notificação
        function showNotification(title, text, isDanger) {
            const notif = document.getElementById('notification');
            
            document.getElementById('notifTitle').textContent = title;
            document.getElementById('notifText').textContent = text;
            
            if(isDanger) notif.classList.add('danger');
            else notif.classList.remove('danger');
            
            notif.classList.add('show');
            
            setTimeout(() => {
                notif.classList.remove('show');
            }, 3000);
        }

        // Service Worker para PWA
        if('serviceWorker' in navigator) {
            navigator.serviceWorker.register('data:text/javascript;base64,' + btoa(`
                self.addEventListener('install', e => self.skipWaiting());
                self.addEventListener('fetch', e => e.respondWith(fetch(e.request).catch(() => new Response('Offline'))));
            `)).catch(() => console.log('SW não registrado'));
        }
    </script>
</body>
</html>
