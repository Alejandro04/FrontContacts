<template>
  <div>
    <AppNav></AppNav>
    <p class="app_title">Contacts</p>

    <loading :active.sync="isLoading" :is-full-page="fullPage"></loading>
    <div>
      <v-client-table :columns="columns" :data="tableData" :options="options"></v-client-table>
    </div>
  </div>
</template>


<script>
import axios from "axios";
// Import component
import Loading from "vue-loading-overlay";
// Import stylesheet
import "vue-loading-overlay/dist/vue-loading.css";

export default {
  mounted() {
    this.getContacts();
  },
  components: {
    Loading
  },
  data() {
    return {
      appointments: [],
      isLoading: false,
      fullPage: true,
      columns: ["nombre", "telefono"],
      tableData: [],
      options: {
        texts: {
          count:
            "Mostrando {from} a {to} de {count} registros|Últimos {count} registros|Un registro",
          first: "First",
          last: "Last",
          filter: "Filtro:",
          filterPlaceholder: "fecha o servicio",
          limit: "Registros:",
          page: "Página:",
          noResults: "",
          filterBy: "Filtro por {column}",
          loading: "Cargando...",
          defaultOption: "Select {column}",
          columns: "Columnas"
        }
      }
    };
  },
  methods: {
    async getContacts() {
      this.isLoading = true;

      try {
        const response = await axios.get("http://localhost:3000/contacts");
        console.log(response);
        response.data.forEach(element => {
          this.tableData.push({
            nombre: element.name,
            telefono: element.number,
          });
        });
        this.isLoading = false;
      } catch (error) {
        console.error(error);
      }
    }
  }
};
</script>

<style>
@media screen and (min-width: 480px) {
}

@media screen and (min-width: 767px) {
}

@media screen and (min-width: 950px) {
}
</style>

