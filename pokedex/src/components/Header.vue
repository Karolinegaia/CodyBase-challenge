<template>
  <div class="title">
    <img src="pokedex.png" alt="pokedex" />
    <h1>Pokedex</h1>
  </div>
   
</template>
<script>
import axios from "axios";

export default {
  name: "App",
  components: {},
  data() {
    return {
      pokemons: [],
      search: "",
      show_dialog: false,
      selected_pokemon: null,
    };
  },

  mounted() {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=100")
      .then((response) => {
        this.pokemons = response.data.results;
      });
  },
  methods: {
    get_id(pokemon) {
      return pokemon.url.split("/")[6];
    },
    get_name(pokemon) {
      return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
    },
  //   show_pokemon(id) {
  //     axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`).then((response) => {
  //       this.selected_pokemon = response.data;
  //       this.show_dialog = !this.show_dialog;
  //     });
  //   },
  // },
  computed: {
    filtered_pokemons() {
      return this.pokemons.filter((item) => {
        return item.name.includes(this.search);
      });
    },
  },
},
}
</script>

<style src="../style/HeaderStyle.sass" lang="sass" />
