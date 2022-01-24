<template>
  <div id="app">
    <ul>
      <li :key="action" v-for="action in log" :class="{'player-action':action.isPlayer,'enemy-action':!action.isPlayer}">
        {{action.msg}}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Log",
  props: {
    log:Array,
  }
}
</script>

<style scoped>
ul {
  list-style: none;
  font-weight: bold;
}
ul li {
  margin: 5px;
}
.player-action {
  color: blue;
  background-color: #e4e8ff;
}
.enemy-action {
  color: red;
  background-color: #ffc0c1;
}
</style>