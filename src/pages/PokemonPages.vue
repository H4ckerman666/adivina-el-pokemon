<template>
<h1 v-if="!pokemon">Espere por favor .......</h1>
<div v-else>
    <h1>Quien es este pokemon</h1>
<PokemonPicture :pokemonId="pokemon.id"  :showPokemon="showPokemon"/>
<PokemonOptions :pokemons="pokemonArr" @selection="checkAnswer"/>
<template v-if="showAnswer" class="fade-in">
<h1>{{winner}}</h1>
<button @click="newGame">Nuevo Juego</button>
</template>

</div>

</template>

<script>

import PokemonOptions from "@/components/PokemonOptions"
import PokemonPicture from "@/components/PokemonPicture.vue"
import getPokemonOptions from "@/helpers/getPokemonOptions"


export default {
  components: {
    PokemonOptions,PokemonPicture
  },
  data(){
    return {
        pokemonArr: [],
        pokemon : null,
        showPokemon: false,
        winner: '',
        showAnswer : false,
    }
  },
  methods: {
    async mixPokemonsArray(){
        this.pokemonArr = await getPokemonOptions()
        console.log(this.pokemonArr);
        const rnInt = Math.floor( Math.random() * 4)
        this.pokemon = this.pokemonArr[rnInt]
    },
    checkAnswer(pokemonId){
        this.showPokemon = true;
        this.showAnswer = true;
        console.log(this.pokemon.id);
        console.log(pokemonId);
        
        this.winner = (this.pokemon.id === pokemonId) ? `Correcto!! es ${this.pokemon.name}` : `Perdiste era ${this.pokemon.name}`;
        
    },
    newGame(){
        console.log('si enttramosn');
        
        this.pokemonArr= [];
        this.pokemon = null;
        this.showPokemon= false;
        this.winner= '';
        this.showAnswer = false;
        this.mixPokemonsArray()
    }
  },
  mounted(){
    this.mixPokemonsArray()
  }
}
</script>

<style>

</style>