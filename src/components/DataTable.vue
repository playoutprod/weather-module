<template lang="html">
  <div class="datatable">
    <ul v-pan="onPan" v-bind:style="{left:scrollPosition+'px'}">
      <li v-for="(stat,key) in weather" :key="key">
          <Stat :value="stat" :name="key"/>
      </li>
    </ul>
  </div>
</template>

<script>

import Stat from './Stat.vue';
import config from '../config/data.js';

export default {
  name:"DataTable",
  props:["wdata"],
  components:{
    Stat
  },
  data(){
    //SCrollposition control the list left style, scrolltarget is used to store the DOM ref.
    return({
      scrollPosition : 0,
      scrollTarget : null
    })
  },
  updated(){
    //Set a dom ref to calculate width (last element of the list)
    this.scrollTarget = this.$el.querySelector('li:last-child');
  },
  methods:{
    onPan(e){
      //Add the delta to the current position of the list
      let newScrollPosition = this.scrollTarget.parentNode.offsetLeft + e.deltaX/10;

      //Calculate element width by addind last element position + last element width
      const larg = this.scrollTarget.offsetLeft+this.scrollTarget.clientWidth;

      if(newScrollPosition > 0){
        //Max left is 0
        newScrollPosition = 0;
      }else if(newScrollPosition<this.scrollTarget.parentNode.clientWidth-larg){
        //Max right is the width of the popin minus width of the list
        newScrollPosition = this.scrollTarget.parentNode.clientWidth-larg;
      }

      this.scrollPosition = newScrollPosition;
    }
  },
  computed:{
    //Weather data is filtered
    weather : function(){
      const weather={}
      if(this.wdata){
        //Convert object to array, filter, then loop to return a new object.
        Object.entries(this.wdata).filter(stat=>{
          return(config.datatable.indexOf(stat[0])>-1);
        }).forEach(stat => {
          weather[stat[0]]=stat[1];
        });
      }
      return(weather)
    }
  }
}
</script>

<style lang="css" scoped>
  .datatable{
    background-color: #333;
    color:#FFF;
    overflow-x: hidden;
  }
  ul{
    position: relative;
    height:100%;
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    align-items: center;
    margin:0;
    padding:0;
    overflow: visible;
    flex-wrap: nowrap;
  }
  li{
    float:left;
    list-style: none;
    height:100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    min-width: 10em;
  }
</style>
