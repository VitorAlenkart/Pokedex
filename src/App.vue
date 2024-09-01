<template>
  <div id="app">
  
    <div class="top">
      <img id="logo" src="./assets/logoPokedex.png" alt="a">
      <input v-on:keyup.prevent="filtrarPokemons" class="input is-rounded" id="inputBuscar" type="text" placeholder="Buscar pokemon..." v-model="busca"> 
      <button v-on:click="filtrarPokemons" class="button is-primary is-rounded" id="btnBuscar">Buscar</button>
    </div>
    
    <div class="body">
      <div  v-for="(poke,index) in pokemonsFiltrados"  :key="poke.name" id="pokemons">
        <Pokemon :index="index +1" :pokemon="(capitalize(poke.name))" :url="poke.url" />
      </div>
    </div>
    
  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/DetailsPokemon.vue'


export default {
  name: 'App',

  data() {
    return {
      pokemons: [],
      pokemonsFiltrados: [],
      busca: ''
    }
  },
  components: {
    Pokemon
  },
  created () {
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0').then(d => {
      
      this.pokemons = (d.data.results);
      this.pokemonsFiltrados = (d.data.results);
    });
    
  },
  methods: {
    capitalize: function (value) {
      if (!value) return ''
      return value.charAt(0).toUpperCase() + value.slice(1)
    },
    filtrarPokemons: async function(event){
      if(event){
        if(this.busca === '' || this.busca === ' '){
          this.pokemonsFiltrados = this.pokemons;
        }else{
          this.pokemonsFiltrados = []
          let l = this.pokemons.length;
          let b = this.busca.length
          for (let i = 0; i < l; i++) {
            let palavra = ""
            for (let j = 0; j < b; j++) {
              palavra += this.pokemons[i].name.at(j)
            }
            console.log(palavra)
            if(palavra == this.busca){
              this.pokemonsFiltrados.push(this.pokemons[i]);
            }
          }
        }
      }
    }
  },
 
}

</script>

<style>
.body {
  margin: 1% 1%;
  

}


@font-face {
  font-family: 'Pokemon';
  src: url('../font/Pokemon\ Hollow.ttf') format('truetype');
}

#pokemons {
  display: inline-block;
  margin: 0 auto;
  margin-left: 3cm;
  margin-top: 1cm;
  width: 250px; /* ou qualquer valor que você preferir */
  height: 600px;
}

#inputBuscar {
  max-width: 70%;
  text-align: center;
  margin-left: 2cm;
}

#btnBuscar {
  margin-left:10px;
}

#logo {
  height: 13%;
  width: 10%;
}

.top{
  display: flex;
  justify-content: center;
  align-items: center;
  height: 3%;
}

#app{
  margin-top: 1cm;
}

</style>
