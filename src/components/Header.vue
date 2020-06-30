//Header component is graphic weather sumpup.

<template lang="html">
  <div class="header">
    <div class="title">
      <h2>{{city}}</h2>
    </div>
    <div class="weather">
      <div class="icon"><img v-bind:src="getIcon()"/></div>
      <div class="quick_infos">
        <h3 class="temperature"><span class="value">{{Math.round(temperature.value)}}</span><span class="unit">°{{temperature.units}}</span></h3>
        <p class="weather_code">{{weather_code.value.replace('_',' ')}}</p>
      </div>
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
  computed:{
    city:function(){
      //Get city name from location
      if(this.location.locale_names){
        //Result from direct search
        return(this.location.locale_names.default[0])
      }else{
        return(this.location.name ? this.location.name : '')
      }

    }
  },
  data(){
    return {
      publicPath: process.env.BASE_URL
    }
  },
  methods :{
    getIcon(){
      //Get icon url from weather_code
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
  }
}
</script>

<style lang="css" scoped>
  .header{
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
  }
  .title{
    font-size: 1.5em;
    height:30%;
    width:100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: 0 5%;
    box-sizing: border-box;
    align-items: center;
  }
  .title h2{
    margin: 0;
  }
  .weather{
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    height:70%;
    width:100%;
  }
  .weather>*{
    width:50%;
  }
  .icon{
    height:100%;
    padding-right: 5%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-end;
  }
  .icon img{
    height:auto;
    width:70%;
    max-width: 200px;
  }
  .quick_infos{
    text-align: left;
    padding-left: 5%;
  }
  .temperature{
    font-size: 6em;
    line-height: .8em;
    margin: 0 0 .2em 0;
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
  }
  .temperature .unit{
    font-size: .5em;
    line-height: .8em;
  }
  .weather_code{
    font-size: 1.5em;
    margin: 0;
    color:#888;
  }
  @media only screen and (max-width: 700px){
    .temperature{
      font-size: 5em;
    }
  }
  @media only screen and (max-width: 500px){
    .temperature{
      font-size: 4em;
    }
    .weather_code{
      font-size: 1em;
    }
    .title{
      height:40%;
    }
    .weather{
      height:60%;
    }
  }
</style>
