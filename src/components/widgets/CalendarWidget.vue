<template>
<div id="widget">
<vc-calendar class="calendar" is-expanded 
  :first-day-of-week="2" 
  :masks="masks"
  :attributes='attributes'></vc-calendar>
</div>
</template>

<script>
/* eslint-disable */
import Vue from 'vue';
import VCalendar from 'v-calendar';

// Use v-calendar & v-date-picker components
Vue.use(VCalendar, {
  componentPrefix: 'vc' // Use <vc-calendar /> instead of <v-calendar />        // ...other defaults
});



export default {
  name: 'CalendarWidget',
  data(){
    return{
      attributes: [
      {
        key: 'today',
        highlight: true,
        dates: new Date(),
      },
          {
            key: 'ny',
            highlight: true,
            //dot: true,
            bar: true,
            //content: 'red',
            dates: new Date(1631491200),
          },],
      masks: {
        weekdays: 'WWW',
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .calendar {
    height: 100%;
  }
  /* .calendar.vc-container{
    --day-content-border-highlight:
    --day-content-height : 50px; */
  /* } */
  .calendar /deep/ .vc-weeks > .vc-grid-cell-row-1{
    border: solid #000000;
    border-width: 1px 1px 1px 0px;
    
  }
  .calendar /deep/ .vc-day{
    border: solid #000000;
    border-width: 0px 1px 1px 0px;
  }
  .calendar /deep/ .vc-weeks > .vc-grid-cell-col--7{
    border-left: 1px solid;
  }

  .calendar /deep/ .vc-w-full, 
  .calendar /deep/ .vc-grid-cell, 
  .calendar /deep/ .vc-grid-container, 
  .calendar /deep/ .vc-pane {
    height: 100%;
  }
</style>
