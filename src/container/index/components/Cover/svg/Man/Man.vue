<template lang="pug">
- var viewBox_size = 500;
svg#svg__man(viewBox=`0 0 ${viewBox_size} ${viewBox_size}`)

  ellipse#body__man.clothes(cx="115" cy="500" rx="150" ry="200")

  path#head__man.skin(d=`
    M50,250
    c0,-80 50,-150 120,-150
    s120,50 120,150
    c-10,80 -20,120 -100,150
    C60,420 50,250 50,250
  `)
  
  path#ear(d=`
    M130,250
    c-40,-10 -10,60 10,50
  `)
  
  g#g__eye_man
    ellipse#eye_w__man(cx="230" cy="205" rx="25" ry="27")
    ellipse#eye_ball__man(cx="238" cy="185" rx="5" ry="5")
  
  g#g__nose_and_hand
    
    g#hand__man
      path#finger.skin(d=`
        M280,300
        q7,-10 15,0
        l0,50
        l-15,0
        z
      `)
      
      g#g__dig
        path#palm__man.skin(d=`
          M275,340 
          q60,0 60,10
          l-5,80
          l-50,0 
          l-10,-65
          Q268,343 275,340
          z
        `)
        path#sleeve.clothes(d=`
          M295,410
          l40,0
          l-5,100
          l-55,0
          l-5,-100
          z
        `)
    path#blood(d=`
        M280,275
        h15
        v20
        q-8,5 -15,-5
        z
      `)
    path#nose__man.skin(d=`
      M280,250
      q10,0 20,30
      c5,0 10,20 0,20
      l-30,0
      q-10,-5 0,-20
      z
    `)
  
  path#hair__man(d=`
    M90,90
    c100,-30 150,20 200,-20
    c20,80 -50,120 -100,120
    s-100,90 -150,60
    C-30,190 50,100 90,90
  `)
</template>

<script>
import { TweenMax, TimelineMax, Power1 } from 'gsap'
export default {
  name: 'Man',
  data() {
    return {
      ani_dig: false,
      ani_shock: false,
      ani_stepBack: false,
    }
  },
  mounted() {
    // for firefox tranform origin problem
    var eye_w__man = this.$el.getElementById('eye_w__man');
    TweenMax.set(eye_w__man, {
      transformOrigin: "center center",
      rotation: 60,
    })
    this.digNoseAnimation()
  },
  beforeDestroy() {
    this.ani_dig.kill(); //or firefox will err
  },
  props: {
    shockSwitch: Boolean,
  },
  watch: {
    shockSwitch: function (newValue, oldValue) {
      if (newValue === true) {
        this.getShockAniamtion();
      }
    }
  },
  methods: {
    digNoseAnimation() {
      var g_dig = this.$el.getElementById('g__dig');
      this.ani_dig = TweenMax.to(g_dig, 1.5, {
        x: '-10%',
        repeat: -1,
        yoyo: true,
        ease: Power1.easeInOut
      });
    },
    getShockAniamtion() {
      this.ani_dig.kill();
      var man = this.$el;
      var eye_ball = this.$el.querySelector('#eye_ball__man');
      var nose_and_hand = this.$el.querySelector('#g__nose_and_hand');
      var blood = this.$el.querySelector('#blood');
      this.ani_shock = new TimelineMax()
        .to(man, 0.1, {
          y: -35,
          repeat: 1,
          yoyo: true,
          ease: Bounce,
        })
        .to(nose_and_hand, 0.1, {
          y: -37,
          repeat: 1,
          yoyo: true,
          ease: Bounce
        }, '-0.1')
        .to(eye_ball, 2.7, {
          delay: 1,
          x: -8,
          y: 20
        })
        .to(blood, 1, {
          y: 20,
        })
        .to(man, 1.7, {
          delay: 0.5,
          x: '-=35%',
          y: 20,
          ease: Power1.easeOut,
        })
        .to(man, 1.5, {
          delay: 0.2,
          y: 40,
          x: '-=35%',
          onComplete: () => {
            this.$emit('makeTitleFly');
          }
        })
    },
  }
}
</script>

<style lang="scss">
#svg__man {
  stroke: red;
  pointer-events: none;
}

#head__man {}

#ear {
  fill: none;
  stroke: red;
  stroke-width: 2;
}

#nose__man {
  fill: #ff6a00;
}

.skin {
  fill: orange;
  stroke: red;
}

#g__nose_and_hand {
  stroke-width: 2;
}

#palm__man {
  // animation: hand_dig 1s linear alternate infinite;
}

#eye_w__man {
  // rotation by tweenMax because of firefox not support transform-origin:'center'
  fill: white;
  stroke-width: 2;
}

#eye_ball__man {
  fill: none;
  stroke: black;
  stroke-width: 4;
}

#hair__man {
  fill: brown;
  storke: gray;
}

#body__man {
  transform-origin: center;
  transform: rotate(15deg);
}

.clothes {
  fill: gray;
}

#blood {
  fill: red;
  stroke: red;
}

@keyframes hand_dig {
  from {
    transform: translateX(0)
  }
  to {
    transform: translateX(-10%)
  }
}
</style>
