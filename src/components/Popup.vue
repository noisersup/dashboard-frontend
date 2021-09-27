<template>
  <div class="popup" :style="cssProps">
    <div class="header"></div>
    <slot></slot>
  </div>
</template>

<script>
/* eslint-disable */
export default {
  name: "Popup",
  props: {
    x: {
      default: 0,
      type: Number,
    },
    y: {
      default: 0,
      type: Number,
    },
    theme: {
      default: "rgba(0,0,0,0)",
      type: Text,
    },
  },
  computed: {
    cssProps() {
      return {
        "--x": this.x + "px",
        "--y": this.y + "px",
        "--theme": this.theme,
      };
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.popup {
  position: absolute;
  left: var(--x);
  top: var(--y);
  box-shadow: 1px 1px 60px black;
  border-radius: 20px;
  width: 400px;
  height: 200px;

  background: rgb(255, 255, 255);
  background: linear-gradient(
    167deg,
    rgba(255, 255, 255, 1) 0%,
    rgba(214, 214, 214, 1) 100%
  );
}

.header {
  width: 100%;
  height: 25px;
  background: var(--theme);
  border-radius: 20px 20px 0 0;
}
</style>
