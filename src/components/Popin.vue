//Generic Popin component

//Id is composed 'popin' and with dynid props
//default id is 'popin-default'

//Pass content as children nodes (slot)

<template lang="html">
  <div class="popin" v-bind:id="'popin-'+dynid">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name:"Popin",
  props:{
    dynid:{
      type:String,
      default:'default'
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
    width:60%;
    height:60%;
    min-width: 320px;
    min-height: 400px;
    background-color: #DDD;
    box-shadow: 2px 2px 21px -6px rgba(0,0,0,0.5);
  }
</style>
