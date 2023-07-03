<template>
  <div class="app">
    <div v-if="viewportWidth > 800">
      <SideBar :list="sidebarList"/>
    </div>
    <div v-else>
      <div class="menu-mobile">
        <button class="menu-burger-btn" @click="isMenuOpen = !isMenuOpen"> {{ burgerBtnText }} </button>
        <InputComponent
            placeholder="Add new list name"
            @input-change="addNewListName"
        />
      </div>
      <SideBar v-if="isMenuOpen" :list="sidebarList" @click="isMenuOpen = false"/>
    </div>

    <div class="app-main-block">
      <InputComponent
          v-if="viewportWidth > 800"
          placeholder="Add new list name"
          @input-change="addNewListName"
          class="input-component-wrapper"
      />
      <TransitionGroup name="list" tag="div" class="app-main-block-list">
        <template v-for="block in list" :key="list.id">
          <ToDoBlock
              :header="block.header"
              :todo-block-id="block.id"
              :data="block.data"
              @to-do-block-change="addNewToDoIntoList"
              @delete-to-do-block="onDeleteToDoBlock"
          />
        </template>
      </TransitionGroup>
    </div>
  </div>
</template>

<script>
import {v4} from "uuid";
import throttle from "lodash/throttle";
import SideBar from "./components/SideBar.vue";
import ToDoBlock from "./components/ToDoBlock.vue";
import InputComponent from "./components/InputComponent.vue";

export default {
  name: "app",
  components: {
    InputComponent,
    ToDoBlock,
    SideBar
  },
  data() {
    return {
      list: [],
      viewportWidth: undefined,
      isMenuOpen: false
    }
  },
  beforeMount() {
    this.$data.viewportWidth = window.innerWidth;
    window.addEventListener('resize',this.getNewResizeThrottle);
    this.$data.list = JSON.parse(localStorage.getItem("myCoolToDoListInLocalStorage")) || [];
  },
  unmounted() {
    window.removeEventListener('resize', this.getNewResizeThrottle);
  },
  methods: {
    addNewListName(value) {
      this.$data.list.push({
        id: v4(),
        header: value,
        data: []
      })
      this.changeLocalStorage();
    },
    addNewToDoIntoList(modifiedList) {
      this.$data.list = this.$data.list.map(l => l.id === modifiedList.id ? modifiedList : l);
      this.changeLocalStorage();
    },
    onDeleteToDoBlock(deleteId) {
      this.$data.list = this.$data.list.filter(l => l.id !== deleteId);
      this.changeLocalStorage();
    },
    getNewResize(e) {
      this.$data.viewportWidth = e.target.innerWidth;
    },
    changeLocalStorage() {
      localStorage.setItem("myCoolToDoListInLocalStorage", JSON.stringify(this.$data.list));
    },
  },
  computed: {
    sidebarList() {
      return this.$data.list.map(l => {
        return { id: l.id, header: l.header }
      })
    },
    getNewResizeThrottle() {
      return throttle(this.getNewResize, 1000);
    },
    burgerBtnText() {
      return this.$data.isMenuOpen ? "✕" : "☰";
    },
  }
}
</script>

<style scoped>
@import "src/variables.css";

.app {
  display: grid;
  grid-template-columns: var(--menu-desktop-width) 1fr;
  gap: 20px;
  min-height: 100vh;
  background-color: #8CEE8C57;
  font-size: 20px;
  font-family: "Open Sans",serif;
}
.app-main-block {
  display: flex;
  flex-direction: column;
  gap: 40px;
  padding: 20px;
  height: 100vh;
  overflow-y: scroll;
}
.app-main-block-list {
  display: flex;
  flex-direction: column;
  gap: 20px;
  padding-bottom: 100px;
}
.input-component-wrapper {
  padding: 16px;
  background: var(--primary-bg);
  box-shadow: var(--box-shadow);
  border-radius: 8px;
}
.menu-mobile {
  display: grid;
  grid-template-columns: 1fr 11fr;
  width: 100%;
  height: var(--menu-mobile-height);
  z-index: 30;
  background-color: var(--primary-bg);
  border-bottom: var(--primary-border);
  box-shadow: var(--box-shadow);
  padding: 20px;
}
.menu-burger-btn {
  cursor: pointer;
  border: none;
  background-color: transparent;
}
.menu-burger-btn:hover {
  transform: scale(1.5);
}
.list-enter-active,
.list-leave-active {
  transition: opacity 0.5s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
}

@media screen and (max-width: 800px){
  .app {
    grid-template-columns: 1fr;
    grid-template-rows: var(--menu-mobile-height) 1fr;
    gap: 0;
    font-size: 16px;
  }
  .app-main-block {
    display: flex;
    flex-direction: column;
    gap: 40px;
    padding: 20px;
    height: calc(100vh - var(--menu-mobile-height));
    overflow-y: scroll;
  }
}
@media screen and (max-width: 600px){
  .app {
    font-size: 14px;
  }
}
</style>
