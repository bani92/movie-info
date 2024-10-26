<template>
  <h1>영화정보</h1>
  <div v-for="(data, i) in data" :key="i">
    <h3 class="bg-yellow" :style="textRed">{{ data.title }}</h3>
    <p>개봉: {{ data.year }}</p>
    <p>장르: {{ data.category }}</p>
    <button @click="increseLike(i)">좋아요</button> <span>{{ data.cnt }}</span>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      data: [
        {
          title: "노량",
          year: 2023,
          category: "액션, 드라마",
          textRed: "color: red",
          cnt: 0,
        },
        {
          title: "아쿠아맨과 로스트 킹덤",
          year: 2023,
          category: "액션, 판타지, 어드벤처",
          cnt: 0,
        },
        {
          title: "3일의 휴가",
          year: 2023,
          category: "판타지, 드라마",
          cnt: 0,
        },
      ],
    };
  },
  methods: {
    increseLike(i) {
      this.data[i].cnt += 1;
    },
  },
};
</script>

<style>
.bg-yellow {
  background: gold;
  padding: 10px;
}
</style>
