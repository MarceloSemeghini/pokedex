<template>
    <div class="list-box" v-for='i in pokemons.length/6' :key='i'>
        <div v-for="pokemon in pokemons.slice(itemPerRow*(i-1), itemPerRow*(i))" :key="pokemon.name">
            <PokemonBox  :pokemon="pokemon"/>
        </div>
    </div>

    <button v-if="offset > 0" @click="previousPage" class="left"> 
        <img src="http://cdn.onlinewebfonts.com/svg/img_266597.png" alt="right">  
    </button>
    <button @click="nextPage" class="right"> 
        <img src="http://cdn.onlinewebfonts.com/svg/img_266597.png" alt="right">    
    </button>
</template>

<script>
import PokemonBox from './PokemonBox.vue'

export default{
    components: {
        PokemonBox
    },
    data() {
        return{
            itemPerRow: 6,
            offset: 0,
            pokemons: []
        }
    },
    mounted() {
        this.loadPokemons()
    },
    methods: {
        async loadPokemons(){
            await fetch(`https://pokeapi.co/api/v2/pokemon?limit=24&offset=${this.offset}`)
             .then(res => res.json())
             .then(data => this.pokemons = data.results)
             .catch(error => console.log(error.message))
        },
        previousPage(){
            this.offset -= 24
            this.loadPokemons()
        },
        nextPage(){
            this.offset += 24
            this.loadPokemons()
        }
    },
}
</script>
