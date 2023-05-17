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

<style scoped>
.to-do-row-wrapper {
  display: grid;
  grid-template-columns: 1fr 10fr 1fr;
  gap: 8px;
  align-items: center;
  padding: 16px;
  border: 1px solid #f0f0f0;
  border-radius: 8px;
  box-shadow: 1px 8px 12px #3a3c4c14, 1px 1px 2px #3a3c4c0a;
}
.to-do-row-btn {
  border: none;
  background-color: transparent;
  cursor: pointer;
  max-width: max-content;
}
.to-do-row-btn:hover {
  transform: scale(2);
}
.to-do-checkbox {
  cursor: pointer;
}
</style>