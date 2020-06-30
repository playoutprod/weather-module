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
      return(locale.translations[this.value.value] ? locale.translations[this.value.value] : this.value.value)
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
