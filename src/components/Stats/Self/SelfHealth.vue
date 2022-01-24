<template>
  <div id="app">
    <div class="health-bar">
      <div id="player" class="health-bar text-center" :style="{width: health.maxHealth===0?(100+'%'):((health.maxHealth-health.lostHealth)*100/health.maxHealth+'%')}">
        {{health.maxHealth-health.lostHealth}}/{{health.maxHealth}}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SelfHealth",
  props:{
    health:Object,
  }
}
</script>

<style scoped>
.health-bar {
  width: 80%;
  height: 40px;
  background-color: gray;
  margin: auto;
  transition: width 500ms;
}
#player {
  background-color: green;
  margin: 0;
  color: lightgray;
}
</style>