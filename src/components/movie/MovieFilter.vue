<template>
    <div>
      <h6>Filtros</h6>
      <div class="category-filter">
        <q-select 
          label="Género" 
          v-model="genreSelected" 
          :options="genres" 
          option-value="id" 
          option-label="name" 
          @update:model-value="onGenreChange" 
        />
      </div>
  
      <div class="year-filter q-mt-md">
        <q-select 
          label="Año de lanzamiento" 
          v-model="yearSelected" 
          :options="years" 
          @update:model-value="onYearChange" 
        />
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "MovieFilter",
    emits: ["filtroCambiado"],
    data() {
      return {
        genres: [],
        years: this.generateYears(),
        genreSelected: null,
        yearSelected: null,
      };
    },
    mounted() {
      this.fetchGenres();
    },
    methods: {
      async fetchGenres() {
        const endpointURL = "/genre/movie/list";
        try {
          const response = await this.$apiTMDB.get(endpointURL, {
            params: { language: "es-PE" },
          });
          this.genres = response.data.genres; // Lista de géneros
        } catch (error) {
          console.error("Error al cargar géneros:", error);
        }
      },
      generateYears() {
        const currentYear = new Date().getFullYear();
        return Array.from({ length: 20 }, (_, i) => currentYear - i).map((year) => ({
          label: year.toString(),
          value: year,
        }));
      },
      onGenreChange(value) {
        this.$emit("filtroCambiado", { genre: value, year: this.yearSelected });
      },
      onYearChange(value) {
        this.$emit("filtroCambiado", { genre: this.genreSelected, year: value });
      },
    },
  };
  </script>
  