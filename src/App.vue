<template>
  <div id="app">
    <header class="site-header">
      <h1>🇨🇷 Cultura Típica de Costa Rica</h1>
      <p>Galería fotográfica interactiva</p>
    </header>

    <main>
      <!-- Filtros de categoría -->
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

      <!-- Filtros visuales CSS -->
      <FiltrosVisuales :filtroActivo="filtroVisual" @cambiar="filtroVisual = $event" />

      <!-- Grilla de fotos -->
      <div v-if="cargando" class="estado-carga"><p>Cargando galería...</p></div>

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

    <!-- Lightbox -->
    <LightboxModal
      v-if="fotoSeleccionada"
      :foto="fotoSeleccionada"
      @cerrar="fotoSeleccionada = null"
    />

    <footer class="site-footer">
      <p>IF7102 Multimedios · UCR Sede Guanacaste · 2026</p>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import FotoCard from './components/FotoCard.vue'
import LightboxModal from './components/LightboxModal.vue'
import FiltrosVisuales from './components/FiltrosVisuales.vue'

const fotos = ref([])
const cargando = ref(true)
const categoriaActiva = ref('todas')
const filtroVisual = ref('ninguno')
const fotoSeleccionada = ref(null)

const categorias = [
  { valor: 'todas', label: '🌟 Todas' },
  { valor: 'comidas', label: '🍽️ Comidas Típicas' },
  { valor: 'trajes', label: '👗 Trajes Típicos' },
  { valor: 'artesanias', label: '🏺 Artesanías' },
  { valor: 'fiestas', label: '🎉 Fiestas y Tradiciones' },
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
  font-family: 'Segoe UI', sans-serif;
  background-color: #1a1a2e;
  color: #eee;
}

.site-header {
  background-color: #c0392b;
  text-align: center;
  padding: 2rem;
}

.site-header h1 {
  font-size: 2rem;
  color: white;
}
.site-header p {
  color: #f5c6c6;
  margin-top: 0.5rem;
}

.filtros {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
  justify-content: center;
  padding: 1.5rem 1rem 0.75rem;
}

.btn-filtro {
  background: #16213e;
  color: #eee;
  border: 2px solid #c0392b;
  padding: 0.5rem 1.1rem;
  border-radius: 999px;
  cursor: pointer;
  font-size: 0.9rem;
  transition: all 0.2s ease;
}

.btn-filtro:hover {
  background: #c0392b;
  color: white;
}
.btn-filtro.activo {
  background: #c0392b;
  color: white;
  font-weight: bold;
}

.galeria-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
  gap: 1.25rem;
  padding: 0 1.5rem 2rem;
}

.estado-carga,
.sin-resultados {
  text-align: center;
  padding: 3rem;
  color: #aaa;
}

.site-footer {
  text-align: center;
  padding: 1rem;
  background-color: #111;
  color: #888;
  font-size: 0.85rem;
}

@media (max-width: 600px) {
  .site-header h1 {
    font-size: 1.4rem;
  }
  .galeria-grid {
    grid-template-columns: 1fr 1fr;
    padding: 0 0.75rem 2rem;
    gap: 0.75rem;
  }
}
</style>
