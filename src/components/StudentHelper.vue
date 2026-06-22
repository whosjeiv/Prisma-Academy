<script setup lang="ts">
import { ref } from 'vue'

interface Task {
  id: number
  title: string
  subject: string
  dueDate: string
  badgeClass: string
  badgeText: string
  status: 'pending' | 'submitted'
}

const subjects = ref([
  'Arquitectura de la Información',
  'Introducción al Pensamiento Crítico',
  'Historia del Diseño Gráfico',
  'Teoría de Sistemas Complejos'
])

const pendingTasks = ref<Task[]>([
  {
    id: 1,
    title: 'Análisis de Flujos de Navegación Académica',
    subject: 'Arquitectura de la Información',
    dueDate: '25 Junio, 23:59',
    badgeClass: 'badge-lavender',
    badgeText: 'Crítico',
    status: 'pending'
  },
  {
    id: 2,
    title: 'Ensayo sobre Dialéctica y Democracia',
    subject: 'Introducción al Pensamiento Crítico',
    dueDate: '28 Junio, 18:00',
    badgeClass: 'badge-clay',
    badgeText: 'Teórico',
    status: 'pending'
  },
  {
    id: 3,
    title: 'Investigación Estética de la Escuela Bauhaus',
    subject: 'Historia del Diseño Gráfico',
    dueDate: '02 Julio, 23:59',
    badgeClass: 'badge-green',
    badgeText: 'Práctico',
    status: 'pending'
  }
])

// Submission Form State
const selectedSubject = ref<string>(subjects.value[0] || '')
const selectedTaskId = ref<number | 'custom'>(pendingTasks.value[0]?.id || 'custom')
const customTaskTitle = ref('')
const fileName = ref('')
const fileSize = ref('')
const isDragging = ref(false)
const isSubmitting = ref(false)
const submissionSuccess = ref(false)
const submissionReceipt = ref<{
  id: string
  taskName: string
  subject: string
  timestamp: string
  file: string
} | null>(null)

// History of submissions in current session
const submittedTasks = ref<Array<any>>([])

const handleFileSelect = (event: Event) => {
  const target = event.target as HTMLInputElement
  if (target.files && target.files.length > 0) {
    const file = target.files[0]
    if (file) {
      fileName.value = file.name
      fileSize.value = (file.size / 1024).toFixed(1) + ' KB'
    }
  }
}

const handleDragOver = (e: DragEvent) => {
  e.preventDefault()
  isDragging.value = true
}

const handleDragLeave = () => {
  isDragging.value = false
}

const handleDrop = (e: DragEvent) => {
  e.preventDefault()
  isDragging.value = false
  if (e.dataTransfer && e.dataTransfer.files.length > 0) {
    const file = e.dataTransfer.files[0]
    if (file) {
      fileName.value = file.name
      fileSize.value = (file.size / 1024).toFixed(1) + ' KB'
    }
  }
}

const triggerFileSelect = () => {
  const input = document.getElementById('file-input') as HTMLInputElement
  if (input) input.click()
}

const removeFile = () => {
  fileName.value = ''
  fileSize.value = ''
}

const submitTask = () => {
  if (!fileName.value) return
  
  isSubmitting.value = true
  
  // Simulate network delay
  setTimeout(() => {
    isSubmitting.value = false
    submissionSuccess.value = true
    
    let taskName = ''
    if (selectedTaskId.value === 'custom') {
      taskName = customTaskTitle.value || 'Tarea Personalizada'
    } else {
      const task = pendingTasks.value.find(t => t.id === selectedTaskId.value)
      if (task) {
        taskName = task.title
        task.status = 'submitted'
      } else {
        taskName = 'Tarea Académica'
      }
    }
    
    const now = new Date()
    const timestampStr = now.toLocaleDateString() + ' ' + now.toLocaleTimeString()
    const receiptId = 'PRISMA-' + Math.random().toString(36).substr(2, 9).toUpperCase()
    
    submissionReceipt.value = {
      id: receiptId,
      taskName,
      subject: selectedSubject.value,
      timestamp: timestampStr,
      file: fileName.value
    }
    
    submittedTasks.value.unshift({
      id: receiptId,
      title: taskName,
      subject: selectedSubject.value,
      timestamp: timestampStr,
      file: fileName.value
    })
  }, 1000)
}

const resetForm = () => {
  fileName.value = ''
  fileSize.value = ''
  submissionSuccess.value = false
  submissionReceipt.value = null
  customTaskTitle.value = ''
  
  // Find next pending task to select
  const nextPending = pendingTasks.value.find(t => t.status === 'pending')
  if (nextPending) {
    selectedTaskId.value = nextPending.id
    selectedSubject.value = nextPending.subject
  } else {
    selectedTaskId.value = 'custom'
  }
}

const updateSubjectFromTask = () => {
  if (selectedTaskId.value !== 'custom') {
    const task = pendingTasks.value.find(t => t.id === selectedTaskId.value)
    if (task) {
      selectedSubject.value = task.subject
    }
  }
}
</script>

<template>
  <div class="helper-container" id="student-lms">
    <div class="helper-header">
      <span class="badge-flat badge-lavender">Portal del Alumno</span>
      <h2 class="helper-title">Herramientas & Simulador de Entregas</h2>
      <p class="helper-intro">Accede a tus recursos académicos o experimenta la sencillez del buzón de entrega de tareas diseñado para Prisma Academy.</p>
    </div>

    <div class="editorial-grid">
      <!-- Left Column: Tasks List & Resources links -->
      <div class="left-col">
        <div class="panel-section flat-border">
          <h3 class="panel-title">Tus Pendientes</h3>
          <p class="panel-sub">Tareas activas para esta semana</p>
          
          <div class="task-list">
            <div 
              v-for="task in pendingTasks" 
              :key="task.id" 
              class="task-item"
              :class="{ 'task-submitted-line': task.status === 'submitted' }"
            >
              <div class="task-info">
                <span :class="['badge-flat', task.badgeClass, 'task-badge']">{{ task.badgeText }}</span>
                <span class="task-subject">{{ task.subject }}</span>
                <h4 class="task-item-title">{{ task.title }}</h4>
                <div class="task-date">Fecha límite: {{ task.dueDate }}</div>
              </div>
              <div class="task-action-status">
                <span v-if="task.status === 'submitted'" class="badge-flat badge-green">Entregado</span>
                <button 
                  v-else 
                  @click="selectedTaskId = task.id; selectedSubject = task.subject; submissionSuccess = false" 
                  class="select-task-btn"
                  :class="{ 'selected': selectedTaskId === task.id }"
                >
                  Entregar
                </button>
              </div>
            </div>
          </div>
        </div>

        <!-- Student Quick Links -->
        <div class="quick-links-grid">
          <a href="#resource-library" class="flat-card quick-link-card">
            <h4 class="card-title">Biblioteca Central</h4>
            <p class="card-desc">Búsqueda de revistas y libros digitales.</p>
          </a>
          <a href="#student-schedule" class="flat-card quick-link-card">
            <h4 class="card-title">Horario Académico</h4>
            <p class="card-desc">Consulta tus asignaturas y aulas asignadas.</p>
          </a>
        </div>
      </div>

      <!-- Right Column: Interactive Dropzone simulator -->
      <div class="right-col" id="lms-demo">
        <div class="panel-section simulator-panel flat-border">
          <div class="simulator-header">
            <span class="simulator-indicator">SIMULADOR ACTIVO</span>
            <h3 class="panel-title">Buzón de Entrega</h3>
            <p class="panel-sub">Arrastra o selecciona el archivo para simular la entrega</p>
          </div>

          <!-- SUCCESS STATE -->
          <div v-if="submissionSuccess && submissionReceipt" class="success-screen">
            <div class="success-icon-area">
              <svg viewBox="0 0 24 24" width="48" height="48" class="success-svg">
                <path fill="currentColor" d="M9 16.17L4.83 12l-1.42 1.41L9 19 21 7l-1.41-1.41z"/>
              </svg>
            </div>
            <h4 class="success-title">¡Tarea Recibida!</h4>
            <p class="success-message">La entrega se ha registrado en los servidores de Prisma Academy de forma exitosa.</p>
            
            <!-- Receipt Box (Flat Ticket design) -->
            <div class="receipt-box flat-border">
              <div class="receipt-row font-serif-receipt">
                <span class="receipt-label">Comprobante:</span>
                <span class="receipt-value">{{ submissionReceipt.id }}</span>
              </div>
              <div class="receipt-row">
                <span class="receipt-label">Asignatura:</span>
                <span class="receipt-value">{{ submissionReceipt.subject }}</span>
              </div>
              <div class="receipt-row">
                <span class="receipt-label">Tarea:</span>
                <span class="receipt-value">{{ submissionReceipt.taskName }}</span>
              </div>
              <div class="receipt-row">
                <span class="receipt-label">Archivo:</span>
                <span class="receipt-value file-value">{{ submissionReceipt.file }}</span>
              </div>
              <div class="receipt-row">
                <span class="receipt-label">Fecha y Hora:</span>
                <span class="receipt-value">{{ submissionReceipt.timestamp }}</span>
              </div>
              <div class="receipt-status-badge">
                <span class="badge-flat badge-green">ENTREGADO A TIEMPO</span>
              </div>
            </div>

            <button @click="resetForm" class="flat-button flat-button-dark w-100" style="margin-top: 1.5rem;">
              Hacer otra entrega
            </button>
          </div>

          <!-- FORM STATE -->
          <div v-else class="form-screen">
            <!-- Selector de Tarea -->
            <div class="input-group">
              <label for="task-select">Selecciona la Tarea:</label>
              <select 
                id="task-select" 
                v-model="selectedTaskId" 
                @change="updateSubjectFromTask"
                class="flat-select flat-border"
              >
                <option 
                  v-for="task in pendingTasks" 
                  :key="task.id" 
                  :value="task.id"
                  :disabled="task.status === 'submitted'"
                >
                  {{ task.status === 'submitted' ? '[Entregada] ' : '' }}{{ task.title }}
                </option>
                <option value="custom">Otra tarea / Entrega libre</option>
              </select>
            </div>

            <!-- Custom Subject & Title (if custom selected) -->
            <div v-if="selectedTaskId === 'custom'" class="custom-fields">
              <div class="input-group">
                <label for="custom-subject">Asignatura:</label>
                <select id="custom-subject" v-model="selectedSubject" class="flat-select flat-border">
                  <option v-for="subj in subjects" :key="subj" :value="subj">{{ subj }}</option>
                </select>
              </div>

              <div class="input-group">
                <label for="custom-title">Título de la Tarea:</label>
                <input 
                  type="text" 
                  id="custom-title" 
                  v-model="customTaskTitle" 
                  placeholder="Ej. Mi Ensayo Final" 
                  class="flat-input flat-border"
                />
              </div>
            </div>

            <!-- Subject Preview (Read only if linked task) -->
            <div v-else class="input-group">
              <label>Asignatura:</label>
              <div class="static-value-field">{{ selectedSubject }}</div>
            </div>

            <!-- File Dropzone -->
            <div class="input-group">
              <label>Archivo Adjunto:</label>
              <div 
                class="dropzone flat-border"
                :class="{ 'dragging': isDragging, 'has-file': fileName }"
                @dragover="handleDragOver"
                @dragleave="handleDragLeave"
                @drop="handleDrop"
                @click="triggerFileSelect"
              >
                <input 
                  type="file" 
                  id="file-input" 
                  @change="handleFileSelect" 
                  class="hidden-file-input"
                  accept=".pdf,.zip,.doc,.docx,.png,.jpg"
                />
                
                <div v-if="!fileName" class="dropzone-prompt">
                  <svg viewBox="0 0 24 24" width="32" height="32" class="upload-icon">
                    <path fill="currentColor" d="M19.35 10.04C18.67 6.59 15.64 4 12 4 9.11 4 6.6 5.64 5.35 8.04 2.34 8.36 0 10.91 0 14c0 3.31 2.69 6 6 6h13c2.76 0 5-2.24 5-5 0-2.64-2.05-4.78-4.65-4.96zM14 13v4h-4v-4H7l5-5 5 5h-3z"/>
                  </svg>
                  <p class="prompt-title">Arrastra tu archivo aquí</p>
                  <p class="prompt-sub">o haz clic para explorar en tu equipo</p>
                  <p class="prompt-hint">Formatos soportados: PDF, ZIP, DOCX, PNG (máx. 10MB)</p>
                </div>
                
                <div v-else class="dropzone-file" @click.stop>
                  <div class="file-icon">
                    <svg viewBox="0 0 24 24" width="28" height="28">
                      <path fill="currentColor" d="M6 2c-1.1 0-1.99.9-1.99 2L4 20c0 1.1.89 2 1.99 2H18c1.1 0 2-.9 2-2V8l-6-6H6zm7 7V3.5L18.5 9H13z"/>
                    </svg>
                  </div>
                  <div class="file-details">
                    <div class="file-name" :title="fileName">{{ fileName }}</div>
                    <div class="file-size">{{ fileSize }}</div>
                  </div>
                  <button @click="removeFile" class="remove-file-btn" aria-label="Remover archivo">
                    <svg viewBox="0 0 24 24" width="18" height="18">
                      <path fill="currentColor" d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"/>
                    </svg>
                  </button>
                </div>
              </div>
            </div>

            <!-- Submit Button -->
            <button 
              @click="submitTask" 
              class="flat-button flat-button-dark w-100" 
              :disabled="!fileName || isSubmitting"
              style="margin-top: 1rem;"
            >
              <span v-if="isSubmitting">Procesando Entrega...</span>
              <span v-else>Confirmar Entrega Académica</span>
            </button>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Submission Log (Session history) -->
    <div v-if="submittedTasks.length > 0" class="submissions-history-area flat-border">
      <h4 class="history-title">Registro de Envíos en esta sesión</h4>
      <div class="history-table-container">
        <table class="history-table">
          <thead>
            <tr>
              <th>ID</th>
              <th>Tarea</th>
              <th>Asignatura</th>
              <th>Archivo</th>
              <th>Fecha de Envío</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="st in submittedTasks" :key="st.id">
              <td class="st-id">{{ st.id }}</td>
              <td class="st-title">{{ st.title }}</td>
              <td>{{ st.subject }}</td>
              <td class="st-file">{{ st.file }}</td>
              <td>{{ st.timestamp }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<style scoped>
.helper-container {
  margin: 6rem 0;
}

.helper-header {
  margin-bottom: 3rem;
  max-width: 800px;
}

.helper-title {
  font-size: 2.5rem;
  margin-top: 0.5rem;
  margin-bottom: 1rem;
}

.helper-intro {
  font-size: 1.1rem;
  color: var(--color-text-muted);
}

.left-col {
  grid-column: span 7;
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.right-col {
  grid-column: span 5;
}

/* Panel Design */
.panel-section {
  background-color: var(--color-bg);
  padding: 2.5rem;
}

@media (max-width: 600px) {
  .panel-section {
    padding: 1.5rem;
  }
}

.panel-title {
  font-size: 1.6rem;
  font-weight: 500;
}

.panel-sub {
  font-size: 0.9rem;
  color: var(--color-text-muted);
  margin-bottom: 1.5rem;
}

/* Task List */
.task-list {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.task-item {
  border-bottom: 1px solid var(--color-border-subtle);
  padding-bottom: 1.25rem;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 1rem;
  transition: opacity var(--transition-fast);
}

.task-item:last-child {
  border-bottom: none;
  padding-bottom: 0;
}

.task-submitted-line {
  opacity: 0.6;
}

.task-info {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.task-badge {
  margin-bottom: 0.5rem;
}

.task-subject {
  font-size: 0.78rem;
  font-weight: 600;
  text-transform: uppercase;
  color: var(--color-text-muted);
  letter-spacing: 0.05em;
  margin-bottom: 0.2rem;
}

.task-item-title {
  font-family: var(--font-sans);
  font-size: 1.05rem;
  font-weight: 500;
  color: var(--color-text);
  margin-bottom: 0.3rem;
  line-height: 1.4;
}

.task-date {
  font-size: 0.82rem;
  color: var(--color-text-muted);
}

.select-task-btn {
  background-color: transparent;
  color: var(--color-text);
  border: 1px solid var(--color-border);
  padding: 0.4rem 1rem;
  font-family: var(--font-sans);
  font-size: 0.85rem;
  font-weight: 500;
  cursor: pointer;
  transition: all var(--transition-fast);
}

.select-task-btn:hover,
.select-task-btn.selected {
  background-color: var(--color-text);
  color: var(--color-bg);
}

/* Quick Links Grid */
.quick-links-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
}

@media (max-width: 600px) {
  .quick-links-grid {
    grid-template-columns: 1fr;
  }
}

.quick-link-card {
  padding: 1.5rem;
  display: block;
}

.card-title {
  font-size: 1.15rem;
  margin-bottom: 0.5rem;
}

.card-desc {
  font-size: 0.88rem;
  color: var(--color-text-muted);
}

/* Simulator Panel specific */
.simulator-panel {
  background-color: var(--color-bg-alt);
}

.simulator-header {
  border-bottom: 1px solid var(--color-border-subtle);
  margin-bottom: 1.5rem;
}

.simulator-indicator {
  display: inline-block;
  font-size: 0.7rem;
  font-weight: 600;
  letter-spacing: 0.1em;
  background-color: var(--color-text);
  color: var(--color-bg);
  padding: 0.2rem 0.6rem;
  margin-bottom: 0.8rem;
}

/* Input Fields */
.input-group {
  margin-bottom: 1.25rem;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.input-group label {
  font-size: 0.88rem;
  font-weight: 500;
  color: var(--color-text);
}

.flat-select, 
.flat-input {
  width: 100%;
  padding: 0.75rem;
  font-family: var(--font-sans);
  font-size: 0.92rem;
  background-color: var(--color-bg);
  color: var(--color-text);
  border-radius: 0;
  outline: none;
}

.flat-select:focus,
.flat-input:focus {
  border-color: var(--color-text);
}

.static-value-field {
  padding: 0.75rem;
  background-color: rgba(28, 27, 25, 0.05);
  font-size: 0.92rem;
  color: var(--color-text-muted);
}

.custom-fields {
  border-left: 2px solid var(--color-border);
  padding-left: 1rem;
  margin-bottom: 1rem;
}

/* Dropzone styling */
.dropzone {
  min-height: 180px;
  background-color: var(--color-bg);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  text-align: center;
  cursor: pointer;
  transition: all var(--transition-fast);
}

.dropzone.dragging {
  background-color: var(--color-bg-alt);
  border-style: dashed;
}

.dropzone.has-file {
  border-style: solid;
  border-color: var(--color-border);
  background-color: var(--color-bg);
}

.hidden-file-input {
  display: none;
}

.upload-icon {
  margin-bottom: 0.75rem;
  color: var(--color-text-muted);
}

.prompt-title {
  font-weight: 500;
  font-size: 0.95rem;
  color: var(--color-text);
  margin-bottom: 0.2rem;
}

.prompt-sub {
  font-size: 0.85rem;
  color: var(--color-text-muted);
  margin-bottom: 0.5rem;
}

.prompt-hint {
  font-size: 0.75rem;
  color: var(--color-text-muted);
}

/* Selected File UI inside dropzone */
.dropzone-file {
  display: flex;
  align-items: center;
  width: 100%;
  text-align: left;
  gap: 1rem;
  padding: 0.5rem;
}

.file-icon {
  background-color: var(--color-bg-alt);
  width: 50px;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid var(--color-border-subtle);
}

.file-details {
  flex-grow: 1;
  overflow: hidden;
}

.file-name {
  font-weight: 500;
  font-size: 0.92rem;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.file-size {
  font-size: 0.8rem;
  color: var(--color-text-muted);
}

.remove-file-btn {
  background: none;
  border: none;
  cursor: pointer;
  color: var(--color-text-muted);
  padding: 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: color var(--transition-fast);
}

.remove-file-btn:hover {
  color: var(--color-text);
}

/* Success screen design */
.success-screen {
  text-align: center;
  padding: 1rem 0;
}

.success-icon-area {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  background-color: var(--pastel-green-bg);
  color: var(--pastel-green-text);
  width: 72px;
  height: 72px;
  border-radius: 50%;
  margin-bottom: 1.5rem;
  border: 1px solid var(--color-border);
}

.success-title {
  font-size: 1.8rem;
  font-weight: 500;
  margin-bottom: 0.5rem;
}

.success-message {
  font-size: 0.95rem;
  color: var(--color-text-muted);
  margin-bottom: 2rem;
}

/* Receipt design */
.receipt-box {
  background-color: var(--color-bg);
  padding: 1.5rem;
  text-align: left;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.receipt-row {
  display: flex;
  justify-content: space-between;
  font-size: 0.88rem;
  border-bottom: 1px dotted var(--color-border-subtle);
  padding-bottom: 0.5rem;
}

.receipt-row:last-of-type {
  border-bottom: none;
}

.font-serif-receipt {
  font-family: var(--font-serif);
  font-weight: 600;
}

.receipt-label {
  color: var(--color-text-muted);
}

.receipt-value {
  color: var(--color-text);
  font-weight: 500;
  text-align: right;
}

.receipt-value.file-value {
  max-width: 180px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.receipt-status-badge {
  display: flex;
  justify-content: center;
  margin-top: 0.5rem;
}

/* History logs design */
.submissions-history-area {
  margin-top: 3rem;
  padding: 2rem;
  background-color: var(--color-bg);
}

.history-title {
  font-size: 1.25rem;
  font-weight: 500;
  margin-bottom: 1.25rem;
  font-family: var(--font-serif);
}

.history-table-container {
  overflow-x: auto;
}

.history-table {
  width: 100%;
  border-collapse: collapse;
  text-align: left;
  font-size: 0.88rem;
}

.history-table th, 
.history-table td {
  padding: 0.75rem 1rem;
  border-bottom: 1px solid var(--color-border-subtle);
}

.history-table th {
  font-weight: 600;
  color: var(--color-text);
  background-color: var(--color-bg-alt);
  text-transform: uppercase;
  font-size: 0.75rem;
  letter-spacing: 0.05em;
}

.st-id {
  font-family: var(--font-serif);
  font-weight: 500;
}

.st-title {
  font-weight: 500;
}

.st-file {
  font-family: monospace;
  color: var(--color-text-muted);
}

.w-100 {
  width: 100%;
}
</style>
