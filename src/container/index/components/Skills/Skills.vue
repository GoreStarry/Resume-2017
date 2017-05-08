<template lang="pug">
transition( :duration="{ enter: 0, leave: 500 }")
  .box__Skills(v-show="planetOpen")
    header
      //- h1 Skills
    //- .container__bg(:style="{backgroundColor: planetColor}")
    .container__bg
      section
        h3 Front-End：
        p 【 Good 】： 
          span ES6、React、GSAP(GreenSock)、A-frame(WebVR)、Sass(SCSS)
        p 【 Competent 】： 
          span Vue.js、AngularJS(1)、Rx.js、Pug、Nightwatch、Chai、Sinon...
        p 【 Tool 】： 
          span Webpack、Gulp、Git、Mercurial(hg)、npm...
        h3 Back-End：
        p 【 Competent 】： 
          span MongoDB(Mongoose)、Express、
        p 【 Limited 】：
          span php、MySQL、
        h3 Design：
        p 【 Graphic 】：
          span Adobe Photoshop、Illustrator (Limited)、
        p 【 3D 】：
          span PTC Creo Elements/Direct、Pro/E、
</template>

<script>
import { TweenMax, TimelineMax } from 'gsap';
let tl__detail;
export default {
  name: 'Skills',
  props: {
    planetOpen: Boolean,
    planetColor: String,
  },
  watch: {
    planetOpen(newValue, oldValue) {
      if (newValue !== oldValue) {
        if (!newValue) {
          this.animationOut();
        }
      }
    }
  },
  updated() {
    if (this.planetOpen) {
      this.extendBoxHeight();
      this.animationIn();
    }
  },
  beforeDestroy() {
    tl__detail.kill();
  },
  methods: {
    extendBoxHeight() {
      var boxHeight = this.$el.offsetHeight;
      if (boxHeight > window.innerHeight) {
        this.$emit('updatePlanetBoxHeight', boxHeight + 'px')
      }
    },
    animationIn() {
      var h3 = this.$el.querySelectorAll('h3');
      var p = this.$el.querySelectorAll('p');
      tl__detail = new TimelineMax({
        onReverseComplete: this.scrollTop
      })
        .staggerFromTo(h3, 0.8, {
          delay: 10,
          opacity: 0,
          x: -50,
        }, {
          opacity: 1,
          x: 0,
        }, 0.2)
        .staggerFromTo(p, 0.5, {
          opacity: 0,
          x: -50,
        }, {
          opacity: 1,
          x: 0,
        }, 0.2, '-=0.5')
    },
    animationOut() {
      tl__detail.timeScale(10).reverse();
    },
    scrollTop() {
      if (this.$el.offsetHeight > window.innerHeight) {
        TweenMax.to(window, 0.1, {
          scrollTo: 0,
          onComplete: this.resetPlantBoxHeight
        });
      } else {
        this.resetPlantBoxHeight();
      }
    },
    resetPlantBoxHeight() {
      this.$emit('updatePlanetBoxHeight', '100vh')
    }
  },
}
</script>

<style lang="scss" scoped>
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

@media screen and (orientation: portrait) {
  @import './styles/_port_Skills.scss'
}
</style>
