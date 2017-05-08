<template lang="pug">
- var window_radious = 100, viewBox_size = 500 ;

- var circle_path=`M${viewBox_size/2},${viewBox_size/4}  a${window_radious},${window_radious} 0 0 0 0,${window_radious*2} a${window_radious},${window_radious} 0 0 0 0, ${-window_radious*2}z`


svg.ship(@click.once="shakeNameplate" viewBox=`0 0 ${viewBox_size} ${viewBox_size}`)

  defs
    path#window__path(d=`${circle_path}`)
    
    path#text_f2e__path(d=`
    M${viewBox_size/2 - window_radious - 12},220 
    a${window_radious + 10} ${window_radious + 6} 0 1 1 ${2*window_radious + 24},0`)
    
  path.wall(d=`
          M0,0 h${viewBox_size} v${viewBox_size} h${-viewBox_size}z
          ${circle_path}`)
          
  g.window_frame
    use(xlink:href='#window__path')

  g.window_screw
    use(xlink:href='#window__path')
    
  text#text_f2e(rotate="0,0,0,0,0,0,0")
    textPath(xlink:href="#text_f2e__path" startOffset="50%" text-anchor="middle") Front-End Engineer

  - var nameplate_width = 100,nameplate_height = 28;
  
  g#g_nameplate
    rect.nameplate__bg(x=`${viewBox_size/2 - nameplate_width/2}` y=`338` rx="10" ry="10" width=`${nameplate_width}` height=`${nameplate_height}` fill="orange")
    text.text__name(x=`${viewBox_size/2}`, y=`356` text-anchor="middle" alignment-baseline="hanging") Gore Wang
      
</template>

<script>
import { TweenMax } from 'gsap'

export default {
  name: 'Window',
  mounted() {

  },
  props: {
    flyFSwtich: Boolean,
  },
  watch: {
    flyFSwtich(newValue, oldValue) {
      if (newValue === true) {
        this.flyF2EAnimation();
      }
    }
  },
  methods: {
    shakeNameplate() {
      var nameplate = this.$el.getElementById('g_nameplate');
      TweenMax.to(nameplate, 0.5, {
        rotation: 720,
        scale: 1.5,
        yoyo: true,
        repeat: 1,
        transformOrigin: "center center",
        onComplete: () => {
          this.$emit('getShock');
        }
      })
    },
    flyF2EAnimation() {
      var window_radious = 100, viewBox_size = 500;

      var text_f2e__path = this.$el.getElementById('text_f2e__path');
      TweenMax.to(text_f2e__path, 2, {
        attr: {
          d: `
      M${viewBox_size / 2 - window_radious - 148},100
      a${window_radious + 300},${0} 
      0 1 1 
      ${2 * window_radious + 300},0
      `
        },
        ease: Power1.easeInOut,
        onComplete: () => {
          this.$emit('zoomIn');
        }
      })
    },
  }
}
</script>
<style lang="scss" scoped>
.ship {
  height: 100vmin;
  width: 100vmin;
  cursor: pointer;
}

.wall {
  fill: brown;
}

.window_frame {
  fill: none;
  stroke: #452020;
  stroke-width: 10px;
}

.window_screw {
  fill: none;
  stroke: green;
  stroke-width: 3;
  stroke-dasharray: 1, 10;
  stroke-linecap: round;
}

.text_path {
  fill: none;
  stroke: black;
}

#text_f2e {
  font-weight: bold;
  text-align: center;
  font-size: 28px;
  font-family: 'Orbitron', sans-serif;
}

.text__name {
  font-weight: bold;
  font-family: 'Abril Fatface', cursive;
}

@media screen and (orientation: portrait) {
  @import './styles/_port_ship.scss'
}
</style>
