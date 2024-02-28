<template>
    <div class="container d-flex align-items-center justify-content-center pt-5 ">
        <div class="pokedex shadow">


            <SearchComponent></SearchComponent>
        </div>
    </div>
</template>

<script>

import { store } from '../data/store';
import SearchComponent from './SearchComponent.vue';
export default {
    store,
    components: { SearchComponent }
}
</script>

<style lang="scss" scoped>
.pokedex {
    background-color: #c00a0b;
    border: 15px solid #8b0001;
    border-radius: 3px;
    width: 100%;
    height: 600px;

}
</style>