<template>
    <div class="movie-page">
      <div class="filter-section">
        <movie-filter @filtroCambiado="aplicarFiltros" />
      </div>
      <div class="list-section">
        <movie-list :movies="filteredMovies" />
      </div>
    </div>
  </template>
  
  <script>
  import MovieFilter from "components/movie/MovieFilter.vue";
  import MovieList from "components/movie/MovieList.vue";
  import { apiTMDB } from "boot/axios";
  
  export default {
    name: "MoviePage",
    components: {
      MovieFilter,
      MovieList,
    },
    data() {
      return {
        allMovies: [],
        filteredMovies: [],
        filters: {
          genre: null,
          year: null,
        },
      };
    },
    methods: {
      async fetchMovies() {
        try {
          const response = await apiTMDB.get("/discover/movie", {
            params: {
              include_adult: false,
              language: "es-PE",
              page: 1,
              sort_by: "popularity.desc",
            },
          });
          this.allMovies = response.data.results;
          this.filteredMovies = this.allMovies;
        } catch (error) {
          console.error("Error al cargar las películas:", error);
          this.$q.notify({
            message: "Error al cargar películas.",
            color: "negative",
            icon: "error",
          });
        }
      },
      aplicarFiltros(filtros) {
        this.filters = filtros;
        this.filteredMovies = this.allMovies.filter((movie) => {
          const matchesGenre =
            !filtros.genre || movie.genre_ids.includes(filtros.genre);
          const matchesYear =
            !filtros.year || movie.release_date.startsWith(filtros.year);
          return matchesGenre && matchesYear;
        });
      },
    },
    mounted() {
      this.fetchMovies();
    },
  };
  </script>
  
  <style>
  .movie-page {
    display: flex;
    gap: 20px;
  }
  
  .filter-section {
    width: 25%;
  }
  
  .list-section {
    width: 75%;
  }
  </style>