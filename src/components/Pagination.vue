<template>
  <p class="d-flex py-5">
    <select v-model="api_params.limit" class="form-select">
      <option
        v-for="index in [20, 40, 80, 160, 200]"
        :key="index"
        :value="index"
      >
        {{ index }}
      </option>
    </select>

    <button
      type="button"
      class="btn btn-primary mx-4"
      :disabled="api_params.offset <= 0"
      @click="api_params.offset -= api_params.limit"
    >
      Previous
    </button>
    <button
      type="button"
      class="btn btn-primary"
      :disabled="api_params.offset + api_params.limit >= api_params.total"
      @click="api_params.offset += api_params.limit"
    >
      Next
    </button>
  </p>
</template>
<script>
export default {

  props: ["modelValue"],

  data() {
    return {
      api_params: this.modelValue,
    };
  },

  watch: {
    api_params: {
      deep: true,
      handler(value, old) {
        console.log('new offset: ' + value.offset);
        this.$emit("input", value);
      },
    },
  },
};
</script>
