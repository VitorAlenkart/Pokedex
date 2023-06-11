<template>
    
  <div class="card" style="width: 18rem;">

    <img class="card-img-top" :src="sprite" alt="Imagem de capa do card">
    <div class="card-body">

      <h5 class="card-title">{{ pokemon }}</h5>

      <div id="cardtext" class="card-body">
        <h7 class="card-text " v-if="type.type2 !== false">{{capitalize(type.type1)+ ' e ' +capitalize(type.type2)}}</h7>
        <h7 class="card-text " v-else>{{capitalize(this.type.type1)}}</h7>            
      </div>
          
      <a href="#" class="btn btn-primary" v-on:click="changeSprite">Trocar de Lado</a>

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
            spriteSide: '',
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
          if(this.spriteSide === true){
            this.spriteSide = false;
            this.sprite = this.spriteBack;
          }else{
            this.sprite = this.spriteFront;
            this.spriteSide = true;
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

#divPrinc {

    margin-top: 2%;
    margin: 0 auto;
   
}



#p1 {
  display: inline;
}
#p2 {
  display: inline;

}


</style>