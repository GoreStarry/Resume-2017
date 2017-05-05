<template>
  <div>
    <index-component></index-component>
  </div>
</template>

<script>
import Index from './container/index/Index.vue';
export default {
  name: 'Resume',
  components: {
    'index-component': Index,
  }
}
</script>

<style lang="scss">
@import './styles/global.scss'
</style>
