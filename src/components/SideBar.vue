<template>
  <div class="side-bar-wrapper">
    <template v-for="task in list" :key="task.name">
      <ButtonUI bg-color="gray-green" type="button" :text="task.header"/>
    </template>
  </div>
</template>

<script>
import ButtonUI from "./ButtonUI.vue";

export default {
  name: "SideBar",
  components: {
    ButtonUI
  },
  props: {
    list: {
      type: Array,
      required: true
    }
  },
}
</script>

<style scoped>
.side-bar-wrapper {
  display: flex;
  flex-direction: column;
  gap: 20px;
  align-items: start;
  width: 100%;
  padding: 16px;
  background: #f5f5f5;
  border: 1px solid #f0f0f0;
  box-shadow: 1px 8px 12px #3a3c4c14, 1px 1px 2px #3a3c4c0a;
}

</style>