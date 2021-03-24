<template>
<div id="widget">
    <div class="clock">
        <p class="minutes">24</p>
        <p class="seconds">59</p>
    </div>
</div>
</template>

<script>
/* eslint-disable */
export default {
  name: 'PomodoroWidget'
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
