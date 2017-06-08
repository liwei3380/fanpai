<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'app'
}
</script>

<style>
body,html{
  padding: 0;
  margin: 0;
}
#app {
  font-size: 14px;
  font-family: Helvetica;
  width: 100%;
  height: 100%;
}
</style>
