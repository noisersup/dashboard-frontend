<template>
<div class="content">
<div class="groups">
  <div v-for="group in groups" :key="group">
    <WaveChart/>
  </div>
  <div class="addGroup">+</div>
</div>
</div>
</template>

<script>
/* eslint-disable */
import WaveChart from '../WaveChart'

export default {
  name: 'FinanceWidget',
  components: {
    WaveChart,
  },
  data () {
    return {
      groups: [
        {
          'maxExpenses': 0,
          'currExpenses': 0,
          'expenses': []
        }
      ]
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.content::-webkit-scrollbar {
  display: none;
}

/* Hide scrollbar for IE, Edge and Firefox */
.content{
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;  /* Firefox */
}
.content {
  overflow: scroll;
  background: lightblue;
}
.groups{
  margin-top: 40px;
  display: grid;
  grid-template-columns: 150px 150px;
  gap: 40px;
  justify-content: center;
  align-content: center;
}
.addGroup{
    background-color: grey;
    width: 150px;
    height: 150px;
    border-radius: 50%;
    font-size: 40px;
    transform: translate3d(0, 0, 0);
    display: flex;
    align-items: center;
    justify-content: center;
}
</style>
