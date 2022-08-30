<template>
  <main>
    <Pagination v-model="this.api_params" />
    <table class="table">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Nombre</th>
          <th scope="col">Url</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(pokemon, index) in pokemons" :key="pokemon.name">
          <td>{{ currentPokemonId(index) }}</td>
          <td>{{ pokemon.name }}</td>
          <td>{{ pokemon.url }}</td>
          <td>
            <button
              id="toggleMyModal"
              type="button"
              class="btn btn-primary"
              data-bs-toggle="modal"
              data-bs-target="#exampleModal"
              @click="findPokemon(currentPokemonId(index))"
            >
              Pokemon Detail
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Modal -->
    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content" v-if="this.selected_pokemon">
          <div class="modal-header">
            <img :src="this.selected_pokemon.sprites.front_default" />
            <h5 class="modal-title" id="exampleModalLabel">
              {{ this.selected_pokemon.name }}
              #{{ this.selected_pokemon.id }}
            </h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
              @click="selected_pokemon = null"
            ></button>
          </div>
          <div class="modal-body">
            <p><b>Height:</b> {{ this.selected_pokemon.height }}</p>
            <p><b>Weight:</b> {{ this.selected_pokemon.weight }}</p>
            <p>
              <b>Base Experience:</b>
              {{ this.selected_pokemon.base_experience }}
            </p>
            {{ this.selected_pokemon.url }}
            <h4>Type</h4>
            <table class="table">
              <thead>
                <td><b>Type</b></td>
                <td><b>Slot</b></td>
              </thead>
              <tbody>
                <tr
                  v-for="item in this.selected_pokemon.types"
                  :key="item.type.name"
                >
                  <td>
                    <b>{{ item.type.name }}</b>
                  </td>
                  <td>
                    {{ item.slot }}
                  </td>
                </tr>
              </tbody>
            </table>
            <h4>Stats</h4>
            <table class="table">
              <thead>
                <td><b>Stat</b></td>
                <td><b>Base Stat</b></td>
                <td><b>Effort</b></td>
              </thead>
              <tbody>
                <tr
                  v-for="stat in this.selected_pokemon.stats"
                  :key="stat.stat.name"
                >
                  <td>
                    <b>{{ stat.stat.name }}</b>
                  </td>
                  <td>
                    {{ stat.base_stat }}
                  </td>
                  <td>
                    {{ stat.effort }}
                  </td>
                </tr>
              </tbody>
            </table>
            <h4>Abilities</h4>
            <table class="table">
              <thead>
                <td><b>Ability</b></td>
                <td><b>Hidden</b></td>
                <td><b>Slot</b></td>
              </thead>
              <tbody>
                <tr
                  v-for="ability in this.selected_pokemon.abilities"
                  :key="ability.ability.name"
                >
                  <td>
                    <b>{{ ability.ability.name }}</b>
                  </td>
                  <td>
                    {{ ability.is_hidden }}
                  </td>
                  <td>
                    {{ ability.slot }}
                  </td>
                </tr>
              </tbody>
            </table>

            <h4>Movements</h4>

            <ul>
              <li v-for="move in this.selected_pokemon.moves" :key="move.name">
                {{ move.move.name }}
              </li>
            </ul>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
              @click="selected_pokemon = null"
            >
              Close
            </button>
            <button
              v-if="this.selected_pokemon.id > 1"
              type="button"
              class="btn btn-primary"
              @click="loadNextPokemon(this.selected_pokemon.id, -1)"
            >
              Previous
            </button>
            <button
              type="button"
              class="btn btn-primary"
              @click="loadNextPokemon(this.selected_pokemon.id, 1)"
            >
              Next
            </button>
          </div>
        </div>
        <div class="modal-content" v-else>
          <div class="modal-body">
            <div class="spinner-border" role="status">
              <span class="visually-hidden">Loading...</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <Pagination v-model="this.api_params" />
  </main>
</template>

<script>
import Pagination from "./components/Pagination.vue";

export default {
  components: {
    Pagination,
  },

  data() {
    return {
      pokemons: [],
      selected_pokemon: null,
      next_pokemon_id: null,
      api_params: {
        offset: 0,
        limit: 20,
        total: 0,
      },
    };
  },

  async created() {
    await this.initTable();
  },

  watch: {
    api_params: {
      deep: true,
      async handler(value, old) {
        value.offset < 0 ? value.offset = 0 : null; 
        /*console.log("New limit: " + value.limit);
        console.log("New offset: " + value.offset);
        console.log("New total: " + value.total);*/
        await this.initTable();
      },
    },

    next_pokemon_id: function (value) {
      if (value > this.api_params.offset + this.api_params.limit) {
        this.api_params.offset += this.api_params.limit;
      } else if (value < this.api_params.offset) {
        this.api_params.offset -= this.api_params.limit;
      }

      this.findPokemon(value);
    },
  },

  methods: {
    async initTable() {
      try {
        let response = await fetch(
          `https://pokeapi.co/api/v2/pokemon?limit=${this.api_params.limit}&offset=${this.api_params.offset}`
        );
        let body = await response.json();
        this.pokemons = body.results;
        this.api_params.total = body.count;
      } catch (e) {
        console.error(e);
      }
    },

    loadNextPokemon(pokemon_id, index) {
      // 9 , -1
      this.next_pokemon_id = pokemon_id + index; // 9 + (-1)
    },

    currentPokemonId(index) {
      return index + 1 + this.api_params.offset;
    },

    async findPokemon(id) {
      this.selected_pokemon = null;

      try {
        let response = await fetch(`https://pokeapi.co/api/v2/pokemon/${id}/`);

        let body = await response.json();

        this.selected_pokemon = body;
      } catch (e) {
        console.error(e);
      }
    },
  },
};
</script>
