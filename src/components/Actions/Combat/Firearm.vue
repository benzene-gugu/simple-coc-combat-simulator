<template>
  <div id="app">
    <button id="firearm" @click="firearm" :disabled="isDisabled">Firearm</button>
  </div>
</template>

<script>
export default {
  name: "Firearm",
  props: {
    isDisabled: Boolean
  },
  methods: {
    firearm() {
      this.$emit("firearm")
    }
  }
}
</script>

<style scoped>
#firearm {
  background-color: #ffaf4f;
}

#firearm:hover {
  background-color: #ff9a2b;
}
</style>