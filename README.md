!DOCTYPE html>
<html lang="es" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NexoLab | Ingeniería Comercial & Automatización</title>

    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        obsidian: '#050508',
                        neon: {
                            purple: '#b026ff',
                            cyan: '#00f2fe',
                            pink: '#ff0844'
                        }
                    },
                    fontFamily: {
                        display: ['Outfit', 'sans-serif'],
                        body: ['Inter', 'sans-serif'],
                        mono: ['JetBrains Mono', 'monospace']
                    }
                }
            }
        }
    </script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&family=JetBrains+Mono:wght@400;700&family=Outfit:wght@400;600;800;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

    <style>
        :root {
            --cursor-x: 50vw;
            --cursor-y: 50vh;
        }

        body {
            margin: 0;
            background-color: #050508;
            color: #ffffff;
            overflow-x: hidden;
            cursor: none; /* Hide default cursor for custom one */
        }

        /* Custom Cursor */
        #custom-cursor {
            position: fixed;
            top: 0; left: 0;
            width: 20px; height: 20px;
            border-radius: 50%;
            background: theme('colors.neon.cyan');
            mix-blend-mode: screen;
            pointer-events: none;
            z-index: 9999;
            transform: translate(calc(var(--cursor-x) - 10px), calc(var(--cursor-y) - 10px));
            transition: width 0.2s, height 0.2s, background-color 0.2s;
            box-shadow: 0 0 20px theme('colors.neon.cyan');
            will-change: transform;
        }
        #custom-cursor.hovering {
            width: 50px; height: 50px;
            background: theme('colors.neon.purple');
            box-shadow: 0 0 30px theme('colors.neon.purple');
            transform: translate(calc(var(--cursor-x) - 25px), calc(var(--cursor-y) - 25px));
        }

        /* Liquid Gradient Background */
        .liquid-bg {
            position: fixed;
            top: 0; left: 0; width: 100vw; height: 100vh;
            z-index: -1;
            overflow: hidden;
            background: #050508;
        }
        .blob {
            position: absolute;
            border-radius: 50%;
            filter: blur(100px);
            opacity: 0.6;
            animation: moveBlobs 25s infinite alternate ease-in-out;
            will-change: transform;
        }
        .blob-1 { width: 60vw; height: 60vw; background: rgba(176, 38, 255, 0.3); top: -20%; left: -10%; }
        .blob-2 { width: 50vw; height: 50vw; background: rgba(0, 242, 254, 0.2); bottom: -20%; right: -10%; animation-delay: -5s; animation-direction: alternate-reverse; }
        .blob-3 { width: 40vw; height: 40vw; background: rgba(255, 8, 68, 0.2); top: 20%; left: 40%; animation-delay: -10s; }

        @keyframes moveBlobs {
            0% { transform: translate(0, 0) scale(1) rotate(0deg); }
            50% { transform: translate(10vw, 10vh) scale(1.2) rotate(180deg); }
            100% { transform: translate(-10vw, 5vh) scale(0.9) rotate(360deg); }
        }

        /* 3D Assembly System */
        .scene-container {
            perspective: 1200px;
            transform-style: preserve-3d;
        }

        /* Disassembled State (Default) */
        .fragment {
            /* Variables injected via JS for randomness */
            transform: translate3d(var(--tx, 0), var(--ty, 0), var(--tz, 0)) 
                       rotateX(var(--rx, 0)) 
                       rotateY(var(--ry, 0)) 
                       rotateZ(var(--rz, 0)) 
                       scale(var(--s, 1));
            opacity: 0;
            filter: blur(15px);
            transition: transform 1.8s cubic-bezier(0.16, 1, 0.3, 1), 
                        opacity 1.5s ease-out,
                        filter 1.5s ease-out;
            will-change: transform, opacity, filter;
        }

        /* Assembled State (Triggered by IntersectionObserver class .is-visible on parent) */
        .scroll-section.is-visible .fragment {
            transform: translate3d(0, 0, 0) rotateX(0) rotateY(0) rotateZ(0) scale(1);
            opacity: 1;
            filter: blur(0);
        }

        /* Glassmorphism */
        .glass-panel {
            background: rgba(255, 255, 255, 0.03);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border-top: 1px solid rgba(255, 255, 255, 0.15);
            border-left: 1px solid rgba(255, 255, 255, 0.1);
            border-bottom: 1px solid rgba(0, 0, 0, 0.4);
            border-right: 1px solid rgba(0, 0, 0, 0.4);
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.3), inset 0 0 20px rgba(255,255,255,0.02);
        }

        /* Text Gradients */
        .text-gradient {
            background: linear-gradient(135deg, #00f2fe 0%, #b026ff 50%, #ff0844 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-size: 200% auto;
            animation: shine 5s linear infinite;
        }
        @keyframes shine {
            to { background-position: 200% center; }
        }

        /* Custom Range Slider */
        input[type=range] { -webkit-appearance: none; width: 100%; background: transparent; cursor: none; }
        input[type=range]::-webkit-slider-thumb {
            -webkit-appearance: none; height: 20px; width: 20px; border-radius: 50%;
            background: #00f2fe; margin-top: -8px;
            box-shadow: 0 0 15px rgba(0,242,254,0.8);
        }
        input[type=range]::-webkit-slider-runnable-track {
            width: 100%; height: 4px; background: rgba(255,255,255,0.1); border-radius: 2px;
        }
    </style>
</head>
<body class="antialiased selection:bg-neon-purple selection:text-white">

    <!-- Custom Cursor -->
    <div id="custom-cursor"></div>

    <!-- Liquid Background -->
    <div class="liquid-bg">
        <div class="blob blob-1"></div>
        <div class="blob blob-2"></div>
        <div class="blob blob-3"></div>
    </div>

    <!-- Header -->
    <header class="fixed top-0 left-0 w-full z-50 transition-all duration-300 backdrop-blur-md bg-obsidian/40 border-b border-white/5 py-4">
        <div class="max-w-7xl mx-auto px-6 flex justify-between items-center">
            <a href="#" class="flex items-center gap-3 interactive" data-cursor="hover">
                <div class="w-10 h-10 rounded-lg glass-panel flex items-center justify-center border-t-neon-cyan shadow-[0_0_15px_rgba(0,242,254,0.2)]">
                    <i class="fa-solid fa-n text-xl text-white"></i>
                </div>
                <span class="font-display font-black text-xl tracking-wide">NEXO<span class="text-neon-cyan">LAB</span></span>
            </a>
            <nav class="hidden md:flex gap-8 items-center">
                <a href="#enfoque" class="font-mono text-xs font-bold tracking-widest uppercase hover:text-neon-cyan transition-colors interactive">Enfoque</a>
                <a href="#herramientas" class="font-mono text-xs font-bold tracking-widest uppercase hover:text-neon-purple transition-colors interactive">Herramientas</a>
                <a href="#contacto" class="glass-panel px-6 py-2.5 rounded-full font-display font-bold text-sm hover:bg-white/10 transition-colors shadow-[0_0_15px_rgba(176,38,255,0.3)] interactive">
                    Agendar Diagnóstico
                </a>
            </nav>
        </div>
    </header>

    <!-- Main Content -->
    <main class="w-full flex flex-col items-center overflow-hidden pt-24">

        <!-- Hero Section (Auto-assembles on load) -->
        <section class="scroll-section scene-container relative w-full min-h-screen flex items-center justify-center px-6 py-20" id="hero">
            <div class="max-w-5xl mx-auto text-center z-10 w-full relative">
                
                <div class="fragment" data-delay="0.1" style="--tx: 0; --ty: -300px; --tz: -500px; --rx: 45deg;">
                    <div class="mx-auto inline-flex items-center gap-2 glass-panel px-4 py-1.5 rounded-full mb-8 border-neon-cyan/30">
                        <span class="w-2 h-2 rounded-full bg-neon-cyan animate-pulse"></span>
                        <p class="font-mono text-[10px] font-bold tracking-[0.2em] uppercase text-white/80">
                            Ingeniería Comercial + Desarrollo Web
                        </p>
                    </div>
                </div>

                <h1 class="fragment font-display text-5xl md:text-7xl lg:text-8xl font-black leading-[1.05] tracking-tight mb-8" data-delay="0.3" style="--tx: -500px; --ty: 200px; --tz: 300px; --ry: -30deg;">
                    Ensamblamos el <br>
                    <span class="text-gradient">Motor Digital</span><br>
                    de tu Negocio.
                </h1>

                <p class="fragment font-body text-lg md:text-xl text-white/70 mb-12 max-w-2xl mx-auto leading-relaxed" data-delay="0.5" style="--tx: 500px; --ty: 100px; --tz: -200px; --ry: 30deg;">
                    No hacemos solo páginas web. Analizamos tu modelo comercial y construimos ecosistemas digitales que automatizan tus ventas, ordenan tus leads y multiplican tu rentabilidad.
                </p>

                <div class="fragment" data-delay="0.7" style="--tx: 0; --ty: 400px; --tz: 500px; --rx: -45deg;">
                    <a href="#herramientas" class="interactive inline-flex items-center gap-3 glass-panel px-8 py-4 rounded-xl font-display font-bold text-lg hover:bg-white/10 transition-all border-neon-cyan/50 shadow-[0_0_30px_rgba(0,242,254,0.3)]">
                        <span>Ver Simulador en Acción</span>
                        <i class="fa-solid fa-arrow-down text-neon-cyan animate-bounce"></i>
                    </a>
                </div>

                <!-- Floating 3D Decorators -->
                <div class="fragment absolute top-[10%] left-[5%] w-24 h-24 glass-panel rounded-2xl flex items-center justify-center border-neon-purple/30 z-[-1]" data-delay="0.8" style="--tx: -800px; --ty: -400px; --rz: -90deg;">
                    <i class="fa-solid fa-chart-pie text-3xl text-neon-purple/50"></i>
                </div>
                <div class="fragment absolute bottom-[15%] right-[5%] w-32 h-32 glass-panel rounded-full flex items-center justify-center border-neon-cyan/30 z-[-1]" data-delay="0.9" style="--tx: 800px; --ty: 600px; --rz: 90deg;">
                    <i class="fa-solid fa-gears text-4xl text-neon-cyan/50"></i>
                </div>
            </div>
        </section>

        <!-- Enfoque / Servicios -->
        <section class="scroll-section scene-container relative w-full py-32 px-6" id="enfoque">
            <div class="max-w-7xl mx-auto">
                <div class="fragment text-center mb-20" data-delay="0" style="--tx: 0; --ty: -200px;">
                    <h2 class="font-display font-black text-4xl md:text-5xl mb-4">Metodología <span class="text-neon-purple">NexoLab</span></h2>
                    <p class="font-body text-white/60 max-w-xl mx-auto">Combinamos el análisis estratégico con tecnología de punta para transformar operaciones locales en sistemas escalables.</p>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
                    <!-- Card 1 -->
                    <div class="fragment glass-panel p-8 rounded-3xl" data-delay="0.2" style="--tx: -400px; --ty: 200px; --ry: -45deg;">
                        <div class="w-14 h-14 rounded-2xl bg-neon-pink/20 flex items-center justify-center mb-6 border border-neon-pink/30">
                            <i class="fa-solid fa-chess-knight text-2xl text-neon-pink"></i>
                        </div>
                        <h3 class="font-display font-bold text-xl mb-3">1. Estrategia Comercial</h3>
                        <p class="font-body text-sm text-white/60 leading-relaxed">
                            Diagnóstico integral de tu PYME. Definimos tu propuesta de valor, analizamos cuellos de botella y diseñamos un embudo de conversión optimizado.
                        </p>
                    </div>
                    
                    <!-- Card 2 -->
                    <div class="fragment glass-panel p-8 rounded-3xl" data-delay="0.4" style="--tx: 0; --ty: 300px; --tz: 200px; --rx: 45deg;">
                        <div class="w-14 h-14 rounded-2xl bg-neon-cyan/20 flex items-center justify-center mb-6 border border-neon-cyan/30">
                            <i class="fa-solid fa-cubes text-2xl text-neon-cyan"></i>
                        </div>
                        <h3 class="font-display font-bold text-xl mb-3">2. Presencia & Repositorio</h3>
                        <p class="font-body text-sm text-white/60 leading-relaxed">
                            Desarrollamos interfaces web ultra rápidas conectadas a un repositorio central, garantizando que ninguna solicitud o lead se pierda.
                        </p>
                    </div>

                    <!-- Card 3 -->
                    <div class="fragment glass-panel p-8 rounded-3xl" data-delay="0.6" style="--tx: 400px; --ty: 200px; --ry: 45deg;">
                        <div class="w-14 h-14 rounded-2xl bg-neon-purple/20 flex items-center justify-center mb-6 border border-neon-purple/30">
                            <i class="fa-solid fa-robot text-2xl text-neon-purple"></i>
                        </div>
                        <h3 class="font-display font-bold text-xl mb-3">3. Automatización IA</h3>
                        <p class="font-body text-sm text-white/60 leading-relaxed">
                            Implementamos flujos de trabajo inteligentes que responden, clasifican y derivan tareas automáticamente, ahorrando decenas de horas semanales.
                        </p>
                    </div>
                </div>
            </div>
        </section>

        <!-- Interactive Tools (ROI & Simulator) -->
        <section class="scroll-section scene-container relative w-full py-32 px-6" id="herramientas">
            <div class="max-w-7xl mx-auto grid grid-cols-1 lg:grid-cols-2 gap-12">
                
                <!-- ROI Calculator -->
                <div class="fragment glass-panel rounded-[2.5rem] p-8 md:p-10 flex flex-col justify-between" data-delay="0.2" style="--tx: -500px; --ty: 0; --rz: -15deg; --s: 0.8;">
                    <div>
                        <div class="flex items-center gap-4 mb-6">
                            <div class="w-12 h-12 rounded-xl bg-neon-cyan/20 flex items-center justify-center border border-neon-cyan/30">
                                <i class="fa-solid fa-chart-line text-xl text-neon-cyan"></i>
                            </div>
                            <h2 class="font-display font-bold text-3xl">Impacto Operativo</h2>
                        </div>
                        <p class="font-body text-sm text-white/60 mb-10">Calcula cuántos recursos libera nuestro ecosistema al automatizar la gestión de prospectos de tu negocio local.</p>

                        <div class="space-y-10">
                            <div class="interactive" data-cursor="hover">
                                <div class="flex justify-between font-mono text-xs font-bold mb-4 uppercase text-white/80">
                                    <span>Horas Manuales Semanales (Atención)</span>
                                    <span id="val-hours" class="text-neon-cyan font-black bg-neon-cyan/10 px-2 py-1 rounded">20h</span>
                                </div>
                                <input type="range" id="slider-hours" min="5" max="80" value="20" class="w-full">
                            </div>

                            <div class="interactive" data-cursor="hover">
                                <div class="flex justify-between font-mono text-xs font-bold mb-4 uppercase text-white/80">
                                    <span>Cotizaciones/Leads Mensuales</span>
                                    <span id="val-leads" class="text-neon-purple font-black bg-neon-purple/10 px-2 py-1 rounded">150</span>
                                </div>
                                <input type="range" id="slider-leads" min="10" max="1000" value="150" step="10" class="w-full">
                            </div>
                        </div>
                    </div>

                    <div class="mt-12 p-6 glass-panel rounded-2xl border-white/5 flex justify-between items-center bg-black/40">
                        <div>
                            <p class="font-mono text-[10px] uppercase font-bold text-white/50 mb-2">Tiempo Recuperado (Mensual)</p>
                            <p class="font-display font-black text-4xl text-white flex items-baseline gap-2">
                                <i class="fa-regular fa-clock text-neon-cyan text-xl"></i>
                                <span id="res-time">64</span> <span class="text-sm font-normal text-white/50">hrs</span>
                            </p>
                        </div>
                        <div class="text-right border-l border-white/10 pl-6">
                            <p class="font-mono text-[10px] uppercase font-bold text-white/50 mb-2">Aumento Tasa Conversión</p>
                            <p class="font-display font-black text-4xl text-neon-pink flex justify-end items-baseline gap-2" id="res-sales">
                                +22%
                            </p>
                        </div>
                    </div>
                </div>

                <!-- Live Repository Simulator -->
                <div class="fragment glass-panel rounded-[2.5rem] p-8 md:p-10 flex flex-col h-[600px]" data-delay="0.4" style="--tx: 500px; --ty: 0; --rz: 15deg; --s: 0.8;">
                    <div class="flex items-center gap-4 mb-6">
                        <div class="w-12 h-12 rounded-xl bg-neon-purple/20 flex items-center justify-center border border-neon-purple/30">
                            <i class="fa-solid fa-microchip text-xl text-neon-purple"></i>
                        </div>
                        <h2 class="font-display font-bold text-3xl">Simulador de Clasificación IA</h2>
                    </div>
                    <p class="font-body text-sm text-white/60 mb-6">Escribe un mensaje como si fueras un cliente. Mira cómo NexoLab lo clasifica, prioriza y almacena en el repositorio al instante.</p>

                    <form id="sim-form" class="relative mb-6 interactive" data-cursor="hover">
                        <input type="text" id="sim-input" required class="w-full bg-black/50 border border-white/10 rounded-xl py-4 px-5 font-body text-sm focus:outline-none focus:border-neon-purple focus:ring-1 focus:ring-neon-purple placeholder:text-white/30 text-white transition-all shadow-inner" placeholder="Ej: Hola, necesito cotizar un servicio urgente...">
                        <button type="submit" class="absolute right-2 top-2 bottom-2 bg-white/5 hover:bg-neon-purple hover:text-white text-white/70 px-6 rounded-lg font-mono text-xs font-bold transition-all border border-white/10 hover:border-transparent">
                            Enviar
                        </button>
                    </form>

                    <!-- Terminal Output -->
                    <div class="flex-1 bg-[#020204] border border-white/10 rounded-2xl p-5 flex flex-col relative overflow-hidden shadow-inner">
                        <!-- Mac OS style header -->
                        <div class="flex gap-2 mb-4 border-b border-white/5 pb-4">
                            <div class="w-3 h-3 rounded-full bg-red-500/80"></div>
                            <div class="w-3 h-3 rounded-full bg-yellow-500/80"></div>
                            <div class="w-3 h-3 rounded-full bg-green-500/80"></div>
                            <span class="ml-2 font-mono text-[10px] text-white/30 uppercase tracking-widest">Repositorio_Activo.exe</span>
                        </div>
                        
                        <div id="sim-output" class="flex-1 overflow-y-auto space-y-4 font-mono text-xs pr-2">
                            <div class="opacity-50 flex gap-3">
                                <span class="text-neon-cyan mt-0.5">></span>
                                <span class="leading-relaxed">Sistema inicializado. <br>Esperando inputs de clientes (Web/WhatsApp)...</span>
                            </div>
                        </div>
                    </div>
                </div>

            </div>
        </section>

        <!-- Footer / CTA -->
        <section class="scroll-section scene-container w-full py-32 relative text-center" id="contacto">
            <div class="fragment max-w-3xl mx-auto px-6" data-delay="0.2" style="--ty: 300px; --rx: 60deg;">
                <h2 class="font-display font-black text-5xl md:text-6xl mb-6">Listos para <span class="text-neon-cyan">Ensamblar.</span></h2>
                <p class="font-body text-xl text-white/70 mb-10">Deja de perder tiempo en tareas manuales y clientes sin responder. Agenda un diagnóstico comercial gratuito y diseñemos tu arquitectura digital.</p>
                <a href="#" class="interactive inline-flex items-center gap-4 bg-white text-black px-10 py-5 rounded-full font-display font-bold text-lg hover:scale-105 transition-transform shadow-[0_0_40px_rgba(255,255,255,0.3)]">
                    Agendar Diagnóstico <i class="fa-solid fa-arrow-right"></i>
                </a>
            </div>
            
            <div class="fragment mt-32 border-t border-white/10 pt-8 w-full max-w-7xl mx-auto flex flex-col md:flex-row justify-between items-center px-6 text-white/40 font-mono text-xs" data-delay="0.4" style="--ty: 100px;">
                <p>&copy; 2026 NexoLab. Todos los derechos reservados.</p>
                <p class="mt-4 md:mt-0 flex items-center gap-2">
                    <i class="fa-solid fa-terminal"></i> Conecta. Innova. Automatiza.
                </p>
            </div>
        </section>
    </main>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            
            // ==========================================
            // 1. Custom Cursor Logic
            // ==========================================
            const cursor = document.getElementById('custom-cursor');
            let mouseX = window.innerWidth / 2;
            let mouseY = window.innerHeight / 2;
            let cursorX = mouseX;
            let cursorY = mouseY;
            
            // Track mouse
            window.addEventListener('mousemove', (e) => {
                mouseX = e.clientX;
                mouseY = e.clientY;
            });

            // Smooth interpolation (Lerp)
            function animateCursor() {
                // Lerp factor (lower = smoother/slower)
                cursorX += (mouseX - cursorX) * 0.15;
                cursorY += (mouseY - cursorY) * 0.15;
                
                document.documentElement.style.setProperty('--cursor-x', `${cursorX}px`);
                document.documentElement.style.setProperty('--cursor-y', `${cursorY}px`);
                
                requestAnimationFrame(animateCursor);
            }
            animateCursor();

            // Hover effects
            const interactives = document.querySelectorAll('.interactive, a, button, input');
            interactives.forEach(el => {
                el.addEventListener('mouseenter', () => cursor.classList.add('hovering'));
                el.addEventListener('mouseleave', () => cursor.classList.remove('hovering'));
            });

            // ==========================================
            // 2. Scroll-Triggered 3D Assembly (IntersectionObserver)
            // ==========================================
            const sections = document.querySelectorAll('.scroll-section');
            
            const observerOptions = {
                root: null,
                rootMargin: '0px',
                threshold: 0.15 // Triggers when 15% of the section is visible
            };

            const sectionObserver = new IntersectionObserver((entries, observer) => {
                entries.forEach(entry => {
                    if (entry.isIntersecting) {
                        // Add class to assemble
                        entry.target.classList.add('is-visible');
                        
                        // Apply delays based on data-delay attribute
                        const fragments = entry.target.querySelectorAll('.fragment');
                        fragments.forEach(frag => {
                            const delay = frag.getAttribute('data-delay') || '0';
                            frag.style.transitionDelay = `${delay}s, ${delay}s, ${delay}s`; // for transform, opacity, filter
                        });
                        
                        // Optional: Stop observing if you only want it to assemble once
                        // observer.unobserve(entry.target); 
                    } else {
                        // Remove class to disassemble when scrolling away (creates the continuous effect)
                        // Comment this out if you want elements to stay assembled once seen.
                        entry.target.classList.remove('is-visible');
                    }
                });
            }, observerOptions);

            sections.forEach(section => {
                sectionObserver.observe(section);
            });

            // Immediately trigger hero section if it's already in view on load
            setTimeout(() => {
                const hero = document.getElementById('hero');
                if(hero) hero.classList.add('is-visible');
            }, 100);


            // ==========================================
            // 3. ROI Calculator Logic
            // ==========================================
            const slHours = document.getElementById('slider-hours');
            const slLeads = document.getElementById('slider-leads');
            const valHours = document.getElementById('val-hours');
            const valLeads = document.getElementById('val-leads');
            const resTime = document.getElementById('res-time');
            const resSales = document.getElementById('res-sales');

            function updateCalc() {
                const h = parseInt(slHours.value);
                const l = parseInt(slLeads.value);
                
                valHours.innerText = `${h}h`;
                valLeads.innerText = l;

                // Simple business logic calculation
                const savedHours = Math.floor(h * 0.8 * 4); // 80% time saved per month
                const salesBoost = Math.floor(Math.min(l * 0.15 + (h*0.3), 350)); 

                resTime.innerText = savedHours;
                resSales.innerText = `+${salesBoost}%`;
            }

            slHours.addEventListener('input', updateCalc);
            slLeads.addEventListener('input', updateCalc);
            updateCalc();


            // ==========================================
            // 4. Live Repository Simulator Logic
            // ==========================================
            const simForm = document.getElementById('sim-form');
            const simInput = document.getElementById('sim-input');
            const simOutput = document.getElementById('sim-output');

            simForm.addEventListener('submit', (e) => {
                e.preventDefault();
                const text = simInput.value.trim();
                if(!text) return;

                const txtLower = text.toLowerCase();
                
                // Simple keyword detection to simulate AI tagging
                const isUrgent = /(urgente|comprar|precio|cotizar|necesito|pago|tarjeta|ahora)/.test(txtLower);
                const isComplaint = /(malo|problema|queja|error|no funciona|ayuda|retraso)/.test(txtLower);
                
                let color, tag, action;
                
                if(isComplaint) {
                    color = 'text-neon-pink';
                    tag = '[ALERTA] SOPORTE';
                    action = 'Asignando a equipo humano -> Prioridad 1';
                } else if(isUrgent) {
                    color = 'text-neon-cyan';
                    tag = '[LEAD CALIENTE] VENTA';
                    action = 'Enviando link de pago automatizado...';
                } else {
                    color = 'text-neon-purple';
                    tag = '[INFO] CONSULTA';
                    action = 'Enviando PDF de servicios corporativos...';
                }

                // Generate ID and Score
                const id = Math.random().toString(36).substring(2, 8).toUpperCase();
                const score = Math.floor(Math.random() * 15 + 85);

                const newLog = `
                    <div class="bg-white/5 p-4 rounded-xl border border-white/10 opacity-0 transform translate-x-8 transition-all duration-500 ease-out" id="anim-log">
                        <div class="flex justify-between items-start mb-2 border-b border-white/5 pb-2">
                            <span class="font-bold ${color} text-[10px] tracking-wider">${tag} // ID:${id}</span>
                            <span class="text-[9px] text-white/50 border border-white/10 px-1.5 py-0.5 rounded bg-black/50">Confianza IA: ${score}%</span>
                        </div>
                        <p class="text-white/90 text-sm mb-3 font-body leading-relaxed">"${text}"</p>
                        <div class="flex gap-2 text-[10px] items-center bg-black/30 p-2 rounded">
                            <i class="fa-solid fa-bolt text-yellow-500"></i>
                            <span class="text-white/60">Acción: <span class="text-white/90">${action}</span></span>
                        </div>
                    </div>
                `;

                // Prepend to output
                simOutput.insertAdjacentHTML('afterbegin', newLog);
                
                // Trigger entry animation
                setTimeout(() => {
                    const log = document.getElementById('anim-log');
                    if(log) {
                        log.classList.remove('opacity-0', 'translate-x-8');
                        log.removeAttribute('id');
                    }
                }, 50);

                simInput.value = '';
                
                // Scroll to top of terminal
                simOutput.scrollTop = 0;
            });
        });
    </script>
</body>
</html>
