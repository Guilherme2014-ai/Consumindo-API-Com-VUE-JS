<template>
  <div id="app">

    <audio src="./assets/main_music.mp3" autoplay hidden></audio>

    <br><a href="/"><img alt="Vue logo" src="https://i2.wp.com/multarte.com.br/wp-content/uploads/2019/03/pokemon-png-logo.png?fit=2000%2C736&ssl=1" width="600"></a><br><br>
    <br>
    <input class="input is-rounded" type="text" placeholder="Procure um Pokemon" id="search" name="search" v-model="input_search" @keypress="searchResults($event)"><br><br><br><br>

    
    <div v-for="(pokemon,index) in filter_pokemons" :key="pokemon.url" class="columns">
      <pokemon :nameP="pokemon.name" :url="pokemon.url" :num="index"/><br><br>
    </div><br><br>



  </div>
</template>

<script>
import axios from "axios"
import pokemon from "./components/pokemon.vue"

export default {
  name: 'App',
  data(){
    return {
      pokemons: [],
      filter_pokemons: [],
      input_search: ""
    }
  },
  components: {
    pokemon
  },
  methods: {
    searchResults: function(event){
      if(this.input_search != "" || this.input_search.length > 0){
        if(event.key == "Enter"){
          this.filter_pokemons = this.filter_pokemons.filter(a => a.name == this.input_search.toLowerCase())
        }
        return
      }
      this.filter_pokemons = this.pokemons
    }
  },

  // Functions de alta prioridades, sao executadas logo quando a pagina e carregada
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=0&offset=200").then(data => {
      let pokes = data.data.results

      this.pokemons = pokes
      this.filter_pokemons = pokes
    }).catch(err => {console.error(err)})
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body{
  background: -webkit-radial-gradient(rgb(156, 134, 134),rgb(162, 255, 193),rgb(255, 155, 155));
}
</style>