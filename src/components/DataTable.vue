<template lang="html">
  <div class="datatable">
    <ul>
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
    height:100%;
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    align-items: center;
    margin:0;
    padding:0;
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
