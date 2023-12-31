<script setup>
import TodoCreator from '@/components/TodoCreator.vue'
import { computed, ref, watch } from 'vue';
import { uid } from "uid";
import TodoItem from '../components/TodoItem.vue';
import { Icon } from '@iconify/vue';
import draggable from 'vuedraggable';

const todoList = ref([])

// track changes in the todoList and run the code if there is any change
watch(todoList, () => {
  setTodoListLocalStorage();
}, {
  deep: true, // track changes deep within the todoList
})

// computed property to check if all todos are completed. computed automatically tracks reactive dependencies e.g. todoList.
const todoCompleted = computed(() => {
  return todoList.value.every((todo) => todo.isCompleted)
})

const fetchTodoList = () => {
  const savedTodoList = JSON.parse(localStorage.getItem("todoList"))
  if (savedTodoList) {
    todoList.value = savedTodoList
  }
}

fetchTodoList();

const setTodoListLocalStorage = () => {
  localStorage.setItem("todoList", JSON.stringify(todoList.value))
}

const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo: todo,
    isCompleted: null,
    isEditing: null,
  });
}

const toggleTodoComplete = (index) => {
  todoList.value[index].isCompleted = !todoList.value[index].isCompleted;
}

const toggleEditTodo = (index) => {
  todoList.value[index].isEditing = !todoList.value[index].isEditing;
}

const updateTodo = (todo, index) => {
  todoList.value[index].todo = todo;
}

const deleteTodo = (index) => {
  todoList.value = todoList.value.filter((todo) => todo.id !== index);
}

// Drag and drop
const onMove = ({ relatedContext, draggedContext }) => {
  const relatedElement = relatedContext.element;
  const draggedElement = draggedContext.element;
  return (
    (!relatedElement || !relatedElement.fixed) && !draggedElement.fixed
  );
}

const dragOptions = () => {
  return {
    animation: 0,
    group: "description",
    disabled: !this.editable,
    ghostClass: "ghost"
  };
}

</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo" />

    <ul class="todo-list" v-if="todoList.length > 0">
      <draggable v-model="todoList" class="todo-list" v-bind="dragOptions" :move="onMove" @start="isDragging = true"
        @end="isDragging = false" item-key="id">
        <template #item="{ element, index }">
          <div>
            <!-- :todo="todo" means props (i.e. passing value, todo, from parent, TodosView, to child, TodoItem). -->
            <TodoItem :todo="element" :index="index" @toggle-complete="toggleTodoComplete" @edit-todo="toggleEditTodo"
              @update-todo="updateTodo" @delete-todo="deleteTodo" />
          </div>
        </template>
      </draggable>
    </ul>

    <p class="todos-msg" v-else>
      <Icon icon="noto-v1:sad-but-relieved-face" color="#41b080" width="22" />
      <span>You have no todos to complete! Add one!</span>
    </p>

    <p v-if="todoCompleted && todoList.length > 0" class="todos-msg">
      <Icon icon="noto-v1:party-popper" />
      <span>You have completed all todos!</span>
    </p>
  </main>
</template>

<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;
}

h1 {
  margin-bottom: 16px;
  text-align: center;
}

.todo-list {
  display: flex;
  flex-direction: column;
  list-style: none;
  margin-top: 24px;
  gap: 20px;
}

.todos-msg {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  margin-top: 24px;
}
</style>
