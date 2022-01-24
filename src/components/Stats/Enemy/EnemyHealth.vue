<template>
  <div id="app">
    <div class="health-bar">
      <div id="enemy" class="health-bar text-center" :style="{width: health.maxHealth===0?(100+'%'):((health.maxHealth-health.lostHealth)*100/health.maxHealth+'%')}">
        {{health.maxHealth-health.lostHealth}}/{{health.maxHealth}}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "EnemyHealth",
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
#enemy {
  background-color: red;
  margin: 0;
  color: lightgray;
}
</style>