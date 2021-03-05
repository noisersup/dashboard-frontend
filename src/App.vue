<template>
  <div id='app' class='dashboard'>
      <component class='widget' v-for='widget in widgets' v-bind:key='widget' v-bind:is=widget></component>
  </div>
</template>

<script>
/* eslint-disable */
import EmptyWidget from './components/widgets/EmptyWidget'
import TodoWidget from './components/widgets/TodoWidget'

export default {
  name: 'App',
  components: {
    EmptyWidget,
    TodoWidget
  },
  data () {
    return {
      widgets: ['EmptyWidget', 'TodoWidget', 'EmptyWidget', 'EmptyWidget']
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
    grid-template-columns: 200px 200px 200px;
    grid-template-rows: 200px 200px 200px 200px 200px;
    grid-gap: 10px;
}
.widget{
    margin: 0;
    background-color: yellow;
    border-radius: 15px;
    border: 0;
}
</style>
