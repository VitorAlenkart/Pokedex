<template>
  <div id="app">
    <div class="jumbotron">
      <h1 class="display-4">Olá, mundo!</h1>
      <p class="lead">Este é um simples componente jumbotron para chamar mais atenção a um determinado conteúdo ou informação.</p>
      <hr class="my-4">
      <p>Ele usa classes utilitárias para tipografia e espaçamento de conteúdo, dentro do maior container.</p>
      <a class="btn btn-primary btn-lg" href="#" role="button">Leia mais</a>
    </div><br>
    
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
  
}

@font-face {
  font-family: 'Pokemon';
  src: url('../font/Pokemon\ Solid.ttf') format('truetype');
}

#btmbuscar {
  float: left;
}

.jumbotron{
  font-family: Pokemon;
}

#inputBuscar {
  max-width: 60%;
}


</style>
