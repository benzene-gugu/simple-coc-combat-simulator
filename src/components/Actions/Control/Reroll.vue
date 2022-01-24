<template>
  <div id="app">
    <button @click="rollAttributes">Roll Attributes</button>
  </div>
</template>

<script>
export default {
  name: "Reroll",
  methods: {
    rollAttributes() {
      this.$emit('roll');
    }
  }
}
</script>

<style scoped>

</style>