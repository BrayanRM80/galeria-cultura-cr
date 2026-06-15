<template>
  <div class="foto-card" @click="$emit('seleccionar', foto)">
    <div class="foto-imagen-wrapper">
      <img
        :src="foto.imagen"
        :alt="foto.titulo"
        class="foto-imagen"
        :style="estiloFiltro"
        loading="lazy"
      />
      <div class="foto-overlay"><span>🔍 Ver</span></div>
    </div>
    <div class="foto-info">
      <h3>{{ foto.titulo }}</h3>
      <span class="foto-categoria">{{ categoriaLabel }}</span>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  foto: { type: Object, required: true },
  filtroVisual: { type: String, default: 'ninguno' },
})

defineEmits(['seleccionar'])

const categoriasLabels = {
  comidas: '🍽️ Comidas Típicas',
  trajes: '👗 Trajes Típicos',
  artesanias: '🏺 Artesanías',
  fiestas: '🎉 Fiestas y Tradiciones',
}

const categoriaLabel = categoriasLabels[props.foto.categoria] ?? props.foto.categoria

const estiloFiltro = computed(() => {
  const filtros = {
    ninguno: 'none',
    grayscale: 'grayscale(100%)',
    sepia: 'sepia(100%)',
    contraste: 'contrast(180%)',
    saturacion: 'saturate(300%)',
  }
  return { filter: filtros[props.filtroVisual] ?? 'none' }
})
</script>

<style scoped>
.foto-card {
  background: #16213e;
  border-radius: 10px;
  overflow: hidden;
  cursor: pointer;
  transition:
    transform 0.2s ease,
    box-shadow 0.2s ease;
}

.foto-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 24px rgba(192, 57, 43, 0.4);
}

.foto-imagen-wrapper {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
}

.foto-imagen {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition:
    transform 0.3s ease,
    filter 0.3s ease;
}

.foto-card:hover .foto-imagen {
  transform: scale(1.05);
}

.foto-overlay {
  position: absolute;
  inset: 0;
  background: rgba(192, 57, 43, 0.6);
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.2s ease;
  font-size: 1.2rem;
  color: white;
  font-weight: bold;
}

.foto-card:hover .foto-overlay {
  opacity: 1;
}

.foto-info {
  padding: 0.75rem 1rem;
}
.foto-info h3 {
  font-size: 0.95rem;
  color: #eee;
  margin-bottom: 0.3rem;
}
.foto-categoria {
  font-size: 0.75rem;
  color: #c0392b;
  font-weight: 600;
}
</style>
