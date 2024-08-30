<template>
    
  <div class="card" style="width: 18rem;">

    <img class="card-img-top" :src="sprite" alt="Imagem de capa do card">
    <div class="card-body">

      <h5 class="card-title text-center">{{ pokemon }}</h5>

      <div id="cardtext" class="card-body text-center">
        <h6 class="card-text " v-if="type.type2 !== false">{{capitalize(type.type1)+ ' e ' +capitalize(type.type2)}}</h6>
        <h6 class="card-text " v-else>{{capitalize(this.type.type1)}}</h6>            
      </div>
          
      <a href="#" class="btn btn-primary" id="btn"  v-on:click.prevent="changeSprite">Trocar de Lado</a>

    </div>
  </div>

        
  <input v-bind:hidden="url">
    
</template>

<script>
import axios from 'axios';

export default {
    data () {
        return {
            sprite: '',
            spriteFront: '',
            spriteBack: '',
            spriteSide: 'front',
            type: {
              type1: '',
              type2: '',
            }
        }
    },
    props: {
        index: Number,
        pokemon: String,
        url: String,
    },
    
    methods: {
      capitalize: function (value) {
        if (!value) return ''
        return value.charAt(0).toUpperCase() + value.slice(1)
      },
      changeSprite: function(event) {
        if(event){
          if(this.spriteSide === "back"){
            this.sprite = this.spriteFront;
            this.spriteSide = "front";
          }else{
            this.spriteSide = "back";
            this.sprite = this.spriteBack;
          }
        }
        
      }
    },
    created () {
        axios.get(this.url).then(d => {
          this.spriteSide = true;
          this.sprite = d.data.sprites.front_default;

          this.spriteFront = d.data.sprites.front_default;
          this.spriteBack = d.data.sprites.back_default;
          
          if(d.data.types[1] != undefined){
            this.type = {
              type1: d.data.types[0].type.name,
              type2: d.data.types[1].type.name
            }
          }else{
            this.type = {
              type1: d.data.types[0].type.name,
              type2: false
            };
          }

        });
    },
}
</script>

<style >

.card {

  margin-left: 4%;
   
}

#btn {
 margin-left: 21%; 
}

</style>