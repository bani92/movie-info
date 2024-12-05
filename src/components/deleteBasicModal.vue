<template>
  <deleteModal>
    <template #title> Delete Todo</template>
    <template #body>Are you sure you want to delete this todo?</template>
    <template #footer>
      <button
        type="button"
        class="btn btn-secondary"
        data-bs-dismiss="modal"
        @click="onClose"
      >
        Close
      </button>

      <button type="button" class="btn btn-danger" @click="onDelete">
        Delete
      </button></template
    >
  </deleteModal>
</template>

<script>
import deleteModal from "./deleteModal.vue";
export default {
  components: {
    deleteModal,
  },
  setup(props, { emit }) {
    const onClose = () => {
      emit("close");
    };

    const onDelete = () => {
      emit("delete");
    };
    return {
      onClose,
      onDelete,
    };
  },
};
</script>

<style lang="scss" scoped></style>
