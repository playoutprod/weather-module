<template lang="html">
  <div class="header">
    <div class="title">
      <h2>{{city}}</h2>
    </div>
    <div class="temperature">
      <h3>{{Math.round(temperature.value)}} °{{temperature.units}}</h3>
    </div>
    <div class="weather_code">
      <p>{{weather_code.value.replace('_',' ')}}</p>
      <img v-bind:src="getIcon()"/>
    </div>
  </div>
</template>

<script>
export default {
  name:"Header",
  props:{
    location:{
      type:Object,
      default:()=>({})
    },
    temperature:{
      type:Object,
      default:()=>({
        value:0,
        units:"C"
      })
    },
    weather_code:{
      type:Object,
      default:()=>({
        value:"clear"
      })
    },
    shift:{
      type:String,
      default:"day"
    }
  },
  data(){
    return {
      city : '',
      publicPath: process.env.BASE_URL,
    }
  },
  methods :{
    getIcon(){
      if(this.weather_code){
        let icon = 'default.svg';
        if(this.weather_code.value === 'clear' || this.weather_code.value === 'mostly_clear' || this.weather_code.value === 'partly_cloudy'){
          icon=this.weather_code.value+'_'+this.shift+'.svg';
        }else{
          icon=this.weather_code.value+'.svg';
        }
        return(`${this.publicPath}statics/weather_icons/${icon}`)
      }else{
        return('');
      }

    }
  },
  updated(){
    this.city = this.location.name
  }
}
</script>

<style lang="css" scoped>
</style>
