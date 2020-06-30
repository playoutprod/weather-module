//Generic Popin component

//Id is composed 'popin' and with dynid props
//default id is 'popin-default'

//Pass content as children nodes (slot)
//Pass message trought prop message

<template lang="html">
  <div class="popin" v-bind:id="'popin-'+dynid">
    <slot></slot>
    <div class="message" v-bind:class="{show :displayMessage}">
      <span class="content">{{message}}</span>
    </div>
  </div>
</template>

<script>
export default {
  name:"Popin",
  props:{
    dynid:{
      type:String,
      default:'default'
    },
    message:{
      type:String,
      default:''
    }
  },
  computed:{
    displayMessage : function(){
      //Display error messages
      if(this.message === ''){
        return(false);
      }else{
        return(true);
      }
    }
  }
}
</script>

<style lang="css" scoped>
  .popin{
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    border-radius: 2em;
    overflow: hidden;
    width:90%;
    height:90%;
    background-color: #EEE;
    box-shadow: 2px 2px 21px -6px rgba(0,0,0,0.5);
    position: relative;
  }
  .message{
    position: absolute;
    top:0;
    left:0;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width:100%;
    height:100%;
    background-color: #FFF;
    color:#888;
    opacity:0;
    font-size: 2em;
    pointer-events:none;
    padding: 2em;
    box-sizing: border-box;
  }
  .message.show{
    opacity:1;
  }
</style>
