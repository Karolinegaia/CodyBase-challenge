<template>
  <div class="container-card">
    <div class="card">      
      <div class="pokemons">
        <div class="row">
          <div
            class="col"
            v-for="pokemon in filtered_pokemons"
            :key="pokemon.name"
          >
            <Search/>
            <div class="card-name" @click="show_pokemon(get_id(pokemon))">
              <div class="conatiner-pokemon">
                {{ get_id(pokemon) }}
                <img
                  :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${get_id(
                    pokemon
                  )}.png`"
                  alt="Karoline-Gaia"
                />
                <h2 class="text-center">{{ get_name(pokemon) }}</h2>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <v-dialog v-model="show_dialog" width="800">
      <v-card v-if="selected_pokemon">
        <v-container>
          <v-row d-flex align-center>
            <v-col cols="8">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${selected_pokemon.id}.png`"
                alt="Karoline-Gaia"
              />
            </v-col>
            <v-col cols="8">
              <h1>{{ get_name(selected_pokemon) }}</h1>
              <v-chip
                label
                v-for="type in selected_pokemon.types"
                :key="type.slot"
                class="mr-2"
              >
                {{ type.type.name }}
              </v-chip>
              <v-divider class="my-4"></v-divider>
              <v-chip label>
                Altura {{ selected_pokemon.height * 2.54 }} cm
              </v-chip>
              <v-chip>
                Peso {{ selected_pokemon.weight * (0.45359237).toFixed(0) }} kg
              </v-chip>
            </v-col>
          </v-row>
          <h1>Moves</h1>
          
        </v-container>
      </v-card>
    </v-dialog>
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
      return Number(pokemon.url.split("/")[6]);
    },
    get_name(pokemon) {
      return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
    },
    show_pokemon(id) {
      axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`).then((response) => {
        this.selected_pokemon = response.data;
        this.show_dialog = !this.show_dialog;
      });
    },
   
  },
  computed: {
    filtered_pokemons() {
      return this.pokemons.filter((item) => {
        return item.name.includes(this.search);
      });
    },
  },
};
</script>

<style src="../style/CardStyle.sass" lang="sass" />
