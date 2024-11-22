<template>
    <q-list bordered class="movie-list">
      <movie-item v-for="movie in movies" :key="movie.id" :movie="movie" />
    </q-list>
  </template>
  
  <script>
  import MovieItem from './MovieItem.vue';
  
  export default {
    components: {
      MovieItem, // Registrar el subcomponente
    },
    props: {
      movies: {
        type: Array,
        required: true, // Recibe la lista de películas como prop
      },
    },
  };
  </script>
  
  <style scoped>
  /* Estilo para la lista de películas */
  .movie-list {
    background-color: #f9f9f9; /* Fondo tenue */
    border-radius: 8px; /* Bordes redondeados */
    padding: 16px; /* Espaciado interno */
    max-width: 800px; /* Ancho máximo para centrar */
    margin: 0 auto; /* Centrar la lista horizontalmente */
  }
  </style>