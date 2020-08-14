<template>
  <div class="home">
    <layout-header></layout-header>
    <layout-footer></layout-footer>
    <router-view @loading='loading'/>
  </div>
</template>

<script>

import LayoutHeader from '../components/LayoutHeader'
import LayoutFooter from '../components/LayoutFooter'

export default {
  name: 'Home',
  components: {
    LayoutFooter,
    LayoutHeader
  },
  methods: {
    loading (status) {
      this.$emit('loading', status)
    }
  },
  created () {
  }
}
</script>
