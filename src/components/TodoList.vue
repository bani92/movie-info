<template>
  <div class="card mt-2" v-for="(item, index) in todos" :key="item.id">
    <div
      class="card-body p-2 d-flex align-items-center"
      style="cursor: pointer"
      @click="moveToPage(item.id)"
    >
      <div class="form-check flex-grow-1">
        <input
          class="form-check-input"
          type="checkbox"
          :checked="item.completed"
          @change="toggleTodo(index, $event)"
          @click.stop
        />
        <label class="form-check-label" :class="{ todo: item.completed }">{{
          item.subject
        }}</label>
      </div>
      <div>
        <button class="btn btn-danger btn-sm" @click.stop="deleteItem(index)">
          Delete
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { useRouter } from "vue-router";
export default {
  props: {
    todos: {
      type: Array,
      required: true,
    },
  },
  emits: ["toggle-todo", "delete-item"],
  setup(props, { emit }) {
    const router = useRouter();
    const toggleTodo = (index, event) => {
      emit("toggle-todo", index, event.target.checked);
    };

    const deleteItem = (index) => {
      emit("delete-item", index);
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
    };
  },
};
</script>

<style></style>
