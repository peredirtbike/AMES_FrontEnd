
<template>
  <div>
    <DataTable :value="filteredClients(searchTerm)" tableStyle="min-width: 50rem">
      <template #header>
          <div class="flex align-items-center justify-content-between gap-2">
              <span class="text-xl text-900 font-bold">Clients</span>
              <Button class="right-at" @click="redirectToNewClientForm">Crea un client</Button>
          </div>

          <div class="flex align-items-center gap-2">
              <InputText size="small" v-model="searchTerm" placeholder="Cerca un client" />
          </div>
      </template>
      <Column field="name" header="Nom"></Column>
      <Column field="lastName" header="Cognoms"></Column>
      <Column field="phone" header="Telèfon"></Column>
      <Column field="email" header="Email"></Column>
      <Column field="address" header="Adreça"></Column>
      <Column field="hasDiscount" header="Descompte">
        <template #body="rowData">
          <i :class="{'pi pi-check': rowData.data.preferential, 'pi pi-times': !rowData.data.preferential}"></i>
        </template>
      </Column>
      <Column field="preferential" header="Preferencial">
        <template #body="rowData">
          <i :class="{'pi pi-check': rowData.data.preferential, 'pi pi-times': !rowData.data.preferential}"></i>
        </template></Column>
      <Column header="Accions">
        <template #body="rowData">
          <Button @click="editClient(rowData.data.id)">Editar</Button>
        </template>
      </Column>

      <template #footer> En total hi ha {{ clients ? clients.length : 0 }} clients. </template>
  </DataTable>
</div>
</template>


<script>
import { mapActions, mapGetters, mapState } from 'vuex';

export default {
  name: 'ClientList',
  computed: {
    ...mapGetters(['filteredClients']),
    ...mapState(['clients']),
  },
  methods: {
    ...mapActions(['fetchClients']),
    editClient(clientId) {
      this.$router.push(`/clients/${clientId}`);
    },
    redirectToNewClientForm() {
      this.$router.push('/clients/new');
    },
  },
  data() {
    return {
      searchTerm: '',
    };
  },
  watch: {
    searchTerm: {
      handler(newTerm) {
        this.filteredClients(newTerm);
      },
      immediate: true,
    },
  },      
  mounted() {
    this.fetchClients();
  },
};
</script>