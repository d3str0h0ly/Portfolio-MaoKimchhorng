<template>
  <div class="portfolio-app" ref="appRoot">
    <!-- AMBIENT LAYERS -->
    <div class="grid-overlay"></div>
    <div class="noise-layer"></div>
    <div class="cursor-glow" :style="{ left: mouse.x + 'px', top: mouse.y + 'px' }"></div>

    <!-- PROGRESS BAR -->
    <div class="scroll-progress" :style="{ width: scrollProgress + '%' }"></div>

    <!-- NAVBAR -->
    <nav class="hud-nav" :class="{ 'nav-active': isScrolled }">
      <div class="container nav-inner">
        <a class="brand tech-text" href="#hero">
          <span class="brand-dot"></span> KIMCHHORNG<span class="text-lime">.</span>DEV
        </a>

        <ul class="nav-links">
          <li v-for="link in navLinks" :key="link.id">
            <a :href="`#${link.id}`" :class="{ active: activeSection === link.id }" @click.prevent="scrollTo(link.id)">
              <span class="nav-index">{{ link.index }}</span>{{ link.label }}
            </a>
          </li>
        </ul>

        <a class="btn-pill" href="#contact" @click.prevent="scrollTo('contact')">
          <span class="status-dot"></span> Let's talk
        </a>

        <button class="nav-toggle" @click="menuOpen = !menuOpen" aria-label="Toggle menu">
          <span :class="{ open: menuOpen }"></span>
        </button>
      </div>

      <transition name="mobile-menu">
        <div v-if="menuOpen" class="mobile-menu">
          <a v-for="link in navLinks" :key="link.id" :href="`#${link.id}`" @click.prevent="scrollTo(link.id); menuOpen = false">
            {{ link.label }}
          </a>
        </div>
      </transition>
    </nav>

    <!-- ============ HERO ============ -->
    <section id="hero" ref="vantaRef" class="hero" @mousemove="onHeroMouseMove">
      <div class="container hero-grid">
        <div class="hero-col">
          <div class="reveal availability-badge" data-reveal>
            <span class="pulse-dot"></span>
            Available for select engagements — {{ availability }}
          </div>

          <h2 class="reveal kicker tech-text" data-reveal style="--delay:0.05s">
            FULL-STACK ENGINEER · SOUTHEAST ASIA
          </h2>

          <h1 class="reveal hero-title" data-reveal style="--delay:0.1s">
            <span class="line">I build interfaces that</span>
            <span class="line gradient-text">{{ typedText }}<span class="caret">|</span></span>
          </h1>

          <p class="reveal hero-sub" data-reveal style="--delay:0.2s">
            Mao Kimchhorng — I design and ship production-grade web applications,
            from pixel-precise frontends to the systems behind them. Currently based
            in Phnom Penh, working with teams worldwide.
          </p>

          <div class="reveal hero-ctas" data-reveal style="--delay:0.3s">
            <button class="btn-primary" @click="scrollTo('projects')">
              View Projects <span class="arrow">→</span>
            </button>
            <a class="btn-ghost" href="/resume-mao-kimchhorng.pdf" download>
              Download Résumé
            </a>
            <button class="btn-ghost" @click="scrollTo('contact')">
              Contact Me
            </button>
          </div>

          <div class="reveal tech-marquee" data-reveal style="--delay:0.4s">
            <div class="marquee-track">
              <span v-for="(b, i) in [...badges, ...badges]" :key="i" class="tech-badge">
                <i :class="b.icon" :style="{ color: b.color }"></i>{{ b.name }}
              </span>
            </div>
          </div>
        </div>

        <div class="hero-col hero-visual" :style="parallaxStyle">
          <div class="orbit-card glass-card">
            <div class="orbit-header">
              <span class="dot red"></span><span class="dot yellow"></span><span class="dot green"></span>
              <span class="ms-2 text-code small">~/now-playing.status</span>
            </div>
            <pre class="orbit-code"><code><span class="c-key">role</span>: <span class="c-str">"Senior Frontend / Full-Stack"</span>
<span class="c-key">stack</span>: [<span class="c-str">"Vue"</span>, <span class="c-str">"React"</span>, <span class="c-str">"Node"</span>, <span class="c-str">"TS"</span>]
<span class="c-key">focus</span>: <span class="c-str">"systems that feel inevitable"</span>
<span class="c-key">status</span>: <span class="c-bool">true</span> <span class="c-comment">// shipping</span></code></pre>
          </div>
        </div>
      </div>

      <div class="hero-footer container">
        <span class="text-code small opacity-50">LAT 11.5564° N</span>
        <span class="text-code small opacity-50">LNG 104.9282° E</span>
        <button class="scroll-cue" @click="scrollTo('who-am-i')" aria-label="Scroll down">
          <span></span>
        </button>
      </div>
    </section>

    <!-- ============ ABOUT ============ -->
    <section id="who-am-i" class="section">
      <div class="container">
        <h6 class="eyebrow reveal" data-reveal>// 01 — ABOUT</h6>
        <h2 class="section-title reveal" data-reveal>Human-machine synergy</h2>

        <div class="about-grid">
          <div class="about-photo reveal" data-reveal>
            <div class="viewfinder">
              <div class="corner tl"></div><div class="corner tr"></div>
              <div class="corner bl"></div><div class="corner br"></div>
              <img src="https://via.placeholder.com/500x620" class="img-fluid" alt="Mao Kimchhorng">
            </div>
            <div class="fun-facts glass-card">
              <h6 class="text-code text-lime small mb-3">FUN_FACTS</h6>
              <ul>
                <li v-for="(f, i) in funFacts" :key="i">{{ f }}</li>
              </ul>
            </div>
          </div>

          <div class="about-content">
            <p class="about-lead reveal" data-reveal>
              I'm a developer obsessed with the gap between an idea and a finished
              product that actually feels good to use — fast, considered, and a
              little bit alive.
            </p>

            <div class="about-cards-grid">
              <div class="info-card glass-card reveal" data-reveal v-for="(c, i) in aboutCards" :key="i" :style="{ '--delay': i * 0.08 + 's' }">
                <i :class="c.icon" class="info-icon"></i>
                <h5>{{ c.title }}</h5>
                <p>{{ c.body }}</p>
              </div>
            </div>

            <div class="story-timeline">
              <div class="timeline-item reveal" v-for="(s, i) in story" :key="i" data-reveal :style="{ '--delay': i * 0.1 + 's' }">
                <div class="timeline-marker"></div>
                <div class="timeline-content glass-card">
                  <span class="text-code text-lime small">{{ s.year }}</span>
                  <h5>{{ s.title }}</h5>
                  <p>{{ s.body }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ============ SKILLS ============ -->
    <section id="my-skills" class="section bg-darker">
      <div class="container">
        <h6 class="eyebrow reveal" data-reveal>// 02 — STACK</h6>
        <h2 class="section-title reveal" data-reveal>Tools I reach for</h2>

        <div class="skill-tabs reveal" data-reveal>
          <button v-for="g in skillGroups" :key="g.name"
                  :class="{ active: activeSkillGroup === g.name }"
                  @click="activeSkillGroup = g.name">
            {{ g.name }}
          </button>
        </div>

        <div class="skill-grid">
          <transition-group name="skill-fade">
            <div v-for="skill in currentSkills" :key="skill.name" class="skill-card glass-card" :style="{ '--glow': skill.color }">
              <div class="skill-top">
                <i :class="skill.icon" :style="{ color: skill.color }"></i>
                <span class="skill-years text-code">{{ skill.years }}</span>
              </div>
              <h5>{{ skill.name }}</h5>
              <div class="skill-bar">
                <div class="skill-bar-fill" :style="{ width: skill.level + '%', background: skill.color }"></div>
              </div>
              <div class="skill-related">
                <span v-for="r in skill.related" :key="r">{{ r }}</span>
              </div>
            </div>
          </transition-group>
        </div>
      </div>
    </section>

    <!-- ============ PROJECTS ============ -->
    <section id="projects" class="section">
      <div class="container">
        <div class="d-flex justify-content-between align-items-end flex-wrap gap-3 mb-4">
          <div>
            <h6 class="eyebrow reveal" data-reveal>// 03 — CORE_FILES</h6>
            <h2 class="section-title reveal mb-0" data-reveal>Selected work</h2>
          </div>
          <div class="filter-pills reveal" data-reveal>
            <button v-for="cat in categories" :key="cat" :class="{ active: activeCategory === cat }" @click="activeCategory = cat">
              {{ cat }}
            </button>
          </div>
        </div>

        <div class="project-grid">
          <transition-group name="skill-fade">
            <article v-for="p in filteredProjects" :key="p.title" class="project-card glass-card"
                      @mousemove="onTilt" @mouseleave="resetTilt">
              <div class="project-media">
                <img :src="p.image" :alt="p.title">
                <div class="project-overlay">
                  <a :href="p.demo" target="_blank" class="btn-primary sm">Live Demo</a>
                  <a :href="p.repo" target="_blank" class="btn-ghost sm">GitHub</a>
                </div>
              </div>
              <div class="project-body">
                <div class="d-flex justify-content-between align-items-start">
                  <h4>{{ p.title }}</h4>
                  <span class="project-tag text-code">{{ p.category }}</span>
                </div>
                <p class="project-desc">{{ p.description }}</p>

                <div class="project-stack">
                  <span v-for="t in p.tech" :key="t">{{ t }}</span>
                </div>

                <details class="project-details">
                  <summary>Details</summary>
                  <p><strong>Features:</strong> {{ p.features }}</p>
                  <p><strong>Challenge solved:</strong> {{ p.challenge }}</p>
                  <p><strong>My role:</strong> {{ p.contribution }}</p>
                </details>

                <div class="project-stats">
                  <div v-for="(v, k) in p.stats" :key="k">
                    <strong>{{ v }}</strong><span>{{ k }}</span>
                  </div>
                </div>
              </div>
            </article>
          </transition-group>
        </div>
      </div>
    </section>

    <!-- ============ EXPERIENCE ============ -->
    <section id="experience" class="section bg-darker">
      <div class="container">
        <h6 class="eyebrow reveal" data-reveal>// 04 — TIMELINE</h6>
        <h2 class="section-title reveal" data-reveal>Where I've worked</h2>

        <div class="exp-timeline">
          <div class="exp-line"></div>
          <div v-for="(job, i) in experience" :key="i" class="exp-item reveal" data-reveal :style="{ '--delay': i * 0.1 + 's' }">
            <div class="exp-dot"></div>
            <div class="exp-card glass-card">
              <div class="d-flex justify-content-between flex-wrap">
                <h5>{{ job.role }} <span class="text-lime">@ {{ job.company }}</span></h5>
                <span class="text-code small opacity-60">{{ job.dates }}</span>
              </div>
              <ul class="exp-list">
                <li v-for="(r, j) in job.responsibilities" :key="j">{{ r }}</li>
              </ul>
              <div class="project-stack">
                <span v-for="t in job.tech" :key="t">{{ t }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ============ TESTIMONIALS ============ -->
    <section id="testimonials" class="section">
      <div class="container">
        <h6 class="eyebrow reveal" data-reveal>// 05 — FEEDBACK</h6>
        <h2 class="section-title reveal" data-reveal>What people say</h2>

        <div class="testimonial-grid">
          <div v-for="(t, i) in testimonials" :key="i" class="testimonial-card glass-card reveal" data-reveal :style="{ '--delay': i * 0.1 + 's' }">
            <p>&ldquo;{{ t.quote }}&rdquo;</p>
            <div class="testimonial-author">
              <img :src="t.avatar" :alt="t.name">
              <div>
                <strong>{{ t.name }}</strong>
                <span>{{ t.role }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ============ ACHIEVEMENTS ============ -->
    <section id="achievements" class="section bg-darker">
      <div class="container">
        <div class="stats-grid">
          <div v-for="(s, i) in stats" :key="i" class="stat-card reveal" data-reveal :style="{ '--delay': i * 0.08 + 's' }">
            <h3 class="counter">{{ s.display }}</h3>
            <p>{{ s.label }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- ============ CONTACT ============ -->
    <section id="contact" class="section">
      <div class="container">
        <h6 class="eyebrow reveal" data-reveal>// 06 — CONTACT</h6>
        <h2 class="section-title reveal" data-reveal>Start a conversation</h2>

        <div class="contact-grid">
          <form @submit.prevent="handleSubmit" class="contact-form glass-card reveal" data-reveal>
            <div class="terminal-header mb-4">
              <span class="dot red"></span><span class="dot yellow"></span><span class="dot green"></span>
              <span class="ms-3 text-code">guest@kimchhorng:~/messages</span>
            </div>
            <div class="row g-4">
              <div class="col-md-6 input-group-cyber">
                <label class="text-code small text-lime">NAME</label>
                <input v-model="form.name" type="text" required>
              </div>
              <div class="col-md-6 input-group-cyber">
                <label class="text-code small text-lime">EMAIL</label>
                <input v-model="form.email" type="email" required>
              </div>
              <div class="col-12 input-group-cyber">
                <label class="text-code small text-lime">MESSAGE</label>
                <textarea v-model="form.message" rows="4" required></textarea>
              </div>
              <div class="col-12">
                <button class="btn-primary w-100" type="submit">
                  {{ submitted ? 'Message sent ✓' : 'Send message' }}
                </button>
              </div>
            </div>
          </form>

          <div class="contact-side">
            <a v-for="c in contacts" :key="c.label" :href="c.href" target="_blank" class="contact-card glass-card reveal" data-reveal>
              <i :class="c.icon" :style="{ color: c.color }"></i>
              <div>
                <strong>{{ c.label }}</strong>
                <span>{{ c.value }}</span>
              </div>
            </a>
            <div class="contact-card glass-card map-card reveal" data-reveal>
              <iframe
                title="map"
                :src="mapEmbedUrl"
                style="border:0; width:100%; height:200px;"
                loading="lazy"
                referrerpolicy="no-referrer-when-downgrade">
              </iframe>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ============ FOOTER ============ -->
    <footer class="footer">
      <div class="container footer-inner">
        <span class="text-code small opacity-60">© {{ year }} Mao Kimchhorng. Built with Vue 3 · GSAP · Bootstrap 5.</span>
        <div class="footer-socials">
          <a v-for="c in contacts" :key="c.label" :href="c.href" target="_blank" :aria-label="c.label">
            <i :class="c.icon"></i>
          </a>
        </div>
        <button class="back-to-top" @click="scrollTo('hero')" aria-label="Back to top">↑</button>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, nextTick } from 'vue';

/* ============================================================
   STATE
   ============================================================ */
const appRoot = ref(null);
const vantaRef = ref(null);
let vantaEffect = null;

const isScrolled = ref(false);
const menuOpen = ref(false);
const scrollProgress = ref(0);
const activeSection = ref('hero');
const year = new Date().getFullYear();
const availability = 'Q3 2026';

const mouse = reactive({ x: 0, y: 0 });
const parallax = reactive({ x: 0, y: 0 });
const parallaxStyle = computed(() => ({
  transform: `translate3d(${parallax.x}px, ${parallax.y}px, 0)`
}));

const navLinks = [
  { id: 'who-am-i', label: 'About', index: '01' },
  { id: 'my-skills', label: 'Stack', index: '02' },
  { id: 'projects', label: 'Work', index: '03' },
  { id: 'experience', label: 'Experience', index: '04' },
  { id: 'contact', label: 'Contact', index: '06' },
];

/* ---------- typing effect ---------- */
const typedPhrases = ['feel inevitable.', 'scale with confidence.', 'people remember.', 'just work.'];
const typedText = ref('');
let typeState = { phrase: 0, char: 0, deleting: false };

function tick() {
  const current = typedPhrases[typeState.phrase];
  if (!typeState.deleting) {
    typeState.char++;
    typedText.value = current.slice(0, typeState.char);
    if (typeState.char === current.length) {
      typeState.deleting = true;
      setTimeout(tick, 1400);
      return;
    }
  } else {
    typeState.char--;
    typedText.value = current.slice(0, typeState.char);
    if (typeState.char === 0) {
      typeState.deleting = false;
      typeState.phrase = (typeState.phrase + 1) % typedPhrases.length;
    }
  }
  setTimeout(tick, typeState.deleting ? 35 : 65);
}

/* ---------- form ---------- */
const form = reactive({ name: '', email: '', message: '' });
const submitted = ref(false);
function handleSubmit() {
  // Replace with real API call
  submitted.value = true;
  setTimeout(() => { submitted.value = false; form.name = ''; form.email = ''; form.message = ''; }, 2500);
}

/* ============================================================
   CONTENT
   ============================================================ */
const badges = [
  { name: 'Vue 3', icon: 'fab fa-vuejs', color: '#42B883' },
  { name: 'React', icon: 'fab fa-react', color: '#61DAFB' },
  { name: 'TypeScript', icon: 'fas fa-code', color: '#3178C6' },
  { name: 'Node.js', icon: 'fab fa-node-js', color: '#339933' },
  { name: 'PostgreSQL', icon: 'fas fa-database', color: '#4169E1' },
  { name: 'Docker', icon: 'fab fa-docker', color: '#0db7ed' },
];

const funFacts = [
  'Ships side-projects faster than coffee gets cold',
  'Reads changelogs for fun',
  'Mechanical keyboard collector (currently: 4)',
  'Self-taught, scholarship-funded, never stopped',
];

const aboutCards = [
  { icon: 'fas fa-graduation-cap', title: 'Education', body: 'Scholarship recipient, ANT Training Centre — Web Development track.' },
  { icon: 'fas fa-certificate', title: 'Certifications', body: 'Meta Front-End Developer, freeCodeCamp Full-Stack.' },
  { icon: 'fas fa-bullseye', title: 'Current focus', body: 'Vue 3 + TypeScript architecture, edge rendering, and design systems.' },
];

const story = [
  { year: '2021', title: 'First line of code', body: 'Started with HTML/CSS, building static pages for local businesses.' },
  { year: '2022', title: 'Scholarship @ ANT', body: 'Selected for an intensive full-stack training program.' },
  { year: '2023', title: 'First freelance clients', body: 'Shipped Vue & React apps for small businesses and startups.' },
  { year: '2025', title: 'Full-time engineer', body: 'Joined a product team building customer-facing platforms at scale.' },
];

const skillGroups = [
  { name: 'Frontend' }, { name: 'Backend' }, { name: 'Database' },
  { name: 'DevOps' }, { name: 'Tools' }, { name: 'UI/UX' },
];
const activeSkillGroup = ref('Frontend');

const skillsByGroup = {
  Frontend: [
    { name: 'Vue 3', icon: 'fab fa-vuejs', color: '#42B883', level: 92, years: '3y', related: ['Pinia', 'Vue Router', 'Nuxt'] },
    { name: 'React', icon: 'fab fa-react', color: '#61DAFB', level: 80, years: '2y', related: ['Next.js', 'Redux'] },
    { name: 'TypeScript', icon: 'fas fa-code', color: '#3178C6', level: 85, years: '2y', related: ['ESLint', 'Zod'] },
    { name: 'CSS / Tailwind', icon: 'fab fa-css3-alt', color: '#1572B6', level: 90, years: '3y', related: ['Bootstrap', 'SCSS'] },
  ],
  Backend: [
    { name: 'Node.js', icon: 'fab fa-node-js', color: '#339933', level: 84, years: '2y', related: ['Express', 'Fastify'] },
    { name: 'REST / GraphQL', icon: 'fas fa-diagram-project', color: '#e535ab', level: 78, years: '2y', related: ['Apollo'] },
  ],
  Database: [
    { name: 'PostgreSQL', icon: 'fas fa-database', color: '#4169E1', level: 80, years: '2y', related: ['Prisma'] },
    { name: 'Firebase', icon: 'fas fa-fire', color: '#FFCA28', level: 75, years: '2y', related: ['Firestore'] },
    { name: 'MongoDB', icon: 'fas fa-leaf', color: '#47A248', level: 70, years: '1y', related: ['Mongoose'] },
  ],
  DevOps: [
    { name: 'Docker', icon: 'fab fa-docker', color: '#0db7ed', level: 72, years: '1y', related: ['Compose'] },
    { name: 'CI / CD', icon: 'fas fa-rotate', color: '#d4ff70', level: 70, years: '1y', related: ['GitHub Actions'] },
  ],
  Tools: [
    { name: 'Git', icon: 'fab fa-git-alt', color: '#F05032', level: 90, years: '3y', related: ['GitHub'] },
    { name: 'Figma', icon: 'fab fa-figma', color: '#F24E1E', level: 78, years: '2y', related: ['Design tokens'] },
  ],
  'UI/UX': [
    { name: 'Design Systems', icon: 'fas fa-layer-group', color: '#8b5cf6', level: 82, years: '2y', related: ['Storybook'] },
    { name: 'Motion / GSAP', icon: 'fas fa-wand-magic-sparkles', color: '#d4ff70', level: 76, years: '1y', related: ['Framer Motion'] },
  ],
};
const currentSkills = computed(() => skillsByGroup[activeSkillGroup.value] || []);

const categories = ['All', 'Vue', 'React', 'Full-Stack'];
const activeCategory = ref('All');
const projects = [
  {
    title: 'Neural Commerce', category: 'Vue',
    tag: 'VUE 3 // FIREBASE', image: 'https://via.placeholder.com/600x400',
    description: 'Headless e-commerce storefront with real-time inventory and a custom checkout flow.',
    tech: ['Vue 3', 'Pinia', 'Firebase', 'Stripe'],
    features: 'Real-time stock sync, saved carts, guest checkout.',
    challenge: 'Cut checkout abandonment by simplifying state across 4 steps into one reactive store.',
    contribution: 'Sole frontend engineer; built design system from scratch.',
    stats: { '40%': 'faster checkout', '99.9%': 'uptime', '12k': 'monthly users' },
    demo: '#', repo: '#',
  },
  {
    title: 'Pulse Interface', category: 'React',
    tag: 'REACT // API', image: 'https://via.placeholder.com/600x400',
    description: 'Realtime analytics dashboard streaming live metrics over WebSockets.',
    tech: ['React', 'TypeScript', 'D3', 'Node'],
    features: 'Live charts, custom alerting, role-based views.',
    challenge: 'Rendering 10k+ data points without frame drops via canvas-based charts.',
    contribution: 'Built the data layer and visualization components.',
    stats: { '60fps': 'render', '10k+': 'data points', '3': 'integrations' },
    demo: '#', repo: '#',
  },
  {
    title: 'Cyber Hub', category: 'Full-Stack',
    tag: 'NODE // POSTGRES', image: 'https://via.placeholder.com/600x400',
    description: 'Internal tool for managing client projects, invoices, and timelines.',
    tech: ['Node', 'Express', 'PostgreSQL', 'Vue'],
    features: 'Invoicing, Gantt timelines, client portal.',
    challenge: 'Designed a permissions model supporting 5 distinct user roles.',
    contribution: 'End-to-end: schema design, API, and UI.',
    stats: { '5': 'user roles', '200+': 'projects tracked', '0': 'downtime incidents' },
    demo: '#', repo: '#',
  },
];
const filteredProjects = computed(() =>
  activeCategory.value === 'All' ? projects : projects.filter(p => p.category === activeCategory.value)
);

const experience = [
  {
    role: 'Full-Stack Engineer', company: 'Product Studio', dates: '2025 — Present',
    responsibilities: ['Lead frontend architecture for a multi-tenant SaaS platform', 'Mentor 2 junior developers', 'Own CI/CD pipeline and release process'],
    tech: ['Vue 3', 'Node', 'PostgreSQL', 'Docker'],
  },
  {
    role: 'Frontend Developer', company: 'Freelance', dates: '2023 — 2025',
    responsibilities: ['Delivered 10+ client projects across e-commerce and dashboards', 'Built reusable component libraries'],
    tech: ['Vue', 'React', 'Bootstrap', 'Firebase'],
  },
  {
    role: 'Web Development Trainee', company: 'ANT Training Centre', dates: '2022 — 2023',
    responsibilities: ['Completed intensive scholarship-funded full-stack program', 'Built capstone project from scratch'],
    tech: ['HTML', 'CSS', 'JavaScript'],
  },
];

const testimonials = [
  { quote: 'Kimchhorng turned a vague brief into something our users actually love using. Rare attention to detail.', name: 'Sok Dara', role: 'Product Lead', avatar: 'https://via.placeholder.com/60' },
  { quote: 'Fast, communicative, and the code quality made our other contractors look bad. Would hire again.', name: 'Lina Chea', role: 'Startup Founder', avatar: 'https://via.placeholder.com/60' },
  { quote: 'One of the few frontend devs who also thinks about backend tradeoffs. Huge asset to the team.', name: 'James Carter', role: 'Engineering Manager', avatar: 'https://via.placeholder.com/60' },
];

const stats = [
  { value: 1200, label: 'GitHub contributions', display: '0' },
  { value: 24, label: 'Projects completed', display: '0' },
  { value: 15, label: 'Happy clients', display: '0' },
  { value: 4, label: 'Years coding', display: '0' },
];

const contacts = [
  { label: 'Email', value: 'hello@kimchhorng.dev', href: 'mailto:hello@kimchhorng.dev', icon: 'fas fa-envelope', color: '#d4ff70' },
  { label: 'LinkedIn', value: '/in/kimchhorng', href: '#', icon: 'fab fa-linkedin', color: '#0A66C2' },
  { label: 'GitHub', value: '/kimchhorng', href: '#', icon: 'fab fa-github', color: '#ffffff' },
  { label: 'Telegram', value: '@kimchhorng', href: '#', icon: 'fab fa-telegram', color: '#26A5E4' },
];
const mapEmbedUrl = 'https://maps.google.com/maps?q=11.5564,104.9282&z=13&output=embed';

/* ============================================================
   LIFECYCLE / INTERACTIONS
   ============================================================ */
function scrollTo(id) {
  document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' });
}

function handleScroll() {
  isScrolled.value = window.scrollY > 40;
  const doc = document.documentElement;
  scrollProgress.value = (window.scrollY / (doc.scrollHeight - doc.clientHeight)) * 100;
}

function onHeroMouseMove(e) {
  const rect = e.currentTarget.getBoundingClientRect();
  const cx = (e.clientX - rect.left) / rect.width - 0.5;
  const cy = (e.clientY - rect.top) / rect.height - 0.5;
  parallax.x = cx * 24;
  parallax.y = cy * 24;
}

function onMouseMoveGlobal(e) {
  mouse.x = e.clientX;
  mouse.y = e.clientY;
}

function onTilt(e) {
  const card = e.currentTarget;
  const rect = card.getBoundingClientRect();
  const x = (e.clientX - rect.left) / rect.width - 0.5;
  const y = (e.clientY - rect.top) / rect.height - 0.5;
  card.style.transform = `perspective(800px) rotateX(${-y * 6}deg) rotateY(${x * 6}deg) translateY(-4px)`;
}
function resetTilt(e) {
  e.currentTarget.style.transform = '';
}

/* IntersectionObserver: scroll reveal + active section + counters */
let observer;
function setupObservers() {
  observer = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add('in-view');
        if (entry.target.id) activeSection.value = entry.target.id;
      }
    });
  }, { threshold: 0.25 });

  document.querySelectorAll('[data-reveal], section[id]').forEach(el => observer.observe(el));

  const statObserver = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        animateCounters();
        statObserver.disconnect();
      }
    });
  }, { threshold: 0.4 });
  const statsEl = document.getElementById('achievements');
  if (statsEl) statObserver.observe(statsEl);
}

function animateCounters() {
  stats.forEach((s) => {
    let start = 0;
    const duration = 1400;
    const startTime = performance.now();
    function step(now) {
      const progress = Math.min((now - startTime) / duration, 1);
      const eased = 1 - Math.pow(1 - progress, 3);
      s.display = Math.round(eased * s.value).toLocaleString();
      if (progress < 1) requestAnimationFrame(step);
    }
    requestAnimationFrame(step);
  });
}

onMounted(async () => {
  window.addEventListener('scroll', handleScroll, { passive: true });
  window.addEventListener('mousemove', onMouseMoveGlobal);
  tick();
  await nextTick();
  setupObservers();

  if (window.VANTA) {
    vantaEffect = window.VANTA.NET({
      el: vantaRef.value,
      mouseControls: true,
      touchControls: true,
      gyroControls: false,
      minHeight: 200.0,
      minWidth: 200.0,
      scale: 1.0,
      scaleMobile: 1.0,
      backgroundColor: 0x05070d,
      color: 0xd4ff70,
      points: 9.0,
      maxDistance: 22.0,
      spacing: 18.0,
    });
  }
});

onBeforeUnmount(() => {
  window.removeEventListener('scroll', handleScroll);
  window.removeEventListener('mousemove', onMouseMoveGlobal);
  if (vantaEffect) vantaEffect.destroy();
  if (observer) observer.disconnect();
});
</script>

<style>
/* ============================================================
   DESIGN TOKENS
   ============================================================ */
:root {
  --bg: #05070d;
  --bg-darker: #03040a;
  --surface: rgba(15, 20, 32, 0.6);
  --surface-solid: #0b0f18;
  --border: rgba(255, 255, 255, 0.08);
  --lime: #d4ff70;
  --violet: #8b5cf6;
  --text: #e6eaf2;
  --text-dim: #8a93a6;
  --mono: 'JetBrains Mono', 'Courier New', monospace;
  --display: 'Space Grotesk', sans-serif;
  --body-font: 'Inter', sans-serif;
  --radius: 14px;
}

@import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;700&family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400;500&display=swap');

* { box-sizing: border-box; }

body {
  background: var(--bg);
  color: var(--text);
  font-family: var(--body-font);
  overflow-x: hidden;
  scroll-behavior: smooth;
}

.text-code { font-family: var(--mono); letter-spacing: 0.5px; }
.text-lime { color: var(--lime); }
.tech-text { font-family: var(--mono); font-weight: 600; letter-spacing: 2px; }
.gradient-text {
  background: linear-gradient(90deg, var(--lime), #8be08a 50%, var(--violet));
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}

/* ---------- ambient layers ---------- */
.grid-overlay {
  position: fixed; inset: 0;
  background-image:
    linear-gradient(rgba(212, 255, 112, 0.04) 1px, transparent 1px),
    linear-gradient(90deg, rgba(212, 255, 112, 0.04) 1px, transparent 1px);
  background-size: 48px 48px;
  pointer-events: none; z-index: 1;
}
.noise-layer {
  position: fixed; inset: 0; z-index: 1; pointer-events: none; opacity: 0.03;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='100' height='100'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='2'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
}
.cursor-glow {
  position: fixed; width: 420px; height: 420px; border-radius: 50%;
  background: radial-gradient(circle, rgba(212,255,112,0.06), transparent 70%);
  transform: translate(-50%, -50%);
  pointer-events: none; z-index: 2; transition: opacity 0.3s;
}
.scroll-progress {
  position: fixed; top: 0; left: 0; height: 2px; background: var(--lime);
  z-index: 2000; box-shadow: 0 0 10px var(--lime); transition: width 0.1s linear;
}

/* ---------- reveal animation ---------- */
[data-reveal] { opacity: 0; transform: translateY(24px); transition: opacity 0.7s ease, transform 0.7s ease; transition-delay: var(--delay, 0s); }
[data-reveal].in-view { opacity: 1; transform: translateY(0); }
@media (prefers-reduced-motion: reduce) {
  [data-reveal] { opacity: 1; transform: none; transition: none; }
}

/* ---------- nav ---------- */
.hud-nav { position: fixed; top: 0; left: 0; right: 0; z-index: 1000; padding: 24px 0; transition: 0.35s; }
.nav-active { padding: 12px 0; background: rgba(5,7,13,0.85); backdrop-filter: blur(16px); border-bottom: 1px solid var(--border); }
.nav-inner { display: flex; align-items: center; justify-content: space-between; gap: 24px; max-width: 1200px; margin: 0 auto; padding: 0 24px; }
.brand { font-family: var(--mono); font-weight: 600; letter-spacing: 1px; color: var(--text); text-decoration: none; display: flex; align-items: center; gap: 8px; }
.brand-dot { width: 8px; height: 8px; border-radius: 50%; background: var(--lime); box-shadow: 0 0 8px var(--lime); }
.nav-links { display: flex; gap: 28px; list-style: none; margin: 0; padding: 0; }
.nav-links a { color: var(--text-dim); text-decoration: none; font-size: 0.88rem; display: flex; align-items: center; gap: 6px; transition: color 0.25s; }
.nav-links a:hover, .nav-links a.active { color: var(--lime); }
.nav-index { font-family: var(--mono); font-size: 0.7rem; opacity: 0.5; }
.btn-pill { display: inline-flex; align-items: center; gap: 8px; padding: 9px 18px; border: 1px solid var(--border); border-radius: 999px; color: var(--text); text-decoration: none; font-size: 0.85rem; transition: 0.25s; }
.btn-pill:hover { border-color: var(--lime); color: var(--lime); }
.status-dot { width: 6px; height: 6px; border-radius: 50%; background: #4ade80; box-shadow: 0 0 6px #4ade80; }
.nav-toggle { display: none; background: none; border: none; }
.nav-toggle span, .nav-toggle span::before, .nav-toggle span::after { content: ''; display: block; width: 22px; height: 2px; background: var(--text); position: relative; transition: 0.25s; }
.mobile-menu { display: flex; flex-direction: column; background: var(--surface-solid); padding: 16px 24px; gap: 14px; }
.mobile-menu a { color: var(--text-dim); text-decoration: none; }
.mobile-menu-enter-active, .mobile-menu-leave-active { transition: 0.25s; }
.mobile-menu-enter-from, .mobile-menu-leave-to { opacity: 0; transform: translateY(-10px); }

/* ---------- hero ---------- */
.hero { min-height: 100vh; position: relative; padding-top: 140px; display: flex; flex-direction: column; justify-content: space-between; }
.hero-grid { display: grid; grid-template-columns: 1.2fr 0.8fr; gap: 48px; align-items: center; flex: 1; position: relative; z-index: 3; }
.availability-badge { display: inline-flex; align-items: center; gap: 8px; padding: 7px 14px; border: 1px solid var(--border); border-radius: 999px; font-size: 0.8rem; color: var(--text-dim); margin-bottom: 20px; }
.pulse-dot { width: 7px; height: 7px; border-radius: 50%; background: var(--lime); box-shadow: 0 0 8px var(--lime); animation: pulse-anim 1.6s infinite; }
@keyframes pulse-anim { 0% { box-shadow: 0 0 0 0 rgba(212,255,112,0.5); } 100% { box-shadow: 0 0 0 10px rgba(212,255,112,0); } }
.kicker { font-size: 0.8rem; color: var(--text-dim); margin-bottom: 14px; }
.hero-title { font-family: var(--display); font-size: 4.2rem; font-weight: 700; line-height: 1.08; margin-bottom: 22px; }
.hero-title .line { display: block; }
.caret { display: inline-block; width: 3px; background: var(--lime); animation: blink 0.9s steps(1) infinite; margin-left: 2px; }
@keyframes blink { 50% { opacity: 0; } }
.hero-sub { color: var(--text-dim); font-size: 1.08rem; max-width: 540px; margin-bottom: 32px; line-height: 1.7; }
.hero-ctas { display: flex; flex-wrap: wrap; gap: 14px; margin-bottom: 40px; }

.btn-primary { background: var(--lime); color: #04140a; border: none; padding: 14px 28px; border-radius: 10px; font-weight: 600; font-family: var(--body-font); display: inline-flex; align-items: center; gap: 8px; transition: 0.25s; cursor: pointer; text-decoration: none; }
.btn-primary:hover { transform: translateY(-2px); box-shadow: 0 8px 26px rgba(212,255,112,0.28); }
.btn-primary.sm { padding: 9px 16px; font-size: 0.85rem; }
.btn-primary .arrow { transition: 0.25s; }
.btn-primary:hover .arrow { transform: translateX(4px); }
.btn-ghost { background: transparent; border: 1px solid var(--border); color: var(--text); padding: 14px 26px; border-radius: 10px; text-decoration: none; transition: 0.25s; cursor: pointer; font-family: var(--body-font); }
.btn-ghost:hover { border-color: var(--lime); color: var(--lime); }
.btn-ghost.sm { padding: 9px 16px; font-size: 0.85rem; }

.tech-marquee { overflow: hidden; mask-image: linear-gradient(90deg, transparent, #000 10%, #000 90%, transparent); }
.marquee-track { display: flex; gap: 14px; width: max-content; animation: marquee 22s linear infinite; }
@keyframes marquee { from { transform: translateX(0); } to { transform: translateX(-50%); } }
.tech-badge { display: inline-flex; align-items: center; gap: 8px; padding: 8px 14px; border: 1px solid var(--border); border-radius: 999px; font-size: 0.82rem; color: var(--text-dim); white-space: nowrap; }

.hero-visual { display: flex; justify-content: center; transition: transform 0.15s ease-out; }
.orbit-card { width: 100%; max-width: 420px; padding: 0; overflow: hidden; }
.orbit-header { display: flex; align-items: center; padding: 12px 16px; border-bottom: 1px solid var(--border); }
.orbit-code { margin: 0; padding: 22px; font-size: 0.85rem; line-height: 1.8; }
.c-key { color: #8be08a; } .c-str { color: var(--lime); } .c-bool { color: #61DAFB; } .c-comment { color: var(--text-dim); }

.hero-footer { display: flex; align-items: center; justify-content: space-between; padding-bottom: 28px; position: relative; z-index: 3; }
.scroll-cue { width: 28px; height: 44px; border-radius: 16px; border: 1px solid var(--border); background: none; position: relative; cursor: pointer; }
.scroll-cue span { position: absolute; top: 8px; left: 50%; width: 4px; height: 8px; background: var(--lime); border-radius: 2px; transform: translateX(-50%); animation: scrollcue 1.6s infinite; }
@keyframes scrollcue { 0% { top: 8px; opacity: 1; } 80% { top: 26px; opacity: 0; } 100% { opacity: 0; } }

/* ---------- sections ---------- */
.section { padding: 130px 0; position: relative; z-index: 3; }
.bg-darker { background: var(--bg-darker); }
.eyebrow { font-family: var(--mono); color: var(--lime); font-size: 0.78rem; letter-spacing: 2px; margin-bottom: 10px; }
.section-title { font-family: var(--display); font-size: 2.6rem; font-weight: 700; margin-bottom: 56px; }

.glass-card { background: var(--surface); backdrop-filter: blur(14px); border: 1px solid var(--border); border-radius: var(--radius); }

/* ---------- about ---------- */
.about-grid { display: grid; grid-template-columns: 0.85fr 1.15fr; gap: 56px; }
.viewfinder { position: relative; padding: 18px; margin-bottom: 24px; }
.viewfinder img { width: 100%; border-radius: 10px; display: block; }
.corner { position: absolute; width: 26px; height: 26px; border: 2px solid var(--lime); }
.tl { top: 0; left: 0; border-right: 0; border-bottom: 0; }
.tr { top: 0; right: 0; border-left: 0; border-bottom: 0; }
.bl { bottom: 0; left: 0; border-right: 0; border-top: 0; }
.br { bottom: 0; right: 0; border-left: 0; border-top: 0; }
.fun-facts { padding: 20px; }
.fun-facts ul { margin: 0; padding-left: 18px; color: var(--text-dim); font-size: 0.88rem; line-height: 2; }
.about-lead { font-size: 1.2rem; color: var(--text); margin-bottom: 30px; line-height: 1.7; }
.about-cards-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; margin-bottom: 44px; }
.info-card { padding: 20px; }
.info-icon { color: var(--lime); font-size: 1.3rem; margin-bottom: 10px; display: block; }
.info-card h5 { margin-bottom: 6px; }
.info-card p { color: var(--text-dim); font-size: 0.88rem; margin: 0; }

.story-timeline { position: relative; padding-left: 24px; border-left: 1px solid var(--border); }
.timeline-item { position: relative; margin-bottom: 26px; }
.timeline-marker { position: absolute; left: -29px; top: 6px; width: 10px; height: 10px; border-radius: 50%; background: var(--lime); box-shadow: 0 0 8px var(--lime); }
.timeline-content { padding: 16px 20px; }
.timeline-content h5 { margin: 4px 0 6px; }
.timeline-content p { color: var(--text-dim); font-size: 0.88rem; margin: 0; }

/* ---------- skills ---------- */
.skill-tabs { display: flex; flex-wrap: wrap; gap: 10px; margin-bottom: 40px; }
.skill-tabs button { background: transparent; border: 1px solid var(--border); color: var(--text-dim); padding: 8px 18px; border-radius: 999px; font-size: 0.85rem; cursor: pointer; transition: 0.25s; }
.skill-tabs button.active, .skill-tabs button:hover { border-color: var(--lime); color: var(--lime); }
.skill-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 20px; }
.skill-card { padding: 22px; transition: 0.3s; }
.skill-card:hover { transform: translateY(-4px); border-color: var(--glow); box-shadow: 0 10px 30px -10px var(--glow); }
.skill-top { display: flex; justify-content: space-between; align-items: center; margin-bottom: 14px; }
.skill-top i { font-size: 1.6rem; }
.skill-years { color: var(--text-dim); font-size: 0.78rem; }
.skill-card h5 { margin-bottom: 14px; }
.skill-bar { height: 6px; border-radius: 4px; background: rgba(255,255,255,0.06); overflow: hidden; margin-bottom: 14px; }
.skill-bar-fill { height: 100%; border-radius: 4px; transition: width 1s cubic-bezier(.2,.8,.2,1); }
.skill-related { display: flex; flex-wrap: wrap; gap: 6px; }
.skill-related span { font-size: 0.72rem; color: var(--text-dim); border: 1px solid var(--border); padding: 3px 8px; border-radius: 999px; }
.skill-fade-enter-active, .skill-fade-leave-active { transition: 0.3s; }
.skill-fade-enter-from, .skill-fade-leave-to { opacity: 0; transform: translateY(8px); }

/* ---------- projects ---------- */
.filter-pills { display: flex; gap: 10px; }
.filter-pills button { background: transparent; border: 1px solid var(--border); color: var(--text-dim); padding: 7px 16px; border-radius: 999px; font-size: 0.82rem; cursor: pointer; transition: 0.25s; }
.filter-pills button.active, .filter-pills button:hover { border-color: var(--lime); color: var(--lime); }
.project-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(340px, 1fr)); gap: 28px; }
.project-card { overflow: hidden; transition: transform 0.15s ease-out; }
.project-media { position: relative; overflow: hidden; }
.project-media img { width: 100%; display: block; transition: 0.4s; }
.project-card:hover .project-media img { transform: scale(1.06); }
.project-overlay { position: absolute; inset: 0; background: rgba(5,7,13,0.7); display: flex; align-items: center; justify-content: center; gap: 12px; opacity: 0; transition: 0.3s; }
.project-card:hover .project-overlay { opacity: 1; }
.project-body { padding: 22px; }
.project-body h4 { margin-bottom: 4px; }
.project-tag { font-size: 0.7rem; color: var(--lime); }
.project-desc { color: var(--text-dim); font-size: 0.9rem; margin: 10px 0 14px; }
.project-stack { display: flex; flex-wrap: wrap; gap: 6px; margin-bottom: 14px; }
.project-stack span { font-size: 0.72rem; border: 1px solid var(--border); padding: 3px 9px; border-radius: 999px; color: var(--text-dim); }
.project-details summary { cursor: pointer; color: var(--lime); font-size: 0.85rem; margin-bottom: 8px; }
.project-details p { color: var(--text-dim); font-size: 0.82rem; margin: 6px 0; }
.project-stats { display: flex; gap: 22px; margin-top: 16px; padding-top: 16px; border-top: 1px solid var(--border); }
.project-stats div { display: flex; flex-direction: column; }
.project-stats strong { color: var(--lime); font-family: var(--mono); }
.project-stats span { color: var(--text-dim); font-size: 0.72rem; }

/* ---------- experience timeline ---------- */
.exp-timeline { position: relative; padding-left: 30px; }
.exp-line { position: absolute; left: 5px; top: 0; bottom: 0; width: 1px; background: var(--border); }
.exp-item { position: relative; margin-bottom: 32px; }
.exp-dot { position: absolute; left: -29px; top: 8px; width: 12px; height: 12px; border-radius: 50%; background: var(--lime); box-shadow: 0 0 8px var(--lime); }
.exp-card { padding: 22px 26px; }
.exp-list { color: var(--text-dim); font-size: 0.88rem; margin: 12px 0; padding-left: 18px; line-height: 1.8; }

/* ---------- testimonials ---------- */
.testimonial-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 24px; }
.testimonial-card { padding: 26px; }
.testimonial-card p { color: var(--text-dim); font-style: italic; margin-bottom: 20px; line-height: 1.7; }
.testimonial-author { display: flex; align-items: center; gap: 12px; }
.testimonial-author img { width: 42px; height: 42px; border-radius: 50%; border: 1px solid var(--border); }
.testimonial-author strong { display: block; font-size: 0.9rem; }
.testimonial-author span { color: var(--text-dim); font-size: 0.78rem; }

/* ---------- achievements ---------- */
.stats-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 20px; text-align: center; }
.stat-card { padding: 30px 16px; border: 1px solid var(--border); border-radius: var(--radius); }
.counter { font-family: var(--display); font-size: 2.4rem; color: var(--lime); margin-bottom: 6px; }
.stat-card p { color: var(--text-dim); font-size: 0.85rem; margin: 0; }

/* ---------- contact ---------- */
.contact-grid { display: grid; grid-template-columns: 1.3fr 0.9fr; gap: 28px; }
.contact-form { padding: 32px; }
.terminal-header { display: flex; align-items: center; }
.dot { height: 11px; width: 11px; border-radius: 50%; display: inline-block; margin-right: 4px; }
.red { background: #ff5f56; } .yellow { background: #ffbd2e; } .green { background: #27c93f; }
.input-group-cyber { display: flex; flex-direction: column; gap: 8px; }
.input-group-cyber input, .input-group-cyber textarea { background: rgba(255,255,255,0.04); border: none; border-bottom: 1px solid var(--border); padding: 12px; color: var(--text); font-family: var(--mono); border-radius: 4px; }
.input-group-cyber input:focus, .input-group-cyber textarea:focus { outline: none; border-color: var(--lime); }
.contact-side { display: flex; flex-direction: column; gap: 16px; }
.contact-card { display: flex; align-items: center; gap: 14px; padding: 18px 20px; text-decoration: none; color: var(--text); transition: 0.25s; }
.contact-card:hover { border-color: var(--lime); transform: translateX(4px); }
.contact-card i { font-size: 1.3rem; }
.contact-card strong { display: block; font-size: 0.9rem; }
.contact-card span { color: var(--text-dim); font-size: 0.78rem; }
.map-card { padding: 8px; overflow: hidden; }
.map-card iframe { border-radius: 8px; filter: grayscale(0.4) invert(0.92) contrast(0.9); }

/* ---------- footer ---------- */
.footer { padding: 30px 0; border-top: 1px solid var(--border); position: relative; z-index: 3; }
.footer-inner { display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 16px; }
.footer-socials { display: flex; gap: 16px; }
.footer-socials a { color: var(--text-dim); transition: 0.25s; }
.footer-socials a:hover { color: var(--lime); }
.back-to-top { background: none; border: 1px solid var(--border); color: var(--text); width: 36px; height: 36px; border-radius: 50%; cursor: pointer; transition: 0.25s; }
.back-to-top:hover { border-color: var(--lime); color: var(--lime); }

/* ---------- responsive ---------- */
@media (max-width: 991px) {
  .nav-links, .btn-pill { display: none; }
  .nav-toggle { display: block; }
  .hero-grid { grid-template-columns: 1fr; }
  .hero-title { font-size: 2.8rem; }
  .about-grid, .contact-grid { grid-template-columns: 1fr; }
  .about-cards-grid { grid-template-columns: 1fr; }
  .stats-grid { grid-template-columns: repeat(2, 1fr); }
}
</style>