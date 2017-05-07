<template lang="pug">
.box__AboutMe(v-show="planetOpen")
  header
    h1 About Me
    h2 - Gore Wang (王偉泰) -
  //- .container__bg(:style="{backgroundColor: planetColor}")
  .container__bg
    section
      h3 「長處」
      p 擅長組織溝通、邏輯分析、問題克服＆創意實現。

      h3 「職業能力」
      p 對不斷變動的主流前端技術有充分自主學習意願，由於自身對視覺設計也有相關涉略，能完美接收實踐來自設計端的細節需求，更願意從工程技術面的角度來錦上添花。
      p 無論效能、視覺、產品企劃，都有解決問題向前邁進的熱情與能力。
      h3 「態度」
      p 作為一個曾參與創業的經驗者，我能理解細節要求對於產品成敗的關鍵性。
      p 所以對於工作上的要求，我絕不會被牽制在一個「我覺得沒差」的心理狀態，而願意將工作務實的做到更高的需求層級。

      h3 「熱情」
      p 對於實現製作好的產品有很大的熱情，樂於征服市場與使用者。
      p 也會不斷自主學習新技術，並願意主動提案分享討論新技術應用方向。
</template>

<script>
export default {
  name: 'AboutMe',
  props: {
    planetOpen: Boolean,
    planetColor: String,
  },
  watch: {
    planetOpen(newValue, oldValue) {
      if (newValue !== oldValue) {
        if (newValue) {// open
          this.animationIn();
        }
        else {
          this.$emit('updatePlanetBoxHeight', '100vh')
          this.animationOut();
        }
      }
    }
  },
  updated() {
    var boxHeight = this.$el.offsetHeight;
    if (boxHeight > window.innerHeight) {
      this.$emit('updatePlanetBoxHeight', boxHeight + 'px')
    }
  },
  methods: {
    animationIn() {
      // console.log('in');
      var h3 = this.$el.querySelectorAll('h3');
      TweenMax.staggerFrom(h3, 0.9, {
        opacity: 0,
        x: -50,
      }, 0.5)
      var p = this.$el.querySelectorAll('p');
      TweenMax.staggerFrom(p, 0.5, {
        delay: 2,
        opacity: 0,
        x: -50,
      }, 0.2)
    },
    animationOut() {
      // console.log('out');
    }
  },
}
</script>

<style lang="scss">
$planet_padding_top: 10vh;

.box__AboutMe {
  padding-top: $planet_padding_top;
  display: flex;
  flex-direction: column; // align-items: center;
  justify-content: center;
  h3 {
    font-size: 1.5rem;
    margin-bottom: 0.5rem;
    font-weight: bold;
    color: yellow;
    &:not(:first-of-type) {
      margin-top: 1.5rem;
    }
  }
  p {
    padding: 0 2rem;
    font-size: 1.1rem;
    line-height: 2rem;
    text-indent: 2.2rem;
  }
}

header {
  min-height: #{ 50vh - $planet_padding_top };
  text-align: center;
  h1 {
    font-size: 10rem;
  }
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
  padding: 0 1.5rem;
}
</style>
