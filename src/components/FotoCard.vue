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
  background: #161b22;
  border: 1px solid #21262d;
  border-radius: 12px;
  overflow: hidden;
  cursor: pointer;
  transition:
    transform 0.2s ease,
    box-shadow 0.2s ease,
    border-color 0.2s ease;
}

.foto-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 12px 32px rgba(0, 0, 0, 0.5);
  border-color: #c0392b;
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
    transform 0.35s ease,
    filter 0.3s ease;
}

.foto-card:hover .foto-imagen {
  transform: scale(1.07);
}

.foto-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(192, 57, 43, 0.85) 0%, transparent 60%);
  display: flex;
  align-items: flex-end;
  justify-content: flex-end;
  padding: 0.75rem;
  opacity: 0;
  transition: opacity 0.25s ease;
}

.foto-overlay span {
  background: white;
  color: #c0392b;
  border-radius: 999px;
  padding: 0.3rem 0.8rem;
  font-size: 0.8rem;
  font-weight: 700;
}

.foto-card:hover .foto-overlay {
  opacity: 1;
}

.foto-info {
  padding: 0.85rem 1rem;
  border-top: 1px solid #21262d;
}

.foto-info h3 {
  font-size: 0.92rem;
  color: #e6edf3;
  margin-bottom: 0.35rem;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.foto-categoria {
  font-size: 0.72rem;
  color: #c0392b;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}
</style>
