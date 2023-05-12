<template>
  <form @submit.prevent="onSubmit" class="form-add">
    <input
        v-model="inputValue"
        type="text"
        class="form-add-input"
    >
    <button type="submit" class="form-add-btn">
      Add
    </button>
  </form>


</template>

<script>
export default {
  name: "InputComponent",
  emits: ["inputChange"],
  data() {
    return {
      inputValue: ""
    }
  },
  methods: {
    onSubmit() {
      const { inputValue } = this.$data;
      if(inputValue) {
        this.$emit("inputChange", this.$data.inputValue);
      }
    }
  }
}
</script>

<style scoped>
.form-add {
  display: grid;
  grid-template-columns: 4fr 1fr;
  gap: 16px;
}
.form-add-input {
  padding: 16px;
  border: 1px solid #f0f0f0;
  border-radius: 8px;
  box-shadow: 1px 8px 12px #3a3c4c14, 1px 1px 2px #3a3c4c0a;
}
.form-add-btn {
  padding: 16px;
  border: 1px solid #f0f0f0;
  border-radius: 8px;
  background-color: palegreen;
  box-shadow: 1px 8px 12px #3a3c4c14, 1px 1px 2px #3a3c4c0a;
  cursor: pointer;
}
.form-add-btn:hover {
  background-color: #64ef64;
}
</style>