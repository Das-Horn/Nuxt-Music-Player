<template>
  <div>
    <canvas id="audio-vis-canvas"></canvas>
    <audio controls autoplay id="audio-player"></audio>
    <slot />
  </div>
</template>

<script>
export default {
  props: {
    children : Object
  }
}
</script>

<style>
  #audio-vis-canvas {
    height : 60vh;
    max-height: 800px;
    width:100vw;
  }


  /* Audio player styling */

  #audio-player {
    width:100vw;
  }

</style>
