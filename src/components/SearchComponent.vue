<template>
    <div class="m-2 h-100">
        <input class="pixel mb-3 display-color" type="text" v-model="pokemonName" placeholder="Cerca Pokémon"
            @keyup.enter="searchPokemon">
        <div @click="searchPokemon" class=" btn btn-warning">Cerca</div>

        <div class="poke-display-img mb-3">
            <div class=" d-flex flex-column align-items-center w-100" v-if="pokemonData">
                <h2 class="text-center pt-2">{{ pokemonData.name }}</h2>
                <img class="img-fluid" :src="pokemonData.sprites.front_default" :alt="pokemonData.name">
            </div>

        </div>

        <div class="pixel w-100 display-color p-2 name-poke mb-2">
            <div class="p-name" v-for="(pokemon, index) in filteredPokemonList" :key="index"
                @click="selectPokemon(pokemon)">
                {{ pokemon.name }}
            </div>
        </div>
    </div>
</template>
  
<script>

import { ref, computed } from 'vue';

import { store } from "../data/store";

export default {
    setup() {

        const pokemonName = ref('');
        const pokemonData = computed(() => store.pokemonData);
        const allPokemon = computed(() => store.allPokemon);
        const filteredPokemonList = computed(() => store.filteredPokemonList);

        const filterPokemonList = () => {
            if (!pokemonName.value.trim()) {
                store.filteredPokemonList = [];
                return;
            }
            store.filteredPokemonList = allPokemon.value.filter(pokemon => pokemon.name.includes(pokemonName.value.toLowerCase()));
        };

        const searchPokemon = async () => {
            store.pokemonData = null
            await filterPokemonList();
        };

        const selectPokemon = async (pokemon) => {
            try {
                const response = await fetch(pokemon.url);
                const data = await response.json();
                store.pokemonData = data;
            } catch (error) {
                console.error('Errore nel recupero delle informazioni del Pokémon:', error);
            }
        };

        const fetchAllPokemon = async () => {
            try {
                const response = await fetch(store.Url.base);
                const data = await response.json();
                store.allPokemon = data.results;
            } catch (error) {
                console.error('Errore nel recupero dei Pokémon:', error);
            }
        };

        fetchAllPokemon();
        return {
            pokemonName,
            pokemonData,
            filteredPokemonList,
            searchPokemon,
            selectPokemon
        };
    }
};
</script>
  
<style lang="scss" scoped>
::placeholder {
    color: black;
}

.poke-display-img {
    width: 80%;
    height: 250px;
    border: 5px solid grey;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: auto;


}

.name-poke {
    height: 300px;
    overflow: auto;

    &::-webkit-scrollbar {
        width: 5px;
        border-radius: 100px;
        border: 1px solid black;
    }

    &::-webkit-scrollbar-thumb {
        background-color: black;
        border-radius: 100px;
    }
}
</style>