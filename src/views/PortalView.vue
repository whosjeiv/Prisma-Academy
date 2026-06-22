<script setup lang="ts">
import StudentHelper from '../components/StudentHelper.vue'
</script>

<template>
  <div class="portal-view-wrapper container-custom fade-in-element">
    <!-- Archival header for print style -->
    <header class="portal-view-header">
      <div class="archival-meta">
        <span class="archival-code">[PORTAL-01 // CENTRAL DE ENTREGAS]</span>
        <span class="archival-date">SISTEMA INTEGRADO PRISMA</span>
      </div>
      <div class="grid-line horizontal-thick"></div>
      <div class="header-main-grid">
        <div class="title-area">
          <h1 class="portal-title">Buzón Digital & Recursos</h1>
          <p class="portal-subtitle">Gestión simplificada de asignaciones académicas.</p>
        </div>
        <div class="info-area">
          <p class="editorial-disclaimer">
            Esta interfaz opera bajo el principio de minimalismo formal. Toda actividad se audita digitalmente generando comprobantes cifrados con validez interna en PRISMA UNIVERSITY.
          </p>
        </div>
      </div>
      <div class="grid-line horizontal-thin"></div>
    </header>

    <!-- Student Helper Component -->
    <StudentHelper />
  </div>
</template>

<style scoped>
.portal-view-wrapper {
  padding-top: 3rem;
  min-height: 80vh;
}

.portal-view-header {
  margin-bottom: 2rem;
}

.archival-meta {
  display: flex;
  justify-content: space-between;
  font-family: var(--font-sans);
  font-size: 0.75rem;
  font-weight: 600;
  color: var(--color-text-muted);
  letter-spacing: 0.08em;
  padding-bottom: 0.5rem;
}

.grid-line {
  background-color: var(--color-border);
}

.grid-line.horizontal-thick {
  height: 2px;
  width: 100%;
}

.grid-line.horizontal-thin {
  height: 1px;
  width: 100%;
  margin-top: 2rem;
}

.header-main-grid {
  display: grid;
  grid-template-columns: 1.2fr 0.8fr;
  gap: 2rem;
  padding: 1.5rem 0;
}

@media (max-width: 768px) {
  .header-main-grid {
    grid-template-columns: 1fr;
    gap: 1rem;
    padding: 1rem 0;
  }
}

.portal-title {
  font-size: 2.8rem;
  font-weight: 500;
  letter-spacing: -0.01em;
  line-height: 1.1;
}

.portal-subtitle {
  font-family: var(--font-serif);
  font-style: italic;
  font-size: 1.15rem;
  color: var(--color-text-muted);
  margin-top: 0.25rem;
}

.editorial-disclaimer {
  font-size: 0.85rem;
  color: var(--color-text-muted);
  border-left: 1px solid var(--color-border);
  padding-left: 1rem;
  line-height: 1.5;
}

@media (max-width: 768px) {
  .editorial-disclaimer {
    border-left: none;
    border-top: 1px solid var(--color-border-subtle);
    padding-left: 0;
    padding-top: 1rem;
  }
}
</style>
