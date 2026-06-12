<template>
  <div class="portfolio-app">
    <!-- TECHNICAL GRID OVERLAY -->
    <div class="grid-overlay"></div>
    <div class="scanline"></div>

    <!-- FLOATING HUD NAVBAR -->
    <nav class="navbar navbar-expand-lg fixed-top hud-nav" :class="{ 'nav-active': isScrolled }">
      <div class="container">
        <a class="navbar-brand tech-text" href="#hero">
          <span class="text-lime">CHHORNG</span>.SYS <span class="version">v2.0</span>
        </a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#menu">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="menu">
          <ul class="navbar-nav ms-auto">
            <li class="nav-item"><a class="nav-link tech-link" href="#who-am-i">[ ABOUT ]</a></li>
            <li class="nav-item"><a class="nav-link tech-link" href="#my-skills">[ STACK ]</a></li>
            <li class="nav-item"><a class="nav-link tech-link" href="#projects">[ CORE_FILES ]</a></li>
            <li class="nav-item ms-lg-4">
              <a class="btn btn-cyber" href="#contact">INITIALIZE_CHAT</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>

    <!-- HERO: THE INTERFACE -->
    <section id="hero" ref="vantaRef" class="hero-interface">
      <div class="container hero-content">
        <div class="row">
          <div class="col-lg-9">
            <div class="glitch-wrapper mb-3">
              <h2 class="glitch tech-text" data-text="FULL_STACK_ARCHITECT">FULL_STACK_ARCHITECT</h2>
            </div>
            <h1 class="display-1 fw-800 text-white main-title">
              DESIGNING <span class="text-outline-cyber">FUTURE</span> <br> 
              <span class="text-lime neon-glow">INTERFACES</span>
            </h1>
            <p class="lead text-code mt-4">
              > User: Mao Kimchhorng <br>
              > Status: Developing high-performance digital ecosystems <br>
              > Location: Phnom Penh, KH
            </p>
            <div class="mt-5 d-flex gap-4">
              <button @click="scrollTo('projects')" class="btn-neon">
                <span>VIEW_REPOSITORY</span>
              </button>
            </div>
          </div>
        </div>
      </div>
      
      <div class="hero-footer-stats container">
        <div class="d-flex gap-5 text-code small opacity-50">
          <span>LAT: 11.5564° N</span>
          <span>LNG: 104.9282° E</span>
          <span>Uptime: 99.9%</span>
        </div>
      </div>
    </section>

    <!-- ABOUT: SYSTEM INFO -->
    <section id="who-am-i" class="py-150">
      <div class="container">
        <div class="row align-items-center">
          <div class="col-lg-5">
            <div class="viewfinder-container">
              <div class="corner tl"></div><div class="corner tr"></div>
              <div class="corner bl"></div><div class="corner br"></div>
              <img src="https://via.placeholder.com/500x600" class="img-fluid cyber-img" alt="Profile">
              <div class="img-overlay-grid"></div>
            </div>
          </div>
          <div class="col-lg-7 ps-lg-5 mt-5 mt-lg-0">
            <h6 class="tech-text text-lime mb-3">// BIOS_INFORMATION</h6>
            <h2 class="display-4 fw-bold text-white mb-4">Human-Machine Synergy</h2>
            <div class="cyber-bio p-4 glass-card border-lime-left">
              <p class="text-code opacity-75">
                I am a web developer focused on the intersection of aesthetic design and logical architecture. 
                My workflow is built on clean code, scalable structures, and futuristic UI paradigms.
              </p>
              <p class="text-code opacity-75 mt-3">
                Scholarship recipient @ ANT Training Centre. Currently exploring the boundaries of Vue 3 and React.
              </p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- SKILLS: TECHNOLOGY GRID -->
    <section id="my-skills" class="py-100 bg-darker">
      <div class="container">
        <div class="text-center mb-5">
          <h2 class="tech-text text-white">TECH_STACK</h2>
          <div class="lime-bar mx-auto"></div>
        </div>
        <div class="row g-4 justify-content-center">
          <div v-for="skill in skills" :key="skill.name" class="col-6 col-md-3">
            <div class="skill-node p-4 text-center glass-card" :style="{ '--glow': skill.color }">
              <i :class="skill.icon" :style="{ color: skill.color }"></i>
              <h5 class="mt-3 text-code">{{ skill.name }}</h5>
              <div class="node-pulse" :style="{ background: skill.color }"></div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- PROJECTS: CORE ARCHIVE -->
    <section id="projects" class="py-150">
      <div class="container">
        <div class="d-flex justify-content-between align-items-end mb-5">
          <h2 class="tech-text text-white h1">PROJECT_ARCHIVE</h2>
        </div>
        <div class="row g-5">
          <div v-for="(project, index) in projects" :key="index" class="col-lg-4">
            <div class="project-frame glass-card">
              <div class="frame-header d-flex justify-content-between px-3 py-2">
                <span class="text-code small">ID: 00{{index + 1}}</span>
                <span class="text-code small">STATUS: STABLE</span>
              </div>
              <div class="frame-body">
                <img :src="project.image" class="img-fluid" alt="">
              </div>
              <div class="frame-footer p-3">
                <h4 class="text-white fw-bold">{{ project.title }}</h4>
                <p class="text-code text-lime small mb-0">{{ project.tag }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- CONTACT: COMMUNICATION -->
    <section id="contact" class="py-150">
      <div class="container">
        <div class="contact-terminal glass-card p-5 mx-auto" style="max-width: 800px;">
          <div class="terminal-header mb-4">
            <span class="dot red"></span><span class="dot yellow"></span><span class="dot green"></span>
            <span class="ms-3 text-code">guest@chhorng.sys:~/messages</span>
          </div>
          <form @submit.prevent="handleSubmit" class="row g-4">
            <div class="col-md-6">
              <div class="input-group-cyber">
                <label class="text-code small text-lime">SENDER_NAME</label>
                <input v-model="form.name" type="text" placeholder="REQUIRED" required>
              </div>
            </div>
            <div class="col-md-6">
              <div class="input-group-cyber">
                <label class="text-code small text-lime">SENDER_EMAIL</label>
                <input v-model="form.email" type="email" placeholder="REQUIRED" required>
              </div>
            </div>
            <div class="col-12">
              <div class="input-group-cyber">
                <label class="text-code small text-lime">MESSAGE_CONTENT</label>
                <textarea v-model="form.message" rows="4" placeholder="TYPE_MESSAGE_HERE..." required></textarea>
              </div>
            </div>
            <div class="col-12 text-center">
              <button class="btn-neon w-100"><span>EXECUTE_SEND</span></button>
            </div>
          </form>
        </div>
      </div>
    </section>

    <footer class="py-4 text-center border-top border-lime border-opacity-10">
      <p class="text-code small opacity-50 mb-0">© 2025 M_KIMCHHORNG. ALL SYSTEMS OPERATIONAL.</p>
    </footer>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, reactive } from 'vue';

const isScrolled = ref(false);
const vantaRef = ref(null);
let vantaEffect = null;
const form = reactive({ name: '', email: '', message: '' });

const skills = [
  { name: 'VUE_JS', icon: 'fab fa-vuejs', color: '#42B883' },
  { name: 'REACT_JS', icon: 'fab fa-react', color: '#61DAFB' },
  { name: 'JS_ES6', icon: 'fab fa-js', color: '#F7DF1E' },
  { name: 'BOOTSTRAP', icon: 'fab fa-bootstrap', color: '#7952b3' },
  { name: 'NODE_JS', icon: 'fab fa-node-js', color: '#339933' },
  { name: 'CSS_3', icon: 'fab fa-css3-alt', color: '#1572B6' }
];

const projects = [
  { title: 'NEURAL_COMMERCE', tag: 'VUE_3 // FIREBASE', image: 'https://via.placeholder.com/400x300' },
  { title: 'PULSE_INTERFACE', tag: 'REACT // API', image: 'https://via.placeholder.com/400x300' },
  { title: 'CYBER_HUB', tag: 'BOOTSTRAP // JS', image: 'https://via.placeholder.com/400x300' }
];

const handleScroll = () => {
  isScrolled.value = window.scrollY > 50;
};

const scrollTo = (id) => {
  document.getElementById(id).scrollIntoView({ behavior: 'smooth' });
};

onMounted(() => {
  window.addEventListener('scroll', handleScroll);
  if (window.VANTA) {
    vantaEffect = window.VANTA.RINGS({
      el: vantaRef.value,
      mouseControls: true,
      touchControls: true,
      gyroControls: false,
      minHeight: 200.00,
      minWidth: 200.00,
      scale: 1.00,
      scaleMobile: 1.00,
      backgroundColor: 0x020617,
      color: 0xd4ff70
    });
  }
});

onBeforeUnmount(() => {
  window.removeEventListener('scroll', handleScroll);
  if (vantaEffect) vantaEffect.destroy();
});
</script>

<style>
/* --- THEME DEFINITION --- */
:root {
  --lime: #d4ff70;
  --cyber-blue: #0ea5e9;
  --bg-dark: #020617;
  --glass: rgba(15, 23, 42, 0.7);
}

body {
  background-color: var(--bg-dark);
  color: #e2e8f0;
  font-family: 'Poppins', sans-serif;
  overflow-x: hidden;
}

.text-code { font-family: 'Courier New', monospace; letter-spacing: 1px; }
.text-lime { color: var(--lime); }
.tech-text { font-family: 'Courier New', monospace; font-weight: 800; letter-spacing: 3px; }

/* --- GRID BACKGROUND --- */
.grid-overlay {
  position: fixed; top: 0; left: 0; width: 100%; height: 100%;
  background-image: 
    linear-gradient(rgba(212, 255, 112, 0.05) 1px, transparent 1px),
    linear-gradient(90deg, rgba(212, 255, 112, 0.05) 1px, transparent 1px);
  background-size: 50px 50px;
  pointer-events: none; z-index: 1;
}

.scanline {
  position: fixed; top: 0; left: 0; width: 100%; height: 100%;
  background: linear-gradient(to bottom, transparent 50%, rgba(0,0,0,0.1) 51%);
  background-size: 100% 4px;
  z-index: 2; pointer-events: none; opacity: 0.3;
}

/* --- HUD NAVBAR --- */
.hud-nav { padding: 30px 0; transition: 0.4s; z-index: 1000; }
.nav-active {
  padding: 15px 0;
  background: rgba(2, 6, 23, 0.9) !important;
  backdrop-filter: blur(20px);
  border-bottom: 1px solid var(--lime);
}
.tech-link { color: #94a3b8 !important; font-size: 0.85rem; padding: 0 15px !important; }
.tech-link:hover { color: var(--lime) !important; }

/* --- HERO SECTION --- */
.hero-interface { min-height: 100vh; position: relative; padding-top: 100px; }
.text-outline-cyber {
  -webkit-text-stroke: 1px var(--lime);
  color: transparent;
  text-shadow: 0 0 15px rgba(212, 255, 112, 0.3);
}
.neon-glow { text-shadow: 0 0 20px rgba(212, 255, 112, 0.6); }

/* --- CYBER BUTTONS --- */
.btn-neon {
  background: transparent;
  border: 1px solid var(--lime);
  color: var(--lime);
  padding: 15px 40px;
  position: relative;
  overflow: hidden;
  font-family: 'Courier New', monospace;
  font-weight: bold;
  transition: 0.3s;
}
.btn-neon:hover {
  background: var(--lime);
  color: #000;
  box-shadow: 0 0 30px var(--lime);
}

/* --- VIEW_FINDER IMAGE --- */
.viewfinder-container { position: relative; padding: 20px; }
.corner { position: absolute; width: 30px; height: 30px; border: 3px solid var(--lime); }
.tl { top: 0; left: 0; border-right: 0; border-bottom: 0; }
.tr { top: 0; right: 0; border-left: 0; border-bottom: 0; }
.bl { bottom: 0; left: 0; border-right: 0; border-top: 0; }
.br { bottom: 0; right: 0; border-left: 0; border-top: 0; }

/* --- SKILL NODE --- */
.skill-node { position: relative; overflow: hidden; transition: 0.4s; }
.skill-node:hover { transform: scale(1.05); border-color: var(--glow); box-shadow: 0 0 20px var(--glow); }
.node-pulse {
  position: absolute; bottom: 10px; right: 10px;
  width: 6px; height: 6px; border-radius: 50%;
  animation: pulse 1.5s infinite;
}

@keyframes pulse {
  0% { transform: scale(1); opacity: 1; }
  100% { transform: scale(4); opacity: 0; }
}

/* --- GLASS CARDS --- */
.glass-card {
  background: var(--glass);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 4px;
}

/* --- INPUTS --- */
.input-group-cyber { margin-bottom: 20px; text-align: left; }
.input-group-cyber input, .input-group-cyber textarea {
  width: 100%;
  background: rgba(255, 255, 255, 0.05);
  border: none;
  border-bottom: 1px solid #475569;
  padding: 12px;
  color: white;
  font-family: 'Courier New', monospace;
}
.input-group-cyber input:focus { outline: none; border-color: var(--lime); }

.dot { height: 12px; width: 12px; border-radius: 50%; display: inline-block; }
.red { background: #ff5f56; }
.yellow { background: #ffbd2e; }
.green { background: #27c93f; }

@media (max-width: 991px) {
  .display-1 { font-size: 3.5rem; }
}
</style>