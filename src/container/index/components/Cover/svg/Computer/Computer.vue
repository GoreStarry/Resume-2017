<template lang="pug">
- var viewBox_size = 400;
svg#svg__computer(viewBox=`0 0 ${viewBox_size} ${viewBox_size}`)

  defs
    linearGradient#lightGradient(x1='1' x2='0')
      stop(offset="0%" stop-color="white"  stop-opacity="0.2")
      stop(offset="100%" stop-color="white" stop-opacity="0")
  
  path#screen.screen--rotate(d=`
    M200,200
    l0,-150
    l15,0
    q20,150 0,300
    h-15
    z
  `)
  path#stand(d=`
    M210,210
    q20,-50 80,80
    q40,90 0,90
    l-100,0
  `)
  rect#table(x="130" y="383" rx="10" ry="10" width="375" height="75" )
  
  path#light.screen--rotate(d=`
    M198,41
    l0,300
    l-30,15
    l0,-330
    z
  ` fill="url(#lightGradient)")

</template>

<script>
export default {
  name: 'Computer'
}
</script>

<style lang="scss">
#svg__computer {
  stroke: green;
  fill: none;
  pointer-events: none;
}

#screen {
  fill: gray;
  transform-origin: center;
}

.screen--rotate {
  transform: rotate(15deg);
}

#stand {
  stroke: gray;
  stroke-width: 7px;
}

#table {
  fill: #d18832;
}

#light {
  stroke: none;
  transform-origin: center 50%;
  animation: shine 3s linear infinite;
}

@keyframes shine {
  from,
  9%,
  20%,
  25% {
    opacity: 0.3,
  }
  to,
  10%,
  23%,
  30% {
    opacity: 1,
  }
}
</style>
