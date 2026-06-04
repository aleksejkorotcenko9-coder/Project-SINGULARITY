<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>S.I.N.G.U.L.A.R.I.T.Y. — Audio Player</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&family=Space+Grotesk:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg: #ffffff;
            --text: #0a0a0a;
            --text-muted: #6b7280;
            --accent-blue: #0ea5e9;
            --accent-green: #10b981;
            --border: #e5e7eb;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }
        html { scroll-behavior: smooth; }

        @media (prefers-reduced-motion: reduce) {
            html { scroll-behavior: auto; }
            *, *::before, *::after {
                animation-duration: 0.01ms !important;
                transition-duration: 0.01ms !important;
            }
        }

        body {
            font-family: 'Inter', -apple-system, sans-serif;
            background: var(--bg);
            color: var(--text);
            line-height: 1.5;
            font-weight: 400;
            letter-spacing: -0.01em;
        }

        h1, h2, h3, h4 {
            font-family: 'Space Grotesk', sans-serif;
            font-weight: 600;
            letter-spacing: -0.03em;
        }

        a { color: inherit; text-decoration: none; }

        /* Buttons */
        .btn-primary {
            display: inline-flex;
            align-items: center;
            gap: 0.625rem;
            padding: 0.875rem 1.5rem;
            background: var(--text);
            color: var(--bg);
            font-size: 0.875rem;
            font-weight: 500;
            border: 1px solid var(--text);
            transition: all 0.2s ease;
        }
        .btn-primary:hover { background: transparent; color: var(--text); }

        .btn-contrast {
            display: inline-flex;
            align-items: center;
            gap: 0.625rem;
            padding: 0.875rem 1.5rem;
            background: transparent;
            color: var(--text);
            font-size: 0.875rem;
            font-weight: 500;
            border: 2px solid var(--text);
            transition: all 0.2s ease;
        }
        .btn-contrast:hover { background: var(--text); color: var(--bg); }

        .btn-web {
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
            color: var(--accent-blue);
            font-weight: 500;
            font-size: 0.875rem;
            position: relative;
        }
        .btn-web::after {
            content: '';
            position: absolute;
            bottom: -2px; left: 0;
            width: 0; height: 1px;
            background: var(--accent-blue);
            transition: width 0.3s ease;
        }
        .btn-web:hover::after { width: 100%; }

        .container { max-width: 1200px; margin: 0 auto; padding: 0 1.5rem; }

        /* Header */
        header {
            position: fixed;
            top: 0; left: 0; right: 0;
            z-index: 50;
            background: rgba(255, 255, 255, 0.9);
            backdrop-filter: blur(4px);
            border-bottom: 1px solid var(--border);
        }
        .header-inner { display: flex; justify-content: space-between; align-items: center; height: 64px; }
        .logo { font-family: 'Space Grotesk', sans-serif; font-weight: 600; font-size: 1rem; letter-spacing: 0.02em; }
        .nav-link { font-size: 0.875rem; color: var(--text-muted); transition: color 0.2s; }
        .nav-link:hover { color: var(--text); }

        /* Hero Section with Enhanced Shimmer */
        .hero {
            min-height: 100vh;
            display: flex;
            align-items: center;
            padding-top: 64px;
            position: relative;
            overflow: hidden;
        }

        .hero::before {
            content: '';
            position: absolute;
            inset: 0;
            background: linear-gradient(
                135deg,
                transparent 0%,
                rgba(14, 165, 233, 0.12) 25%,
                transparent 50%,
                rgba(16, 185, 129, 0.12) 75%,
                transparent 100%
            );
            background-size: 400% 400%;
            animation: shimmer 12s ease infinite;
            z-index: -1;
            pointer-events: none;
        }

        @keyframes shimmer {
            0% { background-position: 0% 0%; }
            50% { background-position: 100% 100%; }
            100% { background-position: 0% 0%; }
        }

        @media (prefers-reduced-motion: reduce) {
            .hero::before {
                animation: none;
                background: radial-gradient(circle at 70% 30%, rgba(14, 165, 233, 0.08) 0%, transparent 50%),
                            radial-gradient(circle at 20% 80%, rgba(16, 185, 129, 0.08) 0%, transparent 50%);
            }
        }

        .hero-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 4rem;
            align-items: center;
        }

        @media (min-width: 1024px) {
            .hero-grid {
                grid-template-columns: 1fr 1fr;
                gap: 2rem;
            }
        }

        .hero-content { max-width: 560px; }

        .hero-label {
            display: inline-block;
            font-size: 0.75rem;
            text-transform: uppercase;
            letter-spacing: 0.15em;
            color: var(--text-muted);
            margin-bottom: 1.5rem;
            border: 1px solid var(--border);
            padding: 0.25rem 0.75rem;
            border-radius: 2px;
        }

        .hero-title {
            font-size: clamp(2.5rem, 7vw, 4.5rem);
            line-height: 1.05;
            margin-bottom: 1.5rem;
            font-weight: 700;
        }

        .hero-subtitle {
            font-size: 1.125rem;
            color: var(--text-muted);
            margin-bottom: 2.5rem;
            font-weight: 300;
            max-width: 440px;
        }

        .accent-blue { color: var(--accent-blue); }

        /* Visualizer */
        .visualizer-container {
            position: relative;
            width: 100%;
            max-width: 480px;
            margin: 0 auto;
            aspect-ratio: 1;
        }

        #visualizer { width: 100%; height: 100%; display: block; }

        /* Sections */
        section { padding: 6rem 0; border-top: 1px solid var(--border); }
        .section-title { font-size: 2rem; margin-bottom: 3rem; }
        .features-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 3rem; }
        .feature-item h3 { font-size: 1rem; font-weight: 600; margin-bottom: 0.5rem; }
        .feature-item p { font-size: 0.9375rem; color: var(--text-muted); line-height: 1.6; }

        /* Download, Docs, Footer (Styles preserved) */
        .download-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 2rem; align-items: start; }
        @media (max-width: 640px) { .download-grid { grid-template-columns: 1fr; } }
        .info-list { list-style: none; }
        .info-list li { display: flex; justify-content: space-between; padding: 0.75rem 0; border-bottom: 1px solid var(--border); font-size: 0.875rem; }
        .info-list li span:last-child { color: var(--text-muted); }
        .doc-block { margin-bottom: 2rem; }
        .doc-block h3 { font-size: 1rem; font-weight: 600; margin-bottom: 1rem; cursor: pointer; display: flex; justify-content: space-between; align-items: center; }
        .doc-block h3 svg { transition: transform 0.2s; }
        .doc-block.open h3 svg { transform: rotate(180deg); }
        .doc-content { max-height: 0; overflow: hidden; transition: max-height 0.3s ease; }
        .doc-block.open .doc-content { max-height: 1000px; }
        .doc-content p, .doc-content ul { font-size: 0.9375rem; color: var(--text-muted); line-height: 1.7; }
        .doc-content ul { padding-left: 1.25rem; margin-top: 0.5rem; }
        .doc-content li { margin-bottom: 0.375rem; }
        footer { padding: 3rem 0; border-top: 1px solid var(--border); }
        .footer-inner { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 1.5rem; }
        .footer-links { display: flex; gap: 2rem; }
        .footer-link { font-size: 0.875rem; color: var(--text-muted); display: flex; align-items: center; gap: 0.5rem; transition: color 0.2s; }
        .footer-link:hover { color: var(--text); }

        /* Mobile */
        .mobile-toggle { display: none; background: none; border: none; cursor: pointer; padding: 0.5rem; }
        @media (max-width: 768px) {
            .desktop-nav { display: none; }
            .mobile-toggle { display: block; }
        }
        .mobile-menu { position: fixed; inset: 0; background: var(--bg); z-index: 100; padding: 1.5rem; display: none; flex-direction: column; }
        .mobile-menu.active { display: flex; }
        .mobile-menu-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 3rem; }
        .mobile-menu nav { display: flex; flex-direction: column; gap: 1.5rem; }
        .mobile-menu nav a { font-size: 1.5rem; font-family: 'Space Grotesk', sans-serif; font-weight: 500; }

        .fade-in { opacity: 0; transform: translateY(20px); transition: opacity 0.6s ease, transform 0.6s ease; }
        .fade-in.visible { opacity: 1; transform: translateY(0); }
    </style>
</head>
<body>

    <!-- Header -->
    <header>
        <div class="container header-inner">
            <a href="#" class="logo">SINGULARITY</a>
            <nav class="desktop-nav flex items-center gap-8">
                <a href="#features" class="nav-link">Возможности</a>
                <a href="#download" class="nav-link">Скачать</a>
                <a href="#docs" class="nav-link">Документация</a>
                <a href="#" class="btn-web" id="webBtnHeader">
                    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <circle cx="12" cy="12" r="10"/><line x1="2" y1="12" x2="22" y2="12"/>
                        <path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/>
                    </svg>
                    Web Version
                </a>
            </nav>
            <button class="mobile-toggle" onclick="toggleMenu()" aria-label="Menu">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <line x1="3" y1="6" x2="21" y2="6"/><line x1="3" y1="12" x2="21" y2="12"/><line x1="3" y1="18" x2="21" y2="18"/>
                </svg>
            </button>
        </div>
    </header>

    <!-- Mobile Menu -->
    <div class="mobile-menu" id="mobileMenu">
        <div class="mobile-menu-header">
            <span class="logo">SINGULARITY</span>
            <button onclick="toggleMenu()" aria-label="Close">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/>
                </svg>
            </button>
        </div>
        <nav>
            <a href="#features" onclick="toggleMenu()">Возможности</a>
            <a href="#download" onclick="toggleMenu()">Скачать</a>
            <a href="#docs" onclick="toggleMenu()">Документация</a>
            <a href="#" class="btn-web mt-4" style="font-size: 1rem;">Web Version</a>
        </nav>
    </div>

    <!-- Hero -->
    <section class="hero">
        <div class="container">
            <div class="hero-grid">
                <div class="hero-content fade-in">
                    <span class="hero-label">Audio Player for Windows</span>
                    <h1 class="hero-title">
                        S.I.N.G.U.L.A.R.I.T.Y.
                    </h1>
                    <p class="hero-subtitle">
                        Минималистичный плеер с круговым спектральным анализатором. 
                        Слушайте музыку, смотрите форму.
                    </p>
                    <div class="flex flex-wrap gap-3">
                        <a href="#download" class="btn-primary">
                            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/>
                                <polyline points="7 10 12 15 17 10"/><line x1="12" y1="15" x2="12" y2="3"/>
                            </svg>
                            Скачать EXE
                        </a>
                        <a href="#" class="btn-contrast" id="webBtnHero">
                            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                <circle cx="12" cy="12" r="10"/>
                                <line x1="2" y1="12" x2="22" y2="12"/>
                                <path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/>
                            </svg>
                            Открыть Web-версию
                        </a>
                    </div>
                </div>
                
                <div class="visualizer-container fade-in" style="transition-delay: 0.2s;">
                    <canvas id="visualizer"></canvas>
                </div>
            </div>
        </div>
    </section>

    <!-- Features -->
    <section id="features">
        <div class="container">
            <h2 class="section-title">О приложении</h2>
            
            <div class="features-grid">
                <div class="feature-item fade-in">
                    <h3>Круговой визуализатор</h3>
                    <p>Частотный анализ в реальном времени. Цвета плавно реагируют на энергию звука.</p>
                </div>
                <div class="feature-item fade-in" style="transition-delay: 0.1s;">
                    <h3>Обложки альбомов</h3>
                    <p>Извлечение метаданных и отображение обложки в центре визуализатора.</p>
                </div>
                <div class="feature-item fade-in" style="transition-delay: 0.2s;">
                    <h3>Управление плейлистами</h3>
                    <p>Создание, удаление, добавление папок. Автопереход между списками.</p>
                </div>
                <div class="feature-item fade-in" style="transition-delay: 0.3s;">
                    <h3>Нативный интерфейс</h3>
                    <p>Работает в собственном окне Windows. Светлая тема, никаких лишних элементов.</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Download -->
    <section id="download">
        <div class="container">
            <h2 class="section-title">Скачать</h2>
            
            <div class="download-grid">
                <div class="fade-in">
                    <div class="flex gap-3 mb-8">
                        <a href="#" class="btn-primary" id="downloadBtn">
                            <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
                                <path d="M0 3.449L9.75 2.1v9.451H0m10.949-9.602L24 0v11.4H10.949M0 12.6h9.75v9.451L0 20.699M10.949 12.6H24V24l-12.9-1.801"/>
                            </svg>
                            Windows EXE
                        </a>
                        <a href="#" class="btn-outline" id="githubBtn">
                            GitHub
                        </a>
                    </div>
                    <p class="text-sm text-gray-500 mb-4">Версия 0.8. Автор: А.А. Коротченко</p>
                    <a href="#" class="btn-web" id="webBtnDownload">Открыть Web-версию в браузере</a>
                </div>
                
                <div class="fade-in" style="transition-delay: 0.1s;">
                    <ul class="info-list">
                        <li><span>Платформа</span><span>Windows 10 / 11</span></li>
                        <li><span>Размер</span><span>~15 MB</span></li>
                        <li><span>Форматы</span><span>MP3, WAV, OGG</span></li>
                        <li><span>Зависимости</span><span>Нет</span></li>
                    </ul>
                </div>
            </div>
        </div>
    </section>

    <!-- Documentation -->
    <section id="docs">
        <div class="container">
            <h2 class="section-title">Документация</h2>
            
            <div class="fade-in">
                <div class="doc-block" onclick="toggleDoc(this)">
                    <h3>
                        Обзор
                        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="6 9 12 15 18 9"/></svg>
                    </h3>
                    <div class="doc-content">
                        <p>Project S.I.N.G.U.L.A.R.I.T.Y. — десктопный аудиоплеер на Python с использованием pywebview. Предлагает современный светлый интерфейс, управление плейлистами, интерактивный визуализатор и поддержку метаданных.</p>
                    </div>
                </div>

                <div class="doc-block" onclick="toggleDoc(this)">
                    <h3>
                        Интерфейс
                        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="6 9 12 15 18 9"/></svg>
                    </h3>
                    <div class="doc-content">
                        <ul>
                            <li><strong>Боковая панель:</strong> Наведите на левый край или нажмите «S» для раскрытия плейлистов.</li>
                            <li><strong>Визуализатор:</strong> Круговые полосы, реагирующие на звук. В центре — обложка или логотип.</li>
                            <li><strong>Нижняя панель:</strong> Управление воспроизведением, громкость, режимы shuffle/repeat.</li>
                        </ul>
                    </div>
                </div>

                <div class="doc-block" onclick="toggleDoc(this)">
                    <h3>
                        Управление
                        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="6 9 12 15 18 9"/></svg>
                    </h3>
                    <div class="doc-content">
                        <ul>
                            <li><strong>Добавление:</strong> Кнопка «+» → выбрать файлы или папку.</li>
                            <li><strong>Плейлисты:</strong> Создание (+), переименование и удаление через боковую панель.</li>
                            <li><strong>Режимы:</strong> Shuffle и Repeat взаимоисключающие. При завершении плейлиста — автопереход на следующий.</li>
                        </ul>
                    </div>
                </div>

                <div class="doc-block" onclick="toggleDoc(this)">
                    <h3>
                        Технологии
                        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="6 9 12 15 18 9"/></svg>
                    </h3>
                    <div class="doc-content">
                        <ul>
                            <li>Python 3.11.0 + pywebview (нативное окно)</li>
                            <li>HTML5 / CSS3 / JavaScript (интерфейс)</li>
                            <li>Web Audio API (анализ частот)</li>
                            <li>jsmediatags (обложки)</li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Footer -->
    <footer>
        <div class="container footer-inner">
            <span class="text-sm text-gray-500">SINGULARITY v0.8</span>
            <div class="footer-links">
                <a href="#" class="footer-link" id="footerVk">
                    <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M15.684 0H8.316C1.592 0 0 1.592 0 8.316v7.368C0 22.408 1.592 24 8.316 24h7.368C22.408 24 24 22.408 24 15.684V8.316C24 1.592 22.408 0 15.684 0zm3.692 17.123h-1.744c-.66 0-.862-.523-2.049-1.723-1.033-1.033-1.49-1.168-1.744-1.168-.356 0-.458.102-.458.593v1.575c0 .424-.135.678-1.253.678-1.846 0-3.896-1.118-5.335-3.202C4.624 10.857 4 8.484 4 8.06c0-.254.102-.491.593-.491h1.744c.44 0 .61.203.78.678.863 2.49 2.303 4.675 2.896 4.675.22 0 .322-.102.322-.66V9.721c-.068-1.186-.695-1.287-.695-1.71 0-.203.17-.407.44-.407h2.744c.373 0 .508.203.508.644v3.473c0 .372.17.508.271.508.22 0 .407-.136.813-.542 1.254-1.406 2.151-3.574 2.151-3.574.119-.254.322-.491.763-.491h1.744c.525 0 .644.27.525.644-.22 1.017-2.354 4.031-2.354 4.031-.186.305-.254.44 0 .78.186.254.796.779 1.203 1.253.745.847 1.32 1.558 1.473 2.049.17.474-.085.716-.576.716z"/></svg>
                    VK
                </a>
                <a href="#" class="footer-link" id="footerGithub">
                    <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/></svg>
                    GitHub
                </a>
            </div>
        </div>
    </footer>

    <script>
        // === Canvas Visualizer ===
        const canvas = document.getElementById('visualizer');
        const ctx = canvas.getContext('2d');
        let animationId = null;
        let time = 0;
        const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

        function resize() {
            const container = canvas.parentElement;
            const size = Math.min(container.offsetWidth, container.offsetHeight);
            const dpr = window.devicePixelRatio || 1;
            
            canvas.width = size * dpr;
            canvas.height = size * dpr;
            canvas.style.width = size + 'px';
            canvas.style.height = size + 'px';
            
            ctx.scale(dpr, dpr);
        }

        function draw() {
            const w = canvas.width / (window.devicePixelRatio || 1);
            const h = canvas.height / (window.devicePixelRatio || 1);
            const cx = w / 2;
            const cy = h / 2;
            const minDim = Math.min(w, h);
            
            ctx.clearRect(0, 0, w, h);
            
            const baseRadius = Math.max(10, minDim * 0.32);
            const maxBarHeight = Math.max(5, minDim * 0.13);
            const bars = 48;

            for (let i = 0; i < bars; i++) {
                const angle = (i / bars) * Math.PI * 2 - Math.PI / 2;
                
                const sim = Math.sin(time * 0.025 + i * 0.15) * 0.4 + 
                            Math.sin(time * 0.04 + i * 0.3) * 0.2 + 0.6;
                const barHeight = Math.max(2, sim * maxBarHeight);
                
                const r = Math.max(5, baseRadius);
                const outer = Math.max(r + 1, r + barHeight);
                
                const x1 = cx + Math.cos(angle) * r;
                const y1 = cy + Math.sin(angle) * r;
                const x2 = cx + Math.cos(angle) * outer;
                const y2 = cy + Math.sin(angle) * outer;

                ctx.beginPath();
                ctx.moveTo(x1, y1);
                ctx.lineTo(x2, y2);
                
                const hue = 190 + (i % 3) * 5; 
                ctx.strokeStyle = `hsl(${hue}, 80%, 55%)`;
                ctx.lineWidth = Math.max(1, (Math.PI * 2 * r / bars) * 0.7);
                ctx.lineCap = 'round';
                ctx.stroke();
            }
            
            ctx.beginPath();
            const centerRadius = Math.max(5, minDim * 0.14);
            ctx.arc(cx, cy, centerRadius, 0, Math.PI * 2);
            ctx.fillStyle = '#000';
            ctx.fill();

            ctx.fillStyle = '#fff';
            ctx.font = `bold ${Math.max(14, minDim * 0.1)}px Space Grotesk`;
            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';
            ctx.fillText('S', cx, cy);

            time++;
            if (!prefersReducedMotion) {
                animationId = requestAnimationFrame(draw);
            }
        }

        resize();
        window.addEventListener('resize', resize);
        
        if (!prefersReducedMotion) {
            draw();
        } else {
            for(let k=0; k<10; k++) { time++; }
            draw();
        }

        // === UI Interactions ===
        function toggleMenu() {
            document.getElementById('mobileMenu').classList.toggle('active');
        }

        function toggleDoc(el) {
            el.classList.toggle('open');
        }

        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('visible');
                }
            });
        }, { threshold: 0.1, rootMargin: '0px 0px -50px 0px' });

        document.querySelectorAll('.fade-in').forEach(el => observer.observe(el));
    </script>
</body>
</html>
