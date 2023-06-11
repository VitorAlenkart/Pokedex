<template>

  <div id="app">
    <NavBar/> <br>

    <div class="alert alert-primary" role="alert">
      Um simples alerta primary com <a href="#" class="alert-link">um link de exemplo</a>. Clique nele, se quiser.
    </div>
    
    <section class="hero is-link is-small">
      <div class="hero-body">
        <p class="title">
          Pokedex
        </p>
        <p class="subtitle">
          Projeto feito por @VitorAlenkart
        </p>
      </div>
    </section><br>

    <input class="input is-rounded" id="inputBuscar" type="text" placeholder="Buscar pokemon..." v-model="busca"> <button v-on:click="filtrarPokemons" class="button is-primary is-rounded" id="btnBuscar">Buscar</button>

    <div v-for="(poke,index) in pokemonsFiltrados" :key="poke.name" id="pokemons">
    <Pokemon :index="index +1" :pokemon="(capitalize(poke.name))" :url="poke.url" />
    </div>
  </div>

</template>

<script>
import axios from 'axios';
import Pokemon from './components/DetailsPokemon.vue'
import NavBar from './components/NavBar'

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
    Pokemon,
    NavBar
  },
  created () {
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0').then(d => {
      
      this.pokemons = (d.data.results);
      this.pokemonsFiltrados = this.pokemons;
    });
    
  },
  methods: {
    capitalize: function (value) {
      if (!value) return ''
      return value.charAt(0).toUpperCase() + value.slice(1)
    },
    filtrarPokemons: function(event){
      if(event){
        if(this.busca === '' || this.busca === ' '){
          this.pokemonsFiltrados = this.pokemons;
        }else{
          this.pokemonsFiltrados = this.pokemons.filter(p => p.name === this.busca || p.name[0].toUpperCase() + p.name.slice(1).toLowerCase() === this.busca);
        }
      }
    }
  },
  computed: {
    // resultadoFiltrado: function(){
    //   if(this.busca === '' || this.busca === ' '){
    //     return this.pokemons
    //   }else{
    //     return this.pokemons.filter(p => p.name === this.busca || p.name[0].toUpperCase() + p.name.slice(1).toLowerCase() === this.busca);
    //   }
    // }
  },
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

#btmbuscar {
  float: left;
}

#inputBuscar {
  max-width: 60%;
}


</style>
