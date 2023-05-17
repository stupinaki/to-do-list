<template>
  <div class="app">
    <SideBar v-if="viewportWidth >= 800" :list="sidebarList"/>
    <div class="app-main-block">
      <InputComponent
          placeholder="Add new list name"
          @input-change="addNewListName"
          class="input-component-wrapper"
      />
      <template v-for="block in list" :key="list.id">
        <ToDoBlock
            :header="block.header"
            :todo-block-id="block.id"
            :data="block.data"
            @to-do-block-change="addNewToDoIntoList"
            @delete-to-do-block="onDeleteToDoBlock"
        />

      </template>
    </div>
  </div>
</template>

<script>
import {v4} from "uuid";
import throttle from "lodash/throttle";
import InputComponent from "./components/InputComponent.vue";
import ToDoBlock from "./components/ToDoBlock.vue";
import SideBar from "./components/SideBar.vue";

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
      viewportWidth: undefined
    }
  },
  beforeMount() {
    this.$data.viewportWidth = window.innerWidth;
    window.addEventListener('resize',this.getNewResizeThrottle);
    this.$data.list = JSON.parse(localStorage.getItem("myCoolToDoList")) || [];
  },
  unmounted() {
    window.removeEventListener('resize', this.getNewResizeThrottle);
  },
  methods: {
    addNewListName(value) {
      //todo проверять есть ли такой список уже?
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
    changeLocalStorage() {
      localStorage.setItem("myCoolToDoList", JSON.stringify(this.$data.list));
    },
    onDeleteToDoBlock(deleteId) {
      this.$data.list = this.$data.list.filter(l => l.id !== deleteId);
      this.changeLocalStorage();
    },
    getNewResize(e) {
      this.$data.viewportWidth = e.target.innerWidth;
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
  }
}
</script>

<style scoped>
.app {
  position: relative;
  display: grid;
  grid-template-columns: 2fr 10fr;
  gap: 20px;
  min-height: 100vh;
  background-color: #8CEE8C57;
  font-size: 20px;
}
.app-main-block {
  display: flex;
  flex-direction: column;
  gap: 40px;
  padding: 20px;
}
.input-component-wrapper {
  padding: 16px;
  background: #f5f5f5;
  box-shadow: 1px 8px 12px #3a3c4c14, 1px 1px 2px #3a3c4c0a;
  border-radius: 8px;
}

@media screen and (max-width: 800px){
  .app {
    grid-template-columns: 1fr;
    font-size: 16px;
  }
}
@media screen and (max-width: 600px){
  .app {
    font-size: 14px;
  }
}
</style>
