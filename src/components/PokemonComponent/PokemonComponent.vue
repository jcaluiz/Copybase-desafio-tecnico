<template>
  <div :class="classClickName" @click="handleClick">
    <div :class="`img-${classClickName}`">
      <img :src="imagePokemon" :alt="`Imagem de ${namePokemon}`" />
    </div>
    <h3>{{ namePokemon }}</h3>
    <ul v-if="morePokemonInformation" class="list-features-pokemon-container">
      <li v-for="(eachFeature, index) in feature" :key="index">
        <span>{{ eachFeature.nameFeature }}</span>
        <span>{{ eachFeature.value }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "PokemonComponent",
  emits: ["clicked"],
  props: {
    namePokemon: String,
    imagePokemon: String,
    feature: Array,
  },
  data() {
    return {
      morePokemonInformation: false,
      classClickName: "pokemon-container",
    };
  },
  methods: {
    handleClick() {
      this.$emit("clicked");
      this.morePokemonInformation = !this.morePokemonInformation;
      if (this.morePokemonInformation) {
        console.log(`handleClick: ${this.morePokemonInformation}`);
        this.classClickName = "pokemon-container-clicked";
      } else {
        this.classClickName = "pokemon-container";
      }
    },
  },
};
</script>

<style src="./PokemonComponent.scss" lang="scss" scoped></style>
