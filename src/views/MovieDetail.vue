<template>
  <div v-if="!loading" class="movie-detail">
    <h2>{{ movie.Title }}</h2>
    <p>{{ movie.Year }}</p>
    <img :src="movie.Poster" :alt="movie.Title + 'Poster'" class="feature-img" />
    <p>{{ movie.Plot }}</p>
  </div>
</template>

<script>
import { ref, onBeforeMount } from "vue";
import { useRoute } from "vue-router";

export default {
  setup() {
    const movie = ref({});
    const route = useRoute();
    const loading = ref(true);

    onBeforeMount(() => {
      fetch(`${process.env.VUE_APP_URL}/?apikey=${process.env.VUE_APP_API_KEY}&i=${route.params.id}&plot=full`)
        .then(res => res.json())
        .then(data => {
          movie.value = data;
          loading.value = false;
        });
    });

    return {
      movie,
      loading,
    };
  },
};
</script>

<style lang="scss" scoped>
.movie-detail {
  padding: 16px;

  h2 {
    color: #ffffff;
    font-size: 28px;
    font-weight: 600;
    margin-bottom: 16px;
  }

  .feature-img {
    display: block;
    max-width: 200px;
    margin-bottom: 16px;
  }

  p {
    color: #ffffff;
    font-size: 18px;
    line-height: 1.4;
  }
}
</style>
