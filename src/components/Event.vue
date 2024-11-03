<template>
  <div class="event">
    <p>{{ text }}</p>
  </div>
</template>

<script>
export default {
  name: "EventCompoonent",
  props: {
    text: String,
  },
};
</script>

<style>
.event {
  background: #444;
  padding: 10px;
  color: #fff;
  text-align: center;
  font-size: small;
}

.event p {
  margin: 0;
}
</style>
