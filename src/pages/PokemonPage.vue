<template>

    <h1 v-if="!pokemon">Buscando en la pokedex...</h1>
    <div v-else>
        <h1>Quién es ese Pokemón?</h1>
        <div class="container-poke">
            <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon"/>
            <PokemonOptions :pokemons="pokemonsArr" @selection="checkAnswer"/>        
        
        <template v-if="showAnswer">
            <h2 class="fade-in">{{ message }}</h2>
            <button @click="newGame">Nuevo Juego</button>
        </template>
        </div>
    </div>

</template>

<script>
import PokemonPicture from '@/components/PokemonPicture.vue'
import PokemonOptions from '@/components/PokemonOptions.vue'
import getPokemonOptions from '@/helpers/getPokemonOptions'

//TODO: añadir sonido hover a opciones
//TODO: añadir componente Share

export default {
    name: 'PokemonPage',
    components: { PokemonPicture, PokemonOptions },
    data(){
        return {
            pokemonsArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: ''
        }
    },
    methods: {
        async mixPokemonArray(){
                this.pokemonsArr = await getPokemonOptions()
                const rndInt = Math.floor(Math.random() * 4)
                this.pokemon = this.pokemonsArr[rndInt]
        },
        checkAnswer(selectedId){
            this.showPokemon = true
            this.showAnswer = true
            if(this.pokemon.id == selectedId){
                this.message = `Correcto! ${this.pokemon.name}`
            } else{
                this.message = `Oops, era ${this.pokemon.name}`
            }
        },
        newGame(){
            
            this.showPokemon= false
            this.showAnswer= false
            this.pokemon = null
            this.pokemonsArr= []
            this.mixPokemonArray()

        }
    },
    mounted(){
        this.mixPokemonArray()
    }

}
</script>

<style scoped>
h1,h2{
    color: white;
}
button {
    background-color: white;
    font-weight: bold;
    padding-top: 10px;
    padding-bottom: 10px; 
    border-radius: 5px;
    border: 1px solid rgba(0, 0, 0, 0.2);
    cursor: pointer;
    margin-bottom: 10px;
    width: 250px;
}
</style>
