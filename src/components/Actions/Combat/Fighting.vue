<template>
  <div id="app">
    <button id="fighting" @click="fight" :disabled="isDisabled">Fighting</button>
  </div>
</template>

<script>
export default {
  name: "Fighting",
  props: {
    isDisabled: Boolean
  },
  methods: {
    fight() {
      this.$emit("fight")
    }
  }
}
</script>

<style scoped>
#fighting {
  background-color: #ff7367;
}

#fighting:hover {
  background-color: #ff3f43;
}
</style>