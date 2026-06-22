<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const activeMenu = ref<string | null>(null)
const activeSubMenu = ref<string | null>(null)
const mobileMenuOpen = ref(false)
const paperToneOpen = ref(false)

// Paper tone themes
type PaperTone = 'ivory' | 'white' | 'papyrus' | 'smoke'

const currentTone = ref<PaperTone>('ivory')

const paperTones: { key: PaperTone; label: string }[] = [
  { key: 'ivory', label: 'Marfil Fino' },
  { key: 'white', label: 'Blanco Puro' },
  { key: 'papyrus', label: 'Papiro Suave' },
  { key: 'smoke', label: 'Humo Mínimo' }
]

const setTone = (tone: PaperTone) => {
  currentTone.value = tone
  // Remove all theme classes from body
  document.body.classList.remove('theme-ivory', 'theme-white', 'theme-papyrus', 'theme-smoke')
  document.body.classList.add(`theme-${tone}`)
  paperToneOpen.value = false
}

const toggleMenu = (menuName: string) => {
  if (activeMenu.value === menuName) {
    activeMenu.value = null
    activeSubMenu.value = null
  } else {
    activeMenu.value = menuName
    activeSubMenu.value = null
  }
}

const toggleSubMenu = (subMenuName: string) => {
  if (activeSubMenu.value === subMenuName) {
    activeSubMenu.value = null
  } else {
    activeSubMenu.value = subMenuName
  }
}

const closeAll = () => {
  activeMenu.value = null
  activeSubMenu.value = null
  mobileMenuOpen.value = false
  paperToneOpen.value = false
}

// Close on click outside
const handleClickOutside = (event: MouseEvent) => {
  const target = event.target as HTMLElement
  if (!target.closest('.navbar-container')) {
    closeAll()
  }
}

onMounted(() => {
  document.addEventListener('click', handleClickOutside)
  // Set default theme class
  document.body.classList.add('theme-ivory')
})

onUnmounted(() => {
  document.removeEventListener('click', handleClickOutside)
})
</script>

<template>
  <nav class="navbar-wrapper flat-border">
    <div class="navbar-container container-custom">
      <!-- Logo -->
      <RouterLink to="/" class="logo-area" @click="closeAll">
        <span class="logo-serif">PRISMA</span>
        <span class="logo-sans">ACADEMY</span>
      </RouterLink>

      <!-- Desktop Links -->
      <div class="nav-links-desktop">
        <!-- Direct Link -->
        <RouterLink to="/" class="nav-link" @click="closeAll">Inicio</RouterLink>

        <!-- Dropdown 1: PRISMA UNIVERSITY -->
        <div class="dropdown-trigger-container">
          <button 
            @click.stop="toggleMenu('university')" 
            class="nav-link dropdown-toggle-btn"
            :class="{ active: activeMenu === 'university' }"
          >
            Universidad
            <svg class="chevron-icon" :class="{ rotated: activeMenu === 'university' }" viewBox="0 0 24 24" width="16" height="16">
              <path fill="currentColor" d="M7.41 8.58L12 13.17l4.59-4.59L18 10l-6 6-6-6z"/>
            </svg>
          </button>
          
          <!-- Level 1 Dropdown -->
          <div v-if="activeMenu === 'university'" class="dropdown-menu flat-border">
            <div class="dropdown-item-with-sub">
              <button @click.stop="toggleSubMenu('about')" class="dropdown-sub-btn" :class="{ active: activeSubMenu === 'about' }">
                Sobre PRISMA
                <svg class="chevron-icon-sub" :class="{ rotated: activeSubMenu === 'about' }" viewBox="0 0 24 24" width="14" height="14">
                  <path fill="currentColor" d="M8.59 16.58L13.17 12 8.59 7.41 10 6l6 6-6 6z"/>
                </svg>
              </button>
              <!-- Level 2 Dropdown (Submenu) -->
              <div v-if="activeSubMenu === 'about'" class="submenu-panel flat-border">
                <a href="#about-philosophy" class="submenu-link" @click="closeAll">Filosofía Educativa</a>
                <a href="#about-campus" class="submenu-link" @click="closeAll">Instalaciones & Campus</a>
                <a href="#about-history" class="submenu-link" @click="closeAll">Historia de PRISMA</a>
              </div>
            </div>

            <div class="dropdown-item-with-sub">
              <button @click.stop="toggleSubMenu('faculties')" class="dropdown-sub-btn" :class="{ active: activeSubMenu === 'faculties' }">
                Facultades
                <svg class="chevron-icon-sub" :class="{ rotated: activeSubMenu === 'faculties' }" viewBox="0 0 24 24" width="14" height="14">
                  <path fill="currentColor" d="M8.59 16.58L13.17 12 8.59 7.41 10 6l6 6-6 6z"/>
                </svg>
              </button>
              <!-- Level 2 Dropdown (Submenu) -->
              <div v-if="activeSubMenu === 'faculties'" class="submenu-panel flat-border">
                <a href="#fac-arts" class="submenu-link" @click="closeAll">Artes & Humanidades</a>
                <a href="#fac-sciences" class="submenu-link" @click="closeAll">Ciencia & Tecnología</a>
                <a href="#fac-business" class="submenu-link" @click="closeAll">Negocios & Innovación</a>
              </div>
            </div>

            <a href="#about-research" class="dropdown-link-item" @click="closeAll">Investigación & Desarrollo</a>
          </div>
        </div>

        <!-- Dropdown 2: SERVICIOS -->
        <div class="dropdown-trigger-container">
          <button 
            @click.stop="toggleMenu('services')" 
            class="nav-link dropdown-toggle-btn"
            :class="{ active: activeMenu === 'services' }"
          >
            Servicios
            <svg class="chevron-icon" :class="{ rotated: activeMenu === 'services' }" viewBox="0 0 24 24" width="16" height="16">
              <path fill="currentColor" d="M7.41 8.58L12 13.17l4.59-4.59L18 10l-6 6-6-6z"/>
            </svg>
          </button>

          <!-- Level 1 Dropdown -->
          <div v-if="activeMenu === 'services'" class="dropdown-menu flat-border">
            <div class="dropdown-item-with-sub">
              <button @click.stop="toggleSubMenu('portal')" class="dropdown-sub-btn" :class="{ active: activeSubMenu === 'portal' }">
                Portal Estudiantil
                <svg class="chevron-icon-sub" :class="{ rotated: activeSubMenu === 'portal' }" viewBox="0 0 24 24" width="14" height="14">
                  <path fill="currentColor" d="M8.59 16.58L13.17 12 8.59 7.41 10 6l6 6-6 6z"/>
                </svg>
              </button>
              <!-- Level 2 Dropdown (Submenu) -->
              <div v-if="activeSubMenu === 'portal'" class="submenu-panel flat-border">
                <RouterLink to="/portal" class="submenu-link" @click="closeAll">Buzón de Tareas</RouterLink>
                <a href="#student-grades" class="submenu-link" @click="closeAll">Calificaciones</a>
                <a href="#student-schedule" class="submenu-link" @click="closeAll">Horarios & Inscripción</a>
              </div>
            </div>

            <div class="dropdown-item-with-sub">
              <button @click.stop="toggleSubMenu('resources')" class="dropdown-sub-btn" :class="{ active: activeSubMenu === 'resources' }">
                Recursos Digitales
                <svg class="chevron-icon-sub" :class="{ rotated: activeSubMenu === 'resources' }" viewBox="0 0 24 24" width="14" height="14">
                  <path fill="currentColor" d="M8.59 16.58L13.17 12 8.59 7.41 10 6l6 6-6 6z"/>
                </svg>
              </button>
              <!-- Level 2 Dropdown (Submenu) -->
              <div v-if="activeSubMenu === 'resources'" class="submenu-panel flat-border">
                <a href="#resource-library" class="submenu-link" @click="closeAll">Biblioteca Central</a>
                <a href="#resource-docs" class="submenu-link" @click="closeAll">Documentación Académica</a>
              </div>
            </div>

            <a href="#resource-support" class="dropdown-link-item" @click="closeAll">Soporte al Estudiante</a>
          </div>
        </div>

        <RouterLink to="/" class="nav-link" @click="closeAll">Admisiones</RouterLink>
      </div>

      <!-- Right Actions: Paper Tone Selector + Portal Button -->
      <div class="nav-action-desktop">
        <!-- Paper Tone Selector -->
        <div class="tone-selector-container">
          <button @click.stop="paperToneOpen = !paperToneOpen" class="tone-selector-btn" aria-label="Cambiar tono de papel">
            <svg viewBox="0 0 24 24" width="18" height="18">
              <path fill="currentColor" d="M12 3c-4.97 0-9 4.03-9 9s4.03 9 9 9c.83 0 1.5-.67 1.5-1.5 0-.39-.15-.74-.39-1.01-.23-.26-.38-.61-.38-1 0-.83.67-1.5 1.5-1.5H16c2.76 0 5-2.24 5-5 0-4.42-4.03-8-9-8zm-5.5 9c-.83 0-1.5-.67-1.5-1.5S5.67 9 6.5 9 8 9.67 8 10.5 7.33 12 6.5 12zm3-4C8.67 8 8 7.33 8 6.5S8.67 5 9.5 5s1.5.67 1.5 1.5S10.33 8 9.5 8zm5 0c-.83 0-1.5-.67-1.5-1.5S13.67 5 14.5 5s1.5.67 1.5 1.5S15.33 8 14.5 8zm3 4c-.83 0-1.5-.67-1.5-1.5S16.67 9 17.5 9s1.5.67 1.5 1.5-.67 1.5-1.5 1.5z"/>
            </svg>
          </button>

          <div v-if="paperToneOpen" class="tone-dropdown flat-border">
            <span class="tone-dropdown-title">Tono de Papel</span>
            <button 
              v-for="tone in paperTones" 
              :key="tone.key" 
              @click="setTone(tone.key)"
              class="tone-option"
              :class="{ active: currentTone === tone.key }"
            >
              <span class="tone-swatch" :class="`swatch-${tone.key}`"></span>
              {{ tone.label }}
            </button>
          </div>
        </div>

        <RouterLink to="/portal" class="flat-button flat-button-dark" @click="closeAll">
          Portal de Tareas
        </RouterLink>
      </div>

      <!-- Mobile Menu Toggle Button -->
      <button @click="mobileMenuOpen = !mobileMenuOpen" class="mobile-toggle-btn">
        <svg v-if="!mobileMenuOpen" viewBox="0 0 24 24" width="24" height="24">
          <path fill="currentColor" d="M3 18h18v-2H3v2zm0-5h18v-2H3v2zm0-7v2h18V6H3z"/>
        </svg>
        <svg v-else viewBox="0 0 24 24" width="24" height="24">
          <path fill="currentColor" d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"/>
        </svg>
      </button>
    </div>

    <!-- Mobile Menu Slideout / Drawer -->
    <div v-if="mobileMenuOpen" class="mobile-menu-panel flat-border">
      <div class="mobile-links-container">
        <RouterLink to="/" class="mobile-nav-link" @click="closeAll">Inicio</RouterLink>
        
        <!-- Mobile Accordeon Level 1 -->
        <div class="mobile-dropdown-section">
          <button @click="toggleMenu('m-university')" class="mobile-nav-link-btn">
            Universidad
            <svg class="chevron-icon" :class="{ rotated: activeMenu === 'm-university' }" viewBox="0 0 24 24" width="16" height="16">
              <path fill="currentColor" d="M7.41 8.58L12 13.17l4.59-4.59L18 10l-6 6-6-6z"/>
            </svg>
          </button>
          
          <div v-if="activeMenu === 'm-university'" class="mobile-submenu-container">
            <button @click="toggleSubMenu('m-about')" class="mobile-sub-link-btn">
              Sobre PRISMA
            </button>
            <div v-if="activeSubMenu === 'm-about'" class="mobile-sub-sub-container">
              <a href="#about-philosophy" class="mobile-sub-link" @click="closeAll">Filosofía Educativa</a>
              <a href="#about-campus" class="mobile-sub-link" @click="closeAll">Instalaciones & Campus</a>
              <a href="#about-history" class="mobile-sub-link" @click="closeAll">Historia de PRISMA</a>
            </div>

            <button @click="toggleSubMenu('m-faculties')" class="mobile-sub-link-btn">
              Facultades
            </button>
            <div v-if="activeSubMenu === 'm-faculties'" class="mobile-sub-sub-container">
              <a href="#fac-arts" class="mobile-sub-link" @click="closeAll">Artes & Humanidades</a>
              <a href="#fac-sciences" class="mobile-sub-link" @click="closeAll">Ciencia & Tecnología</a>
              <a href="#fac-business" class="mobile-sub-link" @click="closeAll">Negocios & Innovación</a>
            </div>
            
            <a href="#about-research" class="mobile-sub-link" @click="closeAll">Investigación</a>
          </div>
        </div>

        <div class="mobile-dropdown-section">
          <button @click="toggleMenu('m-services')" class="mobile-nav-link-btn">
            Servicios
            <svg class="chevron-icon" :class="{ rotated: activeMenu === 'm-services' }" viewBox="0 0 24 24" width="16" height="16">
              <path fill="currentColor" d="M7.41 8.58L12 13.17l4.59-4.59L18 10l-6 6-6-6z"/>
            </svg>
          </button>

          <div v-if="activeMenu === 'm-services'" class="mobile-submenu-container">
            <button @click="toggleSubMenu('m-portal')" class="mobile-sub-link-btn">
              Portal Estudiantil
            </button>
            <div v-if="activeSubMenu === 'm-portal'" class="mobile-sub-sub-container">
              <RouterLink to="/portal" class="mobile-sub-link" @click="closeAll">Buzón de Tareas</RouterLink>
              <a href="#student-grades" class="mobile-sub-link" @click="closeAll">Calificaciones</a>
              <a href="#student-schedule" class="mobile-sub-link" @click="closeAll">Horarios & Inscripción</a>
            </div>

            <button @click="toggleSubMenu('m-resources')" class="mobile-sub-link-btn">
              Recursos Digitales
            </button>
            <div v-if="activeSubMenu === 'm-resources'" class="mobile-sub-sub-container">
              <a href="#resource-library" class="mobile-sub-link" @click="closeAll">Biblioteca Central</a>
              <a href="#resource-docs" class="mobile-sub-link" @click="closeAll">Documentación</a>
            </div>
          </div>
        </div>

        <RouterLink to="/" class="mobile-nav-link" @click="closeAll">Admisiones</RouterLink>

        <!-- Mobile Tone Selector -->
        <div class="mobile-tone-selector">
          <span class="mobile-tone-label">Tono de papel:</span>
          <div class="mobile-tone-options">
            <button 
              v-for="tone in paperTones" 
              :key="tone.key" 
              @click="setTone(tone.key)"
              class="mobile-tone-btn"
              :class="{ active: currentTone === tone.key }"
              :aria-label="tone.label"
            >
              <span class="tone-swatch" :class="`swatch-${tone.key}`"></span>
            </button>
          </div>
        </div>

        <RouterLink to="/portal" class="flat-button flat-button-dark" style="margin-top: 1rem; width: 100%; text-align: center;" @click="closeAll">
          Portal de Tareas
        </RouterLink>
      </div>
    </div>
  </nav>
</template>

<style scoped>
.navbar-wrapper {
  background-color: var(--color-bg);
  border-left: none;
  border-right: none;
  border-top: none;
  position: sticky;
  top: 0;
  z-index: 100;
  width: 100%;
}

.navbar-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 80px;
  position: relative;
}

/* Logo styling */
.logo-area {
  display: flex;
  align-items: baseline;
  gap: 0.3rem;
}

.logo-serif {
  font-family: var(--font-serif);
  font-size: 1.5rem;
  font-weight: 600;
  letter-spacing: -0.01em;
}

.logo-sans {
  font-family: var(--font-sans);
  font-size: 0.8rem;
  font-weight: 500;
  letter-spacing: 0.2em;
  color: var(--color-text-muted);
}

/* Nav Links desktop styling */
.nav-links-desktop {
  display: flex;
  align-items: center;
  gap: 2rem;
  height: 100%;
}

@media (max-width: 900px) {
  .nav-links-desktop,
  .nav-action-desktop {
    display: none;
  }
}

.nav-link {
  font-family: var(--font-sans);
  font-size: 0.9rem;
  font-weight: 500;
  color: var(--color-text-muted);
  cursor: pointer;
  background: none;
  border: none;
  padding: 0.5rem 0;
  display: flex;
  align-items: center;
  gap: 0.25rem;
}

.nav-link:hover, 
.nav-link.active {
  color: var(--color-text);
}

.dropdown-trigger-container {
  position: relative;
  height: 100%;
  display: flex;
  align-items: center;
}

.chevron-icon {
  transition: transform var(--transition-fast);
  opacity: 0.7;
}

.chevron-icon.rotated {
  transform: rotate(180deg);
}

/* Level 1 Dropdown Menus */
.dropdown-menu {
  position: absolute;
  top: 75px;
  left: 0;
  background-color: var(--color-bg);
  min-width: 230px;
  display: flex;
  flex-direction: column;
  padding: 0.5rem 0;
  z-index: 10;
}

.dropdown-link-item, 
.dropdown-sub-btn {
  font-family: var(--font-sans);
  font-size: 0.88rem;
  color: var(--color-text-muted);
  padding: 0.75rem 1.25rem;
  text-align: left;
  background: none;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
}

.dropdown-link-item:hover, 
.dropdown-sub-btn:hover,
.dropdown-sub-btn.active {
  background-color: var(--color-bg-alt);
  color: var(--color-text);
}

/* Level 2 Submenu Panel (Absolute right of level 1) */
.dropdown-item-with-sub {
  position: relative;
}

.submenu-panel {
  position: absolute;
  top: 0;
  left: 100%;
  background-color: var(--color-bg);
  min-width: 220px;
  display: flex;
  flex-direction: column;
  padding: 0.5rem 0;
  margin-left: -1px;
  z-index: 11;
}

.submenu-link {
  font-family: var(--font-sans);
  font-size: 0.85rem;
  color: var(--color-text-muted);
  padding: 0.6rem 1.25rem;
  display: block;
}

.submenu-link:hover {
  background-color: var(--color-bg-alt);
  color: var(--color-text);
}

.chevron-icon-sub {
  opacity: 0.7;
  transition: transform var(--transition-fast);
}

.chevron-icon-sub.rotated {
  transform: rotate(90deg);
}

/* Nav Action Area */
.nav-action-desktop {
  display: flex;
  align-items: center;
  gap: 1rem;
}

/* Paper Tone Selector */
.tone-selector-container {
  position: relative;
}

.tone-selector-btn {
  background: none;
  border: 1px solid var(--color-border-subtle);
  padding: 0.5rem;
  cursor: pointer;
  color: var(--color-text-muted);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all var(--transition-fast);
}

.tone-selector-btn:hover {
  border-color: var(--color-border);
  color: var(--color-text);
}

.tone-dropdown {
  position: absolute;
  top: calc(100% + 8px);
  right: 0;
  background-color: var(--color-bg);
  min-width: 180px;
  display: flex;
  flex-direction: column;
  padding: 0.75rem 0;
  z-index: 20;
}

.tone-dropdown-title {
  font-family: var(--font-sans);
  font-size: 0.7rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--color-text-muted);
  padding: 0 1rem 0.5rem;
  border-bottom: 1px solid var(--color-border-subtle);
  margin-bottom: 0.25rem;
}

.tone-option {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  padding: 0.55rem 1rem;
  background: none;
  border: none;
  cursor: pointer;
  font-family: var(--font-sans);
  font-size: 0.85rem;
  color: var(--color-text-muted);
  width: 100%;
  text-align: left;
  transition: all var(--transition-fast);
}

.tone-option:hover {
  background-color: var(--color-bg-alt);
  color: var(--color-text);
}

.tone-option.active {
  color: var(--color-text);
  font-weight: 500;
}

.tone-swatch {
  width: 16px;
  height: 16px;
  border: 1px solid var(--color-border);
  display: inline-block;
  flex-shrink: 0;
}

.swatch-ivory { background-color: #FAF9F6; }
.swatch-white { background-color: #FFFFFF; }
.swatch-papyrus { background-color: #F0EDE4; }
.swatch-smoke { background-color: #ECECEA; }

/* Mobile Toggle */
.mobile-toggle-btn {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  color: var(--color-text);
}

@media (max-width: 900px) {
  .mobile-toggle-btn {
    display: block;
  }
}

/* Mobile Menu Panel */
.mobile-menu-panel {
  position: absolute;
  top: 80px;
  left: 0;
  right: 0;
  background-color: var(--color-bg);
  border-left: none;
  border-right: none;
  border-bottom: 1px solid var(--color-border);
  padding: 1.5rem;
  z-index: 99;
}

.mobile-links-container {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.mobile-nav-link {
  font-family: var(--font-sans);
  font-weight: 500;
  font-size: 1.1rem;
  padding: 0.5rem 0;
  border-bottom: 1px solid var(--color-border-subtle);
  color: var(--color-text);
}

.mobile-dropdown-section {
  display: flex;
  flex-direction: column;
}

.mobile-nav-link-btn {
  font-family: var(--font-sans);
  font-weight: 500;
  font-size: 1.1rem;
  padding: 0.5rem 0;
  border: none;
  border-bottom: 1px solid var(--color-border-subtle);
  background: none;
  text-align: left;
  color: var(--color-text);
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  cursor: pointer;
}

.mobile-submenu-container {
  padding-left: 1rem;
  margin-top: 0.5rem;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.mobile-sub-link-btn {
  font-family: var(--font-sans);
  font-weight: 500;
  font-size: 0.95rem;
  padding: 0.4rem 0;
  border: none;
  background: none;
  text-align: left;
  color: var(--color-text-muted);
  cursor: pointer;
}

.mobile-sub-sub-container {
  padding-left: 1rem;
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
  border-left: 1px solid var(--color-border-subtle);
  margin-bottom: 0.5rem;
}

.mobile-sub-link {
  font-size: 0.88rem;
  color: var(--color-text-muted);
  padding: 0.2rem 0;
}

.mobile-sub-link:hover {
  color: var(--color-text);
}

/* Mobile Tone Selector */
.mobile-tone-selector {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.75rem 0;
  border-top: 1px solid var(--color-border-subtle);
  margin-top: 0.5rem;
}

.mobile-tone-label {
  font-size: 0.8rem;
  font-weight: 500;
  color: var(--color-text-muted);
  white-space: nowrap;
}

.mobile-tone-options {
  display: flex;
  gap: 0.5rem;
}

.mobile-tone-btn {
  background: none;
  border: 1px solid var(--color-border-subtle);
  padding: 0.3rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all var(--transition-fast);
}

.mobile-tone-btn.active {
  border-color: var(--color-border);
}
</style>
