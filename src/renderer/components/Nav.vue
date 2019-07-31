<template>
  <div class="d-nav">
    <nav class="e-tabs centered">
      <ul>
        <li :class="{active: tab === 'Time'}" @click="goTo('Time')"><a href="#">Time</a></li>
        <li :class="{active: tab === 'Info'}" @click="goTo('Info')"><a href="#">Hot</a></li>
      </ul>
    </nav>
  </div>
</template>
<script>
export default {
  props: {
    tab: {
      type: String
    }
  },
  methods: {
    goTo (tab) {
      const route = {
        Time: '/',
        Info: '/infopage'
      }
      if (tab !== this.tab) {
        this.$router.push(route[tab])
      }
    }
  }
}
</script>