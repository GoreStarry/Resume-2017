<template lang="pug">
transition( :duration="{ enter: 0, leave: 500 }")
  .box__AboutMe(v-show="planetOpen")
    header
      h2 - Gore Wang (王偉泰) -
    .container__bg
      section
        h3 [ 工作經歷 ]
        p.p_company 燿麒科技
        p 
          span 新版愛在轉角
          | ：網站開發、
        p 
          span 易賾單品商城
          | ：網站開發、
        p 
          span 韋氏中醫
          |：形象官網、WebVR應用開發、
        p 
          span 愛在轉角
          |：網站的維護，「愛在發聲」開發、
        p.p_company 謎攻城工作室
        p 企劃、製作、平面設計、網站設計、主持

        h3 [ 技術分享 ]
        p 
          a(href="http://slides.com/gorestarry/deck-2#/") WebVR vs Chinese@Taipei WebGL&WebVR
        p
          a(href="https://www.youtube.com/watch?v=bnorojYDays&t=265s") Draft.js @線上React讀書會
        p
          a(href="http://slides.com/gorestarry/deck#/") A-Frame WebVR @Web Tech Topic

        h3 [ 長處 ]
        p 擅長組織溝通、邏輯分析、問題克服＆創意實現。

        h3 [ 職業能力 ]
        p 對不斷變動的主流前端技術有充分自主學習意願，由於自身對視覺設計也有相關涉略，能完美接收實踐來自設計端的細節需求，更願意從工程技術面的角度來錦上添花。
        p 無論效能、視覺、產品企劃，都有解決問題向前邁進的熱情與能力。
        h3 [ 態度 ]
        p 作為一個曾參與創業的經驗者，我能理解細節要求對於產品成敗的關鍵性。
        p 所以對於工作上的要求，我絕不會被牽制在一個「我覺得沒差」的心理狀態，而願意將工作務實的做到更高的需求層級。

        h3 [ 熱情 ]
        p 對於實現製作好的產品有很大的熱情，樂於征服市場與使用者。
        p 也會不斷自主學習新技術，並願意主動提案分享討論新技術應用方向。
</template>

<script>
import { TweenMax, TimelineMax } from 'gsap';
let tl__detail;
export default {
  name: 'AboutMe',
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
      var h2 = this.$el.querySelector('h2');
      var h3 = this.$el.querySelectorAll('h3');
      var p = this.$el.querySelectorAll('p');
      tl__detail = new TimelineMax({
        onReverseComplete: this.scrollTop
      })
        .fromTo(h2, 0.8, {
          rotationX: '-90',
        }, {
          rotationX: 0,
        })
        .fromTo(h3, 0.5, {
          opacity: 0,
          x: -50,
        }, {
          opacity: 1,
          x: 0,
        }, 0.5, '-=0.2')
        .staggerFromTo(p, 0.5, {
          opacity: 0,
          x: -50,
        }, {
          opacity: 1,
          x: 0,
        }, 0.2)
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

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s
}

.fade-enter,
.fade-leave-to {
  opacity: 0
}
</style>

<style lang="scss" scoped>
$planet_padding_top: 35vh;

.box__AboutMe {
  padding-top: $planet_padding_top;
  display: flex;
  flex-direction: column; // align-items: center;
  justify-content: center;
}

header {
  min-height: #{ 50vh - $planet_padding_top };
  text-align: center;
  h2 {
    font-size: 2rem;
  }
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
  h3 {
    font-size: 1.5rem;
    margin-bottom: 0.5rem;
    font-weight: bold;
    color: yellow;
    opacity: 0;
    &:not(:first-of-type) {
      margin-top: 1.5rem;
    }
  }
  $p_padding: 2rem;
  $p_textIndent: 2.2rem;
  a {
    font-weight: bold;
    color: white;
  }
  p {
    opacity: 0;
    padding: 0 $p_padding;
    font-size: 1.1rem;
    line-height: 2rem;
    text-indent: $p_textIndent;
    &.p_company {
      font-size: 1.3rem;
      margin: 1.5rem 0 3px -1.3rem;
    }
  }
}

@media screen and (orientation: portrait) {
  @import './styles/_port_AboutMe.scss';
}
</style>
