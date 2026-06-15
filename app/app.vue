<script setup lang="ts">
import { computed, onBeforeUnmount, onMounted, ref } from 'vue'

useHead({
  title: 'Portfolio',
  meta: [
    { name: 'viewport', content: 'width=device-width, initial-scale=1.0' }
  ],
  link: [
    {
      rel: 'icon',
      type: 'image/png',
      href: '/image copy 3.png'
    },
    {
      rel: 'preconnect',
      href: 'https://fonts.googleapis.com'
    },
    {
      rel: 'preconnect',
      href: 'https://fonts.gstatic.com',
      crossorigin: ''
    },
    {
      rel: 'stylesheet',
      href: 'https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;0,9..40,600;1,9..40,300&family=DM+Serif+Display&family=Space+Mono:wght@400;700&display=swap'
    }
  ]
})

const activeSection = ref('home')
const clock = ref('--:-- --')
const typedRole = ref('')

const roles = ['builder.', 'developer.', 'researcher.', 'engineer.']

const experienceItems = [
  { label: 'CrispAI', short: 'S', className: 'sandlogic' },
  { label: 'Delta Freight', short: 'DFA', className: 'iui' },
  { label: 'Fuzu LTD', short: 'F', className: 'iub' }
]

const resumeTags = ['AI/ML', 'Backend', 'Cloud']

const connectLinks = [
  {
    label: 'GitHub',
    href: 'https://github.com/kipkirui63',
    icon: 'github'
  },
  {
    label: 'LinkedIn',
    href: 'https://www.linkedin.com/in/enock-sang',
    icon: 'linkedin'
  }
]

const imageGallery = [
  { src: '/image.png', alt: 'Portfolio image one', label: 'AI tools' },
  { src: '/image copy.png', alt: 'Portfolio image two', label: 'ML' },
  { src: '/image copy 2.png', alt: 'Portfolio image three', label: 'Automation' }
]

const activeProject = {
  badge: 'ACTIVE',
  sub: 'One click. Issue fixed. PR merged.',
  title: 'Building',
  accent: 'AutoFlow.'
}

const bigProjects = [
  {
    name: 'Lead Scraper',
    tagline: 'Find prospects. Fill the pipeline.',
    variant: 'default',
    header: 'Lead Scraper — Google Maps'
  },
  {
    name: 'FB AutoDM',
    tagline: 'No itinerary? No problem. Just ask.',
    variant: 'blue',
    header: 'Facebook Messenger Bot'
  },
  {
    name: 'PadCare Box',
    tagline: 'IoT smart lock for student welfare.',
    variant: 'green',
    emoji: '🔒'
  }
]

const currentYear = computed(() => new Date().getFullYear())

let clockTimer: ReturnType<typeof setInterval> | null = null
let typingTimer: ReturnType<typeof setTimeout> | null = null

function updateClock() {
  const now = new Date()
  let hours = now.getHours()
  const minutes = String(now.getMinutes()).padStart(2, '0')
  const meridiem = hours >= 12 ? 'PM' : 'AM'

  hours = hours % 12 || 12
  clock.value = `${String(hours).padStart(2, '0')}:${minutes} ${meridiem}`
}

function startTypingEffect() {
  let roleIndex = 0
  let charIndex = 0
  let deleting = false

  const tick = () => {
    const word = roles[roleIndex]

    if (!deleting) {
      charIndex += 1
      typedRole.value = word.slice(0, charIndex)

      if (charIndex === word.length) {
        deleting = true
        typingTimer = setTimeout(tick, 1800)
        return
      }
    } else {
      charIndex -= 1
      typedRole.value = word.slice(0, charIndex)

      if (charIndex === 0) {
        deleting = false
        roleIndex = (roleIndex + 1) % roles.length
      }
    }

    typingTimer = setTimeout(tick, deleting ? 60 : 100)
  }

  tick()
}

function setActive(section: string) {
  activeSection.value = section
}

onMounted(() => {
  updateClock()
  clockTimer = setInterval(updateClock, 1000)
  startTypingEffect()
})

onBeforeUnmount(() => {
  if (clockTimer) {
    clearInterval(clockTimer)
  }

  if (typingTimer) {
    clearTimeout(typingTimer)
  }
})
</script>

<template>
  <div class="page-shell">
    <NuxtRouteAnnouncer />

    <nav class="top-nav">
      <div class="avatar">
        <img src="/image copy 3.png" alt="Enock Sang avatar" class="avatar-image">
      </div>
      <a
        href="#home"
        :class="{ active: activeSection === 'home' }"
        @click="setActive('home')"
      >
        Home
      </a>
      <a
        href="#work"
        :class="{ active: activeSection === 'work' }"
        @click="setActive('work')"
      >
        Work
      </a>
    </nav>

    <main id="home" class="portfolio-main">
      <div class="top-layout">
        <section class="card hero-card">
          <div class="hero-blob" />
          <h1 class="hero-title">
            <span class="name">Enock Sang </span>
            <span class="is-a">is a </span>
            <span class="role">{{ typedRole }}</span>
            <span class="cursor" />
          </h1>
          <div class="hero-sub">
            <p>I code, research, and build things that matter.</p>
            <p>Still learning. Still building.</p>
          </div>
        </section>

        <div class="top-right-stack">
          <section class="card experience-card">
            <div class="exp-label">Experience</div>
            <div class="exp-logos">
              <article
                v-for="item in experienceItems"
                :key="item.label"
                class="exp-logo-item"
              >
                <div class="exp-logo-icon" :class="item.className">
                  {{ item.short }}
                </div>
                <span>{{ item.label }}</span>
              </article>
            </div>
          </section>

          <div class="mini-card-row">
            <section class="card resume-card resume-card-compact">
              <div class="resume-card-left">
                <div class="resume-title-row">
                  <div class="resume-dot" />
                  <span class="resume-title">RÉSUMÉ .</span>
                </div>
                <div class="resume-tags">
                  <span v-for="tag in resumeTags" :key="tag" class="tag">
                    {{ tag }}
                  </span>
                </div>
                <a href="#" class="view-btn">View ↗</a>
              </div>
              <div class="resume-icon">📄</div>
            </section>

            <section class="card project-card project-card-compact">
              <div class="project-badge">
                <span class="project-badge-dot" />
                Published
                <span class="project-year">arXiv · 2025</span>
              </div>
              <div class="project-name">LeadGen Pro</div>
              <div class="project-desc">AI-powered lead generation & automation</div>
            </section>
          </div>
        </div>
      </div>

      <div class="row-4 spaced-row">
        <section class="card quote-card">
          <p class="quote-text">
            "The human brain is Schrödinger's cat. Open the box and think beyond it.
            Stay inside and you never existed at all."
          </p>
          <span class="quote-author">— ænet</span>
        </section>

        <section class="card time-card">
          <div class="time-display">
            <span>{{ clock }}</span>
            <span class="moon-icon">🌙</span>
          </div>
          <div class="location-row">📍 Nairobi, KE</div>
        </section>

        <section class="card edu-card">
          <span class="edu-year">2024</span>
          <div class="edu-logo">U</div>
          <div class="edu-name">University of Nairobi</div>
          <div class="edu-degree">Bachelor's in Computer Science</div>
        </section>

        <section class="card live-card">
          <div class="live-badge">LIVE</div>
          <div class="live-title">
            Meet <br>
            <span>n8n Flows.</span>
          </div>
        </section>
      </div>

      <div class="third-row spaced-row">
        <section class="card images-card">
          <div
            v-for="image in imageGallery"
            :key="image.src"
            class="img-slot"
          >
            <img :src="image.src" :alt="image.alt" class="gallery-image">
            <span class="img-label">{{ image.label }}</span>
          </div>
        </section>

        <section class="card connect-card">
          <div class="connect-label">Connect</div>
          <a
            v-for="link in connectLinks"
            :key="link.label"
            :href="link.href"
            class="connect-btn"
            target="_blank"
            rel="noreferrer"
          >
            <svg v-if="link.icon === 'github'" viewBox="0 0 24 24" aria-hidden="true">
              <path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22" />
            </svg>
            <svg v-else viewBox="0 0 24 24" aria-hidden="true">
              <path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z" />
              <rect x="2" y="9" width="4" height="12" />
              <circle cx="4" cy="4" r="2" />
            </svg>
            {{ link.label }}
          </a>
        </section>

        <section class="card active-card">
          <div class="active-badge">{{ activeProject.badge }}</div>
          <div class="active-sub">{{ activeProject.sub }}</div>
          <div class="active-title">
            {{ activeProject.title }}
            <span>{{ activeProject.accent }}</span>
          </div>
        </section>
      </div>

      <div id="work" class="section-label">Projects</div>
      <div class="projects-grid">
        <article
          v-for="project in bigProjects"
          :key="project.name"
          class="project-big-card"
        >
          <div class="proj-content">
            <div class="proj-name">{{ project.name }}</div>
            <div class="proj-tagline">{{ project.tagline }}</div>
          </div>

          <div class="proj-screenshot" :class="project.variant">
            <div v-if="project.emoji" class="emoji-shot">
              {{ project.emoji }}
            </div>
            <div v-else class="fake-ui">
              <div class="fake-ui-bar" :class="{ facebook: project.variant === 'blue' }">
                {{ project.header }}
              </div>
              <div class="fake-ui-row">
                <div class="fake-ui-cell" :class="{ accent: project.variant === 'blue' }" />
                <div class="fake-ui-cell" />
                <div class="fake-ui-cell" />
              </div>
              <div class="fake-ui-row">
                <div class="fake-ui-cell" />
                <div class="fake-ui-cell" />
              </div>
              <div class="fake-ui-row">
                <div class="fake-ui-cell" />
                <div class="fake-ui-cell" />
                <div class="fake-ui-cell" />
              </div>
              <div class="fake-ui-row">
                <div class="fake-ui-cell" />
                <div class="fake-ui-cell" />
              </div>
            </div>
          </div>
        </article>
      </div>

      <div class="cta-section">
        <section class="cta-card">
          <div class="cta-label">Always open to new conversations</div>
          <div class="cta-title">
            LETS
            <br>
            TALK
          </div>
          <a href="mailto:you@example.com" class="cta-link">Email me here</a>
        </section>
      </div>
    </main>

    <footer class="page-footer">
      <div>
        <p>© {{ currentYear }} Enock Sang. All rights reserved.</p>
        
      </div>
      <div class="footer-links">
        <a href="https://github.com" target="_blank" rel="noreferrer">GitHub</a>
        <a href="https://linkedin.com" target="_blank" rel="noreferrer">LinkedIn</a>
      </div>
    </footer>
  </div>
</template>

<style>
html {
  scroll-behavior: smooth;
}

*,
*::before,
*::after {
  box-sizing: border-box;
}

body {
  margin: 0;
  min-height: 100vh;
  font-family: 'DM Sans', sans-serif;
  background:
    radial-gradient(circle at top left, rgba(249, 115, 22, 0.08), transparent 28%),
    linear-gradient(180deg, #f7f4ef 0%, #efebe5 100%);
  color: #1a1a1a;
}

a {
  color: inherit;
}

:root {
  --bg: #f0efed;
  --card: rgba(255, 255, 255, 0.82);
  --text: #1a1a1a;
  --muted: #777777;
  --accent: #f97316;
  --accent-2: #fb923c;
  --green: #16a34a;
  --blue: #2563eb;
  --border: rgba(120, 104, 84, 0.14);
  --dark-card: #111111;
  --shadow: 0 18px 50px rgba(35, 25, 12, 0.08);
}

.page-shell {
  min-height: 100vh;
}

.top-nav {
  position: fixed;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  align-items: center;
  gap: 4px;
  z-index: 100;
  padding: 6px 8px;
  border: 1px solid var(--border);
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.72);
  backdrop-filter: blur(16px);
  box-shadow: 0 2px 20px rgba(0, 0, 0, 0.08);
}

.avatar {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: #ffffff;
  font-size: 14px;
  font-weight: 700;
  overflow: hidden;
}

.avatar-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.top-nav a {
  padding: 6px 16px;
  border-radius: 999px;
  color: var(--muted);
  font-size: 14px;
  font-weight: 500;
  text-decoration: none;
  transition: color 0.2s ease, background 0.2s ease;
}

.top-nav a.active {
  background: var(--text);
  color: #ffffff;
}

.top-nav a:hover:not(.active) {
  color: var(--text);
}

.portfolio-main {
  max-width: 1050px;
  margin: 0 auto;
  padding: 100px 20px 40px;
}

.top-layout {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 14px;
  align-items: stretch;
}

.top-right-stack {
  display: flex;
  flex-direction: column;
  gap: 14px;
}

.mini-card-row {
  display: grid;
  grid-template-columns: minmax(0, 0.92fr) minmax(0, 1.08fr);
  gap: 14px;
}

.row-2,
.projects-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 14px;
}

.row-4 {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  gap: 14px;
  align-items: stretch;
}

.row-4 > .card {
  min-height: 220px;
}

.row-half-big {
  display: grid;
  grid-template-columns: minmax(0, 1.2fr) minmax(0, 0.8fr);
  gap: 14px;
}

.third-row {
  display: grid;
  grid-template-columns: minmax(0, 1.45fr) minmax(0, 0.8fr) minmax(0, 0.9fr);
  gap: 14px;
  align-items: stretch;
}

.grid-gap {
  display: flex;
  flex-direction: column;
  gap: 14px;
}

.spaced-row {
  margin-top: 14px;
}

.card,
.project-big-card {
  border: 1px solid var(--border);
  border-radius: 20px;
  background: var(--card);
  backdrop-filter: blur(10px);
  box-shadow: var(--shadow);
  overflow: hidden;
  animation: fade-up 0.55s ease both;
}

.hero-card {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  min-height: 260px;
  padding: 44px 40px;
}

.hero-blob {
  position: absolute;
  top: -30px;
  right: -20px;
  width: 240px;
  height: 240px;
  border-radius: 60% 40% 70% 30% / 50% 60% 40% 50%;
  background: linear-gradient(145deg, #f97316, #fb923c, #f59e0b);
  opacity: 0.95;
}

.hero-title {
  position: relative;
  z-index: 1;
  max-width: 14ch;
  font-family: 'DM Serif Display', serif;
  font-size: clamp(2.1rem, 4vw, 2.9rem);
  line-height: 1.1;
}

.hero-title .name,
.hero-title .role {
  color: var(--text);
}

.hero-title .is-a {
  color: var(--muted);
  font-weight: 300;
  font-style: italic;
}

.hero-title .role {
  text-decoration: underline;
  text-decoration-thickness: 1.5px;
  text-underline-offset: 4px;
}

.hero-sub {
  position: relative;
  z-index: 1;
}

.hero-sub p {
  margin: 0;
  color: #555555;
  font-size: 14px;
  line-height: 1.6;
}

.experience-card,
.project-card,
.connect-card,
.edu-card,
.active-card {
  padding: 24px;
}

.exp-label,
.connect-label,
.section-label,
.cta-label {
  margin-bottom: 16px;
  color: var(--muted);
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
}

.exp-logos {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 10px;
}

.exp-logo-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  padding: 16px 12px 12px;
  border-radius: 14px;
  background: #f5f5f3;
  cursor: pointer;
  transition: transform 0.2s ease;
}

.exp-logo-item:hover,
.project-big-card:hover {
  transform: translateY(-3px);
}

.exp-logo-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 52px;
  height: 52px;
  border-radius: 10px;
  font-size: 22px;
  font-weight: 700;
}

.exp-logo-icon.sandlogic {
  background: #1a1a2e;
  color: #ffffff;
}

.exp-logo-icon.iui,
.exp-logo-icon.iub {
  background: #990000;
  color: #ffffff;
  font-size: 14px;
  letter-spacing: -0.5px;
}

.exp-logo-item span {
  color: var(--muted);
  font-size: 11px;
  font-weight: 500;
}

.resume-card {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  padding: 22px 24px;
  transition: background 0.2s ease;
}

.resume-card-compact {
  min-height: 100%;
  padding: 18px 20px;
}

.resume-card-compact .resume-title {
  font-size: 14px;
}

.resume-card-compact .resume-tags {
  gap: 6px;
}

.resume-card-compact .tag {
  padding: 2px 8px;
  font-size: 10px;
}

.resume-card-compact .resume-icon {
  width: 38px;
  height: 48px;
  font-size: 20px;
}

.project-card-compact {
  min-height: 100%;
  padding: 18px 20px;
}

.project-card-compact .project-name {
  font-size: 16px;
}

.project-card-compact .project-desc {
  font-size: 12px;
}

.resume-card:hover,
.project-card:hover,
.connect-btn:hover,
.view-btn:hover {
  background: rgba(255, 255, 255, 0.95);
}

.resume-card-left {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.resume-title-row {
  display: flex;
  align-items: center;
  gap: 8px;
}

.resume-dot,
.project-badge-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
}

.resume-dot {
  background: var(--muted);
}

.resume-title {
  font-size: 16px;
  font-weight: 600;
  letter-spacing: 0.06em;
}

.resume-tags,
.project-meta,
.footer-links {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.tag,
.view-btn {
  border: 1px solid var(--border);
  border-radius: 999px;
}

.tag {
  padding: 2px 10px;
  color: var(--muted);
  font-size: 11px;
}

.view-btn {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  width: fit-content;
  margin-top: 12px;
  padding: 6px 14px;
  color: var(--text);
  font-size: 12px;
  font-weight: 500;
  text-decoration: none;
  transition: background 0.2s ease;
}

.resume-icon {
  display: flex;
  flex-shrink: 0;
  align-items: center;
  justify-content: center;
  width: 44px;
  height: 54px;
  border-radius: 8px;
  background: linear-gradient(135deg, #fce4ec, #f8bbd0);
  font-size: 24px;
}

.project-card {
  cursor: pointer;
  transition: background 0.2s ease;
}

.project-badge,
.active-badge,
.live-badge {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  width: fit-content;
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.08em;
}

.project-badge {
  margin-bottom: 10px;
  padding: 3px 10px;
  border-radius: 999px;
  background: #f0fdf4;
  color: var(--green);
  letter-spacing: normal;
}

.project-badge-dot {
  background: var(--green);
}

.project-year {
  margin-left: 4px;
  color: #aaaaaa;
}

.project-name,
.edu-name,
.proj-name {
  font-size: 18px;
  font-weight: 700;
}

.project-desc,
.quote-author,
.active-sub,
.proj-tagline,
.page-footer p,
.footer-links a {
  color: var(--muted);
  font-size: 13px;
}

.quote-card {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 20px;
  padding: 28px;
}

.quote-text,
.cta-title {
  font-family: 'DM Serif Display', serif;
}

.quote-text {
  margin: 0;
  color: #333333;
  font-size: 16px;
  line-height: 1.65;
}

.time-card {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 14px;
  padding: 24px 28px;
}

.time-display {
  display: flex;
  align-items: center;
  gap: 12px;
  font-family: 'Space Mono', monospace;
  font-size: 30px;
  font-weight: 700;
}

.moon-icon {
  font-size: 22px;
}

.location-row {
  color: #555555;
  font-size: 14px;
}

.edu-card {
  position: relative;
  gap: 10px;
}

.edu-year {
  position: absolute;
  top: 20px;
  right: 20px;
  color: var(--muted);
  font-size: 13px;
  font-weight: 500;
}

.edu-logo {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 44px;
  height: 44px;
  border-radius: 10px;
  background: #990000;
  color: #ffffff;
  font-size: 18px;
  font-weight: 700;
}

.edu-degree {
  color: var(--muted);
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}

.live-card {
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding: 28px;
}

.live-badge {
  color: var(--accent);
}

.live-title,
.active-title {
  font-size: clamp(2rem, 4vw, 2.15rem);
  font-weight: 700;
  line-height: 1.1;
}

.live-title span,
.active-title span {
  color: var(--blue);
}

.images-card {
  display: flex;
  padding: 0;
}

.img-slot {
  position: relative;
  display: flex;
  flex: 1;
  align-items: center;
  justify-content: center;
  min-height: 180px;
  border-right: 1px solid var(--border);
  overflow: hidden;
  background: #d9d7d2;
}

.img-slot:last-child {
  border-right: 0;
}

.img-label {
  position: absolute;
  bottom: 8px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 1;
  color: rgba(255, 255, 255, 0.82);
  font-size: 10px;
  white-space: nowrap;
}

.gallery-image {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.connect-card {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.connect-btn {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 14px 16px;
  border-radius: 12px;
  background: #f5f5f3;
  font-size: 14px;
  font-weight: 500;
  text-decoration: none;
  transition: background 0.2s ease;
}

.connect-btn svg {
  width: 18px;
  height: 18px;
  fill: none;
  stroke: var(--muted);
  stroke-linecap: round;
  stroke-linejoin: round;
  stroke-width: 1.8;
}

.active-card {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 24px 28px;
}

.active-badge {
  color: var(--green);
}

.section-label {
  margin: 40px 0 16px;
}

.projects-grid {
  grid-template-columns: repeat(3, minmax(0, 1fr));
}

.proj-content {
  padding: 22px 20px 14px;
}

.proj-tagline {
  margin-bottom: 16px;
}

.proj-screenshot {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 160px;
  overflow: hidden;
  background: #f0f0ee;
}

.proj-screenshot.blue {
  background: #f0f4ff;
}

.proj-screenshot.green {
  background: #f0fdf4;
}

.fake-ui {
  width: 90%;
  overflow: hidden;
  border: 1px solid #dddddd;
  border-radius: 6px;
  background: #ffffff;
  font-size: 9px;
}

.fake-ui-bar {
  padding: 5px 8px;
  background: #990000;
  color: #ffffff;
  font-size: 9px;
  font-weight: 600;
}

.fake-ui-bar.facebook {
  background: #1877f2;
}

.fake-ui-row {
  display: flex;
  gap: 6px;
  padding: 5px 8px;
  border-bottom: 1px solid #eeeeee;
}

.fake-ui-cell {
  flex: 1;
  height: 6px;
  border-radius: 3px;
  background: #eeeeee;
}

.fake-ui-cell.accent {
  background: #d0e8ff;
}

.emoji-shot {
  font-size: 52px;
}

.cta-section {
  margin-top: 14px;
}

.cta-card {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 70px 60px;
  border-radius: 20px;
  background:
    radial-gradient(circle at top right, rgba(249, 115, 22, 0.18), transparent 28%),
    linear-gradient(180deg, #101010 0%, #151515 100%);
}

.cta-label {
  margin-bottom: 0;
  color: #666666;
}

.cta-title {
  color: #2f2f2f;
  font-size: clamp(3.2rem, 8vw, 4.5rem);
  line-height: 1.02;
  font-weight: 400;
}

.cta-link {
  width: fit-content;
  margin-top: 8px;
  color: #666666;
  font-size: 14px;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-decoration: underline;
  text-transform: uppercase;
  text-underline-offset: 4px;
  transition: color 0.2s ease;
}

.cta-link:hover {
  color: #ffffff;
}

.page-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1050px;
  margin: 0 auto;
  padding: 30px 20px 40px;
  border-top: 1px solid var(--border);
}

.page-footer p {
  margin: 0 0 4px;
}

.page-footer span {
  color: #aaaaaa;
  font-size: 11px;
}

.footer-links a {
  text-decoration: none;
  transition: color 0.2s ease;
}

.footer-links a:hover {
  color: var(--text);
}

.cursor {
  display: inline-block;
  width: 2px;
  height: 1em;
  margin-left: 2px;
  background: var(--text);
  vertical-align: middle;
  animation: blink 1s step-end infinite;
}

.card:nth-child(1),
.project-big-card:nth-child(1) {
  animation-delay: 0.05s;
}

.card:nth-child(2),
.project-big-card:nth-child(2) {
  animation-delay: 0.1s;
}

.card:nth-child(3),
.project-big-card:nth-child(3) {
  animation-delay: 0.15s;
}

@keyframes blink {
  0%,
  100% {
    opacity: 1;
  }

  50% {
    opacity: 0;
  }
}

@keyframes fade-up {
  from {
    opacity: 0;
    transform: translateY(16px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (max-width: 900px) {
  .projects-grid,
  .row-4 {
    grid-template-columns: 1fr;
  }

  .third-row {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 768px) {
  .top-layout,
  .row-2,
  .row-half-big,
  .third-row,
  .projects-grid,
  .row-4,
  .exp-logos {
    grid-template-columns: 1fr;
  }

  .mini-card-row {
    grid-template-columns: 1fr;
  }

  .portfolio-main {
    padding-top: 92px;
  }

  .top-nav {
    width: calc(100% - 24px);
    justify-content: center;
  }

  .hero-card,
  .cta-card {
    padding: 32px 24px;
  }

  .resume-card,
  .page-footer,
  .images-card {
    flex-direction: column;
    align-items: flex-start;
  }

  .images-card {
    width: 100%;
  }

  .img-slot {
    width: 100%;
    min-height: 140px;
    border-right: 0;
    border-bottom: 1px solid var(--border);
  }

  .img-slot:last-child {
    border-bottom: 0;
  }

  .page-footer {
    gap: 16px;
  }
}
</style>
