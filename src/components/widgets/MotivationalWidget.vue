<template>
<div class="widget">
  <p class="text"> {{ this.getText() }}</p>
</div>
</template>

<script>
/* eslint-disable */
export default {
  name: 'MotivationalWidget',
  methods: {
    getText: function() {
      // TODO: return string from request
        (async () => {
            const res = await fetch("http://localhost/quote");
            res.json().then(
                data => {
                    return data.quote;
                }
            )
        })();
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.widget {
  background-color: rgb(247, 247, 247);
  display: flex;
  justify-content: center;
  align-items: center;
}
.text{
  font-size: 2em;
}
</style>
