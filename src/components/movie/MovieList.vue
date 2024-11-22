<template>
    <div class="movies-grid">
      <movie-item v-for="movie in movies" :key="movie.id" :movie="movie" />
    </div>
  </template>
  
  <script>
  import MovieItem from "components/movie/MovieItem.vue";
  
  export default {
    name: "MovieList",
    components: {
      MovieItem,
    },
    props: {
      movies: {
        type: Array,
        required: true,
      },
    },
  };
  </script>
  
  <style>
  .movies-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
  }
  </style>