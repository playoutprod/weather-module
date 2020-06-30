//Places search form

<template lang="html">
  <div class="search">
    <form @submit="search">
      <input type="text"/>
    </form>
  </div>
</template>

<script>

import config from '../config/config.js'
import places from 'places.js';
import algoliasearch from 'algoliasearch';

//const searchClient = algoliasearch(config.places_appid, config.places_key);
const placesClient = algoliasearch.initPlaces(config.places_appid, config.places_key);

export default {
  name:"Search",
  data(){
    return({input : null})
  },
  mounted(){
     this.input = this.$el.querySelector('input')
    //Places autocomplete
    this.instance = places({
      type: 'city',
      container: this.input,
    });
    this.instance.on('change', responce => {
      this.input.blur();
      this.$emit('change', responce.suggestion);
    });

  },
  methods:{
    search(e){

      //On sumbit, usefull if autocomplete is very slow or have not result
      e.preventDefault();
      this.input.blur();
      placesClient.search({
        query: this.input.value,
        type: 'city',
        hitsPerPage: 50
      }).then(({hits}) =>{
        hits.sort((a,b)=>{
          //Sort cities by suburd (whole city should be first)
          if(a.is_suburb && !b.is_suburb){
            return(1)
          }else if(!a.is_suburb && b.is_suburb){
            return(-1)
          }else{
            return(0)
          }
        })
        //if there's results, send the first one
        if(hits.length>0){
          this.$emit('change', hits[0]);
        }else{
          this.$emit('change','notfound');
        }
      })
    }
  }
}
</script>

<style lang="css" scoped>
  .search{
    margin-bottom: 2em;
  }
  input{
    border-radius: 2em;
  }
</style>
