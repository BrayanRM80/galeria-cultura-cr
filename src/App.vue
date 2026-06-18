<template>
  <div id="app">
    <header class="site-header">
      <div class="header-content">
        <div class="header-flag">🇨🇷</div>
        <div>
          <h1>Cultura Típica de Costa Rica</h1>
          <p>Galería fotográfica interactiva</p>
        </div>
      </div>
    </header>

    <AudioPlayer />

    <main>
      <section class="controles">
        <!-- Filtros de categoría -->
        <div class="filtros-wrapper">
          <p class="filtros-titulo">Categoría</p>
          <div class="filtros">
            <button
              v-for="cat in categorias"
              :key="cat.valor"
              :class="['btn-filtro', { activo: categoriaActiva === cat.valor }]"
              @click="categoriaActiva = cat.valor"
            >
              {{ cat.label }}
            </button>
          </div>
        </div>

        <FiltrosVisuales :filtroActivo="filtroVisual" @cambiar="filtroVisual = $event" />
      </section>

      <div class="contador">
        <span>{{ fotosFiltradas.length }} foto{{ fotosFiltradas.length !== 1 ? 's' : '' }}</span>
      </div>

      <div v-if="cargando" class="estado-carga">
        <div class="spinner"></div>
        <p>Cargando galería...</p>
      </div>

      <div v-else class="galeria-grid">
        <FotoCard
          v-for="foto in fotosFiltradas"
          :key="foto.id"
          :foto="foto"
          :filtroVisual="filtroVisual"
          @seleccionar="abrirLightbox"
        />
      </div>

      <div v-if="!cargando && fotosFiltradas.length === 0" class="sin-resultados">
        <p>No hay fotos en esta categoría.</p>
      </div>
    </main>

    <LightboxModal
      v-if="fotoSeleccionada"
      :foto="fotoSeleccionada"
      @cerrar="fotoSeleccionada = null"
    />

    <footer class="site-footer">
      <p>🇨🇷 IF7102 Multimedios · UCR Sede Guanacaste · 2026</p>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import FotoCard from './components/FotoCard.vue'
import LightboxModal from './components/LightboxModal.vue'
import FiltrosVisuales from './components/FiltrosVisuales.vue'
import AudioPlayer from './components/AudioPlayer.vue'

const fotos = ref([])
const cargando = ref(true)
const categoriaActiva = ref('todas')
const filtroVisual = ref('ninguno')
const fotoSeleccionada = ref(null)

const categorias = [
  { valor: 'todas', label: ' Todas' },
  { valor: 'comidas', label: ' Comidas' },
  { valor: 'trajes', label: ' Trajes' },
  { valor: 'artesanias', label: ' Artesanías' },
  { valor: 'fiestas', label: ' Fiestas' },
  { valor: 'lugares', label: ' Lugares' },
]

const fotosFiltradas = computed(() => {
  if (categoriaActiva.value === 'todas') return fotos.value
  return fotos.value.filter((f) => f.categoria === categoriaActiva.value)
})

onMounted(async () => {
  try {
    const respuesta = await fetch('/fotos.json')
    fotos.value = await respuesta.json()
  } catch (error) {
    console.error('Error cargando fotos.json:', error)
  } finally {
    cargando.value = false
  }
})

function abrirLightbox(foto) {
  fotoSeleccionada.value = foto
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, sans-serif;
  background-color: #0d1117;
  color: #eee;
  min-height: 100vh;
}

/* HEADER */
.site-header {
  background: linear-gradient(135deg, #c0392b 0%, #922b21 60%, #7b241c 100%);
  padding: 2rem 1.5rem;
  text-align: center;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.4);
}

.header-content {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
}

.header-flag {
  font-size: 3rem;
  filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.3));
}

.site-header h1 {
  font-size: 2rem;
  color: white;
  font-weight: 700;
  letter-spacing: 0.5px;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

.site-header p {
  color: rgba(255, 255, 255, 0.75);
  margin-top: 0.25rem;
  font-size: 0.95rem;
  letter-spacing: 1px;
  text-transform: uppercase;
}

/* CONTROLES */
.controles {
  background: #161b22;
  border-bottom: 1px solid #30363d;
  padding: 1.25rem 1rem 0.5rem;
}

.filtros-wrapper {
  margin-bottom: 0.5rem;
}

.filtros-titulo {
  text-align: center;
  font-size: 0.78rem;
  color: #6e7681;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 0.6rem;
}

.filtros {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  justify-content: center;
}

.btn-filtro {
  background: transparent;
  color: #8b949e;
  border: 1.5px solid #30363d;
  padding: 0.45rem 1rem;
  border-radius: 999px;
  cursor: pointer;
  font-size: 0.85rem;
  transition: all 0.2s ease;
}

.btn-filtro:hover {
  border-color: #c0392b;
  color: #eee;
  background: rgba(192, 57, 43, 0.1);
}

.btn-filtro.activo {
  background: #c0392b;
  border-color: #c0392b;
  color: white;
  font-weight: 600;
  box-shadow: 0 0 12px rgba(192, 57, 43, 0.4);
}

.contador {
  text-align: right;
  padding: 0.6rem 1.75rem 0;
  font-size: 0.8rem;
  color: #6e7681;
}

.galeria-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
  gap: 1.25rem;
  padding: 1rem 1.5rem 3rem;
}

.estado-carga {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  padding: 4rem;
  color: #6e7681;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 3px solid #30363d;
  border-top-color: #c0392b;
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.sin-resultados {
  text-align: center;
  padding: 3rem;
  color: #6e7681;
  font-size: 1.1rem;
}

.site-footer {
  text-align: center;
  padding: 1.25rem;
  background: #010409;
  color: #6e7681;
  font-size: 0.82rem;
  border-top: 1px solid #21262d;
  letter-spacing: 0.5px;
}

@media (max-width: 600px) {
  .site-header h1 {
    font-size: 1.3rem;
  }
  .header-flag {
    font-size: 2rem;
  }
  .galeria-grid {
    grid-template-columns: 1fr 1fr;
    padding: 0.75rem 0.75rem 2rem;
    gap: 0.75rem;
  }
  .contador {
    padding: 0.5rem 1rem 0;
  }
}
</style>
