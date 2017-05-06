<template lang="pug">
.box__Works(v-show="planetOpen")
  h1 Works
</template>

<script>
export default {
  name: 'Works',
  props: {
    planetOpen: Boolean,
    planetColor: String,
  },
  watch: {
    planetOpen(newValue, oldValue) {
      if (newValue !== oldValue) {
        if (newValue) {// open
          this.animationIn();
        }
        else {
          this.animationOut();
        }
      }
    }
  },
  methods: {
    animationIn() {
      console.log('in');
    },
    animationOut() {
      console.log('out');
    }
  },
}
</script>

<style lang="scss">

</style>
