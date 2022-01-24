<template>
  <div id="app">
    <table class="table table-bordered skills">
      <tr>
        <th>Fighting:{{fightingName}}</th>
        <th>Firearm:{{firearmName}}</th>
        <th>First Aid</th>
        <th>Dodge</th>
      </tr>
      <tr>
        <td>{{skills.fighting}}</td>
        <td>{{skills.firearm}}</td>
        <td>{{skills.firstAid}}</td>
        <td>{{skills.dodge}}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: "EnemyAbility",
  props: {
    skills:Object,
    fightingName:String,
    firearmName:String,
  }
}
</script>

<style scoped>
.skills{
  min-width:max-content;
  text-align: center;
}
</style>