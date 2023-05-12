<template>

  <div class="to-do-list">
    <InputComponent @input-change="onChange"/>

    <div class="select-delete-btns">
      <button @click="onSelect" class="select-all-btn">
        Select all
      </button>
      <button @click="onDelete" class="delete-selected-btn">
        Delete selected
      </button>
    </div>

    <template v-for="task in tasks" :key="task.id" >
      <ToDoBlock
          :id="task.id"
          :text="task.text"
          @delete-task="onDeleteTask"
      />
    </template>
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
  data() {
    return {
      tasks: [
        {id: "som1", text: "task 1"},
        {id: "som2", text: "task 2"},
        {id: "som3", text: "task 3"},
        {id: "som4", text: "task 4"},
      ],

    }
  },
  methods: {
    onDeleteTask(id) {
      this.$data.tasks = this.$data.tasks.filter(t => t.id !== id);
    },
    onChange(value) {
      const newTask = {
        id: value,
        text: value
      }
      this.$data.tasks.push(newTask);
    },
    onSelect() {
      console.log("on select")
    },
    onDelete() {
      console.log("onDelete")
    }
  }
}
</script>

<style scoped>
.to-do-list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}
.select-delete-btns {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

.select-delete-btns > * {
  padding: 16px;
  border: 1px solid #f0f0f0;
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

</style>
