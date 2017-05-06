<template lang="pug">
.box__Skills(v-show="planetOpen")
  header
    h1 Skills
  //- .container__bg(:style="{backgroundColor: planetColor}")
  .container__bg
    section
      h3 Front-End：
      p 【 Good 】： ES6、React、GSAP(GreenSock)、A-frame(WebVR)、Sass(SCSS)
      p 【 Competent 】： Vue.js、AngularJS(1)、Rx.js、Pug、
      p 【 Tool 】： Webpack、Gulp、Git、Mercurial(hg)、npm...
      h3 Back-End：
      p 【 Competent 】： MongoDB(Mongoose)、Express、
      p 【 Limited 】：php、MySQL、
      h3 Design：
      p 【 Graphic 】：Adobe Photoshop、Illustrator、
      p 【 3D 】：PTC Creo Elements/Direct、Pro/E、
</template>

<script>
export default {
  name: 'Skills',
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
      var h3 = this.$el.querySelectorAll('h3');
      TweenMax.staggerFrom(h3, 0.8, {
        opacity: 0,
        x: -50,
      }, 0.2)
      var p = this.$el.querySelectorAll('p');
      TweenMax.staggerFrom(p, 0.5, {
        delay: 1,
        opacity: 0,
        x: -50,
      }, 0.2)
    },
    animationOut() {
      console.log('out');
    }
  },
}
</script>

<style lang="scss">
$planet_margin_top: 3vh;

.box__Skills {
  margin-top: $planet_margin_top;
  display: flex;
  flex-direction: column; // align-items: center;
  justify-content: center;
  h3 {
    font-size: 2rem;
    margin-bottom: 0.5rem;
    font-weight: bold;
    &:not(:first-of-type) {
      margin-top: 1.5rem;
    }
  }
  p {
    font-size: 1.2rem;
    line-height: 2rem;
    text-indent: 1.5rem;
  }
}

header {
  min-height: #{ 50vh - $planet_margin_top };
  text-align: center;
  font-size: 10rem;
}

.container__bg {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

section {
  padding: 0 1.5rem;
  transform: translateY(-5rem);
}
</style>
