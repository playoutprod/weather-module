
<template lang="html">
  <div id="weather-module" v-bind:class="classObject">
    <Search v-on:change="onLocationChange"></Search>
    <Popin dynid="weather" v-bind:message="message" v-bind:loading="status === 'searching'">
      <div class="container" v-if="showContent">
        <Header v-bind:location="location" v-bind:shift="location.shift" v-bind:temperature="wdata.temp" v-bind:weather_code="wdata.weather_code"></Header>
        <DataTable v-bind:wdata="wdata"></DataTable>
      </div>
    </Popin>
  </div>
</template>

<script>

import axios from 'axios';


//CONFIGS
import api_config from '../config/config.js';
import data_config from '../config/data.js'

//Merging config files into one object;
const config = {api:api_config,data:data_config}

//COMPONENTS
import Search from './Search.vue';
import Popin from './Popin.vue';
import Header from './Header.vue';
import DataTable from './DataTable.vue';

export default {
  name:"WeatherModule",
  components: {
    Search,
    Popin,
    Header,
    DataTable
  },
  methods:{
    onLocationChange(location){
      //Called each time search is performed

      //Change status to searching
      this.status='searching';

      //Reset datas
      this.wdata = {};
      this.location= {};

      if(location !== 'notfound'){
        //Set location data then get local time and then get weather data
        this.location = location;
        //Direct search return geoloc, autocomplete latlng
        const latlng = location._geoloc ? location._geoloc : location.latlng;
        this.getTime(latlng).then(()=>{
          this.getWeather(latlng)
        })
      }else{
        //Nos result found change status to display message
        this.status='endsearch';
      }

    },
    getTime(latlng){
      //Get local time form location
      return(
        new Promise((resolve,reject)=>{
          axios.get(config.api.time_endpoint,{
            params : {
              lat:latlng.lat,
              lng:latlng.lng,
              key:config.api.time_key,
              format:"json",
              by:"position"
            }
          })
          .then(resp => {
            //Set locatime to location
            this.location.gmtOffset = resp.data.gmtOffset;
            this.location.localTime = new Date(resp.data.timestamp*1000 + new Date().getTimezoneOffset()*60*1000);
            resolve(resp);
          }).catch(err => {
            console.log(err);
            reject(err);
          })
        })
      )
    },
    getWeather(latlng){
      return(
        new Promise((resolve,reject)=>{
          axios.get(config.api.weather_endpoint,{
            params : {
              lat:latlng.lat,
              lon:latlng.lng,
              apikey:config.api.weather_key,
              fields:config.data.filter
            }
          })
          .then(resp => {
            //Save weather data
            this.wdata = resp.data;
            //Convert sunrise and sunset to UTC timestamp
            this.wdata.sunrise.value = (new Date(this.wdata.sunrise.value)).getTime() + parseInt(this.location.gmtOffset*1000)+ new Date().getTimezoneOffset()*60*1000;
            this.wdata.sunset.value = (new Date(this.wdata.sunset.value)).getTime() + parseInt(this.location.gmtOffset*1000)+ new Date().getTimezoneOffset()*60*1000;

            //Set shift (night or day) from sunrise time and localtime
            if(this.wdata.sunrise.value - this.location.localTime.getTime() < 0){
              if(this.wdata.sunset.value - this.location.localTime.getTime() > 0){
                this.location.shift = 'day';
              }else{
                this.location.shift = 'night';
              }
            }else{
              this.location.shift = 'night';
            }

            this.status='endsearch';
            resolve(resp);
          }).catch(err => {
            console.log(err);
            reject(err);
          })
      }))
    }
  },
  data(){
    //Location : current location
    //wdata weather data fetched
    //Status to handle messages
    return {
      location : {shift:"day"},
      wdata:{},
      status:'init'
    }
  },
  computed:{
    classObject : function (){
      return(
        {
          hasdata :Object.keys(this.wdata).length > 0,
          day:this.location.shift === "day",
          night : this.location.shift === "night",
        }
      )
    },
    showContent: function(){
      return(this.status === 'endsearch');
    },
    message : function(){
      //Message content
      if(Object.keys(this.wdata).length === 0){
        if(this.status === 'init'){
          return("Veuillez chercher une ville");
        }if(this.status === 'searching'){
          return("Patience, la recherche est en court");
        }else{
          return("Désolé nous n'avons pas trouvé de résultat");
        }

      }else{
        return("");
      }
    }
  }
}
</script>

<style lang="css">
  #weather-module{
    display:flex;
    flex-direction: column;
    justify-content:center;
    align-items:center;
    width:100%;
    height:100%;
  }
  #weather-module:before,#weather-module:after{
    content:'';
    display: block;
    position: absolute;
    top:0;
    left:0;
    width:100%;
    height:100%;
    z-index:-1;
    transition: opacity 1s linear;
  }
  #weather-module:before{
    background: rgb(255,160,0);
    background: linear-gradient(90deg, rgba(255,160,0,1) 0%, rgba(255,255,106,1) 100%);
  }
  #weather-module:after{
    opacity:0;
    background: rgb(23,0,78);
    background: linear-gradient(90deg, rgba(23,0,78,1) 0%, rgba(0,129,185,1) 100%);
  }
  #weather-module.night:after{
    opacity:1;
  }
  #weather-module .popin{
    width:70%;
    height:70%;
    min-width: 320px;
    min-height: 400px;
  }
  #weather-module .container{
    position: relative;
    width:100%;
    height:100%;
    display: flex;
    flex-direction: column;
    align-items: stretch;
    justify-content: center;
  }
  #weather-module .header{
    height:70%;
  }
  #weather-module .datatable{
    height:30%;
  }
  @media only screen and (max-width: 950px){
    #weather-module .popin{
      height:50%;
    }
  }
  @media only screen and (max-width: 950px){
    #weather-module .popin{
      height:70%;
    }
  }
</style>
