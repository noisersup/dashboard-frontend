<template>
<div class="wrapper" :style="cssProps">
    <div class="input-data">
        <input :type=type required>
      <div class="underline"></div>
      <label>{{label}}</label>
    </div>
</div>
</template>

<script>
/* eslint-disable */
export default {
  name: 'Input',
  props:{
    label: {
      default: "",
      type: Text
    },
    color: {
      default: "rgba(0,0,0,0)",
      type: Text
    },
    bgcolor: {
      default: "rgba(0,0,0,0)",
      type: Text
    },
    type: {
      default: "text",
      type: Text,
    },
  },
  computed: {
    cssProps() {
        return {
          '--bar-color': this.color,
          '--bg-color': this.bgcolor,
        }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.wrapper{
  background: var(--bg-color); 
  border: none !important;
  padding: 20px 10px 5px 10px;
  box-shadow: 0px 0px 10px rgba(0,0,0,0.1);
}
.wrapper *{
  background: rgba(0,0,0,0);
}
.wrapper .input-data{
  height: 20px;
  width: 100%;         
  position: relative;
}

.wrapper .input-data input{
  height: 100%;
  width: 100%;
  border: none;
  padding: 0;
  font-size: 17px;
  border-bottom: 2px solid silver;
}

    .wrapper .input-data label{
      position: absolute;
      bottom: 0px;
      left:0;
      color: grey;
      pointer-events: none;
      transition: all 0.3s ease;
    }

    .input-data input:focus ~ label,
    .input-data input:valid ~ label {
      transform: translateY(-20px);
      font-size: 15px;
    }


    .wrapper .input-data .underline{
      position:absolute;
      bottom: -2px;
      left: 0px;
      height: 2px;
      width:100%;
    }

    .input-data .underline:before{
      position:absolute;
      content: "";
      height: 100%;
      background-color: var(--bar-color);
      width:100%;
      transform: scaleX(0);
      left: 0;
      transition: transform 0.3s ease;
    }

    input:focus{
      outline-width: 0;
          outline: none;
    }

    .input-data input:focus ~ .underline:before,
    .input-data input:valid ~  .underline:before{
        transform: scaleX(1);
    }

</style>
