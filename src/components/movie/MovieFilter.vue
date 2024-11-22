<template>
    <q-input
      v-model="filterText"
      label="Buscar películas"
      placeholder="Escribe el título de una película"
      outlined
      dense
      @input="applyFilter"
      class="q-my-md movie-filter-input"
    />
  </template>
  
  <script>
  export default {
    data() {
      return {
        filterText: '', // Texto para filtrar las películas
      };
    },
    methods: {
      applyFilter() {
        this.$emit('filter', this.filterText); // Emitir el texto para filtrar al componente padre
      },
    },
  };
  </script>
  
  <style scoped>
  /* Estilo para el filtro */
  .movie-filter-input {
    max-width: 600px; /* Limitar el ancho del input */
    margin: 0 auto; /* Centrar horizontalmente */
  }
  .q-my-md {
    margin-top: 16px;
    margin-bottom: 16px;
  }
  </style>
  