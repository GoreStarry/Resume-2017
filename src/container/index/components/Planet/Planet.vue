<template lang="pug">
.box__planet(
      )
  svg.ring.ring--en(viewBox="0 0 400 400")
    defs
      path#path__ring--en(d="M100,200a100,100 0 1,1 200,0a100,100 0 1,1 -200,0")
    path.stroke_assist(d="M70,200a130,130 0 1,0 260,0a130,130 0 1,0 -260,0")
    path.stroke_assist(stroke-dasharray="15, 10, 5, 10, 15" d="M60,200a140,140 0 1,0 280,0a140,140 0 1,0 -280,0")
    path.stroke_assist(d="M105,200a95,95 0 1,0 190,0a95,95 0 1,0 -190,0")
    text.text_ring.text_ring--en
      textPath(xlink:href="#path__ring--en" startOffset="70%" text-anchor="middle") | {{name_en}} |
    text.text_ring.text_ring--en
      textPath(xlink:href="#path__ring--en" startOffset="20%" text-anchor="middle") | {{name_zh}} |
  -var tile_padding = 50;
  svg.planet(@mouseenter="hoverEnter"
      @mouseleave="hoverLeave" 
      @click="zoomInCheck"
      x="0px" y="0px" viewBox=`0 ${-tile_padding} 512 ${512+tile_padding*2}`)
    defs
      path#path__title(d="M16,256a240,240 0 1,1 480,0a240,240 0 1,1 -480,0")
    path(
      :style="color_planet_style"
      d="M434.087,256c0-256-244.87-247.185-244.87-247.185C107.153,30.91,41.403,92.802,13.92,172.52 l15.763,22.034L4.897,205.911C1.685,222.11,0,238.858,0,256c0,56.145,18.074,108.07,48.721,150.271l48.903,19.327l-19.968,14.059 c30.739,29.856,68.951,52.061,111.561,63.539C189.217,503.196,434.087,512,434.087,256z"
      )
    polygon.cloud(
      style="fill:#4A8EC6;" 
      points="403.013,339.478 256.105,339.478 222.713,322.783 256.105,306.087 403.013,306.087 ")
    rect.cloud(x="172.522" y="306.087" style="fill:#77AAD4;" width="83.578" height="33.391")
    rect.cloud(x="256.1" y="239.304" style="fill:#4A8EC6;" width="155.726" height="33.391")
    rect.cloud(x="72.348" y="306.087" style="fill:#A4C6E2;" width="66.783" height="33.391")      
    path.cloud(style="fill:#77AAD4;" d="M77.651,439.652h133.931v-33.391H48.728C57.406,418.21,67.082,429.387,77.651,439.652z")
    path.cloud(style="fill:#A4C6E2;" d="M4.909,205.913h150.918v-33.391H13.928C10.197,183.342,7.175,194.491,4.909,205.913z")
    rect.cloud(x="189.217" y="172.522" style="fill:#77AAD4;" width="33.391" height="33.391")
    path(
      :style="color_planet_shadow_style" 
      d="M256,0c-23.106,0-45.49,3.08-66.783,8.819C298.213,38.196,378.435,137.721,378.435,256 s-80.221,217.804-189.217,247.181C210.51,508.92,232.894,512,256,512c141.385,0,256-114.615,256-256S397.385,0,256,0z")

    text
      textPath.text__title(xlink:href="#path__title" startOffset="12%" text-anchor="middle") {{name_en}}

</template>

<script>
import { TweenMax, TimelineMax } from 'gsap';
import MobileDetect from 'mobile-detect';

//prevent resize event emit when mobile broswer scroll up/down and address bar showed/hidden
var md = new MobileDetect(window.navigator.userAgent);
const windowInnerHeightWithAddressBar = md.mobile() ? window.innerHeight : false;


let zoomLock = false; // will lock three planet component click zoom
export default {
  name: 'Planet',
  data() {
    return {
      tl_ring: new TimelineMax(),
      planet_offsetLeft: 0,
      planet_offsetTop: 0,
      tl_zoom: false,
    }
  },
  props: {
    name_en: String,
    name_zh: String,
    color_planet: String,
    color_planet_shadow: String,
    color_name: String,
    planetOpen: [String, Boolean],
    planetIndex: Number,
  },
  watch: {
    planetOpen(newVal, oldVal) {
      if (oldVal == this.name_en && newVal == false) {
        this.zoomOutPlanet();
      }
    },
  },
  mounted() {
    this.getPlanetOffset();
    window.addEventListener('resize', this.getPlanetOffset);
  },
  destroyed() {
    window.removeEventListener('resize', this.getPlanetOffset);
  },
  computed: {
    color_planet_style() {
      return `fill: ${this.color_planet}`
    },
    color_planet_shadow_style() {
      return `fill: ${this.color_planet_shadow}`
    },
  },
  methods: {
    getPlanetOffset() {
      var planet = this.$el;
      if (window.innerWidth >= window.innerHeight) { // landscape
        this.planet_offsetLeft = - (this.planetIndex - 1) * (window.innerWidth * 0.15 + 100)
        this.planet_offsetTop = 0;
      } else {// portrait 
        if (!this.planetOpen) { //prevent resize event emit when mobile broswer scroll up/down and address bar showed/hidden
          this.planet_offsetTop = (this.planetIndex - 1) * window.innerHeight * 0.23;
          console.log(this.planet_offsetTop);
          this.planet_offsetLeft = 0;
        }
      }

      // function getPositionX(element) {
      //   var xPosition = 0;
      //   while (element) {
      //     xPosition += (element.offsetLeft - element.scrollLeft + element.clientLeft);
      //     element = element.offsetParent;
      //   }
      //   return xPosition;
      // }

      // function getPositionY(element) {
      //   var yPosition = 0;
      //   while (element) {
      //     yPosition += (element.offsetTop - element.scrollTop + element.clientTop);
      //     element = element.offsetParent;
      //   }
      //   return yPosition;
      // }

    },
    zoomInCheck() {
      if (zoomLock) {
        return false;
      } else {
        zoomLock = true;
        this.zoomInPlaent();
      }
    },
    zoomInPlaent() {
      var planet = this.$el.querySelector(".planet");
      var title = this.$el.querySelector(".text__title")
      var cloud = this.$el.querySelectorAll(".cloud");
      var x, y;

      if (window.innerWidth >= window.innerHeight) { // landscape
        x = this.planet_offsetLeft;
        y = 1250 - window.innerHeight / 2; // 1100 = planet heiht 100px * scale 22 / 2
      } else { // portrait
        x = false;
        y = (windowInnerHeightWithAddressBar || window.innerHeight) * 1.21 - this.planet_offsetTop;
        console.log(y);
      }

      this.tl_zoom = new TimelineMax()
        .set(this.$el, {
          zIndex: 1,
        })
        .to(planet, 0.5, {
          force3D: false,
          scale: 1,
          x,
          y,
          transformOrigin: "center center",
          ease: Power4.easeIn,
        })
        .to(title, 0.5, {
          opacity: 1,
          force3D: false,
          attr: {
            startOffset: '25%',
          },
          onComplete: () => {
            this.$emit("openPlanet", this.name_en)
          },
        })
        .to(cloud, 0.1, {
          opacity: 0,
          onReverseComplete: () => {
            zoomLock = false;
          }
        })
    },
    zoomOutPlanet() {
      this.tl_zoom.reverse();
    },
    hoverEnter() {
      const ring = this.$el.querySelector('.ring');
      if (window.innerWidth >= window.innerHeight) {
        TweenMax.to(ring, 0.5, {
          scale: 0.9,
          rotation: '+=45',
          force3D: false,
          ease: Bounce.easeOut,
          onComplete: rotate,
        });
      }

      function rotate() {
        TweenMax.to(ring, 20, {
          rotation: '+=360',
          repeat: -1,
          ease: Power0.easeNone
        });
      }

    },
    hoverLeave() {
      if (window.innerWidth >= window.innerHeight) {
        const ring = this.$el.querySelector('.ring');
        TweenMax.to(ring, 0.5, {
          scale: 0.4,
          rotation: "+=90",
        })
      }
    }
  }
}
</script>

<style lang="scss" scoped>
$planet_origin_size: 100px;
$planet_size: $planet_origin_size*22;

.box__planet {
  position: absolute;
  pointer-events: none;
  top: 50%;
  left: 50vw;
  &:nth-child(1) {
    transform: translate(calc( -50% - #{$planet_origin_size} - 15vw), -50%);
  }
  &:nth-child(2) {
    transform: translate(-50%, -50%);
  }
  &:nth-child(3) {
    transform: translate(calc( -50% + #{$planet_origin_size} + 15vw), -50%);
  }
}

.text__title {
  font-size: 3rem;
  fill: white;
  opacity: 0;
  font-family: 'Orbitron', sans-serif;
  font-weight: bold;
}

.planet {
  position: relative;
  cursor: pointer;
  width: $planet_size;
  height: #{$planet_size*1.2};
  transform: scale(0.045);
  transform-origin: center;
  pointer-events: all;
}

.ring {
  width: 300px;
  height: 300px;
  fill: none;
  stroke: red;
  stroke-width: 2px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%) scale(0.45);
  &--en {
    transform-origin: center;
  }
}

.stroke_assist {
  stroke: #5edafd;
}

.text_ring {
  font-size: 2rem;
  &--en {
    fill: #5edafd;
    stroke: none;
  }
  &--ch {

    // transform-origin: center;
    // transform: rotate(180deg);
  }
}

@media screen and (orientation: portrait) {
  @import './styles/_port_Planet.scss'
}
</style>
