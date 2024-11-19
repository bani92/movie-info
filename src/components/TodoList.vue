<template>
  <div class="card mt-2" v-for="(item, index) in todos" :key="item.id">
    <div class="card-body p-2 d-flex align-items-center">
      <div class="form-check flex-grow-1">
        <input
          class="form-check-input"
          type="checkbox"
          :checked="item.completed"
          @change="toggleTodo(index)"
        />
        <label class="form-check-label" :class="{ todo: item.completed }">{{
          item.subject
        }}</label>
      </div>
      <div>
        <button class="btn btn-danger btn-sm" @click="deleteItem(index)">
          Delete
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    todos: {
      type: Array,
      required: true,
    },
  },
  emits: ["toggle-todo", "delete-item"],
  setup(props, { emit }) {
    const toggleTodo = (index) => {
      emit("toggle-todo", index);
    };

    const deleteItem = (index) => {
      emit("delete-item", index);
    };

    return {
      toggleTodo,
      deleteItem,
    };
  },
};
</script>

<style></style>
