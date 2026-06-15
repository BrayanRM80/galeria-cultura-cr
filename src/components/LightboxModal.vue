<template>
  <div class="lightbox-overlay" @click.self="$emit('cerrar')">
    <div class="lightbox-contenido">
      <button class="btn-cerrar" @click="$emit('cerrar')">✕</button>

      <img :src="foto.imagen" :alt="foto.titulo" class="lightbox-img" />

      <div class="lightbox-info">
        <h2>{{ foto.titulo }}</h2>
        <p>{{ foto.descripcion }}</p>
        <span class="lightbox-licencia">📷 {{ foto.fuente }} — {{ foto.licencia }}</span>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  foto: { type: Object, required: true },
})
defineEmits(['cerrar'])
</script>

<style scoped>
.lightbox-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.88);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 1rem;
}

.lightbox-contenido {
  background: #16213e;
  border-radius: 12px;
  max-width: 700px;
  width: 100%;
  overflow: hidden;
  position: relative;
  animation: aparecer 0.2s ease;
}

@keyframes aparecer {
  from {
    opacity: 0;
    transform: scale(0.95);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

.btn-cerrar {
  position: absolute;
  top: 0.75rem;
  right: 0.75rem;
  background: rgba(0, 0, 0, 0.6);
  color: white;
  border: none;
  border-radius: 50%;
  width: 2rem;
  height: 2rem;
  font-size: 1rem;
  cursor: pointer;
  z-index: 10;
}

.lightbox-img {
  width: 100%;
  max-height: 420px;
  object-fit: cover;
}

.lightbox-info {
  padding: 1.25rem;
}

.lightbox-info h2 {
  font-size: 1.3rem;
  margin-bottom: 0.5rem;
  color: white;
}

.lightbox-info p {
  color: #ccc;
  font-size: 0.95rem;
  line-height: 1.5;
  margin-bottom: 0.75rem;
}

.lightbox-licencia {
  font-size: 0.75rem;
  color: #888;
}
</style>
