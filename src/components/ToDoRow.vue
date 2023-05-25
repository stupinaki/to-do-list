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
          v-show="isEdit"
          ref="inputRowEditText"
          type="text"
          :value="text"
          class="input-edit-text"
          @change="onTextChange"
          @blur="isEdit = false"
          @focus="onFocus"
      >
      <div
          v-show="!isEdit"
          :class="textStyle"
          @click="onTextClick"
      >
        {{ text }}
      </div>
    </div>
    <button
        @click="onEditClick"
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
  emits: ["deleteTask", "checkboxChange", "textChange", "rowTextClick"],
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
    },
    isCrossedOut: {
      type: Boolean,
      required: true
    }
  },
  methods: {
    deleteRow () {
      this.$emit("deleteTask", this.$props.id);
    },
    onChange() {
      const { id, isSelected } = this.$props;
      this.$emit("checkboxChange", {id, keyName: "isSelected", value: !isSelected});
    },
    onTextChange(e) {
      const { id } = this.$props;
      const text = e.target.value;
      this.$emit("textChange", {id, keyName: "text", value: text});
    },
    onTextClick() {
      const { id, isCrossedOut } = this.$props;
      this.$emit("rowTextClick", { id, keyName: "isCrossedOut", value: !isCrossedOut });
    },
    onFocus(e) {
      e.target.select();
    },
    onEditClick() {
      this.$data.isEdit = !this.$data.isEdit;
      //todo
    }

  },
  computed: {
    textStyle() {
      return this.$props.isCrossedOut ? "crossed-out-text" : "normal-text";
    }
  }
}
</script>

<style scoped>
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
.input-edit-text {
  background-color: transparent;
  width: 100%;
  border: none;
  border-bottom: 1px solid #cacaca;
}
.crossed-out-text {
  cursor: pointer;
  text-decoration: line-through;
}
.normal-text {
  cursor: pointer;
  text-decoration: none;
}
</style>