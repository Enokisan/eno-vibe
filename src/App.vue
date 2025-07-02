<template>
  <div id="app">
    <LuckyDecision />
  </div>
</template>

<script>
import LuckyDecision from './components/LuckyDecision.vue'

export default {
  name: 'App',
  components: {
    LuckyDecision
  }
}
</script>

<style>
#app {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}
</style> 