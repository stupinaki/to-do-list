<template>
  <button
      :type="type"
      :class="btnStyle"
      @click="$emit('click')"
  >
    {{ text }}
  </button>
</template>

<script>
export default {
  name: "ButtonUI",
  emits: ["click"],
  props: {
    text: {
      type: String,
      required: true,
    },
    bgColor: {
      type: String,
      required: false,
      default: "grey-green",
      validator(bgColor) {
        return ["green", "blue", "red", "grey-red", "grey-green"].includes(bgColor);
      }
    },
    type: {
      type: String,
      required: true
    }
  },
  computed: {
    btnStyle() {
      const { bgColor } = this.$props;
      return "default-btn-style" + ` btn-${bgColor}`;
    }
  }
}
</script>

<style scoped>
.default-btn-style {
  padding: 16px;
  border: none;
  border-radius: 8px;
  box-shadow: 1px 8px 12px #3a3c4c14, 1px 1px 2px #3a3c4c0a;
  cursor: pointer;
  width: 100%;
  white-space: normal;
}
.btn-green {
  background-color: palegreen;
}
.btn-green:hover,
.btn-gray-green:hover {
  background-color: #64ef64;
}
.btn-blue {
  background-color: aqua;
}
.btn-blue:hover {
  background-color: #0be7e7;
}
.btn-red {
  background-color: #db3636;
}
.btn-red:hover,
.btn-gray-red:hover {
  background-color: #d02d2d;
}

@media screen and (max-width: 600px){
  .default-btn-style {
    padding: 10px;
  }
}

</style>