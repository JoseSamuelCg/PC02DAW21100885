<template>
  <div class="q-pa-md">
    <q-row class="q-gutter-md" style="width: 100%; align-items: center;">
      <q-col cols="12" sm="5" class="q-mb-sm">
        <q-input
          v-model="query"
          label="Buscar película"
          outlined
          debounce="500"
          @input="onSearchChange"
          dense
        />
      </q-col>

      <q-col cols="12" sm="5" class="q-mb-sm" style="display: flex; justify-content: flex-end;">
        <q-select
          label="Ordenar por"
          v-model="sortBy"
          :options="sortOptions"
          option-value="value"
          option-label="label"
          @update:model-value="onSortChange"
          dense
        />
      </q-col>
    </q-row>
  </div>
</template>

<script>
export default {
  name: "MovieFilter",
  emits: ["filtersChange"], // Declara el evento
  data() {
    return {
      query: "",
      sortBy: "popularity.desc",
      sortOptions: [
        { label: "Popularidad (Desc)", value: "popularity.desc" },
        { label: "Popularidad (Asc)", value: "popularity.asc" },
        { label: "Mejor puntuadas", value: "vote_average.desc" },
        { label: "Menos votadas", value: "vote_average.asc" },
      ],
    };
  },
  mounted() {
    this.emitFilters(); // Emitir filtros al cargar el componente
  },
  methods: {
    onSortChange() {
      this.emitFilters(); // Emitir filtros cuando cambie el orden
    },
    onSearchChange() {
      this.emitFilters(); // Emitir filtros cuando se cambie la búsqueda
    },
    emitFilters() {
      const filters = {
        query: this.query.trim(),
        sort_by: this.sortBy,
      };
      console.log("Filtros emitidos desde MovieFilter:", filters); // Depuración
      this.$emit("filtersChange", filters);
    },
  },
};
</script>

<style scoped>

.q-row {
  display: flex;
  justify-content: space-between;
}

.q-col {
  display: flex;
  justify-content: flex-start; 
}
</style>
 