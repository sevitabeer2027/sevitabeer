
<html lang="en">
<head>
  <link rel="stylesheet"
href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css">
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Sevita Beer</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --teal-dark: #1a6b6b; --teal-mid: #2a9d8f; --teal-light: #76c8c2;
    --orange: #e07b39; --orange-deep: #b85c28;
    --cream: #faf8f3; --dark: #1a1a1a; --mid: #444; --light-gray: #e8e4dc;
  }
  * { margin:0; padding:0; box-sizing:border-box; }
  html { scroll-behavior:smooth; }
  body { font-family:'DM Sans',sans-serif; background:var(--cream); color:var(--dark); overflow-x:hidden; cursor:none; }
  body.modal-open { overflow:hidden; }
 
  /* CURSOR */
  .cursor { width:12px;height:12px;background:var(--orange);border-radius:50%;position:fixed;pointer-events:none;z-index:9999;transition:transform 0.15s ease;transform:translate(-50%,-50%); }
  .cursor-ring { width:36px;height:36px;border:1.5px solid var(--teal-mid);border-radius:50%;position:fixed;pointer-events:none;z-index:9998;transition:transform 0.35s ease,width 0.3s,height 0.3s;transform:translate(-50%,-50%); }
 
  /* NAV */
  nav {
  position:fixed;
  top:0;
  left:0;
  width:100%;
 
  z-index:200;
  display:flex;
  justify-content:space-between;
  align-items:center;
  padding:1.5rem 4rem;
  background-color:var(--teal-mid);
 
  transform:translateY(-100%);
  transition:transform 0.35s ease;
}
 
nav.visible {
  transform:translateY(0);
}
.nav-logo {
  display:flex;
  align-items:center;
  text-decoration:none;
}
 
.nav-logo img {
  height:48px;
  width:auto;
  display:block;
}
  .nav-links { display:flex;gap:2.5rem;list-style:none; }
  .nav-links a { font-size:0.82rem;letter-spacing:0.12em;text-transform:uppercase;text-decoration:none;color:var(--dark);position:relative;transition:color 0.2s; }
  .nav-links a::after { content:'';position:absolute;bottom:-3px;left:0;width:0;height:1.5px;background:var(--orange);transition:width 0.3s ease; }
  .nav-links a:hover::after { width:100%; }
 
  /* ========== HERO ========== */
  /*
    3 columns × 3 rows.
    Side cols span all 3 rows (tall, dominant).
    Center col: thin top image / text zone / taller bottom image.
    White faded gradient covers the center text zone — no hard box edge.
  */
  .hero {
    position: relative;
    overflow: hidden;
    display: grid;
    grid-template-columns: 1fr 420px 1fr;   /* wide sides, fixed center */
    grid-template-rows: 110px 1fr 260px;     /* thin top, flex middle, tall bottom */
    min-height: 92vh;
  }
 
  /* side images — span all 3 rows */
  .hero-col-left  { grid-column:1; grid-row:1/4; overflow:hidden; cursor:none; position:relative; }
  .hero-col-right { grid-column:3; grid-row:1/4; overflow:hidden; cursor:none; position:relative; }
  /* top image — center column, row 1 */
  .hero-top    { grid-column:2; grid-row:1; overflow:hidden; cursor:none; position:relative; }
  /* text — center column, row 2 — NO hard background, gradient only */
  .hero-center {
    grid-column:2; grid-row:2;
    display:flex; flex-direction:column; align-items:center; justify-content:center;
    text-align:center; padding:2.5rem 2rem;
    position:relative; z-index:3;
    /* no background here — white fade done by pseudo-elements on side/top/bottom cells */
  }
  /* bottom image — center column, row 3 */
  .hero-bottom { grid-column:2; grid-row:3; overflow:hidden; cursor:none; position:relative; }
 
  .hero-col-left img, .hero-col-right img,
  .hero-top img, .hero-bottom img {
    width:100%; height:100%; object-fit:cover;
    filter:saturate(0.78) brightness(0.82);
    transition:transform 0.5s ease, filter 0.4s;
    display:block;
  }
  .hero-col-left:hover img, .hero-col-right:hover img,
  .hero-top:hover img, .hero-bottom:hover img {
    transform:scale(1.04);
    filter:saturate(1) brightness(0.92);
  }
 
  /*
    Gradient overlays so the center text area fades white
    without a hard-edged box. Each side/top/bottom cell gets
    an inward white fade toward the text center.
  */
  .hero-col-left::after {
    content:''; position:absolute; inset:0;
    background: linear-gradient(to right, transparent 55%, rgba(250,248,243,0.92) 100%);
    pointer-events:none; z-index:2;
  }
  .hero-col-right::after {
    content:''; position:absolute; inset:0;
    background: linear-gradient(to left, transparent 55%, rgba(250,248,243,0.92) 100%);
    pointer-events:none; z-index:2;
  }
  .hero-top::after {
    content:''; position:absolute; inset:0;
    background: linear-gradient(to top, rgba(250,248,243,0.95) 0%, transparent 70%);
    pointer-events:none; z-index:2;
  }
  .hero-bottom::after {
    content:''; position:absolute; inset:0;
    background: linear-gradient(to bottom, rgba(250,248,243,0.95) 0%, transparent 55%);
    pointer-events:none; z-index:2;
  }
 
  /* clickable collage cells */
  .collage-clickable { position:relative; }
  .collage-clickable .cc-icon {
    position:absolute; bottom:0.7rem; right:0.8rem;
    font-size:1rem; color:rgba(255,255,255,0.9);
    background:rgba(0,0,0,0.28); border-radius:50%;
    width:28px; height:28px; display:flex; align-items:center; justify-content:center;
    opacity:0; transition:opacity 0.25s;
    pointer-events:none; z-index:4;
  }
  .collage-clickable:hover .cc-icon { opacity:1; }
 
  /* hero text */
  .hero-name { font-family:'Playfair Display',serif;font-size:clamp(3rem,5vw,5rem);line-height:1.05;letter-spacing:-0.02em;color:var(--dark);opacity:0;animation:fadeUp 0.8s 0.4s ease forwards; }
  .hero-name em { font-style:italic;color:var(--teal-dark); }
  .hero-tagline { margin-top:1.2rem;font-size:0.98rem;line-height:1.75;color:var(--mid);max-width:360px;opacity:0;animation:fadeUp 0.8s 0.65s ease forwards; }
  .hero-scroll { margin-top:2rem;display:inline-flex;align-items:center;gap:1rem;opacity:0;animation:fadeUp 0.8s 0.9s ease forwards; }
  .hero-scroll span { font-size:0.75rem;letter-spacing:0.15em;text-transform:uppercase;color:var(--mid); }
  .scroll-line { width:50px;height:1px;background:var(--orange);position:relative;overflow:hidden; }
  .scroll-line::after { content:'';position:absolute;top:0;left:-100%;width:100%;height:100%;background:var(--teal-mid);animation:scanline 2s ease infinite; }
 
  /* ========== ABOUT ========== */
  .philosophy-section { padding:6rem 4rem;background:var(--teal-dark);color:white;position:relative;overflow:hidden; }
  .philosophy-section::before { content:'"';position:absolute;font-family:'Playfair Display',serif;font-size:30rem;color:rgba(255,255,255,0.04);top:-5rem;left:-2rem;line-height:1;pointer-events:none; }
  .philosophy-inner { max-width:700px;position:relative;z-index:1; }
  .phil-label { font-size:0.72rem;letter-spacing:0.2em;text-transform:uppercase;color:var(--teal-light);margin-bottom:1.5rem; }
  .phil-quote { font-family:'Playfair Display',serif;font-size:clamp(1.5rem,3vw,2.4rem);line-height:1.4;margin-bottom:2rem; }
  .phil-quote em { color:var(--orange); }
  .phil-body { line-height:1.85;color:rgba(255,255,255,0.72); }
 
  /* ========== FILTER / PROJECTS ========== */
  .filter-section { padding:5rem 4rem 0; }
  .filter-section h2 { font-family:'Playfair Display',serif;font-size:2rem;margin-bottom:1.5rem;border-bottom:1px solid var(--light-gray);padding-bottom:1.2rem; }
  .filter-buttons { display:flex;flex-wrap:wrap;gap:0.5rem;padding-bottom:2rem; }
  .filter-btn { padding:0.45rem 1.1rem;border-radius:100px;font-size:0.76rem;letter-spacing:0.08em;text-transform:uppercase;border:1px solid var(--light-gray);background:transparent;cursor:none;transition:all 0.25s;color:var(--mid); }
  .filter-btn:hover,.filter-btn.active { background:var(--dark);color:var(--cream);border-color:var(--dark); }
  .filter-btn.active { background:var(--teal-dark);border-color:var(--teal-dark); }
  .projects-grid { padding:0 4rem 6rem;display:grid;grid-template-columns:repeat(3,1fr);gap:1.5rem; }
 
  /* PROJECT CARDS */
  .project-card { position:relative;border-radius:6px;overflow:hidden;background:white;box-shadow:0 2px 20px rgba(0,0,0,0.06);transition:transform 0.35s ease,box-shadow 0.35s ease;cursor:none; }
  .project-card:hover { transform:translateY(-6px);box-shadow:0 12px 40px rgba(0,0,0,0.13); }
  .project-card.wide { grid-column:span 2; }
  .project-thumb { width:100%;aspect-ratio:16/9;background:var(--light-gray);position:relative;overflow:hidden;display:flex;align-items:center;justify-content:center; }
  .project-card.wide .project-thumb { aspect-ratio:21/9; }
  .project-thumb img { width:100%;height:100%;object-fit:cover; }
  .project-thumb::after { content:'';position:absolute;inset:0;background:var(--teal-dark);opacity:0;transition:opacity 0.35s; }
  .project-card:hover .project-thumb::after { opacity:0.7; }
  .project-hover-label { position:absolute;inset:0;display:flex;align-items:center;justify-content:center;color:white;font-family:'Playfair Display',serif;font-size:1rem;font-style:italic;z-index:2;opacity:0;transition:opacity 0.35s; }
  .project-card:hover .project-hover-label { opacity:1; }
  .project-meta { padding:1.4rem 1.6rem 1.6rem; }
  
  .project-summary{
  font-size:1rem;
  line-height:1.8;
  color:var(--mid);

  padding:1rem 1.25rem;
  margin-bottom:2rem;

  background:rgba(42,157,143,.06);
  border-left:4px solid var(--teal-mid);
  border-radius:6px;
}

  .project-tags { display:flex;gap:0.4rem;flex-wrap:wrap;margin-bottom:0.7rem; }
  .project-tag { font-size:0.65rem;letter-spacing:0.1em;text-transform:uppercase;padding:0.2rem 0.6rem;border-radius:100px;background:var(--cream);color:var(--teal-dark);border:1px solid var(--teal-light); }
  .project-tag.eng { color:#4a6fa5;border-color:#a5b8d9;background:#f0f4fa; }
  .project-tag.art { color:#a54a6f;border-color:#d9a5b8;background:#faf0f4; }
  .project-tag.coming { color:#888;border-color:#ccc;background:#f5f5f5; }
  .project-tag.des { color:#ED5FAB;border-color:#F8BADB;background:#FCF3F9; }
  .project-tag.res { color:#E67A37;border-color:#F6D5C1;background:#F9F0E6; }
  .project-tag.comm { color:#8A5FE7;border-color:#CAB6F6;background:#E7E6FA; }
  .project-tag.projman { color:#56D2CE;border-color:#B8EAE8;background:#E6F9F6; }
  .project-title { font-family:'Playfair Display',serif;font-size:1.1rem;line-height:1.3;margin-bottom:0.5rem; }
  .project-desc { font-size:0.83rem;color:var(--mid);line-height:1.6; }
  .project-org { font-size:0.72rem;letter-spacing:0.08em;color:#aaa;margin-top:0.7rem;text-transform:uppercase; }
  .coming-soon-badge { position:absolute;top:1rem;right:1rem;background:rgba(255,255,255,0.9);font-size:0.65rem;letter-spacing:0.15em;text-transform:uppercase;padding:0.3rem 0.7rem;border-radius:100px;color:var(--mid);z-index:3; }
 
  /* ========== INVOLVEMENTS — text-only cards ========== */
  .involvements-section { padding:5rem 4rem 6rem;background:white; }
  .involvements-section h2 { font-family:'Playfair Display',serif;font-size:2rem;margin-bottom:1.5rem;border-bottom:1px solid var(--light-gray);padding-bottom:1.2rem; }
  .involvements-grid { display:grid;grid-template-columns:repeat(3,1fr);gap:1.5rem; }
  .inv-card {
    padding:1.8rem; border:1px solid var(--light-gray); border-radius:6px;
    background:var(--cream); cursor:none;
    transition:border-color 0.3s, box-shadow 0.3s, transform 0.3s;
    display:flex; flex-direction:column;
  }
  .inv-card:hover { border-color:var(--teal-light);box-shadow:0 8px 30px rgba(0,0,0,0.07);transform:translateY(-4px); }
  .inv-category { font-size:0.62rem;letter-spacing:0.2em;text-transform:uppercase;margin-bottom:0.8rem;font-weight:500; }
  .inv-category.cat-club { color:var(--teal-dark); }
  .inv-category.cat-volunteer { color:var(--orange-deep); }
  .inv-category.cat-art { color:#a54a6f; }
  .inv-title { font-family:'Playfair Display',serif;font-size:1.05rem;line-height:1.3;margin-bottom:0.4rem;color:var(--dark); }
  .inv-role { font-size:0.75rem;letter-spacing:0.07em;color:var(--mid);text-transform:uppercase;margin-bottom:0.7rem; }
  .inv-desc { font-size:0.83rem;line-height:1.65;color:#777; }
  .inv-view-more { margin-top:1rem;font-size:0.75rem;letter-spacing:0.08em;color:var(--teal-mid);text-transform:uppercase; }
 
  /* ========== SHARED MODAL ========== */
  .modal-overlay { display:none;position:fixed;inset:0;background:rgba(0,0,0,0.6);backdrop-filter:blur(4px);z-index:500;align-items:center;justify-content:center;padding:2rem; }
  .modal-overlay.open { display:flex; }
  .modal { background:var(--cream);border-radius:8px;max-width:780px;width:100%;max-height:92vh;overflow-y:auto;animation:modalIn 0.35s ease; }
  .modal-wrap { position:relative; }
  .modal-close { position:absolute;top:1rem;right:1rem;background:white;border:none;width:36px;height:36px;border-radius:50%;font-size:1.1rem;cursor:none;display:flex;align-items:center;justify-content:center;box-shadow:0 2px 10px rgba(0,0,0,0.1);z-index:10; }
 
  .modal-body { padding:2.5rem; }
  .modal-tags { display:flex;gap:0.4rem;flex-wrap:wrap;margin-bottom:1rem; }
  .modal-title { font-family:'Playfair Display',serif;font-size:1.8rem;line-height:1.2;margin-bottom:1.5rem; }
  .modal-sections { display:flex;flex-direction:column; }
  .modal-section { border-top:1px solid var(--light-gray);padding:1.4rem 0; }
  .modal-section:last-child { border-bottom:1px solid var(--light-gray); }
  .modal-section-label { font-size:0.65rem;letter-spacing:0.18em;text-transform:uppercase;color:var(--teal-dark);margin-bottom:0.6rem;font-weight:500; }
  .modal-section-content { font-size:0.9rem;line-height:1.75;color:var(--mid); }
  .modal-skills-list { display:flex;flex-wrap:wrap;gap:0.4rem;margin-top:0.2rem; }
  .modal-skill-tag { font-size:0.68rem;letter-spacing:0.07em;text-transform:uppercase;padding:0.28rem 0.7rem;border-radius:100px;background:var(--teal-dark);color:white; }
 
  /* ========== CAROUSEL (shared) ========== */
  .modal-carousel {
    width:100%; position:relative;
    background:var(--dark);
    border-radius:0 0 8px 8px;
  }
  .modal-carousel.top { border-radius:8px 8px 0 0; }
  .carousel-viewport { aspect-ratio:16/9; position:relative; overflow:hidden; }
  .carousel-slides { display:flex;width:100%;height:100%;transition:transform 0.4s ease; }
  .carousel-slide {
    min-width:100%;height:100%;
    display:flex;align-items:center;justify-content:center;
    background:var(--light-gray);
    font-size:0.75rem;letter-spacing:0.12em;text-transform:uppercase;color:#888;
  }
  .carousel-slide img { width:100%;height:100%;object-fit:contain;background:var(--dark); }
  .carousel-btn { position:absolute;top:50%;transform:translateY(-50%);background:rgba(255,255,255,0.85);border:none;width:38px;height:38px;border-radius:50%;font-size:1rem;cursor:none;display:flex;align-items:center;justify-content:center;box-shadow:0 2px 10px rgba(0,0,0,0.12);transition:background 0.2s;z-index:2; }
  .carousel-btn:hover { background:white; }
  .carousel-prev { left:1rem; }
  .carousel-next { right:1rem; }
  .carousel-dots { position:absolute;bottom:0.8rem;left:50%;transform:translateX(-50%);display:flex;gap:6px;z-index:3; }
  .carousel-dot { width:6px;height:6px;border-radius:50%;background:rgba(255,255,255,0.45);cursor:none;border:none;transition:background 0.2s,transform 0.2s; }
  .carousel-dot.active { background:white;transform:scale(1.3); }
  /* Caption bar below the image viewport, inside the dark carousel wrapper */
  .carousel-caption {
    padding:0.65rem 3.5rem; /* side padding to not overlap buttons */
    min-height:2.4rem;
    text-align:center;
    font-size:0.78rem;
    color:rgba(255,255,255,0.65);
    letter-spacing:0.04em;
    line-height:1.4;
    border-top:1px solid rgba(255,255,255,0.08);
  }
 
  /* ========== LIGHTBOX (collage popups) ========== */
  .lightbox-overlay {
    display:none;position:fixed;inset:0;
    background:rgba(0,0,0,0.88);
    backdrop-filter:blur(6px);
    z-index:600;
    align-items:center;justify-content:center;
    flex-direction:column;
    padding:2rem;
  }
  .lightbox-overlay.open { display:flex; }
  .lightbox-img { max-width:85vw;max-height:78vh;object-fit:contain;border-radius:4px;box-shadow:0 8px 60px rgba(0,0,0,0.5); }
  .lightbox-caption { margin-top:1rem;color:rgba(255,255,255,0.72);font-size:0.88rem;letter-spacing:0.04em;text-align:center;max-width:600px;line-height:1.6; }
  .lightbox-close { position:absolute;top:1.2rem;right:1.5rem;background:rgba(255,255,255,0.15);border:none;color:white;width:38px;height:38px;border-radius:50%;font-size:1.1rem;cursor:none;display:flex;align-items:center;justify-content:center;transition:background 0.2s; }
  .lightbox-close:hover { background:rgba(255,255,255,0.3); }
 
  /* ========== FOOTER ========== */
  footer { background:var(--dark);color:var(--cream);padding:6rem 4rem 4rem; }
  .footer-top { display:grid;grid-template-columns:1fr 1fr;gap:4rem;margin-bottom:5rem; }
  .footer-headline { font-family:'Playfair Display',serif;font-size:clamp(2rem,4vw,3.5rem);line-height:1.1; }
  .footer-headline em { color:var(--orange);font-style:italic; }
  .footer-contact-list { margin-top:2rem;display:flex;flex-direction:column;gap:1rem; }
  .contact-item { display:flex;gap:1.2rem;align-items:flex-start; }
  .contact-icon { font-size:1rem;margin-top:2px; }
  .contact-label { font-size:0.72rem;letter-spacing:0.12em;text-transform:uppercase;color:#888;margin-bottom:0.2rem; }
  .contact-value { font-size:0.92rem;color:var(--cream);text-decoration:none; }
  .contact-value:hover { color:var(--orange); }
  .footer-right { padding-top:0.5rem; }
  .footer-right p { line-height:1.8;color:#888;margin-bottom:1.5rem; }
  .cta-btn { display:inline-block;padding:0.9rem 2.2rem;background:var(--orange);color:white;border-radius:2px;font-size:0.82rem;letter-spacing:0.1em;text-transform:uppercase;text-decoration:none;transition:background 0.25s;cursor:none; }
  .cta-btn:hover { background:var(--orange-deep); }
  .footer-bottom { border-top:1px solid #2a2a2a;padding-top:2rem;display:flex;justify-content:space-between;align-items:center;font-size:0.75rem;color:#555; }
  .footer-logo{
  display:block;
  margin-top:3rem;
  margin-bottom:-1rem;
}
  .th-1{background:#c8ddd9;}.th-2{background:#d4e3c8;}.th-3{background:#e8d5c8;}
  .th-4{background:#c8cfe8;}.th-5{background:#e0c8e8;}.th-6{background:#e8e0c8;}
  .th-7{background:#c8e8d9;}.th-8{background:#e8c8c8;}
 
  @keyframes fadeUp { from{opacity:0;transform:translateY(20px);}to{opacity:1;transform:translateY(0);} }
  @keyframes fadeIn { from{opacity:0;}to{opacity:1;} }
  @keyframes scanline { 0%{left:-100%;}100%{left:100%;} }
  @keyframes modalIn { from{opacity:0;transform:scale(0.95) translateY(10px);}to{opacity:1;transform:scale(1) translateY(0);} }
 
  .about-grid{
  display:grid;
  grid-template-columns: 2fr 1fr;
  gap:4rem;
  align-items:start;
}
 
.skills-panel{
  background:rgba(255,255,255,0.06);
  border:1px solid rgba(255,255,255,0.12);
  padding:2rem;
  border-radius:8px;
  backdrop-filter:blur(4px);
}
 
.skills-panel h3{
  font-family:'Playfair Display',serif;
  margin-bottom:1.5rem;
  color:white;
}
 
.skill-item{
  display:flex;
  align-items:center;
  gap:1rem;
  margin-bottom:1.2rem;
}
 
.skill-item img{
  width:34px;
  height:34px;
  object-fit:contain;
  filter:brightness(0) invert(1);
}
 
.skill-item span{
  color:rgba(255,255,255,0.85);
  font-size:0.95rem;
}
 
.about-gallery{
  margin-top:2rem;

  display:grid;
  grid-template-columns:repeat(9,1fr);

  gap:8px;

  max-width:1750px; /* adjust if needed */
}

.about-gallery-item{
  position:relative;
  overflow:hidden;
  border-radius:6px;
}

.about-gallery img{
  width:100%;
  aspect-ratio:1/1;
  object-fit:cover;
  display:block;
  transition:transform .25s ease;
}

.about-gallery-item:hover img{
  transform:scale(1.06);
}

.about-gallery-label{
  position:absolute;
  inset:0;

  display:flex;
  align-items:center;
  justify-content:center;

  background:rgba(0,0,0,.55);

  color:white;
  font-size:.65rem;
  letter-spacing:.08em;
  text-transform:uppercase;

  opacity:0;
  transition:opacity .2s ease;
}

.about-gallery-item:hover .about-gallery-label{
  opacity:1;
}
@media (max-width:900px){
  .about-grid{
    grid-template-columns:1fr;
  }
}
 
/* ── FLOWCHART ───────────────────────────────────────── */

.flowchart-wrap{
  margin-top:1.2rem;
}

.flowchart{
  display:flex;
  flex-wrap:wrap;
  justify-content:center;
  align-items:center;
  gap:10px;
  width:100%;
}

.fc-node{
  color:white;
  border-radius:6px;
  padding:0.55rem 0.85rem;
  font-size:0.72rem;
  line-height:1.35;
  text-align:center;

  flex:1 1 140px;
  min-width:120px;
  max-width:180px;

  transition:filter .18s, transform .18s;
}

.fc-node:hover{
  filter:brightness(1.18);
  transform:translateY(-2px);
}

.fc-num{
  display:block;
  font-size:.54rem;
  letter-spacing:.14em;
  opacity:.62;
  margin-bottom:.18rem;
  text-transform:uppercase;
}

.fc-arrow{
  color:var(--orange);
  font-size:1rem;
  flex-shrink:0;
}

.fc-node.c0{background:#1a6b6b;}
.fc-node.c1{background:#1f7a72;}
.fc-node.c2{background:#b85c28;}
.fc-node.c3{background:#c96930;}
 
</style>
</head>
<body>
 
<div class="cursor" id="cursor"></div>
<div class="cursor-ring" id="cursorRing"></div>
 
<!-- NAV -->
<nav>
  <a href="#" class="nav-logo">
    <img src="sblogo.PNG" alt="Sevita Beer Logo">
  </a>
  <ul class="nav-links">
    <li><a href="#about">About</a></li>
    <li><a href="#projects">Work</a></li>
    <li><a href="#involvements">Involvements</a></li>
    <li><a href="#contact">Contact</a></li>
  </ul>
</nav>
 
<!-- ===== HERO ===== -->
<!--
  Grid:  [left-col] [center] [right-col]
         tall image  top-img  tall image
                     TEXT
                     bot-img
 
  To add captions for lightbox popups, set data-caption="..." on each clickable cell.
  Replace src values with your image filenames.
-->
<section class="hero">
 
  <!-- Left tall image -->
  <div class="hero-col-left collage-clickable" onclick="openLightbox('AL1alt.png','[Aluminum casting lab]')">
    <img src="AL1alt.png" alt="Simply-Sci illustration">
  </div>
 
  <!-- Center top image -->
  <div class="hero-top collage-clickable" onclick="openLightbox('DTC1.jpg','[Design Thinking & Communication Project]')">
    <img src="DTC1.jpg" alt="EWB site visit">
  </div>
 
  <!-- Center text — sits between top and bottom images, full white bg -->
  <div class="hero-center">
    <h1 class="hero-name"><em>Sevita<br>Beer</em></h1>
    <p class="hero-tagline">I am a thinker, builder, and leader. I am dedicated to bridging people, communities, and solutions using engineering.</p>
    <div class="hero-scroll">
      <span>Scroll to explore</span>
      <div class="scroll-line"></div>
    </div>
  </div>
 
  <!-- Center bottom image -->
  <div class="hero-bottom collage-clickable" onclick="openLightbox('SS1.jpg','[Simply-Sci Illustration]')">
    <img src="SS1.jpg" alt="Aluminum casting lab">
  </div>
 
  <!-- Right tall image -->
  <div class="hero-col-right collage-clickable" onclick="openLightbox('EWBcover.jpg','[Engineers Without Borders site visit]')">
    <img src="EWBcover.JPG" alt="DFA user research">
  </div>
 
</section>

 
<!-- LIGHTBOX -->
<div class="lightbox-overlay" id="lightboxOverlay" onclick="closeLightboxClick(event)">
  <button class="lightbox-close" onclick="closeLightbox()">✕</button>
  <img class="lightbox-img" id="lightboxImg" src="" alt="">
  <p class="lightbox-caption" id="lightboxCaption"></p>
</div>
 
<!-- ABOUT -->
<section class="philosophy-section" id="about">
  <div class="about-grid">
 
    <div class="philosophy-inner">
      <p class="phil-label">About Me</p>
      <h2 class="phil-quote"><em>Hello!</em> My name is Sevita :)</h2>
 
      <p class="phil-body">
        As a growing engineer, I'm learning the hard skills to rework <b>processes and technology</b> and the soft skills to make those solutions accessible to all. I believe that the end goal of engineering and design is to <b>improve lives</b>, and it doesn't hurt to <b>make the world a better place</b> along the way.
 
        <br><br>
 
        My major is <b>chemical engineering</b> but I'm minoring in <b>materials science</b> and pursuing the <b>Segal design certificate.</b> I'm heavily involved in student leadership and the Evanston community. I enjoy leading people and projects, and I find that strong technical skills must be supplemented by strong interpersonal skills for projects to really take off. I'm interested in exploring material properties, processing, and scaleup in sustainable R&D applications.
 
        <br><br>
 
        When I'm not in the library or the lab, you can find me painting, enjoying the sunshine, or getting good food with good friends.
      </p>
    </div>
 
    <div class="skills-panel">
 
      <h3>Skills</h3>
 
      <div class="skill-item">
        <i class="fa-solid fa-microscope"></i>
        <span>Materials Characterization</span>
      </div>
 
      <div class="skill-item">
        <i class="fa-solid fa-chart-line"></i>
        <span>Data Analysis & Modelling</span>
      </div>
 
      <div class="skill-item">
        <i class="fa-solid fa-flask"></i>
        <span>Experimental Design</span>
      </div>
 
      <div class="skill-item">
        <i class="fa-solid fa-gear"></i>
        <span>Process Engineering</span>
      </div>
      
      <div class="skill-item">
        <i class="fa-solid fa-user"></i>
        <span>Leadership</span>
      </div>
 
      <br> <br>
 
      <h3>Course Highlights</h3>
      <div class="skill-item">
        <i class="fa-solid fa-compass-drafting"></i>
        <span>Chemical Product Design</span>
      </div>
 
      <div class="skill-item">
        <i class="fa-solid fa-robot"></i>
        <span>Machine Learning: Materials Informatics</span>
      </div>
 
      <div class="skill-item">
        <i class="fa-solid fa-atom"></i>
        <span>Thermodynamics; Fluid Mechanics; Heat & Mass Transfer</span>
      </div>
 
    </div>
 
  </div>

  <div class="about-gallery">

    <div class="about-gallery-item">
      <img src="gal1.jpg">
      <div class="about-gallery-label">my henna</div>
    </div>
  
    <div class="about-gallery-item">
      <img src="gal2.jpg">
      <div class="about-gallery-label">fish charcuterie</div>
    </div>
  
    <div class="about-gallery-item">
      <img src="gal3.jpg">
      <div class="about-gallery-label">pov: first sips cafe watercolor</div>
    </div>
  
    <div class="about-gallery-item">
      <img src="gal4.jpg">
      <div class="about-gallery-label">backyard bbq</div>
    </div>
  
    <div class="about-gallery-item">
      <img src="gal5.jpg">
      <div class="about-gallery-label">tiramasu with friends</div>
    </div>
  
    <div class="about-gallery-item">
      <img src="gal6.jpg">
      <div class="about-gallery-label">chicagooo</div>
    </div>
  
    <div class="about-gallery-item">
      <img src="gal7.jpg">
      <div class="about-gallery-label">the best hot dogs</div>
    </div>
  
    <div class="about-gallery-item">
      <img src="gal8.jpg">
      <div class="about-gallery-label">i love cats</div>
    </div>
  
    <div class="about-gallery-item">
      <img src="gal9.jpg">
      <div class="about-gallery-label">indiana dunes</div>
    </div>
  
    <div class="about-gallery-item">
      <img src="gal10.jpg">
      <div class="about-gallery-label">yummyy</div>
    </div>
  
    <div class="about-gallery-item">
      <img src="gal11.jpg">
      <div class="about-gallery-label">kingston mines blues bar</div>
    </div>
  
    <div class="about-gallery-item">
      <img src="gal17.jpg">
      <div class="about-gallery-label">my cat, patches <3</div>
    </div>

    <div class="about-gallery-item">
      <img src="gal13.jpg">
      <div class="about-gallery-label">yum yum yum</div>
    </div>

    <div class="about-gallery-item">
      <img src="gal14.jpg">
      <div class="about-gallery-label">indiana dunes</div>
    </div>

    <div class="about-gallery-item">
      <img src="gal15.jpg">
      <div class="about-gallery-label">museum</div>
    </div>

    <div class="about-gallery-item">
      <img src="gal16.jpg">
      <div class="about-gallery-label">go cubs!</div>
    </div>

    <div class="about-gallery-item">
      <img src="gal12.jpg">
      <div class="about-gallery-label">bffs henna</div>
    </div>

    <div class="about-gallery-item">
      <img src="gal18.jpg">
      <div class="about-gallery-label">my fav smiski</div>
    </div>
  
  </div>
</section>
 
<!-- PROJECTS -->
<section class="filter-section" id="projects">
  <h2>Selected Work</h2>
  <div class="filter-buttons">
    <button class="filter-btn active" onclick="filterProjects('all',this)">All</button>
    <button class="filter-btn" onclick="filterProjects('research',this)">Laboratory</button>
    <button class="filter-btn" onclick="filterProjects('engineering',this)">Analysis</button>
    <button class="filter-btn" onclick="filterProjects('projman',this)">Project Management</button>
    <button class="filter-btn" onclick="filterProjects('product',this)">Product Dev.</button>
    <button class="filter-btn" onclick="filterProjects('community',this)">Art</button>
  </div>
</section>
 
<div class="projects-grid" id="projectsGrid">
 
  <div class="project-card wide" data-tags="research" onclick="openProjectModal('bimetallic')">
    <div class="project-thumb th-2">
      <img src="lab1.png" style="width:100%;height:100%;object-fit:cover;">
      <div class="project-hover-label">View Project →</div>
    </div>
    <div class="project-meta">
      <div class="project-tags"><span class="project-tag res">Laboratory</span></div>
      <h3 class="project-title">Bimetallic Cathode Investigation</h3>
      <p class="project-desc">Investigating alloyed platinum cathodes in solid acid fuel cells to reduce precious metal usage in novel clean energy generation methods.</p>
      <p class="project-org">Haile Group</p>
    </div>
  </div>
 
  <div class="project-card" data-tags="product" onclick="openProjectModal('breakfast')">
    <div class="project-thumb th-5">
      <img src="SQ1.png" style="width:100%;height:100%;object-fit:cover;">
      <div class="project-hover-label">View Project →</div>
    </div>
    <div class="project-meta">
      <div class="project-tags"><span class="project-tag">Product Development</span></div>
      <p class="project-desc">Developing a novel breakfast product from concept to prototype — balancing nutritional science, sensory design, and market viability.</p>
      <p class="project-org">Food Science Lab</p>
    </div>
  </div>
 
  <div class="project-card" data-tags="engineering" onclick="openProjectModal('matsci')">
    <div class="project-thumb th-4">
      <img src="ML1.png" style="width:100%;height:100%;object-fit:cover;">
      <div class="project-hover-label">View Project →</div>
    </div>
    <div class="project-meta">
      <div class="project-tags"><span class="project-tag eng">Analysis</span></div>
      <h3 class="project-title">Machine Learning for Materials Science</h3>
      <p class="project-desc">Applying ML methods to predict materials properties and accelerate experimental discovery in a research setting.</p>
      <p class="project-org">Materials Science Research</p>
    </div>
  </div>
 
  <div class="project-card" data-tags="engineering" onclick="openProjectModal('lithium')">
    <div class="project-thumb th-1">
      <img src="DLE1.png" style="width:100%;height:100%;object-fit:cover;">
      <div class="project-hover-label">View Project →</div>
    </div>
    <div class="project-meta">
      <div class="project-tags"><span class="project-tag eng">Analysis</span></div>
      <h3 class="project-title">Direct Lithium Extraction — Technical Recommendation</h3>
      <p class="project-desc">A comprehensive technical analysis and recommendation report on DLE technologies for sustainable battery material sourcing.</p>
      <p class="project-org">Engineering Capstone</p>
    </div>
  </div>
 
  <div class="project-card" data-tags="research" onclick="openProjectModal('casting')">
    <div class="project-thumb th-3">
      <img src="AL1.jpeg" style="width:100%;height:100%;object-fit:cover;">
      <div class="project-hover-label">View Project →</div>
    </div>
    <div class="project-meta">
      <div class="project-tags"><span class="project-tag res">Laboratory</span></div>
      <h3 class="project-title">Aluminum Casting Lab</h3>
      <p class="project-desc">Hands-on manufacturing lab exploring casting parameters, microstructure, and mechanical property relationships in aluminum alloys.</p>
      <p class="project-org">Materials Engineering Lab</p>
    </div>
  </div>
 
  <div class="project-card" data-tags="projman" onclick="openProjectModal('dfa')">
    <div class="project-thumb th-7">
      <img src="DFA1.png" style="width:100%;height:100%;object-fit:cover;">
      <div class="project-hover-label">View Project →</div>
    </div>
    <div class="project-meta">
      <div class="project-tags"><span class="project-tag projman">Project Management</span></div>
      <h3 class="project-title">English Accessibility — User-Facing Design</h3>
      <p class="project-desc">Designing an accessible, user-centered interface to lower language barriers for English learners in partnership with community organizations.</p>
      <p class="project-org">Design for America (DFA)</p>
    </div>
  </div>
 
  <div class="project-card wide" data-tags="projman" onclick="openProjectModal('ewb')">
    <div class="project-thumb th-6">
      <img src="EWB1.jpeg" style="width:100%;height:100%;object-fit:cover;">
      <div class="project-hover-label">View Project →</div>
    </div>
    <div class="project-meta">
      <div class="project-tags"><span class="project-tag projman">Project Management</span><span class="project-tag eng">Analysis</span></div>
      <h3 class="project-title">Sustainable School Construction — EWB</h3>
      <p class="project-desc">Site work and community engagement for a sustainable school building project, developing communication and cross-cultural collaboration skills.</p>
      <p class="project-org">Engineers Without Borders</p>
    </div>
  </div>

  <div class="project-card wide" data-tags="community" onclick="openProjectModal('simplysci')">
    <div class="project-thumb th-6">
      <img src="SS1.jpg" style="width:100%;height:100%;object-fit:cover;">
      <div class="project-hover-label">View Project →</div>
    </div>
    <div class="project-meta">
      <div class="project-tags"><span class="project-tag comm">Art</span></div>
      <h3 class="project-title">Simply-Sci Science Newsletters</h3>
      <p class="project-desc">Designing newsletter pages' layout and illustrations to publicize recent scientfic research in a legible and engaging manner.</p>
      <p class="project-org">Simply-Sci</p>
    </div>
  </div>
 
  <div class="project-card" data-tags="projman, product, community" onclick="openProjectModal('msab')">
    <!-- <span class="coming-soon-badge">Coming Soon</span> !-->
    <div class="project-thumb th-8" style="opacity:0.6">
      <img src="MSAB2.png" style="width:100%;height:100%;object-fit:cover;">
      <div class="project-hover-label">View Project →</div>
    </div>
    <div class="project-meta">
      <div class="project-tags"><span class="project-tag comm">Art</span><span class="project-tag projman">Project Management</span><span class="project-tag">Product Development</span></div>
      <h3 class="project-title">E-Week Event Production &amp; Merchandise</h3>
      <p class="project-desc">Project management and event production for Engineering Week, bringing together hundreds of students and industry professionals.</p>
      <p class="project-org">Materials Science Advisory Board</p>
    </div>
  </div>
 
</div>
 
<!-- PROJECT MODAL (text first, image slideshow at bottom) -->
<div class="modal-overlay" id="projectModalOverlay" onclick="handleProjectOverlayClick(event)">
  <div class="modal-wrap">
    <button class="modal-close" onclick="closeProjectModal()">✕</button>
    <div class="modal">
      <div class="modal-body">
        <div class="modal-tags" id="pModalTags"></div>
        <h2 class="modal-title" id="pModalTitle"></h2>
        <div class="project-summary" id="pModalSummary"></div>
        <div class="modal-sections">
          <div class="modal-section"><div class="modal-section-label">Skills</div><div class="modal-skills-list" id="pModalSkills"></div></div>
          <div class="modal-section"><div class="modal-section-label">My Role</div><div class="modal-section-content" id="pModalRole"></div></div>
          <div class="modal-section"><div class="modal-section-label">Thinking &amp; Approach</div><div class="modal-section-content" id="pModalThinking"></div></div>
          <div class="modal-section"><div class="modal-section-label">Methodology</div>
            <div class="flowchart-wrap" id="pFlowchartWrap" style="display:none;"><div class="flowchart" id="pFlowchart"></div></div>
            <div class="modal-section-content" id="pModalMethodology"></div>
          </div>
          <div class="modal-section"><div class="modal-section-label">Solution &amp; Outcomes</div><div class="modal-section-content" id="pModalSolution"></div></div>
        </div>
      </div>
      <!-- Image slideshow at bottom -->
      <div class="modal-carousel" id="pCarouselWrap">
        <div class="carousel-viewport">
          <div class="carousel-slides" id="pCarouselSlides"></div>
          <button class="carousel-btn carousel-prev" onclick="pMove(-1)">‹</button>
          <button class="carousel-btn carousel-next" onclick="pMove(1)">›</button>
          <div class="carousel-dots" id="pCarouselDots"></div>
        </div>
        <div class="carousel-caption" id="pCaption"></div>
      </div>
    </div>
  </div>
</div>
 
<!-- INVOLVEMENTS — text-only cards -->
<section class="involvements-section" id="involvements">
  <h2>Other Involvements</h2>
  <div class="involvements-grid">
 
    <div class="inv-card" onclick="openInvModal('msab_inv')">
      <div class="inv-category cat-club">Club — Leadership</div>
      <h3 class="inv-title">McCormick Student Advisory Board</h3>
      <div class="inv-role">Co-President</div>
      <p class="inv-desc">Overseeing three committees and managing a $160K programming budget. Coordinated with 24 campus organizations to deliver professional, social, and networking opportunities for more than 500 engineering students. Focused on <b>building community</b> and strengthening connections <b>between students, faculty, and industry partners.</b></p>
      <div class="inv-view-more">View photos →</div>
    </div>
 
    <div class="inv-card" onclick="openInvModal('ewb_inv')">
      <div class="inv-category cat-volunteer">Project — Leadership</div>
      <h3 class="inv-title">Engineers Without Borders</h3>
      <div class="inv-role">International Team Lead</div>
      <p class="inv-desc">Led an international <b>field expedition</b> to Uganda in collaboration with NGOs and local officials to support the <b>sustainable construction</b> of a school serving more than 300 students, serving as communication lead in user interviews. Upon return, developed and led Revit modeling workshops for a team of 20 students.</p>
      <div class="inv-view-more">View photos →</div>
    </div>
 
    <div class="inv-card" onclick="openInvModal('simplysci_inv')">
      <div class="inv-category cat-art">Design — Leadership</div>
      <h3 class="inv-title">Simply-Sci</h3>
      <div class="inv-role">Design Lead</div>
      <p class="inv-desc">Serve as Design Lead for Simply-Sci, a <b>science literacy startup</b> dedicated to making current research accessible to broader audiences. Lead the <b>visual communication and design strategy</b> for monthly newsletters that distill complex academic research into clear, engaging explanations supported by original graphics.</p>
      <div class="inv-view-more">View photos →</div>
    </div>
 
    <div class="inv-card" onclick="openInvModal('dfa_inv')">
      <div class="inv-category cat-volunteer">Project — Leadership</div>
      <h3 class="inv-title">Design for America</h3>
      <div class="inv-role">Whitespace Project Lead</div>
      <p class="inv-desc">Initiated a project to improve immigrant communities' access to public resources through <b>human-centered design</b> in collaboration with the Chinese Mutual Aid Association in Chicago. Secured a community client through extensive <b>outreach</b> and led cross-stakeholder <b>user research</b> to identify barriers to accessing services.</p>
      <div class="inv-view-more">View photos →</div>
    </div>
 
    <div class="inv-card" onclick="openInvModal('art_inv')">
      <div class="inv-category cat-art">Art — Independent</div>
      <h3 class="inv-title">Studio Art Practice</h3>
      <div class="inv-role">Independent Artist</div>
      <p class="inv-desc">Ongoing <b>personal art</b> practice exploring the intersection of science, nature, and visual storytelling through illustration and mixed media. Private <b>henna/mehndi artist</b>, often doing events with Northwestern's premier South Asian student club.</p>
      <div class="inv-view-more">View photos →</div>
    </div>
 
    <div class="inv-card" onclick="openInvModal('volunteer_inv')">
      <div class="inv-category cat-volunteer">Volunteering</div>
      <h3 class="inv-title">Community Engagement</h3>
      <div class="inv-role">Volunteer</div>
      <p class="inv-desc">Volunteering with <b>Evanston youth</b>; active in an after-school <b>Book Buddies</b> reading program, <b>Society of Women Engineers</b> outreach to local middle &amp; high schoolers.</p>
      <div class="inv-view-more">View photos →</div>
    </div>
 
  </div>
</section>
 
<!-- INVOLVEMENT MODAL — slideshow only -->
<div class="modal-overlay" id="invModalOverlay" onclick="handleInvOverlayClick(event)">
  <div class="modal-wrap">
    <button class="modal-close" onclick="closeInvModal()">✕</button>
    <div class="modal" style="background:var(--dark);">
      <div class="modal-carousel top" style="border-radius:8px;" id="iCarouselWrap">
        <div class="carousel-viewport">
          <div class="carousel-slides" id="iCarouselSlides"></div>
          <button class="carousel-btn carousel-prev" onclick="iMove(-1)">‹</button>
          <button class="carousel-btn carousel-next" onclick="iMove(1)">›</button>
          <div class="carousel-dots" id="iCarouselDots"></div>
        </div>
        <div class="carousel-caption" id="iCaption"></div>
      </div>
    </div>
  </div>
</div>
 
<!-- FOOTER -->
<footer id="contact">
  <div class="footer-top">
    <div><h2 class="footer-headline"><em>Let's chat!</em></h2>
      <a href="#">
        <img src="sblogo.PNG"
     alt="Sevita Beer Logo"
     width="200"
     height="200"
     class="footer-logo">
      </a></div>
    <div class="footer-right">
      <p>I'm currently open to internships and collaborative projects in product development, R&amp;D, and human-centered design. I'd love to hear from you.</p>
      <a href="mailto:sevitabeer2027@u.northwestern.edu" class="cta-btn">Say Hello →</a>
      <div class="footer-contact-list" style="margin-top:2rem;">
        <div class="contact-item">
          <div class="contact-icon">✉</div>
          <div><div class="contact-label">Email</div><a class="contact-value" href="mailto:sevitabeer2027@u.northwestern.edu">sevitabeer2027@u.northwestern.edu</a></div>
        </div>
        <div class="contact-item">
          <div class="contact-icon">in</div>
          <div><div class="contact-label">LinkedIn</div><a class="contact-value"
          href="https://www.linkedin.com/in/sevita-beer-2645b4316/"
          target="_blank"
          rel="noopener noreferrer">
          linkedin.com/in/sevita-beer
       </a>
        </div>
        <div class="contact-item">
          <div class="contact-icon">📍</div>
          <div><div class="contact-label">Location</div><span class="contact-value">Evanston, IL | Philadelpia, PA</span></div>
        </div>
      </div>
    </div>
  </div>
  <div class="footer-bottom">
    <span>© 2025 Sevita Beer. DSGN 370 Portfolio.</span>
    <span style="color:#333">Design Thinker &amp; Doer</span>
  </div>
 
<script>
// ── CURSOR ──────────────────────────────────────────────
const cursor = document.getElementById('cursor');
const ring   = document.getElementById('cursorRing');
document.addEventListener('mousemove', e => {
  cursor.style.left = e.clientX+'px'; cursor.style.top = e.clientY+'px';
  setTimeout(()=>{ ring.style.left = e.clientX+'px'; ring.style.top = e.clientY+'px'; }, 80);
});
document.querySelectorAll('a,button,.project-card,.filter-btn,.inv-card,.collage-clickable').forEach(el=>{
  el.addEventListener('mouseenter',()=>{ ring.style.width='60px';ring.style.height='60px';cursor.style.transform='translate(-50%,-50%) scale(0.5)'; });
  el.addEventListener('mouseleave',()=>{ ring.style.width='36px';ring.style.height='36px';cursor.style.transform='translate(-50%,-50%) scale(1)'; });
});
 
// ── SCROLL LOCK ─────────────────────────────────────────
function lockScroll()   { document.body.classList.add('modal-open'); }
function unlockScroll() { document.body.classList.remove('modal-open'); }
 
// ── FILTER ──────────────────────────────────────────────
function filterProjects(tag,btn){
  document.querySelectorAll('.filter-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  document.querySelectorAll('.project-card').forEach(card=>{
    card.style.display=(tag==='all'||card.dataset.tags.includes(tag))?'':'none';
  });
}
 
// ── LIGHTBOX ─────────────────────────────────────────────
function openLightbox(src, caption){
  document.getElementById('lightboxImg').src = src;
  document.getElementById('lightboxCaption').textContent = caption || '';
  document.getElementById('lightboxOverlay').classList.add('open');
  lockScroll();
}
function closeLightbox(){
  document.getElementById('lightboxOverlay').classList.remove('open');
  unlockScroll();
}
function closeLightboxClick(e){
  if(e.target===document.getElementById('lightboxOverlay')) closeLightbox();
}
 
// ── CAROUSEL CORE ────────────────────────────────────────
/*
  Each project/involvement entry now has a `slides` array of objects:
    { src: 'filename.jpg', caption: 'your caption text' }
  OR still works with plain strings (treated as no-caption images).
*/
function buildCarousel(slidesId, dotsId, captionId, slides){
  const slidesEl = document.getElementById(slidesId);
  slidesEl.innerHTML = slides.map(s=>{
    const src     = typeof s === 'string' ? s : s.src;
    const isImg   = /\.(png|jpe?g|gif|webp|svg)$/i.test(src);
    return isImg
      ? `<div class="carousel-slide"><img src="${src}" alt=""></div>`
      : `<div class="carousel-slide">${src}</div>`;
  }).join('');
  slidesEl.style.transform = 'translateX(0)';
  document.getElementById(dotsId).innerHTML = slides.map((_,i)=>
    `<button class="carousel-dot ${i===0?'active':''}" data-idx="${i}"></button>`
  ).join('');
  // show first caption
  setCaption(captionId, slides, 0);
}
function setCaption(captionId, slides, idx){
  const el = document.getElementById(captionId);
  if(!el) return;
  const s = slides[idx];
  el.textContent = (typeof s === 'object' && s.caption) ? s.caption : '';
}
function goToSlide(slidesId, dotsId, captionId, slides, idx){
  const n = slides.length;
  const c = ((idx % n) + n) % n;
  document.getElementById(slidesId).style.transform = `translateX(-${c*100}%)`;
  document.querySelectorAll(`#${dotsId} .carousel-dot`).forEach((d,i)=>d.classList.toggle('active',i===c));
  setCaption(captionId, slides, c);
  return c;
}
 
// project carousel
let pSlides=[], pIdx=0;
function pMove(dir){ pIdx = goToSlide('pCarouselSlides','pCarouselDots','pCaption',pSlides,pIdx+dir); }
document.getElementById('pCarouselDots').addEventListener('click',e=>{
  if(e.target.dataset.idx!==undefined) pIdx=goToSlide('pCarouselSlides','pCarouselDots','pCaption',pSlides,+e.target.dataset.idx);
});
 
// involvement carousel
let iSlides=[], iIdx=0;
function iMove(dir){ iIdx = goToSlide('iCarouselSlides','iCarouselDots','iCaption',iSlides,iIdx+dir); }
document.getElementById('iCarouselDots').addEventListener('click',e=>{
  if(e.target.dataset.idx!==undefined) iIdx=goToSlide('iCarouselSlides','iCarouselDots','iCaption',iSlides,+e.target.dataset.idx);
});
 
// ── PROJECT DATA ─────────────────────────────────────────
// slides: array of { src, caption } objects. caption is shown below the image.
const projects = {
  bimetallic:{
    title:'Bimetallic Cathode Investigation', tags:['Laboratory'],
    summary:'doin what i do best',
    skills:['Electrochemical testing','Catalyst Synthesis', 'Magnetron Co-Sputtering', 'Experimental Design','Data Analysis','Literature Review'],
    role:'I co-sputter thin film cathodes, synthesize electrolyte pellets, and assemble membrane electrodes. I conduct electrochemical testing under elevated temperature conditions before plotting and interpreting data, and further troubleshoot MEA processes and testing configuration to ensure consistent experimental results. I characterize thin film cathodes via XRR, XRD, and XPS to determine film thickness, roughness, and composition.',
    thinking:'The oxygen reduction reaction (ORR) occurs primarily via the Langmuir-Hinshelwood mechanism, in which both reactants must be adsorbed to the catalyst surface and react there, before the product desorbs. Conventional Pt cathode catalysts, while the theoretical paramount, demonstrate underperformance in experimental trials. We hypothesize that by varying the bonding energies of the catalyst through an alloyed composition, thus varying the adsorption and desorption ability of the cathode, the ORR will be enhanced.',
    methodology:'I fabricate thin-film cathodes using magnetron co-sputtering to achieve precise compositional control over platinum alloy films. I synthesize electrolyte pellets and integrate them with cathode films into membrane electrode assemblies, which are then evaluated under elevated temperature conditions representative of solid acid fuel cell operation. Electrochemical testing quantifies catalytic activity, durability, and degradation behavior. Structural characterization via XRR determines film thickness and interfacial roughness, XRD resolves crystallographic phase and texture, and XPS identifies surface elemental composition and oxidation states. I connect the material composition with the catalytic activity to understand how varying alloyed elements enhance or inhibit the ORR at the cathode.',
    solution:'This is ongoing research. Currently, we are working with postdoctoral students at MIT to better understand the ORR mechanism at the cathode surface to elucidate how an alloying element may affect the rate of reaction.',
    flowchart:[
      'Catalyst Design', 'Pellet Synthesis','Co-Sputter Thin Films','MEA Assembly','Electrochemical Testing','XRD/XPS/XRR Cathode Characterization','ORR Analysis'
    ],
    slides:[
      {src:'lab1.png', caption:'The cathode side of an MEA, recording the active cathode surface area for the ORR'},
      {src:'lab2.png', caption:'A diagram of the ORR ocurring in an SAFC'},
      {src:'lab3.png', caption:'An electrolyte pellet I synthesized and polished'},
      {src:'lab4.png', caption:'Setup for GIXRD to determine cathode thickness'}
    ]
  },
  breakfast:{
    title:'Breakfast Food Product Innovation', tags:['Product Development'],
    summary:'We developed a novel breakfast food product, Squeereal©(squeeze - cereal), based on the current market need for fast & easy on-the-go breakfast. We modelled the product viscosity off of GoGo-Squeez applesauce, as that was our container model and used Cheerios in the prototyping. Our product was distributed to our Chemical Product Design class for taste and commercialization potential evaluation.',
    skills:['Market Research','Product Development','Risk Specification','Iterative Prototyping','Experimental Design','Viscosity Analysis','Scalability Assessment'],
    role:'I led research in marketing to understand global trends in breakfast foods and consumption, patents for product packaging specifications, and scale-up to make a shelf-stable milk-based breakfast product. I recorded data from experimental trials for viscosity analysis. I led branding with the Squeereal mascot design.',
    thinking:'Breakfast consumers are increasingly prioritizing quick, clean, on-the-go breakfast solutions. We brainstormed novel ideas, and downselected to Squeereal for its easy commercialization potential. Squeereal is a squeezable cereal pouch with grain and dairy inclusions for an easy handheld breakfast that retains traditional tastes.',
    methodology:'',
    solution:'We made three editions of Squeereal with varying dairy inclusion ratios for different creaminess, the highest of which reached the target viscosity specs based on available patents and reports. The end-product ended up with the targeted flavor profile as well. If we researched further, we would focus on enhancing a multitextural experience and branching out the cereal base from Cheerios.',
    flowchart:['Market Research','Risk Specifications','Formulation Design','Prototype Testing','Viscosity & Composition Analysis','Commercialization Assessment'],
    slides:[
      {src:'SQ2.png', caption:'Idea downselection matrix'},
      {src:'SQ3.jpg', caption:'Prototyping: blending Cheerios and dairy into paste'},
      {src:'SQ4.jpg', caption:'Prototyping: mixing in for multitextural inclusion'},
      {src:'SQ5.jpg', caption:'Prototyping: Inserting product into specified packaging'},
      {src:'SQ1.png', caption:'Final prototypes, three different variations'},
      {src:'SQ6.png', caption:'Brand mascot design'}
    ]
  },
  matsci:{
    title:'Machine Learning for Materials Science', tags:['Research','Engineering'],
    summary:'',
    skills:['Python','ML Pipeline Design','Data Preprocessing','Statistical Validation','Phase Segmentation','RMSE Evaluation'],
    role:'ML engineer and materials informatics researcher, building a pipeline to analyze optical micrographs of bismuth–tin alloys for phase identification and compositional estimation from scratch.',
    thinking:'Faced a real constraint: limited training data made deep learning architectures impractical. Selected a model architecture — lightweight feature extraction plus random forest — appropriate to the data reality, not the hype cycle.',
    methodology:'Built a custom dataset from optical micrographs. Preprocessed images to remove artifacts, then extracted texture-based features using Gabor filters. Combined feature filtering with random forest regression.',
    solution:'Model achieved RMSE of 0.37 for phase fractions and 0.28 for composition — strong performance given limited training data.',
    flowchart:['Collect Micrographs','Image Preprocessing','Feature Extraction','Random Forest Model','Validation','Property Prediction'],
    slides:[
      {src:'ML1.png', caption:'[Caption: e.g. Optical micrograph dataset sample]'},
      {src:'ML2.png', caption:'[Caption: e.g. Gabor filter feature extraction]'},
      {src:'ML3.png', caption:'[Caption: e.g. RMSE performance comparison]'}
    ]
  },
  lithium:{
    title:'Direct Lithium Extraction — Technical Recommendation', tags:['Engineering','Research'],
    summary:'',
    skills:['Technical Communication','Life Cycle Analysis','Sustainability Analysis','Systems Thinking','Literature Review','Process Engineering','Quantitative Benchmarking'],
    role:'Technical analyst evaluating DLE technologies for recovering lithium from energy-sector wastewater.',
    thinking:'Approached the problem as a real technology selection decision — not a survey. Designed the analysis to produce a clear, defensible recommendation using quantitative benchmarking and risk analysis.',
    methodology:'Compared adsorption, membrane, and solvent extraction approaches across performance, cost, and scalability. Applied process engineering principles to evaluate upstream and downstream purification strategies.',
    solution:'Downselected manganese-oxide ion sieves as the most promising adsorption medium for sustainable lithium recovery.',
    flowchart:['Technology Survey','Performance Benchmarking','Cost Analysis','Scalability Review','Risk Assessment','Technology Recommendation'],
    slides:[
      {src:'DLE1.png', caption:'[Caption: e.g. Technology comparison matrix]'},
      {src:'DLE2.png', caption:'[Caption: e.g. Process flow diagram — adsorption route]'},
      {src:'DLE3.png', caption:'[Caption: e.g. Cost benchmarking chart]'},
      {src:'DLE4.png', caption:'[Caption: e.g. Sustainability scorecard]'},
      {src:'DLE5.png', caption:'[Caption: e.g. Recommendation summary slide]'}
    ]
  },
  casting:{
    title:'Aluminum Casting Lab', tags:['Engineering','Experimental Design'],
    summary:'',
    skills:['SEM/EDS','Optical Microscopy','Heat Treatment Design','Experimental Design','Hardness Testing','Metallographic Sample Preparation','Materials Characterization'],
    role:'Lead investigator on a materials characterization case study examining the unexpected hardening of a historical aluminum crankcase alloy.',
    thinking:'Treated the anomalous hardening as a real failure analysis challenge. Developed competing hypotheses — age hardening vs. slow-cooling supersaturation — and designed experiments to test each independently.',
    methodology:'Designed controlled heat treatments and modified casting processes. Characterized microstructures using hardness testing, conductivity, SEM, EDS, and optical microscopy.',
    solution:'Identified the probable mechanism behind the unexpected hardening through rigorous experimental evidence.',
    flowchart:['Hypothesis Development','Casting Trials','Heat Treatment','Microstructure Characterization','Hardness Testing','Mechanism Identification'],
    slides:[
      {src:'AL1.jpeg', caption:'[Caption: e.g. As-cast specimen cross-section]'},
      {src:'AL2.png',  caption:'[Caption: e.g. SEM micrograph — copper-rich phases]'},
      {src:'AL3.png',  caption:'[Caption: e.g. Hardness vs. aging time plot]'},
      {src:'AL4.png',  caption:'[Caption: e.g. EDS elemental map]'}
    ]
  },
  dfa:{
    title:'English Accessibility — User-Facing Design', tags:['Project Management','Community'],
    summary:'',
    skills:['User Research','Journey Mapping','Stakeholder Outreach','Human-Centered Design','Community Interviews','Team Scaling'],
    role:'Project Lead who initiated and scoped the project, secured a community partner, and scaled the team threefold.',
    thinking:"Began from the observation that immigrant communities often have access barriers to public resources that aren't visible to designers.",
    methodology:"Partnered with the Chinese Mutual Aid Association in Chicago. Conducted cross-stakeholder user research to map barriers. Coordinated research, prototyping, and community engagement.",
    solution:'Laid the groundwork for scalable resource navigation solutions for underserved immigrant communities.',
    flowchart:['Community Outreach','Stakeholder Interviews','Journey Mapping','Problem Definition','Prototype Concepts','Implementation Strategy'],
    slides:[
      {src:'DFA1.png', caption:'[Caption: e.g. Journey map workshop with community partners]'}
    ]
  },
  ewb:{
    title:'Sustainable School Construction — EWB', tags:['Community','Project Management'],
    summary:'',
    skills:['Field Engineering','Community Engagement','Project Management','Technical Communication','Cross-Cultural Collaboration'],
    role:'[Placeholder: Describe your specific role on the EWB project.]',
    thinking:'[Placeholder: What was the guiding design philosophy? How did community needs shape engineering decisions?]',
    methodology:'[Placeholder: Describe the project phases and methods used for community engagement and technical verification.]',
    solution:'[Placeholder: Where is the project now? What was the impact on the school community?]',
    flowchart:['Community Engagement','Needs Assessment','Site Evaluation','Design Development','Construction Planning','Impact Review'],
    slides:[
      {src:'EWB1.jpeg', caption:'[Caption: e.g. Site assessment, Uganda 2024]'}
    ]
  },
  simplysci:{
    title:'Simply-Sci — Science Communication Art', tags:['Art','Project Management'],
    summary:'',
    skills:['Illustration','Science Communication','Visual Storytelling','Adobe Illustrator','Concept Simplification'],
    role:'[Placeholder: Describe your role and how you collaborated with the Simply-Sci team.]',
    thinking:'[Placeholder: How do you approach translating a complex scientific idea into an image?]',
    methodology:'[Placeholder: Describe your process — from topic brief to research, sketching, iteration, final production.]',
    solution:'[Placeholder: Share examples and describe the response from the audience.]',
    flowchart:['Research Paper Review','Key Concept Selection','Sketch Development','Visual Iteration','Final Illustration','Publication'],
    slides:[
      {src:'SS1.jpg', caption:'[Caption: e.g. Newsletter cover illustration, Jan 2024]'},
      {src:'SS2.png', caption:'[Caption: e.g. Infographic — CRISPR mechanism]'},
      {src:'SS3.png', caption:'[Caption: e.g. Sketch process for issue 7]'}
    ]
  },
  msab:{
    title:'E-Week Event Production & Merchandise', tags:['Coming Soon','Community','Art','Project Management'],
    summary:'',
    skills:['Event Production','Project Management','Stakeholder Coordination','Logistics','Public Programming'],
    role:'[Placeholder: Describe your officer role and responsibilities.]',
    thinking:'[Placeholder: What does a successful Engineering Week look like?]',
    methodology:'[Placeholder: How did you plan and manage the event logistics?]',
    solution:'[Placeholder: Add outcomes when the event is complete.]',
    flowchart:['Goal Definition','Budget Planning','Vendor Coordination','Merchandise Design','Event Execution','Post-Event Review'],
    slides:[
      {src:'MSAB1.png', caption:'[Caption: e.g. E-Week 2025 merchandise design]'}
    ]
  }
};
 

function renderFlowchart(steps){
  const wrap = document.getElementById('pFlowchartWrap');
  const chart = document.getElementById('pFlowchart');
  if(!steps || !steps.length){
    wrap.style.display='none';
    chart.innerHTML='';
    return;
  }
  wrap.style.display='block';
  chart.innerHTML = steps.map((step,i)=>`
    ${i>0 ? `<div class="fc-arrow">➜</div>` : ''}
    <div class="fc-node c${i%4}">
      <span class="fc-num">Step ${i+1}</span>
      ${step}
    </div>
  `).join('');
}

function openProjectModal(key){

  const p = projects[key]; if(!p) return;
  document.getElementById('pModalTitle').textContent = p.title;
  document.getElementById('pModalSummary').textContent = p.summary;
  document.getElementById('pModalTags').innerHTML = p.tags.map(t=>`<span class="project-tag">${t}</span>`).join('');
  document.getElementById('pModalSkills').innerHTML = p.skills.map(s=>`<span class="modal-skill-tag">${s}</span>`).join('');
  document.getElementById('pModalRole').textContent = p.role;
  document.getElementById('pModalThinking').textContent = p.thinking;
  document.getElementById('pModalMethodology').textContent = p.methodology;
  renderFlowchart(p.flowchart);
  document.getElementById('pModalSolution').textContent = p.solution;
  pSlides = p.slides; pIdx = 0;
  buildCarousel('pCarouselSlides','pCarouselDots','pCaption', pSlides);
  document.getElementById('pCarouselWrap').style.display = pSlides.length ? '' : 'none';
  document.getElementById('projectModalOverlay').classList.add('open');
  lockScroll();
}
function closeProjectModal(){ document.getElementById('projectModalOverlay').classList.remove('open'); unlockScroll(); }
function handleProjectOverlayClick(e){ if(e.target===document.getElementById('projectModalOverlay')) closeProjectModal(); }
 
// ── INVOLVEMENT DATA ─────────────────────────────────────
// involvement modals are SLIDESHOW ONLY — just add slides with captions
const involvements = {
  msab_inv:{
    title:'McCormick Student Advisory Board',
    slides:[
      {src:'MSAB1.png', caption:'[Caption: e.g. E-Week 2025 kickoff event]'},
      {src:'[Add more — e.g. MSAB2.png]', caption:'[Caption]'}
    ]
  },
  ewb_inv:{
    title:'Engineers Without Borders',
    slides:[
      {src:'EWB1.jpeg', caption:'[Caption: e.g. Site assessment walk, Uganda 2024]'},
      {src:'[Add more — e.g. EWB2.jpg]', caption:'[Caption]'}
    ]
  },
  simplysci_inv:{
    title:'Simply-Sci',
    slides:[
      {src:'SS1.jpg', caption:'Astronomy newsletter introduction, Mar 2026'},
      {src:'SS2.png', caption:'Gene editing newsletter article page 1, Nov 2026'},
      {src:'SS3.png', caption:'Gene editing newsletter article page 2, Nov 2026'}
    ]
  },
  dfa_inv:{
    title:'Design for America',
    slides:[
      {src:'DFA1.png', caption:'[Caption: e.g. User research session with community partners]'},
      {src:'[Add more — e.g. DFA2.png]', caption:'[Caption]'}
    ]
  },
  art_inv:{
    title:'Studio Art Practice',
    slides:[
      {src:'[Add image — e.g. ART1.jpg]', caption:'[Caption: e.g. Watercolor study, 2024]'},
      {src:'[Add image — e.g. ART2.jpg]', caption:'[Caption: e.g. Henna design, Diwali event]'}
    ]
  },
  volunteer_inv:{
    title:'Community Engagement',
    slides:[
      {src:'[Add image — e.g. VOL1.jpg]', caption:'[Caption: e.g. Book Buddies session, Evanston 2024]'}
    ]
  }
};
 
function openInvModal(key){
  const p = involvements[key]; if(!p) return;
  iSlides = p.slides; iIdx = 0;
  buildCarousel('iCarouselSlides','iCarouselDots','iCaption', iSlides);
  document.getElementById('invModalOverlay').classList.add('open');
  lockScroll();
}
function closeInvModal(){ document.getElementById('invModalOverlay').classList.remove('open'); unlockScroll(); }
function handleInvOverlayClick(e){ if(e.target===document.getElementById('invModalOverlay')) closeInvModal(); }
 
// ── SCROLL FADE-IN ───────────────────────────────────────
const observer = new IntersectionObserver(entries=>{
  entries.forEach(en=>{ if(en.isIntersecting) en.target.style.opacity='1'; });
},{threshold:0.1});
document.querySelectorAll('.project-card,.inv-card').forEach(el=>{
  el.style.opacity='0';
  el.style.transition='opacity 0.6s ease, transform 0.35s ease, box-shadow 0.35s ease, border-color 0.3s';
  observer.observe(el);
});
const nav = document.querySelector('nav');
let lastScroll = 0;
 
window.addEventListener('scroll', () => {
 
  const currentScroll = window.scrollY;
 
  if (currentScroll < 50) {
    nav.classList.remove('visible');
  }
  else if (currentScroll < lastScroll) {
    nav.classList.add('visible');
  }
  else {
    nav.classList.remove('visible');
  }
 
  lastScroll = currentScroll;
});
</script>
