<template>
  <!-- <div class="alert toast-box" :class="`alert-${type}`" role="alert"></div>
    <div> -->
  <!-- type이 'success'일 때 -->
  <div
    v-if="type === 'success'"
    class="alert alert-success toast-box"
    role="alert"
  >
    {{ message }}
  </div>
  <!-- type이 'danger'일 때 -->
  <div v-else class="alert alert-danger toast-box" role="alert">
    {{ message }}
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: String,
      required: true,
    },
    type: {
      type: String,
      default: "success",
    },
  },
};
</script>

<style>
.toast-box {
  position: fixed;
  top: 10px;
  right: 10px;
}
</style>
