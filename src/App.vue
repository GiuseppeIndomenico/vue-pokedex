<template>
  <div>
    <MainComponent></MainComponent>

  </div>
</template>

<script>

import MainComponent from './components/MainComponent.vue';
import { store } from './data/store';
export default {
  store,
  components: { MainComponent }
}
</script>

<style lang="scss" scoped></style>