<template>
  <div class="audio-player">
    <span class="audio-label"> Ambiente sonoro</span>

    <button class="btn-play" @click="togglePlay">
      {{ reproduciendo ? '⏸ Pausar' : '▶ Reproducir' }}
    </button>

    <div class="control-volumen">
      <span>🔈</span>
      <input type="range" min="0" max="1" step="0.05" :value="volumen" @input="cambiarVolumen" />
      <span>🔊</span>
    </div>

    <select class="selector-pista" @change="cambiarPista" :value="pistaActiva">
      <option v-for="pista in pistas" :key="pista.archivo" :value="pista.archivo">
        {{ pista.label }}
      </option>
    </select>

    <audio ref="audioRef" :src="pistaActiva" loop @ended="reproduciendo = false"></audio>
  </div>
</template>

<script setup>
import { ref, watch, onUnmounted } from 'vue'

const audioRef = ref(null)
const reproduciendo = ref(false)
const volumen = ref(0.5)
const pistaActiva = ref('/audio/ambiente-cr.wav')

const pistas = [
  { archivo: '/audio/ambiente-cr.wav', label: ' Naturaleza tropical' },
  { archivo: '/audio/marimba.wav', label: ' Marimba costarricense' },
]

function togglePlay() {
  const audio = audioRef.value
  if (!audio) return
  if (reproduciendo.value) {
    audio.pause()
  } else {
    audio.play()
  }
  reproduciendo.value = !reproduciendo.value
}

function cambiarVolumen(e) {
  volumen.value = parseFloat(e.target.value)
  if (audioRef.value) audioRef.value.volume = volumen.value
}

function cambiarPista(e) {
  pistaActiva.value = e.target.value

  setTimeout(() => {
    const audio = audioRef.value
    if (audio) {
      audio.load()
      if (reproduciendo.value) {
        audio.play().catch((err) => console.log('Error al reproducir:', err))
      }
    }
  }, 50)
}

watch(audioRef, (audio) => {
  if (audio) audio.volume = volumen.value
})

onUnmounted(() => {
  if (audioRef.value) audioRef.value.pause()
})
</script>

<style scoped>
.audio-player {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 0.75rem;
  justify-content: center;
  background: #0f3460;
  padding: 0.75rem 1.5rem;
  border-bottom: 2px solid #c0392b;
}

.audio-label {
  font-size: 0.9rem;
  color: #aaa;
}

.btn-play {
  background: #c0392b;
  color: white;
  border: none;
  padding: 0.4rem 1rem;
  border-radius: 999px;
  cursor: pointer;
  font-size: 0.85rem;
  transition: background 0.2s;
}

.btn-play:hover {
  background: #a93226;
}

.control-volumen {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  font-size: 0.85rem;
}

.control-volumen input[type='range'] {
  width: 90px;
  accent-color: #c0392b;
  cursor: pointer;
}

.selector-pista {
  background: #16213e;
  color: #eee;
  border: 1px solid #c0392b;
  padding: 0.3rem 0.6rem;
  border-radius: 6px;
  font-size: 0.82rem;
  cursor: pointer;
}
</style>
