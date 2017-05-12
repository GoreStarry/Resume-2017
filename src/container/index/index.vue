<template>
  <div class="Index">
    <starry-bg></starry-bg>
    <!--box__planets is for overflow hidden-->
    <div :style="{height: planetBoxHeight}" class="box__planets">
      <!--box__innerPlanets is for flexbox-->
      <div class="box__innerPlanets" :style="{initInnerHeight}">
        <planet v-for="(planet, index) in planet_datas" :planetIndex="index" :planetOpen="planet_open" :key="planet.name_en" @openPlanet="openPlanet" :name_en="planet.name_en" :name_zh="planet.name_zh" :color_planet="planet.color_planet" :color_planet_shadow="planet.color_planet_shadow" :color_name="planet.color_name"></planet>
      </div>
      <div class="box__link">
        <a href="mailto:sunrise91.t3@gmail.com">
          <img src="./components/AboutMe/images/icons/envelop.svg" alt="email icon" class="icon">
        </a>
        <a target="_blank" href="https://github.com/GoreStarry">
          <img src="./components/AboutMe/images/icons/github.svg" alt="github icon" class="icon">
        </a>
        <a href="tel:+886988104329">
          <img src="./components/AboutMe/images/icons/phone2.svg" alt="phone icon" class="icon">
        </a>
      </div>
    </div>
    <div :style="{pointerEvents: planet_open ? 'initial' : 'none'}" class="box__detail">
      <img @click="closePlanet" v-show="planet_open" class="btn__closePlanet" src="./images/repeat.svg" alt="">
      <skill-detail @updatePlanetBoxHeight="updatePlanetBoxHeight" :planetOpen="planet_open==planet_datas[0].name_en" :planetColor="planet_datas[0].color_planet"></skill-detail>
      <works-detail @updatePlanetBoxHeight="updatePlanetBoxHeight" :planetOpen="planet_open==planet_datas[1].name_en" :planetColor="planet_datas[1].color_planet"></works-detail>
      <aboutme-detail @updatePlanetBoxHeight="updatePlanetBoxHeight" :planetOpen="planet_open==planet_datas[2].name_en" :planetColor="planet_datas[2].color_planet"></aboutme-detail>
    </div>
    <svg-cover v-if="cover_mount" @unmountCover="unmountCover"></svg-cover>
  </div>
</template>

<script> 
import StarryBG from './components/StarryBg/StarryBg.vue';
import Cover from './components/Cover/Cover.vue';
import Skills from './components/Skills/Skills.vue';
import AboutMe from './components/AboutMe/AboutMe.vue';
import Works from './components/Works/Works.vue';
import Planet from './components/Planet/Planet.vue';

export default {
  name: 'Index',
  components: {
    'starry-bg': StarryBG,
    'planet': Planet,
    'svg-cover': Cover,
    'skill-detail': Skills,
    'aboutme-detail': AboutMe,
    'works-detail': Works,
  },
  data() {
    return {
      cover_mount: true,
      planet_open: false,
      planetBoxHeight: false,
      initInnerHeight: false,//for mobile vh problem
      planet_datas: [
        {
          name_en: "Skills",
          name_zh: "技能",
          color_planet: "#d12c2c",
          color_planet_shadow: "#791313",
          color_name: "#5edafd",
        }, {
          name_en: "Works",
          name_zh: "作品集",
          color_planet: "#1D71B8",
          color_planet_shadow: "#134a79",
          color_name: "#5edafd",
        }, {
          name_en: "About Me",
          name_zh: "關於我",
          color_planet: "#179013",
          color_planet_shadow: "#127020",
          color_name: "#5edafd",
        }
      ],
    }
  },
  mounted() {
    this.initInnerHeight = `height: ${window.innerHeight}`;
  },
  methods: {
    unmountCover() {
      this.cover_mount = false;
    },
    openPlanet(planet_name) {
      this.planet_open = planet_name;
    },
    closePlanet() {
      this.planet_open = false;
    },
    updatePlanetBoxHeight(height) {
      this.planetBoxHeight = height;
    }
  }
} 
</script>

<style lang="scss" scoped>
.Index {}


.box__planets,
.box__innerPlanets {
  box-sizing: border-box;
  position: absolute;
  left: 0;
  top: 0;
  width: 100vw;
  height: 100vh; // it will be changed by style for detail content height
}

.box__planets {
  overflow: hidden;
  perspective-origin: center bottom;
  perspective: 50vh;
}

$box_link_padding: 0.6rem;
.box__link {
  box-sizing: border-box;
  position: absolute;
  width: 20rem;
  height: 3.5rem;
  bottom: -$box_link_padding;
  left: 0;
  right: 0;
  margin: auto;

  padding: $box_link_padding;
  border: solid 3px rgba(white, 0.5);
  border-radius: 0.3rem;
  transform: rotateX(40deg);

  display: flex;
  flex-direction: row;
  justify-content: space-around;
  a {
    display: inline-block;
  }
  img {
    height: 100%;
    transition: transform 0.2s linear;
    opacity: 0.7;
    &:hover {
      transform: scale(1.2);
      animation: icon_shine 0.5s infinite alternate;
    }
  }
}

@keyframes icon_shine {
  from {
    opacity: 0.5;
  }
  to {
    opacity: 1;
  }
}

.planet__active {
  z-index: 1;
}

.box__detail {
  width: 100%;
  position: relative;
  z-index: 2;
  @at-root {
    .btn__closePlanet {
      position: fixed;
      z-index: 1;
      top: 5rem;
      right: 5rem;
      background-color: rgba(black, 0.5);
      box-shadow: -0.2rem 0.2rem 1rem black;
      transition: 0.2s linear;
      border-radius: 0.5rem;
      border: solid 1px white;
      padding: 0.5rem;
      transform-origin: center;
      cursor: pointer;
      transform: scaleX(-1);
      &:hover {
        transform: scale(-1.2, 1.2);
      }
    }
  }
}

@media screen and (orientation: landscape) {
  @import './styles/_land_index.scss';
}

@media screen and (orientation: portrait) {
  @import './styles/_port_index.scss';
}
</style>
