<template>
    <q-card class="movie-card">
      <q-card-section>
        <q-img
          :src="posterUrl"
          alt="Poster de la película"
          class="movie-poster"
        />
      </q-card-section>
      <q-card-section>
        <q-card-title>{{ movie.title }}</q-card-title>
        <div>
          <q-icon name="star" color="yellow" /> {{ movie.vote_average }}
          <small>({{ movie.vote_count }} votos)</small>
        </div>
      </q-card-section>
    </q-card>
  </template>
  
  <script>
  export default {
    name: "MovieItem",
    props: {
      movie: {
        type: Object,
        required: true,
      },
    },
    computed: {
      posterUrl() {
        return this.movie.poster_path
          ? `http://image.tmdb.org/t/p/w500${this.movie.poster_path}`
          : "https://via.placeholder.com/300x450?text=No+Image";
      },
    },
  };
  </script>
  
  <style>
  .movie-card {
    width: 200px;
  }
  .movie-poster {
    height: auto;
  }
  </style>