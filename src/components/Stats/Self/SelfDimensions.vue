<template>
  <div id="app">
    <table class="table table-bordered dimensions">
      <tr>
        <th>STR:</th>
        <td>{{dimensions.strength}}</td>
        <th>DEX:</th>
        <td>{{dimensions.dexterity}}</td>
        <th>POW:</th>
        <td>{{dimensions.power}}</td>
      </tr>
      <tr>
        <th>CON:</th>
        <td>{{dimensions.condition}}</td>
        <th>APP:</th>
        <td>{{dimensions.appearance}}</td>
        <th>EDU:</th>
        <td>{{dimensions.education}}</td>
      </tr>
      <tr>
        <th>SIZ:</th>
        <td>{{dimensions.size}}</td>
        <th>INT:</th>
        <td>{{dimensions.intelligence}}</td>
        <th>LUC:</th>
        <td>{{dimensions.luck}}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: "SelfDimensions",
  props: {
    'dimensions':Object,
  }
}
</script>

<style scoped>
.dimensions{
  min-width:max-content;
  text-align: center;
}
</style>