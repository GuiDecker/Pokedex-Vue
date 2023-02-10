<template>
  <v-app>
    <v-conteiner>
      <v-card>
        <v-container>
          <v-text-field
            label="Search Pokemon"
            aria-placeholder="Search Pokemon"
            solo
          ></v-text-field>

          <v-row>
            <v-col
              cols="4"
              v-for="pokemon in pokemons.slice(0, 10)"
              :key="pokemon.name"
            >
              <v-card>
                <v-container>
                  {{ id_pokemon(pokemon) }}
                  <v-row class="mx-0 d-flexd-flex justify-center">
                    <img
                      :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${id_pokemon(
                        pokemon
                      )}.png`"
                      :alt="pokemon.name"
                      width="100%"
                    />
                    <h2 class="text-center">
                      {{ name_pokemon(pokemon) }}
                    </h2>
                  </v-row>
                </v-container>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
      </v-card>
    </v-conteiner>
  </v-app>
</template>

<script>
import axios from "axios"
// import { response } from "express"

export default {
  name: "App",

  components: {},

  data: () => {
    return {
      pokemons: [],
    }
  },

  mounted() {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=143")
      .then((response) => {
        this.pokemons = response.data.results
      })
    console.log("executed")
  },
  methods: {
    id_pokemon(pokemon) {
      return pokemon.url.split("/")[6]
    },

    name_pokemon(pokemon) {
      return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1)
    },
  },
}
</script>

<style>
#app {
  padding: 100px;
  color: white;
  background: linear-gradient(
      to bottom,
      rgba(10, 10, 10, 1) 0.2%,
      rgb(255, 238, 0),
      rgb(255, 238, 1)
    )
    no-repeat center center fixed !important;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover !important;
  background-position: center;
  min-height: 100vh;
}
</style>
