<template>
<div id="widget">
    <div v-if="startTimestamp != 0 && (minutes >= 0 || seconds >= 1)" class="clock">
        <p class="minutes">{{minutes}}</p>
        <p class="seconds">{{seconds}}</p>
    </div>
    <div v-else>
        <div @click="start" class="startCircle">
            <div class="startBtn"></div>
        </div>
    </div>
</div>
</template>

<script>
/* eslint-disable */
export default {
  name: 'PomodoroWidget',
  data () {
      return{
        minutes: 0,
        seconds: 0,
        startTimestamp: 0,
      }
      
  },
  methods: {
    sync(){
        if(this.startTimestamp != 0){
            let curr = Math.floor(Date.now() /1000);
            let timeLeft = 25*60 + (this.startTimestamp - curr);

            this.minutes = Math.floor(timeLeft/60);
            this.seconds = Math.floor(timeLeft%60);
        }
    },
    syncTimestamp(){
        (async () => {
            const res = await fetch("http://localhost:8005/pomodoro");
            res.json().then(
                data => {
                    this.startTimestamp = data.timestamp;
                    this.sync();
                }
            )
        })();
    },
    genTimestamp(){
        this.startTimestamp = Math.floor(Date.now().toString()/1000);
    },
    start(){
        this.genTimestamp();
        (async () => {
          const res = await fetch("http://localhost:8005/pomodoro", {
            method: 'POST',
            cache: 'no-cache',
            credentials: 'same-origin',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify({timestamp: this.startTimestamp})
          });
        })();
    },
  },
  
  mounted() {
    this.startTimestamp = this.syncTimestamp();
    this.sync()
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

.startBtn{
    width: 74px;
    height: 74px;
    border-style: solid;
    box-sizing: border-box;
    border-width: 37px;
    border-color: white;
    border-width: 37px 0px 37px 74px;
    border-color: transparent transparent transparent white;

    transform: translateX(7px);
}
.startCircle{
    padding: 30px;
    border-radius: 100%;
    border-style: solid;
    border-width: 5px;
    border-color: white;

}
</style>
