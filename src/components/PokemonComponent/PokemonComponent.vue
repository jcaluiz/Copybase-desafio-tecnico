<template>
  <div class="pokemon-container">
    <img :src="imagePokemon" alt="`Imagem de ${namePokemon}`" />
    <p>{{ namePokemon }}</p>
    <!-- <p>{{ feature[0].value }}</p> -->
  </div>
</template>

<script>
export default {
  name: "PokemonComponent",
  props: {
    namePokemon: String,
    imagePokemon: String,
    // feature: Array,
  },
};
</script>

<style src="./PokemonComponent.scss" lang="scss" scoped></style>
