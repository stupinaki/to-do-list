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

<style>
@import "src/variables.css";

.side-bar-wrapper {
  display: flex;
  flex-direction: column;
  gap: 20px;
  align-items: start;
  width: 100%;
  padding: 16px;
  background: var(--primary-bg);
  border-radius: var(--border-radius);
  box-shadow: var(--box-shadow);
  border: var(--primary-border);
}

</style>