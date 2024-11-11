<!-- <template>
  <Navbar />
  <Event :text="text[eventTextNum]" />
  {{ eventTextNum }}
  <SearchBar :data="data_temp" @searchMovie="searchMovie($event)" />
  <p>
    <button @click="showAllMoive">전체보기</button>
  </p>
  <Movies
    :data="data_temp"
    @openModal="
      isModal = true;
      selectedMovie = $event;
    "
    @increseLike="increseLike($event)"
  />
  <Modal
    :data="data"
    :isModal="isModal"
    :selectedMovie="selectedMovie"
    @closeModal="isModal = false"
  />
</template>

<script>
import data from "./assets/movies";
import Navbar from "./components/Navbar.vue";
import Modal from "./components/Modal.vue";
import Event from "./components/Event.vue";
import Movies from "./components/Movies.vue";
import SearchBar from "./components/SearchBar.vue";

export default {
  name: "App",
  data() {
    return {
      isModal: false,
      data: data,
      data_temp: [...data],
      selectedMovie: 0,
      text: ["NETPLIX 첫번째", "디즈니 100주년", "대한민국 서울의 봄"],
      eventTextNum: 0,
      interval: null,
    };
  },
  methods: {
    increseLike(id) {
      // this.data[i].cnt += 1;
      this.data.find((movie) => {
        if (movie.id == id) {
          movie.cnt += 1;
        }
      });
    },
    searchMovie(title) {
      // 영화제목이 포함된 데이터를 가져옴
      this.data_temp = this.data.filter((movie) => {
        return movie.title.includes(title);
      });
    },
    showAllMoive() {
      this.data_temp = [...this.data];
    },
  },
  components: {
    Navbar: Navbar,
    Modal: Modal,
    Event: Event,
    Movies: Movies,
    SearchBar: SearchBar,
  },
  mounted() {
    this.interval = setInterval(() => {
      if (this.eventTextNum === this.text.length - 1) {
        this.eventTextNum = 0;
      } else {
        this.eventTextNum += 1;
      }
    }, 3000);
  },
  unmounted() {
    clearInterval(this.interval); // 컴포넌트가 종료되도 계속 실행되는 이슈 , 타이머 해제
  },
};
</script>

<style>
.bg-yellow {
  background: gold;
  padding: 10px;
}
* {
  box-sizing: border-box;
  margin: 0;
}

body {
  max-width: 768px;
  margin: 0 auto;
  padding: 20px;
}

h1,
h2,
h3 {
  margin-bottom: 1rem;
}

p {
  margin-bottom: 0.5rem;
}

button {
  margin-right: 10px;
  margin-top: 1rem;
}

.item {
  width: 100%;
  border: 1px solid #ccc;
  display: flex;
  margin-bottom: 20px;
  padding: 1rem;
}

.item figure {
  width: 30%;
  margin-right: 1rem;
}

.item img {
  width: 100%;
}

.item .info {
  width: 100%;
}

.modal {
  background: rgba(0, 0, 0, 0.7);
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal .inner {
  background: #fff;
  width: 80%;
  padding: 20px;
  border-radius: 10px;
}
</style> -->

<template>
  <div class="container">
    <h2>To-Do List</h2>
    <TodoSimpleForm @add-todo="addTodo" />
    <div v-if="!todos.length">추가된 Todo가 없습니다</div>
    <TodoList
      :todos="todos"
      @toggle-todo="toggleTodo"
      @delete-item="deleteItem"
    />
  </div>
</template>

<script>
import { ref } from "vue";
import TodoSimpleForm from "./components/TodoSimpleForm.vue";
import TodoList from "./components/TodoList.vue";

export default {
  components: {
    TodoSimpleForm,
    TodoList,
  },
  setup() {
    const toggle = ref(false);
    const todos = ref([]);
    const todoStyle = {
      textDecoration: "line-through",
      color: "gray",
    };

    // form의 submit를 하면 화면이 리로딩되는데 preventDefault로 리로딩 방지
    const addTodo = (todo) => {
      todos.value.push(todo);
    };

    const onToggle = () => {
      toggle.value = !toggle.value;
    };

    const deleteItem = (index) => {
      todos.value.splice(index, 1);
      // todos.value = todos.value.filter((e) => e.id !== e1); 내가 한 방법
    };

    const toggleTodo = (index) => {
      todos.value[index].completed = !todos.value[index].completed;
    };
    return {
      toggle,
      todos,
      addTodo,
      onToggle,
      todoStyle,
      deleteItem,
      toggleTodo,
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
