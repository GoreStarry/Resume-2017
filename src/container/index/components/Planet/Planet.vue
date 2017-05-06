<template lang="pug">
.box__planet(
      @mouseenter="hoverEnter"
      @mouseleave="hoverLeave" 
      @click="zoomInPlaent")
  slot
  svg.ring.ring--en(viewBox="0,0,400,400")
    defs
      path#path__ring--en(d="M100,200a100,100 0 1,1 200,0a100,100 0 1,1 -200,0")
    path.stroke_assist(d="M70,200a130,130 0 1,0 260,0a130,130 0 1,0 -260,0")
    path.stroke_assist(stroke-dasharray="15, 10, 5, 10, 15" d="M60,200a140,140 0 1,0 280,0a140,140 0 1,0 -280,0")
    path.stroke_assist(d="M105,200a95,95 0 1,0 190,0a95,95 0 1,0 -190,0")
    text.text_ring.text_ring--en
      textPath(xlink:href="#path__ring--en" startOffset="70%" text-anchor="middle") | {{name_en}} |
    text.text_ring.text_ring--en
      textPath(xlink:href="#path__ring--en" startOffset="20%" text-anchor="middle") | {{name_zh}} |

  svg.planet(
      x="0px" y="0px" viewBox="0 0 512 512" style="enable-background:new 0 0 512 512;")
    path(
      :style="color_planet_style"
      d="M434.087,256c0-256-244.87-247.185-244.87-247.185C107.153,30.91,41.403,92.802,13.92,172.52 l15.763,22.034L4.897,205.911C1.685,222.11,0,238.858,0,256c0,56.145,18.074,108.07,48.721,150.271l48.903,19.327l-19.968,14.059 c30.739,29.856,68.951,52.061,111.561,63.539C189.217,503.196,434.087,512,434.087,256z"
      )
    polygon(
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

  svg.svg__sub(viewBox="0,0,100,100")
    circle.sub_planet(:style="color_planet_style" cx="50" cy="50" r="50")
</template>

<script>
import { TweenMax, TimelineMax } from 'gsap';

export default {
  name: 'Planet',
  data() {
    return {
      tl_ring: new TimelineMax(),
      sub_planet_offsetLeft: false,
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
  },
  watch: {
    planetOpen(newVal, oldVal) {
      if (oldVal == this.name_en && newVal == false) {
        this.zoomOutPlanet();
      }
    },
  },
  mounted() {
    var sub = this.$el;
    this.sub_planet_offsetLeft = getPosition(sub) + sub.offsetWidth / 2;

    function getPosition(element) {
      var xPosition = 0;

      while (element) {
        xPosition += (element.offsetLeft - element.scrollLeft + element.clientLeft);
        element = element.offsetParent;
      }
      return xPosition;
    }

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
    zoomInPlaent() {
      var sub = this.$el.querySelector(".planet");
      var cloud = this.$el.querySelectorAll(".cloud");

      this.tl_zoom = new TimelineMax().to(sub, 0.5, {
        scale: 22,
        y: window.innerHeight,
        x: window.innerWidth / 2 - this.sub_planet_offsetLeft,
        transformOrigin: "center center",
        force3D: false,
        ease: Power4.easeIn,
        zIndex: 1,

      }).to(cloud, 0.1, {
        opacity: 0,
        onComplete: () => {
          this.$emit("openPlanet", this.name_en)
        },
        onReverseComplete: () => {

        }
      })
    },
    zoomOutPlanet() {
      this.tl_zoom.reverse();
    },
    hoverEnter() {
      console.log('enter planet');
      const ring = this.$el.querySelector('.ring');

      TweenMax.to(ring, 0.5, {
        scale: 0.9,
        rotation: '+=45',
        force3D: false,
        ease: Bounce.easeOut,
        // ease: Power3.easeIn,
        onComplete: rotate,
      });

      function rotate() {
        TweenMax.to(ring, 20, {
          rotation: '+=360',
          repeat: -1,
          ease: Power0.easeNone
        });
      }

    },
    hoverLeave() {
      console.log('enter leave');
      const ring = this.$el.querySelector('.ring');
      TweenMax.to(ring, 0.5, {
        scale: 0.4,
        rotation: "+=90",
      })
    }
  }
}
</script>

<style lang="scss" scoped>
$planet_size: 100px;

.box__planet {
  position: relative;
  top: calc( 50% - #{$planet_size});
}


.planet {
  position: relative;
  cursor: pointer;
  width: $planet_size;
  height: $planet_size;
}

.svg__sub {
  position: absolute;
  top: 0;
  left: 0;
  transform: scale(0.1);
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
    transform-origin: center; // animation: ring_rotate 20s linear infinite;
  }
}

.stroke_assist {
  stroke: #5edafd;
}

@keyframes ring_rotate {
  from {
    transform: translate(-50%, -50%) rotate(0);
  }
  to {
    transform: translate(-50%, -50%) rotate(360deg);
  }
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
</style>
