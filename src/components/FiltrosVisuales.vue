<template>
  <div class="filtros-visuales">
    <h3>🎨 Filtros Visuales</h3>
    <div class="btns-filtro-visual">
      <button
        v-for="filtro in filtros"
        :key="filtro.valor"
        :class="['btn-visual', { activo: filtroActivo === filtro.valor }]"
        @click="$emit('cambiar', filtro.valor)"
      >
        {{ filtro.label }}
      </button>
    </div>
  </div>
</template>

<script setup>
defineProps({
  filtroActivo: { type: String, default: 'ninguno' },
})
defineEmits(['cambiar'])

const filtros = [
  { valor: 'ninguno', label: '🖼️ Original' },
  { valor: 'grayscale', label: '⚫ Escala de grises' },
  { valor: 'sepia', label: '🟤 Sepia' },
  { valor: 'contraste', label: '⚡ Contraste' },
  { valor: 'saturacion', label: '🌈 Saturación' },
]
</script>

<style scoped>
.filtros-visuales {
  text-align: center;
  padding: 0.5rem 1rem 1.25rem;
}

.filtros-visuales h3 {
  font-size: 0.95rem;
  color: #aaa;
  margin-bottom: 0.75rem;
}

.btns-filtro-visual {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  justify-content: center;
}

.btn-visual {
  background: #0f3460;
  color: #eee;
  border: 2px solid transparent;
  padding: 0.4rem 0.9rem;
  border-radius: 999px;
  cursor: pointer;
  font-size: 0.85rem;
  transition: all 0.2s ease;
}

.btn-visual:hover {
  border-color: #c0392b;
}

.btn-visual.activo {
  background: #c0392b;
  color: white;
  font-weight: bold;
}
</style>
