<script setup>
import { Icon } from "@iconify/vue"

// const props = defineProps(["todo"]) // not recommended as it is not readable.
const props = defineProps({
 todo: {
  type: Object,
  required: true,
  // default: "hello" // works if the type is String, number of boolean, NOT Object as in this case
  // for Object, use default() {
  //  return {}
  // }
 }
})

</script> 

<template>
 <li>
  <input type="checkbox" :checked="todo.isCompleted" />
  <div class="todo">
   <!-- note: when handling data for a prop, it's best to handle in the parent component i.e. TodosView in this case -->
   <input type="text" v-if="todo.isEditing" :value="todo.todo" />
   <span v-else>{{ todo.todo }}</span>
  </div>

  <div class="todo-actions">
   <Icon icon="ph:check-circle" class="icon" color="#41b080" width="22" v-if="todo.isEditing" />
   <Icon icon="ph:pencil-fill" class="icon" color="#41b080" width="22" v-else />
   <Icon icon="ph:trash" class="icon" color="#f95e5e" width="22" />
  </div>
 </li>
</template>



<style lang="scss" scoped>
li {
 display: flex;
 align-items: center;
 gap: 10px;
 padding: 16px 10px;
 background-color: #f1f1f1;
 box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1),
  0 8px 10px -6px rgb(0 0 0 / 0.1);

 &:hover {
  .todo-actions {
   opacity: 1;
  }
 }

 input[type="checkbox"] {
  appearance: none;
  width: 20px;
  height: 20px;
  background-color: #fff;
  border-radius: 50%;
  box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);

  &:checked {
   background-color: #41b080;
  }
 }

 .todo {
  flex: 1;

  input[type="text"] {
   width: 100%;
   padding: 2px 6px;
   border: 2px solid #41b080;
  }
 }

 .todo-actions {
  display: flex;
  gap: 6px;
  opacity: 0;
  transition: 150ms ease-in-out;

  .icon {
   cursor: pointer;
  }
 }
}
</style>