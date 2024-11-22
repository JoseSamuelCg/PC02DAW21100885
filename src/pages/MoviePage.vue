<template>
  <q-page>
    <MovieFilter @filtersChange="updateFilters" />
    <MovieList :movies="movies" />
  </q-page>
</template>

<script>
import { tmdbApi } from "boot/axios";
import MovieFilter from "../components/movie/MovieFilter.vue";
import MovieList from "../components/movie/MovieList.vue";

export default {
  components: { MovieFilter, MovieList },
  data() {
    return {
      movies: [],
      filters: {
        sort_by: "popularity.desc",
        query: "",
        page: 1,
      },
    };
  },
  methods: {
    async fetchMovies() {
      try {
        const endpoint = this.filters.query ? "/search/movie" : "/discover/movie";
        console.log("Solicitando con filtros a la API:", this.filters); // Depuración
        const response = await tmdbApi.get(endpoint, { params: this.filters });
        console.log("Películas obtenidas de la API:", response.data.results); // Depuración
        this.movies = response.data.results;
      } catch (error) {
        console.error("Error al obtener las películas:", error);
      }
    },
    updateFilters(newFilters) {
      this.filters = { ...this.filters, ...newFilters, page: 1 };
      console.log("Filtros actualizados:", this.filters); // <-- Agregado para depurar
      this.fetchMovies();
    },
  },
  mounted() {
    this.fetchMovies();
  },
};
</script>
  
  <style scoped>
  .movie-page {
    padding: 16px;
    background-color: #ffffff; /* Fondo blanco para un diseño limpio */
    min-height: 100vh; /* Asegura que la página ocupe toda la altura */
    display: flex;
    flex-direction: column;
    align-items: center; /* Centra los elementos en la página */
  }
  
  .q-page {
    display: flex;
    flex-direction: column;
    align-items: center; /* Alineación vertical */
  }
  
  .movie-filter-input {
    max-width: 600px; /* Limitar el tamaño del filtro */
    margin: 0 auto 20px; /* Centrar y agregar margen inferior */
  }
  
  .movie-list {
    width: 100%;
    max-width: 1200px; /* Ancho máximo de la lista */
    padding: 16px;
    background-color: #f9f9f9; /* Fondo tenue */
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Sombra suave */
  }
  </style>
  