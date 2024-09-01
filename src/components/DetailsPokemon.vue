<template>

  <div class="card">

    <div class="media">
        <div class="media-left">
          <figure class="image">
            <div class="oneType" v-if="type[1] !== undefined">
              <img
              :src="type[0].url"
              alt="Placeholder image"
              />
              <img
              :src="type[1].url"
              alt="Placeholder image"
              />
            </div>
            <div class="twoTypes" v-else-if="type[0] !== undefined">
              <img
              :src="type[0].url"
              alt="Placeholder image"
              />
            </div>
            
          </figure>
        </div>
        
      </div>
    <div class="card-image">
      <figure class="image">
        <img
          :src="sprite"
          alt="Placeholder image"
        />
      </figure>
    </div>
    <div class="card-content">
      

      <div class="content">
        
        <h1>{{ pokemon }}</h1>
        
        <div id="cardtext" class="card-body text-center">
        <p class="card-text " v-if="type[1] !== undefined">{{capitalize(type[1].name)+ ' e ' +capitalize(type[0].name)}}</p>
        <p class="card-text " v-else-if="type[0] !== undefined">{{capitalize(this.type[0].name)}}</p>            
        </div>

        <br />
        <a href="#" class="btn btn-primary" id="btn"  v-on:click.prevent="changeSprite">Trocar de Lado</a>
      </div>
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
            type: []
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
          if(this.sprite === this.spriteBack){
            this.sprite = this.spriteFront;
          }else{
            this.sprite = this.spriteBack;
          }
        }
        
      }
    },
    created () {
        axios.get(this.url).then(d => {
          this.sprite = d.data.sprites.other.showdown.front_default;

          this.spriteFront = d.data.sprites.other.showdown.front_default;
          this.spriteBack = d.data.sprites.other.showdown.back_default;
          
          for(let i = 0; i < d.data.types.length; i++){
            let urlImg = "";
            axios.get(d.data.types[i].type.url).then(dUrl => {
              urlImg = dUrl.data.sprites['generation-vii']['lets-go-pikachu-lets-go-eevee'].name_icon;
              this.type.push({name: d.data.types[i].type.name,
                url:urlImg})
            })
            
          }
        });
    },
}
</script>

<style >

.card {

  margin-left: 4%;
  flex: 1 0 21%; /* Ajusta a largura dos elementos */
  margin: 10px;  /* Ajusta o espaço entre os elementos */
  box-sizing: border-box;
}

.media {
  padding: 30px;
  display:inline-block;
  justify-items: center;
  align-items: center;
  
}

#btn {
  display: flex;
  justify-items: center;
  align-self: center;
  position: relative;
  bottom: 1%;

}
.card-image{
  width: 115px; /* ou qualquer valor que você preferir */
  height: auto; /* mantém a proporção da imagem */
  margin: 0 auto;
}

</style>