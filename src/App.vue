<template>
  <div id="app">
      <router-link to="/">Home</router-link> |
      <router-link to='/products'>1234</router-link>
      <loading :active.sync="isLoading"/>
    <router-view @loading='loading'/>
  </div>
</template>

<script>
export default {
  data () {
    return {
      isLoading: false,
      fullPage: true
    }
  },
  methods: {
    loading (status) {
      this.isLoading = status
    }
  },
  created () {
  }
}
</script>

<style lang="scss">
  @import './assets/scss/all.scss';
</style>
