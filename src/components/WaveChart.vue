<template>
<div :style="cssProps" style="position: relative">
<div class="chart">
    <div class="fill">
    <svg  version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="300px" height="300px" viewBox="0 0 300 300" enable-background="new 0 0 300 300" xml:space="preserve">
          <path class="wave" fill="#04ACFF" id="waveShape" d="M300,300V2.5c0,0-0.6-0.1-1.1-0.1c0,0-25.5-2.3-40.5-2.4c-15,0-40.6,2.4-40.6,2.4
        c-12.3,1.1-30.3,1.8-31.9,1.9c-2-0.1-19.7-0.8-32-1.9c0,0-25.8-2.3-40.8-2.4c-15,0-40.8,2.4-40.8,2.4c-12.3,1.1-30.4,1.8-32,1.9
        c-2-0.1-20-0.8-32.2-1.9c0,0-3.1-0.3-8.1-0.7V300H300z"/>
    </svg>
    </div>
</div>
<div class="percentage"><p>{{percentage}}%</p></div>
    </div>
</template>

<script>
/* eslint-disable */
export default {
  name: 'WaveChart',
  props: ['percentage'],
  computed: {
    cssProps() {
      return {
          '--water-height': (this.percentage>100) ? -5 + "px": 150 - this.percentage * 1.55 + "px" ,
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.percentage{
    display:flex;
    justify-content: center;
    align-items: center;
    position: absolute;
    height:100%;
    width:100%;
    top:0;
    left:0;
    font-size: 50px;
    color: rgba(255, 255, 255, 0.68);
}
.percentage p {
    margin: 0;
    padding: 0;
}

.chart{
    background: #000;
    width: 150px;
    height: 150px;
    border-radius: 50%;
    overflow: hidden;
}
.chart .fill{
    animation-name: fillAnim;
    animation-duration: 4s;
    animation-timing-function: cubic-bezier(.2, .6, .8, .4);
    animation-fill-mode: forwards;
}
.chart .wave {
    fill: #04ACFF;
    width: 300px;
    height: 150px;
    animation-name: waveAnim;
    animation-iteration-count:infinite;
    animation-timing-function: linear;
    animation-duration: 0.5s;
}

@keyframes fillAnim {
    0% {
        transform: translate(0,150px);
    }
    100% {
        /*TODO:
0%      150 - 0 * 1.55 = 150
50%     150 - 50 * 1.55 = 72.5
100%    150 - 100 * 1.55 = -5
         */
        transform: translate(0,var(--water-height));
    }
}
@keyframes waveAnim {
    0% {
        transform: translate(-150px,0);
    }
    100% {
        transform: translate(0,0);
    }
}
</style>
