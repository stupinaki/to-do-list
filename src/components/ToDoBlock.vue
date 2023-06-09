<template>
  <div :id="todoBlockId" class="to-do-block">
    <div class="form-btns-wrapper">
      <div class="to-do-block-header">
        <div/>
        <h3 class="to-do-block-header-text"> {{ header }} </h3>
        <button class="to-do-block-header-delete-btn" @click="deleteToDoBlock"> ❌ </button>
      </div>
      <InputComponent @input-change="addNewTask"/>
    </div>

    <div v-if="tasks.length" class="select-delete-btns">
      <ButtonUI bg-color="blue" type="button" :text="selectBtnText" @click="onSelectBtnClick"/>
      <ButtonUI bg-color="red" type="button" text="Delete selected" @click="onDeleteTask" />
    </div>

    <div class="to-do-list">
      <div v-if="!tasks.length" class="empty-list"> To-do list <strong> «{{header}}» </strong> is empty</div>

      <TransitionGroup name="small-task-list" tag="div">
        <template v-for="task in tasks" :key="task.id" >
          <ToDoRow
              :id="task.id"
              :text="task.text"
              :is-selected="task.isSelected"
              :is-crossed-out="task.isCrossedOut"
              @delete-task="onDeleteTask"
              @checkbox-change="onRowChange"
              @text-change="onRowChange"
              @row-text-click="onRowChange"
          />
        </template>
      </TransitionGroup>
    </div>
  </div>
</template>

<script>
import {v4} from "uuid";
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
    onDeleteTask(id = undefined) {
      this.$data.tasks =  id ? this.$data.tasks.filter(t => t.id !== id) : this.$data.tasks.filter(t => !t.isSelected);
      this.changeTodoBlock();
    },
    addNewTask(value) {
      const isExist = !!this.$data.tasks.find(t => t.text === value);
      if(isExist) {
        return;
      }
      const newTask = {
        id: v4(),
        text: value,
        isSelected: false,
        isCrossedOut: false,
      }
      this.$data.tasks.push(newTask);
      this.changeTodoBlock();
    },
    onRowChange(data) {
      this.$data.tasks = this.$data.tasks.map(t => {
        if(t.id === data.id) {
          return {
            ...t,
            [data.keyName]: data.value,
          }
        }
        return t;
      });
      this.changeTodoBlock();
    },
    onSelectBtnClick() {
      this.$data.tasks = this.$data.tasks.map(t => {
        return {
          ...t,
          isSelected: !this.isAllSelected
        }
      })
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
  },
  computed: {
    isAllSelected() {
      return this.$data.tasks.every(t => t.isSelected);
    },
    selectBtnText() {
      return this.isAllSelected ? "Remove selection" : "Select all";
    }
  }
}
</script>

<style scoped>
@import "src/variables.css";

.to-do-block {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 30px;
  padding: 20px;
  background: var(--primary-bg);
  border-radius: var(--border-radius);
  box-shadow: var(--box-shadow);
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
  background: var(--primary-bg);
  box-shadow: var(--box-shadow);
  border-radius: var(--border-radius);
}
.select-delete-btns {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}
.empty-list {
  font-size: 20px;
  text-align: center;
}
.to-do-block-header {
  display: grid;
  grid-template-columns: 1fr 10fr 1fr;
  gap: 8px;
  align-items: center;
  justify-content: space-between;
}
.to-do-block-header-text {
  text-align: center;
}
.to-do-block-header-delete-btn {
  cursor: pointer;
  max-width: max-content;
  border: none;
  background-color: transparent;
}
.to-do-block-header-delete-btn:hover {
  transform: scale(1.5);
}
.small-task-list-enter-active,
.small-task-list-leave-active {
  transition: all 0.5s ease;
}
.small-task-list-enter-from,
.small-task-list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
@media screen and (max-width: 600px){
  .select-delete-btns,
  .to-do-block-header {
    gap: 10px;
  }
  .empty-list {
    font-size: 16px;
  }
}

</style>