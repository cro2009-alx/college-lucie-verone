<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import facade1 from './assets/gallery/facade-1.jpg'
import facade2 from './assets/gallery/facade-2.jpg'
import couloir1 from './assets/gallery/couloir-1.jpg'
import couloir2 from './assets/gallery/couloir-2.jpg'
import couloir3 from './assets/gallery/couloir-3.jpg'
import logo from './assets/brand/logo.png'

// ===== Coordonnées réelles de l'établissement =====
const PHONE_DISPLAY = '01 97 11 58 69'
const PHONE_TEL = '+2290197115869'
const PHONE_WHATSAPP = 'https://wa.me/2290197115869'
const ADDRESS = "Quartier Akpakpa Ayélawadjè 1, 06 BP 0964, Cotonou, République du Bénin"
const EMAIL = 'lucieverone2002@gmail.com'
const FACEBOOK_URL = 'https://www.facebook.com/lucieverone/?locale=fr_FR'
const MAPS_URL = 'https://www.google.com/maps/place/Coll%C3%A8ge+Lucy+Verone/@6.3765434,2.4451832,17z/data=!3m1!4b1!4m6!3m5!1s0x102355397479357f:0x3d5363c6b2fb2f06!8m2!3d6.3765381!4d2.4477581!16s%2Fg%2F11g8w51vsj?entry=ttu'
const MAPS_EMBED_SRC = 'https://www.google.com/maps?q=6.3765381,2.4477581&z=17&output=embed'
const HOURS_DISPLAY = 'Du lundi au vendredi : 7h – 19h · Le samedi : 8h – 13h'

const navLinks = [
  { href: '#accueil', label: 'Accueil' },
  { href: '#etablissement', label: "L'établissement" },
  { href: '#formations', label: 'Nos formations' },
  { href: '#frais', label: 'Scolarité' },
  { href: '#vie-scolaire', label: 'Vie scolaire' },
  { href: '#galerie', label: 'Galerie' },
  { href: '#actualites', label: 'Actualités' },
  { href: '#contact', label: 'Contact' },
]

const gallery = [
  { src: facade1, caption: "Façade — Groupe Scolaire Ronsard-Lucie Vérone" },
  { src: facade2, caption: "Entrée principale de l'établissement" },
  { src: couloir1, caption: "Couloir et salles de classe à l'étage" },
  { src: couloir2, caption: "Espace intérieur décoré" },
  { src: couloir3, caption: "Vue du couloir et de la cour intérieure" },
]

const scrolled = ref(false)
const menuOpen = ref(false)
const showTop = ref(false)
const lightboxIndex = ref(null)
const activeSection = ref('accueil')

function onScroll(){
  scrolled.value = window.scrollY > 20
  showTop.value = window.scrollY > 500
}

function closeMenu(){ menuOpen.value = false }

function openLightbox(i){ lightboxIndex.value = i }
function closeLightbox(){ lightboxIndex.value = null }

function scrollTop(){ window.scrollTo({ top: 0, behavior: 'smooth' }) }

let observer
onMounted(() => {
  window.addEventListener('scroll', onScroll)

  const revealEls = document.querySelectorAll('.reveal')
  const io = new IntersectionObserver((entries) => {
    entries.forEach(e => { if (e.isIntersecting) { e.target.classList.add('in'); io.unobserve(e.target) } })
  }, { threshold: 0.15 })
  revealEls.forEach(el => io.observe(el))

  const sections = navLinks.map(l => document.querySelector(l.href))
  observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) activeSection.value = entry.target.id
    })
  }, { rootMargin: '-45% 0px -50% 0px' })
  sections.forEach(s => s && observer.observe(s))
})
onBeforeUnmount(() => {
  window.removeEventListener('scroll', onScroll)
  if (observer) observer.disconnect()
})
</script>

<template>
  <!-- ============ HEADER ============ -->
  <header :class="{ scrolled }">
    <div class="nav-wrap">
      <div class="brand">
        <div class="emblem"><img :src="logo" alt="Logo du Collège Lucie Vérone"></div>
        <div class="brand-name">Collège Lucie Vérone<small>Akpakpa, Cotonou</small></div>
      </div>
      <nav class="nav-links">
        <a v-for="l in navLinks" :key="l.href" :href="l.href" :class="{ active: activeSection === l.href.slice(1) }">{{ l.label }}</a>
      </nav>
      <div class="nav-right">
        <a href="#contact" class="btn btn-navy btn-sm">Nous contacter</a>
        <button class="burger" @click="menuOpen = !menuOpen" aria-label="Ouvrir le menu">
          <span></span><span></span><span></span>
        </button>
      </div>
    </div>
  </header>

  <div class="mobile-menu" :class="{ open: menuOpen }">
    <a v-for="l in navLinks" :key="l.href" :href="l.href" @click="closeMenu">{{ l.label }}</a>
    <a href="#contact" class="btn btn-gold" @click="closeMenu">Nous contacter</a>
  </div>

  <!-- ============ HERO / ACCUEIL ============ -->
  <section class="hero" id="accueil">
    <div class="container hero-grid">
      <div>
        <div class="hero-eyebrow">Établissement scolaire · Akpakpa, Cotonou</div>
        <h1>COLLÈGE<br><em>LUCIE VÉRONE</em></h1>
        <p class="hero-tagline">« Discipline · Travail · Excellence »</p>
        <p class="desc">Bienvenue sur le site officiel du Collège Lucie Vérone, membre du Groupe Scolaire Ronsard-Lucie Vérone. Découvrez notre établissement, notre environnement éducatif, nos activités et les informations utiles aux élèves et aux parents.</p>
        <div class="hero-btns">
          <a href="#etablissement" class="btn btn-gold">Découvrir l'établissement</a>
          <a href="#contact" class="btn btn-outline-light">Nous contacter</a>
          <a href="#formations" class="btn btn-outline-light">Informations / Inscriptions</a>
        </div>
      </div>
      <div>
        <div class="hero-visual">
          <img :src="facade1" alt="Façade du Collège Lucie Vérone, Akpakpa, Cotonou" loading="eager">
        </div>
      </div>
    </div>
    <div class="hero-scroll"><span>Découvrir</span><span class="line"></span></div>
  </section>

  <!-- ============ PRESENTATION RAPIDE ============ -->
  <section class="section-pad" id="presentation">
    <div class="container intro-grid">
      <div class="reveal intro-photo">
        <img :src="couloir2" alt="Intérieur du Collège Lucie Vérone" loading="lazy">
      </div>
      <div class="reveal">
        <div class="eyebrow">Bienvenue au Collège Lucie Vérone</div>
        <h2 class="section-title">Un environnement dédié à la réussite</h2>
        <p style="color:var(--ink-soft); font-size:1.02rem; max-width:520px;">Le Collège Lucie Vérone accompagne ses élèves dans leur parcours scolaire en mettant l'accent sur l'apprentissage, l'encadrement et l'épanouissement, dans l'esprit de sa devise : Discipline, Travail, Excellence.</p>
        <div class="intro-values">
          <div class="value-chip"><span class="num">01</span><h4>Encadrement</h4><p>Rigueur et suivi au quotidien</p></div>
          <div class="value-chip"><span class="num">02</span><h4>Apprentissage</h4><p>École, action, réflexion, savoir</p></div>
          <div class="value-chip"><span class="num">03</span><h4>Épanouissement</h4><p>Réussite, culture, joie, communication</p></div>
        </div>
      </div>
    </div>
  </section>

  <!-- ============ ETABLISSEMENT (photos + a propos) ============ -->
  <section class="section-pad about-section" id="etablissement">
    <div class="container">
      <div class="eyebrow">L'établissement</div>
      <h2 class="section-title">Le Collège Lucie Vérone</h2>
      <p class="section-sub">Situé au quartier Akpakpa Ayélawadjè, à Cotonou, le Collège Lucie Vérone fait partie du Groupe Scolaire Ronsard-Lucie Vérone, qui accueille les élèves de la 6ème à la Terminale.</p>

      <div class="motto-strip">
        <span>Discipline</span><span class="dot"></span>
        <span>Travail</span><span class="dot"></span>
        <span>Excellence</span>
      </div>

      <div class="establishment-grid">
        <div class="establishment-photo reveal"><img :src="facade1" alt="Entrée du Collège Lucie Vérone" loading="lazy"></div>
        <div class="establishment-photo reveal"><img :src="facade2" alt="Façade du Groupe Scolaire Ronsard-Lucie Vérone" loading="lazy"></div>
      </div>

      <!-- Textes rédactionnels génériques, cohérents avec la devise et les valeurs
           affichées sur l'établissement. À faire valider par l'administration. -->
      <div class="about-grid">
        <div class="about-card reveal">
          <div class="icon"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#C9A24B" stroke-width="1.6"><path d="M3 12h18M3 6h18M3 18h18"/></svg></div>
          <h3>Notre histoire</h3>
          <p>Fondé entre 2001 et 2003, le Collège Lucie Vérone — Complexe Scolaire Privé — fait partie du Groupe Scolaire Ronsard-Lucie Vérone, implanté au quartier Akpakpa Ayélawadjè à Cotonou, où il accompagne les élèves de la 6ème à la Terminale depuis plus de vingt ans.</p>
        </div>
        <div class="about-card reveal">
          <div class="icon"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#C9A24B" stroke-width="1.6"><path d="M12 2l3 7h7l-5.5 4.2L18.5 21 12 16.8 5.5 21l2-7.8L2 9h7z"/></svg></div>
          <h3>Notre mission</h3>
          <p>Offrir à chaque élève un encadrement rigoureux et un enseignement de qualité, de la 6ème à la Terminale, pour l'accompagner vers la réussite de ses examens.</p>
        </div>
        <div class="about-card reveal">
          <div class="icon"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#C9A24B" stroke-width="1.6"><circle cx="12" cy="12" r="9"/><circle cx="12" cy="12" r="3"/></svg></div>
          <h3>Notre vision</h3>
          <p>Faire du Collège Lucie Vérone une référence en matière de discipline, de travail et d'excellence académique à Akpakpa et au-delà.</p>
        </div>
        <div class="about-card reveal">
          <div class="icon"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#C9A24B" stroke-width="1.6"><path d="M12 21s-7-4.5-9-9a5 5 0 019-3 5 5 0 019 3c-2 4.5-9 9-9 9z"/></svg></div>
          <h3>Nos valeurs</h3>
          <p>Portées par notre emblème, elles guident chaque journée au collège :</p>
          <div class="values-tags">
            <span>École</span><span>Action</span><span>Réflexion</span><span>Rigueur</span>
            <span>Savoir</span><span>Réussite</span><span>Culture</span><span>Joie</span><span>Communication</span>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- ============ FORMATIONS ============ -->
  <section class="section-pad" id="formations">
    <div class="container">
      <div class="eyebrow">Nos enseignements</div>
      <h2 class="section-title">Nos formations</h2>
      <p class="section-sub">Le Groupe Scolaire Ronsard-Lucie Vérone accueille les élèves de la 6ème aux classes de Terminale, séries A, B, C, D, E, F, G.</p>
      <div class="formations-grid">
        <div class="formation-card reveal">
          <div class="formation-header">
            <div class="icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="#C9A24B" stroke-width="1.6"><path d="M4 19V5a2 2 0 012-2h9l5 5v11a2 2 0 01-2 2H6a2 2 0 01-2-2z"/><path d="M14 3v5h5"/></svg></div>
            <h3 style="margin:0;">Premier cycle</h3>
          </div>
          <div class="formation-body">
            <p>De la 6ème à la 3ème, le premier cycle pose les bases du parcours scolaire de l'élève. L'entrée en 6ème est subordonnée à l'obtention du CEP ; les entrées en 5ème et 4ème se font sur dossier (bulletins et relevés de notes de l'année précédente) et test de niveau.</p>
            <div class="serie-tags"><span>6ème</span><span>5ème</span><span>4ème</span><span>3ème</span></div>
          </div>
        </div>
        <div class="formation-card reveal">
          <div class="formation-header">
            <div class="icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="#C9A24B" stroke-width="1.6"><path d="M22 10L12 4 2 10l10 6 10-6z"/><path d="M6 12v5a6 3 0 0012 0v-5"/></svg></div>
            <h3 style="margin:0;">Second cycle</h3>
          </div>
          <div class="formation-body">
            <p>De la Seconde à la Terminale, le second cycle prépare les élèves aux examens de fin d'études secondaires, selon plusieurs séries. Les entrées en 2nde, 1ère et Terminale se font sur dossier (bulletins et relevés de notes de l'année précédente) et test de niveau.</p>
            <div class="serie-tags"><span>Série A</span><span>Série B</span><span>Série C</span><span>Série D</span><span>Série E</span><span>Série F</span><span>Série G</span></div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- ============ FRAIS DE SCOLARITÉ ============ -->
  <section class="section-pad" style="background:var(--bg-alt);" id="frais">
    <div class="container">
      <div class="eyebrow">Scolarité</div>
      <h2 class="section-title">Frais de scolarité</h2>
      <p class="section-sub">Modalités de paiement en trois tranches, selon le cycle.</p>

      <div class="fees-grid">
        <div class="fees-card reveal">
          <h3>Premier cycle</h3>
          <table class="fees-table">
            <tbody>
              <tr><td>1ère tranche (à l'inscription)</td><td>55 000 F</td></tr>
              <tr><td>2ème tranche (fin novembre)</td><td>45 000 F</td></tr>
              <tr><td>3ème tranche (fin janvier)</td><td>30 000 F</td></tr>
              <tr class="total"><td>Total</td><td>130 000 F</td></tr>
            </tbody>
          </table>
        </div>
        <div class="fees-card reveal">
          <h3>Second cycle</h3>
          <table class="fees-table">
            <tbody>
              <tr><td>1ère tranche (à l'inscription)</td><td>65 000 F</td></tr>
              <tr><td>2ème tranche (fin novembre)</td><td>60 000 F</td></tr>
              <tr><td>3ème tranche (fin janvier)</td><td>47 000 F</td></tr>
              <tr class="total"><td>Total</td><td>172 000 F</td></tr>
            </tbody>
          </table>
        </div>
      </div>

      <div class="fees-extra">
        <div class="fees-extra-card reveal">
          <h4>Travaux dirigés (TD)</h4>
          <ul>
            <li>Classe de 3ème : 25 000 F / an</li>
            <li>Classe de Terminale : 30 000 F / an</li>
          </ul>
        </div>
        <div class="fees-extra-card reveal">
          <h4>Uniformes</h4>
          <ul>
            <li>Tenue (pantalon + chemise) : 6 500 F</li>
            <li>Lacoste : 5 000 F</li>
            <li>Tee-shirt de sport : 3 000 F</li>
          </ul>
        </div>
        <div class="fees-extra-card reveal">
          <h4>Petit déjeuner &amp; cantine</h4>
          <ul>
            <li>Petit déjeuner : 6 000 F / mois</li>
            <li>Déjeuner (goûter compris) : 17 000 F / mois</li>
            <li>Séjour : 5 000 F / mois</li>
          </ul>
        </div>
      </div>
      <p class="why-note">Montants indiqués à titre informatif d'après la fiche d'inscription de l'établissement ; à confirmer auprès de l'administration pour l'année en cours.</p>
    </div>
  </section>

  <!-- ============ POURQUOI CHOISIR ============ -->
  <section class="section-pad" style="background:var(--bg-alt);">
    <div class="container">
      <div class="eyebrow">Nos atouts</div>
      <h2 class="section-title">Pourquoi choisir Lucie Vérone ?</h2>
      <p class="section-sub">Quelques axes qui structurent notre accompagnement au quotidien.</p>
      <div class="why-grid">
        <div class="why-card reveal">
          <div class="icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="#C9A24B" stroke-width="1.6"><path d="M12 14a4 4 0 100-8 4 4 0 000 8z"/><path d="M4 21v-1a6 6 0 016-6h4a6 6 0 016 6v1"/></svg></div>
          <h3>Encadrement pédagogique</h3>
          <p>Un corps enseignant présent à chaque niveau, de la 6ème à la Terminale, pour accompagner la progression de chaque élève.</p>
        </div>
        <div class="why-card reveal">
          <div class="icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="#C9A24B" stroke-width="1.6"><path d="M3 12l9-7 9 7"/><path d="M5 10v9h14v-9"/></svg></div>
          <h3>Suivi des élèves</h3>
          <p>Un suivi régulier du travail et des résultats des élèves tout au long de l'année scolaire.</p>
        </div>
        <div class="why-card reveal">
          <div class="icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="#C9A24B" stroke-width="1.6"><rect x="3" y="4" width="18" height="14" rx="2"/><path d="M8 20h8"/></svg></div>
          <h3>Environnement d'apprentissage</h3>
          <p>Des salles de classe réparties sur plusieurs niveaux, autour d'une cour intérieure propice au travail.</p>
        </div>
        <div class="why-card reveal">
          <div class="icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="#C9A24B" stroke-width="1.6"><path d="M12 2l3 7h7l-5.5 4.2L18.5 21 12 16.8 5.5 21l2-7.8L2 9h7z"/></svg></div>
          <h3>Activités scolaires et éducatives</h3>
          <p>Des activités qui viennent compléter les enseignements dispensés en classe tout au long de l'année.</p>
        </div>
        <div class="why-card reveal">
          <div class="icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="#C9A24B" stroke-width="1.6"><path d="M21 11.5a8.4 8.4 0 01-8.9 8.4A8.7 8.7 0 013 11a8.5 8.5 0 0116.7-2.3L21 11.5z"/></svg></div>
          <h3>Communication avec les parents</h3>
          <p>Un dialogue régulier avec les familles autour de la scolarité et du parcours de chaque élève.</p>
        </div>
        <div class="why-card reveal">
          <div class="icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="#C9A24B" stroke-width="1.6"><circle cx="12" cy="8" r="4"/><path d="M4 21c0-4 4-6 8-6s8 2 8 6"/></svg></div>
          <h3>Accompagnement des élèves</h3>
          <p>Un encadrement fondé sur la discipline, le travail et l'excellence, à chaque étape du parcours scolaire.</p>
        </div>
      </div>
    </div>
  </section>

  <!-- ============ VIE SCOLAIRE ============ -->
  <section class="section-pad" id="vie-scolaire">
    <div class="container">
      <div class="eyebrow">Vie scolaire</div>
      <h2 class="section-title">La vie au Collège Lucie Vérone</h2>
      <p class="section-sub">Le déroulement des cours et le cadre dans lequel évoluent les élèves au quotidien.</p>
      <div class="life-grid">
        <div class="life-card reveal">
          <div class="icon"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#B8912F" stroke-width="1.6"><path d="M4 19.5A2.5 2.5 0 016.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 014 19.5v-15A2.5 2.5 0 016.5 2z"/></svg></div>
          <h4>Déroulement des cours</h4>
          <p>Un emploi du temps propre à chaque niveau, de la 6ème à la Terminale, assuré par les enseignants de l'établissement.</p>
        </div>
        <div class="life-card reveal">
          <div class="icon"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#B8912F" stroke-width="1.6"><rect x="3" y="8" width="18" height="12"/><path d="M3 8l9-5 9 5"/></svg></div>
          <h4>Cadre d'apprentissage</h4>
          <p>Salles de classe réparties sur plusieurs niveaux, autour d'une cour intérieure commune.</p>
        </div>
        <div class="life-card reveal">
          <div class="icon"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#B8912F" stroke-width="1.6"><path d="M12 2l3 7h7l-5.5 4.2L18.5 21 12 16.8 5.5 21l2-7.8L2 9h7z"/></svg></div>
          <h4>Activités culturelles</h4>
          <p>Des moments organisés par l'établissement pour enrichir la vie scolaire des élèves.</p>
        </div>
        <div class="life-card reveal">
          <div class="icon"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#B8912F" stroke-width="1.6"><circle cx="12" cy="12" r="9"/><path d="M8 12l3 3 5-6"/></svg></div>
          <h4>Discipline et encadrement</h4>
          <p>Un cadre fondé sur la discipline, le travail et l'excellence, valeurs portées par l'établissement.</p>
        </div>
        <div class="life-card reveal">
          <div class="icon"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#B8912F" stroke-width="1.6"><path d="M3 3v18h18"/><path d="M7 15l4-4 3 3 5-6"/></svg></div>
          <h4>Sorties éducatives</h4>
          <p>Des sorties organisées ponctuellement pour compléter l'enseignement reçu en classe.</p>
        </div>
        <div class="life-card reveal">
          <div class="icon"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#B8912F" stroke-width="1.6"><circle cx="9" cy="7" r="3"/><circle cx="17" cy="7" r="3"/><path d="M2 20c0-3.3 3.1-6 7-6s7 2.7 7 6M13 14c3.9 0 7 2.7 7 6"/></svg></div>
          <h4>Clubs / associations</h4>
          <p>Des espaces d'échange et d'activités entre élèves, en dehors des heures de cours.</p>
        </div>
      </div>
    </div>
  </section>

  <!-- ============ GALERIE ============ -->
  <section class="section-pad" style="background:var(--bg-alt);" id="galerie">
    <div class="container">
      <div class="eyebrow">Galerie</div>
      <h2 class="section-title">Découvrez notre établissement</h2>
      <p class="section-sub">Quelques photos réelles du Collège Lucie Vérone. Cliquez sur une image pour l'agrandir.</p>
      <div class="gallery-grid">
        <div class="gallery-item reveal" v-for="(g, i) in gallery" :key="i" @click="openLightbox(i)">
          <img :src="g.src" :alt="g.caption" loading="lazy">
          <div class="caption">{{ g.caption }}</div>
          <div class="zoom">⤢</div>
        </div>
      </div>
    </div>
  </section>

  <div class="lightbox" :class="{ open: lightboxIndex !== null }" @click.self="closeLightbox">
    <div class="lightbox-close" @click="closeLightbox">✕</div>
    <div class="lightbox-inner" v-if="lightboxIndex !== null">
      <img :src="gallery[lightboxIndex].src" :alt="gallery[lightboxIndex].caption">
      <p class="lightbox-caption">{{ gallery[lightboxIndex].caption }}</p>
    </div>
  </div>

  <!-- ============ ACTUALITES ============ -->
  <!-- À compléter au fil de l'année par l'administration : sorties, TD, événements réels -->
  <section class="section-pad" id="actualites">
    <div class="container">
      <div class="eyebrow">Actualités</div>
      <h2 class="section-title">Actualités &amp; événements</h2>
      <p class="section-sub">Programmes de sorties, travaux dirigés et informations du Collège Lucie Vérone.</p>
      <div class="news-grid">
        <div class="news-card reveal">
          <div class="news-thumb">Inscriptions</div>
          <div class="news-body">
            <span class="news-date">À partir du 1er juin</span>
            <h3>Inscriptions ouvertes</h3>
            <p>Dépôt des dossiers à partir du 1er juin pour les entrées en 6ème, 5ème, 4ème, 3ème, 2nde, 1ère et Terminale, sur dossier et test de niveau.</p>
          </div>
        </div>
        <div class="news-card reveal">
          <div class="news-thumb">Scolarité</div>
          <div class="news-body">
            <span class="news-date">Année en cours</span>
            <h3>Frais de scolarité en 3 tranches</h3>
            <p>Paiement en trois tranches (inscription, fin novembre, fin janvier), avec des montants distincts selon le premier ou le second cycle. Voir la section « Scolarité ».</p>
          </div>
        </div>
        <div class="news-card reveal">
          <div class="news-thumb">Rentrée</div>
          <div class="news-body">
            <span class="news-date">—</span>
            <h3>Aucune information prévue pour le moment</h3>
            <p>Aucune information prévue pour le moment.</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- ============ INFOS PARENTS ============ -->
  <!-- Champs factuels propres à l'établissement : à confirmer par l'administration -->
  <section class="section-pad" style="background:var(--bg-alt);">
    <div class="container">
      <div class="eyebrow">Pour les parents</div>
      <h2 class="section-title">Informations utiles</h2>
      <p class="section-sub">Toutes les informations pratiques concernant la scolarité de votre enfant.</p>
      <div class="info-grid">
        <div class="info-card"><div class="icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#B8912F" stroke-width="1.6"><circle cx="12" cy="12" r="9"/><path d="M12 7v5l3 3"/></svg></div><h4>Horaires</h4><p>{{ HOURS_DISPLAY }}</p></div>
        <div class="info-card"><div class="icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#B8912F" stroke-width="1.6"><rect x="3" y="5" width="18" height="16" rx="2"/><path d="M3 10h18M8 3v4M16 3v4"/></svg></div><h4>Calendrier scolaire</h4><p>Dépôt des dossiers d'inscription à partir du 1er juin. <span class="placeholder-text">Aucune information supplémentaire prévue pour le moment.</span></p></div>
        <div class="info-card"><div class="icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#B8912F" stroke-width="1.6"><path d="M4 19V6a2 2 0 012-2h8l6 6v9a2 2 0 01-2 2H6a2 2 0 01-2-2z"/><path d="M14 4v6h6"/></svg></div><h4>Modalités d'inscription</h4><p>Sur dossier (bulletins et relevés de notes de l'année précédente) et test de niveau. Détails au {{ PHONE_DISPLAY }}.</p></div>
        <div class="info-card"><div class="icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#B8912F" stroke-width="1.6"><path d="M9 12l2 2 4-4"/><circle cx="12" cy="12" r="9"/></svg></div><h4>Pièces à fournir</h4><p>Bulletins et relevés de notes, photo d'identité, certificat de scolarité, extrait de naissance, chemise à rabat et un paquet de papier rame 80g (obligatoire).</p></div>
        <div class="info-card"><div class="icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#B8912F" stroke-width="1.6"><path d="M22 16.9v3a2 2 0 01-2.2 2 19.8 19.8 0 01-8.6-3.1 19.5 19.5 0 01-6-6A19.8 19.8 0 012.1 4.2 2 2 0 014.1 2h3a2 2 0 012 1.7c.1.9.3 1.8.6 2.7a2 2 0 01-.4 2.1L8 9.9a16 16 0 006 6l1.4-1.4a2 2 0 012.1-.4c.9.3 1.8.5 2.7.6a2 2 0 011.8 2.2z"/></svg></div><h4>Contacts administratifs</h4><p>{{ PHONE_DISPLAY }}</p></div>
        <div class="info-card"><div class="icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#B8912F" stroke-width="1.6"><path d="M12 9v4M12 17h.01"/><circle cx="12" cy="12" r="9"/></svg></div><h4>Informations importantes</h4><p class="placeholder-text">Aucune information prévue pour le moment.</p></div>
      </div>
    </div>
  </section>

  <!-- ============ INSCRIPTIONS ============ -->
  <section class="section-pad">
    <div class="container">
      <div class="enroll">
        <div class="enroll-text">
          <div class="eyebrow">Admissions</div>
          <h2>Inscrivez votre enfant</h2>
          <p>Pour connaître les conditions d'admission, les pièces à fournir et les modalités d'inscription, contactez directement l'administration du Collège Lucie Vérone au {{ PHONE_DISPLAY }}.</p>
        </div>
        <a :href="'tel:' + PHONE_TEL" class="btn btn-navy">Appeler le {{ PHONE_DISPLAY }}</a>
      </div>
    </div>
  </section>

  <!-- ============ CONTACT ============ -->
  <section class="section-pad" id="contact">
    <div class="container contact-grid">
      <div>
        <div class="eyebrow">Contact</div>
        <h2 class="section-title">Nous contacter</h2>
        <p style="color:var(--ink-soft); margin-bottom:20px;">Collège Lucie Vérone — Groupe Scolaire Ronsard-Lucie Vérone</p>

        <div class="contact-highlight">
          <div class="icon"><svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="#071B33" stroke-width="1.8"><path d="M22 16.9v3a2 2 0 01-2.2 2 19.8 19.8 0 01-8.6-3.1 19.5 19.5 0 01-6-6A19.8 19.8 0 012.1 4.2 2 2 0 014.1 2h3a2 2 0 012 1.7c.1.9.3 1.8.6 2.7a2 2 0 01-.4 2.1L8 9.9a16 16 0 006 6l1.4-1.4a2 2 0 012.1-.4c.9.3 1.8.5 2.7.6a2 2 0 011.8 2.2z"/></svg></div>
          <div>
            <div class="label">Numéro à contacter</div>
            <div class="num">{{ PHONE_DISPLAY }}</div>
          </div>
        </div>

        <div class="contact-list">
          <div class="contact-row">
            <div class="icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="1.6"><path d="M12 22s7-6.5 7-12a7 7 0 10-14 0c0 5.5 7 12 7 12z"/><circle cx="12" cy="10" r="2.5"/></svg></div>
            <div><h4>Adresse</h4><p>{{ ADDRESS }}</p></div>
          </div>
          <div class="contact-row">
            <div class="icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="1.6"><path d="M21 11.5a8.4 8.4 0 01-8.9 8.4A8.7 8.7 0 013 11a8.5 8.5 0 0116.7-2.3L21 11.5z"/></svg></div>
            <div><h4>WhatsApp</h4><p>{{ PHONE_DISPLAY }}</p></div>
          </div>
          <div class="contact-row">
            <div class="icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="1.6"><rect x="3" y="5" width="18" height="14" rx="2"/><path d="M3 7l9 6 9-6"/></svg></div>
            <div><h4>E-mail</h4><p><a :href="'mailto:' + EMAIL">{{ EMAIL }}</a></p></div>
          </div>
          <div class="contact-row">
            <div class="icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="1.6"><circle cx="12" cy="12" r="9"/><path d="M12 7v5l3 3"/></svg></div>
            <div><h4>Horaires</h4><p>{{ HOURS_DISPLAY }}</p></div>
          </div>
          <div class="contact-row">
            <div class="icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="1.6"><path d="M18 2h-3a5 5 0 00-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 011-1h3z"/></svg></div>
            <div><h4>Facebook</h4><p><a :href="FACEBOOK_URL" target="_blank" rel="noopener">facebook.com/lucieverone</a></p></div>
          </div>
        </div>
      </div>
      <div class="contact-visual reveal">
        <img :src="couloir3" alt="Intérieur du Collège Lucie Vérone">
      </div>
    </div>
  </section>

  <!-- ============ LOCALISATION ============ -->
  <section class="section-pad" style="background:var(--bg-alt);">
    <div class="container">
      <div class="eyebrow">Localisation</div>
      <h2 class="section-title">Nous trouver</h2>
      <p class="section-sub">{{ ADDRESS }}</p>
      <div class="map-wrap">
        <iframe
          class="map-frame"
          :src="MAPS_EMBED_SRC"
          loading="lazy"
          referrerpolicy="no-referrer-when-downgrade"
          title="Localisation du Collège Lucie Vérone sur Google Maps">
        </iframe>
      </div>
      <div style="text-align:center; margin-top:24px;">
        <a :href="MAPS_URL" target="_blank" rel="noopener" class="btn btn-outline-navy btn-sm">Itinéraire</a>
      </div>
    </div>
  </section>

  <!-- ============ RESEAUX SOCIAUX ============ -->
  <section class="section-pad">
    <div class="container" style="text-align:center;">
      <div class="eyebrow" style="justify-content:center;">Réseaux sociaux</div>
      <h2 class="section-title">Suivez-nous</h2>
      <div class="social-row">
        <a class="social-pill" :href="FACEBOOK_URL" target="_blank" rel="noopener">Facebook</a>
        <a class="social-pill" :href="PHONE_WHATSAPP">WhatsApp <span class="placeholder-text">{{ PHONE_DISPLAY }}</span></a>
      </div>
    </div>
  </section>

  <!-- ============ CTA FINAL ============ -->
  <section class="cta-final">
    <div class="container">
      <div class="emblem"><img :src="logo" alt="Logo du Collège Lucie Vérone"></div>
      <h2>Une question concernant notre établissement ?</h2>
      <p>Notre équipe reste à votre disposition pour vous renseigner au {{ PHONE_DISPLAY }}.</p>
      <div class="hero-btns">
        <a :href="'tel:' + PHONE_TEL" class="btn btn-gold">Appeler le {{ PHONE_DISPLAY }}</a>
        <a :href="PHONE_WHATSAPP" class="btn btn-outline-light">Nous écrire sur WhatsApp</a>
      </div>
    </div>
  </section>

  <!-- ============ FOOTER ============ -->
  <footer>
    <div class="container">
      <div class="footer-grid">
        <div class="footer-brand">
          <div class="brand">
            <div class="emblem"><img :src="logo" alt="Logo du Collège Lucie Vérone"></div>
            <div class="brand-name" style="color:#fff;">Collège Lucie Vérone</div>
          </div>
          <p>Établissement scolaire du Groupe Scolaire Ronsard-Lucie Vérone, dédié à l'apprentissage, l'encadrement et l'épanouissement des élèves, de la 6ème à la Terminale.</p>
        </div>
        <div class="footer-col">
          <h4>Navigation</h4>
          <a v-for="l in navLinks" :key="l.href" :href="l.href">{{ l.label }}</a>
        </div>
        <div class="footer-col">
          <h4>Coordonnées</h4>
          <p>{{ ADDRESS }}</p>
          <p>{{ PHONE_DISPLAY }}</p>
          <p>{{ EMAIL }}</p>
        </div>
        <div class="footer-col">
          <h4>Réseaux Sociaux</h4>
          <div class="footer-social">
            <a :href="FACEBOOK_URL" target="_blank" rel="noopener" aria-label="Facebook" title="Facebook">f</a>
            <a :href="PHONE_WHATSAPP" target="_blank" rel="noopener" aria-label="WhatsApp" title="WhatsApp">wa</a>
          </div>
        </div>
      </div>
      <div class="footer-bottom">
        <span>© 2026 Collège Lucie Vérone — Tous droits réservés.</span>
        <span>Site réalisé par MOVA — Solutions numériques &amp; automatisation</span>
      </div>
    </div>
  </footer>

  <!-- ============ FLOTTANTS ============ -->
  <div class="float-stack">
    <button class="float-btn float-top" :class="{ show: showTop }" @click="scrollTop" aria-label="Retour en haut">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 19V5M5 12l7-7 7 7"/></svg>
    </button>
    <a class="float-btn float-call" :href="'tel:' + PHONE_TEL" aria-label="Appeler l'établissement" title="Appeler">
      <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M22 16.9v3a2 2 0 01-2.2 2 19.8 19.8 0 01-8.6-3.1 19.5 19.5 0 01-6-6A19.8 19.8 0 012.1 4.2 2 2 0 014.1 2h3a2 2 0 012 1.7c.1.9.3 1.8.6 2.7a2 2 0 01-.4 2.1L8 9.9a16 16 0 006 6l1.4-1.4a2 2 0 012.1-.4c.9.3 1.8.5 2.7.6a2 2 0 011.8 2.2z"/></svg>
    </a>
    <a class="float-btn float-whatsapp" :href="PHONE_WHATSAPP" aria-label="Contacter l'établissement sur WhatsApp" title="Contacter l'établissement">
      <svg width="26" height="26" viewBox="0 0 24 24" fill="currentColor"><path d="M20.5 3.5a11 11 0 00-17.4 13.2L2 22l5.5-1.1A11 11 0 1020.5 3.5zM12 20a9 9 0 01-4.6-1.3l-.3-.2-3.3.7.7-3.2-.2-.3A9 9 0 1112 20zm5-6.7c-.3-.1-1.6-.8-1.8-.9-.2-.1-.4-.1-.6.1-.2.3-.7.9-.8 1-.2.2-.3.2-.5.1-1.4-.7-2.4-1.3-3.3-2.9-.3-.4.3-.4.7-1.3.1-.2 0-.4 0-.5C10.6 8.6 10.1 7.4 9.9 7c-.2-.4-.4-.4-.6-.4h-.5c-.2 0-.5.1-.7.3-.3.3-1 1-1 2.4s1 2.8 1.2 3c.1.2 2 3 4.8 4.2.7.3 1.2.5 1.6.6.7.2 1.3.2 1.8.1.5-.1 1.6-.6 1.8-1.3.2-.6.2-1.1.2-1.2-.1-.1-.3-.2-.5-.3z"/></svg>
    </a>
  </div>
</template>