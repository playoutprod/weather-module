<template lang="html">
  <div class="search">
    <form>
      <input type="text"/>
    </form>
  </div>
</template>

<script>

import places from 'places.js';

export default {
  name:"Search",
  mounted(){
    this.instance = places({
      type: 'city',
      container: this.$el.querySelector('input'),
    });
    this.instance.on('change', responce => {
      this.$emit('change', responce.suggestion);
    });
  },
  methods:{

  }
}
</script>

<style lang="css" scoped>
</style>


/*
searchLocation(search){
  axios.get(config.location_endpoint+'cities/autocomplete?apikey='+config.apikey+'&q='+search).then(resp => {
    this.location = resp.data;
  }).catch(err => {console.log(err)})
},
getLocation(locationID){
  return(
    new Promise((resolve,reject)=>{
    axios.get(config.location_endpoint+'?apikey='+config.apikey+'&loc='+locationID).then(resp => {
      this.location = resp.data;
      resolve(resp);
    }).catch(err => {
      console.log(err);
      reject(err);
    })
  }))
}
*/
