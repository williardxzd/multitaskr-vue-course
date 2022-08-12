<template>
    <input type="number" v-model="total">
</template>

<script>
    export default {
        props: ['subtotal', 'modelValue'],

        data() {
            return {
                total: this.modelValue,
                discount: 0,
                taxes: 0,
            };
        },

        mounted() {
            console.log(this.total)
        },

        watch: {
            total: function (value) {
                this.$emit('update:modelValue', value);
            },
        },

        methods: {
            getTotalAmount() {
                return this.total + this.getSubtotalAmount();
            },

            getSubtotalAmount() {
                return this.subtotal;
            },
        },
    };
</script>
