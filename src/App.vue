<template>
    <main>
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Nombre</th>
                    <th scope="col">Url</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(pokemon, index) in pokemons">
                    <td>{{ index }}</td>
                    <td>{{ pokemon.name }}</td>
                    <td>{{ pokemon.url }}</td>
                    <td>
                        <button
                            id="toggleMyModal"
                            type="button"
                            class="btn btn-primary"
                            data-bs-toggle="modal"
                            data-bs-target="#exampleModal"
                            @click="openModal(pokemon)"
                        >
                            Launch demo modal
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
                        <h5 class="modal-title" id="exampleModalLabel">
                            {{ this.selected_pokemon.name }}
                        </h5>
                        <button
                            type="button"
                            class="btn-close"
                            data-bs-dismiss="modal"
                            aria-label="Close"
                        ></button>
                    </div>
                    <div class="modal-body">
                        {{ this.selected_pokemon.url }}
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
    </main>
</template>

<script>
export default {
    data() {
        return {
            pokemons: [],
            selected_pokemon: null
        };
    },

    async created() {
        try {
            let response = await fetch('https://pokeapi.co/api/v2/pokemon');
            let body = await response.json();
            this.pokemons = body.results;
        } catch (e) {
            console.error(e);
        }
    },

    methods: {
        async openModal(pokemon) {
            try {
                let response = await fetch(pokemon.url);
                let body = await response.json();
                setTimeout(() => {
                    this.selected_pokemon = body;
                }, 3000)
            } catch (e) {
                console.error(e);
            }
        }
    }
};
</script>
