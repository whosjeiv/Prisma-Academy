<script setup lang="ts">
import { ref, computed } from 'vue'

interface Slide {
  number: string
  title: string
  subtitle: string
  description: string
  badgeText: string
  badgeClass: string
  quote: string
  quoteAuthor: string
  bgColor: string
}

const slides = ref<Slide[]>([
  {
    number: '01',
    title: 'Filosofía Educativa y Misión',
    subtitle: 'Un nuevo paradigma en la educación universitaria.',
    description: 'En PRISMA UNIVERSITY redefinimos el aprendizaje superior. Eliminamos la memorización pasiva y la sustituimos por el pensamiento crítico riguroso, la investigación aplicada desde el primer año y el trabajo colaborativo interdisciplinario.',
    badgeText: 'Misión Académica',
    badgeClass: 'badge-green',
    quote: '“No enseñamos qué pensar, sino cómo estructurar el pensamiento libre.”',
    quoteAuthor: 'Dr. Alejandro Silva, Rector de PRISMA',
    bgColor: 'var(--pastel-green-bg)'
  },
  {
    number: '02',
    title: 'Investigación sin Fronteras',
    subtitle: 'El conocimiento no se detiene en los libros de texto.',
    description: 'Nuestros laboratorios y semilleros de investigación están abiertos a todos los estudiantes. Fomentamos la publicación académica temprana y el desarrollo de proyectos tecnológicos con impacto social real.',
    badgeText: 'Investigación Abierta',
    badgeClass: 'badge-blue',
    quote: '“La teoría cobra vida cuando se aplica a resolver los problemas de nuestra comunidad.”',
    quoteAuthor: 'Dra. Elena Rostova, Dir. de Investigación',
    bgColor: 'var(--pastel-blue-bg)'
  },
  {
    number: '03',
    title: 'Aulas Digitales & Entorno Físico',
    subtitle: 'Integración tecnológica al servicio de la ergonomía cognitiva.',
    description: 'Combinamos un campus físico minimalista con plataformas digitales hiper-funcionales de diseño plano. Sin ruidos visuales, sin distracciones: un ecosistema enfocado al estudio eficiente y al flujo libre de ideas.',
    badgeText: 'Infraestructura',
    badgeClass: 'badge-lavender',
    quote: '“El espacio de estudio define la calidad del pensamiento y la concentración.”',
    quoteAuthor: 'Arq. Clara Fuentes, Diseñadora de Espacios PRISMA',
    bgColor: 'var(--pastel-lavender-bg)'
  },
  {
    number: '04',
    title: 'Comunidad & Cultura Activa',
    subtitle: 'El aprendizaje es un evento social y constructivo.',
    description: 'Promovemos coloquios de debate semanal, clubes de programación, talleres de diseño y simposios de filosofía autogestionados. En PRISMA, la formación extracurricular es tan fundamental como la malla formal.',
    badgeText: 'Cultura Prisma',
    badgeClass: 'badge-clay',
    quote: '“La universidad no es solo un lugar al que asistes; es una red de colaboración activa.”',
    quoteAuthor: 'Mateo Ortiz, Presidente de la Unión Estudiantil',
    bgColor: 'var(--pastel-clay-bg)'
  }
])

const currentSlide = ref(0)

const currentSlideData = computed<Slide>(() => {
  return (slides.value[currentSlide.value] || slides.value[0]) as Slide
})

const nextSlide = () => {
  currentSlide.value = (currentSlide.value + 1) % slides.value.length
}

const prevSlide = () => {
  currentSlide.value = (currentSlide.value - 1 + slides.value.length) % slides.value.length
}

const setSlide = (index: number) => {
  currentSlide.value = index
}
</script>

<template>
  <div class="carousel-wrapper flat-border">
    <!-- Carousel Grid -->
    <div class="carousel-grid">
      <!-- Left Content (Text) -->
      <div class="carousel-content">
        <div class="carousel-header">
          <span :class="['badge-flat', currentSlideData.badgeClass]">{{ currentSlideData.badgeText }}</span>
          <span class="slide-counter">{{ currentSlideData.number }} / 04</span>
        </div>

        <div class="slide-body">
          <h3 class="slide-title">{{ currentSlideData.title }}</h3>
          <p class="slide-subtitle">{{ currentSlideData.subtitle }}</p>
          <p class="slide-description">{{ currentSlideData.description }}</p>
        </div>

        <!-- Controls -->
        <div class="carousel-controls">
          <div class="nav-arrows">
            <button @click="prevSlide" class="nav-arrow-btn flat-border" aria-label="Slide anterior">
              <svg viewBox="0 0 24 24" width="20" height="20">
                <path fill="currentColor" d="M15.41 16.58L10.83 12l4.58-4.59L14 6l-6 6 6 6z"/>
              </svg>
            </button>
            <button @click="nextSlide" class="nav-arrow-btn flat-border" aria-label="Siguiente slide">
              <svg viewBox="0 0 24 24" width="20" height="20">
                <path fill="currentColor" d="M8.59 16.58L13.17 12 8.59 7.41 10 6l6 6-6 6z"/>
              </svg>
            </button>
          </div>

          <!-- Dot Indicators -->
          <div class="nav-dots">
            <button 
              v-for="(slide, index) in slides" 
              :key="index"
              @click="setSlide(index)"
              class="dot-btn flat-border"
              :class="{ active: currentSlide === index }"
              :aria-label="`Ir al slide ${index + 1}`"
            ></button>
          </div>
        </div>
      </div>

      <!-- Right Visual Column (Muted Colors, Quotes, Minimalist Graphic Layout) -->
      <div class="carousel-visual" :style="{ backgroundColor: currentSlideData.bgColor }">
        <div class="visual-inner">
          <span class="decorative-quotes" aria-hidden="true">“</span>
          <div class="quote-text">{{ currentSlideData.quote }}</div>
          <div class="quote-author">— {{ currentSlideData.quoteAuthor }}</div>
          
          <!-- Decorative flat grid layout -->
          <div class="decorative-grid">
            <div class="grid-line horizontal"></div>
            <div class="grid-line vertical"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.carousel-wrapper {
  background-color: var(--color-bg);
  width: 100%;
  overflow: hidden;
}

.carousel-grid {
  display: grid;
  grid-template-columns: 1.2fr 0.8fr;
  min-height: 480px;
}

@media (max-width: 900px) {
  .carousel-grid {
    grid-template-columns: 1fr;
    min-height: auto;
  }
}

/* Content Left Column */
.carousel-content {
  padding: 3rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  position: relative;
}

@media (max-width: 600px) {
  .carousel-content {
    padding: 1.5rem;
  }
}

.carousel-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
}

.slide-counter {
  font-family: var(--font-sans);
  font-size: 0.9rem;
  font-weight: 500;
  letter-spacing: 0.1em;
  color: var(--color-text-muted);
}

.slide-body {
  margin-bottom: 3rem;
}

.slide-title {
  font-size: 2.2rem;
  font-weight: 500;
  margin-bottom: 0.5rem;
  letter-spacing: -0.01em;
}

.slide-subtitle {
  font-family: var(--font-serif);
  font-style: italic;
  font-size: 1.15rem;
  color: var(--color-text-muted);
  margin-bottom: 1.5rem;
}

.slide-description {
  font-size: 1rem;
  line-height: 1.7;
  max-width: 600px;
  color: var(--color-text-muted);
}

/* Controls */
.carousel-controls {
  display: flex;
  align-items: center;
  gap: 2.5rem;
  flex-wrap: wrap;
}

.nav-arrows {
  display: flex;
  gap: 0.5rem;
}

.nav-arrow-btn {
  background-color: var(--color-bg);
  color: var(--color-text);
  width: 44px;
  height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all var(--transition-fast);
}

.nav-arrow-btn:hover {
  background-color: var(--color-text);
  color: var(--color-bg);
}

.nav-dots {
  display: flex;
  gap: 0.5rem;
}

.dot-btn {
  width: 12px;
  height: 12px;
  background-color: transparent;
  cursor: pointer;
  padding: 0;
  transition: all var(--transition-fast);
}

.dot-btn.active {
  background-color: var(--color-text);
  width: 32px; /* Stretch effect for active */
}

/* Visual Right Column */
.carousel-visual {
  padding: 3rem;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  border-left: 1px solid var(--color-border);
  transition: background-color var(--transition-normal);
}

@media (max-width: 900px) {
  .carousel-visual {
    border-left: none;
    border-top: 1px solid var(--color-border);
    padding: 2.5rem 1.5rem;
    min-height: 250px;
  }
}

.visual-inner {
  max-width: 350px;
  position: relative;
  z-index: 2;
  text-align: center;
}

.decorative-quotes {
  font-family: var(--font-serif);
  font-size: 6rem;
  line-height: 1;
  position: absolute;
  top: -45px;
  left: -20px;
  opacity: 0.08;
  color: var(--color-text);
}

.quote-text {
  font-family: var(--font-serif);
  font-style: italic;
  font-size: 1.25rem;
  color: var(--color-text);
  line-height: 1.5;
  margin-bottom: 1rem;
}

.quote-author {
  font-family: var(--font-sans);
  font-size: 0.8rem;
  font-weight: 500;
  color: var(--color-text-muted);
  letter-spacing: 0.05em;
  text-transform: uppercase;
}

/* Decorative Flat Lines inside visual */
.decorative-grid {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 1;
}

.grid-line {
  position: absolute;
  background-color: rgba(28, 27, 25, 0.05);
}

.grid-line.horizontal {
  width: 100%;
  height: 1px;
  top: 30%;
  left: 0;
}

.grid-line.vertical {
  width: 1px;
  height: 100%;
  left: 30%;
  top: 0;
}
</style>
