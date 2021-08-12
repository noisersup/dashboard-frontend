<template>
  <div id='app' class='dashboard'>
      <component class='widget' :class="{'vPanoramic' : widget.size == 1, 'hPanoramic' : widget.size == 2,'big' : widget.size == 3,'bigger' : widget.size == 4,'giant' : widget.size == 5,'ghPanoramic' : widget.size == 6}" v-for='widget in widgets' v-bind:key='widget' v-bind:is=widget.name></component>
  </div>
</template>

<script>
/* eslint-disable */
import EmptyWidget from './components/widgets/EmptyWidget'
import TodoWidget from './components/widgets/TodoWidget'
import PomodoroWidget from './components/widgets/PomodoroWidget'
import CalendarWidget from './components/widgets/CalendarWidget'

export default {
  name: 'App',
  components: {
    EmptyWidget,
    TodoWidget,
    PomodoroWidget,
    CalendarWidget
  },
  data () {
    return {
      widgets: [
      {name:'PomodoroWidget', size:3}, 
      {name:'TodoWidget', size:6},
      {name:'CalendarWidget', size:5}, 
      {name:'EmptyWidget', size:0}, 
      {name:'EmptyWidget', size:0}, 
      {name:'EmptyWidget', size:0}, 
      {name:'EmptyWidget', size:0}, 
      {name:'EmptyWidget', size:0}, 
      {name:'EmptyWidget', size:0}, 
      {name:'EmptyWidget', size:0},  
      {name:'EmptyWidget', size:0}]
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.dashboard{
    display: grid;
    grid-template-columns: repeat(auto-fill,minmax(200px,200px));
    grid-auto-rows: 200px;
    grid-gap: 10px;
}
.widget{
    margin: 0;
    background-color: yellow;
    border-radius: 15px;
    border: 0;
}
.hPanoramic{
    grid-row-start: auto;
    grid-row-end: span 2;
}
.vPanoramic{
    grid-column-start: auto;
    grid-column-end: span 2;
}
.big{
    grid-column-start: auto;
    grid-column-end: span 2;
    grid-row-start: auto;
    grid-row-end: span 2;
}
.bigger{
  grid-column-start: auto;
  grid-column-end: span 3;
  grid-row-start: auto;
  grid-row-end: span 2;
}
.giant{
  grid-column-start: auto;
  grid-column-end: span 4;
  grid-row-start: auto;
  grid-row-end: span 3;
}
.ghPanoramic{
  grid-column-start: auto;
  grid-column-end: span 2;
  grid-row-start: auto;
  grid-row-end: span 4;
}
</style>
