<template>
  <div class="todo-list">
    <h1 class="text-4xl font-extrabold text-gray-800 mb-6 text-center">Todo List</h1>

    <!-- 添加任务 -->
    <div class="flex gap-4 mb-6">
      <input
        v-model="newTodo"
        @keyup.enter="addTodo"
        placeholder="Add a new task"
        class="flex-1 p-1 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-600 placeholder-gray-500"
      />
      <Button
        @click="addTodo"
        class="w-32 px-2 py-2 text-lg text-white bg-blue-500 rounded-lg shadow-md hover:bg-blue-600 focus:ring-4 focus:ring-blue-300"
      >
        Add
      </Button>
    </div>

    <!-- 任务列表 -->
    <ul class="space-y-4">
      <li
        v-for="(todo, index) in todos"
        :key="index"
        class="flex items-center justify-between p-4 bg-gray-50 rounded-lg shadow-sm hover:bg-gray-100"
      >
        <div class="flex items-center gap-3">
          <input
            type="checkbox"
            v-model="todo.completed"
            class="w-5 h-5 rounded-full border-gray-300 text-blue-500 focus:ring-blue-500"
          />
          <span
            :class="{'line-through text-gray-500': todo.completed, 'text-gray-800': !todo.completed}"
            class="text-lg font-semibold"
          >
            {{ todo.text }}
          </span>
        </div>
        <Button
          @click="removeTodo(index)"
          class="text-white bg-red-500 hover:bg-red-700 focus:ring-2 focus:ring-red-300"
        >
          Delete
        </Button>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import Button from "@/components/ui/button/Button.vue";

interface Todo {
  text: string;
  completed: boolean;
}

export default defineComponent({
  name: 'App',
  components: {
    Button,
  },
  setup() {
    const newTodo = ref('');
    const todos = ref<Todo[]>([]);

    const addTodo = () => {
      if (newTodo.value.trim() === '') return;
      todos.value.push({
        text: newTodo.value.trim(),
        completed: false,
      });
      newTodo.value = '';
    };

    const removeTodo = (index: number) => {
      todos.value.splice(index, 1);
    };

    return {
      newTodo,
      todos,
      addTodo,
      removeTodo,
    };
  },
});
</script>

<style scoped lang="scss">
.todo-list {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
}
</style>