<template>
<div id="widget">
    <div class="clock">
        <p class="minutes">{{minutes}}</p>
        <p class="seconds">{{seconds}}</p>
    </div>
</div>
</template>

<script>
/* eslint-disable */
export default {
  name: 'PomodoroWidget',
  data () {
      return{
        minutes: 25,
        seconds: 0,
        startTimestamp: 1616586344,
      }
      
  },
  methods: {
    sync(){
        let curr = Math.floor(Date.now() /1000);
        let timeLeft = 25*60 + (this.startTimestamp - curr);

        this.minutes = Math.floor(timeLeft/60);
        this.seconds = Math.floor(timeLeft%60);
    }
  },
  mounted() {
      this.startTimestamp = Math.floor(Date.now() /1000);
      this.sync();
      setInterval( this.sync,1000);
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#widget{
    background-color: red;
    display: flex;
    justify-content: center;
    align-items: center;
}
p{
    margin: 0;
}
.clock{
    width:200px;
    /* height: 100px; */
    /* text-align: center; */
}
.minutes{
    display: inline;
    font-size: 80px;
}
.seconds{
    display: inline;
    font-size: 30px;
}
</style>
