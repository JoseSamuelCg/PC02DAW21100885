<template>
    <q-page class="movie-page">
      <!-- Filtro -->
      <movie-filter @filter="updateFilter" />
  
      <!-- Lista de películas -->
      <movie-list :movies="filteredMovies" />
    </q-page>
  </template>
  
  <script>
  import MovieFilter from '../components/movie/MovieFilter.vue';
  import MovieList from '../components/movie/MovieList.vue';
  
  export default {
    components: {
      MovieFilter,
      MovieList,
    },
    data() {
      return {
        movies: [], // Lista completa de películas
        filteredMovies: [], // Lista filtrada según el criterio del usuario
      };
    },
    methods: {
      async fetchMovies() {
        try {
          // Llamada a la API para obtener películas
          const response = await this.$tmdbApi.get('/discover/movie', {
            params: {
              language: 'es-PE',
              sort_by: 'popularity.desc',
            },
          });
          this.movies = response.data.results; // Guardar la lista completa de películas
          this.filteredMovies = this.movies; // Inicialmente no filtrado
          console.log('Películas cargadas:', this.movies); // Depuración
        } catch (error) {
          console.error('Error al cargar las películas:', error);
        }
      },
      updateFilter(filterText) {
        const searchText = filterText.toLowerCase().trim();
        if (searchText === '') {
          // Mostrar todas las películas si no hay texto en el filtro
          this.filteredMovies = this.movies;
        } else {
          // Filtrar las películas que contienen el texto ingresado en el título
          this.filteredMovies = this.movies.filter(movie =>
            movie.title?.toLowerCase().includes(searchText)
          );
        }
        console.log('Películas filtradas:', this.filteredMovies); // Depuración
      },
    },
    mounted() {
      this.fetchMovies(); // Cargar películas al montar el componente
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
  