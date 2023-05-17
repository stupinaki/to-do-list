<template>
  <form @submit.prevent="onSubmit" class="form-add">
    <div class="form-input-wrapper">
      <input
          v-model.trim="inputValue"
          type="text"
          class="form-add-input"
          :placeholder="placeholder"
          @focus="onFocus"
      >
      <button
          v-if="inputValue"
          type="button"
          class="form-delete-btn"
          @click="inputValue = ''"
      >
        x
      </button>
    </div>

    <ButtonUI
        type="submit"
        text="Add"
        bg-color="green"
    />
  </form>


</template>

<script>
import ButtonUI from "./ButtonUI.vue";

export default {
  name: "InputComponent",
  emits: ["inputChange"],
  components: {
    ButtonUI
  },
  data() {
    return {
      inputValue: "",
      isOnFocus: false,
    }
  },
  props: {
    placeholder: {
      type: String,
      required: false,
      default: "Add new task"
    }
  },
  methods: {
    onSubmit() {
      const {inputValue} = this.$data;
      if (inputValue) {
        this.$emit("inputChange", inputValue);
        this.$data.inputValue = "";
      }
    },
    onFocus(e) {
      e.target.select();
    }
  },
}
</script>

<style scoped>
.form-add {
  display: grid;
  grid-template-columns: 4fr 1fr;
  gap: 16px;
}

.form-input-wrapper {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 20px;
  padding: 16px;
  border: 1px solid #f0f0f0;
  border-radius: 8px;
  box-shadow: 1px 8px 12px #3a3c4c14, 1px 1px 2px #3a3c4c0a;
}

.form-add-input {
  height: 100%;
  width: 100%;
  border: none;
  background-color: transparent;
}

.form-add-input:focus-visible {
  outline: none;
  border: none;
  box-shadow: none;
}

.form-delete-btn {
  border: none;
  background-color: transparent;
  cursor: pointer;
  max-width: max-content;
}

.form-delete-btn:hover {
  transform: scale(2);
}
</style>