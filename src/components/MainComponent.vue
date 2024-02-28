<template>
    <div>
        <SearchComponent></SearchComponent>
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

<style lang="scss" scoped></style>