<template>
  <div class="side-bar-wrapper">
     <template v-for="task in list" :key="task.name">
        <ButtonUI bg-color="gray-green" type="button" :text="task.header" @click="onClick(task.id)"/>
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
    },
  },
  methods: {
    onClick(id) {
      const target =  document.getElementById(id);
      target.scrollIntoView({block: "start", inline: "center", behavior: "smooth"});
    }
  }
}
</script>

<style scoped>
@import "src/variables.css";

.side-bar-wrapper {
  position: fixed;
  display: flex;
  flex-direction: column;
  align-items: start;
  gap: 20px;
  width: var(--menu-desktop-width);
  height: 100%;
  padding: 16px;
  overflow-y: auto;
  background: var(--primary-bg);
  box-shadow: var(--box-shadow);
  border-right: var(--primary-border);
}
@media screen  and (max-width: 800px) {
  .side-bar-wrapper {
    position: fixed;
    top: var(--menu-mobile-height);
    left: 0;
    right: 0;
    bottom: 0;
    align-items: center;
    width: 100%;
    height: auto;
    z-index: 20;
  }
}
</style>