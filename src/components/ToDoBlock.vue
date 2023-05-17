<template>
  <div class="to-do-block">
    <div class="to-do-block-header">
      <h2> {{ header }} </h2>
      <ButtonUI bg-color="gray-red" type="button" text="Delete List" @click="deleteToDoBlock"/>
    </div>
    <div class="form-btns-wrapper">
      <InputComponent @input-change="addNewTask"/>

      <div v-if="tasks.length" class="select-delete-btns">
        <ButtonUI bg-color="blue" type="button" text="Select all" @click="selectAll"/>
        <ButtonUI bg-color="blue" type="button" text="Remove selection" @click="removeSelectAll" />
        <ButtonUI bg-color="red" type="button" text="Delete selected" @click="onDelete" />
      </div>
    </div>

    <div class="to-do-list">
      <div v-if="!tasks.length" class="empty-list"> {{ `To-do list "${header}" is empty` }} </div>
      <template v-for="task in tasks" :key="task.id" >
        <ToDoRow
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
import ToDoRow from "./ToDoRow.vue";
import ButtonUI from "./ButtonUI.vue";
import InputComponent from "./InputComponent.vue";

export default {
  name: "ToDo",
  emits: ["toDoBlockChange", "deleteToDoBlock"],
  components: {
    ToDoRow,
    ButtonUI,
    InputComponent
  },
  beforeMount() {
    this.$data.tasks = this.$props.data;
  },
  data() {
    return {
      tasks: [],
      isAllSelected: false,
    }
  },
  props: {
    todoBlockId: {
      type: String,
      required: true,
    },
    header: {
      type: String,
      required: true
    },
    data: {
      type: Array,
      required: true,
    }
  },
  methods: {
    onDeleteTask(id) {
      this.$data.tasks = this.$data.tasks.filter(t => t.id !== id);
      this.changeTodoBlock();
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
      this.changeTodoBlock();
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
      this.changeTodoBlock();
    },
    selectAll() {
      this.$data.tasks = this.$data.tasks.map(t => {
        return {
          ...t,
          isSelected: true
        }
      })
      this.$data.isAllSelected = true;
      this.changeTodoBlock();
    },
    removeSelectAll() {
      this.$data.tasks = this.$data.tasks.map(t => {
        return {
          ...t,
          isSelected: false
        }
      })
      this.changeTodoBlock();
    },
    onDelete() {
      this.$data.tasks = this.$data.tasks.filter(t => !t.isSelected);
      this.changeTodoBlock();
    },
    changeTodoBlock() {
      const { todoBlockId, header } = this.$props;
      this.$emit("toDoBlockChange", {
        id: todoBlockId,
        header: header,
        data: this.$data.tasks,
      })
    },
    deleteToDoBlock() {
      this.$emit("deleteToDoBlock", this.$props.todoBlockId);
    }
  }
}
</script>

<style scoped>
.to-do-block {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 30px;
  padding: 20px;
  background: #f5f5f5;
  border-radius: 8px;
  box-shadow: 1px 8px 12px #3a3c4c14, 1px 1px 2px #3a3c4c0a;
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
  padding: 16px;
  background: #f5f5f5;
  box-shadow: 1px 8px 12px #3a3c4c14, 1px 1px 2px #3a3c4c0a;
  border-radius: 8px;
}
.select-delete-btns {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 20px;
}
.empty-list {
  font-size: 20px;
  text-align: center;
  margin-top: 40px;
}
.to-do-block-header {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: 20px;
  align-items: center;
  justify-content: space-between;
  padding: 0 16px;
}

</style>