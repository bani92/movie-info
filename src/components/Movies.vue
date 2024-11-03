<template>
  <h1>영화정보</h1>
  <div v-for="(data, i) in data" :key="i">
    <figure>
      <img :src="`${data.imgUrl}`" :alt="data.title" class="item" />
    </figure>
    <div class="info">
      <h3 class="bg-yellow">{{ data.title }}</h3>
      <p>개봉: {{ data.year }}</p>
      <p>장르: {{ data.category }}</p>
      <button @click="$emit('increseLike', i)">좋아요</button>
      <span>{{ data.cnt }}</span>
    </div>
    <p>
      <button @click="$emit('openModal', i)">상세보기</button>
    </p>
  </div>
</template>

<script>
export default {
  name: "MoviesComponent",
  props: {
    data: Array,
  },
};
</script>

<style></style>
