<template lang="html">
  <div id="weather-module">
    <Search v-on:change="onLocationChange"></Search>
    <Popin dynid="weather">
      <div class="container">
        <Header v-bind:location="location" v-bind:shift="shift" v-bind:temperature="wdata.temp" v-bind:weather_code="wdata.weather_code"></Header>
        <DataTable v-bind:weather="wdata"></DataTable>
      </div>
    </Popin>
  </div>
</template>

<script>

import axios from 'axios';

import config from '../config/config.js';

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
      this.location = location;
      this.getTime(location.latlng).then(()=>{
        this.getWeather(location.latlng)
      })
    },
    getTime(latlng){
      return(
        new Promise((resolve,reject)=>{
          axios.get(config.time_endpoint,{
            params : {
              lat:latlng.lat,
              lng:latlng.lng,
              key:config.time_key,
              format:"json",
              by:"position"
            }
          })
          .then(resp => {
            this.localTime = new Date((resp.data.timestamp-resp.data.gmtOffset)*1000);
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
          axios.get(config.weather_endpoint,{
            params : {
              lat:latlng.lat,
              lon:latlng.lng,
              apikey:config.weather_key,
              fields:["temp","humidity","wind_speed","wind_direction","baro_pressure","precipitation","sunrise","sunset","cloud_cover","weather_code"]
            }
          })
          .then(resp => {
            this.wdata = resp.data;
            this.shift = new Date(this.wdata.sunrise.value)-this.localTime < 0 ? 'day' : 'night';
            resolve(resp);
          }).catch(err => {
            console.log(err);
            reject(err);
          })
      }))
    }
  },
  data(){
    return {
      location : {},
      wdata:{},
      localTime:{},
      shift:"day"
    }
  }
}
</script>

<style lang="css" scoped>
</style>
