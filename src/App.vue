<script>
export default {
  data() {
    return {
      msg : "Press the spacebar or touch me to start!" ,
      milliseconds: 0,
      intervalId: null,
      running: false ,
      recorde_list : [],
    };
  },
  computed: {
    formattedTime() {
      const totalSeconds = Math.floor(this.milliseconds / 1000);
      const hours = Math.floor(totalSeconds / 3600).toString().padStart(2, '0');
      const minutes = Math.floor((totalSeconds % 3600) / 60).toString().padStart(2, '0');
      const seconds = (totalSeconds % 60).toString().padStart(2, '0');
      const millis = (this.milliseconds % 1000).toString().padStart(3, '0');
      return `${hours}:${minutes}:${seconds}.${millis}`;
    }
  },
  methods: {
    startTimer(){
      if (!this.running) {
        this.intervalId = setInterval(() => {
          this.milliseconds += 10;}, 10);
        this.running = true;

      }
    },
    stopTimer(){
      clearInterval(this.intervalId)
      this.running = false
    },
    restart(){
      this.milliseconds = 0
      this.recorde_list = []
    },
    play_sound(){
      let random = Math.floor((Math.random()*32) + 1)
      if (random < 16){
        var sound = new Audio(`/src/assets/khandeh/${random}.mp3`)
      }else{
        var sound = new Audio(`/src/assets/khandeh/${random}.wav`)
      }
      sound.play()
    },
    handleKeyup(event){
      if (event.code === 'Space' | event.type === "touchend") {
        this.play_sound()
        if (this.running){
          this.recorde_list.push(this.formattedTime)
        }
        else{
          this.startTimer();
        }
      } else if (event.code === 'Enter') {
        this.stopTimer();
      }
      else if (event.code === 'Backspace') {
        this.stopTimer();
        this.restart();
      }
    }
  },
  mounted() {
    window.addEventListener('keyup', this.handleKeyup);
    window.addEventListener( 'touchend', this.handleKeyup);
  },
  beforeDestroy() {
    window.removeEventListener('keyup', this.handleKeyup);
    this.stopTimer();
  }
};
</script>
<template>

<div :class="[{jus_center:running},{blue:running}]"  class="box">
  <div :class="{timer:!running}">
    <span class="timer-fs">{{ formattedTime }}</span>
  </div>
  <span>{{ random }}</span>
  <span v-if="!running" class="descriptions">{{msg}}</span>
</div>


















</template>

  <!-- <span>{{ recorde_list }}</span> -->
  <!-- <audio controls>
    <source src="/src/assets/khandeh/MediaBaz.net-baby-laugh-giggle-.mp3" type="audio/mpeg">
    Your browser does not support the audio tag.
  </audio> -->