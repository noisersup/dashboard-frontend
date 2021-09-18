<template>
    <div id="widget">
    <FullCalendar :options="calendarOptions"/>
    </div>
</template>

<script>
import '@fullcalendar/core/vdom'
import FullCalendar from '@fullcalendar/vue'
import dayGridPlugin from '@fullcalendar/daygrid'
import interactionPlugin from '@fullcalendar/interaction'

export default {
  components: {
    FullCalendar // make the <FullCalendar> tag available
  },
  data () {
    return {
      calendarOptions: {
        height: '100%',
        plugins: [dayGridPlugin, interactionPlugin],
        initialView: 'dayGridMonth',
        editable: true,

        firstDay: 1,

        dateClick: this.handleDateClick,
        eventChange: this.handleEventChange,

        events: [
          {title: 'event 1', date: '2021-09-13'}
        ]
      },
      inputEvent: {}
    }
  },
  methods: {
    handleDateClick: function (arg) {
      this.addPopup(arg.date)
    },
    handleEventChange: function (arg) {
      alert('event changed! ' + '\n' + arg.event.start + '\n' + arg.event.end)
    },

    addPopup: function (day) {
      // Create mock event with name as textbox
      this.inputEvent = {title: 'task title...', date: this.dateStr(day)}
      // this.inputEvent.date = this.dateStr(day)
      console.log(this.inputEvent)
      /* 
          -remove if unfocused
      */
    },
    dateStr: function (date) {
      let yyyy = date.getFullYear().toString();
      let mm = (date.getMonth()+1).toString();
      let dd  = date.getDate().toString();

      let mmChars = mm.split('');
      let ddChars = dd.split('');

      return yyyy + '-' + (mmChars[1]?mm:"0"+mmChars[0]) + '-' + (ddChars[1]?dd:"0"+ddChars[0]);},
  },
  mounted: function () {
    this.inputEvent = {title: 'task title...', date: '2021-09-09'}
    this.calendarOptions.events.push(this.inputEvent)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#widget{
    background-color: #ededed;
}
</style>
