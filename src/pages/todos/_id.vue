<template>
  <h1>To-Do Page</h1>
  <div v-if="loading">Loading...</div>
  <form v-else @submit.prevent="onSave">
    <div class="row">
      <div class="col-6">
        <div class="form-group">
          <label>Subject</label>
          <input v-model="todo.subject" type="text" class="form-control" />
        </div>
      </div>
      <div class="col-6">
        <div class="form-group">
          <label>Status</label>
          <div>
            <button
              class="btn"
              type="button"
              :class="todo.completed ? `btn-success` : `btn-danger`"
              @click="toggleTodoStatus()"
            >
              {{ todo.completed ? "Incomplete" : "Danger" }}
            </button>
          </div>
        </div>
      </div>
    </div>

    <button type="submit" class="btn btn-primary" :disabled="!todoUpdated">
      Save
    </button>
    <button
      type="button"
      class="btn btn-outline-dark ml-2"
      @click="moveToTodoListPage()"
    >
      Cancel
    </button>
  </form>
  <ToastTest v-if="showToast" :message="toastMessage" :type="toastAlertType" />
</template>

<script>
import { useRoute, useRouter } from "vue-router";
import axios from "axios";
import { ref, computed } from "vue";
import _ from "lodash";
import ToastTest from "@/components/ToastTest.vue";

export default {
  components: {
    ToastTest,
  },
  setup() {
    const route = useRoute();
    const todo = ref(null);
    const originalTodo = ref(null);
    const loading = ref(true);
    const showToast = ref(false);
    const toastMessage = ref("");
    const toastAlertType = ref("");
    const router = useRouter();
    const todoId = route.params.id;

    const getTodo = async () => {
      try {
        const res = await axios.get(
          "http://localhost:3000/todos/" + route.params.id
        );
        todo.value = { ...res.data };
        originalTodo.value = { ...res.data };
        loading.value = false;
      } catch (error) {
        triggerToast("Something went wrong", "danger");
      }
    };

    const todoUpdated = computed(() => {
      return !_.isEqual(todo.value, originalTodo.value);
    });

    const toggleTodoStatus = () => {
      console.log(todo.value.completed);
      todo.value.completed = !todo.value.completed;
    };

    const moveToTodoListPage = () => {
      router.push({
        name: "Todos",
      });
    };
    const triggerToast = (message, type = "success") => {
      toastMessage.value = message;
      toastAlertType.value = type;
      showToast.value = true;
      setTimeout(() => {
        toastMessage.value = "";
        toastAlertType.value = "";
        showToast.value = false;
      }, 3000);
    };
    const onSave = async () => {
      try {
        const res = await axios.put(`http://localhost:3000/todos/${todoId}`, {
          subject: todo.value.subject,
          completed: todo.value.completed,
        });
        console.log(res);
        originalTodo.value = { ...res.data };
        triggerToast("Successfully saved");
      } catch (error) {
        triggerToast("Something went wrong", "danger");
      }

      // router.push({
      //   name: "Todos",
      // });
    };
    getTodo();

    return {
      todo,
      loading,
      toggleTodoStatus,
      moveToTodoListPage,
      onSave,
      todoUpdated,
      showToast,
      toastMessage,
      toastAlertType,
    };
  },
};
</script>

<style lang="scss" scoped></style>
