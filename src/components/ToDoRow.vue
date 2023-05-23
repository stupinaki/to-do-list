<template>
  <div class="to-do-row-wrapper">
    <input
        type="checkbox"
        :checked="isSelected"
        class="to-do-checkbox"
        @change="onChange"
    >
    <div>
      <input
          v-if="isEdit"
          type="text"
          :value="text"
          @change="onTextChange"
          @blur="isEdit = false"
      >
      <div v-else> {{ text }} </div>
    </div>
    <button
        @click="onClick"
        class="to-do-row-btn"
    >
      ✎
    </button>
    <button
        @click="deleteRow"
        class="to-do-row-btn"
    >
      ❌
    </button>
  </div>
</template>

<script>
export default {
  name: "ToDoBlock",
  emits: ["deleteTask", "checkboxChange", "textChange"],
  data() {
    return {
      isEdit: false,
    }
  },
  props: {
    text: {
      type: String,
      required: true
    },
    id: {
      type: String,
      required: true
    },
    isSelected: {
      type: Boolean,
      required: true
    }
  },
  methods: {
    deleteRow () {
      this.$emit("deleteTask", this.$props.id);
    },
    onClick() {
      this.$data.isEdit = !this.$data.isEdit;
    },
    onChange() {
      const { id, text, isSelected } = this.$props;
      this.$emit("checkboxChange", {id, text, isSelected});
    },
    onTextChange(e) {
      const { id, isSelected } = this.$props;
      const text = e.target.value;
      this.$emit("textChange", {id, text, isSelected});
    }
  }
}
</script>

<style>
@import "src/variables.css";

.to-do-row-wrapper {
  display: grid;
  grid-template-columns: 1fr 9fr 1fr 1fr;
  gap: 8px;
  align-items: center;
  padding: 16px;
  border: var(--primary-border);
  border-radius: var(--border-radius);
  box-shadow: var(--box-shadow);
}
.to-do-row-btn {
  cursor: pointer;
  max-width: max-content;
  border: none;
  background-color: transparent;
}
.to-do-row-btn:hover {
  transform: scale(1.5);
}
.to-do-checkbox {
  cursor: pointer;
}
</style>