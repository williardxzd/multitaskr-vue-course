<template>
    <main>
        <Total v-model="total" :subtotal="subtotal"></Total>
        {{ total }}
    </main>
</template>

<script>
    import Total from './components/Total.vue';

    export default {
        components: {
            Total
        },

        data() {
            return {
                total: 0,
                subtotal: 10000
            }
        },

        async created() {
            let response = await fetch('totals.json')
            let body = await response.json()

            this.total = body.data.total
        },

        mounted() {

        },

        watch: {
            total: function(newValue) {
                console.log(newValue)
            }
        }
    }
</script>
