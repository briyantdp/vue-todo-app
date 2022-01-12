<template>
  <div class="container mt-4">
    <div class="card">
      <div class="card-body">
        <h1 class="h1">Simple Todolist App</h1>
        <div class="row gx-4 mt-3">
          <div class="col">
            <input
              class="form-control"
              placeholder="Masukkan task mu..."
              type="text"
              v-model="todo"
              @keyup.enter="addTodo"
            />
          </div>
          <div class="col-auto">
            <button class="btn btn-success" @click="addTodo">Tambah</button>
          </div>
        </div>

        <TodoList :todoList="list" @deleteTodo="deleteTodo" @doneTodo="doneTodo" />

        <div class="todo-stats mt-4">
          <span>Total todolist saat ini : {{ totalTodo }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive, onMounted, computed, toRefs } from 'vue'
import TodoList from './components/TodoList.vue'
export default {
  components: {
    TodoList
  },
  setup() {
    const todo = ref("");
    const todoList = reactive({
      list: []
    });

    onMounted(() => {
      const items = localStorage.getItem('todoList');
      todoList.list = items ? JSON.parse(items) : [];
    })

    const totalTodo = computed(() => {
      return todoList.list.length;
    })

    const addTodo = () => {
      todoList.list.unshift({ activity: todo.value, isDone: false });
      todo.value = "";
      saveToLocalStorage()
    }

    const deleteTodo = indexTodo => {
      // todoList.list.shift();
      todoList.list = todoList.list.filter((item, index) => {
        if (indexTodo != index) {
          return item;
        }
      })
      saveToLocalStorage();
    }

    const doneTodo = indexTodo => {
      todoList.list = todoList.list.filter((item, index) => {
        if (index == indexTodo) {
          item.isDone = !item.isDone;
        }
        return item
      })
      saveToLocalStorage()
    }

    const saveToLocalStorage = () => {
      localStorage.setItem('todoList', JSON.stringify(todoList.list));
    }

    return {
      todo, ...toRefs(todoList), totalTodo, addTodo, deleteTodo, doneTodo
    }
  }
}
</script>

<style>
</style>
