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
    </main>
</template>

<script>
export default {
    data() {
        return {
            pokemons: []
        };
    },

    async created() {
        try {
            let response = await fetch('https://pokeapi.co/api/v2/pokemon');
            let body = await response.json();
            this.pokemons = body.results;
        } catch(e) {
            console.error(e)
        }
    },
};
</script>
