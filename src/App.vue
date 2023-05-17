<template>
  <div class="app">
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

    <SideBar :list="sidebarList"/>

  </div>
</template>

<script>
import {v4} from "uuid";
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
      list: []
    }
  },
  beforeMount() {
    this.$data.list = JSON.parse(localStorage.getItem("myCoolToDoList")) || [];
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
    }
  },
  computed: {
    sidebarList() {
      return this.$data.list.map(l => {
        return { id: l.id, header: l.header }
      })
    }
  }
}
</script>

<style scoped>
.app {
  position: relative;
  display: grid;
  grid-template-columns: 10fr 2fr;
  gap: 20px;
  min-height: 100vh;
  background-color: #8CEE8C57;
  padding-left: 20px;
}
.app-main-block {
  display: flex;
  flex-direction: column;
  gap: 40px;
  padding-bottom: 20px;
}
.input-component-wrapper {
  padding: 16px;
  background: #f5f5f5;
  box-shadow: 1px 8px 12px #3a3c4c14, 1px 1px 2px #3a3c4c0a;
  border-radius: 8px;
  margin-top: 20px;
}


</style>
