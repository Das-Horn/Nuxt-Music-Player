<template>
  <div>
    <Header />
    <Content>
      <MusicPlayer />
    </Content>
  </div>
</template>

<script>
  // import * as Vue from 'vue'
  // import { BootstrapVue, BootstrapVueIcons } from 'bootstrap-vue'
  import Content from '~/components/Content.vue'

  // Vue.use(BootstrapVue)
  // Vue.use(BootstrapVueIcons)


  export default {
    data : () => ({
      musicUpdate : Object,
      ctx : Object,
      canvas : Object,
      audioElement : Object,
      analyser : Object,
      data : Array,
      first : true,
    }),
    methods : {
      // This is the function that is called when the user clicks the "Open File" button. It opens a file
      // picker and allows the user to select a file. It then sets the source of the audio player to the file
      // that was selected.
      async openFile() {
        try {
          let f;
          [f] = await window.showOpenFilePicker();
          console.log(f);
          const mp3Player = document.querySelector('#audio-player');
          mp3Player.src = URL.createObjectURL(await f.getFile());
          if(this.first){
            this.first = !this.first;
            this.musicVis();
          }
        } catch (error) {
          console.warn(error);
        }
      },
      async selectSong() {
        const newSong = window.sessionStorage.getItem('Song');
      },
      async musicVis() {
        this.canvas = document.getElementById("audio-vis-canvas");
        this.ctx = this.canvas.getContext("2d");
        this.audioElement = document.getElementById("audio-player");
        let audioCtx = new AudioContext();
        this.analyser = audioCtx.createAnalyser();
        this.analyser.fftSize = 2048;
        let source = audioCtx.createMediaElementSource(this.audioElement);
        source.connect(this.analyser);
        //this connects our music back to the default output, such as your
        //speakers
        source.connect(audioCtx.destination);
        this.data = new Uint8Array(this.analyser.frequencyBinCount);
        requestAnimationFrame(this.loopingFunction);
      },
      // A recursive function that calls itself.
      loopingFunction() {
          requestAnimationFrame(this.loopingFunction);
          this.analyser.getByteFrequencyData(this.data);
          this.draw();
      },
      // Drawing the lines on the canvas.
      draw(){
        let data = [...this.data];
        // console.log(this.getAverageDataPoints(data, 3));
        data = this.getAverageDataPoints(data, 10)
        this.ctx.clearRect(0,0,this.canvas.width,this.canvas.height);
        let space = this.canvas.width / (data.length * 1);
        data.forEach((value,i)=>{
            this.ctx.beginPath();
            this.ctx.moveTo(space*i,this.canvas.height); //x,y
            this.ctx.lineTo(space*i,this.canvas.height-(value)); //x,y
            this.ctx.strokeStyle = '#ff9e1f';
            this.ctx.stroke();
        });
      },
      // Taking the data array and dividing it into segments. It then averages the values in each segment and
      // returns a new array with the average values.
      getAverageDataPoints(data, seg) {
        const intervalAmount = Math.floor(data.length / seg);
        let newData = [];
        let total = 0;


        data.forEach((value, i) => {
          if(((i+1) % intervalAmount) === 0){
            newData = [...newData, total / intervalAmount];
            total = 0
            }else {
            // console.warn('counting');
            total += value;
          }
        });
        return newData;
      }
    },
    components: { Content },
    name: 'IndexPage'
  }
</script>
