<template>
    <p class="d-flex py-5">
      <select v-model="form.limit" class="form-select">
        <option v-for="index in [20, 40, 80, 160, 200]" 
          :value="index">
          <slot name="options" :value="index">
            {{ index }}
          </slot>
        </option>
      </select>

      <button
        type="button"
        class="btn btn-primary mx-4"
        :disabled="form.offset <= 0"
        @click="form.offset -= form.limit"
      >
        <slot name="previous">
          Previous
        </slot>
      </button>
      <button
        type="button"
        class="btn btn-primary"
        :disabled="(form.offset + form.limit) >= form.total"
        @click="onNextClick"
      >
        <slot name="next">
          Next
        </slot>
      </button>
    </p>
</template>

<script>
export default {
  props: ['modelValue'],

  data() {
    return {
      form: this.modelValue
    }
  },

  watch: {
    form: {
      deep: true,
      handler(value, old) {
        this.$emit('input', value)
      }
    }
  },

  methods: {
    onNextClick() {
      this.form.offset += this.form.limit
      this.$emit('next', true);
    }
  }
}
</script>