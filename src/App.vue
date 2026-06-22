<script setup lang="ts">
import { RouterView } from 'vue-router'
import Navbar from './components/Navbar.vue'
</script>

<template>
  <div class="app-layout">
    <!-- Global Navbar -->
    <Navbar />

    <!-- Main Content View -->
    <div class="main-content">
      <RouterView />
    </div>

    <!-- Editorial Footer -->
    <footer class="footer-wrapper flat-border">
      <div class="footer-container container-custom">
        <div class="footer-brand">
          <span class="brand-serif">PRISMA</span>
          <span class="brand-sans">UNIVERSITY</span>
          <p class="brand-copy">&copy; 2026 PRISMA Academy. Todos los derechos reservados.</p>
        </div>
        <div class="footer-meta">
          <div class="meta-section">
            <span class="meta-title">Ubicación</span>
            <span class="meta-val">Distrito Académico Prisma, Pabellón Central</span>
          </div>
          <div class="meta-section">
            <span class="meta-title">Contacto</span>
            <span class="meta-val">contacto@prisma.edu</span>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<style scoped>
.app-layout {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.main-content {
  flex-grow: 1;
}

/* Footer Styling */
.footer-wrapper {
  background-color: var(--color-bg-alt);
  border-left: none;
  border-right: none;
  border-bottom: none;
  padding: 4rem 0;
  margin-top: 8rem;
}

.footer-container {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  flex-wrap: wrap;
  gap: 3rem;
}

.brand-serif {
  font-family: var(--font-serif);
  font-size: 1.4rem;
  font-weight: 600;
  display: inline-block;
  margin-right: 0.3rem;
}

.brand-sans {
  font-family: var(--font-sans);
  font-size: 0.8rem;
  font-weight: 500;
  letter-spacing: 0.2em;
  color: var(--color-text-muted);
}

.brand-copy {
  margin-top: 1rem;
  font-size: 0.85rem;
  color: var(--color-text-muted);
}

.footer-meta {
  display: flex;
  gap: 4rem;
  flex-wrap: wrap;
}

@media (max-width: 600px) {
  .footer-meta {
    gap: 2rem;
  }
}

.meta-section {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.meta-title {
  font-family: var(--font-sans);
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  color: var(--color-text);
  letter-spacing: 0.05em;
}

.meta-val {
  font-size: 0.88rem;
  color: var(--color-text-muted);
}
</style>
