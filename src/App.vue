<template>
  <div id="app">
    <div class="jumbotron" style="background-color: rgba(255,0,0,0.1); width:100%; ">
      <hr>
      <h1 class="display-4 " style="margin-left: 10px;">Olá, mundo!</h1> <br>
      <h2 class="lead" style="margin-left: 20px;">Este é um simples componente jumbotron para chamar mais atenção a um determinado conteúdo ou informação.</h2>
      <br class="my-4">
      <h2 class="lead" style="margin-left: 20px;">Ele usa classes utilitárias para tipografia e espaçamento de conteúdo, dentro do maior container.</h2>
      <br class="my-4">
      <a class="btn btn-primary btn-lg" style="margin-left: 20px;" href="https://github.com/VitorAlenkart" role="button">Leia mais</a>
      <hr>
    </div><br>

    <input class="input is-rounded" id="inputBuscar" type="text" placeholder="Buscar pokemon..." v-model="busca"> 
    <button v-on:click="filtrarPokemons" class="button is-primary is-rounded" id="btnBuscar">Buscar</button>

    <div v-for="(poke,index) in pokemonsFiltrados"  :key="poke.name" id="pokemons">
    <Pokemon :index="index +1" :pokemon="(capitalize(poke.name))" :url="poke.url" />
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
  margin-top: 2%;

}

@font-face {
  font-family: 'Pokemon';
  src: url('../font/Pokemon\ Hollow.ttf') format('truetype');
}

#pokemons {
  display: inline-block;
  margin-left: 3%;
  margin-top: 3%;

}

#inputBuscar {
  max-width: 90%;
  
}


</style>
