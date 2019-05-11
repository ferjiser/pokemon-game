<template>
  <div id="app">
     <h1>
            El juego de las parejas
        </h1>
        <form class="game-options" v-for="(url, key) in urls" :key="key">
            <label>
                <input type="radio" :value="key" v-model="selectedDeck" @change="getCards"> {{key}}
            </label>
        </form>
        <p>
            Intentos: 1
        </p>
        <ul class="pokemons-wrapper four-items">
            <li class="pokemon-card" v-for="(card,index) in cards" :key="index">
                <img  :src="card.image">
                <img  class="cover-image" src="https://via.placeholder.com/160x195/30d9c4/ffffff/?text=?" @click="turnOver(index)">
            </li>
        </ul>
  </div>
</template>

<script>

const urls = {
   "4": "https://api.jsonbin.io/b/5cd2137a4c004c0eb4950d03",
   "6": "https://api.jsonbin.io/b/5cd213c964d4fc359ead3a5a",
   "8": "https://api.jsonbin.io/b/5cd213f6c07f283511e1c251"
}

export default {
  name: 'app',
  components: {},
  data(){
    return {
        selectedDeck: 4,
        cards: [],
        pairedCards: [],
        selectedCards: []
    }
  },
  watch: {
      selectedDeck: {
          inmediate: true,
          handler(){
            fetch(urls[this.selectedDeck])
                    .then(response => response.json())
                    .then(cards => this.cards = cards
                )
          }
      }
  },
   methods: {
       selectedCard(card){
           this.selectedCards.push(card)
       }
    }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Roboto:400,700');
 
        body {
            font-family: 'Roboto', sans-serif;
            display: grid;
            grid-column: 1fr;
            grid-row: 2fr 4fr;
            text-align: center;
        }
 
        .game-options {
            padding: 30px;
        }
 
        .pokemons-wrapper {
            display: grid;
            grid-gap: 10px;
            grid-area: auto;
            padding: 0;
            margin: auto;
        }
 
        .four-items {
            grid-template-columns: 1fr 1fr 1fr 1fr;
            grid-template-rows: 1fr;
        }
 
        .six-items {
            grid-template-columns: 1fr 1fr 1fr;
            grid-template-rows: 1fr 1fr;
        }
 
        .eight-items {
            grid-template-columns: 1fr 1fr 1fr 1fr;
            grid-template-rows: 1fr 1fr;
        }
 
        .pokemon-card {
            list-style: none;
            border-radius: 5px;
            width: 100px;
            height: 150px;
            border: 1px solid #d0cfcf;
            position: relative;
        }
 
        .cover-image {
            z-index: 999;
            width: 100px;
            height: 150px;
            border: 1px solid #d0cfcf;
            border-radius: 5px;
            position: absolute;
            top: 0;
            left: 0;
            display: none
        }
 
        .pokemon-card.covered .cover-image {
            display: inherit;
        }
</style>
