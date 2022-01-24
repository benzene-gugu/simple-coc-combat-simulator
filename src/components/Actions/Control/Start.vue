<template>
  <div id="app">
    <button @click="startGame">Start Game</button>
  </div>
</template>

<script>
export default {
  name: "Start",
  methods: {
    startGame() {
      this.$emit('start')
    }
  }
}
</script>

<style scoped>

</style>