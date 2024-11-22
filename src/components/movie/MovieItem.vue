<template>
    <q-item clickable class="movie-item">
      <q-item-section avatar>
        <img :src="posterUrl" alt="Poster" class="movie-poster" />
      </q-item-section>
      <q-item-section>
        <q-item-label class="movie-title text-bold">{{ movie.title }}</q-item-label>
        <q-item-label caption class="movie-details">
          ⭐ Calificación: {{ movie.vote_average }} ({{ movie.vote_count }} votos)
        </q-item-label>
      </q-item-section>
    </q-item>
  </template>
  
  <script>
  export default {
    props: {
      movie: {
        type: Object,
        required: true, 
      },
    },
    computed: {
      posterUrl() {
        return `http://image.tmdb.org/t/p/w500${this.movie.poster_path}`;
      },
    },
  };
  </script>
  
  <style scoped>
  /* Estilos para el ítem de película */
  .movie-item {
    padding: 16px; /* Espaciado interno */
    border-bottom: 1px solid #e0e0e0; /* Línea divisoria entre ítems */
    transition: background-color 0.2s ease; /* Animación al pasar el mouse */
  }
  
  .movie-item:hover {
    background-color: #f5f5f5; /* Color de fondo al pasar el mouse */
  }
  
  .movie-poster {
    width: 224px; /* Ancho fijo para las imágenes */
    height: auto; /* Mantener la proporción */
    border-radius: 8px; /* Esquinas redondeadas */
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Sombra ligera */
  }
  
  .movie-title {
    font-size: 1.2em; /* Tamaño del texto para el título */
    color: #333; /* Color del texto */
  }
  
  .movie-details {
    font-size: 0.9em; /* Tamaño más pequeño para los detalles */
    color: #666; /* Color tenue */
  }
  </style>
