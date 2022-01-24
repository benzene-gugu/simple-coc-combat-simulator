<template>
  <div id="app">
    <button id="firstAid" @click="firstAid" :disabled="isDisabled">FirstAid</button>
  </div>
</template>

<script>
export default {
  name: "FirstAid",
  props: {
    isDisabled: Boolean
  },
  methods: {
    firstAid() {
      this.$emit("heal")
    }
  }
}
</script>

<style scoped>
#firstAid {
  background-color: #aaffb0;
}

#firstAid:hover {
  background-color: #76ff7e;
}
</style>