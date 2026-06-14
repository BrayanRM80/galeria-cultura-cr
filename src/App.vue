<template>
  <div id="app">
    <header class="site-header">
      <h1>🇨🇷 Cultura Típica de Costa Rica</h1>
      <p>Galería fotográfica interactiva</p>
    </header>

    <main>
      <p>Proyecto en construcción — Vue 3 funcionando correctamente.</p>
    </main>

    <footer class="site-footer">
      <p>IF7102 Multimedios · UCR Sede Guanacaste · 2026</p>
    </footer>
  </div>
</template>

<script setup>
// Commit 1: Setup inicial — componentes se añaden en commits siguientes
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

main {
  padding: 2rem;
  text-align: center;
}

.site-footer {
  text-align: center;
  padding: 1rem;
  background-color: #111;
  color: #888;
  font-size: 0.85rem;
}
</style>
