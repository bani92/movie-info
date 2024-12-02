<template>
  <div class="card mt-2" v-for="(item, index) in todos" :key="item.id">
    <div
      class="card-body p-2 d-flex align-items-center"
      style="cursor: pointer"
      @click="moveToPage(item.id)"
    >
      <div class="flex-grow-1">
        <input
          class="ml-2 mr-2"
          type="checkbox"
          :checked="item.completed"
          @change="toggleTodo(index, $event)"
          @click.stop
        />
        <span :class="{ todo: item.completed }">{{ item.subject }}</span>
      </div>
      <div>
        <button class="btn btn-danger btn-sm" @click.stop="openModal(item.id)">
          Delete
        </button>
      </div>
    </div>
  </div>
  <teleport to="#modal">
    <deleteModal v-if="showModal" @close="closeModal" @delete="deleteItem" />
  </teleport>
</template>

<script>
import { useRouter } from "vue-router";
import deleteModal from "./deleteBasicModal.vue";
import { ref } from "vue";

export default {
  components: {
    deleteModal,
  },
  props: {
    todos: {
      type: Array,
      required: true,
    },
  },
  emits: ["toggle-todo", "delete-item"],
  setup(props, { emit }) {
    const router = useRouter();
    const showModal = ref(false);
    const todoDeleteId = ref(null);
    const toggleTodo = (index, event) => {
      emit("toggle-todo", index, event.target.checked);
    };

    const openModal = (id) => {
      todoDeleteId.value = id;
      showModal.value = true;
    };

    const closeModal = () => {
      todoDeleteId.value = null;
      showModal.value = false;
    };
    const deleteItem = () => {
      emit("delete-item", todoDeleteId.value);
      showModal.value = false;
      todoDeleteId.value = null;
    };

    const moveToPage = (todoId) => {
      console.log(todoId);
      // router.push("/todos/" + todoId);
      router.push({
        name: "Todo",
        params: {
          id: todoId,
        },
      });
    };
    return {
      toggleTodo,
      deleteItem,
      moveToPage,
      showModal,
      openModal,
      closeModal,
    };
  },
};
</script>

<style></style>
