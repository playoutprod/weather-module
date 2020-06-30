/Stat dispay components

<template lang="html">
  <div class="stat">
    <div class="stat_name">{{localized_name}}</div>
    <div class="stat_value"><span class="value">{{localized_value}}</span><span class="unit">{{localized_unit}}</span></div>
  </div>
</template>

<script>

//Consider this to be handled globally in next version
import locale from '../locales/fr_FR.js';

//Values are computed from localize values (if exists)
export default {
  name:"Stat",
  props:["value","name"],
  computed:{
    localized_name:function(){
      return(locale.translations[this.name] ? locale.translations[this.name] : this.name)
    },
    localized_value:function(){
      let value = this.value.value;
      //Rounded values
      if(this.value.units === 'hPa' || this.value.units === 'degrees' || this.value.units === '%'){
        value = Math.round(value)
      }else if(!this.value.units){
        value=new Date(value);
        value = value.getHours()+':'+value.getMinutes()+':'+value.getSeconds();
      }
      return(locale.translations[value] ? locale.translations[value] : value)
    },
    localized_unit:function(){
      return(locale.translations[this.value.units] ? locale.translations[this.value.units] : this.value.units)
    },
  }
}
</script>

<style lang="css" scoped>
  .stat_name{
    font-size: .8em;
    margin-bottom: 1em;
  }
  .stat_value{
    font-size: 1.5em;
  }
  .unit{
    font-size: .5em;
  }
</style>
