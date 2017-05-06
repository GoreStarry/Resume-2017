<template>
  <div class="Index">
    <starry-bg></starry-bg>
    <div :style="{height: planetBoxHeight}" class="box__planets">
      <planet v-for="planet in planet_datas" :planetOpen="planet_open" :key="planet.name_en" @openPlanet="openPlanet" :name_en="planet.name_en" :name_zh="planet.name_zh" :color_planet="planet.color_planet" :color_planet_shadow="planet.color_planet_shadow" :color_name="planet.color_name"></planet>
      <!--<planet-02 @openPlanet="openPlanet" name_en="Skills" name_zh="技能" color_planet="#d12c2c" color_planet_shadow="#791313" color_name="#5edafd"></planet-02>-->
      <!--<planet-02 @openPlanet="openPlanet" name_en="Work" name_zh="作品集" color_planet="#1D71B8" color_planet_shadow="#134a79" color_name="#5edafd"></planet-02>-->
      <!--<planet-02 @openPlanet="openPlanet" name_en="About Me" name_zh="關於我" color_planet="#1aac16" color_planet_shadow="#127020" color_name="#5edafd"></planet-02>-->
    </div>
    <div v-show="planet_open" class="box__detail">
      <img @click="closePlanet" v-show="planet_open" class="btn__closePlanet" src="./images/repeat.svg" alt="">
      <skill-detail @updatePlanetBoxHeight="updatePlanetBoxHeight" :planetOpen="planet_open==planet_datas[0].name_en" :planetColor="planet_datas[0].color_planet"></skill-detail>
      <works-detail @updatePlanetBoxHeight="updatePlanetBoxHeight" :planetOpen="planet_open==planet_datas[1].name_en" :planetColor="planet_datas[1].color_planet"></works-detail>
      <aboutme-detail @updatePlanetBoxHeight="updatePlanetBoxHeight" :planetOpen="planet_open==planet_datas[2].name_en" :planetColor="planet_datas[2].color_planet"></aboutme-detail>
    </div>
    <!--<svg-cover v-if="cover_mount" @unmountCover="unmountCover"></svg-cover>-->
  </div>
</template>

<script> 
import StarryBG from './components/StarryBg/StarryBg.vue';
import Planet from './components/Planet/Planet.vue';
import Cover from './components/Cover/Cover.vue';
import Skills from './components/Skills/Skills.vue';
import AboutMe from './components/AboutMe/AboutMe.vue';
import Works from './components/Works/Works.vue';

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
      console.log('set height:' + height);
      this.planetBoxHeight = height;
    }
  }
} 
</script>

<style lang="scss" scoped>
@import './styles/index.scss';

.Index {
  // display: flex;
  // flex-direction: column;
  // justify-content: center;
  // align-items: center;
  // min-height: 100vh;
}

.box__planets {
  box-sizing: border-box;
  position: absolute;
  left: 0;
  top: 0;
  width: 100vw;
  height: 100vh;
  padding: 0 5rem;
  overflow: hidden;

  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: flex-start;
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
      top: 5rem;
      right: 5rem;
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
</style>
