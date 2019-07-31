<template>
  <div class="info-page">
    <Nav tab="Info"></Nav>
  </div>
</template>
<script>
import Nav from './Nav'
export default {
  components: {
    Nav
  }
}
</script>
<style lang="scss" scoped>
.info-page {
  background:
    radial-gradient(
      ellipse at top left,
      rgba(255, 255, 255, 1) 40%,
      rgba(229, 229, 229, .9) 100%
    );
  height: 100vh;
  width: 100vw;
  padding: 1rem;
}

</style>
