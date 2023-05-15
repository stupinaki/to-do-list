<template>
  <div class="app">
    <div class="form-btns-wrapper">
      <InputComponent @input-change="addNewTask"/>

      <div v-if="tasks.length" class="select-delete-btns">
        <button @click="selectAll" class="select-all-btn">
          Select all
        </button>
        <button @click="removeSelectAll" class="select-all-btn">
          Remove selection
        </button>
        <button @click="onDelete" class="delete-selected-btn">
          Delete selected
        </button>
      </div>
    </div>

    <div class="to-do-list">
      <div v-if="!tasks.length" class="empty-list"> Your to-do list is empty </div>

      <template v-for="task in tasks" :key="task.id" >
        <ToDoBlock
            :id="task.id"
            :text="task.text"
            :is-selected="task.isSelected"
            @delete-task="onDeleteTask"
            @checkbox-change="onCheckboxChange"
        />
      </template>
    </div>

  </div>

</template>

<script>
import ToDoBlock from "./components/ToDoBlock.vue";
import InputComponent from "./components/InputComponent.vue";

export default {
  name: "app",
  components: {
    ToDoBlock,
    InputComponent
  },
  beforeMount() {
    this.$data.tasks = JSON.parse(localStorage.getItem("myCoolToDoList")) || [];
  },
  data() {
    return {
      tasks: [],
      isAllSelected: false,
    }
  },
  methods: {
    onDeleteTask(id) {
      this.$data.tasks = this.$data.tasks.filter(t => t.id !== id);
      this.changeLocalStorage();
    },
    addNewTask(value) {
      const isExist = !!this.$data.tasks.find(t => t.text === value);
      if(isExist) {
        return;
      }
      const newTask = {
        id: value,
        text: value,
        isSelected: false,
      }
      this.$data.tasks.push(newTask);
      this.changeLocalStorage();
    },
    onCheckboxChange(task) {
      this.$data.tasks = this.$data.tasks.map(t => {
        if(t.id === task.id) {
          return {
            ...t,
            isSelected: !t.isSelected,
          }
        }
        return t;
      });
      this.changeLocalStorage();
    },
    selectAll() {
      this.$data.tasks = this.$data.tasks.map(t => {
        return {
          ...t,
          isSelected: true
        }
      })
      this.$data.isAllSelected = true;
      this.changeLocalStorage();
    },
    removeSelectAll() {
      this.$data.tasks = this.$data.tasks.map(t => {
        return {
          ...t,
          isSelected: false
        }
      })
      this.changeLocalStorage();
    },
    onDelete() {
      this.$data.tasks = this.$data.tasks.filter(t => !t.isSelected);
      this.changeLocalStorage();
    },
    changeLocalStorage() {
      localStorage.setItem("myCoolToDoList", JSON.stringify(this.$data.tasks));
    }
  }
}
</script>

<style scoped>
.app {
  position: relative;
}
.to-do-list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}
.form-btns-wrapper{
  position: sticky;
  top: 0;
  left: 0;
  right: 0;
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 30px 8px;
  margin-bottom: 40px;
  background: #f5f5f5;
  box-shadow: 1px 8px 12px #3a3c4c14, 1px 1px 2px #3a3c4c0a;
  border-radius: 8px;
}
.select-delete-btns {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 20px;
}

.select-delete-btns > * {
  padding: 16px;
  border: none;
  border-radius: 8px;
  box-shadow: 1px 8px 12px #3a3c4c14, 1px 1px 2px #3a3c4c0a;
  cursor: pointer;
}

.select-all-btn {
  background-color: aqua;
}

.select-all-btn:hover {
  background-color: #04b1b1;
}

.delete-selected-btn {
  background-color: #db3636;
}

.delete-selected-btn:hover {
  background-color: #952020;
}

.empty-list {
  font-size: 20px;
  text-align: center;
  margin-top: 40px;
}

</style>
