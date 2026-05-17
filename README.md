<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Portfolio Premium | Tchissambou Gloire</title>

    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Oswald:wght@400;700&family=Poppins:wght@300;400;500;600&display=swap" rel="stylesheet">

    <style>
        :root {
            --cyan: #00e5ff;
            --dark: #050505;
            --card: rgba(255, 255, 255, 0.05);
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            font-family: 'Poppins', sans-serif;
            background: #000;
            color: white;
            overflow-x: hidden;
            scroll-behavior: smooth;
        }

        /* --- EFFET NEIGE & FOND --- */
        #snowCanvas { position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: 0; pointer-events: none; }
        .bg-glow { position: fixed; inset: 0; background: radial-gradient(circle at center, #10101a 0%, #000 100%); z-index: -1; }

        /* --- BARRE DE NAVIGATION FIXE --- */
        .navbar-custom {
            position: fixed; top: 0; left: 0; width: 100%; padding: 15px 8%;
            display: flex; justify-content: space-between; align-items: center;
            background: rgba(0, 0, 0, 0.85); backdrop-filter: blur(15px); z-index: 1000;
            border-bottom: 1px solid rgba(0, 229, 255, 0.1);
        }
        .logo-box { display: flex; align-items: center; gap: 12px; }
        .logo-box img { width: 45px; height: 45px; border-radius: 50%; border: 2px solid var(--cyan); object-fit: cover; }
        .logo-box h1 { font-family: 'Oswald'; font-size: 1.6rem; margin: 0; letter-spacing: 2px; }
        .logo-box span { color: var(--cyan); }
        .menu-toggle { font-size: 1.8rem; color: var(--cyan); cursor: pointer; }

        /* --- MENU OVERLAY --- */
        .nav-overlay {
            position: fixed; top: 0; right: -100%; width: 100%; height: 100vh;
            background: rgba(5, 5, 5, 0.98); display: flex; flex-direction: column;
            justify-content: center; align-items: center; transition: 0.6s cubic-bezier(0.85, 0, 0.15, 1); z-index: 999;
        }
        .nav-overlay.open { right: 0; }
        .nav-overlay a { color: white; text-decoration: none; font-size: 2.5rem; margin: 15px; font-family: 'Oswald'; transition: 0.3s; text-transform: uppercase; }
        .nav-overlay a:hover { color: var(--cyan); letter-spacing: 5px; }

        /* --- SECTION HEADER --- */
        header { min-height: 85vh; display: flex; flex-direction: column; justify-content: center; align-items: center; text-align: center; position: relative; z-index: 1; padding-top: 120px; }
        header h1 { font-family: 'Oswald'; font-size: clamp(2.5rem, 8vw, 5rem); text-transform: uppercase; line-height: 1.1; }
        header h1 span { color: var(--cyan); }
        header p { letter-spacing: 4px; color: #888; font-weight: 300; margin-top: 10px; }
        .profile-box img { width: 200px; height: 200px; border-radius: 50%; border: 4px solid var(--cyan); margin-top: 30px; box-shadow: 0 0 35px rgba(0, 229, 255, 0.3); object-fit: cover; }

        /* --- SECTIONS & REVEAL --- */
        section { padding: 90px 10%; max-width: 1200px; margin: auto; position: relative; z-index: 1; }
        .reveal { opacity: 0; transform: translateY(50px); transition: 1s ease-out; }
        .reveal.active { opacity: 1; transform: translateY(0); }
        h2 { font-family: 'Oswald'; font-size: 2.3rem; text-transform: uppercase; border-left: 5px solid var(--cyan); padding-left: 20px; margin-bottom: 40px; }

        /* --- COMPÉTENCES (SKILLS) --- */
        .skill-item { margin-bottom: 30px; }
        .skill-info { display: flex; align-items: center; gap: 12px; margin-bottom: 8px; font-weight: 500; }
        .skill-info i { font-size: 1.5rem; }
        .bar { background: #1a1a1a; height: 12px; width: 100%; border-radius: 6px; overflow: hidden; position: relative; }
        .fill { background: var(--cyan); height: 100%; width: 0; transition: 2s cubic-bezier(0.1, 0.5, 0.5, 1); box-shadow: 0 0 12px var(--cyan); }

        /* --- PROJETS (GRID) --- */
        .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 30px; }
        .card { background: var(--card); padding: 30px; border-radius: 15px; border: 1px solid rgba(255,255,255,0.08); transition: 0.4s; display: flex; flex-direction: column; justify-content: space-between; }
        .card:hover { border-color: var(--cyan); transform: translateY(-10px); background: rgba(0, 229, 255, 0.03); }
        .card h3 { color: var(--cyan); font-family: 'Oswald'; margin-bottom: 10px; font-size: 1.4rem; }
        .tech-list { margin: 15px 0; min-height: 80px; }
        .tech-badge { font-size: 0.7rem; background: rgba(255, 255, 255, 0.1); padding: 4px 10px; border-radius: 20px; color: var(--cyan); border: 1px solid rgba(0, 229, 255, 0.2); margin: 0 5px 8px 0; display: inline-block; }
        .project-footer { display: flex; justify-content: space-between; align-items: center; margin-top: 15px; border-top: 1px solid rgba(255,255,255,0.1); padding-top: 15px; }
        .status-done { color: #00ff99; font-weight: bold; font-size: 0.75rem; text-transform: uppercase; }
        .status-wait { color: #ffd700; font-weight: bold; font-size: 0.75rem; text-transform: uppercase; }

        /* --- FORMULAIRE CONTACT --- */
        form input, form textarea { width: 100%; padding: 18px; margin: 12px 0; background: rgba(255,255,255,0.03); border: 1px solid #333; color: white; border-radius: 12px; outline: none; transition: 0.3s; }
        form input:focus, form textarea:focus { border-color: var(--cyan); background: rgba(0, 229, 255, 0.02); }
        form button { background: var(--cyan); color: #000; border: none; padding: 18px; font-weight: 800; text-transform: uppercase; cursor: pointer; transition: 0.3s; width: 100%; border-radius: 12px; margin-top: 10px; }
        form button:hover { box-shadow: 0 0 25px var(--cyan); transform: scale(1.02); }

        footer { text-align: center; padding: 60px 20px; color: #666; font-size: 0.9rem; }
    </style>
</head>
<body>

    <canvas id="snowCanvas"></canvas>
    <div class="bg-glow"></div>

    <nav class="navbar-custom">
        <div class="logo-box">
            <img src="gloire.png" alt="Gloire Logo">
            <h1>GLOIRE<span>.</span></h1>
        </div>
        <div class="menu-toggle" onclick="toggleMenu()">
            <i class="fas fa-bars" id="menu-icon"></i>
        </div>
    </nav>

    <nav class="nav-overlay" id="nav-overlay">
        <a href="#home" onclick="toggleMenu()">Accueil</a>
        <a href="#about" onclick="toggleMenu()">Profil</a>
        <a href="#skills" onclick="toggleMenu()">Compétences</a>
        <a href="#projects" onclick="toggleMenu()">Projets</a>
        <a href="#contact" onclick="toggleMenu()">Contact</a>
    </nav>

    <header id="home">
        <div class="reveal active">
            <h1>Tchissambou <span>Gloire</span></h1>
            <p>DÉVELOPPEUR WEB & DESIGNER INNOVANT</p>
            <div class="profile-box">
                <img src="gloire.png" alt="Tchissambou Gloire">
            </div>
        </div>
    </header>

    <section id="about" class="reveal">
        <h2>Mon Profil</h2>
        <p>Développeur Web spécialisé dans la conception d'interfaces intuitives et la sécurisation des architectures applicatives. J'allie une expertise en développement front-end/back-end à de solides compétences en cybersécurité et infrastructures réseaux pour livrer des solutions robustes, évolutives et hautement sécurisées. Mon objectif : transformer des exigences techniques complexes en plateformes fluides, performantes et protégées contre les cybermenaces.</p>
    </section>

    <section id="skills" class="reveal">
        <h2>Mes Compétences</h2>
        <div class="skill-item">
            <div class="skill-info"><i class="fab fa-html5" style="color: #e34c26;"></i><i class="fab fa-css3-alt" style="color: #264de4;"></i><span>HTML 5 / CSS 3</span></div>
            <div class="bar"><div class="fill" data-width="100%"></div></div>
        </div>
        <div class="skill-item">
            <div class="skill-info"><i class="fab fa-js" style="color: #f7df1e;"></i><i class="fab fa-react" style="color: #61dbfb;"></i><span>JavaScript / React</span></div>
            <div class="bar"><div class="fill" data-width="90%"></div></div>
        </div>
        <div class="skill-item">
            <div class="skill-info"><i class="fas fa-code" style="color: #00599c;"></i><span>Langage C++</span></div>
            <div class="bar"><div class="fill" data-width="85%"></div></div>
        </div>
        <div class="skill-item">
            <div class="skill-info"><i class="fas fa-microchip" style="color: #a8b9cc;"></i><span>Langage C</span></div>
            <div class="bar"><div class="fill" data-width="80%"></div></div>
        </div>
        <div class="skill-item">
            <div class="skill-info"><i class="fas fa-mobile-alt" style="color: #47d1fd;"></i><span>FlutterFlow</span></div>
            <div class="bar"><div class="fill" data-width="75%"></div></div>
        </div>
        <div class="skill-item">
            <div class="skill-info"><i class="fas fa-network-wired" style="color: #00ffcc;"></i><span>Réseaux informatiques</span></div>
            <div class="bar"><div class="fill" data-width="85%"></div></div>
        </div>
        <div class="skill-item">
            <div class="skill-info"><i class="fas fa-project-diagram" style="color: #ff9900;"></i><span>Algorithmes</span></div>
            <div class="bar"><div class="fill" data-width="95%"></div></div>
        </div>
    </section>

    <section id="projects" class="reveal">
        <h2>Mes Projets</h2>
        <div class="grid">
            
            <div class="card">
                <h3>Portfolio Premium v2</h3>
                <p>Interface immersive avec effets visuels avancés.</p>
                <div class="tech-list">
                    <span class="tech-badge">HTML5</span><span class="tech-badge">CSS3</span><span class="tech-badge">JS</span><span class="tech-badge">React</span>
                </div>
                <div class="project-footer"><span>2026</span><span class="status-done">Terminé</span></div>
            </div>

            <div class="card">
                <h3>Gestion de Salle Réseau</h3>
                <p>Conception d'infrastructure et câblage structuré.</p>
                <div class="tech-list">
                    <span class="tech-badge">Câblage</span><span class="tech-badge">Baie de brassage</span><span class="tech-badge">Cisco</span>
                </div>
                <div class="project-footer"><span>2026</span><span class="status-done">Terminé</span></div>
            </div>

            <div class="card">
                <h3>Système de Cybersécurité</h3>
                <p>Audit de sécurité et détection de vulnérabilités.</p>
                <div class="tech-list">
                    <span class="tech-badge">Kali Linux</span><span class="tech-badge">Wireshark</span><span class="tech-badge">Metasploit</span><span class="tech-badge">Python</span>
                </div>
                <div class="project-footer"><span>2025</span><span class="status-wait">En cours</span></div>
            </div>

            <div class="card">
                <h3>Mini-Jeu Interactif</h3>
                <p>Développement de mécaniques de jeu et logique IA.</p>
                <div class="tech-list">
                    <span class="tech-badge">Python</span><span class="tech-badge">Algorithmes</span>
                </div>
                <div class="project-footer"><span>2025</span><span class="status-done">Terminé</span></div>
            </div>

            <div class="card">
                <h3>App Mobile No-Code</h3>
                <p>Application cross-plateforme performante.</p>
                <div class="tech-list">
                    <span class="tech-badge">FlutterFlow</span><span class="tech-badge">Firebase</span>
                </div>
                <div class="project-footer"><span>2025</span><span class="status-wait">En cours</span></div>
            </div>

            <div class="card">
                <h3>Optimisation Algorithmique</h3>
                <p>Résolution de problèmes complexes et structures de données.</p>
                <div class="tech-list">
                    <span class="tech-badge">C++</span><span class="tech-badge">STL</span>
                </div>
                <div class="project-footer"><span>2024</span><span class="status-done">Terminé</span></div>
            </div>

            <div class="card">
                <h3>Plateforme E-commerce</h3>
                <p>Boutique en ligne avec gestion de panier et paiements.</p>
                <div class="tech-list">
                    <span class="tech-badge">React</span><span class="tech-badge">PHP</span><span class="tech-badge">MySQL</span>
                </div>
                <div class="project-footer"><span>2024</span><span class="status-wait">En cours</span></div>
            </div>

            <div class="card">
                <h3>Gestionnaire de Mémoire</h3>
                <p>Programmation bas niveau et allocation dynamique.</p>
                <div class="tech-list">
                    <span class="tech-badge">Langage C</span><span class="tech-badge">Pointeurs</span>
                </div>
                <div class="project-footer"><span>2024</span><span class="status-done">Terminé</span></div>
            </div>

        </div>
    </section>

    <section id="contact" class="reveal">
        <h2>Contactez-moi</h2>
        <form action="https://api.web3for  ms.com/submit" method="POST">
            <input type="text" placeholder="Nom" required>
                        <input type="text" placeholder="Prenom" required>

            <input type="e-mail" placeholder="E-mail" required>
            <textarea rows="6" placeholder="Comment puis-je vous aider ?" required></textarea>
            <button type="submit">Envoyer le message</button>
        </form>
    </section>

    <footer>
        &copy; 2026 Tchissambou Gloire - Brazzaville, Congo.
    </footer>

    <script>
        function toggleMenu() {
            const overlay = document.getElementById('nav-overlay');
            const icon = document.getElementById('menu-icon');
            overlay.classList.toggle('open');
            icon.classList.toggle('fa-bars');
            icon.classList.toggle('fa-times');
        }

        function handleScroll() {
            const reveals = document.querySelectorAll('.reveal');
            reveals.forEach(el => {
                const rect = el.getBoundingClientRect();
                if (rect.top < window.innerHeight - 100) {
                    el.classList.add('active');
                    const bars = el.querySelectorAll('.fill');
                    bars.forEach(bar => {
                        bar.style.width = bar.getAttribute('data-width');
                    });
                }
            });
        }

        const canvas = document.getElementById('snowCanvas');
        const ctx = canvas.getContext('2d');
        let width, height, flakes = [];

        function initSnow() {
            width = window.innerWidth;
            height = window.innerHeight;
            canvas.width = width; canvas.height = height;
            flakes = [];
            for (let i = 0; i < 120; i++) {
                flakes.push({
                    x: Math.random() * width,
                    y: Math.random() * height,
                    size: Math.random() * 3 + 1,
                    speed: Math.random() * 1 + 0.5,
                    opacity: Math.random() * 0.5 + 0.3
                });
            }
        }

        function drawSnow() {
            ctx.clearRect(0, 0, width, height);
            ctx.fillStyle = "white";
            flakes.forEach(f => {
                ctx.globalAlpha = f.opacity;
                ctx.beginPath();
                ctx.arc(f.x, f.y, f.size, 0, Math.PI * 2);
                ctx.fill();
                f.y += f.speed;
                if (f.y > height) f.y = -10;
            });
            requestAnimationFrame(drawSnow);
        }

        window.addEventListener('scroll', handleScroll);
        window.addEventListener('resize', initSnow);
        window.onload = () => { handleScroll(); initSnow(); drawSnow(); };
    </script>
</body>
</html>
