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
                </tr>
            </tbody>
        </table>

        <!-- Button trigger modal -->
        <button
            type="button"
            class="btn btn-primary"
            data-bs-toggle="modal"
            data-bs-target="#exampleModal"
        >
            Launch demo modal
        </button>

        <!-- Modal -->
        <div
            class="modal fade"
            id="exampleModal"
            tabindex="-1"
            aria-labelledby="exampleModalLabel"
            aria-hidden="true"
        >
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLabel">
                            Modal title
                        </h5>
                        <button
                            type="button"
                            class="btn-close"
                            data-bs-dismiss="modal"
                            aria-label="Close"
                        ></button>
                    </div>
                    <div class="modal-body">...</div>
                    <div class="modal-footer">
                        <button
                            type="button"
                            class="btn btn-secondary"
                            data-bs-dismiss="modal"
                        >
                            Close
                        </button>
                        <button type="button" class="btn btn-primary">
                            Save changes
                        </button>
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
};
</script>
