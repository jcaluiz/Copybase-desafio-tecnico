<template>
  <main id="home-container">
    <h1>Pokemon</h1>
    <input type="text" v-model="value" />
    <section id="pokemons-home-container">
      <PokemonComponent
        v-for="(pokemon, index) in getPokemons"
        :key="index"
        :namePokemon="pokemon.name"
        :imagePokemon="pokemon.image"
        :feature="pokemon.feature"
      />
    </section>
  </main>
</template>

<script>
import axios from "axios";
import PokemonComponent from "../../components/PokemonComponent/PokemonComponent.vue";

export default {
  name: "HomeView",
  data() {
    return {
      getPokemons: [],
      staticPokemons: [],
      value: "",
    };
  },
  components: {
    PokemonComponent,
  },
  methods: {
    formatPokemonsObjectInArray(pokemonList) {
      const backDefault = "back_default";
      pokemonList.map(({ name, url }) =>
        axios(url)
          .then((response) => {
            const { sprites, stats } = response.data;
            const pokemons = [
              ...this.getPokemons,
              {
                name,
                image: sprites[backDefault],
                feature: stats.map((item) => ({
                  nameFeature: item.stat.name,
                  value: item["base_stat"],
                })),
              },
            ];
            this.getPokemons = pokemons;
            this.staticPokemons = pokemons;
          })
          .catch((error) => console.log(error))
      );
    },
    fetchPokemonsInformations() {
      axios
        .get("https://pokeapi.co/api/v2/pokemon/")
        .then((response) => {
          const { results } = response.data;
          this.formatPokemonsObjectInArray(results);
        })
        .catch((error) => console.log(error));
    },
  },
  mounted() {
    this.fetchPokemonsInformations();
  },
  beforeUpdate() {
    this.getPokemons = this.staticPokemons.filter(({ name }) =>
      name.includes(this.value)
    );
  },
};
</script>

<style src="./HomeView.scss" lang="scss" scoped></style>
