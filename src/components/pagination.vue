<template>

<p class="d-flex py-5">
      <select v-model="form.limit" class="form-select">
        <option v-for="index in [20, 40, 80, 160, 200]" 
          :value="index">{{ index }}
        </option>
      </select>

      <button
        type="button"
        class="btn btn-primary mx-4"
        :disabled="form.offset <= 0"
        @click="form.offset -= form.limit"
      >
        Previous
      </button>
      <button
        type="button"
        class="btn btn-primary"
        :disabled="(form.offset + form.limit) >= form.total"
        @click="form.offset += form.limit"
      >
        Next
      </button>
    </p>

</template>

<script>
export default{
    props: ['modelValue'],

    data(){
        return{
            form: this.modelValue
        }
    },

    watch:{
        form: {
            deep: true,
            handler (value, old){
                this.$emit('input', value)
            }
        }
    }
}



</script>