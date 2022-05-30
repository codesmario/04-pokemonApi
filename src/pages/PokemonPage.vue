<template>
  <h1 v-if="!pokemon">Espere por favor ...</h1>
  <div v-else>
    <h1>¿Quién es este pokémon?</h1>
    <!--  -->
    <PokemonPicture 
      :pokemonId="pokemon.id" 
      :showPokemon="showPokemon"/>

    <PokemonOptions 
      :pokemons="pokemonArr" 
      @selection="checkAnswer"/>
    <!--  -->
    <dir v-if="showAnswer">
      <h2>{{message}}</h2>
      <button @click="newGame">
        Nuevo juego
      </button>
    </dir>

  </div>
</template>

<script>
import PokemonPicture from "@/components/PokemonPicture"
import PokemonOptions from "@/components/PokemonOptions"

import getPokemonOptions from "@/helpers/getPokemonOptions"

// console.log(getPokemonOptions());

export default {
    name: 'PokemonPage',
    components: { PokemonPicture, PokemonOptions },
    data() {
      return {
        pokemonArr: [],
        pokemon:null,
        showPokemon:false,
        showAnswer:false,
        message:''
      }
    },
    methods: {
      async mixPokemonArray() {
        this.pokemonArr = await getPokemonOptions()
        const rndInt = Math.floor(Math.random() * 4)
        this.pokemon = this.pokemonArr[rndInt]
        console.log(this.pokemon);
      },
      checkAnswer (pokemonId) {    
        this.showAnswer = true    
        this.showPokemon = this.pokemon.id == pokemonId? true:false
        this.message = this.pokemon.id == pokemonId? `Correcto, ${this.pokemon.name}!`:`Oops, era ${this.pokemon.name}`
      },
      newGame () {
        this.showAnswer = false
        this.showPokemon = false
        this.pokemonArr = []
        this.pokemon = null
        this.mixPokemonArray()
      }
    },
    mounted() {
      this.mixPokemonArray()
    }

}
</script>