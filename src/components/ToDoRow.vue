<template>
  <div class="to-do-row-wrapper">
    <input
        type="checkbox"
        :checked="isSelected"
        class="to-do-checkbox"
        @change="onChange"
    >
    <div> {{ text }} </div>
    <button
        @click="onClick"
        class="to-do-row-btn"
    >
      x
    </button>
  </div>
</template>

<script>
export default {
  name: "ToDoBlock",
  emits: ["deleteTask", "checkboxChange"],
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
    onClick () {
      this.$emit("deleteTask", this.$props.id);
    },
    onChange() {
      const { id, text, isSelected } = this.$props;
      this.$emit("checkboxChange", {id, text, isSelected});
    }
  }
}
</script>

<style>
@import "src/variables.css";

.to-do-row-wrapper {
  display: grid;
  grid-template-columns: 1fr 10fr 1fr;
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
  transform: scale(2);
}
.to-do-checkbox {
  cursor: pointer;
}
</style>