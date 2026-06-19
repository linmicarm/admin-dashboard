/* ============================================================
   Admin Dashboard — "soft-girl engineer" kawaii direction
   Grid for layout · Flex for component rows · responsive · a11y
   ============================================================ */

@import url('https://fonts.googleapis.com/css2?family=Fredoka:wght@400;500;600;700&family=Nunito:wght@400;600;700;800&display=swap');

* { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --pink:     #ffd6e8;
  --lavender: #e0d4ff;
  --mint:     #c8f4dd;
  --butter:   #ffe89e;
  --peach:    #ffcdb8;
  --sky:      #bfe3ff;

  --primary:     #ff6f9c;   /* candy pink — darkened for AA contrast on white */
  --primary-deep:#e8537f;
  --surface:     #fffafc;
  --bg:          #fdeef5;
  --sidebar-bg:  #f6d9ec;
  --ink:         #4a3a4e;   /* warm plum, darkened for AA body contrast */
  --muted:       #8a6f8c;   /* darkened from #a692a8 → passes AA on white */
  --line:        #f0e0ea;
  --shadow:      0 8px 20px rgba(255, 111, 156, 0.18);
  --shadow-sm:   0 4px 12px rgba(150, 120, 150, 0.15);
  --focus:       #6b4ea8;   /* high-contrast focus ring */

  --display: 'Fredoka', 'Poppins', system-ui, sans-serif;
  --body:    'Nunito', 'Poppins', system-ui, sans-serif;
}

body {
  font-family: var(--body);
  color: var(--ink);
  background: var(--bg);

  display: grid;
  grid-template-columns: 250px 1fr;
  grid-template-rows: auto 1fr;
  grid-template-areas:
    "sidebar header"
    "sidebar main";
  min-height: 100vh;
}

h1, h2, h3, h4, .btn, .name { font-family: var(--display); }

/* ---- a11y utilities ---- */
.sr-only {
  position: absolute; width: 1px; height: 1px;
  padding: 0; margin: -1px; overflow: hidden;
  clip: rect(0,0,0,0); white-space: nowrap; border: 0;
}
.skip-link {
  position: absolute; left: 0; top: -3rem;
  background: var(--primary); color: white;
  padding: 0.6rem 1.2rem; border-radius: 0 0 1rem 0;
  z-index: 100; text-decoration: none; font-weight: 700;
  transition: top 0.15s;
}
.skip-link:focus { top: 0; }

/* visible focus ring everywhere */
a:focus-visible, button:focus-visible, input:focus-visible {
  outline: 3px solid var(--focus);
  outline-offset: 2px;
}

/* ============ SIDEBAR ============ */
.sidebar {
  grid-area: sidebar;
  background: var(--sidebar-bg);
  color: var(--ink);
  padding: 1.5rem 1.25rem;
  display: flex;
  flex-direction: column;
  gap: 2.5rem;
}

.branding { display: flex; align-items: center; gap: 0.6rem; }
.branding .logo {
  background: var(--primary);
  width: 2.5rem; height: 2.5rem;
  border-radius: 50%;
  display: grid; place-items: center;
}
.branding .logo svg { width: 1.4rem; height: 1.4rem; fill: white; }
.branding h1 { font-size: 1.5rem; font-weight: 600; }

.nav-primary, .nav-secondary {
  display: flex; flex-direction: column; gap: 0.5rem;
}

.nav-item {
  display: flex; align-items: center; gap: 1rem;
  color: var(--ink); text-decoration: none; font-weight: 700;
  padding: 0.6rem 0.9rem; border-radius: 1rem;
  transition: background 0.15s, transform 0.15s;
}
.nav-item .icon { width: 1.4rem; height: 1.4rem; fill: var(--ink); flex-shrink: 0; }
.nav-item:hover { background: rgba(255,255,255,0.55); transform: translateX(3px); }

/* ============ HEADER ============ */
.header {
  grid-area: header;
  background: var(--surface);
  box-shadow: var(--shadow-sm);
  padding: 1.1rem 2.5rem;
  position: relative; z-index: 1;
  border-bottom-left-radius: 2rem;
  display: flex; flex-direction: column; gap: 1.25rem;
}

.header-row {
  display: flex; align-items: center;
  justify-content: space-between; gap: 1.5rem;
}

.search {
  display: flex; align-items: center; gap: 0.75rem;
  flex: 1; max-width: 600px;
}
.search .icon { width: 1.3rem; height: 1.3rem; fill: var(--muted); flex-shrink: 0; }
.search input {
  flex: 1; border: 2px solid var(--line);
  background: var(--bg); border-radius: 1.5rem;
  padding: 0.55rem 1.1rem; font: inherit; color: var(--ink);
}
.search input::placeholder { color: var(--muted); }
.search input:focus { outline: none; border-color: var(--primary); }

.header-user { display: flex; align-items: center; gap: 1.25rem; }
.icon-btn {
  border: none; background: none; cursor: pointer;
  padding: 0.3rem; border-radius: 0.6rem; display: grid; place-items: center;
}
.icon-btn svg { width: 1.4rem; height: 1.4rem; fill: var(--ink); }
.username-sm { font-weight: 800; }

.greeting { display: flex; align-items: center; gap: 1rem; }
.greeting-text .hi { font-size: 0.85rem; font-weight: 700; color: var(--muted); }
.greeting-text .name { font-size: 1.5rem; font-weight: 600; }

.header-actions { display: flex; gap: 0.9rem; }
.btn {
  display: inline-flex; align-items: center; gap: 0.4rem;
  border: none; background: var(--primary); color: white;
  font-weight: 600; font-size: 0.95rem;
  padding: 0.6rem 1.5rem; border-radius: 1.5rem;
  cursor: pointer; box-shadow: var(--shadow);
  transition: transform 0.15s, background 0.15s;
}
.btn-icon { width: 1.1rem; height: 1.1rem; fill: white; }
.btn:hover { background: var(--primary-deep); transform: translateY(-2px); }

/* avatars — distinct pastel blobs */
.avatar {
  border-radius: 50%; display: inline-block; flex-shrink: 0;
  background: linear-gradient(135deg, var(--lavender), var(--pink));
}
.avatar-sm { width: 2.25rem; height: 2.25rem; }
.avatar-lg { width: 4rem; height: 4rem; }
.avatar-a { background: linear-gradient(135deg, var(--lavender), var(--pink)); }
.avatar-b { background: linear-gradient(135deg, var(--mint), var(--sky)); }
.avatar-c { background: linear-gradient(135deg, var(--butter), var(--peach)); }
.avatar-d { background: linear-gradient(135deg, var(--sky), var(--lavender)); }

/* ============ MAIN ============ */
.main-content {
  grid-area: main;
  padding: 2rem;
  display: grid;
  grid-template-columns: 1fr 280px;
  gap: 1.5rem;
}

.section-title { font-size: 1.2rem; font-weight: 600; margin-bottom: 0.85rem; }

.projects { display: flex; flex-direction: column; }

/* fixed 2-col sticker-book grid; flex:1 + equal rows = even spacing,
   bottom aligned with the trending box. Assumes 6 cards / 3 rows. */
.projects-grid {
  flex: 1;
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: repeat(3, 1fr);
  gap: 1.5rem;
}

.card {
  background: var(--surface);
  border-radius: 1.5rem;
  padding: 1.6rem;
  box-shadow: var(--shadow-sm);
}

.project-card {
  border-left: 10px solid var(--pink);
  display: flex; flex-direction: column;
  transition: transform 0.15s, box-shadow 0.15s;
}
.project-card:hover { transform: translateY(-4px); box-shadow: var(--shadow); }
.project-card:nth-child(6n+1) { border-left-color: var(--pink); }
.project-card:nth-child(6n+2) { border-left-color: var(--lavender); }
.project-card:nth-child(6n+3) { border-left-color: var(--mint); }
.project-card:nth-child(6n+4) { border-left-color: var(--butter); }
.project-card:nth-child(6n+5) { border-left-color: var(--peach); }
.project-card:nth-child(6n+6) { border-left-color: var(--sky); }

.project-card h3 { font-weight: 600; margin-bottom: 0.4rem; }
.project-card p { color: var(--muted); flex: 1; line-height: 1.5; }
.card-actions {
  display: flex; justify-content: flex-end; gap: 0.5rem;
  margin-top: 1rem;
}
.action-btn {
  border: none; background: none; cursor: pointer;
  padding: 0.35rem; border-radius: 0.6rem;
  display: grid; place-items: center;
  transition: background 0.15s;
}
.action-btn svg { width: 1.25rem; height: 1.25rem; fill: var(--primary); }
.action-btn:hover { background: var(--pink); }

/* rail fills the main grid row's height; each section grows so its
   card stretches to fill — bottoms align with the project cards,
   no empty gap in between */
.right-rail {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}
.right-rail > section { display: flex; flex-direction: column; flex: 1; }
.right-rail > section .card { flex: 1; }

.announcement h4 { font-size: 0.92rem; font-weight: 700; }
.announcement p { font-size: 0.82rem; color: var(--muted); line-height: 1.45; }
.announcements hr { border: none; border-top: 1px dashed var(--line); margin: 0.8rem 0; }

.trending .card { display: flex; flex-direction: column; gap: 1rem; }
.trending-item { display: flex; align-items: center; gap: 0.75rem; }
.trending-item .handle { font-weight: 800; font-size: 0.9rem; }
.trending-item .muted { color: var(--muted); font-size: 0.8rem; }

/* ============ RESPONSIVE ============ */

/* tablet: rail drops below projects; projects go fluid */
@media (max-width: 1024px) {
  .main-content {
    grid-template-columns: 1fr;   /* single column: projects, then rail */
  }
  .projects-grid {
    grid-template-rows: none;     /* let rows size to content again */
    grid-auto-rows: 1fr;
  }
  .right-rail { flex-direction: row; flex-wrap: wrap; }
  .right-rail > section { flex: 1; min-width: 260px; }
}

/* narrow tablet / large phone: sidebar collapses to icon rail */
@media (max-width: 768px) {
  body { grid-template-columns: 72px 1fr; }
  .branding h1 { display: none; }
  /* hide labels visually but keep them for screen readers */
  .nav-item span {
    position: absolute; width: 1px; height: 1px;
    padding: 0; margin: -1px; overflow: hidden;
    clip: rect(0,0,0,0); white-space: nowrap; border: 0;
  }
  .branding { justify-content: center; }
  .nav-item { justify-content: center; padding: 0.7rem; }
  .header { padding: 1rem 1.25rem; }
  .header-row { flex-wrap: wrap; }
}

/* phone: stack everything; sidebar becomes a top bar */
@media (max-width: 560px) {
  body {
    grid-template-columns: 1fr;
    grid-template-rows: auto auto 1fr;
    grid-template-areas:
      "sidebar"
      "header"
      "main";
  }
  .sidebar { flex-direction: row; flex-wrap: wrap; gap: 0.75rem; align-items: center; }
  .nav-primary, .nav-secondary { flex-direction: row; flex-wrap: wrap; }
  .branding h1 { display: inline; }
  .nav-item span {
    position: absolute; width: 1px; height: 1px;
    padding: 0; margin: -1px; overflow: hidden;
    clip: rect(0,0,0,0); white-space: nowrap; border: 0;
  }
  .projects-grid { grid-template-columns: 1fr; }  /* 1 card per row */
  .header-actions { flex: 1; }
  .btn { flex: 1; justify-content: center; padding: 0.6rem 0.75rem; }
  .main-content { padding: 1.25rem; }
}

/* respect reduced motion */
@media (prefers-reduced-motion: reduce) {
  * { transition: none !important; }
}