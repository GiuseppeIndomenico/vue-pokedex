<template>
    <div>
        <input class="m-2" type="text" v-model="pokemonName" placeholder="Cerca Pokémon" @keyup.enter="searchPokemon">
        <div @click="searchPokemon" class=" btn btn-warning">Cerca</div>
        <div>
            <div class="p-name" v-for="(pokemon, index) in filteredPokemonList" :key="index"
                @click="selectPokemon(pokemon)">
                {{ pokemon.name }}
            </div>
        </div>
        <div v-if="pokemonData">
            <h2>{{ pokemonData.name }}</h2>
            <img :src="pokemonData.sprites.front_default" :alt="pokemonData.name">
        </div>
    </div>
</template>
  
<script>

import { ref, computed } from 'vue'; // Importa ref e computed da Vue

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
.p-name {
    cursor: pointer;
}
</style>