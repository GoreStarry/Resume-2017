<template>
  <div class="Cover__container">
    <div class="wall__fill"></div>
    <svg-window @zoomIn="zoomInAnimation" @getShock="getShockAniamtion" :flyFSwtich="flyFSwtich"></svg-window>
    <div class="wall__fill"></div>
    <svg-man :shockSwitch="shockSwitch" @makeTitleFly="makeTitleFly" class="cover__man"></svg-man>
    <svg-computer class="cover__computer"></svg-computer>
    <div id="btn__skip" @click.once="zoomInAnimation">SKIP!</div>
  </div>
</template>

<script>
import ShipWindow from './svg/ShipWindow/ShipWindow';;
import Man from './svg/Man/Man';
import Computer from './svg/Computer/Computer.vue';

import { TweenMax } from 'gsap'
export default {
  name: 'Cover',
  components: {
    'svg-window': ShipWindow,
    'svg-man': Man,
    'svg-computer': Computer,
  },
  mounted() {

  },
  data() {
    return {
      shockSwitch: false,
      flyFSwtich: false,
    }
  },
  methods: {
    getShockAniamtion() {
      this.shockSwitch = true;
    },
    makeTitleFly() {
      this.flyFSwtich = true;
    },
    zoomInAnimation() {
      var cover = this.$el;
      TweenMax.to(cover, 1.5, {
        scale: 7,
        force3D: false,
        ease: Power1.easeOut,
        onComplete: () => {
          this.$emit('unmountCover');
        }
      })
    }
  }
}
</script>

<style lang="scss">
@import './styles/Cover.scss';

.Cover__container {
  display: flex;
  justify-content: row;
  position: fixed;
  left: 0;
  top: 0;
  width: 100vw;
  height: 100vh;
  transform-origin: center; // animation: cover_zoom 2s linear infinite;
}

@keyframes cover_zoom {
  form {
    transform: scale(1);
  }
  to {
    transform: scale(8);
  }
}

.wall__fill {
  flex: 1;
  background-color: brown;
}

.cover__man {
  position: absolute;
  bottom: -30px;
  left: 0;
  height: 90vh;
}

.cover__computer {
  position: absolute;
  bottom: 0;
  right: 0;
  height: 80vh;
}

#btn__skip {
  cursor: pointer;
  opacity: 0.6;
  border-radius: 1rem;
  border: 3px solid white;
  background-color: rgba(255, 255, 255, 0.3);
  padding: 0.5rem 1rem;
  color: white;
  position: fixed;
  right: 1.5rem;
  bottom: 2rem;
  font-size: 1.5rem;
}
</style>
