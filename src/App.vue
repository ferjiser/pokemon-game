<template>
  <div id="app">
     <h1>
            El juego de las parejas
        </h1>
        <form class="game-options">
            <label v-for="(url, key) in urls" :key="key">
                <input type="radio" :value="key" v-model="selectedDeck"> {{key}}
            </label>
        </form>
       <p>Intentos: {{ count }}</p>
        <ul class="pokemons-wrapper four-items">
            <li class="pokemon-card" @click="selectCard(card)" :class="{covered: coveredCards.includes(card)}" v-for="card in cards" :key="card.image">
                <img :src="card.image">
                <img class="cover-image" src="https://via.placeholder.com/160x195/30d9c4/ffffff/?text=?">
            </li>
        </ul>
  </div>
</template>

<script>

export default {
  name: 'app',
  components: {},
  data(){
    return {
        urls: {
            "4": "https://api.jsonbin.io/b/5cd2137a4c004c0eb4950d03",
            "6": "https://api.jsonbin.io/b/5cd213c964d4fc359ead3a5a",
            "8": "https://api.jsonbin.io/b/5cd213f6c07f283511e1c251"
        },
        selectedDeck: 4,
        cards: [],
        pairedCards: [],
        selectedCards: [],
        count: 0
     }
    },
    computed: {
        uncoveredCards(){
            return [...this.pairedCards, ...this.selectedCards];
        },
        coveredCards() {
            return this.cards.filter(card => !this.uncoveredCards.includes(card));
        },
    },
    watch:{
        selectedDeck: {
            immediate: true,
            handler() {
                fetch(this.urls[this.selectedDeck])
                .then(res => res.json())
                .then(cards => {
                    this.cards = cards;
                })
            },
        }
    },
    methods: {
        selectCard(card) {
            this.selectedCards.push(card);
            if (this.selectedCards.length === 2) {
                this.count++;
                const [card1, card2] = this.selectedCards;
                if (card1.pair === card2.pair) {
                    this.pairedCards = this.pairedCards.concat(this.selectedCards);
                }
                setTimeout(() => {
                    this.selectedCards = [];
                }, 500);
            }
        }
    }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Roboto:400,700');

body {
    font-family: 'Roboto', sans-serif;
    text-align: center;
}

#app {
    display: grid;
    grid-column: 1fr;
    grid-row: 2fr 4fr;
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
    display: none;
}

.pokemon-card.covered .cover-image {
    display: inherit;
}
</style>
