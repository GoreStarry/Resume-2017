<template>
  <div class="Index">
    <div id="stars1" class="stars"></div>
    <div id="stars2" class="stars"></div>
    <div id="stars3" class="stars"></div>
    <div class="box__planets">
      <planet-02 name_en="Skills" name_zh="技能" color_planet="#d12c2c" color_planet_shadow="#791313" color_name="#5edafd"></planet-02>
      <planet-02 name_en="Work" name_zh="作品集" color_planet="#1D71B8" color_planet_shadow="#134a79" color_name="#5edafd"></planet-02>
      <planet-02 name_en="About Me" name_zh="關於我" color_planet="#1aac16" color_planet_shadow="#127020" color_name="#5edafd"></planet-02>
    </div>
    <!--<svg-cover v-if="cover_mount" @unmountCover="unmountCover"></svg-cover>-->
  </div>
</template>

<script> 
import Planet01 from './components/Planet/Planet01.vue';
import Planet02 from './components/Planet/Planet02.vue';
import Cover from './components/Cover/Cover.vue';
export default {
  name: 'Index',
  components: {
    'planet-01': Planet01,
    'planet-02': Planet02,
    'svg-cover': Cover,
  },
  data() {
    return {
      cover_mount: true,
      planet_open: false,
    }
  },
  methods: {
    unmountCover() {
      this.cover_mount = false;
    },
    openPlanet(planet_name) {// 'Skill'||'Work'||'AboutMe'
      this.planet_open = planet_name;
    },
    closePlanet() {
      this.planet_open = false;
    },
  }
} 
</script>

<style lang="scss" scoped>
@import './styles/index.scss';

.box__planets {
  position: relative;
}

// design by https://codepen.io/saransh/pen/BKJun
@function multiple-box-shadow ($n) {
  $value: '#{random(2000)}px #{random(2000)}px #FFF';
  @for $i from 2 through $n {
    $value: '#{$value} , #{random(2000)}px #{random(2000)}px #FFF'
  }
  @return unquote($value)
}

$shadows-small: multiple-box-shadow(700);
$shadows-medium: multiple-box-shadow(200);
$shadows-big: multiple-box-shadow(100);


.stars {
  position: absolute;
  left: 0;
  top: 0;
}

#stars1 {
  width: 1px;
  height: 1px;
  background: transparent;
  box-shadow: $shadows-small;
  animation: animStar 250s linear infinite;

  &:after {
    content: " ";
    position: absolute;
    top: 2000px;
    width: 1px;
    height: 1px;
    background: transparent;
    box-shadow: $shadows-small;
  }
}

#stars2 {
  width: 2px;
  height: 2px;
  background: transparent;
  box-shadow: $shadows-medium;
  animation: animStar 300s linear infinite;

  &:after {
    content: " ";
    position: absolute;
    top: 2000px;
    width: 2px;
    height: 2px;
    background: transparent;
    box-shadow: $shadows-medium;
  }
}

#stars3 {
  width: 3px;
  height: 3px;
  background: transparent;
  box-shadow: $shadows-big;
  animation: animStar 350s linear infinite;

  &:after {
    content: " ";
    position: absolute;
    top: 2000px;
    width: 3px;
    height: 3px;
    background: transparent;
    box-shadow: $shadows-big;
  }
}

@keyframes animStar {
  from {
    transform: translateX(0px)
  }
  to {
    transform: translateX(-2000px);
  }
}
</style>
