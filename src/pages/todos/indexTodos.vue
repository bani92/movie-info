<template>
  <router-view />
  <div class="container">
    <h2>To-Do List</h2>
    <input
      class="form-control"
      type="text"
      v-model="searchText"
      placeholder="Search"
      @keyup.enter="searchTodo"
    />
    <hr />
    <TodoSimpleForm @add-todo="addTodo" />
    <div>{{ error }}</div>
    <div v-if="!todos.length">추가된 Todo가 없습니다</div>
    <TodoList
      :todos="todos"
      @toggle-todo="toggleTodo"
      @delete-item="deleteItem"
    />

    <hr />
    <nav aria-label="Page navigation example">
      <ul class="pagination">
        <li v-if="currentPage !== 1" class="page-item">
          <a
            style="cursor: pointer"
            class="page-link"
            @click="getTodos(currentPage - 1)"
            >Previous</a
          >
        </li>

        <li
          v-for="page in numberOfPages"
          :key="page"
          class="page-item"
          :class="currentPage === page ? 'active' : ''"
        >
          <a
            style="cursor: pointer"
            class="page-link"
            @click="getTodos(page)"
            >{{ page }}</a
          >
        </li>
        <li v-if="numberOfPages !== currentPage" class="page-item">
          <a
            style="cursor: pointer"
            class="page-link"
            @click="getTodos(currentPage + 1)"
            >Next</a
          >
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
import { ref, computed, watch } from "vue";
import TodoSimpleForm from "@/components/TodoSimpleForm.vue";
import TodoList from "@/components/TodoList.vue";
import axios from "axios";

export default {
  components: {
    TodoSimpleForm,
    TodoList,
  },
  setup() {
    const toggle = ref(false);
    const error = ref("");
    const todos = ref([]);
    const numberOfTodos = ref(0);
    const limit = 5;
    const currentPage = ref(1);
    const searchText = ref("");
    const numberOfPages = computed(() => {
      return Math.ceil(numberOfTodos.value / limit);
    });

    const todoStyle = {
      textDecoration: "line-through",
      color: "gray",
    };

    const getTodos = async (page = currentPage.value) => {
      currentPage.value = page;
      try {
        const res = await axios.get(
          `http://localhost:3000/todos?_sort=id&_order=desc&subject_like=${searchText.value}&_page=${page}&_limit=${limit}`
        );
        numberOfTodos.value = res.headers["x-total-count"];
        todos.value = res.data;
      } catch (err) {
        console.log(err);
        error.value = "Something went wrong";
      }
    };

    getTodos();

    // form의 submit를 하면 화면이 리로딩되는데 preventDefault로 리로딩 방지
    const addTodo = async (todo) => {
      error.value = "";
      try {
        await axios.post("http://localhost:3000/todos", {
          subject: todo.subject,
          completed: todo.completed,
        });
        getTodos(1);
      } catch (err) {
        console.log(err);
        error.value = "Something went wrong";
      }
    };

    const onToggle = () => {
      toggle.value = !toggle.value;
    };

    const deleteItem = async (index) => {
      error.value = "";
      const id = todos.value[index].id;
      try {
        await axios.delete("http://localhost:3000/todos/" + id);
        getTodos(1);
      } catch (err) {
        error.value = "Something went wrong";
      }
      // todos.value = todos.value.filter((e) => e.id !== e1); 내가 한 방법
    };

    const toggleTodo = async (index, checked) => {
      error.value = "";
      const id = todos.value[index].id;
      try {
        await axios.patch("http://localhost:3000/todos/" + id, {
          completed: checked,
        });
        todos.value[index].completed = checked;
      } catch (err) {
        error.value = "Something went wrong";
      }
    };

    let timeout = null;
    const searchTodo = () => {
      clearTimeout(timeout);
      getTodos(1);
    };
    watch(searchText, () => {
      clearTimeout(timeout);
      timeout = setTimeout(() => {
        getTodos(1);
      }, 2000);
    });
    // const filteredTodos = computed(() => {
    //   if (searchText.value) {
    //     return todos.value.filter((todo) => {
    //       return todo.subject.includes(searchText.value);
    //     });
    //   }

    //   return todos.value;
    // });
    return {
      toggle,
      todos,
      addTodo,
      onToggle,
      todoStyle,
      deleteItem,
      toggleTodo,
      searchText,
      // filteredTodos,
      error,
      numberOfPages,
      currentPage,
      getTodos,
      searchTodo,
    };
  },
};
</script>

<style>
.todo {
  color: gray;
  text-decoration: line-through;
}
</style>
