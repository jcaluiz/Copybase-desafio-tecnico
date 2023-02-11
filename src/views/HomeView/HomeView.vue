<template>
  <main :id="idMain">
    <h1>Pokemon</h1>
    <input
      type="text"
      v-model="value"
      class="search-input"
      placeholder="Pesquise pelo nome"
    />
    <section id="pokemons-home-container">
      <PokemonComponent
        v-for="(pokemon, index) in getPokemons"
        :key="index"
        :namePokemon="pokemon.name"
        :imagePokemon="pokemon.image"
        :feature="pokemon.feature"
        :moreInformation="morePokemonInformation"
        @clicked="clicked"
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
      morePokemonInformation: false,
      idMain: "home-container",
      timeClicked: 0,
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
    clicked() {
      if (this.timeClicked === 0) {
        this.timeClicked = 1;
        return (this.idMain = "home-container-dark");
      }
      this.timeClicked = 0;
      this.idMain = "home-container";
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

<style src="./HomeView.scss" lang="scss"></style>
