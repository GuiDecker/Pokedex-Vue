<template>
  <div>
    <v-app class="v-app">
      <img class="logo" :src="require('../assets/logo-pokemon.png')" alt="" />
      <v-conteiner>
        <v-text-field
          v-model="search"
          label="Search Pokemon"
          aria-placeholder="Search Pokemon"
          solo
        ></v-text-field>
        {{ search }}
        <v-row>
          <v-col
            cols="4"
            v-for="pokemon in filter_pokemons"
            :key="pokemon.name"
          >
            <v-card @click="show_pokemon(id_pokemon(pokemon))" width="90%">
              <v-container>
                {{ id_pokemon(pokemon) }}
                <v-row class="mx-0 d-flexd-flex justify-center">
                  <img
                    :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${id_pokemon(
                      pokemon
                    )}.png`"
                    :alt="pokemon.name"
                    width="60%"
                  />
                  <h2 class="text-center">
                    {{ name_pokemon(pokemon) }}
                  </h2>
                </v-row>
              </v-container>
            </v-card>
          </v-col>
        </v-row>
      </v-conteiner>
      <!-- ////////////////////////////////////// -->
      <v-dialog v-model="show_dialog" width="800">
        <!-- TESTE IF pokemon is clicked != null -->
        <v-card v-if="clicked_pokemon">
          <v-container class="px-6">
            <v-row class="d-flex align-center">
              <v-col cols="4">
                <img
                  :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${clicked_pokemon.id}.png`"
                  :alt="clicked_pokemon.name"
                  width="80%"
                />
              </v-col>
              <v-col cols="8">
                <h1>{{ name_pokemon(clicked_pokemon) }}</h1>
                <v-chip
                  class="mr-2"
                  v-for="pokeType in clicked_pokemon.types"
                  :key="pokeType.slot"
                  >{{ pokeType.type.name }}
                </v-chip>
                <v-divider class="my-4" />
                <v-chip class="mr-2">
                  <span
                    >Height
                    {{ clicked_pokemon.height * (2.54).toFixed(1) }}cm</span
                  ></v-chip
                >
                <v-chip class="ml-4">
                  <span
                    >Weight
                    {{ clicked_pokemon.height * (0.453592).toFixed(1) }}kg</span
                  ></v-chip
                >
              </v-col>
            </v-row>
            <!-- //////////////////////////////////////////////// -->
            <h1>Skills</h1>
          </v-container>
        </v-card>
      </v-dialog>
      <!-- ////////////////////////////////////// -->
    </v-app>
  </div>
</template>

<script>
import axios from "axios"

export default {
  name: "PokeHome",

  data: () => {
    return {
      pokemons: [],
      search: "",
      show_dialog: false,
      clicked_pokemon: null,
    }
  },

  mounted() {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=300")
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
    show_pokemon(id) {
      axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`).then((response) => {
        this.clicked_pokemon = response.data
        this.show_dialog = !this.show_dialog
      })
    },

    filter_skills(pokeSkill) {
      return pokeSkill.moves.filter((skillParm) => {
        var include = false
        // version_group_details = Object from poke API
        for (let version of skillParm.version_group_details) {
          if (version.version_group_details == "sword-shield") {
            include = true
          }
          // console.log(version)
        }
        return include
      })
    },
  },

  computed: {
    filter_pokemons() {
      return this.pokemons.filter((item) => {
        return item.name.includes(this.search)
      })
    },
  },
}
</script>

<style lang="scss">
.logo {
  width: 35%;
  margin: 100px auto;
}

div {
  animation: fromTop 0.7s 0.6s backwards;
}

@keyframes fromTop {
  from  /* Ou 0%*/ {
    opacity: 0;
    transform: translateY(-100px);
  }

  to /*Ou 100%*/ {
    opacity: 1;
    transform: translateX(0);
  }
}
</style>
