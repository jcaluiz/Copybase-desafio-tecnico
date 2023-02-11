<template>
  <main id="home-container">
    <h1>Pokemon</h1>
    <section id="pokemons-home-container">
      <PokemonComponent />
    </section>
  </main>
</template>

<script>
import PokemonComponent from "../../components/PokemonComponent/PokemonComponent.vue";

export default {
  name: "HomeView",
  data() {
    return {
      getPokemons: [],
    };
  },
  components: {
    PokemonComponent,
  },
};
</script>

<style src="./HomeView.scss" lang="scss" scoped></style>
