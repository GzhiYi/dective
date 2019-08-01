<template>
  <div id="app">
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

<style>
  /* CSS */
</style>
