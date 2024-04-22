<template>
    <div class="m-2 h-100">
        <div class="input-group mb-3">
            <input class="form-control pixel display-color" type="text" v-model="pokemonName"
                placeholder="Cerca Pokémon" @keyup.enter="searchPokemon">
            <button @click="searchPokemon" class="btn btn-warning">Cerca</button>
        </div>


        <div class="poke-display-img mb-3 bg-dark">


            <div class="h-100 pb-2 pixel display-color" v-if="pokemonData">
                <h6 class="text-center fs-6 fw-bold">{{ pokemonData.name }}</h6>

                <img class="" :src="pokemonData.sprites.front_default" :alt="pokemonData.name">

            </div>
            <div v-else class="h-100 pb-2 pixel display-color d-flex align-items-center justify-content-center">

                <div class="fs-2">
                    Seleziona un Pokémon
                </div>
            </div>

        </div>

        <div class="pixel w-100 display-color p-2 name-poke mb-2">
            <div v-if="filteredPokemonList.length === 1 && filteredPokemonList[0].name === 'Nessun Pokémon trovato'">
                {{ filteredPokemonList[0].name }}
            </div>
            <div v-else>
                <div class="p-name" v-for="(pokemon, index) in filteredPokemonList" :key="index"
                    @click="selectPokemon(pokemon)">
                    {{ pokemon.name }}
                </div>
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
            store.pokemonData = null;
            await filterPokemonList();

            if (pokemonName.value.trim() && store.filteredPokemonList.length === 0) {
                store.filteredPokemonList = []; // Reset filtered list
                store.filteredPokemonList.push({ name: 'Nessun Pokémon trovato' });
            }
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
    border: 5px solid rgb(46, 46, 46);
    margin: auto;

    img {
        width: 100%;
        height: 100%;
        object-fit: contain;

    }
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

.input-group input:focus {
    background-color: #849c1c;
    box-shadow: 0 0 0 2px rgba(0, 0, 0, 0);

}

.input-group {
    user-select: none;
}
</style>