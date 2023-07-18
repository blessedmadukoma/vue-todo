<script setup>
import TodoCreator from '@/components/TodoCreator.vue'
import { ref, watch } from 'vue';
import { uid } from "uid";
import TodoItem from '../components/TodoItem.vue';
import { Icon } from '@iconify/vue';

const todoList = ref([])

// track changes in the todoList and run the code if there is any change
watch(todoList, () => {
  setTodoListLocalStorage();
}, {
  deep: true, // track changes deep within the todoList
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

</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo" />

    <ul class="todo-list" v-if="todoList.length > 0">
      <!-- :todo="todo" means props (i.e. passing value, todo, from parent, TodosView, to child, TodoItem). -->
      <TodoItem v-for="(todo, index) in todoList" :todo="todo" :index="index" @toggle-complete="toggleTodoComplete"
        @edit-todo="toggleEditTodo" @update-todo="updateTodo" @delete-todo="deleteTodo" />
    </ul>

    <p class="todos-msg" v-else>
      <Icon icon="noto-v1:sad-but-relieved-face" color="#41b080" width="22" />
      <span>You have no todos to complete! Add one!</span>
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
