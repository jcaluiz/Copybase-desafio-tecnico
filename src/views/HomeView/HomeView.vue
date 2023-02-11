<template>
  <main :id="idMain">
    <HeaderComponent />
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
    <FooterComponent />
  </main>
</template>

<script>
import axios from "axios";
import PokemonComponent from "../../components/PokemonComponent/PokemonComponent.vue";
import HeaderComponent from "../../components/Header/HeaderComponent.vue";
import FooterComponent from "../../components/Footer/FooterComponent.vue";

export default {
  name: "HomeView",
  data() {
    return {
      getPokemons: [], // Estado que renderiza os pokemons
      staticPokemons: [], // Estado estático de pokemons
      value: "", // É o estado que recebe o evento onChange do input
      morePokemonInformation: false, // Estado que gerencia a renderização das características
      idMain: "home-container", // Muda o nome da classe para criar um efeito contraste ao clicar num card de pokemon
      timeClicked: 0, // Estado que auxilia no efeito de contraste
    };
  },
  components: {
    PokemonComponent, // Componente que cria os cards dos pokemons
    HeaderComponent, // Componente de cabeçalho da aplicação
    FooterComponent, // Componente de rodapé da aplicação
  },
  methods: {
    formatPokemonsObjectInArray(pokemonList) {
      // Recebe a primeira requisição do primeiro link no parâmetro
      pokemonList.map(
        (
          { name, url } // desestrutura o objeto que recebe nome e url
        ) =>
          axios(url) // faz a requisição da url relacionada a cada pokemon
            .then((response) => {
              const { sprites, stats } = response.data; // Desestrutura dois objetos da requisição
              const pokemons = [
                ...this.getPokemons, // Desestrutura o array
                {
                  name,
                  image: sprites["back_default"],
                  feature: stats.map((item) => ({
                    nameFeature: item.stat.name,
                    value: item["base_stat"],
                  })), // Cria um objeto personalizado para ser mais performático na renderização
                },
              ];
              this.getPokemons = pokemons; // Recebe o array de objeto no estado que renderiza os cards
              this.staticPokemons = pokemons; // Recebe o array de objeto no estado estático
            })
            .catch((error) => console.log(error)) // Recebe o erro caso aconteça um
      );
    },
    fetchPokemonsInformations() {
      axios
        .get("https://pokeapi.co/api/v2/pokemon/")
        .then((response) => {
          const { results } = response.data;
          this.formatPokemonsObjectInArray(results); // Faz a primeira requisição da url para ser enviado para o método acima
        })
        .catch((error) => console.log(error)); // Recebe o erro caso aconteça um
    },
    clicked() {
      // É o emit para quando for feito o clique na outra página, ter a possibilidade de usar no componente pai
      if (this.timeClicked === 0) {
        // Quando renderizado, inicia esse estado com 0 para ser possível trocar a classe
        this.timeClicked = 1; // Passa para o estado o 1 para que no próximo clique não cair nessa condicional
        return (this.idMain = "home-container-dark"); // Retorna para que a função encerre nessa linha e não continua na função
      }
      this.timeClicked = 0; // Passa para o estado o 0 para que no próximo clique cair nessa condicional
      this.idMain = "home-container"; // Passa para a classe o seu nome original
    },
  },
  mounted() {
    this.fetchPokemonsInformations(); // Faz a requisição a API quando a página for montada
  },
  beforeUpdate() {
    this.getPokemons = this.staticPokemons.filter(
      ({ name }) => name.includes(this.value) // Quando for digitado no input, seu valor faz um filtro nas listas
    ); // getPokemons é o estado que renderiza os cards e ele recebe o filtro dos pokemons que estão no estado estático
  },
};
</script>

<style src="./HomeView.scss" lang="scss"></style>
