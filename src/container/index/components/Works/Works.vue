<template lang="pug">
transition( :duration="{ enter: 0, leave: 500 }")
  .box__Works(v-show="planetOpen")
    header
    .container__bg
      section
        .box__work(v-for="work in works")
          h3
            a(:href="work.link" target="_new") {{work.name}}
          h4 - {{work.technologies}}
          p {{work.description}}
</template>


<script>
import { TweenMax, TimelineMax } from 'gsap';
import ScrollToPlugin from 'gsap/ScrollToPlugin';
let tl__detail;
export default {
  name: 'Works',
  props: {
    planetOpen: Boolean,
    planetColor: String,
  },
  data() {
    return {
      works: [
        {
          name: 'Stories Kingdom',
          link: 'https://github.com/GoreStarry/Stories_Kingdom',
          technologies: 'React,Express, Mongo, Mongoose, Draft.js, Webpack, Nightwatch, Chai, Sinon',
          description: '針對中文直式文章書寫排版需求，以及重現翻頁文韻所開發的線上編輯平台',
        },
        {
          name: 'Nuns on the run',
          link: 'https://github.com/GoreStarry/nuns_on_the_run',
          technologies: 'React, Express, Mongo, Socket.io, Canvas',
          description: '將桌遊轉成再投影機上即時顯示所有玩家手機輸入的移動操作，玩家並可用手機拍出套上修女造型的照片。',
        },
        {
          name: 'Mind Test VR',
          link: 'https://www.youtube.com/watch?v=Pxxw_ZY2nf8',
          technologies: 'A-Frame, Vue.js, TheaterJS',
          description: 'WebVR的中文演出相關研究開發。',
        },
        {
          name: 'CodePen',
          link: 'http://codepen.io/GoreWang/',
          technologies: 'SCSS, SVG, PUG, GSAP...',
          description: '視覺刻版相關習作',
        },
        {
          name: '韋氏中醫（中國）',
          link: 'http://www.weis-cmg.com/',
          technologies: 'AngularJS(1), Gulp, Django...',
          description: '形象官網製作'
        },
        {
          name: 'Remiel',
          link: 'https://github.com/GoreStarry/Remiel_github',
          technologies: 'jQuery, Gulp, SASS...',
          description: '中文直式翻頁，自寫自繪自做的線上圖文展演平台'
        },
        {
          name: '愛在轉角新版（尚未上線）',
          link: false,
          technologies: 'React, i18n, Socket.IO, Canvas, WebVR, Webpack, Google map(Geo) , 百度地圖, Velocity...',
          description: '聊天室,雷達動態,照片牆,緣份地圖...'
        },
        {
          name: '愛在轉角(舊) - 愛在發聲',
          link: 'https://www.l-somewhere.com/graffiti/',
          technologies: 'AngularJS(1), Django...',
          description: '具有私下連結性的討論版...'
        },
        {
          name: '易賾商城',
          link: false,
          technologies: 'React, Django...',
          description: '中國單品銷售商城'
        },
        {
          name: 'Resume 2015',
          link: 'https://gorestarry.github.io/Resume-2015/',
          technologies: 'jQuery, Canvas, Gulp, SASS...',
          description: '2015前相關前端作品...'
        },
      ]
    }
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
  beforeDestroy() {
    tl__detail.kill();
  },
  updated() {
    if (this.planetOpen) {
      this.extendBoxHeight();
      this.animationIn();
    }
  },
  methods: {
    extendBoxHeight() {
      var boxHeight = this.$el.offsetHeight;
      if (boxHeight > window.innerHeight) {
        this.$emit('updatePlanetBoxHeight', boxHeight + 'px')
      }
    },
    animationIn() {
      var box__work = this.$el.querySelectorAll('.box__work');
      // var p = this.$el.querySelectorAll('p');
      tl__detail = new TimelineMax({
        onReverseComplete: this.scrollTop
      })
        .staggerFromTo(box__work, 0.4, {
          opacity: 0,
          y: 30,
        }, {
          opacity: 1,
          y: 0,
        }, 0.2)
    },
    animationOut() {
      tl__detail.timeScale(5).reverse();
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
$planet_padding_top: 45vh;

.box__Works {
  padding-top: $planet_padding_top;
  display: flex;
  flex-direction: column; // align-items: center;
  justify-content: center;
}

.container__bg {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

section {
  max-width: 900px;
  padding: 0 1.5rem 3rem 1.5rem;
}

.box__work {
  margin-bottom: 1rem;
  h3 {
    font-size: 1.5rem;
    line-height: 2rem;
    font-weight: bold;
    a {
      color: goldenrod;
      font-style: italic;
    }
  }
  h4 {
    font-size: 1rem;
    line-height: 1.5rem;
    text-indent: 1rem;
    color: #b6dcff;
  }
  p {
    font-size: 1.1rem;
    line-height: 2rem;
    text-indent: 1rem;
  }
}

@media screen and (orientation: portrait) {
  @import './styles/_port_Works.scss'
}
</style>
