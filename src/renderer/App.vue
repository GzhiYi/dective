<template>
  <div id="app">
    <div class="drag" style="-webkit-app-region: drag"></div>
    <keep-alive :include="['landing-page']">
      <router-view></router-view>
    </keep-alive>
  </div>
</template>

<script>
  export default {
    name: 'dective'
  }
</script>

<style lang="scss" scoped>
.drag {
  height: 25px;
  width: 100%;
  position: fixed;
}
</style>
