<script setup lang="ts">
import InfoCarousel from '../components/InfoCarousel.vue'
</script>

<template>
  <main class="home-wrapper fade-in-element">
    <!-- Hero Section -->
    <header class="hero-section container-custom">
      <!-- Archival reference tag -->
      <div class="section-index-tag">[SEC-01 // INF-HERO]</div>
      <div class="editorial-grid">
        <!-- Hero Text (Left) -->
        <div class="hero-text-col">
          <span class="badge-flat badge-green hero-badge">Admisiones Abiertas 2026</span>
          <h1 class="editorial-title-large hero-title">
            Pensamiento crítico. <br />
            Rigor formal. <br />
            Estructura plana.
          </h1>
          <p class="hero-description">
            Prisma Academy es el entorno digital oficial de <strong>PRISMA UNIVERSITY</strong>. 
            Diseñado bajo una filosofía de minimalismo cognitivo, descartamos las distracciones visuales y los neones decorativos para ofrecer una plataforma centrada en la lectura profunda, la escritura clara y la entrega eficiente de tus asignaciones universitarias.
          </p>
          <div class="hero-actions">
            <RouterLink to="/portal" class="flat-button flat-button-dark">Ir al Portal de Tareas</RouterLink>
            <a href="#about-prisma" class="flat-button">Conocer la Universidad</a>
          </div>
        </div>

        <!-- Hero Graphic/Quote (Right) -->
        <div class="hero-quote-col">
          <!-- Editorial border layout -->
          <div class="editorial-quote-box flat-border">
            <span class="quote-symbol">«</span>
            <blockquote class="hero-quote">
              La arquitectura del entorno digital en el que un estudiante trabaja influye de manera determinante en la claridad y profundidad de su desarrollo intelectual.
            </blockquote>
            <cite class="hero-quote-author">— Consejo de Decanos, PRISMA UNIVERSITY</cite>
            
            <div class="quick-stats-flat">
              <div class="stat-flat-item">
                <span class="stat-num">100%</span>
                <span class="stat-lbl">Libre de Ruido</span>
              </div>
              <div class="stat-flat-item">
                <span class="stat-num">04</span>
                <span class="stat-lbl">Facultades Clave</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </header>

    <!-- Manifesto / Core Principles Section -->
    <section class="manifesto-section container-custom">
      <div class="section-divider-grid">
        <div class="grid-line horizontal-thick"></div>
        <div class="section-index-tag">[SEC-02 // MANIFESTO-PRISMA]</div>
      </div>
      <div class="manifesto-header">
        <span class="badge-flat badge-blue">Nuestros Fundamentos</span>
        <h2 class="section-title">Una universidad diseñada de manera diferente</h2>
      </div>
      
      <div class="principles-grid">
        <div class="principle-card flat-border">
          <div class="principle-num">I</div>
          <h3 class="principle-title">Enfoque Académico Puro</h3>
          <p class="principle-desc">Frente a la sobreestimulación de los portales modernos, abogamos por un diseño institucional plano y tipografía de alta legibilidad que permita enfocar la mente en el estudio activo.</p>
        </div>
        <div class="principle-card flat-border">
          <div class="principle-num">II</div>
          <h3 class="principle-title">Estructura Descentralizada</h3>
          <p class="principle-desc">El estudiante es el centro de su propio trayecto. Cada semestre se organiza en base a coloquios interdisciplinarios y proyectos entregables sin exámenes estandarizados.</p>
        </div>
        <div class="principle-card flat-border">
          <div class="principle-num">III</div>
          <h3 class="principle-title">Rastreo de Entregas Claro</h3>
          <p class="principle-desc">Sistemas transparentes de envío. Cada tarea entregada genera un comprobante único de auditoría digital, garantizando la fiabilidad de las entregas académicas de los alumnos.</p>
        </div>
      </div>
    </section>

    <!-- Carousel Section (PRISMA UNIVERSITY information) -->
    <section class="university-info-section container-custom" id="about-prisma">
      <div class="section-divider-grid">
        <div class="grid-line horizontal-thick"></div>
        <div class="section-index-tag">[SEC-03 // REGISTRO-CAMPUS]</div>
      </div>
      <div class="carousel-section-header">
        <div class="header-left">
          <span class="badge-flat badge-clay">Explora el Campus</span>
          <h2 class="section-title">PRISMA UNIVERSITY</h2>
          <p class="section-intro">Descubre los pilares que sostienen nuestra institución educativa.</p>
        </div>
      </div>
      
      <!-- Component Carrusel -->
      <InfoCarousel />
    </section>
  </main>
</template>

<style scoped>
.home-wrapper {
  padding-top: 3rem;
}

.section-index-tag {
  font-family: var(--font-sans);
  font-size: 0.72rem;
  font-weight: 600;
  letter-spacing: 0.1em;
  color: var(--color-text-muted);
  margin-top: 0.5rem;
  margin-bottom: 1.5rem;
}

/* Hero Section */
.hero-section {
  padding-bottom: 4rem;
}

.hero-section .section-index-tag {
  margin-top: -1.5rem;
  margin-bottom: 2rem;
}

.hero-badge {
  margin-bottom: 1.5rem;
}

.hero-text-col {
  grid-column: span 7;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.hero-title {
  margin-bottom: 2rem;
}

.hero-description {
  font-size: 1.15rem;
  line-height: 1.7;
  color: var(--color-text-muted);
  max-width: 620px;
  margin-bottom: 2.5rem;
}

.hero-actions {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
}

.hero-quote-col {
  grid-column: span 5;
  display: flex;
  align-items: center;
  justify-content: flex-end;
}

@media (max-width: 1024px) {
  .hero-quote-col {
    justify-content: flex-start;
    margin-top: 2rem;
  }
}

.editorial-quote-box {
  padding: 3rem 2.5rem;
  background-color: var(--color-bg-alt);
  position: relative;
  max-width: 440px;
}

.quote-symbol {
  font-family: var(--font-serif);
  font-size: 6rem;
  line-height: 1;
  position: absolute;
  top: -20px;
  left: 15px;
  opacity: 0.05;
  pointer-events: none;
}

.hero-quote {
  font-family: var(--font-serif);
  font-size: 1.2rem;
  font-style: italic;
  line-height: 1.6;
  color: var(--color-text);
  margin-bottom: 1.5rem;
}

.hero-quote-author {
  font-size: 0.8rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: var(--color-text-muted);
  display: block;
  border-bottom: 1px solid var(--color-border-subtle);
  padding-bottom: 1.5rem;
  margin-bottom: 1.5rem;
}

.quick-stats-flat {
  display: flex;
  gap: 2.5rem;
}

.stat-flat-item {
  display: flex;
  flex-direction: column;
}

.stat-num {
  font-family: var(--font-serif);
  font-size: 1.8rem;
  font-weight: 500;
  line-height: 1.1;
}

.stat-lbl {
  font-size: 0.75rem;
  font-weight: 500;
  color: var(--color-text-muted);
  text-transform: uppercase;
  margin-top: 0.2rem;
}

/* Sections Dividers & Headers */
.section-divider-grid {
  width: 100%;
  margin-top: 6rem;
  margin-bottom: 3rem;
}

.grid-line {
  background-color: var(--color-border);
}

.grid-line.horizontal-thick {
  width: 100%;
  height: 1px;
}

.section-title {
  font-size: 2.5rem;
  margin-top: 0.5rem;
  letter-spacing: -0.01em;
}

.section-intro {
  font-size: 1.05rem;
  color: var(--color-text-muted);
  margin-top: 0.5rem;
}

/* Manifesto Section */
.manifesto-section {
  margin-top: 2rem;
}

.manifesto-header {
  margin-bottom: 3rem;
  max-width: 800px;
}

.principles-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2rem;
  border-bottom: 1px solid var(--color-border-subtle);
  padding-bottom: 4rem;
}

@media (max-width: 850px) {
  .principles-grid {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }
}

.principle-card {
  padding: 2.5rem;
  background-color: var(--color-bg);
  transition: transform var(--transition-fast), box-shadow var(--transition-fast), background-color var(--transition-fast);
  box-shadow: 0 0 0 var(--color-border);
}

.principle-card:hover {
  background-color: var(--color-bg-alt);
  transform: translate(-3px, -3px);
  box-shadow: 3px 3px 0px var(--color-border);
}


.principle-num {
  font-family: var(--font-serif);
  font-size: 2.2rem;
  font-style: italic;
  color: var(--color-text-muted);
  margin-bottom: 1.5rem;
  opacity: 0.5;
}

.principle-title {
  font-size: 1.35rem;
  font-weight: 500;
  margin-bottom: 0.75rem;
  font-family: var(--font-sans);
}

.principle-desc {
  font-size: 0.95rem;
  color: var(--color-text-muted);
  line-height: 1.6;
}

/* Carousel Section Specific */
.university-info-section {
  margin-top: 2rem;
  padding-bottom: 4rem;
}

.carousel-section-header {
  margin-bottom: 3rem;
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
}
</style>
