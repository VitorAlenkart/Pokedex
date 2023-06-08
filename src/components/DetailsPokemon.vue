<template>
    <div id="divPrinc">
        <div class="card " id="card">
            <div class="card-image ">
              <figure class="image is-centered is-120x120 ">
                <img :src="sprite">
              
              </figure>
            </div>
            <div class="card-content">
              <div class="media">
                <div class="media-left">
                  <h2>{{index}}</h2>
                </div>
                <div class="media-content">

                  <p class="title is-4">{{ pokemon }}</p>
                  
                  <p class="subtitle is-6" id="p1">{{capitalize(this.type.type1) + '   '}}</p>
                  <p class="subtitle is-6" id="p2" >{{capitalize(this.type.type2)}}</p>
      
                </div>
                
              </div>
          
              <div class="content">
                <button class="button is-primary is-normal" v-on:click="changeSprite">Trocar lado!</button>
              </div>
            </div>
          </div>
        <input v-bind:hidden="url">
    </div>
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
              type1: d.data.types[0].type.name
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

#card {

  height: auto;
  width: 250px;
  margin-left: 30px;
  margin-top: 30px;
  float: left;

}

#p1 {
  display: inline;
}
#p2 {
  display: inline;

}

</style>