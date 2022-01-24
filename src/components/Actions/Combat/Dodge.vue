<template>
  <div id="app">
    <button id="dodge" @click="dodge" :disabled="isDisabled">Dodge</button>
  </div>
</template>

<script>
export default {
  name: "Dodge",
  props: {
    isDisabled: Boolean
  },
  methods: {
    dodge() {
      this.$emit("dodge")
    }
  }
}
</script>

<style scoped>
#dodge {
  background-color: #ffffff;
}

#dodge:hover {
  background-color: #c7c7c7;
}
</style>