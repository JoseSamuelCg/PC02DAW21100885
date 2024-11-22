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
          :loading="isLoadingGenres"
          :disable="genres.length === 0"
        />
      </div>
  
      <div class="year-filter q-mt-md">
        <q-select 
          label="Año de lanzamiento" 
          v-model="yearSelected" 
          :options="years" 
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
        isLoadingGenres: false,
      };
    },
    mounted() {
      this.fetchGenres();
    },
    methods: {
      async fetchGenres() {
        const endpointURL = "/genre/movie/list";
        this.isLoadingGenres = true;
        try {
          const response = await this.$apiTMDB.get(endpointURL, {
            params: { language: "es-PE" },
          });
          this.genres = response.data.genres;
        } catch (error) {
          console.error("Error al cargar géneros:", error);
        } finally {
          this.isLoadingGenres = false;
        }
      },
      generateYears() {
        const currentYear = new Date().getFullYear();
        return [
          { label: "Todos los años", value: null },
          ...Array.from({ length: 20 }, (_, i) => currentYear - i).map((year) => ({
            label: year.toString(),
            value: year,
          })),
        ];
      },
      emitirFiltros() {
        this.$emit("filtroCambiado", {
          genre: this.genreSelected,
          year: this.yearSelected,
        });
      },
    },
    watch: {
      genreSelected() {
        this.emitirFiltros();
      },
      yearSelected() {
        this.emitirFiltros();
      },
    },
  };
  </script>
  